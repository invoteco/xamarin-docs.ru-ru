---
title: Почему конструктор XAML в Visual Studio не работает с XAML-файлами Xamarin.Forms?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: cab2eefb-c52f-4d81-866e-8f1feabbdd64
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 43088beba6c6a86330cac164856be98d88f07fe2
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61247806"
---
# <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-files"></a>Почему конструктор XAML в Visual Studio не работает с XAML-файлами Xamarin.Forms?

Xamarin.Forms сейчас не поддерживает визуальные конструкторы для файлов XAML. По этой причине при попытке открыть файл Forms XAML в Visual Studio *конструктор пользовательского интерфейса XAML* или *конструктор пользовательского интерфейса XAML с кодировкой*, выдается сообщение об ошибке:

> «Не удается открыть файл с помощью выбранного редактора. Выберите другой редактор.»

Это ограничение описан в [Обзор](~/xamarin-forms/xaml/xaml-basics/index.md#Overview) раздел [основы XAML Xamarin.Forms](~/xamarin-forms/xaml/xaml-basics/index.md) руководства:

> «Не существует, но визуальный конструктор для создания XAML в приложениях Xamarin.Forms, так что все XAML должен быть написан вручную».

Тем не менее, могут отображаться средство предварительного просмотра XAML Xamarin.Forms, выбрав **View > Other Windows > Xamarin.Forms средства предварительного просмотра** пункт меню.
