---
title: Критические оповещения в Xamarin. iOS
description: В этом документе описывается использование критических оповещений с помощью Xamarin. iOS. Критические оповещения, появившиеся в iOS 12, — это неустранимые уведомления, которые воспроизводят звук независимо от того, включен ли параметр не беспокоить или выключен.
ms.prod: xamarin
ms.assetid: 75742257-081D-44F4-B49E-FB807DF85262
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/04/2018
ms.openlocfilehash: 54a214215f77b66f6a4b134dcb8d27b26c44fb6c
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291293"
---
# <a name="critical-alerts-in-xamarinios"></a>Критические оповещения в Xamarin. iOS

С iOS 12 приложения могут отсылать критические оповещения. Критические оповещения воспроизводят звук независимо от того, включено ли или не беспокоить, или выключено действие звонка. Эти уведомления нарушают работу, и их следует использовать только в том случае, если пользователи должны предпринять немедленные действия.

## <a name="custom-critical-alert-entitlement"></a>Настраиваемый уровень обслуживания критических оповещений

Чтобы отобразить критические оповещения в приложении, сначала [запросите уведомления о настраиваемых критических оповещениях](https://developer.apple.com/contact/request/notifications-critical-alerts-entitlement/) от Apple.

После получения этого права от Apple и выполнения всех связанных инструкций о настройке [приложения для его использования Добавьте настраиваемое](~/ios/deploy-test/provisioning/entitlements.md) право в файлы прав вашего приложения **. plist** . Затем настройте параметры **подписывания пакета iOS** для использования прав **. plist** при подписывании приложения на симуляторе и на устройстве.

## <a name="request-authorization"></a>Запрос авторизации

Запрос на авторизацию уведомления приложения предлагает пользователю разрешить или запретить уведомления приложения. Если запрос на авторизацию уведомления запрашивает разрешение на отправку критических оповещений, приложение также предоставит пользователю возможность принять участие в критических оповещениях.

Следующий код запрашивает разрешение на отправку критических предупреждений и стандартных уведомлений и звуков путем передачи соответствующего[`UNAuthorizationOptions`](xref:UserNotifications.UNAuthorizationOptions)
[`RequestAuthorization`](xref:UserNotifications.UNUserNotificationCenter.RequestAuthorization*)значения:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.CriticalAlert;
    center.RequestAuthorization(options, (bool success, NSError error) => {
        // ...
    );
    return true;
}
```

## <a name="local-critical-alerts"></a>Локальные критические оповещения

Чтобы отправить локальное критическое оповещение, создайте[`UNMutableNotificationContent`](xref:UserNotifications.UNMutableNotificationContent)
и задайте для `Sound` его свойства значение:

- `UNNotificationSound.DefaultCriticalSound`, который использует звуковое оповещение по умолчанию.
- `UNNotificationSound.GetCriticalSound`, который позволяет указать пользовательский звук, объединенный с вашим приложением и томом.

Затем создайте `UNNotificationRequest` из содержимого уведомления и добавьте его в центр уведомлений:

```csharp
var content = new UNMutableNotificationContent()
{
    Title = "Critical alert title",
    Body = "Text of the critical alert",
    CategoryIdentifier = "my-critical-alert-category",
    // Sound = UNNotificationSound.DefaultCriticalSound
    Sound = UNNotificationSound.GetCriticalSound("my_critical_sound.m4a", 1.0f)
};

var request = UNNotificationRequest.FromIdentifier(
    Guid.NewGuid().ToString(),
    content,
    UNTimeIntervalNotificationTrigger.CreateTrigger(3, false)
);

var center = UNUserNotificationCenter.Current;
center.AddNotificationRequest(request, null);
```

> [!IMPORTANT]
> Критические оповещения не будут доставляться, если они не включены в приложении. Вместе с запросом, который появляется в первый раз, когда приложение запрашивает разрешение на отправку критических предупреждений, пользователь может включить или отключить критические оповещения в разделе **уведомлений** приложения в приложении " **Параметры** iOS".

## <a name="remote-critical-alerts"></a>Удаленные критические оповещения

Дополнительные сведения об удаленных критических оповещениях см. в разделе [что нового в сеансе уведомлений пользователей](https://developer.apple.com/videos/play/wwdc2018/710/) из ввдк 2018 и в статье [Создание удаленного документа уведомления](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification) .

## <a name="related-links"></a>Связанные ссылки

- [Платформа уведомлений пользователей в Xamarin. iOS](~/ios/platform/user-notifications/index.md)
- [Усернотификатионс (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [Новые возможности уведомлений пользователей (ВВДК 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Рекомендации и новые возможности уведомлений пользователей (ВВДК 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Создание удаленного уведомления (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
