---
title: Уведомления службы
description: В этом руководство объясняется, как служба Android может использовать локальные уведомления для отправки информации пользователю.
ms.prod: xamarin
ms.assetid: 6C06FDE7-6385-40EF-AC7C-8EFB54E29F45
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 5d25604db1f88702f4c24df21b3ebba6c9c2fc95
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70754818"
---
# <a name="service-notifications"></a>Уведомления службы

_В этом руководство объясняется, как служба Android может использовать локальные уведомления для отправки информации пользователю._

## <a name="service-notifications-overview"></a>Общие сведения об уведомлениях службы

Уведомления службы позволяют приложению отображать сведения пользователю, даже если приложение Android не находится на переднем плане. Уведомление может предоставить пользователю действия, например отображение действия из приложения. В следующем примере кода показано, как служба может отправлять уведомления пользователю.

```csharp
[Service]
public class MyService: Service 
{
    // A notification requires an id that is unique to the application.
    const int NOTIFICATION_ID = 9000;
    
    public override StartCommandResult OnStartCommand(Intent intent, StartCommandFlags flags, int startId)
    {
        // Code omitted for clarity - here is where the service would do something.
    
        // Work has finished, now dispatch anotification to let the user know.
        Notification.Builder notificationBuilder = new Notification.Builder(this)
            .SetSmallIcon(Resource.Drawable.ic_notification_small_icon)
            .SetContentTitle(Resources.GetString(Resource.String.notification_content_title))
            .SetContentText(Resources.GetString(Resource.String.notification_content_text));
        
        var notificationManager = (NotificationManager)GetSystemService(NotificationService);
        notificationManager.Notify(NOTIFICATION_ID, notificationBuilder.Build());
    }
}
```

На этом снимке экрана показан пример отображаемого уведомления:

[![Значок уведомления, отображаемый в строке состояния](service-notifications-images/01-notification-sml.png)](service-notifications-images/01-notification.png#lightbox)

Когда пользователь выводит на экран уведомление в верхней части экрана, отображается полное уведомление:

![Оповещение, отображаемое в области уведомлений](service-notifications-images/02-fullnotification.png)

## <a name="updating-a-notification"></a>Обновление уведомления

Чтобы обновить уведомление, служба повторно опубликует уведомление, используя тот же идентификатор уведомления. При необходимости Android отобразит или обновит уведомление в строке состояния.

```csharp 
void UpdateNotification(string content)
{
   var notification = GetNotification(content, pendingIntent);

   NotificationManager notificationManager = (NotificationManager)GetSystemService(Context.NotificationService);
   notificationManager.Notify(NOTIFICATION_ID, notification);
}

Notification GetNotification(string content, PendingIntent intent)
{
   return new Notification.Builder(this)
           .SetContentTitle(tag)
           .SetContentText(content)
           .SetSmallIcon(Resource.Drawable.NotifyLg)
           .SetLargeIcon(BitmapFactory.DecodeResource(Resources, Resource.Drawable.Icon))
           .SetContentIntent(intent).Build();
}
```

Дополнительные сведения об уведомлениях см. в разделе [локальные уведомления](~/android/app-fundamentals/notifications/local-notifications.md) в статье [об уведомлениях Android](~/android/app-fundamentals/notifications/index.md) .

## <a name="related-links"></a>Связанные ссылки

- [Локальные уведомления в Android](~/android/app-fundamentals/notifications/local-notifications.md)
