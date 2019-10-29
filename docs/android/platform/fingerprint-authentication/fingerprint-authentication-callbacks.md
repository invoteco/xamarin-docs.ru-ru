---
title: Ответ на обратные вызовы проверки подлинности
ms.prod: xamarin
ms.assetid: 6533AFC9-1A1C-4897-A154-4D4ECFE27761
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/06/2017
ms.openlocfilehash: 8dc06740355bd95828e1a1bd8d9d15a2ef37e6b2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027534"
---
# <a name="responding-to-authentication-callbacks"></a>Ответ на обратные вызовы проверки подлинности

Сканер отпечатков пальцев выполняется в фоновом режиме в собственном потоке, и по завершении он будет сообщать результаты проверки, вызвав один метод `FingerprintManager.AuthenticationCallback` в потоке пользовательского интерфейса. Приложение Android должно предоставить собственный обработчик, который расширяет этот абстрактный класс, реализуя все следующие методы:

- **`OnAuthenticationError(int errorCode, ICharSequence errString)`** &ndash; вызывается при возникновении неустранимой ошибки. Нет ничего большего, что приложение или пользователь может исправить ситуацию, только если возможно, повторите попытку.
- **`OnAuthenticationFailed()`** &ndash; этот метод вызывается, когда отпечаток обнаружен, но не распознан устройством.
- **`OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)`** &ndash; вызывается при возникновении устранимой ошибки, например пальца, который проводится для ускорения работы сканера.
- **`OnAuthenticationSucceeded(FingerprintManagerCompati.AuthenticationResult result)`** &ndash; это вызывается при распознавании отпечатка.

Если при вызове `Authenticate`использовался `CryptoObject`, рекомендуется вызвать `Cipher.DoFinal` в `OnAuthenticationSuccessful`.
`DoFinal` выдаст исключение, если шифр был незаконно изменен или неправильно инициализирован, что свидетельствует о том, что результат сканера отпечатков пальцев мог быть изменен вне приложения.

> [!NOTE]
> Рекомендуется, чтобы класс обратного вызова оставался относительно недостаточной толщиной и иметь логику конкретного приложения. Обратные вызовы должны действовать как "трафик COP" между приложением Android и результатами сканера отпечатков пальцев.

## <a name="a-sample-authentication-callback-handler"></a>Пример обработчика обратного вызова проверки подлинности

Следующий класс является примером минимальной реализации `FingerprintManager.AuthenticationCallback`: 

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

`OnAuthenticationSucceeded` проверяет, была ли предоставлена `Cipher` для `FingerprintManager` при вызове `Authentication`. Если это так, в шифре вызывается метод `DoFinal`. Это закрывает `Cipher`, восстанавливая его в исходное состояние. Если возникла проблема с шифром, `DoFinal` выдаст исключение, и попытка проверки подлинности будет считаться неудачной.

Обратные вызовы `OnAuthenticationError` и `OnAuthenticationHelp` получают целое число, обозначающее причину проблемы. В следующем разделе описываются все возможные коды справки и ошибок. Два обратных вызова служат аналогичным образом &ndash; для информирования приложения о том, что проверка подлинности отпечатков пальцев завершилась сбоем. Они отличаются по степени серьезности. `OnAuthenticationHelp` — это устранимая ошибка пользователя, например, прокрутка отпечатков пальцев слишком быстро; `OnAuthenticationError` более серьезная ошибка, например повреждение сканера отпечатков пальцев.

Обратите внимание, что `OnAuthenticationError` будет вызываться, когда сканирование отпечатков пальцев отменяется с помощью сообщения `CancellationSignal.Cancel()`. Параметр `errMsgId` будет иметь значение 5 (`FingerprintState.ErrorCanceled`). В зависимости от требований реализация `AuthenticationCallbacks` может обрабатывать эту ситуацию иначе, чем другие ошибки. 

`OnAuthenticationFailed` вызывается, когда отпечаток был успешно проверен, но не соответствует ни одному отпечатку, зарегистрированному на устройстве. 

## <a name="help-codes-and-error-message-ids"></a>Коды справки и идентификаторы сообщений об ошибках 

Список и описание кодов ошибок и справочных кодов можно найти в [пакет SDK для Android документации](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#FINGERPRINT_ACQUIRED_GOOD) по классу финжерпринтманажер. Xamarin. Android представляет эти значения с помощью перечисления `Android.Hardware.Fingerprints.FingerprintState`.

- **`AcquiredGood`** &ndash; (значение 0) полученное изображение было хорошим.

- **`AcquiredImagerDirty`** &ndash; (значение 3) изображение отпечатка слишком шумным из-за вероятности или обнаружения мотокросса на датчике. Например, разумно возвращать это значение после нескольких `AcquiredInsufficient` или фактического обнаружения мотокросса на датчике (зависание пикселов, отрезки и т. д.). Пользователь должен предпринять действия для очистки датчика при возвращении.

- **`AcquiredInsufficient`** &ndash; (значение 2) изображение отпечатка пальца слишком шумным для обработки из-за обнаруженного условия (т. е. обложки сухой) или, возможно, некорректного датчика (см. `AcquiredImagerDirty`.

- **`AcquiredPartial`** &ndash; (значение 1) обнаружено только частичное изображение отпечатка. Во время регистрации пользователю следует сообщить о том, что необходимо сделать для решения этой проблемы, например, &ldquo;нажать кнопку «плотно» на датчике.&rdquo;

- **`AcquiredTooFast`** &ndash; (значение 5) изображение отпечатка пальца было неполным из-за быстрого перемещения. Если в основном подходит для датчиков линейного массива, это также может произойти, если палец был перемещен во время приобретения. Пользователю должно быть предложено переместить палец медленнее (линейный) или оставить палец на датчике дольше.

- **`AcquiredToSlow`** &ndash; (значение 4) изображение отпечатка пальца было нечитаемым из-за отсутствия движения. Это наиболее подходящее для датчиков линейного массива, требующих движения прокрутки.

- **`ErrorCanceled`** &ndash; (значение 5) операция была отменена, так как датчик отпечатков пальцев недоступен. Например, это может произойти, когда пользователь переключается, устройство заблокировано или другая ожидающая операция предотвращает или отключает его.

- **`ErrorHwUnavailable`** &ndash; (значение 1) оборудование недоступно. Повторите попытку позже.

- **`ErrorLockout`** &ndash; (значение 7) операция была отменена, так как API заблокирован из-за слишком большого количества попыток.

- состояние ошибки **`ErrorNoSpace`** &ndash; (значение 4), возвращаемое для таких операций, как регистрация. не удается завершить операцию, так как не осталось места для завершения операции.

- **`ErrorTimeout`** &ndash; (значение 3) состояние ошибки, возвращенное, если текущий запрос был запущен слишком долго. Это предназначено для предотвращения неограниченного ожидания программами датчика отпечатков пальцев. Время ожидания является платформой и зависит от датчика, но обычно составляет около 30 секунд.

- состояние ошибки **`ErrorUnableToProcess`** &ndash; (значение 2), возвращенное, когда датчик не смог обработать текущее изображение.

## <a name="related-links"></a>Связанные ссылки

- [Шифр](https://docs.oracle.com/javase/7/docs/api/javax/crypto/Cipher.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.AuthenticationCallback.html)
