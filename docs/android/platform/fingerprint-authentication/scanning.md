---
title: Сканирование отпечатков пальцев
ms.prod: xamarin
ms.assetid: 1CDDC096-77E0-47B3-BE0B-8953E2DDACD3
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/23/2016
ms.openlocfilehash: 61edd0e4b532f18a8fc28502e5bb990703068776
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027497"
---
# <a name="scanning-for-fingerprints"></a>Сканирование отпечатков пальцев

Теперь, когда мы увидели, как подготовить приложение Xamarin.Android для использования проверки подлинности по отпечаткам пальцев, давайте вернемся к методу `FingerprintManager.Authenticate` и рассмотрим его роль в проверке подлинности по отпечаткам пальцев в Android 6.0. Краткий обзор рабочего процесса для проверки подлинности по отпечаткам пальцев:

1. Вызов `FingerprintManager.Authenticate`, передача `CryptoObject` и экземпляра `FingerprintManager.AuthenticationCallback`. `CryptoObject` используется, чтобы убедиться, что результат проверки подлинности по отпечаткам пальцев не был изменен. 
2. Подкласс класса [FingerprintManager.AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html). Экземпляр этого класса будет предоставлен для `FingerprintManager` при запуске проверки подлинности по отпечаткам пальцев. После завершения сканирования отпечатков пальцев он будет вызывать один из методов обратного вызова для этого класса.
3. Написание кода для обновления пользовательского интерфейса, чтобы сообщить пользователю о том, что устройство запустило сканер отпечатков пальцев и ожидает взаимодействия с пользователем. 
4. По завершении сканирования отпечатков пальцев Android вернет результаты в приложение, вызвав метод на экземпляре `FingerprintManager.AuthenticationCallback`, который был предоставлен на предыдущем шаге.
5. Приложение будет уведомлять пользователя о результатах проверки подлинности по отпечаткам пальцев и реагировать на результаты надлежащим образом. 

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

- Первый параметр — это _криптографический_ объект, который сканер отпечатков пальцев будет использовать для проверки подлинности результатов сканирования отпечатков пальцев. Этот объект может быть `null`, в этом случае приложение должно быть надежным и результаты сканирования отпечатка пальца не должны быть изменены. Рекомендуется создавать экземпляры `CryptoObject` и предоставлять их `FingerprintManager`, а не NULL. В статье [Создание CryptObject](~/android/platform/fingerprint-authentication/creating-a-cryptoobject.md) объясняется, как создать экземпляр `CryptoObject` на основе `Cipher`.
- Второй параметр всегда равен нулю. Документация по Android определяет его как набор флагов и, скорее всего, он зарезервирован для использования в будущем. 
- Третий параметр, `cancellationSignal` — это объект, используемый для отключения сканера отпечатков пальцев и отмены текущего запроса. Это [Android CancellationSignal](https://developer.android.com/reference/android/os/CancellationSignal.html), а не тип из .NET Framework.
- Четвертый параметр является обязательным и является классом, подклассом которого является абстрактный класс `AuthenticationCallback`. Методы в этом классе будут вызываться для передачи клиентам сигнала о завершении работы `FingerprintManager` и о ее результатах. О реализации `AuthenticationCallback` доступно множество информации, поэтому она будет изложена в [отдельном разделе](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md).
- Пятый параметр является необязательным экземпляром `Handler`. Если объект `Handler` предоставлен, `FingerprintManager` будет использовать `Looper` из этого объекта при обработке сообщений с устройства для сканирования отпечатков пальца. Как правило, не требуется предоставлять `Handler`, и FingerprintManager будет использовать `Looper` из приложения.

## <a name="cancelling-a-fingerprint-scan"></a>Отмена сканирования отпечатков пальцев

Может потребоваться, чтобы пользователь (или приложение) отменил сканирование отпечатков пальцев после запуска. В этом случае вызовите метод [`IsCancelled`](https://developer.android.com/reference/android/os/CancellationSignal.html#isCanceled()) на [`CancellationSignal`](https://developer.android.com/reference/android/os/CancellationSignal.html), который был предоставлен для `FingerprintManager.Authenticate` при вызове для запуска сканирования отпечатков пальцев.

Теперь, когда мы видели метод `Authenticate`, давайте внимательнее рассмотрим некоторые наиболее важные параметры. Сначала обратим внимание на статью [Ответ на обратные вызовы проверки подлинности](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md), в которой обсуждается подкласс [FingerprintManager.AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html), который позволяет приложению Android реагировать на данные, предоставленные сканером отпечатков пальцев.

## <a name="related-links"></a>Связанные ссылки

- [CancellationSignal](https://developer.android.com/reference/android/os/CancellationSignal.html)
- [FingerprintManager.AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [FingerprintManager.CryptoObject](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
