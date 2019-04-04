---
title: Уведомлений в Xamarin.Android
ms.prod: xamarin
ms.assetid: 2E54F1D0-45F4-43A7-B3A3-4F483B7150CB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: b98ee5afbd65d5cf32bc6e3151284678e248cf47
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2019
ms.locfileid: "57666845"
---
# <a name="notifications-in-xamarinandroid"></a>Уведомлений в Xamarin.Android


## <a name="overview"></a>Обзор

В этом разделе объясняется, как реализовать уведомления в Xamarin.Android. Он описывает различные элементы пользовательского интерфейса уведомление Android и описываются API связанных с созданием и отображение уведомления.


## <a name="sections"></a>Разделы

### <a name="local-notifications-in-androidlocal-notificationsmd"></a>[Локальные уведомления в Android](local-notifications.md)

В этом разделе объясняется, как реализовать локальных уведомлений в Xamarin.Android. Он описывает различные элементы пользовательского интерфейса уведомление Android и рассматриваются API-Интерфейс связанных с созданием и отображение уведомления. 

### <a name="walkthrough---using-local-notifications-in-xamarinandroidlocal-notifications-walkthroughmd"></a>[Пошаговое руководство. Использование локальных уведомлений в Xamarin.Android](local-notifications-walkthrough.md)  
 
В этом пошаговом руководстве описывается использование локальных уведомлений в приложение Xamarin.Android. Он демонстрирует основные принципы создания и публикации уведомления. При щелчке уведомления на панели уведомлений о запуске второго действия. 


## <a name="for-further-reading"></a>Дополнительные сведения

[Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) &ndash; Firebase Cloud Messaging (FCM) — это служба, которая облегчает обмен сообщениями между мобильные приложения и серверные приложения. Firebase Cloud Messaging можно использовать для реализации удаленных уведомлений (также называемые Push-уведомления) в приложениях Xamarin.Android.

[Уведомления](https://developer.android.com/guide/topics/ui/notifiers/notifications.html) &ndash; разделе This разработчика Android — полное руководство для уведомлений Android. Она включает рекомендации раздел, который поможет вам разработать уведомлений, таким образом, чтобы они соответствуют рекомендациям Android пользовательского интерфейса для макета. Предоставляет дополнительные сведения о навигации preserviing, при запуске действия, и здесь объясняется, как отображение хода выполнения в уведомлений и управления воспроизведением мультимедиа на экране блокировки. 

[NotificationListenerService](https://developer.xamarin.com/api/type/Android.Service.Notification.NotificationListenerService/) &ndash; Android эта служба делает возможным для вашего приложения на прослушивание (и взаимодействовать с) учтены все уведомления на устройстве Android, а не только уведомления, которые ваше приложение будет зарегистрировано для получения. Обратите внимание на то, что пользователю необходимо явно предоставить разрешения приложению для того, чтобы иметь возможность прослушивать уведомления на устройстве.





## <a name="related-links"></a>Связанные ссылки

- [Локальные уведомления (пример)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Удаленные уведомления (пример)](https://developer.xamarin.com/samples/monodroid/RemoteNotifications/)
