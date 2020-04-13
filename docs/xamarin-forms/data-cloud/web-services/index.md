---
title: Xamarin.Forms и веб-службы
description: В этом руководстве объясняется, как общаться с различными веб-службами для создания, чтения, обновления и удаления (CRUD) функциональности приложения Xamarin.Forms. Темы включают общение с службами ASMX, услугами WCF, службами REST.
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
ms.openlocfilehash: 799a0a97f7c1a212b10dc62f8b3e7bd2cf2060c8
ms.sourcegitcommit: bf3b5925018e1bd6a00505e9f37500761b66809d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2020
ms.locfileid: "80395460"
---
# <a name="xamarinforms-and-web-services"></a>Xamarin.Forms и веб-службы

## <a name="introduction"></a>[Введение](introduction.md)

В этой статье приводится пошаговка примера приложения Xamarin.Forms, в рамках которого показано, как общаться с различными веб-службами. Рассматриваемые темы включают анатомию приложения, страницы, модель данных и использование операций веб-службы.

## <a name="consume-an-aspnet-web-service-asmx"></a>[Использование веб-службы ASP.NET (ASMX)](~/xamarin-forms/data-cloud/web-services/asmx.md)

ASP.NET Web Services (ASMX) предоставляют возможность создавать веб-сервисы, которые отправляют сообщения через HTTP с помощью простого протокола доступа к объекту (SOAP). SOAP является независимым от платформы и языковым протоколом для создания и доступа к веб-сервисам. Потребителям службы ASMX не нужно ничего знать о платформе, объектной модели или языке программирования, используемом для реализации службы. Им нужно только понять, как отправлять и получать сообщения SOAP. В этой статье показано, как использовать веб-сервис ASMX из приложения Xamarin.Forms.

## <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>[Потребляйте веб-службу Фонда связи Windows (WCF)](~/xamarin-forms/data-cloud/web-services/wcf.md)

WCF — это единая платформа корпорации Майкрософт для создания приложений, ориентированных на обслуживание. Это позволяет разработчикам создавать безопасные, надежные, совместимые и совместимые распределенные приложения. Существуют различия между ASP.NET Web Services (ASMX) и WCF, но важно понимать, что WCF поддерживает те же возможности, которые предоставляет ASMX - SOAP сообщения через HTTP. В этой статье показано, как использовать wCF SOAP службы из приложения Xamarin.Forms.

## <a name="consume-a-restful-web-service"></a>[Потребляйте веб-службу RESTful](~/xamarin-forms/data-cloud/web-services/rest.md)

Представительский государственный перевод (REST) — архитектурный стиль для создания веб-сервисов. ЗАПРОСы REST делаются по HTTP с использованием тех же глаголов HTTP, которые используют веб-браузеры для извлечения веб-страниц и отправки данных на серверы. В этой статье показано, как использовать веб-сервис RESTful из приложения Xamarin.Forms.
