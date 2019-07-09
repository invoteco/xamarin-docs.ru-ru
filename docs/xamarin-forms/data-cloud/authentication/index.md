---
title: Аутентификация Xamarin.Forms веб-службы
description: В этом руководстве объясняется, как интегрировать службы проверки подлинности в приложении Xamarin.Forms, чтобы пользователи могли совместно использовать серверную часть только Имея доступ к собственным данным.
ms.prod: xamarin
ms.assetid: E6FCFAE1-4F83-4F93-9190-EC5290360C54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
ms.openlocfilehash: a36dfba7aa07de1633ca9620674ddb4887902ba9
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2019
ms.locfileid: "67650428"
---
# <a name="xamarinforms-web-service-authentication"></a>Аутентификация Xamarin.Forms веб-службы

## <a name="authenticate-a-restful-web-servicerestmd"></a>[Проверка подлинности веб-службу RESTful](rest.md)

Протокол HTTP поддерживает использование следующих механизмов проверки подлинности для управления доступом к ресурсам. Обычная проверка подлинности обеспечивает доступ к ресурсам только клиентам, которые имеют правильные учетные данные. В этой статье объясняется, как использовать обычную проверку подлинности для защиты доступа к ресурсам службы веб-RESTful.

## <a name="authenticate-users-with-an-identity-provideroauthmd"></a>[Проверка подлинности пользователей с помощью поставщика удостоверений](oauth.md)

Xamarin.Auth — это кроссплатформенный пакет SDK для проверки подлинности пользователей и хранения учетной записью. Он включает в себя структур проверки подлинности OAuth, которые предоставляют поддержку использования поставщиков удостоверений, например Google, Майкрософт, Facebook и Twitter. В этой статье описываются способы использования Xamarin.Auth для управления процессом проверки подлинности в приложении Xamarin.Forms.

## <a name="authenticate-users-with-azure-active-directory-b2cazure-ad-b2cmd"></a>[Проверка подлинности пользователей с Azure Active Directory B2C](azure-ad-b2c.md)

Azure Active Directory B2C является это облаке решение управления удостоверениями для мобильных приложений и веб-потребитель. В этой статье объясняется, как использовать библиотеки аутентификации Майкрософт (MSAL) и Azure Active Directory B2C для интеграции управления удостоверениями в приложения Xamarin.Forms.

## <a name="authenticate-users-with-an-azure-cosmos-db-document-database-and-xamarinformsazure-cosmosdb-authmd"></a>[Проверка подлинности пользователей с помощью базы данных документов Azure Cosmos DB и Xamarin.Forms](azure-cosmosdb-auth.md)

Базы данных документов Azure Cosmos DB поддерживает секционированные коллекции, которые могут охватывать несколько серверов и секций, поддерживая неограниченный объем хранилища и пропускной способности. В этой статье объясняется, как объединить управление доступом с секционированным коллекциям, таким образом, чтобы пользователь имеет доступ только к своих собственных документов в приложении Xamarin.Forms.
