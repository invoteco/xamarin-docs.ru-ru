---
title: Использование ContentProvider
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: c99b3d64f4f5033240c086af8677d31485a44f01
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028924"
---
# <a name="using-a-contentprovider-with-xamarinandroid"></a>Использование ContentProvider с Xamarin. Android

Курсорадаптерс также можно использовать для вывода данных из ContentProvider.
Контентпровидерс позволяют получать доступ к данным, предоставляемым другими приложениями (включая данные системы Android, такие как контакты, носители и сведения о календаре).

Предпочтительный способ доступа к ContentProvider — с Курсорлоадер с помощью Лоадерманажер. Лоадерманажер был введен в Android 3,0 (API уровня 11, Хонэйкомб) для перемещения задач блокировки из основного потока, а использование Курсорлоадер позволяет загружать данные в поток перед привязкой к ListView для показа.

Дополнительные сведения см. в статье [Введение в контентпровидерс](~/android/platform/content-providers/index.md) .
