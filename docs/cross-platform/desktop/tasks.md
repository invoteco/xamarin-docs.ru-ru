---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: Сравнение стандартных задач
description: В этом документе сравнивается, как выполнять различные распространенные задачи в WPF и Xamarin. Forms. В нем рассматриваются кнопки, таймеры, размеры шрифтов, открытие URI и отображение листа действий.
author: davidortinau
ms.author: daortin
ms.date: 04/26/2017
ms.openlocfilehash: d1f1430d8044f94c17a19d747334b5ed8a1441cf
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016423"
---
# <a name="common-tasks-comparison"></a>Сравнение стандартных задач

| Задача | WPF | Xamarin.Forms |
|--- |--- |--- |
|Отображение на экране сообщения с кнопками|`MessageBox`|`Page.DisplayAlert`|
|Создание таймера|Класс `DispatcherTimer`|`Device.StartTimer` статический метод|
|Получить размер шрифта по умолчанию|`SystemFonts` статический класс|`Device.GetNamedSize` статический метод|
|Открыть URI/URL-адрес|`Process.Start`|`Device.OpenUri`|
|Отображение листа действий (список кнопок)|Н/Д|`Page.DisplayActionSheet`|
