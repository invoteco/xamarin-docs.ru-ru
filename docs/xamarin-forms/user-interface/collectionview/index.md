---
title: Xamarin.Forms CollectionView
description: CollectionView — это гибкий и эффективный представление для представления списков данных, используя спецификации другой макет.
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 73e68f29c61661019cfc56f8caa26c6a0b1ce4ba
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/21/2019
ms.locfileid: "65971001"
---
# <a name="xamarinforms-collectionview"></a>Xamarin.Forms CollectionView

## <a name="introductionintroductionmd"></a>[Введение](introduction.md)

[ `CollectionView` ](xref:Xamarin.Forms.CollectionView) Является представлением гибкий и эффективный для представления списков данных, используя спецификации другой макет.

## <a name="datapopulate-datamd"></a>[Данные](populate-data.md)

Объект [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) заполняется данными, задав его [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView.ItemsSource) свойство к любой коллекции, реализующий `IEnumerable`. Внешний вид каждого элемента в списке может быть определена путем задания [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView.ItemTemplate) свойства [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate).

## <a name="layoutlayoutmd"></a>[Макет](layout.md)

По умолчанию [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) будет отображения элементов в вертикальном списке. Тем не менее можно указать вертикальные и горизонтальные списки и сеток.

## <a name="selectionselectionmd"></a>[Выбор](selection.md)

По умолчанию [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) выбор отключен. Тем не менее можно включить одиночное и множественное выделение.

## <a name="empty-viewsemptyviewmd"></a>[Пустой представления](emptyview.md)

В [ `CollectionView` ](xref:Xamarin.Forms.CollectionView), можно указать пустое представление, предоставляющий отзыв для пользователя, когда данные недоступны для отображения. Пустое представление может быть строка, представление или несколько представлений.

## <a name="scrollingscrollingmd"></a>[Прокрутка](scrolling.md)

Когда пользователь вставляет, чтобы инициировать прокрутки, конечное положение прокрутки можно управлять, чтобы полностью отображаются элементы. Кроме того [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) определяет два [ `ScrollTo` ](xref:Xamarin.Forms.ItemsView.ScrollTo*) методы, которые программно прокрутки элементов в представлении. Одной из перегрузок прокручивает элемент по указанному индексу в представление, пока другой прокрутке указанного элемента.
