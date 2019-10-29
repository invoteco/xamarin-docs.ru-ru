---
title: Обмен сообщениями Google
description: В этом разделе содержатся руководства, в которых описывается реализация приложений Xamarin. Android с помощью служб обмена сообщениями Google.
ms.prod: xamarin
ms.assetid: 85E8DF92-D160-4763-A7D3-458B4C31635F
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/12/2018
ms.openlocfilehash: e0d257cd5d443c77fd0a8132d9cd31ec572182aa
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73023533"
---
# <a name="google-messaging"></a>Обмен сообщениями Google

_В этом разделе содержатся руководства, в которых описывается реализация приложений Xamarin. Android с помощью служб обмена сообщениями Google._

## <a name="firebase-cloud-messagingfirebase-cloud-messagingmd"></a>[Обмен сообщениями Firebase Cloud](firebase-cloud-messaging.md)

Firebase Cloud Messaging (FCM) — это служба, которая упрощает обмен сообщениями между мобильными приложениями и серверных приложений. FCM является последователем Google Google Cloud Messaging. В этой статье приводятся общие сведения о том, как работает FCM, и приводится пошаговая процедура получения учетных данных, чтобы приложение может использовать службы FCM.

## <a name="remote-notifications-with-firebase-cloud-messagingremote-notifications-with-fcmmd"></a>[Удаленные уведомления с помощью Firebase Cloud Messaging](remote-notifications-with-fcm.md)

В этом пошаговом руководстве представлено пошаговое описание использования Firebase Cloud Messaging для реализации удаленных уведомлений (также называемых Push-уведомлениями) в приложении Xamarin. Android. В нем показано, как реализовать различные классы, необходимые для связи с Firebase Cloud Messaging (FCM), а также приводятся примеры настройки манифеста Android для доступа к FCM и демонстрации подчиненного обмена сообщениями с помощью Firebase Консол.

## <a name="google-cloud-messaginggoogle-cloud-messagingmd"></a>[Google Cloud Messaging](google-cloud-messaging.md)

В этом разделе представлен общий обзор того, как Google Cloud Messaging (GCM) маршрутизирует сообщения между приложением и сервером приложений и предоставляет пошаговую процедуру для получения учетных данных, чтобы приложение может использовать службы GCM. (Обратите внимание, что GCM был заменен FCMом.)

> [!NOTE]
> GCM был заменен [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) (FCM).
> API-интерфейсы сервера и клиента GCM [устарели](https://firebase.googleblog.com/2018/04/time-to-upgrade-from-gcm-to-fcm.html) и больше не будут доступны сразу после 11 апреля 2019.

## <a name="remote-notifications-with-google-cloud-messagingremote-notifications-with-gcmmd"></a>[Удаленные уведомления с Google Cloud Messaging](remote-notifications-with-gcm.md)

В этом разделе содержится пошаговое описание способов реализации удаленных уведомлений в Xamarin. Android с помощью Google Cloud Messaging.
В нем объясняются различные компоненты, которые необходимо использовать для включения Google Cloud Messaging в приложении Android.
