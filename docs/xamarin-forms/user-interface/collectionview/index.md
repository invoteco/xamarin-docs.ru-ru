---
title: Xamarin.Forms CollectionView
description: CollectionView — это гибкий и эффективный представление для представления списков данных, используя спецификации другой макет.
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: a6cb6e695a4c19627b183060a7636320f8083ee2
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65048139"
---
# <a name="xamarinforms-collectionview"></a>Xamarin.Forms CollectionView

![](~/media/shared/preview.png "Этот API в настоящее время в предварительной версии")

## <a name="introductionintroductionmd"></a>[Введение](introduction.md)

`CollectionView` Является представлением гибкий и эффективный для представления списков данных, используя спецификации другой макет.

## <a name="datapopulate-datamd"></a>[Данные](populate-data.md)

Объект `CollectionView` заполняется данными, задав его `ItemsSource` свойство к любой коллекции, реализующий `IEnumerable`. Внешний вид каждого элемента в списке может быть определена путем задания `ItemTemplate` свойства [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate).

## <a name="layoutlayoutmd"></a>[Макет](layout.md)

По умолчанию `CollectionView` будет отображения элементов в вертикальном списке. Тем не менее можно указать вертикальные и горизонтальные списки и сеток.

## <a name="selectionselectionmd"></a>[Выбор](selection.md)

По умолчанию `CollectionView` выбор отключен. Тем не менее можно включить одиночное и множественное выделение.

## <a name="empty-viewsemptyviewmd"></a>[Пустой представления](emptyview.md)

В `CollectionView`, можно указать пустое представление, предоставляющий отзыв для пользователя, когда данные недоступны для отображения. Пустое представление может быть строка, представление или несколько представлений.

## <a name="scrollingscrollingmd"></a>[Прокрутка](scrolling.md)

Когда пользователь вставляет, чтобы инициировать прокрутки, конечное положение прокрутки можно управлять, чтобы полностью отображаются элементы. Кроме того `CollectionView` определяет два `ScrollTo` методы, которые программно прокрутки элементов в представлении. Одной из перегрузок прокручивает элемент по указанному индексу в представление, пока другой прокрутке указанного элемента.
