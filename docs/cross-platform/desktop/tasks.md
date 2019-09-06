---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: Сравнение стандартных задач
description: В этом документе сравнивается, как выполнять различные распространенные задачи в WPF и Xamarin. Forms. В нем рассматриваются кнопки, таймеры, размеры шрифтов, открытие URI и отображение листа действий.
author: conceptdev
ms.author: crdun
ms.date: 04/26/2017
ms.openlocfilehash: fecd8ed774adbacf69e3b2db514a4698e71711d8
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290334"
---
# <a name="common-tasks-comparison"></a>Сравнение стандартных задач

| Задача | WPF | Xamarin.Forms |
|--- |--- |--- |
|Отображение на экране сообщения с кнопками|`MessageBox`|`Page.DisplayAlert`|
|Создание таймера|Класс `DispatcherTimer`|`Device.StartTimer`статический метод|
|Получить размер шрифта по умолчанию|`SystemFonts`Статический класс|`Device.GetNamedSize`статический метод|
|Открыть URI/URL-адрес|`Process.Start`|`Device.OpenUri`|
|Отображение листа действий (список кнопок)|Н/Д|`Page.DisplayActionSheet`|
