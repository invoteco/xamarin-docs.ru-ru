---
title: Иерархия классов элементов управления Xamarin. Forms
description: Разработчики должны быть знакомы с иерархией типов, используемых для создания пользовательского интерфейса приложения Xamarin. Forms.
ms.prod: xamarin
ms.assetid: C89E6B98-464D-4BBE-BF11-13A5FCBBF420
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2019
ms.openlocfilehash: f08146d4439ff1fc22edea71ab1cbb337f64c037
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "68984396"
---
# <a name="xamarinforms-controls-class-hierarchy"></a>Иерархия классов элементов управления Xamarin. Forms

Xamarin. Forms состоят из сотен типов, которые находятся в нескольких пространствах имен. Разработчики должны быть знакомы с иерархией типов, используемых для создания пользовательского интерфейса приложения Xamarin. Forms, которое находится в пространстве имен `Xamarin.Forms`.

Эти типы можно разделить на страницы, макеты, представления и ячейки. Страница Xamarin. Forms обычно занимает весь экран, а все типы страниц являются производными от класса [`Page`](xref:Xamarin.Forms.Page) . Страницы обычно содержат макет, а все типы макетов являются производными от класса [`Layout`](xref:Xamarin.Forms.Layout) . Макет обычно содержит представления и, возможно, другие макеты, а все типы представлений являются производными от класса [`View`](xref:Xamarin.Forms.View) . Наконец, ячейки являются специализированными элементами управления, которые используются для вывода данных в [`TableView`](xref:Xamarin.Forms.TableView) и [`ListView`](xref:Xamarin.Forms.ListView) элементов управления. Страницы, макеты, представления и ячейки в конечном итоге являются производными от класса [`Element`](xref:Xamarin.Forms.Element) .

На следующей схеме классов показана иерархия типов, которые обычно используются для создания пользовательского интерфейса в Xamarin. Forms:

[![Схема классов элементов управления Xamarin. Forms](class-hierarchy-images/class-diagram.png "Схема классов элементов управления Xamarin. Forms")](class-hierarchy-images/class-diagram-large.png#lightbox "Схема классов элементов управления Xamarin. Forms")

> [!NOTE]
> Версию схемы классов с высоким разрешением можно скачать [отсюда](class-hierarchy-images/class-diagram-high-resolution.png). Однако обратите внимание, что на диаграмме в настоящее время не отображаются типы `CarouselView` и `CollectionView`. Они будут добавлены после того, как элементы управления выходят за пределы предварительной версии. Кроме того, на схеме показан только один тип оболочки.

## <a name="related-links"></a>Связанные ссылки

- [Справочник по элементам управления Xamarin. Forms](~/xamarin-forms/user-interface/controls/index.md)
