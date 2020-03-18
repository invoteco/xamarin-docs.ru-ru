---
title: Ответ на обратные вызовы проверки подлинности
ms.prod: xamarin
ms.assetid: 6533AFC9-1A1C-4897-A154-4D4ECFE27761
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/06/2017
ms.openlocfilehash: 8dc06740355bd95828e1a1bd8d9d15a2ef37e6b2
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027534"
---
# <a name="responding-to-authentication-callbacks"></a>Ответ на обратные вызовы проверки подлинности

Сканер отпечатков пальцев работает в фоновом режиме в собственном потоке, а после завершения сообщает результаты сканирования, вызывая один из методов `FingerprintManager.AuthenticationCallback` в потоке пользовательского интерфейса. Приложение Android должно предоставить собственный обработчик, который расширяет этот абстрактный класс и реализует все следующие методы.

- **`OnAuthenticationError(int errorCode, ICharSequence errString)`** &ndash; вызывается в случае неустранимой ошибки. В этой ситуации ни приложение, ни пользователь уже не могут никак исправить ситуацию, кроме повторной попытки выполнить операцию.
- **`OnAuthenticationFailed()`** &ndash; этот метод вызывается, когда отпечаток обнаружен, но не распознан устройством.
- **`OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)`** &ndash; вызывается, когда возникает устранимая ошибка, например, палец проведен по сканеру слишком быстро.
- **`OnAuthenticationSucceeded(FingerprintManagerCompati.AuthenticationResult result)`** &ndash; вызывается, когда получен и распознан отпечаток пальца.

Если при вызове `Authenticate` использовался `CryptoObject`, мы рекомендуем вызвать `Cipher.DoFinal` в `OnAuthenticationSuccessful`.
`DoFinal` вызовет исключение, если шифр был изменен или неправильно инициализирован, то есть присутствуют признаки вмешательства внешних для приложения субъектов в результат работы сканера отпечатков пальцев.

> [!NOTE]
> Мы рекомендуем сохранять класс обратного вызова относительно небольшим и не размещать здесь логику конкретного приложения. Обратные вызовы должны действовать как "дорожный инспектор" при передаче результатов сканера отпечатков пальцев в приложение Android.

## <a name="a-sample-authentication-callback-handler"></a>Пример обработчика обратного вызова для проверки подлинности

Следующий код содержит пример простейшей реализации `FingerprintManager.AuthenticationCallback`. 

```csharp
class MyAuthCallbackSample : FingerprintManagerCompat.AuthenticationCallback
{
    // Can be any byte array, keep unique to application.
    static readonly byte[] SECRET_BYTES = {1, 2, 3, 4, 5, 6, 7, 8, 9};
    // The TAG can be any string, this one is for demonstration.
    static readonly string TAG = "X:" + typeof (SimpleAuthCallbacks).Name;

    public MyAuthCallbackSample()
    {
    }

    public override void OnAuthenticationSucceeded(FingerprintManagerCompat.AuthenticationResult result)
    {
        if (result.CryptoObject.Cipher != null) 
        {
            try
            {
                // Calling DoFinal on the Cipher ensures that the encryption worked.
                byte[] doFinalResult = result.CryptoObject.Cipher.DoFinal(SECRET_BYTES);
    
                // No errors occurred, trust the results.              
            }
            catch (BadPaddingException bpe)
            {
                // Can't really trust the results.
                Log.Error(TAG, "Failed to encrypt the data with the generated key." + bpe);
            }
            catch (IllegalBlockSizeException ibse)
            {
                // Can't really trust the results.
                Log.Error(TAG, "Failed to encrypt the data with the generated key." + ibse);
            }
        }
        else
        {
            // No cipher used, assume that everything went well and trust the results.
        }
    }

    public override void OnAuthenticationError(int errMsgId, ICharSequence errString)
    {
        // Report the error to the user. Note that if the user canceled the scan,
        // this method will be called and the errMsgId will be FingerprintState.ErrorCanceled.
    }

    public override void OnAuthenticationFailed()
    {
        // Tell the user that the fingerprint was not recognized.
    }

    public override void OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)
    {
        // Notify the user that the scan failed and display the provided hint.
    }
}
```

`OnAuthenticationSucceeded` проверяет, была ли предоставлена `Cipher` для `FingerprintManager` при вызове `Authentication`. Если да, то вызывается метод `DoFinal` для шифра. Этот метод закрывает `Cipher`, восстанавливая его исходное состояние. Если возникла проблема с шифром, `DoFinal` выдаст исключение и попытка проверки подлинности будет считаться неудачной.

Обратные вызовы `OnAuthenticationError` и `OnAuthenticationHelp` получают целое число, указывающее причину проблемы. В следующем разделе описываются все возможные коды справки и ошибок. Два обратных вызова работают аналогичным образом &ndash;, информируя приложение о сбое при проверке подлинности по отпечатку пальца. Они отличаются по степени серьезности. `OnAuthenticationHelp` обозначает устранимую ошибку, например слишком быстро проведенный по сканеру палец, а `OnAuthenticationError` — более серьезную ошибку, например повреждение сканера отпечатков пальцев.

Обратите внимание, что `OnAuthenticationError` будет вызываться при отмене сканирования отпечатка пальцев с помощью сообщения `CancellationSignal.Cancel()`. Параметр `errMsgId` будет иметь значение 5 (`FingerprintState.ErrorCanceled`). В зависимости от конкретных требований, реализация `AuthenticationCallbacks` может обрабатывать эту ситуацию не так, как другие ошибки. 

`OnAuthenticationFailed` вызывается, когда отпечаток пальца успешно просканирован, но не соответствует ни одному из зарегистрированных на устройстве отпечатков. 

## <a name="help-codes-and-error-message-ids"></a>Идентификаторы кодов помощи и сообщений об ошибках 

Список и описание кодов ошибок и помощи можно найти в описании класса FingerprintManager из [документации по пакету SDK для Android](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#FINGERPRINT_ACQUIRED_GOOD). Xamarin.Android представляет такие значения в перечислении `Android.Hardware.Fingerprints.FingerprintState`.

- **`AcquiredGood`** &ndash; (значение 0) означает, что получено хорошее изображение.

- **`AcquiredImagerDirty`** &ndash; (значение 3) означает, что изображение отпечатка шумное из-за обнаруженной или предполагаемой грязи на датчике. Например, этот код разумно возвращать после нескольких попыток `AcquiredInsufficient` или при реальном обнаружении грязи (одинаковые пиксели на нескольких изображениях, полосы на изображении и т. п.). При возвращении такой ошибки ожидается, что пользователю нужно очистить датчик.

- **`AcquiredInsufficient`** &ndash; (значение 2) изображение отпечатка пальца слишком шумное для нормальной обработки по известной причине (например, сухая кожа) или из-за предполагаемого загрязнения датчика (см. `AcquiredImagerDirty`.

- **`AcquiredPartial`** &ndash; (значение 1) получено лишь частичное изображение отпечатка пальца. При регистрации пользователю необходимо сообщать о действиях для устранения такой проблемы, например предложить &ldquo;сильнее нажать пальцем на датчик&rdquo;.

- **`AcquiredTooFast`** &ndash; (значение 5) изображение отпечатка пальца неполное из-за слишком быстрого движения. Это состояние более типично для датчиков с линейным массивом, но может случиться и на других типах оборудования при движениях пальца во время сканирования. Пользователю следует предложить перемещать палец медленнее (если это линейный датчик) или дольше удерживать палец на сканере.

- **`AcquiredToSlow`** &ndash; (значение 4) изображение отпечатка пальца неразборчиво из-за слишком быстрого движения. Это состояние более типично для датчиков с линейным массивом, на которых требуется движение пальца.

- **`ErrorCanceled`** &ndash; (значение 5) операция была отменена из-за отсутствия датчика отпечатка пальца. Например, такое может происходить при переключении пользователя, блокировке устройства или использовании (блокировке) датчика другой операцией.

- **`ErrorHwUnavailable`** &ndash; (значение 1) оборудование недоступно. Повторите попытку позже.

- **`ErrorLockout`** &ndash; (значение 7) операция была отменена из-за блокировки API или слишком большого числа попыток.

- **`ErrorNoSpace`** &ndash; (значение 4) это состояние ошибки для таких операций, как регистрация отпечатка, которое обозначает невозможность завершить операцию из-за отсутствия свободного места в хранилище.

- **`ErrorTimeout`** &ndash; (значение 3) это состояние ошибки возвращается, если текущий запрос выполняется слишком долго. Оно предназначено для того, чтобы программы не ожидали информации от датчика отпечатка пальца неограниченно долго. Время ожидания зависит от конкретных платформы и датчика, обычно около 30 секунд.

- **`ErrorUnableToProcess`** &ndash; (значение 2) это состояние ошибки возвращается, если датчик не может обработать текущее изображение.

## <a name="related-links"></a>Связанные ссылки

- [Шифр](https://docs.oracle.com/javase/7/docs/api/javax/crypto/Cipher.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.AuthenticationCallback.html)
