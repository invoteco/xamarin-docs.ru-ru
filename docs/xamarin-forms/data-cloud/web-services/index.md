---
title: Xamarin.Forms и веб-служб
description: В этом руководстве объясняется, как взаимодействуют различные веб-службы для обеспечения создания, чтения, обновления и удаления (CRUD) для приложения Xamarin.Forms. Рассматриваются связи с службами ASMX, WCF services, службы REST.
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
ms.openlocfilehash: 1cf2714191528c5619b4f877bcb43e80464c44d1
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2019
ms.locfileid: "67659011"
---
# <a name="xamarinforms-and-web-services"></a>Xamarin.Forms и веб-служб

## <a name="introductionintroductionmd"></a>[Введение](introduction.md)

Эта статья содержит пошаговое руководство по Xamarin.Forms пример приложения, демонстрирующий способы взаимодействия с различных веб-служб. Рассматриваются Анатомия приложения, страницы, модели данных и вызова операций веб-службы.

## <a name="consume-an-aspnet-web-service-asmxxamarin-formsdata-cloudweb-servicesasmxmd"></a>[Использование службы веб-ASP.NET (ASMX)](~/xamarin-forms/data-cloud/web-services/asmx.md)

Веб-службы ASP.NET (ASMX) предоставляют возможность создавать веб-службы, отправляющие сообщения по протоколу HTTP с помощью Simple Object Access Protocol (SOAP). SOAP — это протокол независимо от платформы и независимый от языка, для создания и вызова веб-служб. Потребителям службы ASMX не нужно ничего знать об используемой платформе, объектной модели или язык программирования, используемый для реализации службы. Они должны понять, как отправлять и получать сообщения SOAP. В этой статье показано, как использовать веб-службу ASMX, в приложении Xamarin.Forms.

## <a name="consume-a-windows-communication-foundation-wcf-web-servicexamarin-formsdata-cloudweb-serviceswcfmd"></a>[Использование веб-службы Windows Communication Foundation (WCF)](~/xamarin-forms/data-cloud/web-services/wcf.md)

WCF — это унифицированная платформа корпорации Microsoft для построения сервисноориентированных приложений. Он позволяет разработчикам создавать безопасные, надежные, с поддержкой транзакций с возможностью взаимодействия распределенных приложений. Существуют различия между ASP.NET Web Services (ASMX) и WCF, но это важно понимать, что WCF поддерживает те же возможности, предоставляемые ASMX — сообщений SOAP по протоколу HTTP. В этой статье показано, как использовать службу WCF SOAP из приложения Xamarin.Forms.

## <a name="consume-a-restful-web-servicexamarin-formsdata-cloudweb-servicesrestmd"></a>[Использование веб-службу RESTful](~/xamarin-forms/data-cloud/web-services/rest.md)

Representational State Transfer (REST) представляет собой архитектурный стиль для построения веб-служб. REST запросы выполняются по протоколу HTTP, используя те же глаголы HTTP, использующих веб-браузеры для получения веб-страниц и для отправки данных на серверы. В этой статье показано, как использовать веб-службу RESTful в приложении Xamarin.Forms.
