---
title: Данные и облачные службы в приложениях Xamarin.iOS
description: Этот документ содержит ссылки на руководства, описывающие способы работы с локальными данными, iCloud и CloudKit в приложении Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 945719F7-7CE6-4207-BF0F-23195125FC84
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/13/2017
ms.openlocfilehash: 930ff3df8266053ab5de09c01d82d8d583ffac81
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865552"
---
# <a name="data-and-cloud-services-in-xamarinios-apps"></a>Данные и облачные службы в приложениях Xamarin.iOS

## <a name="data-accessiosdata-clouddataindexmd"></a>[Доступ к данным](~/ios/data-cloud/data/index.md)

Большинство приложений имеют некоторые требования, чтобы сохранить данные на устройстве локально. Если объем данных, не является тривиально небольшой, обычно для этого базу данных и уровень данных в приложении для управления доступом к базе данных. iOS имеет ядро базы данных Sqlite, «заложены», и доступ к данным стало проще благодаря платформе Xamarin, который поставляется вместе с поставщиком данных SQLite.

## <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

Хранилище iCloud API в iOS 5 позволяет приложениям сохранять пользовательские документы и данные приложения в центральном расположении и доступ к ним получить все его устройства.

## <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

Инфраструктура CloudKit упрощает разработку приложений, доступа к iCloud. Сюда входят методы для получения данных приложений и средств права, а также возможность обеспечить безопасное хранение сведений о приложении. Этот комплект предоставляет пользователям уровня анонимности, разрешив доступ к приложениям с их iCloud идентификаторы без обмена личными данными.
