---
title: Уведомления в Xamarin. Android
ms.prod: xamarin
ms.assetid: 2E54F1D0-45F4-43A7-B3A3-4F483B7150CB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 57c77b5d8d6a4763dc7b5c7a62a51ffe3137827d
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70755368"
---
# <a name="notifications-in-xamarinandroid"></a>Уведомления в Xamarin. Android

В этом разделе объясняется, как реализовать уведомления в Xamarin. Android. В нем описываются различные элементы пользовательского интерфейса уведомления Android и обсуждаются API, связанные с созданием и отображением уведомлений.

## <a name="local-notifications-in-androidlocal-notificationsmd"></a>[Локальные уведомления в Android](local-notifications.md)

В этом разделе объясняется, как реализовать локальные уведомления в Xamarin. Android. Здесь описываются различные элементы пользовательского интерфейса уведомления Android и обсуждаются интерфейсы API, связанные с созданием и отображением уведомлений.

## <a name="walkthrough---using-local-notifications-in-xamarinandroidlocal-notifications-walkthroughmd"></a>[Пошаговое руководство. Использование локальных уведомлений в Xamarin. Android](local-notifications-walkthrough.md)  

В этом пошаговом руководстве описывается, как использовать локальные уведомления в приложении Xamarin. Android. В нем демонстрируются основы создания и публикации уведомления. Когда пользователь щелкает уведомление в ящике уведомлений, он начинает второе действие. 

## <a name="further-reading"></a>Дополнительные сведения

[Firebase облачные сообщения](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) &ndash; Firebase Cloud Messaging (FCM) — это служба, которая упрощает обмен сообщениями между мобильными приложениями и серверных приложений. Firebase Cloud Messaging можно использовать для реализации удаленных уведомлений (также называемых Push-уведомлениями) в приложениях Xamarin. Android.

[Уведомления](https://developer.android.com/guide/topics/ui/notifiers/notifications.html) &ndash; Этот раздел для разработчиков Android представляет собой полное справочное по для уведомлений Android. Он содержит раздел "рекомендации по проектированию", который помогает разрабатывать уведомления таким образом, чтобы они соответствовали правилам интерфейса пользователя Android. Он предоставляет дополнительные сведения о пресервиинг навигации при запуске действия, а также объясняет, как отображать ход выполнения в уведомлении и управлять воспроизведением мультимедиа на экране блокировки.

[Нотификатионлистенерсервице](xref:Android.Service.Notification.NotificationListenerService) &ndash; Эта служба Android позволяет приложению прослушивать все уведомления, размещенные на устройстве Android, а также взаимодействовать с ними, а не только с уведомлениями, зарегистрированными для получения приложения.
Обратите внимание, что пользователь должен явно предоставить приложению разрешение на прослушивание уведомлений на устройстве.

## <a name="related-links"></a>Связанные ссылки

- [Локальные уведомления (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/localnotifications)
- [Удаленные уведомления (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/remotenotifications)
