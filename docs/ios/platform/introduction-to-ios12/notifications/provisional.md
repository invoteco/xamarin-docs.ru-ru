---
title: Подготовка уведомлений в Xamarin. iOS
description: В этом документе описывается, как использовать Xamarin. iOS для работы с подготовленными уведомлениями. Подготовленные уведомления, появившиеся в iOS 12, позволяют приложениям отправлять тихий уведомления без явного разрешения пользователя.
ms.prod: xamarin
ms.assetid: 5DCB36B9-2637-48AE-8FC0-F6124F08AC48
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/04/2018
ms.openlocfilehash: 7d9fe2a651d8d75d8dd9d8c0dd1225350a58373d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031885"
---
# <a name="provisional-notifications-in-xamarinios"></a>Подготовка уведомлений в Xamarin. iOS

Подготовленные уведомления позволяют приложениям доставлять уведомления без явного согласия пользователя. Эти уведомления поступают в незаметном виде и отображаются только в центре уведомлений, что позволяет пользователям просматривать их перед продолжением или отказаться от их непрерывной доставки.

В центре уведомлений пользователи могут указать, что приложение должно прекратить доставку подготовительных уведомлений, продолжить их подготовку или сделать их более наглядными.

## <a name="sample-app-redgreennotifications"></a>Пример приложения: Редгриннотификатионс

Ознакомьтесь с примером приложения [редгриннотификатионс](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications) , которое отправляет подготовленные уведомления.

## <a name="sending-provisional-notifications"></a>Отправка подготовленных уведомлений

Чтобы отправить подготовленные уведомления, укажите `UNAuthorizationOptions.Provisional` в качестве параметра для [`RequestAuthorization`](xref:UserNotifications.UNUserNotificationCenter.RequestAuthorization*)
метод `UNUserNotificationCenter`:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.Provisional;
    center.RequestAuthorization(options, (bool success, NSError error) => {
        // ...
    );
    return true;
}
```

Если пользователь продвигает подготовленные уведомления для выразительной доставки, `UNAuthorizationOptions` значения, переданные в `RequestAuthorization`, будут определять новые параметры доставки уведомлений (в приведенном выше коде `UNAuthorizationOptions.Alert` и `UNAuthorizationOptions.Sound`).

## <a name="related-links"></a>Связанные ссылки

- [Пример приложения — Редгриннотификатионс](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications)
- [Платформа уведомлений пользователей в Xamarin. iOS](~/ios/platform/user-notifications/index.md)
- [Усернотификатионс (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [Новые возможности уведомлений пользователей (ВВДК 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Рекомендации и новые возможности уведомлений пользователей (ВВДК 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Создание удаленного уведомления (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
