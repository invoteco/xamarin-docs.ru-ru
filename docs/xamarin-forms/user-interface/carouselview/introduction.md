---
title: Введение в Xamarin. Forms Карауселвиев
description: Карауселвиев — это представление для представления данных в виде схемы, похожей на «обойму».
ms.prod: xamarin
ms.assetid: 2a96e4bd-c29b-4658-bb4c-ab00872b0f8f
ms.technology: xamarin-forms
author: pauldipietro
ms.author: padipi
ms.date: 09/09/2019
ms.openlocfilehash: 244582d579780a962dfcb25c1a081d768b42acd3
ms.sourcegitcommit: e83035c746f165ee6d03f2e9fd0066ee4f20a9fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/11/2019
ms.locfileid: "70908352"
---
# <a name="xamarinforms-carouselview-introduction"></a>Введение в Xamarin. Forms Карауселвиев

![](~/media/shared/preview.png "Этот API в настоящее время предоставляется в режиме предварительной версии")

[`CarouselView`](xref:Xamarin.Forms.CarouselView)представляет собой представление для представления информации в виде, похожем на обойму.


[`CarouselView`](xref:Xamarin.Forms.CarouselView)доступна в Xamarin. Forms 4,3. Однако в настоящее время он эксперименталь и может использоваться только путем добавления следующей строки кода в `AppDelegate` класс в iOS или `MainActivity` в класс в Android перед вызовом `Forms.Init`:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

_Примечание. На момент написания этой статьи Карауселвиев по-прежнему использует флаг CollectionView, чтобы включить его функциональность._

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)доступна в iOS и Android, но некоторые функции могут быть частично доступны только на универсальная платформа Windows.

## <a name="when-to-use-carouselview"></a>Когда следует использовать Карауселвиев

Несмотря на то, что Карауселвиев значительно из реализации CollectionView, его назначение является более специализированным. Хотя использование CollectionView и Карауселвиев осуществляется по собственному усмотрению, они обычно используются в приложениях для выделения информации и ограничены в общем количестве используемых элементов.
