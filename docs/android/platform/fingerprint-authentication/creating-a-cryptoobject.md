---
title: Создание объекта CryptoObject
ms.prod: xamarin
ms.assetid: 4D159B80-FFF4-4136-A7F0-F8A5C6B86838
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 871058d1c128b37a0f2e77b43587139efb433de1
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "75487780"
---
# <a name="creating-a-cryptoobject"></a>Создание объекта CryptoObject

Целостность результатов проверки подлинности по отпечаткам пальцев важна для приложения &ndash;, так как приложение знает о подлинности пользователя. Теоретически вредоносные программы сторонних производителей могут перехватывать и изменять результаты, возвращаемые сканером отпечатков пальцев. В этом разделе рассматривается один прием, позволяющий сохранить достоверность результатов отпечатка. 

`FingerprintManager.CryptoObject` — это оболочка для криптографических API Java, которая используется `FingerprintManager` для защиты целостности запроса проверки подлинности. Как правило, объект `Javax.Crypto.Cipher` — это механизм для шифрования результатов сканирования отпечатков пальцев. Сам объект `Cipher` будет использовать ключ, созданный приложением с помощью API-интерфейсов хранилища ключей для Android.

Чтобы понять, как все эти классы работают вместе, рассмотрим сначала следующий код, демонстрирующий создание `CryptoObject`, а затем дадим подробное объяснение:

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

В примере кода создается новый `Cipher` для каждого `CryptoObject` с использованием ключа, созданного приложением. Ключ определяется переменной `KEY_NAME`, которая была задана в начале класса `CryptoObjectHelper`. Метод `GetKey` попытается получить ключ с помощью API-интерфейсов хранилища ключей Android. Если ключ не существует, метод `CreateKey` создаст для приложения новый ключ.

Для создания экземпляра шифра используется вызов `Cipher.GetInstance`, принимающий _преобразование_ (строковое значение, указывающее алгоритму, как шифровать и расшифровывать данные). Вызов `Cipher.Init` завершит инициализацию шифра, предоставив ключ из приложения. 

Важно понимать, что в некоторых ситуациях Android может сделать ключ недействительным: 

- на устройстве зарегистрирован новый отпечаток;
- на устройстве не зарегистрирован ни один отпечаток пальца;
- пользователь отключил блокировку экрана;
- пользователь изменил блокировку экрана (тип блокировки экрана или используемый ПИН-код или жест).

В этом случае `Cipher.Init` выдаст [`KeyPermanentlyInvalidatedException`](https://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html). Приведенный выше пример кода перехватит это исключение, удалит ключ, а затем создаст новый.

В следующем разделе рассказывается, как создать ключ и сохранить его на устройстве.

## <a name="creating-a-secret-key"></a>Создание секретного ключа

`CryptoObjectHelper` класс использует Android [`KeyGenerator`](xref:Javax.Crypto.KeyGenerator), чтобы создать ключ и сохранить его на устройстве. Класс `KeyGenerator` может создать ключ, но ему требуются определенные метаданные о типе создаваемого ключа. Эти сведения предоставляются экземпляром класса [`KeyGenParameterSpec`](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html). 

Экземпляр `KeyGenerator` создается с помощью фабричного метода `GetInstance`. В примере кода используется алгоритм шифрования [_Advanced Encryption Standard_](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) (_AES_). Алгоритм AES будет разбивать данные на блоки фиксированного размера и шифровать каждый из этих блоков отдельно.

Затем с помощью `KeyGenParameterSpec.Builder` создается `KeyGenParameterSpec`. `KeyGenParameterSpec.Builder` заключает в оболочку перечисленные ниже сведения о создаваемом ключе.

- Имя ключа.
- Ключ должен быть допустимым для шифрования и расшифровки.
- В примере кода для `BLOCK_MODE` задано значение _Cipher Block_  (`KeyProperties.BlockModeCbc`), что означает, что каждый блок сравнивается (XOR) с предыдущим блоком (создавая зависимости между каждым блоком). 
- `CryptoObjectHelper` использует [_Стандарт шифрования с открытым ключом № 7_](https://tools.ietf.org/html/rfc2315) (_PKCS 7_), чтобы создать байты, которые будут дополнять блоки, чтобы выровнять их размер.
- `SetUserAuthenticationRequired(true)` означает, что для использования ключа требуется проверка подлинности пользователя.

После создания `KeyGenParameterSpec` используется для инициализации `KeyGenerator`, что приведет к созданию ключа, который будет безопасно сохранен на устройстве. 

## <a name="using-the-cryptoobjecthelper"></a>Использование CryptoObjectHelper

Теперь, когда пример кода содержит большую часть логики для создания `CryptoWrapper` в классе `CryptoObjectHelper`, перейдем к коду в начале этого учебника и используем `CryptoObjectHelper` для создания объекта Cipher и запуска сканера отпечатков пальцев: 

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

Теперь, когда мы рассмотрели создание `CryptoObject`, можно перейти к просмотру того, как `FingerprintManager.AuthenticationCallbacks` используются для передачи результатов сканирования отпечатков пальцев в приложение Android.

## <a name="related-links"></a>Связанные ссылки

- [Шифр](xref:Javax.Crypto.Cipher)
- [FingerprintManager.CryptoObject](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [KeyGenerator](xref:Javax.Crypto.KeyGenerator)
- [KeyGenParameterSpec](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)
- [KeyGenParameterSpec.Builder](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html)
- [KeyPermanentlyInvalidatedException](https://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)
- [KeyProperties](https://developer.android.com/reference/android/security/keystore/KeyProperties.html)
- [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
- [RFC 2315 — PCKS #7](https://tools.ietf.org/html/rfc2315)
