---
title: Использование ContentProvider
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 5948d4b5db53db97c4e76cb7568c109b5faf90fe
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762202"
---
# <a name="using-a-contentprovider-with-xamarinandroid"></a>Использование ContentProvider с Xamarin. Android

Курсорадаптерс также можно использовать для вывода данных из ContentProvider.
Контентпровидерс позволяют получать доступ к данным, предоставляемым другими приложениями (включая данные системы Android, такие как контакты, носители и сведения о календаре).

Предпочтительный способ доступа к ContentProvider — с Курсорлоадер с помощью Лоадерманажер. Лоадерманажер был введен в Android 3,0 (API уровня 11, Хонэйкомб) для перемещения задач блокировки из основного потока, а использование Курсорлоадер позволяет загружать данные в поток перед привязкой к ListView для показа.

Дополнительные сведения см. в статье [Введение в контентпровидерс](~/android/platform/content-providers/index.md) .
