---
title: CollectionView Xamarin. Forms
description: CollectionView — это гибкое и производительное представление для представления списков данных с использованием различных спецификаций макета.
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/24/2019
ms.openlocfilehash: 8050d952556ce0b55a7ce72bc5f25de903fee6e5
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697001"
---
# <a name="xamarinforms-collectionview"></a>CollectionView Xamarin. Forms

## <a name="introductionintroductionmd"></a>[Введение](introduction.md)

[@No__t_1](xref:Xamarin.Forms.CollectionView) — это гибкое и производительное представление для представления списков данных с использованием различных спецификаций макета.

## <a name="datapopulate-datamd"></a>[Данные](populate-data.md)

[@No__t_1](xref:Xamarin.Forms.CollectionView) заполняется данными путем присвоения свойству [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) любой коллекции, реализующей `IEnumerable`. Внешний вид каждого элемента в списке можно определить, присвоив свойству [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate).

## <a name="layoutlayoutmd"></a>[Макет](layout.md)

По умолчанию в [`CollectionView`](xref:Xamarin.Forms.CollectionView) будут отображаться элементы в вертикальном списке. Однако можно указать вертикальные и горизонтальные списки и сетки.

## <a name="selectionselectionmd"></a>[Выбор](selection.md)

По умолчанию [`CollectionView`](xref:Xamarin.Forms.CollectionView) выбор отключен. Однако можно включить один и несколько элементов.

## <a name="empty-viewsemptyviewmd"></a>[Пустые представления](emptyview.md)

В [`CollectionView`](xref:Xamarin.Forms.CollectionView)можно указать пустое представление, которое предоставляет пользователю отзыв о том, что данные недоступны для отображения. Пустое представление может быть строкой, представлением или несколькими представлениями.

## <a name="scrollingscrollingmd"></a>[Прокрутка](scrolling.md)

Когда пользователь начинает прокручивать, можно управлять конечной позицией прокрутки, чтобы элементы отображались полностью. Кроме того, [`CollectionView`](xref:Xamarin.Forms.CollectionView) определяет два метода [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) , которые программным путем прокрутки элементов в представление. Одна из перегрузок прокручивается элемент по указанному индексу в представление, а другой Прокручивает указанный элемент в представление.

## <a name="groupinggroupingmd"></a>[Группирование](grouping.md)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) может отображать правильно сгруппированные данные, присвоив свойству `IsGrouped` значение `true`.
