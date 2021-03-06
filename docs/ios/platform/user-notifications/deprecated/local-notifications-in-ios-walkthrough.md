---
title: Пошаговое руководство. Использование локальных уведомлений в Xamarin. iOS
description: В этом разделе мы рассмотрим, как использовать локальные уведомления в приложении Xamarin. iOS. В нем демонстрируются основы создания и публикации уведомления, которое будет отображать оповещение при получении приложением.
ms.prod: xamarin
ms.assetid: 32B9C6F0-2BB3-4295-99CB-A75418969A62
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 764be6319e95b16dc043bebd2abfb27ba0696457
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031406"
---
# <a name="walkthrough---using-local-notifications-in-xamarinios"></a>Пошаговое руководство. Использование локальных уведомлений в Xamarin. iOS

_В этом разделе мы рассмотрим, как использовать локальные уведомления в приложении Xamarin. iOS. В нем демонстрируются основы создания и публикации уведомления, которое будет отображать оповещение при получении приложением._

> [!IMPORTANT]
> Сведения в этом разделе относятся к iOS 9 и более ранним версиям, поэтому она оставлена для поддержки более старых версий iOS. Для iOS 10 и более поздних версий ознакомьтесь с [руководством по платформе пользовательских уведомлений](~/ios/platform/user-notifications/index.md) для поддержки локальных и удаленных уведомлений на устройстве iOS.

## <a name="walkthrough"></a>Пошаговое руководство

Позвольте создать простое приложение, которое будет отображать локальные уведомления в действии. Это приложение будет иметь одну кнопку. При нажатии кнопки будет создано локальное уведомление. По истечении указанного периода времени будет отображено уведомление.

1. В Visual Studio для Mac создайте новое решение для iOS с одним представлением и вызовите его `Notifications`.
1. Откройте файл `Main.storyboard` и перетащите кнопку на представление. Назовите кнопку **кнопки и**присвойте ей название **добавить уведомление**. В этот момент может потребоваться задать некоторые [ограничения](~/ios/user-interface/designer/designer-auto-layout.md) для кнопки: 

    ![](local-notifications-in-ios-walkthrough-images/image3.png "Setting some constraints on the button")
1. Измените класс `ViewController` и добавьте следующий обработчик событий в метод ViewDidLoad:

    ```csharp
    button.TouchUpInside += (sender, e) =>
    {
        // create the notification
        var notification = new UILocalNotification();

        // set the fire date (the date time in which it will fire)
        notification.FireDate = NSDate.FromTimeIntervalSinceNow(60);

        // configure the alert
        notification.AlertAction = "View Alert";
        notification.AlertBody = "Your one minute alert has fired!";

        // modify the badge
        notification.ApplicationIconBadgeNumber = 1;

        // set the sound to be the default sound
        notification.SoundName = UILocalNotification.DefaultSoundName;

        // schedule it
        UIApplication.SharedApplication.ScheduleLocalNotification(notification);
    };
    ```

    Этот код создает уведомление, использующее звук, устанавливает значение значка значок 1 и отображает предупреждение для пользователя.

1. Затем измените файл `AppDelegate.cs`. сначала добавьте следующий код в метод `FinishedLaunching`. Мы проверили, работает ли устройство под управлением iOS 8, если это так, поэтому **нам нужно запросить** разрешение пользователя на получение уведомлений:

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
        var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes (
            UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
        );

        application.RegisterUserNotificationSettings (notificationSettings);
    }
    ```

1. По-прежнему в `AppDelegate.cs`добавьте следующий метод, который будет вызываться при получении уведомления:

    ```csharp
    public override void ReceivedLocalNotification(UIApplication application, UILocalNotification notification)
    {
        // show an alert
        UIAlertController okayAlertController = UIAlertController.Create(notification.AlertAction, notification.AlertBody, UIAlertControllerStyle.Alert);
        okayAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));

        UIApplication.SharedApplication.KeyWindow.RootViewController.PresentViewController(okayAlertController, true, null);

        // reset our badge
        UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
    }
    ```

1. Необходимо обменять случай, когда уведомление было запущено из-за локального уведомления. Измените метод `FinishedLaunching` в `AppDelegate`, чтобы включить следующий фрагмент кода:

    ```csharp
    // check for a notification

    if (launchOptions != null)
    {
        // check for a local notification
        if (launchOptions.ContainsKey(UIApplication.LaunchOptionsLocalNotificationKey))
        {
            var localNotification = launchOptions[UIApplication.LaunchOptionsLocalNotificationKey] as UILocalNotification;
            if (localNotification != null)
            {
                UIAlertController okayAlertController = UIAlertController.Create(localNotification.AlertAction, localNotification.AlertBody, UIAlertControllerStyle.Alert);
                okayAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));

                Window.RootViewController.PresentViewController(okayAlertController, true, null);

                // reset our badge
                UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
            }
        }
    }
    ```

1. Наконец, запустите приложение. В iOS 8 вам будет предложено разрешить уведомления. Нажмите кнопку **ОК** , а затем нажмите кнопку **добавить уведомление** . После небольшой паузы вы увидите диалоговое окно предупреждения, как показано на следующих снимках экрана:

    ![](local-notifications-in-ios-walkthrough-images/image0.png "Подтверждение возможности отправки уведомлений") ![](local-notifications-in-ios-walkthrough-images/image1.png "Кнопка добавления уведомления")
    ![](local-notifications-in-ios-walkthrough-images/image2.png "The notification alert dialog")

## <a name="summary"></a>Сводка

В этом пошаговом руководстве показано, как использовать различные API для создания и публикации уведомлений в iOS. Также показано, как обновить значок приложения с помощью значка, чтобы предоставить пользователю специальные отзывы.

## <a name="related-links"></a>Связанные ссылки

- [Локальные уведомления (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/localnotifications)
- [Инструкции по программированию локальных и push-уведомлений](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
