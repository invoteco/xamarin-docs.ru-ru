---
title: Microsoft Azure и Xamarin
description: В этом документе содержатся ссылки на документацию по Подключенные службы в Visual Studio для Mac, мобильных приложениях Azure, Active Directory проверки подлинности и WebAPI.
ms.prod: xamarin
ms.assetid: 7b9aa8d9-c181-4c33-8ab0-2f56e4dbfc04
author: conceptdev
ms.author: crdun
ms.date: 10/09/2017
ms.openlocfilehash: 11a98b61d958e2e0d0fb226ba4eb05c458b5bbf2
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70766101"
---
# <a name="microsoft-azure-and-xamarin"></a>Microsoft Azure и Xamarin

[![](images/evolve-mikej-azure-sml.png "Функции служб приложений Azure легко добавлять в приложения Xamarin, включая облачное хранилище данных и Push-уведомления между платформами.")](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

[Усовершенствовано в 2016 г. Разработка подключенных приложений с помощью Azure и Xamarin](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

## <a name="connected-services-in-visual-studio-for-mac"></a>Подключенные службы в Visual Studio для Mac

Новая функция [Подключенные службы](connected-services.md) Visual Studio для Mac помогает разработчикам быстро и легко добавлять функции Azure в мобильные приложения из интегрированной среды разработки. В настоящее время доступно для тестирования в альфа-канале.

## <a name="azure-app-services"></a>Службы приложений Azure

Существует коллекция [документации по мобильным приложениям Azure](~/cross-platform/data-cloud/mobile-apps.md) , в которой описывается процесс реализации [мобильного клиента Azure](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).
Xamarin также предлагает пакеты NuGet для обмена сообщениями Azure для [iOS](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.iOS/) и [Android](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.Android/) , которые помогают реализовать push-уведомления на разных платформах.

Настройте приложения на [портале служб приложений Azure](https://portal.azure.com/) , чтобы получить доступ к мобильным приложениям, веб-API, хранилищу и т. д. Узнайте, [как отличаются службы приложений](https://azure.microsoft.com/updates/whats-new-with-azure-app-service/) и смотрите их в [этих видеороликах корпорации Майкрософт](https://azure.microsoft.com/campaigns/azure-march-announcement/).

## <a name="active-directory-authentication"></a>Проверка подлинности Active Directory

[Azure Active Directory](~/cross-platform/data-cloud/active-directory/index.md) можно использовать для входа пользователей в приложения Xamarin с помощью [компонента Xamarin. auth](https://www.nuget.org/packages/Xamarin.Auth/).
Затем приложения могут получить доступ к дополнительным службам, таким как Office 365.

## <a name="webapi"></a>WebAPI

Веб-API Майкрософт предоставляет интерфейс, подобный ОСТАЛЬному, который может легко использоваться приложениями Xamarin.
Вы можете легко развернуть [веб-сайт Azure](https://trywebsites.azurewebsites.net/) и создать приложение на основе WebAPI для подключения к приложениям Xamarin.

### <a name="introduction-to-web-servicescross-platformdata-cloudweb-servicesindexmd"></a>[Общие сведения о веб-службах](~/cross-platform/data-cloud/web-services/index.md)

В этом учебнике рассказывается, как интегрировать технологии веб-служб RESTFUL, WCF и SOAP с мобильными приложениями Xamarin. Он изучает различные реализации служб, оценивает доступные инструменты и библиотеки для их интеграции и предоставляет образцы шаблонов для использования данных службы. Наконец, он предоставляет базовый обзор создания веб-службы RESTFUL для использования с мобильным приложением Xamarin.

## <a name="samples"></a>Примеры

Помимо [примеров документации](https://github.com/xamarin/mobile-samples/tree/master/Azure), в следующих готовых приложениях демонстрируются различные функции Azure, включенные в приложения Xamarin:

- [Спорт](https://github.com/xamarin/Sport) — удобное приложение для отличаются отслеживания, которое использует хранилище данных & push-уведомления.
- [Время](https://github.com/pierceboggan/Moments) — Мгновенный общий доступ к фотографиям, использующий службу хранилища Azure для образов.
- [XAMARIN CRM](https://github.com/xamarin/app-crm) — использует веб-API для серверной части.
- [Мишоппе](https://github.com/jamesmontemagno/MyShoppe) — мобильные приложения Azure.

- [ешоп](https://github.com/dotnet-architecture/eShopOnContainers) — образец для серии книг по [архитектуре](https://www.microsoft.com/net/learn/architecture) .
- [MyDriving](https://azure.microsoft.com/campaigns/mydriving/) — пример Azure + IOT из сборки 2016.

## <a name="related-links"></a>Связанные ссылки

- [Пример Azure PCL (by @paulbatum) (пример)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [портал Azure](https://azure.microsoft.com/)
- [Мобильный клиент для Xamarin (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
