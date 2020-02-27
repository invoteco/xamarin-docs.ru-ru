---
title: Введение в Xamarin. Forms Карауселвиев
description: Карауселвиев — это представление для представления данных в прокручиваемом макете, где пользователи могут перемещаться для перемещения по коллекции элементов.
ms.prod: xamarin
ms.assetid: 2a96e4bd-c29b-4658-bb4c-ab00872b0f8f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/08/2019
ms.openlocfilehash: 2c3e15ce68ad1507318a1d8155f9ab03095ea409
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635584"
---
# <a name="xamarinforms-carouselview-introduction"></a>Введение в Xamarin. Forms Карауселвиев

![](~/media/shared/preview.png "This API is currently pre-release")

[`CarouselView`](xref:Xamarin.Forms.CarouselView) представляет собой представление для представления данных в прокручиваемом макете, где пользователи могут перемещаться для перемещения по коллекции элементов. По умолчанию `CarouselView` отображает элементы по горизонтали. На экране будет отображаться один элемент с жестами прокрутки, приводящими к пересылке и обратной навигации по коллекции элементов. Кроме того, можно отображать индикаторы, представляющие каждый элемент в `CarouselView`:

[![Снимок экрана Карауселвиев и Индикаторвиев на iOS и Android](populate-data-images/indicators.png "Индикаторвиев круги")](populate-data-images/indicators-large.png#lightbox "Индикаторвиев круги")

[`CarouselView`](xref:Xamarin.Forms.CarouselView) доступен в Xamarin. forms 4,3. Однако в настоящее время это экспериментальное и может использоваться только путем добавления следующей строки кода в класс `AppDelegate` в iOS или в класс `MainActivity` в Android перед вызовом `Forms.Init`:

```csharp
Forms.SetFlags("CarouselView_Experimental");
```

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView) доступен в iOS и Android, но некоторые функции могут быть доступны только частично на универсальная платформа Windows.

[`CarouselView`](xref:Xamarin.Forms.CarouselView) использует большую часть своей реализации с [`CollectionView`](xref:Xamarin.Forms.CollectionView). Однако эти два элемента управления имеют разные варианты использования. `CollectionView` обычно используется для представления списков данных любой длины, в то время как `CarouselView` обычно используется для выделения информации в списке ограниченной длины.
