---
title: Сканирование отпечатков пальцев
ms.prod: xamarin
ms.assetid: 1CDDC096-77E0-47B3-BE0B-8953E2DDACD3
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/23/2016
ms.openlocfilehash: 61edd0e4b532f18a8fc28502e5bb990703068776
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027497"
---
# <a name="scanning-for-fingerprints"></a>Сканирование отпечатков пальцев

Теперь, когда мы увидели, как подготовить приложение Xamarin. Android для использования проверки подлинности отпечатков пальцев, давайте вернемся к методу `FingerprintManager.Authenticate` и обсудим его место в проверке подлинности отпечатков пальцев 6,0. В этом списке описывается краткий обзор рабочего процесса для проверки подлинности отпечатков пальцев.

1. Вызов `FingerprintManager.Authenticate`, передача `CryptoObject` и экземпляра `FingerprintManager.AuthenticationCallback`. `CryptoObject` используется, чтобы убедиться, что результат проверки подлинности отпечатков пальцев не был изменен. 
2. Подклассировать класс [финжерпринтманажер. AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html) . Экземпляр этого класса будет предоставлен для `FingerprintManager` при запуске проверки подлинности отпечатков пальцев. После завершения сканирования отпечатков пальцев он будет вызывать один из методов обратного вызова для этого класса.
3. Напишите код для обновления пользовательского интерфейса, чтобы сообщить пользователю о том, что устройство запустило сканер отпечатков пальцев и ожидает взаимодействия с пользователем. 
4. По завершении сканирования отпечатков пальцев Android возвратит результаты в приложение, вызвав метод на экземпляре `FingerprintManager.AuthenticationCallback`, который был указан на предыдущем шаге.
5. Приложение будет уведомлять пользователя о результатах проверки подлинности отпечатков пальцев и реагировать на результаты соответствующим образом. 

В следующем фрагменте кода приведен пример метода в действии, который начнет сканирование отпечатков пальцев:

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */

    // CryptoObjectHelper is described in the previous section.
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();    
    
    // cancellationSignal can be used to manually stop the fingerprint scanner. 
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(this);
    
    // AuthenticationCallback is a base class that will be covered later on in this guide.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Start the fingerprint scanner.
    fingerprintManager.Authenticate(cryptoHelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

Давайте рассмотрим каждый из этих параметров в методе `Authenticate` более подробно:

- Первый параметр — это объект _шифрования_ , который сканер отпечатков пальцев будет использовать для проверки подлинности результатов сканирования отпечатков пальцев. Этот объект может быть `null`. в этом случае приложение должно быть доверенным и не было изменено с помощью результатов отпечатка пальца. Рекомендуется создавать экземпляры `CryptoObject` и предоставлять их `FingerprintManager` а не NULL. [Создание криптобжект](~/android/platform/fingerprint-authentication/creating-a-cryptoobject.md) объясняет, как создать экземпляр `CryptoObject` на основе `Cipher`.
- Второй параметр всегда равен нулю. Документация по Android определяет этот набор флагов и, скорее всего, зарезервирован для использования в будущем. 
- Третий параметр, `cancellationSignal` — это объект, используемый для отключения сканера отпечатков пальцев и отмены текущего запроса. Это [Канцеллатионсигнал Android](https://developer.android.com/reference/android/os/CancellationSignal.html), а не тип из .NET Framework.
- Четвертый параметр является обязательным и является классом, подклассом которого является абстрактный класс `AuthenticationCallback`. Методы в этом классе будут вызываться для передачи клиентам сигнала о завершении `FingerprintManager` и о результатах. Так как есть много сведений о реализации `AuthenticationCallback`, она будет рассмотрена в [собственном разделе](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md).
- Пятый параметр является необязательным `Handler` экземпляром. Если предоставлен объект `Handler`, `FingerprintManager` будет использовать `Looper` из этого объекта при обработке сообщений с устройства отпечатка пальца. Как правило, не требуется предоставлять `Handler`, Финжерпринтманажер будет использовать `Looper` из приложения.

## <a name="cancelling-a-fingerprint-scan"></a>Отмена сканирования отпечатков пальцев

Может потребоваться, чтобы пользователь (или приложение) отменил сканирование отпечатков пальцев после запуска. В этом случае вызовите метод [`IsCancelled`](https://developer.android.com/reference/android/os/CancellationSignal.html#isCanceled()) на [`CancellationSignal`](https://developer.android.com/reference/android/os/CancellationSignal.html) , который был предоставлен для `FingerprintManager.Authenticate` при вызове для запуска сканирования отпечатков пальцев.

Теперь, когда мы видели метод `Authenticate`, давайте более подробно рассмотрим некоторые из более важных параметров. Во первых, мы рассмотрим [реакцию на обратные вызовы проверки подлинности](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md), в которых обсуждается подкласс [финжерпринтманажер. AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html), что позволяет приложению Android реагировать на результаты, предоставленные сканером отпечатков пальцев.

## <a name="related-links"></a>Связанные ссылки

- [канцеллатионсигнал](https://developer.android.com/reference/android/os/CancellationSignal.html)
- [Финжерпринтманажер. AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [Финжерпринтманажер. Криптубжект](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [Финжерпринтманажеркомпат. Криптубжект](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [финжерпринтманажер](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [финжерпринтманажеркомпат](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
