---
title: Управление уведомлениями в Xamarin. iOS
description: В этом документе описывается использование Xamarin. iOS для использования новых функций управления уведомлениями, появившихся в iOS 12.
ms.prod: xamarin
ms.assetid: F1D90729-F85A-425B-B633-E2FA38FB4A0C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/04/2018
ms.openlocfilehash: 671b6c00a41d719a7ccb8247fd4a7bc008d91adf
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031894"
---
# <a name="notification-management-in-xamarinios"></a>Управление уведомлениями в Xamarin. iOS

В iOS 12 операционная система может получить глубокую ссылку из центра уведомлений и приложения параметров на экран управления уведомлениями приложения. Этот экран должен дать пользователям возможность принять и отказаться от различных типов уведомлений, которые отправляет приложение.

## <a name="sample-app-redgreennotifications"></a>Пример приложения: Редгриннотификатионс

Чтобы увидеть пример работы управления уведомлениями, ознакомьтесь с примером приложения [редгриннотификатионс](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications) .

Этот пример приложения отправляет два типа уведомлений — красный и зеленый — и предоставляет экран, позволяющий пользователям принять или отказаться от любого из этих типов.

Фрагменты кода в этом пошаговом окне поступают из этого примера приложения.

## <a name="notification-management-screen"></a>Экран управления уведомлениями

В примере приложения `ManageNotificationsViewController` определяет пользовательский интерфейс, который позволяет пользователям независимо включать и отключать красные уведомления и зеленые уведомления. Это Стандартный [`UIViewController`](xref:UIKit.UIViewController)
содержит [`UISwitch`](xref:UIKit.UISwitch) для каждого типа уведомления. При переключении переключателя для любого типа сохранений уведомлений в настройках пользователя по умолчанию предпочтения пользователя для этого типа уведомлений:

```csharp
partial void HandleRedNotificationsSwitchValueChange(UISwitch sender)
{
    NSUserDefaults.StandardUserDefaults.SetBool(sender.On, RedNotificationsEnabledKey);
}
```

> [!NOTE]
> На экране управления уведомлениями также проверяется, полностью отключил ли пользователь уведомления для приложения. В этом случае переключатели для отдельных типов уведомлений скрываются. Для этого на экране управления уведомлениями выполните следующие действия.
>
> - Вызывает [`UNUserNotificationCenter.Current.GetNotificationSettingsAsync`](xref:UserNotifications.UNUserNotificationCenter.GetNotificationSettingsAsync) и проверяет свойство [`AuthorizationStatus`](xref:UserNotifications.UNNotificationSettings.AuthorizationStatus) .
> - Скрывает переключатели для отдельных типов уведомлений, если для приложения были полностью отключены уведомления.
> - Повторно проверяет, были ли отключены уведомления при каждом перемещении приложения на передний план, так как пользователь может в любое время включить или отключить уведомления в параметрах iOS.

В примере класс `ViewController` приложения, который отправляет уведомления, перед отправкой локального уведомления установите флажок, чтобы убедиться, что уведомление относится к типу, который пользователь хочет получить:

```csharp
partial void HandleTapRedNotificationButton(UIButton sender)
{
    bool redEnabled = NSUserDefaults.StandardUserDefaults.BoolForKey(ManageNotificationsViewController.RedNotificationsEnabledKey);
    if (redEnabled)
    {
        // ...
```

## <a name="deep-link"></a>Прямая ссылка

глубокие ссылки iOS на экран управления уведомлениями приложения из центра уведомлений и параметры уведомлений приложения в приложении "Параметры". Чтобы упростить это, приложение должно:

- Укажите, что вы можете получить доступ к экрану управления уведомлениями, передав `UNAuthorizationOptions.ProvidesAppNotificationSettings` запрос авторизации уведомления приложения.
- Реализуйте метод `OpenSettings` из [`IUNUserNotificationCenterDelegate`](xref:UserNotifications.IUNUserNotificationCenterDelegate).

### <a name="authorization-request"></a>Запрос авторизации

Чтобы указать операционной системе, что доступен экран управления уведомлениями, приложение должно передать параметр `UNAuthorizationOptions.ProvidesAppNotificationSettings` (вместе с другими нужными параметрами доставки уведомлений) в метод `RequestAuthorization` на `UNUserNotificationCenter`.

Например, в `AppDelegate`примера приложения:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    // Request authorization to send notifications
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.ProvidesAppNotificationSettings | UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.Provisional;
    center.RequestAuthorization(options, (bool success, NSError error) =>
    {
        // ...
```

### <a name="opensettings-method"></a>Метод Опенсеттингс

Метод `OpenSettings`, вызванный системой для глубокой ссылки на экран управления уведомлениями приложения, должен перейти непосредственно к этому экрану.

В примере приложения этот метод выполняет перехода в `ManageNotificationsViewController` при необходимости:

```csharp
[Export("userNotificationCenter:openSettingsForNotification:")]
public void OpenSettings(UNUserNotificationCenter center, UNNotification notification)
{
    var navigationController = Window.RootViewController as UINavigationController;
    if (navigationController != null)
    {
        var currentViewController = navigationController.VisibleViewController;
        if (currentViewController is ViewController)
        {
            currentViewController.PerformSegue(ManageNotificationsViewController.ShowManageNotificationsSegue, this);
        }

    }
}
```

## <a name="related-links"></a>Связанные ссылки

- [Пример приложения — Редгриннотификатионс](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications)
- [Платформа уведомлений пользователей в Xamarin. iOS](~/ios/platform/user-notifications/index.md)
- [Усернотификатионс (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [Новые возможности уведомлений пользователей (ВВДК 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Рекомендации и новые возможности уведомлений пользователей (ВВДК 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Создание удаленного уведомления (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
