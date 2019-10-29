---
title: Пользовательские интерфейсы интерактивного уведомления в Xamarin. iOS
description: В iOS 12 можно создавать интерактивные пользовательские интерфейсы для локальных и удаленных уведомлений. В этом руководство описано, как использовать эти функции с Xamarin. iOS.
ms.prod: xamarin
ms.assetid: E3562E1B-E0EF-4C99-9F51-59DE22AFDE46
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/04/2018
ms.openlocfilehash: e629cd8f481558991d02c7fb879502ebd54753bd
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031941"
---
# <a name="interactive-notification-user-interfaces-in-xamarinios"></a>Пользовательские интерфейсы интерактивного уведомления в Xamarin. iOS

[Расширения содержимого уведомлений](~/ios/platform/user-notifications/advanced-user-notifications.md), появившиеся в iOS 10, позволяют создавать настраиваемые пользовательские интерфейсы для уведомлений. Начиная с iOS 12, пользовательские интерфейсы уведомлений могут содержать интерактивные элементы, такие как кнопки и ползунки.

## <a name="sample-app-redgreennotifications"></a>Пример приложения: Редгриннотификатионс

Пример приложения [редгриннотификатионс](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications) содержит расширение содержимого уведомлений с интерактивным пользовательским интерфейсом.

Фрагменты кода в этом пошаговом окне взяты из этого примера.

## <a name="notification-content-extension-infoplist-file"></a>Файл сведений о расширении содержимого уведомлений. plist

В примере приложения файл **info. plist** в проекте **редгриннотификатионсконтентекстенсион** содержит следующую конфигурацию:

```xml
<!-- ... -->
<key>NSExtension</key>
<dict>
    <key>NSExtensionAttributes</key>
    <dict>
        <key>UNNotificationExtensionCategory</key>
        <array>
            <string>red-category</string>
            <string>green-category</string>
        </array>
        <key>UNNotificationExtensionUserInteractionEnabled</key>
        <true/>
        <key>UNNotificationExtensionDefaultContentHidden</key>
        <true/>
        <key>UNNotificationExtensionInitialContentSizeRatio</key>
        <real>0.6</real>
    </dict>
    <key>NSExtensionMainStoryboard</key>
    <string>MainInterface</string>
    <key>NSExtensionPointIdentifier</key>
    <string>com.apple.usernotifications.content-extension</string>
    <key></key>
    <true/>
</dict>
<!-- ... -->
```

Обратите внимание на следующие возможности.

- `UNNotificationExtensionCategory` массив указывает тип категорий уведомлений, обрабатываемых расширением содержимого.
- Для поддержки интерактивного содержимого расширение содержимого уведомлений устанавливает для `UNNotificationExtensionUserInteractionEnabled` ключа значение `true`.
- Ключ `UNNotificationExtensionInitialContentSizeRatio` задает начальное соотношение высоты и ширины для интерфейса расширения содержимого.

## <a name="interactive-interface"></a>Интерактивный интерфейс

**Маининтерфаце. Storyboard**, определяющий интерфейс для расширения содержимого уведомлений, представляет собой стандартную раскадровку, содержащую один контроллер представления. В примере приложения контроллер представления имеет тип `NotificationViewController`и содержит представление изображений, три кнопки и ползунок. Раскадровка связывает эти элементы управления с обработчиками, определенными в **NotificationViewController.CS**:

- Обработчик кнопки **запустить приложение** вызывает метод действия `PerformNotificationDefaultAction` для `ExtensionContext`, который запускает приложение:

    ```csharp
    partial void HandleLaunchAppButtonTap(UIButton sender)
    {
        ExtensionContext.PerformNotificationDefaultAction();
    }
    ```

    В приложении `Delegate` центра уведомлений пользователей (в примере приложения `AppDelegate`) может реагировать на взаимодействие в методе `DidReceiveNotificationResponse`:

    ```csharp
    [Export("userNotificationCenter:didReceiveNotificationResponse:withCompletionHandler:")]
    public void DidReceiveNotificationResponse(UNUserNotificationCenter center, UNNotificationResponse response, System.Action completionHandler)
    {
        if (response.IsDefaultAction)
        {
            Console.WriteLine("ACTION: Default");
            // ...
    ```

- Обработчик кнопки **закрытия уведомления** вызывает `DismissNotificationContentExtension` на `ExtensionContext`, который закрывает уведомление:

    ```csharp
    partial void HandleDismissNotificationButtonTap(UIButton sender)
    {
        ExtensionContext.DismissNotificationContentExtension();
    }
    ```

- Обработчик кнопки **удалить уведомление** закрывает уведомление и удаляет его из центра уведомлений:

    ```csharp
    partial void HandleRemoveNotificationButtonTap(UIButton sender)
    {
        ExtensionContext.DismissNotificationContentExtension();
        UNUserNotificationCenter.Current.RemoveDeliveredNotifications(new string[] { notification.Request.Identifier });
    }
    ```

- Метод, обрабатывающий изменения значений на ползунке, обновляет альфа изображения, отображаемого в интерфейсе уведомления:

    ```csharp
    partial void HandleSliderValueChanged(UISlider sender)
    {
        Xamagon.Alpha = sender.Value;
    }
    ```

## <a name="related-links"></a>Связанные ссылки

- [Пример приложения — Редгриннотификатионс](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications)
- [Платформа уведомлений пользователей в Xamarin. iOS](~/ios/platform/user-notifications/index.md)
- [Усернотификатионс (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [Новые возможности уведомлений пользователей (ВВДК 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Рекомендации и новые возможности уведомлений пользователей (ВВДК 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Расширенные уведомления (ВВДК 2017)](https://developer.apple.com/videos/play/wwdc2017/817/)
- [Создание удаленного уведомления (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
