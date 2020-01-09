---
title: Создание Криптубжект
ms.prod: xamarin
ms.assetid: 4D159B80-FFF4-4136-A7F0-F8A5C6B86838
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 871058d1c128b37a0f2e77b43587139efb433de1
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75487780"
---
# <a name="creating-a-cryptoobject"></a>Создание Криптубжект

Целостность результатов проверки подлинности отпечатков пальцев важна для приложения &ndash; это то, как приложение знает удостоверение пользователя. Теоретически вредоносные программы сторонних производителей могут перехватывать и изменять результаты, возвращаемые сканером отпечатков пальцев. В этом разделе рассматривается один прием, позволяющий сохранить достоверность результатов отпечатка. 

`FingerprintManager.CryptoObject` является оболочкой для API-интерфейсов шифрования Java и используется `FingerprintManager` для защиты целостности запроса проверки подлинности. Как правило, объект `Javax.Crypto.Cipher` — это механизм для шифрования результатов проверки отпечатков пальцев. Сам объект `Cipher` будет использовать ключ, созданный приложением с помощью API-интерфейсов хранилища для Android.

Чтобы понять, как все эти классы работают вместе, давайте сначала рассмотрим следующий код, демонстрирующий создание `CryptoObject`и более подробное объяснение:

```csharp
public class CryptoObjectHelper
{
    // This can be key name you want. Should be unique for the app.
    static readonly string KEY_NAME = "com.xamarin.android.sample.fingerprint_authentication_key";

    // We always use this keystore on Android.
    static readonly string KEYSTORE_NAME = "AndroidKeyStore";

    // Should be no need to change these values.
    static readonly string KEY_ALGORITHM = KeyProperties.KeyAlgorithmAes;
    static readonly string BLOCK_MODE = KeyProperties.BlockModeCbc;
    static readonly string ENCRYPTION_PADDING = KeyProperties.EncryptionPaddingPkcs7;
    static readonly string TRANSFORMATION = KEY_ALGORITHM + "/" +
                                            BLOCK_MODE + "/" +
                                            ENCRYPTION_PADDING;
    readonly KeyStore _keystore;

    public CryptoObjectHelper()
    {
        _keystore = KeyStore.GetInstance(KEYSTORE_NAME);
        _keystore.Load(null);
    }

    public FingerprintManagerCompat.CryptoObject BuildCryptoObject()
    {
        Cipher cipher = CreateCipher();
        return new FingerprintManagerCompat.CryptoObject(cipher);
    }

    Cipher CreateCipher(bool retry = true)
    {
        IKey key = GetKey();
        Cipher cipher = Cipher.GetInstance(TRANSFORMATION);
        try
        {
            cipher.Init(CipherMode.EncryptMode, key);
        } catch(KeyPermanentlyInvalidatedException e)
        {
            _keystore.DeleteEntry(KEY_NAME);
            if(retry)
            {
                CreateCipher(false);
            } else
            {
                throw new Exception("Could not create the cipher for fingerprint authentication.", e);
            }
        }
        return cipher;
    }

    IKey GetKey()
    {
        IKey secretKey;
        if(!_keystore.IsKeyEntry(KEY_NAME))
        {
            CreateKey();
        }

        secretKey = _keystore.GetKey(KEY_NAME, null);
        return secretKey;
    }

    void CreateKey()
    {
        KeyGenerator keyGen = KeyGenerator.GetInstance(KeyProperties.KeyAlgorithmAes, KEYSTORE_NAME);
        KeyGenParameterSpec keyGenSpec =
            new KeyGenParameterSpec.Builder(KEY_NAME, KeyStorePurpose.Encrypt | KeyStorePurpose.Decrypt)
                .SetBlockModes(BLOCK_MODE)
                .SetEncryptionPaddings(ENCRYPTION_PADDING)
                .SetUserAuthenticationRequired(true)
                .Build();
        keyGen.Init(keyGenSpec);
        keyGen.GenerateKey();
    }
}
```

В примере кода создается новый `Cipher` для каждой `CryptoObject`с использованием ключа, созданного приложением. Ключ определяется переменной `KEY_NAME`, которая была задана в начале класса `CryptoObjectHelper`. Метод `GetKey` попытается получить ключ с помощью API-интерфейсов хранилища для Android. Если ключ не существует, метод `CreateKey` создаст новый ключ для приложения.

Для создания экземпляра шифра используется вызов `Cipher.GetInstance`, принимающий _Преобразование_ (строковое значение, указывающее шифру, как шифровать и расшифровывать данные). Вызов `Cipher.Init` завершит инициализацию шифра, предоставив ключ из приложения. 

Важно понимать, что в некоторых ситуациях Android может сделать ключ недействительным. 

- На устройстве был зарегистрирован новый отпечаток.
- На устройстве не зарегистрированы отпечатки пальцев.
- Пользователь отключил блокировку экрана.
- Пользователь изменил блокировку экрана (тип скринлокк или используемый ПИН-код/шаблон).

В этом случае `Cipher.Init` выдаст [`KeyPermanentlyInvalidatedException`](https://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html). Приведенный выше пример кода будет выполнять треппинг этого исключения, удалить ключ, а затем создать новый.

В следующем разделе рассказывается, как создать ключ и сохранить его на устройстве.

## <a name="creating-a-secret-key"></a>Создание секретного ключа

Класс `CryptoObjectHelper` использует [`KeyGenerator`](xref:Javax.Crypto.KeyGenerator) Android для создания ключа и сохранения его на устройстве. Класс `KeyGenerator` может создать ключ, но ему требуются метаданные о типе создаваемого ключа. Эти сведения предоставляются экземпляром класса [`KeyGenParameterSpec`](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html) . 

Экземпляр `KeyGenerator` создается с помощью метода фабрики `GetInstance`. В примере кода в качестве алгоритма шифрования используется [_AES_](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) (_AES_). AES будет разбивать данные на блоки фиксированного размера и шифровать каждый из этих блоков.

Затем создается `KeyGenParameterSpec` с помощью `KeyGenParameterSpec.Builder`. `KeyGenParameterSpec.Builder` заключает в оболочку следующие сведения о создаваемом ключе:

- Имя ключа.
- Ключ должен быть допустимым для шифрования и расшифровки.
- В примере кода для `BLOCK_MODE` задано _шифр блочных цепочек_ (`KeyProperties.BlockModeCbc`), что означает, что каждый блок ксоред с предыдущим блоком (создание зависимостей между блоками). 
- В `CryptoObjectHelper` используется [_стандарт криптографии с открытым ключом #7_](https://tools.ietf.org/html/rfc2315) (_PKCS7_) для создания байтов, которые будут отключаться между блоками, чтобы убедиться, что они имеют одинаковый размер.
- `SetUserAuthenticationRequired(true)` означает, что для использования ключа требуется проверка подлинности пользователя.

После создания `KeyGenParameterSpec` используется для инициализации `KeyGenerator`, что приведет к созданию ключа и безопасно сохранить его на устройстве. 

## <a name="using-the-cryptoobjecthelper"></a>Использование Криптубжекселпер

Теперь, когда пример кода содержит большую часть логики для создания `CryptoWrapper` в `CryptoObjectHelper` классе, давайте перейдем к коду в начале этого учебника и используем `CryptoObjectHelper` для создания шифра и запуска сканера отпечатков пальцев: 

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */
    
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    // Using the Support Library classes for maximum reach
    FingerprintManagerCompat fingerPrintManager = FingerprintManagerCompat.From(this);
    
    // AuthCallbacks is a C# class defined elsewhere in code.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Here is where the CryptoObjectHelper builds the CryptoObject. 
    fingerprintManager.Authenticate(cryptohelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

Теперь, когда мы рассмотрели создание `CryptoObject`, можно перейти к просмотру того, как `FingerprintManager.AuthenticationCallbacks` используются для передачи результатов службы сканера отпечатков пальцев в приложение Android.

## <a name="related-links"></a>Связанные ссылки

- [Шифр](xref:Javax.Crypto.Cipher)
- [Финжерпринтманажер. Криптубжект](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [Финжерпринтманажеркомпат. Криптубжект](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [кэйженератор](xref:Javax.Crypto.KeyGenerator)
- [кэйженпараметерспек](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)
- [Кэйженпараметерспек. Builder](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html)
- [кэйперманентлинвалидатедексцептион](https://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)
- [KeyProperties](https://developer.android.com/reference/android/security/keystore/KeyProperties.html)
- [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
- [RFC 2315-ПККС #7](https://tools.ietf.org/html/rfc2315)
