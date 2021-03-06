---
title: Поля и заполнение
description: Свойства Margin и Padding управляют поведением макета при отрисовке элемента в пользовательском интерфейсе. В этой статье показано различие между двумя свойствами и их заданием.
ms.prod: xamarin
ms.assetid: BEB096BB-51DF-410F-B0F1-D235287B0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 66ac81631466131cf1ef44dde39aa768d31b65a1
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "70772489"
---
# <a name="margin-and-padding"></a>Поля и заполнение

_Свойства Margin и Padding управляют поведением макета при отрисовке элемента в пользовательском интерфейсе. В этой статье показано различие между двумя свойствами и их заданием._

## <a name="overview"></a>Обзор

Поля и заполнение — это связанные понятия макета:

- Свойство [`Margin`](xref:Xamarin.Forms.View.Margin) представляет расстояние между элементом и его соседними элементами, а также используется для управления позицией отрисовки элемента и позицией отрисовки ее соседей. `Margin` значения могут быть заданы в классах [макета](~/xamarin-forms/user-interface/controls/layouts.md) и [представления](~/xamarin-forms/user-interface/controls/views.md) .
- Свойство [`Padding`](xref:Xamarin.Forms.Layout.Padding) представляет расстояние между элементом и его дочерними элементами и используется для отделения элемента управления от его собственного содержимого. `Padding` значения могут быть указаны в классах [макета](~/xamarin-forms/user-interface/controls/layouts.md) .

На следующей схеме показаны два понятия:

[![](margin-and-padding-images/margins-and-padding-sml.png "Margins and Padding Concepts")](margin-and-padding-images/margins-and-padding.png#lightbox "Margins and Padding Concepts")

Обратите внимание, что [`Margin`](xref:Xamarin.Forms.View.Margin) значения являются аддитивными. Таким образом, если два соседних элемента задают поле шириной 20 пикселей, расстояние между элементами будет 40 пикселей. Кроме того, поля и дополнения являются аддитивными при применении обоих типов, в которых расстояние между элементом и любым содержимым будет полем плюс заполнение.

## <a name="specifying-a-thickness"></a>Задание толщины

Свойства [`Margin`](xref:Xamarin.Forms.View.Margin) и [`Padding`](xref:Xamarin.Forms.Layout.Padding) имеют тип [`Thickness`](xref:Xamarin.Forms.Thickness). Существует три варианта создания структуры `Thickness`:

- Создайте структуру [`Thickness`](xref:Xamarin.Forms.Thickness) , определенную одним равномерным значением. Единственное значение применяется к левой, верхней, правой и нижней сторонам элемента.
- Создание [`Thickness`](xref:Xamarin.Forms.Thickness) структуры, определяемой горизонтальными и вертикальными значениями. Горизонтальное значение будет симметрично применено к левой и правой сторонам элемента, а вертикальное значение будет симметрично применено к верхней и нижней сторонам элемента.
- Создайте структуру [`Thickness`](xref:Xamarin.Forms.Thickness) , определяемую четырьмя разными значениями, которые применяются к левой, верхней, правой и нижней сторонам элемента.

В следующем примере кода XAML показаны все три возможности:

```xaml
<StackLayout Padding="0,20,0,0">
  <Label Text="Xamarin.Forms" Margin="20" />
  <Label Text="Xamarin.iOS" Margin="10, 15" />
  <Label Text="Xamarin.Android" Margin="0, 20, 15, 5" />
</StackLayout>
```

Эквивалентный код на языке C# показан в следующем примере:

```csharp
var stackLayout = new StackLayout {
  Padding = new Thickness(0,20,0,0),
  Children = {
    new Label { Text = "Xamarin.Forms", Margin = new Thickness (20) },
    new Label { Text = "Xamarin.iOS", Margin = new Thickness (10, 25) },
    new Label { Text = "Xamarin.Android", Margin = new Thickness (0, 20, 15, 5) }
  }
};
```

> [!NOTE]
> `Thickness` значения могут быть отрицательными, что обычно обрезает или перерисовывает содержимое.

## <a name="summary"></a>Сводка

В этой статье было продемонстрировано различие между свойствами [`Margin`](xref:Xamarin.Forms.View.Margin) и [`Padding`](xref:Xamarin.Forms.Layout.Padding) и их заданием. Поведение макета элемента управления свойства при отрисовке элемента в пользовательском интерфейсе.

## <a name="related-links"></a>Связанные ссылки

- [Страниц](xref:Xamarin.Forms.View.Margin)
- [Заполнение](xref:Xamarin.Forms.Layout.Padding)
- [Толщина](xref:Xamarin.Forms.Thickness)
