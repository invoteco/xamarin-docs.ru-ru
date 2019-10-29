---
title: Данные и облачные службы в приложениях Xamarin. iOS
description: В этом документе содержатся ссылки на руководства, в которых описывается работа с локальными данными, iCloud и CloudKit в приложении Xamarin. iOS.
ms.prod: xamarin
ms.assetid: 945719F7-7CE6-4207-BF0F-23195125FC84
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/13/2017
ms.openlocfilehash: 4b3bf1fcc6be7713c87b16ef9ec083189793b324
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73008056"
---
# <a name="data-and-cloud-services-in-xamarinios-apps"></a>Данные и облачные службы в приложениях Xamarin. iOS

## <a name="data-accessiosdata-clouddataindexmd"></a>[Доступ к данным](~/ios/data-cloud/data/index.md)

Большинству приложений предъявляются некоторые требования для сохранения данных на устройстве локально. Если объем данных не является тривиальным, для управления доступом к базе данных обычно требуется база данных и уровень данных в приложении. в iOS имеется встроенное ядро СУБД SQLite, а доступ к данным упрощен платформой Xamarin, которая поставляется с поставщиком данных SQLite.

## <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

API хранилища iCloud в iOS 5 позволяет приложениям сохранять пользовательские документы и данные приложений в центральном расположении и обращаться к этим элементам со всех устройств пользователя.

## <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

Платформа CloudKit упрощает разработку приложений, обращающихся к iCloud. Сюда входит получение данных приложения и права доступа к ресурсам, а также возможность безопасного хранения сведений о приложении. Этот пакет обеспечивает пользователям уровень анонимности, предоставляя доступ к приложениям с помощью идентификаторов iCloud без предоставления персональных данных.
