---
title: Microsoft Azure и Xamarin
description: Этот документ содержит ссылки на документацию о подключенных служб в Visual Studio для Mac, мобильных приложений Azure, проверка подлинности Active Directory и веб-API.
ms.prod: xamarin
ms.assetid: 7b9aa8d9-c181-4c33-8ab0-2f56e4dbfc04
author: asb3993
ms.author: amburns
ms.date: 10/09/2017
ms.openlocfilehash: 25eec247a0cb664897541a0e6e818a77018fda43
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61187783"
---
# <a name="microsoft-azure-and-xamarin"></a>Microsoft Azure и Xamarin

[ ![](images/evolve-mikej-azure-sml.png "Функции служб приложений Azure можно легко добавлять в приложения Xamarin, включая Облачное хранилище данных и кросс платформенные Push-уведомлений")](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

[Усовершенствовано в 2016 г. Разработка подключенных приложений с помощью Azure и Xamarin](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

## <a name="connected-services-in-visual-studio-for-mac"></a>Подключенные службы в Visual Studio для Mac

Новый [подключенных служб](connected-services.md) компонент Visual Studio для Mac позволяет разработчикам быстро и легко добавлять функциональные возможности Azure для мобильных приложений из интегрированной среды разработки. В настоящее время доступны для тестирования в альфа-канал.

## <a name="azure-app-services"></a>Службы приложений Azure

Имеется коллекция [документация по мобильным приложениям Azure](~/cross-platform/data-cloud/mobile-apps.md) , в котором описаны процесс реализации [Azure Mobile Client](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).
Xamarin также предлагает пакеты NuGet обмена сообщениями Azure для [iOS](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.iOS/) и [Android](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.Android/) для реализации Push-уведомлений в разных платформах.

Настройте свои приложения на [портала службы приложений](https://portal.azure.com/) для доступа к мобильным приложениям, веб-API, хранения и многое другое. Дополнительные сведения о [как служб приложений отличаются](http://azure.microsoft.com/updates/whats-new-with-azure-app-service/) и смотреть в [эти видеоматериалы от корпорации Майкрософт](http://azure.microsoft.com/campaigns/azure-march-announcement/).

## <a name="active-directory-authentication"></a>Проверка подлинности Active Directory

[Azure Active Directory](~/cross-platform/data-cloud/active-directory/index.md) можно использовать для входа пользователей в приложениях Xamarin через [Xamarin.Auth компонент](https://www.nuget.org/packages/Xamarin.Auth/).
Приложения можно затем получить доступ к дополнительным службам, таким как Office 365.

## <a name="webapi"></a>Веб-API

Веб-API корпорации Майкрософт предоставляет интерфейс REST по принципу, который можно легко использовать в приложениях Xamarin.
Вы можете легко регулировать [веб-сайта Azure](https://trywebsites.azurewebsites.net/) и создайте приложение на основе веб-API для подключения к приложениям Xamarin.


###  <a name="introduction-to-web-servicescross-platformdata-cloudweb-servicesindexmd"></a>[Введение в веб-служб](~/cross-platform/data-cloud/web-services/index.md)

В этом учебнике рассказывается, как интегрировать REST, WCF и SOAP веб-технологиями служб с помощью мобильных приложений Xamarin. Он проверяет различные реализации службы, оценивает доступные инструменты и библиотеки, чтобы интегрировать их и предоставляет образец шаблона для использования службы данных. Наконец он предоставляет общий обзор Создание RESTful веб-службы для использования с помощью мобильного приложения Xamarin.

## <a name="samples"></a>Примеры

В дополнение к [образцы из документации по](https://github.com/xamarin/mobile-samples/tree/master/Azure), следующих полный приложений демонстрируют различные функции Azure, включить в приложения Xamarin:

- [Спорт](https://github.com/xamarin/Sport) — приложение для отслеживания понятное sports лиги, использующий данные хранилища и принудительная отправка уведомлений.
- [Секунд](https://github.com/pierceboggan/Moments) — мгновенные, для обмена фотографиями использует службу хранилища Azure для образов.
- [Xamarin CRM](https://github.com/xamarin/app-crm) — использует веб-API для серверной части.
- [MyShoppe](https://github.com/jamesmontemagno/MyShoppe) -мобильные приложения Azure.

- [eShop](https://github.com/dotnet-architecture/eShopOnContainers) — образец [серии архитектура](https://www.microsoft.com/net/learn/architecture) электронных книг.
- [MyDriving](https://azure.microsoft.com/campaigns/mydriving/) — Azure "и" Интернет вещей выборки из Build 2016.


## <a name="related-links"></a>Связанные ссылки

- [Пример Azure переносимой библиотеки Классов (с @paulbatum) (пример)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Портал Azure](http://azure.microsoft.com/)
- [Мобильный клиент для Xamarin (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
