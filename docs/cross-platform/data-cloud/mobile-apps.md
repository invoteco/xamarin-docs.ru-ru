---
title: Microsoft Azure мобильные приложения
description: В этом документе содержатся ссылки на руководства, в которых описывается создание приложения Xamarin, подключенного к Azure. В нем обсуждается работа с компонентом Xamarin Azure, пользователями и Push-уведомлениями.
ms.prod: xamarin
ms.assetid: 7B9AA8D9-C181-4C33-8AB0-2F56E4DBFC03
author: davidortinau
ms.author: daortin
ms.date: 04/02/2017
ms.openlocfilehash: 84517e4961dc3ad728b6cc352e9fb992d9e8b5bf
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016591"
---
# <a name="microsoft-azure-mobile-apps"></a>Microsoft Azure мобильные приложения

> [!NOTE]
> Центр приложений Visual Studio поддерживает комплексные и интегрированные службы для разработки мобильных приложений. Разработчики могут использовать службы **сборки**, **тестирования** и **распространения** для настройки конвейера непрерывной интеграции и доставки. После развертывания приложения разработчики могут отслеживать состояние и использование своих приложений с помощью служб **аналитики** и **диагностики** , а также привлекать пользователей с помощью службы **push-уведомлений** . Разработчики также **могут использовать проверку подлинности** для аутентификации пользователей и службы **данных** , чтобы сохранять и синхронизировать данные приложений в облаке.
> Если вы хотите интегрировать облачные службы в мобильное приложение, зарегистрируйтесь в [центре приложений](https://appcenter.ms/signup?utm_source=XamarinDocs&utm_medium=Azure&utm_campaign=docs) уже сегодня.

_Примеры и загрузка кода для портал Azureной документации._

<!--
NOTE TO AUTHORS: this page is referenced from
https://azure.microsoft.com/develop/mobile/xamarin/
as https://developer xamarin com/guides/cross-platform/data-cloud/mobile-services/
A redirect has been put in place to /mobile-apps/ HOWEVER the /Resources/ .ZIP files are still located in /mobile-services/ so that the following permalinks don't break

The ZIPs in /Resources/ are also referenced by inbound links
Getting Started https://go.microsoft.com/fwlink/p/?LinkId=331359
Get started with data https://go.microsoft.com/fwlink/p/?LinkId=331302
Get started with push https://go.microsoft.com/fwlink/p/?LinkId=331303
Get started with authentication https://go.microsoft.com/fwlink/p/?LinkId=331328
Get started with Notification Hubs https://go.microsoft.com/fwlink/p/?LinkId=331329
Validate and modify data  https://go.microsoft.com/fwlink/p/?LinkId=331330
-->

Эти ссылки предназначены для документации по Xamarin, доступной на веб-сайте [мобильных приложений Azure](https://docs.microsoft.com/azure/app-service-mobile/) .
Добавление функций Azure в приложение Xamarin путем скачивания [мобильного клиента Azure](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).

## <a name="working-with-the-xamarin-azure-component"></a>Работа с компонентом Xamarin Azure

Общая документация по [работе с клиентской библиотекой Xamarin (компонентом)](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library) для выполнения различных задач с помощью мобильных приложений Azure. На этой странице содержится множество примеров фрагментов кода без подробного объяснения и примеров, доступных в каждой из пошаговых руководств, перечисленных ниже.

## <a name="getting-started"></a>Начало работы

В этой статье содержатся пошаговые инструкции по созданию и запуску первого приложения Xamarin Azure.
Здесь рассматривается создание нового мобильного приложения Azure на портале, а затем Загрузка и запуск предварительно настроенного приложения.

- [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started/)
- [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started/)
- [Xamarin.Forms](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started)

<!--
## Validate, Modify and Augment Data in Scripts

Demonstrates how to add server-side scripts to Azure Mobile Services data tables to implement server-side validation and other functionality.

- [iOS](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
- [Android](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-->

<!--
## Add Paging to Data

A quick example of paging large sets of data using Skip() and Take().

- [iOS](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
- [Android](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-->

## <a name="get-started-with-users"></a>Начало работы с пользователями

Содержит полные инструкции по настройке и кодированию экрана входа с помощью мобильных служб Azure. Поддерживаемые поставщики проверки подлинности включают в себя Microsoft, Google, Facebook и Twitter.

- [iOS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-ios-get-started-users/)
- [Android](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-android-get-started-users/)

## <a name="authorize-users-in-scripts"></a>Авторизация пользователей в скриптах

Пример кода для интерфейсов JavaScript

- [TODO. js](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/personal-table/tables/TodoItem.js#L38)

## <a name="get-started-with-push"></a>Начало работы с Push-уведомлениями

Выполните инструкции по настройке push-уведомлений на сайтах Apple и Google, а затем отправьте push-уведомление из мобильных служб Azure на устройство.

- [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started-push)
- [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started-push)

## <a name="get-started-with-notification-hubs"></a>Приступая к работе с центрами уведомлений

Выполните инструкции по настройке push-уведомлений на сайтах Apple и Google, настройте центр уведомлений Azure, а затем создайте push-уведомления на устройствах.

- [iOS](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started)
- [Android](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm)

## <a name="related-links"></a>Связанные ссылки

- [GettingStarted (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GettingStarted)
- [GetStartedWithData (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithData)
- [Жетстартедвисусерс (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithUsers)
- [Жетстартедвиспуш (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithPush)
- [NotificationHubs (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs)
- [Мобильный клиент Azure](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [Схема обучения мобильных приложений Azure](https://azure.microsoft.com/documentation/learning-paths/appservice-mobileapps/)

<!--
- [ValidateModifyData (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/ValidateModifyData)
-->
