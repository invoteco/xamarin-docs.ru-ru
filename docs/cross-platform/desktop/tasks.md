---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: Распространенные задачи сравнения
description: В этом документе сравнивается как выполнять различные общие задачи в WPF и Xamarin.Forms. Он рассматривает кнопок, таймеры, размеры шрифтов, открыв URI и отображение лист действие.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 2991e74ec59e3c43c665941706c2d9ac94bfb9ad
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61269599"
---
# <a name="common-tasks-comparison"></a>Распространенные задачи сравнения

| Задача | WPF | Xamarin.Forms |
|--- |--- |--- |
|Отображать сообщение на экране с кнопками|`MessageBox`|`Page.DisplayAlert`|
|Создать таймер|Класс `DispatcherTimer`|`Device.StartTimer` статический метод|
|Получить размер шрифта по умолчанию|`SystemFonts` статический класс|`Device.GetNamedSize` статический метод|
|Откройте URI или URL-АДРЕСУ|`Process.Start`|`Device.OpenUri`|
|Открыть окно действия (список кнопок)|Н/Д|`Page.DisplayActionSheet`|
