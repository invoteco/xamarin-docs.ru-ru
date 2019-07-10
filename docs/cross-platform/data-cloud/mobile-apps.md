---
title: Мобильные приложения Microsoft Azure
description: Этот документ содержит ссылки на руководства, описывающие, как создать приложение Xamarin, который подключен к Azure. В нем описывается работа с Azure для компонента Xamarin, пользователей и Push-уведомлений.
ms.prod: xamarin
ms.assetid: 7B9AA8D9-C181-4C33-8AB0-2F56E4DBFC03
author: conceptdev
ms.author: crdun
ms.date: 04/02/2017
ms.openlocfilehash: a1a0b078659441f0f45af66728a5f37d578d6274
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67675089"
---
# <a name="microsoft-azure-mobile-apps"></a>Мобильные приложения Microsoft Azure

_Примеры и код загружает документация портала Azure._

<!--
NOTE TO AUTHORS: this page is referenced from
https://azure.microsoft.com/develop/mobile/xamarin/
as https://developer.xamarin.com/guides/cross-platform/data-cloud/mobile-services/
A redirect has been put in place to /mobile-apps/ HOWEVER the /Resources/ .ZIP files are still located in /mobile-services/ so that the following permalinks don't break

The ZIPs in /Resources/ are also referenced by inbound links
Getting Started  http://go.microsoft.com/fwlink/p/?LinkId=331359
Get started with data   http://go.microsoft.com/fwlink/p/?LinkId=331302
Get started with push   http://go.microsoft.com/fwlink/p/?LinkId=331303
Get started with authentication http://go.microsoft.com/fwlink/p/?LinkId=331328
Get started with Notification Hubs  http://go.microsoft.com/fwlink/p/?LinkId=331329
Validate and modify data    http://go.microsoft.com/fwlink/p/?LinkId=331330
-->


Эти ссылки служат для документации по Xamarin на [мобильных приложений Azure](https://docs.microsoft.com/azure/app-service-mobile/) веб-сайта.
Добавление функций Azure в приложение Xamarin, загрузив [Azure Mobile Client](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).

## <a name="working-with-the-xamarin-azure-component"></a>Работа с помощью компонента Xamarin Azure

Общая документация [работа с клиентская библиотека Xamarin (компонент)](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library) для выполнения различных задач с помощью мобильных приложений Azure. Эта страница содержит много фрагменты кода, без подробные объяснения и примеры доступны в пошаговом руководстве статьях, перечисленных ниже.

## <a name="getting-started"></a>Начало работы

В этой статье приведена пошаговая инструкция для получения первого приложения Xamarin Azure работу программ.
Он охватывает Создание новой мобильной службы Azure на портале и затем загрузки и выполнения предварительно настроенных приложений.

-  [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started/)
-  [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started/)
-  [Xamarin.Forms](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started)

<!--
## Validate, Modify and Augment Data in Scripts

Demonstrates how to add server-side scripts to Azure Mobile Services data tables to implement server-side validation and other functionality.

-  [iOS](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-  [Android](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-->

<!--
## Add Paging to Data

A quick example of paging large sets of data using Skip() and Take().

-  [iOS](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-  [Android](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-->

## <a name="get-started-with-users"></a>Начало работы с пользователями

Полные инструкции по настройке и программировании экран входа с помощью мобильных служб Azure. Поддерживаемых поставщиков проверки подлинности включают Microsoft, Google, Facebook и Twitter.

-  [iOS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-ios-get-started-users/)
-  [Android](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-android-get-started-users/)


## <a name="authorize-users-in-scripts"></a>Авторизация пользователей в скриптах

Пример кода для внутренних серверов Javascript

-  [TODO.js](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/personal-table/tables/TodoItem.js#L38)


## <a name="get-started-with-push"></a>Начало работы с Push-уведомлений

Выполните инструкции по настройке Push-уведомлений Apple и Google веб-узлов, а затем отправить Push-уведомления из мобильных служб Azure на устройство.

-  [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started-push)
-  [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started-push)


## <a name="get-started-with-notification-hubs"></a>Начало работы с центрами уведомлений

Полные инструкции Настройка Push-уведомлений Apple и Google веб-узлов, настроить центр уведомлений Azure и затем создать Push-уведомлений на устройства.

-  [iOS](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started)
-  [Android](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm)



## <a name="related-links"></a>Связанные ссылки

- [GettingStarted (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GettingStarted)
- [GetStartedWithData (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithData)
- [GetStartedWithUsers (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithUsers)
- [GetStartedWithPush (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithPush)
- [NotificationHubs (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs)
- [Azure мобильный клиент](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [Схема обучения Azure мобильных приложений](https://azure.microsoft.com/documentation/learning-paths/appservice-mobileapps/)

<!--
- [ValidateModifyData (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/ValidateModifyData)
-->
