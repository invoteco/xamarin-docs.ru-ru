---
title: Данные и облачные службы в приложениях Xamarin. iOS
description: В этом документе содержатся ссылки на руководства, в которых описывается работа с локальными данными, iCloud и CloudKit в приложении Xamarin. iOS.
ms.prod: xamarin
ms.assetid: 945719F7-7CE6-4207-BF0F-23195125FC84
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/13/2017
ms.openlocfilehash: e9f910804f3da9d173206d51c59e1d7ddd9b90a6
ms.sourcegitcommit: bad1ab3f78d7f94d48511666626b54f8ba155689
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/04/2020
ms.locfileid: "75663516"
---
# <a name="data-and-cloud-services-in-xamarinios-apps"></a>Данные и облачные службы в приложениях Xamarin. iOS

## <a name="data-accessiosdata-clouddataindexmd"></a>[Доступ к данным](~/ios/data-cloud/data/index.md)

Большинству приложений предъявляются некоторые требования для сохранения данных на устройстве локально. Если объем данных не является тривиальным, для управления доступом к базе данных обычно требуется база данных и уровень данных в приложении. в iOS имеется встроенное ядро СУБД SQLite, а доступ к данным упрощен платформой Xamarin, которая поставляется с поставщиком данных SQLite.

## <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

API хранилища iCloud позволяет приложениям сохранять пользовательские документы и данные приложений в центральном расположении и обращаться к этим элементам со всех устройств пользователя.

## <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

Платформа CloudKit упрощает разработку приложений, обращающихся к iCloud. Сюда входит получение данных приложения и права доступа к ресурсам, а также возможность безопасного хранения сведений о приложении. Этот пакет обеспечивает пользователям уровень анонимности, предоставляя доступ к приложениям с помощью идентификаторов iCloud без предоставления персональных данных.
