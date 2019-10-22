---
title: Карауселвиев Xamarin. Forms
description: Карауселвиев — это представление для представления данных в прокручиваемом макете, где пользователи могут перемещаться для перемещения по коллекции элементов.
ms.prod: xamarin
ms.assetid: 5b673347-cdba-4532-820f-fb5f070c86bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/08/2019
ms.openlocfilehash: 816c1b6e4ab497d0ada0f80fa3ad4800912587c3
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696990"
---
# <a name="xamarinforms-carouselview"></a>Карауселвиев Xamarin. Forms

![](~/media/shared/preview.png "This API is currently pre-release")

## <a name="introductionintroductionmd"></a>[Введение](introduction.md)

[@No__t_1](xref:Xamarin.Forms.CarouselView) представляет собой представление для представления данных в прокручиваемом макете, где пользователи могут перемещаться для перемещения по коллекции элементов.

## <a name="datapopulate-datamd"></a>[Данные](populate-data.md)

[@No__t_1](xref:Xamarin.Forms.CarouselView) заполняется данными путем присвоения свойству [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) любой коллекции, реализующей `IEnumerable`. Внешний вид каждого элемента можно определить, задав для свойства [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate).

## <a name="layoutlayoutmd"></a>[Макет](layout.md)

По умолчанию в [`CarouselView`](xref:Xamarin.Forms.CarouselView) будут отображаться элементы в горизонтальном списке. Однако он также имеет доступ к тем же макетам, что и CollectionView, включая вертикальную ориентацию.

## <a name="interactioninteractionmd"></a>[Взаимодействовать](interaction.md)

Доступ к отображаемому в данный момент элементу в [`CarouselView`](xref:Xamarin.Forms.CarouselView) можно получить с помощью свойств `CurrentItem` и `Position`.

## <a name="empty-viewsemptyviewmd"></a>[Пустые представления](emptyview.md)

В [`CarouselView`](xref:Xamarin.Forms.CarouselView)можно указать пустое представление, которое предоставляет пользователю отзыв о том, что данные недоступны для отображения. Пустое представление может быть строкой, представлением или несколькими представлениями.

## <a name="scrollingscrollingmd"></a>[Прокрутка](scrolling.md)

Когда пользователь начинает прокручивать, можно управлять конечной позицией прокрутки, чтобы элементы отображались полностью. Кроме того, [`CarouselView`](xref:Xamarin.Forms.CarouselView) определяет два метода [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) , которые программным путем прокрутки элементов в представление. Одна из перегрузок прокручивается элемент по указанному индексу в представление, а другой Прокручивает указанный элемент в представление.
