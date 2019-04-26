---
title: Использование ContentProvider
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: d1ec628de3481820f320a5a8e6ef88fcbaab75a6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61189741"
---
# <a name="using-a-contentprovider"></a>Использование ContentProvider

CursorAdapters также может использоваться для отображения данных из ContentProvider.
Поставщиков содержимого позволяют получить доступ к данным, доступных в других приложениях (включая Android системные данные, такие как контакты, мультимедиа и информации календаря).

С помощью LoaderManager CursorLoader является предпочтительным способом доступа к ContentProvider. LoaderManager появился в Android 3.0 (уровень API 11, Honeycomb) для перемещения блокирующих задачах вне основного потока, и с помощью CursorLoader позволяет данные загружаются в потоке перед привязанным к ListView для отображения.

Ссылаться на [введение поставщиков содержимого](~/android/platform/content-providers/index.md) Дополнительные сведения.

