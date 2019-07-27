---
title: Использование ContentProvider
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 142541dcc35b55e43b54eeb729c486ac9fc88b54
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510075"
---
# <a name="using-a-contentprovider-with-xamarinandroid"></a>Использование ContentProvider с Xamarin. Android

Курсорадаптерс также можно использовать для вывода данных из ContentProvider.
Контентпровидерс позволяют получать доступ к данным, предоставляемым другими приложениями (включая данные системы Android, такие как контакты, носители и сведения о календаре).

Предпочтительный способ доступа к ContentProvider — с Курсорлоадер с помощью Лоадерманажер. Лоадерманажер был введен в Android 3,0 (API уровня 11, Хонэйкомб) для перемещения задач блокировки из основного потока, а использование Курсорлоадер позволяет загружать данные в поток перед привязкой к ListView для показа.

Дополнительные сведения см. в статье [Введение в контентпровидерс](~/android/platform/content-providers/index.md) .

