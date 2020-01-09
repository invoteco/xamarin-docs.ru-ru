---
title: Уведомления в Xamarin. Android
ms.prod: xamarin
ms.assetid: 2E54F1D0-45F4-43A7-B3A3-4F483B7150CB
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: e0035414cb8a24e5571b2972ec638eec550c593c
ms.sourcegitcommit: 191f1f3b13a14e2afadcb95126c5f653722f126f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/30/2019
ms.locfileid: "75545560"
---
# <a name="notifications-in-xamarinandroid"></a>Уведомления в Xamarin. Android

В этом разделе объясняется, как реализовать уведомления в Xamarin. Android. В нем описываются различные элементы пользовательского интерфейса уведомления Android и обсуждаются API, связанные с созданием и отображением уведомлений.

## <a name="local-notifications-in-androidlocal-notificationsmd"></a>[Локальные уведомления в Android](local-notifications.md)

В этом разделе объясняется, как реализовать локальные уведомления в Xamarin. Android. Здесь описываются различные элементы пользовательского интерфейса уведомления Android и обсуждаются интерфейсы API, связанные с созданием и отображением уведомлений.

## <a name="walkthrough---using-local-notifications-in-xamarinandroidlocal-notifications-walkthroughmd"></a>[Пошаговое руководство. Использование локальных уведомлений в Xamarin. Android](local-notifications-walkthrough.md)  

В этом пошаговом руководстве описывается, как использовать локальные уведомления в приложении Xamarin. Android. В нем демонстрируются основы создания и публикации уведомления. Когда пользователь щелкает уведомление в ящике уведомлений, он начинает второе действие. 

## <a name="further-reading"></a>Дополнительные сведения

[Облачная система обмена сообщениями](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) &ndash; Firebase Cloud MESSAGING (FCM) — это служба, которая упрощает обмен сообщениями между мобильными приложениями и серверами приложений. Firebase Cloud Messaging можно использовать для реализации удаленных уведомлений (также называемых Push-уведомлениями) в приложениях Xamarin. Android.

[Уведомления](https://developer.android.com/guide/topics/ui/notifiers/notifications.html) &ndash; этом разделе разработчика Android — это полное справочное по для уведомлений Android. Он содержит раздел "рекомендации по проектированию", который помогает разрабатывать уведомления таким образом, чтобы они соответствовали правилам интерфейса пользователя Android. Он содержит более фундаментальные сведения о сохранении навигации при запуске действия, а также объясняет, как отображать ход выполнения в уведомлении и управлять воспроизведением мультимедиа на экране блокировки.

[Нотификатионлистенерсервице](xref:Android.Service.Notification.NotificationListenerService) &ndash; этой службе Android позволяет вашему приложению прослушивать и взаимодействовать с) все уведомления, размещенные на устройстве Android, а не только уведомления, зарегистрированные в приложении для получения.
Обратите внимание, что пользователь должен явно предоставить приложению разрешение на прослушивание уведомлений на устройстве.

## <a name="related-links"></a>Связанные ссылки

- [Локальные уведомления (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/localnotifications)
