---
title: Microsoft Azure и Xamarin
description: Этот документ ссылается на документацию о подключенных службах в Визуальной студии для Mac, мобильных приложениях Azure, Active Directory Authentication и WebAPI.
ms.prod: xamarin
ms.assetid: 7b9aa8d9-c181-4c33-8ab0-2f56e4dbfc04
author: davidortinau
ms.author: daortin
ms.date: 10/09/2017
ms.openlocfilehash: 2b6dfeb0de0fac59556280609dbf870c23a9298b
ms.sourcegitcommit: 2a8eb8bce427e72d4e7edd06ce432e19f17dcdd7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2020
ms.locfileid: "80388529"
---
# <a name="microsoft-azure-and-xamarin"></a>Microsoft Azure и Xamarin

[![](images/evolve-mikej-azure-sml.png "Azure App Services features are easy to add to Xamarin apps, including cloud data storage and cross-platform push notifications")](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

[Эволюция 2016: Разработка подключенных приложений с использованием Azure и Xamarin](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

## <a name="connected-services-in-visual-studio-for-mac"></a>Подключенные службы в Visual Studio для Mac

Новая функция [Connected Services](connected-services.md) Visual Studio для Mac помогает разработчикам быстро и легко добавлять функциональность Azure в мобильные приложения из IDE. В настоящее время доступен для тестирования в альфа-канале.

## <a name="azure-app-services"></a>Службы приложений Azure

Существует коллекция [документации Azure Mobile Apps,](~/cross-platform/data-cloud/mobile-apps.md) которая поможет вам в процессе реализации [мобильного клиента Azure.](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
Xamarin также предлагает пакеты Azure Messaging NuGet для [iOS](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.iOS/) и [Android,](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.Android/) чтобы помочь реализовать push-уведомления на разных платформах.

Назначьте приложения на [портале служб приложений Azure](https://portal.azure.com/) для доступа к мобильным приложениям, Web-аПО, хранилищам и многое другое. Узнайте о [том, чем отличаются службы приложений,](https://azure.microsoft.com/updates/whats-new-with-azure-app-service/) и наблюдайте в [этих видео от корпорации Майкрософт.](https://azure.microsoft.com/campaigns/azure-march-announcement/)

## <a name="active-directory-authentication"></a>Каталог библиотеки проверки подлинности Active Directory ADAL

[Активный каталог Azure](~/cross-platform/data-cloud/active-directory/index.md) можно использовать для входа пользователей в приложения Xamarin. Приложения могут получить доступ к дополнительным службам, таким как Office 365.

## <a name="webapi"></a>WebAPI

Веб-aPI корпорации Майкрософт предоставляет REST-подобный интерфейс, который может быть легко использован приложениями Xamarin.
Вы можете легко подвести [сайт Azure](https://trywebsites.azurewebsites.net/) и создать приложение на основе WebAPI для подключения к приложениям Xamarin.

### <a name="introduction-to-web-services"></a>[Введение в веб-службы](~/cross-platform/data-cloud/web-services/index.md)

В этом уроке представлено, как интегрировать технологии веб-сервиса REST, WCF и SOAP с мобильными приложениями Xamarin. Он рассматривает различные реализации служб, оценивает имеющиеся инструменты и библиотеки для их интеграции и предоставляет шаблоны выборки для потребления данных служб. Наконец, он предоставляет базовый обзор создания веб-сервиса RESTful для потребления с помощью мобильного приложения Xamarin.

## <a name="samples"></a>Примеры

В дополнение к [образцам документации,](https://github.com/xamarin/mobile-samples/tree/master/Azure)следующие полные приложения демонстрируют различные функции Azure, включенные в приложения Xamarin:

- [Спорт](https://github.com/xamarin/Sport) - дружественное приложение для отслеживания спортивной лиги, которое использует хранение данных & push-уведомления.
- [Моменты](https://github.com/pierceboggan/Moments) — мгновенный обмен фотографиями, который использует хранилище Azure для изображений.
- [Xamarin CRM](https://github.com/xamarin/app-crm) — использует Web API для бэк-энда.
- [MyShoppe](https://github.com/jamesmontemagno/MyShoppe) - Мобильные приложения Azure.

- [eShop](https://github.com/dotnet-architecture/eShopOnContainers) - образец для [серии архитектуры](https://www.microsoft.com/net/learn/architecture) электронных книг.
- [MyDriving](https://azure.microsoft.com/campaigns/mydriving/) - Лазурный и IoT образец из Build 2016.

## <a name="related-links"></a>Связанные ссылки

- [Пример Azure PCL @paulbatum(по) (образец)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Портал Azure](https://azure.microsoft.com/)
- [Мобильный клиент для Xamarin (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
