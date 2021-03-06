---
title: Обновление приложения Xamarin. iOS в фоновом режиме
description: В этом документе описываются различные способы обновления приложения Xamarin. iOS в фоновом режиме, например мониторинг региона, фоновая выборка и удаленные уведомления.
ms.prod: xamarin
ms.assetid: A2B2231A-C045-4C11-8176-F9966485197A
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 2d56af364d63ff78bafbdd7d8043ae4d75d97959
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306041"
---
# <a name="updating-a-xamarinios-app-in-the-background"></a>Обновление приложения Xamarin. iOS в фоновом режиме

Фоновое обновление — это процесс пробуждения приложения, которое приостановлено или не запущено, и его обновление новым содержимым. iOS предоставляет три варианта обновления содержимого в фоновом режиме:

1. *Мониторинг региона* и *значительное расположение. изменения* в API с учетом расположения службы активируют фоновые обновления в зависимости от изменений в расположении пользователя. Эти API-интерфейсы можно использовать с осторожностью для обновления содержимого в приложениях iOS 6, не относящихся к расположению, где другие параметры недоступны.
1. *Фоновая выборка (iOS 7 +)* — временный подход к обновлению *некритического* содержимого, которое *часто* обновляется.
1. *Удаленные уведомления (iOS 7 +)* . приложения, которые получают push-уведомления, могут использовать уведомления для активации обновления фонового содержимого. Этот метод можно использовать для обновления с *важным, зависящим от времени* содержимым, *которое обновляется в* период с временным обновлением.

В следующих разделах рассматриваются основные сведения об этих вариантах.

## <a name="region-monitoring-and-significant-location-changes"></a>Мониторинг региона и значительное изменение расположения

iOS предоставляет два API, поддерживающих расположение, с возможностями фонового изображения:

1. *Мониторинг региона* — это процесс настройки регионов с границами и пробуждения устройства при входе или выходе пользователя из региона. Регионы являются циклическими и могут иметь различные размеры. Когда пользователь пересекает границу области, устройство будет пробудиться для обработки события, как правило, путем срабатывания уведомления или запуска задачи. Мониторинг региона требует наличия GPS и увеличивает использование аккумулятора и данных.
1. *Служба существенных изменений расположения* — это более простой вариант, который позволяет использовать устройства с сотовыми радио. Приложение, слушающее значительные изменения расположения, будет уведомлено, когда устройство переключается на ячейку Towers. Эта служба может использоваться для пробуждения приостановленного или завершенного приложения и предоставляет возможность проверки нового содержимого в фоновом режиме. Фоновое действие ограничено до 10 секунд, если только не связано с [фоновой задачей](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md) .

Приложению не требуется расположение `UIBackgroundMode` для использования этих интерфейсов API, учитывающих расположение. Поскольку iOS не следит за типами задач, которые могут выполняться, когда устройство пробуждениися изменениями в расположении пользователя, эти API-интерфейсы предоставляют обходной путь для обновления содержимого в фоновом режиме на iOS 6. Помните *, что активация фоновых обновлений с помощью API-интерфейсов на основе расположения приведет к размещению ресурсов устройства и может запутать пользователей, которые не понимают, почему приложению требуется доступ к их расположению*. Используйте осторожность при реализации мониторинга региона или значительных изменений расположения для фоновой обработки в приложениях, которые еще не используют API расположения.

В приложениях, использующих мониторинг расположения для фоновой обработки, есть изъян в iOS 6. Если потребности приложения не помещаются в категорию, необходимую для фонового режима, это может быть ограничено. С появлением двух новых API, *фоновой выборки* и *удаленными уведомлениями*iOS 7 (и более поздние версии) предоставляет возможности для большего количества приложений. В следующих двух разделах представлены эти новые API-интерфейсы.

<a name="background_fetch" />

## <a name="background-fetch-ios-7-and-greater"></a>Фоновая выборка (iOS 7 и более поздние версии)

В iOS 6 приложение, которое вводит на передний план время, необходимое для загрузки нового содержимого, кратко демонстрирует пользователей с уже замеченным содержимым. Функция фоновой выборки позволяет приложениям загружать новые данные *до* запуска приложения пользователем и предоставлять пользователю наиболее актуальное содержимое.

Чтобы реализовать фоновую выборку, измените *info. plist* и установите флажки **включить фоновые режимы** и **Фоновая выборка** :

 [![](updating-an-application-in-the-background-images/fetch.png "Edit the Info.plist and check the Enable Background Modes and Background Fetch check boxes")](updating-an-application-in-the-background-images/fetch.png#lightbox)

Затем в `AppDelegate`Переопределите метод `FinishedLaunching`, чтобы задать минимальный интервал выборки. В этом примере мы позволяем ОС решить, как часто извлекать новое содержимое:

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
  UIApplication.SharedApplication.SetMinimumBackgroundFetchInterval (UIApplication.BackgroundFetchIntervalMinimum);
  return true;
}
```

Наконец, выполните выборку, переопределив метод `PerformFetch` в `AppDelegate`и передав *обработчик завершения*. Обработчик завершения — это делегат, который принимает `UIBackgroundFetchResult`:

```csharp
public override void PerformFetch (UIApplication application, Action<UIBackgroundFetchResult> completionHandler)
{
  // Check for new data, and display it
  ...
  
  // Inform system of fetch results
  completionHandler (UIBackgroundFetchResult.NewData);
}
```

Завершив обновление содержимого, мы разберем ОС, вызвав обработчик завершения с соответствующим состоянием. iOS предлагает три варианта состояния обработчика завершения:

1. `UIBackgroundFetchResult.NewData` — вызывается при выборке нового содержимого и обновлении приложения.
1. `UIBackgroundFetchResult.NoData` — вызывается, когда выбиралось новое содержимое, но содержимое недоступно.
1. `UIBackgroundFetchResult.Failed` — полезен для обработки ошибок, он вызывается, когда не удалось выполнить выборку.

Приложения, использующие функцию фоновой выборки, могут вызывать для обновления пользовательского интерфейса в фоновом режиме. Когда пользователь открывает приложение, Пользовательский интерфейс будет обновлен и отобразится новое содержимое. Это также приведет к обновлению моментального снимка переключателя приложения приложения, чтобы пользователь мог видеть, когда приложение имеет новое содержимое.

> [!IMPORTANT]
> После вызова `PerformFetch` приложение занимает примерно 30 секунд, чтобы начать загрузку нового содержимого и вызвать блок обработчика завершения. Если это занимает слишком много времени, приложение будет завершено. При загрузке мультимедиа или других больших файлов рассмотрите возможность использования фоновой выборки со _службой фоновой передачи_ .

### <a name="backgroundfetchinterval"></a>баккграундфетчинтервал

В приведенном выше примере кода мы позволяем операционной системе решить, как часто извлекать новое содержимое, установив для интервала минимальной выборки значение `BackgroundFetchIntervalMinimum`. iOS предлагает три варианта интервала выборки:

1. `BackgroundFetchIntervalNever` — укажите, что система никогда не извлекает новое содержимое. Используйте этот параметр, чтобы отключить выборку в определенных ситуациях, например, если пользователь не вошел в систему. Это значение по умолчанию для интервала выборки. 
1. `BackgroundFetchIntervalMinimum`. Позвольте системе решить, как часто следует выбирать на основе пользовательских шаблонов, времени работы батареи, использования данных и потребностей других приложений.
1. `BackgroundFetchIntervalCustom`. Если известно, как часто обновляется содержимое приложения, можно указать интервал "спящего режима" после каждой выборки, в течение которого приложение не сможет получить новое содержимое. По истечении этого интервала система определит, когда следует получать содержимое.

И `BackgroundFetchIntervalMinimum`, и `BackgroundFetchIntervalCustom` используют систему для планирования выборки. Этот интервал динамически адаптируется к потребностям устройства, а также к привычкам конкретного пользователя. Например, если один пользователь проверяет приложение каждую утро, а второй проверяет каждый час, iOS обеспечит актуальность содержимого для обоих пользователей при каждом открытии приложения.

Фоновая выборка должна использоваться для приложений, которые часто обновляются с некритическим содержимым. Для приложений с критически важными обновлениями следует использовать удаленные уведомления. Удаленные уведомления основываются на фоновой выборке и используют один и тот же обработчик завершения. Далее мы рассмотрим удаленные уведомления.

 <a name="remote_notifications" />

## <a name="remote-notifications-ios-7-and-greater"></a>Удаленные уведомления (iOS 7 и более поздние версии)

Push-уведомления — это сообщения JSON, отправляемые от поставщика на устройство посредством *службы push-уведомлений Apple (APNs)* .

В iOS 6 входящие push-уведомления сообщают системе о предупреждении пользователя о том, что в приложении возникли какие-то интересные события. Если щелкнуть уведомление, приложение перейдет в приостановленное или прерванное состояние, и приложение начнет обновлять содержимое. iOS 7 (и более поздние версии) расширяет обычные push-уведомления, предоставляя приложениям возможность обновлять содержимое в фоновом режиме *перед* уведомлением пользователя, чтобы пользователь мог открыть приложение и немедленно представить новое содержимое.

Чтобы реализовать удаленные уведомления, измените *info. plist* и установите флажки **включить фоновые режимы** и **Удаленные уведомления** :

 [![](updating-an-application-in-the-background-images/remote.png "Background Mode set to Enable Background Modes and Remote notifications")](updating-an-application-in-the-background-images/remote.png#lightbox)

Затем установите флаг `content-available` для самого push-уведомления в значение 1. Это позволяет приложению получить новое содержимое перед отображением оповещения:

```csharp
'aps' {
  'content-available': 1,
  'alert': 'Something new has happened in your app!''
}
```

В *AppDelegate*Переопределите метод `DidReceiveRemoteNotification`, чтобы проверить полезные данные уведомления для доступного содержимого, и вызовите соответствующий блок обработчика завершения:

```csharp
public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
  if([content-available]) {
    // fetch content
    completionHandler (UIBackgroundFetchResult.NewData);
  }
}
```

Удаленные уведомления следует использовать для редко встречающихся обновлений с содержимым, решающим функциональность приложения. Дополнительные сведения об удаленных уведомлениях см. в статье [Push-уведомления Xamarin в iOS](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md) .

> [!IMPORTANT]
> Поскольку механизм обновления в удаленных уведомлениях основан на фоновой выборке, приложение должно запускать загрузку нового содержимого и вызывать блок обработчика завершения в течение 30 секунд после получения уведомления, иначе iOS завершит работу приложения. Рассмотрите возможность связывания удаленных уведомлений с _фоновой службой передачи_ при загрузке мультимедиа или других больших файлов в фоновом режиме.

### <a name="silent-remote-notifications"></a>Удаленные уведомления без уведомлений

Удаленные уведомления — это простой способ уведомления приложений об обновлениях и запуска получения нового содержимого, но в некоторых случаях нам не нужно уведомлять пользователя о том, что что-то изменилось. Например, если пользователь помечает файл для синхронизации, не нужно уведомлять о них каждый раз при обновлении файла. Синхронизация файлов не является удивительным событием и не требует немедленного вмешательства пользователя. Пользователи сразу же хотят, чтобы файл был в актуальном состоянии при его открытии.

В случаях, аналогичных приведенным выше, iOS позволяет отправлять push-уведомления автоматически, т. е. без предупреждения. Чтобы преобразовать регулярное уведомление в сообщение без уведомления, просто удалите оповещение из полезных данных уведомления:

```csharp
'aps' {
  'content-available': 1
}
```

#### <a name="rate-limits"></a>Ограничения скорости

Наибольшее различие между обычными и автоматическими уведомлениями с точки зрения разработчика заключается в том, что автоматическая отправка push-уведомлений ограничена. APNs задерживает доставку сообщений на устройство без уведомления, если скорость push-уведомлений слишком высока. Это гарантирует, что приложения не забрасывают ресурсы устройства с помощью слишком большого количества автоматических уведомлений.

Тем не менее APNs будет разрешать автоматическому уведомлению "проникновения" с обычным удаленным уведомлением или ответом на проверку активности. Так как для обычных уведомлений не применяется ограничение частоты, их можно использовать для отправки в устройство уведомлений о неавтоматическом просмотре с APNs, как показано на следующей схеме:

 [![](updating-an-application-in-the-background-images/silent.png "Regular notifications can be used to push stored silent notifications from the APNs to the device, as illustrated by this diagram")](updating-an-application-in-the-background-images/silent.png#lightbox)

> [!IMPORTANT]
> Компания Apple рекомендует разработчикам отправлять автоматические push-уведомления каждый раз, когда требуется приложение, и позволить APNs запланировать доставку.

В этом разделе мы рассмотрели различные варианты обновления содержимого в фоновом режиме, чтобы выполнять задачи, которые не помещаются в категорию, необходимую для фоновой работы. Теперь давайте посмотрим некоторые из этих API-интерфейсов в действии.

 [Далее. часть 4. пошаговые руководства по работе с iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
