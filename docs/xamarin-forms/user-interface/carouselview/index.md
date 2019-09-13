---
title: Карауселвиев Xamarin. Forms
description: Карауселвиев — это представление для представления списков данных в виде схемы, похожей на «обойма».
ms.prod: xamarin
ms.assetid: 5b673347-cdba-4532-820f-fb5f070c86bc
ms.technology: xamarin-forms
author: pauldipietro
ms.author: padipi
ms.date: 09/09/2019
ms.openlocfilehash: 06d20341053c4f77cb72aac9e1abdc85d9f95fdb
ms.sourcegitcommit: e83035c746f165ee6d03f2e9fd0066ee4f20a9fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/11/2019
ms.locfileid: "70908334"
---
# <a name="xamarinforms-carouselview"></a>Карауселвиев Xamarin. Forms

![](~/media/shared/preview.png "Этот API в настоящее время предоставляется в режиме предварительной версии")

> [!IMPORTANT]
> Документация по Карауселвиев находится на стадии разработки, а некоторые ссылки могут ссылаться на документацию по CollectionView. В большинстве случаев информация должна быть применима из-за природы Карауселвиев, основанной на CollectionView.

## <a name="introductionintroductionmd"></a>[Введение](introduction.md)

Представляет [`CarouselView`](xref:Xamarin.Forms.CarouselView) собой представление для представления данных в виде схемы, похожей на «обойму». Его реализация основана на этом [`CollectionView`](xref:Xamarin.Forms.CollectionView).

## <a name="datacollectionviewpopulate-datamd"></a>[Данные](../collectionview/populate-data.md)

Заполняется данными путем установки его [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) свойства в любую коллекцию, реализующую `IEnumerable`. [`CarouselView`](xref:Xamarin.Forms.CarouselView) Внешний вид каждого элемента в списке можно определить, задав [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)для свойства значение.

## <a name="layoutlayoutmd"></a>[Макет](layout.md)

По умолчанию [`CarouselView`](xref:Xamarin.Forms.CarouselView) элементы отображаются в горизонтальном списке. Однако он также имеет доступ к тем же макетам, что и CollectionView, включая вертикальную ориентацию.

## <a name="empty-viewscollectionviewemptyviewmd"></a>[Пустые представления](../collectionview/emptyview.md)

В [`CarouselView`](xref:Xamarin.Forms.CarouselView)можно указать пустое представление, которое предоставляет пользователю отзыв о том, что данные недоступны для отображения. Пустое представление может быть строкой, представлением или несколькими представлениями.

## <a name="scrollingcollectionviewscrollingmd"></a>[Прокрутка](../collectionview/scrolling.md)

Когда пользователь начинает прокручивать, можно управлять конечной позицией прокрутки, чтобы элементы отображались полностью. Кроме того, [`CarouselView`](xref:Xamarin.Forms.CarouselView) определяет два [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) метода: программными средствами прокрутки элементов в представление. Одна из перегрузок прокручивается элемент по указанному индексу в представление, а другой Прокручивает указанный элемент в представление.
