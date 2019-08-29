---
title: Ответ на обратные вызовы проверки подлинности
ms.prod: xamarin
ms.assetid: 6533AFC9-1A1C-4897-A154-4D4ECFE27761
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/06/2017
ms.openlocfilehash: f1fc484931ba7a574ac660b4856f20b1cb1e08a3
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/28/2019
ms.locfileid: "70119589"
---
# <a name="responding-to-authentication-callbacks"></a>Ответ на обратные вызовы проверки подлинности

Сканер отпечатков пальцев выполняется в фоновом режиме в собственном потоке, и по завершении он будет сообщать результаты проверки, вызвав один метод `FingerprintManager.AuthenticationCallback` в потоке пользовательского интерфейса. Приложение Android должно предоставить собственный обработчик, который расширяет этот абстрактный класс, реализуя все следующие методы:

- **`OnAuthenticationError(int errorCode, ICharSequence errString)`** &ndash; Вызывается, когда возникает неустранимая ошибка. Нет ничего большего, что приложение или пользователь может исправить ситуацию, только если возможно, повторите попытку.
- **`OnAuthenticationFailed()`** &ndash; Этот метод вызывается, когда отпечаток обнаружен, но не распознан устройством.
- **`OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)`** &ndash; Вызывается, когда возникает устранимая ошибка, например, когда палец проводится для ускорения работы сканера.
- **`OnAuthenticationSucceeded(FingerprintManagerCompati.AuthenticationResult result)`** &ndash; Вызывается, когда распознается отпечаток.

Если при вызове `Authenticate`использовался метод, рекомендуется вызвать `Cipher.DoFinal` в `OnAuthenticationSuccessful`. `CryptoObject`
`DoFinal`вызовет исключение, если шифр был изменен или неправильно инициализирован, что свидетельствует о том, что результат сканера отпечатков пальцев мог быть изменен вне приложения.


> [!NOTE]
> Рекомендуется, чтобы класс обратного вызова оставался относительно недостаточной толщиной и иметь логику конкретного приложения. Обратные вызовы должны действовать как "трафик COP" между приложением Android и результатами сканера отпечатков пальцев.

## <a name="a-sample-authentication-callback-handler"></a>Пример обработчика обратного вызова проверки подлинности

Следующий класс является примером минимальной `FingerprintManager.AuthenticationCallback` реализации: 

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

`OnAuthenticationSucceeded`проверяет, `Cipher` `Authentication` предоставлено ли значение привызовеметода.`FingerprintManager` Если это так, `DoFinal` метод вызывается для шифра. Это приведет к `Cipher`закрытию, восстановленному в исходное состояние. Если возникла проблема с шифром, то `DoFinal` вызовет исключение, и попытка проверки подлинности будет считаться неудачной.

Каждый из `OnAuthenticationError` них получает целое число, указывающее на проблему. `OnAuthenticationHelp` В следующем разделе описываются все возможные коды справки и ошибок. Два обратных вызова служат &ndash; для информирования приложения о том, что проверка подлинности отпечатков пальцев завершилась ошибкой. Они отличаются по степени серьезности. `OnAuthenticationHelp`— Это устранимая ошибка пользователя, например, прокрутка отпечатков пальцев слишком быстро; `OnAuthenticationError` более серьезная ошибка, например повреждение сканера отпечатков пальцев.

Обратите `OnAuthenticationError` внимание, что будет вызываться, когда сканирование отпечатков пальцев `CancellationSignal.Cancel()` отменяется через сообщение. Параметр будет иметь значение 5 (`FingerprintState.ErrorCanceled`). `errMsgId` В зависимости от требований реализация метода `AuthenticationCallbacks` может обрабатывать эту ситуацию иначе, чем другие ошибки. 

`OnAuthenticationFailed`вызывается, когда отпечаток был успешно проверен, но не соответствует ни одному отпечатку, зарегистрированному на устройстве. 

## <a name="help-codes-and-error-message-ids"></a>Коды справки и идентификаторы сообщений об ошибках 

Список и описание кодов ошибок и справочных кодов можно найти в [пакет SDK для Android документации](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#FINGERPRINT_ACQUIRED_GOOD) по классу финжерпринтманажер. Xamarin. Android представляет эти значения с `Android.Hardware.Fingerprints.FingerprintState` помощью перечисления:


- **`AcquiredGood`** &ndash; (значение 0) полученное изображение было хорошим.


- **`AcquiredImagerDirty`** &ndash; (значение 3) изображение отпечатка слишком шумным из-за вероятности или обнаружения мотокросса на датчике. Например, разумно вернуть это значение после нескольких `AcquiredInsufficient` или реальных обнаружений мотокросса на датчике (задержать Пиксели, отрезки и т. д.). Пользователь должен предпринять действия для очистки датчика при возвращении.


- **`AcquiredInsufficient`** (значение 2) изображение отпечатка слишком шумным для обработки из-за обнаруженного условия (т. е. обложки сухой) или, возможно, некорректного датчика (см. раздел `AcquiredImagerDirty`. &ndash;



- **`AcquiredPartial`** &ndash; (значение 1) обнаружено только частичное изображение отпечатка. Во время регистрации пользователю следует сообщить о том, что необходимо сделать для решения этой проблемы, например, &ldquo;нажать на датчике кнопку «плотно».&rdquo;



- **`AcquiredTooFast`** &ndash; (значение 5) изображение отпечатка пальца было неполным из-за быстрого перемещения. Если в основном подходит для датчиков линейного массива, это также может произойти, если палец был перемещен во время приобретения. Пользователю должно быть предложено переместить палец медленнее (линейный) или оставить палец на датчике дольше.




- **`AcquiredToSlow`** &ndash; (значение 4) изображение отпечатка пальца было нечитаемым из-за отсутствия движения. Это наиболее подходящее для датчиков линейного массива, требующих движения прокрутки.



- **`ErrorCanceled`** &ndash; (значение 5) операция была отменена, так как датчик отпечатков пальцев недоступен. Например, это может произойти, когда пользователь переключается, устройство заблокировано или другая ожидающая операция предотвращает или отключает его.



- **`ErrorHwUnavailable`** &ndash; (значение 1) оборудование недоступно. Повторите попытку позже.




- **`ErrorLockout`** &ndash; (значение 7) операция была отменена, так как API заблокирован из-за слишком большого количества попыток.




- **`ErrorNoSpace`** &ndash; (значение 4) состояние ошибки, возвращенное для таких операций, как регистрация; операция не может быть завершена из-за недостаточного объема хранилища для завершения операции.



- **`ErrorTimeout`** &ndash; (значение 3) состояние ошибки, возвращенное, если текущий запрос был запущен слишком долго. Это предназначено для предотвращения неограниченного ожидания программами датчика отпечатков пальцев. Время ожидания является платформой и зависит от датчика, но обычно составляет около 30 секунд.



- **`ErrorUnableToProcess`** &ndash; (значение 2) состояние ошибки, возвращенное, когда датчик не смог обработать текущее изображение.



## <a name="related-links"></a>Связанные ссылки

- [Шифр](https://docs.oracle.com/javase/7/docs/api/javax/crypto/Cipher.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.AuthenticationCallback.html)
