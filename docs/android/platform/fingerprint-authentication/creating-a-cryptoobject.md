---
title: Создание Криптубжект
ms.prod: xamarin
ms.assetid: 4D159B80-FFF4-4136-A7F0-F8A5C6B86838
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 03796af880aaef74c2d4b54007ac34ef1c5dc180
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/28/2019
ms.locfileid: "70119840"
---
# <a name="creating-a-cryptoobject"></a>Создание Криптубжект

Целостность результатов проверки подлинности отпечатков пальцев важна &ndash; для приложения, так как приложение знает удостоверение пользователя. Теоретически вредоносные программы сторонних производителей могут перехватывать и изменять результаты, возвращаемые сканером отпечатков пальцев. В этом разделе рассматривается один прием, позволяющий сохранить достоверность результатов отпечатка. 

Является оболочкой `FingerprintManager` для API-интерфейсов шифрования Java и используется для защиты целостности запроса проверки подлинности. `FingerprintManager.CryptoObject` Как правило, `Javax.Crypto.Cipher` объект — это механизм для шифрования результатов проверки отпечатков пальцев. Сам `Cipher` объект будет использовать ключ, созданный приложением с помощью API-интерфейсов хранилища для Android.

Чтобы понять, как все эти классы работают вместе, давайте сначала рассмотрим следующий код, демонстрирующий создание `CryptoObject`, а затем более подробное описание.

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
            cipher.Init(CipherMode.EncryptMode | CipherMode.DecryptMode, key);
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

В примере кода создается новый `Cipher` объект для каждого `CryptoObject`с использованием ключа, созданного приложением. Ключ определяется `KEY_NAME` переменной, заданной в начале `CryptoObjectHelper` класса. Метод `GetKey` попытается получить ключ с помощью API-интерфейсов хранилища ключей Android. Если ключ не существует, метод `CreateKey` создаст новый ключ для приложения.

Шифр создается с помощью вызова `Cipher.GetInstance`, принимающего _Преобразование_ (строковое значение, которое сообщает шифру о шифровании и расшифровке данных). Вызов метода приведет `Cipher.Init` к завершению инициализации шифра путем предоставления ключа из приложения. 

Важно понимать, что в некоторых ситуациях Android может сделать ключ недействительным. 

- На устройстве был зарегистрирован новый отпечаток.
- На устройстве не зарегистрированы отпечатки пальцев.
- Пользователь отключил блокировку экрана.
- Пользователь изменил блокировку экрана (тип скринлокк или используемый ПИН-код/шаблон).

`Cipher.Init` [В`KeyPermanentlyInvalidatedException`](https://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)этом случае вызовет исключение. Приведенный выше пример кода будет выполнять треппинг этого исключения, удалить ключ, а затем создать новый.

В следующем разделе рассказывается, как создать ключ и сохранить его на устройстве.

## <a name="creating-a-secret-key"></a>Создание секретного ключа

Класс использует Android [`KeyGenerator`](xref:Javax.Crypto.KeyGenerator) для создания ключа и сохранения его на устройстве. `CryptoObjectHelper` `KeyGenerator` Класс может создать ключ, но ему необходимы некоторые метаданные о типе создаваемого ключа. Эти сведения предоставляются экземпляром [`KeyGenParameterSpec`](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html) класса. 

Экземпляр создается с помощью метода Factory `GetInstance` `KeyGenerator` . В примере кода в качестве алгоритма шифрования используется [_AES_](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) (_AES_). AES будет разбивать данные на блоки фиксированного размера и шифровать каждый из этих блоков.

`KeyGenParameterSpec` Затем создается с помощью `KeyGenParameterSpec.Builder`. В `KeyGenParameterSpec.Builder` оболочке переносится следующая информация о создаваемом ключе:

- Имя ключа.
- Ключ должен быть допустимым для шифрования и расшифровки.
- В образце кода `BLOCK_MODE` для параметра задается значение _шифра цепочки блоков_ (`KeyProperties.BlockModeCbc`), означающее, что каждый блок ксоред с предыдущим блоком (создание зависимостей между каждым блоком). 
- Использует стандарт [_криптографии с открытым ключом #7_](https://tools.ietf.org/html/rfc2315) (PKCS7) для создания байтов, которые будут выдавать блоки, чтобы убедиться, что они имеют одинаковый размер. `CryptoObjectHelper`
- `SetUserAuthenticationRequired(true)`означает, что для использования ключа требуется проверка подлинности пользователя.

После создания `KeyGenParameterSpec` объект используется для `KeyGenerator`инициализации, что приведет к созданию ключа и безопасно сохранить его на устройстве. 

## <a name="using-the-cryptoobjecthelper"></a>Использование Криптубжекселпер

Теперь, когда пример кода содержит большую часть логики для создания `CryptoWrapper` `CryptoObjectHelper` класса, давайте перейдем к коду с начала `CryptoObjectHelper` этого учебника и используем для создания шифра и запуска сканера отпечатков пальцев: 

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

Теперь, когда мы увидели `CryptoObject`, как создать, можно перейти к статье, чтобы увидеть, `FingerprintManager.AuthenticationCallbacks` как используется для передачи результатов службы сканера отпечатков пальцев в приложение Android.



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
