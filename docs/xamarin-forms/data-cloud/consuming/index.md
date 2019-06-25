---
title: Использование веб-служб
description: В этом руководстве показано, как взаимодействуют различные веб-службы для обеспечения создания, чтения, обновления и удаления (CRUD) для приложения Xamarin.Forms. Рассматриваются связи с службами ASMX, WCF services, службы REST и мобильных приложений Azure.
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 163b82590e95a1587836502883cd5f9f2afbe19c
ms.sourcegitcommit: 6e04246207aa743820029e8c217a43cfdd24f991
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/25/2019
ms.locfileid: "67352112"
---
# <a name="consuming-web-services"></a>Использование веб-служб

_В этом руководстве показано, как взаимодействуют различные веб-службы для обеспечения создания, чтения, обновления и удаления (CRUD) для приложения Xamarin.Forms. Рассматриваются связи с службами ASMX, WCF services, службы REST и мобильных приложений Azure._

## <a name="consuming-an-aspnet-web-service-asmxxamarin-formsdata-cloudconsumingasmxmd"></a>[Использование службы веб-ASP.NET (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md)

Веб-службы ASP.NET (ASMX) предоставляют возможность создавать веб-службы, отправляющие сообщения по протоколу HTTP с помощью Simple Object Access Protocol (SOAP). SOAP — это протокол независимо от платформы и независимый от языка, для создания и вызова веб-служб. Потребителям службы ASMX не нужно ничего знать об используемой платформе, объектной модели или язык программирования, используемый для реализации службы. Они должны понять, как отправлять и получать сообщения SOAP. В этой статье показано, как использовать веб-службу ASMX, в приложении Xamarin.Forms.

## <a name="consuming-a-windows-communication-foundation-wcf-web-servicexamarin-formsdata-cloudconsumingwcfmd"></a>[Использование веб-службы Windows Communication Foundation (WCF)](~/xamarin-forms/data-cloud/consuming/wcf.md)

WCF — это унифицированная платформа корпорации Microsoft для построения сервисноориентированных приложений. Он позволяет разработчикам создавать безопасные, надежные, с поддержкой транзакций с возможностью взаимодействия распределенных приложений. Существуют различия между ASP.NET Web Services (ASMX) и WCF, но это важно понимать, что WCF поддерживает те же возможности, предоставляемые ASMX — сообщений SOAP по протоколу HTTP. В этой статье показано, как использовать службу WCF SOAP из приложения Xamarin.Forms.

## <a name="consuming-a-restful-web-servicexamarin-formsdata-cloudconsumingrestmd"></a>[Использование веб-службу RESTful](~/xamarin-forms/data-cloud/consuming/rest.md)

Representational State Transfer (REST) представляет собой архитектурный стиль для построения веб-служб. REST запросы выполняются по протоколу HTTP, используя те же глаголы HTTP, использующих веб-браузеры для получения веб-страниц и для отправки данных на серверы. В этой статье показано, как использовать веб-службу RESTful в приложении Xamarin.Forms.

## <a name="consuming-an-azure-mobile-appxamarin-formsdata-cloudconsumingazuremd"></a>[Использование мобильного приложения Azure](~/xamarin-forms/data-cloud/consuming/azure.md)

Мобильные приложения Azure дают возможность разрабатывать приложения с помощью масштабируемых серверных систем, размещенных в службе приложений Azure, поддерживая мобильная проверка подлинности, автономной синхронизации и Push-уведомлений. В этой статье, которая применяется только для мобильных приложений Azure, использовать серверной части Node.js, объясняется, как для запросов, вставки, обновления и удаления данных, хранящихся в таблице экземпляра мобильных приложений Azure.

## <a name="related-links"></a>Связанные ссылки

- [Введение в веб-службы](~/cross-platform/data-cloud/web-services/index.md)
- [Общие сведения о поддержке асинхронного выполнения](~/cross-platform/platform/async.md)
