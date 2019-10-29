---
title: Фоновая перенаправление и NSURLSession в Xamarin. iOS
description: В этом документе представлено пошаговое руководство, в котором показано, как использовать фоновую пересылку и NSUrlSession, чтобы начать загрузку большого образа и продолжить загрузку, когда приложение помещается в фоновом режиме.
ms.prod: xamarin
ms.assetid: 6960E025-3D5C-457A-B893-25B734F8626D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 6004598b215c85b70b057ff156c3a905a0879138
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73010715"
---
# <a name="background-transfer-and-nsurlsession-in-xamarinios"></a>Фоновая перенаправление и NSURLSession в Xamarin. iOS

Фоновая передача инициируется путем настройки фонового `NSURLSession` и постановкуных задач отправки или загрузки. Если задачи выполняются во время фонового, приостановленного или завершенного работы приложения, iOS уведомляет приложение, вызывая обработчик завершения в *AppDelegateе*приложения. Следующая схема демонстрирует это в действии:

 [![](background-transfer-walkthrough-images/transfer.png "A background transfer is initiated by configuring a background NSURLSession and enqueuing upload or download tasks")](background-transfer-walkthrough-images/transfer.png#lightbox)

Давайте посмотрим, как это выглядит в коде.

## <a name="configuring-a-background-session"></a>Настройка фонового сеанса

Чтобы создать фоновый сеанс, создайте новый `NSUrlSession` и настройте его с помощью объекта `NSUrlSessionConfiguration`.

Объект конфигурации определяет действия, выполняемые сеансом, и типы задач, которые он может выполнять.
Сеансы, настроенные с помощью метода `CreateBackgroundSessionConfiguration`, выполняются в отдельном процессе и выполняют передачу по усмотрению (Wi-Fi) для сохранения данных и времени работы аккумулятора.
В следующем примере кода показана правильная настройка сеанса фоновой трансляции с помощью метода `CreateBackgroundSessionConfiguration` и уникального идентификатора строки:

```csharp
public partial class SimpleBackgroundTransferViewController : UIViewController
{
  NSUrlSession session = null;

  NSUrlSessionConfiguration configuration =
      NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration ("com.SimpleBackgroundTransfer.BackgroundSession");
  session = NSUrlSession.FromConfiguration
      (configuration, (NSUrlSessionDelegate) new MySessionDelegate(), new NSOperationQueue());

}
```

Помимо объекта конфигурации, сеансу также требуется делегат сеанса и очередь.
Очередь определяет порядок, в котором задачи будут выполнены. Делегат сеанса чаперонес процесс передачи и обрабатывает проверку подлинности, кэширование и другие проблемы, связанные с сеансом.

## <a name="working-with-tasks-and-delegates"></a>Работа с задачами и делегатами

Теперь, когда мы настроили фоновый сеанс, давайте начнем задачи по обработке перемещения. Мы можем отследить эти задачи с помощью экземпляра `NSUrlSessionDelegate`, который называется делегатом сеанса. Делегат сеанса отвечает за пробуждение завершенного или приостановленного приложения в фоновом режиме для выполнения проверки подлинности, ошибок или завершения передачи.

`NSUrlSessionDelegate` предоставляет следующие основные методы для проверки состояния перемещения.

- *Дидфинишевентсфорбаккграундсессион* — этот метод вызывается после завершения всех задач и завершения перемещения.
- *Дидрецеивечалленже* — вызывается для запроса учетных данных, когда требуется авторизация.
- *Дидбекомеинвалидвисеррор* — вызывается, если `NSURLSession` станет недействительным.

Для фоновых сеансов требуются более специализированные делегаты в зависимости от типов выполняемых задач. Фоновые сеансы ограничены двумя типами задач:

- *Отправка задач* . задачи типа `NSUrlSessionUploadTask` используют `NSUrlSessionTaskDelegate`, который наследуется от `NSUrlSessionDelegate`. Этот делегат предоставляет дополнительные методы отслеживания хода передачи, обработки перенаправления HTTP и т. д.
- *Задачи скачивания* — задачи типа `NSUrlSessionDownloadTask` используют `NSUrlSessionDownloadDelegate`, который наследуется от `NSUrlSessionTaskDelegate`. Этот делегат предоставляет все методы для отправки задач, а также методы, связанные с загрузкой, для отслеживания хода загрузки и определения момента возобновления или завершения задачи загрузки.

В следующем коде определяется задача, которую можно использовать для загрузки изображения из URL-адреса. Мы начнем задачу, вызвав `CreateDownloadTask` в нашем фоновом сеансе и передав запрос URL-адреса:

```csharp
const string DownloadURLString = "http://cdn1.xamarin.com/webimages/images/xamarin.png";
public NSUrlSessionDownloadTask downloadTask;

NSUrl downloadURL = NSUrl.FromString (DownloadURLString);
NSUrlRequest request = NSUrlRequest.FromUrl (downloadURL);
downloadTask = session.CreateDownloadTask (request);
```

Далее мы создадим новый делегат для скачивания сеанса, чтобы контролировать все задачи скачивания в этом сеансе:

```csharp
public class MySessionDelegate : NSUrlSessionDownloadDelegate
{
  public override void DidWriteData (NSUrlSession session, NSUrlSessionDownloadTask downloadTask, long bytesWritten, long totalBytesWritten, long totalBytesExpectedToWrite)
  {
    Console.WriteLine (string.Format ("DownloadTask: {0}  progress: {1}", downloadTask, progress));
    InvokeOnMainThread( () => {
      // update UI with progress bar, if desired
    });
  }
  ...
}
```

Если нам нужно узнать о ходе выполнения задачи загрузки, можно переопределить метод `DidWriteData` для отслеживания хода выполнения и даже обновить пользовательский интерфейс. Обновления пользовательского интерфейса будут отображаться немедленно, если приложение находится на переднем плане, или будет ожидать, когда пользователь в следующий раз откроет приложение.

API делегата сеанса предоставляет широкий набор средств для взаимодействия с задачами. Полный список методов делегата сеанса см. в документации по API `NSUrlSessionDelegate`.

> [!IMPORTANT]
> Фоновые сеансы запускаются в фоновом потоке, поэтому все вызовы для обновления пользовательского интерфейса должны быть явным образом запущены в потоке пользовательского интерфейса путем вызова `InvokeOnMainThread`, чтобы избежать завершения работы приложения iOS. 

## <a name="handling-transfer-completion"></a>Обработка завершения перемещения

Заключительным этапом является предоставление приложению сведений о завершении всех связанных с сеансом задач и обработке нового содержимого.

В *AppDelegate*Подпишитесь на событие `HandleEventsForBackgroundUrl`. Когда приложение переходит в фоновый режим и выполняется сеанс передачи, этот метод вызывается, и система передает нам обработчик завершения:

```csharp
public System.Action backgroundSessionCompletionHandler;

public override void HandleEventsForBackgroundUrl (UIApplication application, string sessionIdentifier, System.Action completionHandler)
{
  this.backgroundSessionCompletionHandler = completionHandler;
}
```

Мы будем использовать обработчик завершения, чтобы сообщить iOS о завершении обработки приложения.

Вспомним, что сеанс может порождать несколько задач для обработки перемещения. По завершении выполнения последней задачи приостановленное или завершенное приложение повторно запускается в фоновом режиме. Затем приложение повторно подключается к `NSURLSession` с помощью уникального идентификатора сеанса и вызывает `DidFinishEventsForBackgroundSession` в делегате сеанса. Этот метод является возможностью приложения для работы с новым содержимым, включая обновление пользовательского интерфейса в соответствии с результатами перемещения:

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  // Handle new information, update UI, etc.
}
```

После завершения обработки нового содержимого мы вызываем обработчик завершения, чтобы система знала, что можно получить моментальный снимок приложения и вернуться в спящий режим:

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  var appDelegate = UIApplication.SharedApplication.Delegate as AppDelegate;

  // Handle new information, update UI, etc.

  // call completion handler when you're done
  if (appDelegate.backgroundSessionCompletionHandler != null) {
    NSAction handler = appDelegate.backgroundSessionCompletionHandler;
    appDelegate.backgroundSessionCompletionHandler = null;
    handler.Invoke ();
  }
}
```

В этом пошаговом руководстве мы рассмотрели основные шаги для реализации службы фоновой пересылки в iOS 7.

## <a name="related-links"></a>Связанные ссылки

- [Простая фоновая перенаправление (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/simplebackgroundtransfer)
