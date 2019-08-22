---
title: CollectionView Xamarin. Forms
description: CollectionView — это гибкое и производительное представление для представления списков данных с использованием различных спецификаций макета.
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/24/2019
ms.openlocfilehash: b3841ed1287c980212ce37078f38f4984393c414
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/21/2019
ms.locfileid: "69888594"
---
# <a name="xamarinforms-collectionview"></a>CollectionView Xamarin. Forms

![](~/media/shared/preview.png "Этот API в настоящее время предоставляется в режиме предварительной версии")

## <a name="introductionintroductionmd"></a>[Введение](introduction.md)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) — Это гибкое и производительное представление для представления списков данных с использованием различных спецификаций макета.

## <a name="datapopulate-datamd"></a>[Данные](populate-data.md)

Заполняется данными путем установки его [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) свойства в любую коллекцию, реализующую `IEnumerable`. [`CollectionView`](xref:Xamarin.Forms.CollectionView) Внешний вид каждого элемента в списке можно определить, задав [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)для свойства значение.

## <a name="layoutlayoutmd"></a>[Макет](layout.md)

По умолчанию [`CollectionView`](xref:Xamarin.Forms.CollectionView) элементы отображаются в вертикальном списке. Однако можно указать вертикальные и горизонтальные списки и сетки.

## <a name="selectionselectionmd"></a>[Выбор](selection.md)

По умолчанию [`CollectionView`](xref:Xamarin.Forms.CollectionView) выделение отключено. Однако можно включить один и несколько элементов.

## <a name="empty-viewsemptyviewmd"></a>[Пустые представления](emptyview.md)

В [`CollectionView`](xref:Xamarin.Forms.CollectionView)можно указать пустое представление, которое предоставляет пользователю отзыв о том, что данные недоступны для отображения. Пустое представление может быть строкой, представлением или несколькими представлениями.

## <a name="scrollingscrollingmd"></a>[Прокрутка](scrolling.md)

Когда пользователь начинает прокручивать, можно управлять конечной позицией прокрутки, чтобы элементы отображались полностью. Кроме того, [`CollectionView`](xref:Xamarin.Forms.CollectionView) определяет два [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) метода: программными средствами прокрутки элементов в представление. Одна из перегрузок прокручивается элемент по указанному индексу в представление, а другой Прокручивает указанный элемент в представление.

## <a name="groupinggroupingmd"></a>[Группирование](grouping.md)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)может отображать правильно сгруппированные данные, присвоив `IsGrouped` `true`свойству значение.
