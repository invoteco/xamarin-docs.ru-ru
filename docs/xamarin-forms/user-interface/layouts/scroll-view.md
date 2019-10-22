---
title: Скроллвиев Xamarin. Forms
description: В этой статье объясняется, как использовать класс Xamarin. Forms Скроллвиев для представления макетов, которые не могут уместиться только на одном экране, и которые имеют содержимое, освобождая место для клавиатуры.
ms.prod: xamarin
ms.assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/17/2019
ms.openlocfilehash: 8d523c6da6ca7feaf6894123822f789f37455865
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696856"
---
# <a name="xamarinforms-scrollview"></a>Скроллвиев Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

[`ScrollView`](xref:Xamarin.Forms.ScrollView) содержит макеты и позволяет им прокручивать экран. `ScrollView` также используется, чтобы разрешить представлениям автоматически перемещаться в видимую часть экрана при отображении клавиатуры.

[![](scroll-view-images/layouts-sml.png "Xamarin.Forms Layouts")](scroll-view-images/layouts.png#lightbox "Xamarin.Forms Layouts")

## <a name="purpose"></a>Цель

[`ScrollView`](xref:Xamarin.Forms.ScrollView) можно использовать для того, чтобы более крупные представления хорошо отображались на небольших телефонах. Например, макет, который работает на iPhone 6S, может быть обрезан на iPhone 4S. Использование `ScrollView` позволит отображать обрезанные части макета на экране меньшего размера.

## <a name="usage"></a>Использование

> [!NOTE]
> [`ScrollView`](xref:Xamarin.Forms.ScrollView) объекты не должны быть вложенными. Кроме того, `ScrollView`s не должны быть вложенными с другими элементами управления, обеспечивающими прокрутку, например `ListView` и `WebView`.

[`ScrollView`](xref:Xamarin.Forms.ScrollView) предоставляет свойство `Content`, для которого можно задать одно представление или макет. Рассмотрим этот пример макета с очень большим Боксвиев, за которым следует `Entry`:

```xaml
<ContentPage.Content>
    <ScrollView>
        <StackLayout>
            <BoxView BackgroundColor="Red" HeightRequest="600" WidthRequest="150" />
            <Entry />
        </StackLayout>
    </ScrollView>
</ContentPage.Content>
```

В C#:

```csharp
var scroll = new ScrollView();
Content = scroll;
var stack = new StackLayout();
stack.Children.Add(new BoxView { BackgroundColor = Color.Red,    HeightRequest = 600, WidthRequest = 600 });
stack.Children.Add(new Entry());
```

Перед прокруткой пользователя отображается только `BoxView`:

![](scroll-view-images/scroll-start.png "BoxView in ScrollView")

Обратите внимание, что когда пользователь начинает вводить текст в `Entry`, представление прокручивается, чтобы оно оставалось видимым на экране:

![](scroll-view-images/scroll-end.png "Entry in ScrollView")

## <a name="properties"></a>Свойства

[`ScrollView`](xref:Xamarin.Forms.ScrollView) определяет следующие свойства:

- [`ContentSize`](xref:Xamarin.Forms.ScrollView.ContentSizeProperty) получает значение [`Size`](xref:Xamarin.Forms.Size) , представляющее размер содержимого.
- [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) получает или задает значение перечисления [`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation) , представляющее направление прокрутки `ScrollView`.
- [`ScrollX`](xref:Xamarin.Forms.ScrollView.ScrollXProperty) получает `double`, представляющий текущую позицией прокрутки по оси X.
- [`ScrollY`](xref:Xamarin.Forms.ScrollView.ScrollYProperty) получает `double`, представляющий текущую точку прокрутки по оси Y.
- [`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibilityProperty) получает или задает значение [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) , представляющее, когда отображается горизонтальная полоса прокрутки.
- [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.VerticalScrollBarVisibilityProperty) получает или задает [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) значение, представляющее, когда отображается вертикальная полоса прокрутки.

> [!NOTE]
> Прокрутку можно отключить, задав для свойства [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) значение `Neither`.

## <a name="methods"></a>Методы

[`ScrollView`](xref:Xamarin.Forms.ScrollView) предоставляет метод `ScrollToAsync`, который можно использовать для прокрутки представления либо с помощью координат, либо путем указания конкретного представления, которое следует сделать видимым.

При использовании координат укажите `x` и `y` координаты, а также логическое значение, указывающее, следует ли анимировать прокрутку:

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

> [!IMPORTANT]
> Метод `ScrollToAsync` не приведет к прокрутке, если свойство [`ScrollView.Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) имеет значение `Neither`.

При прокрутке к определенному элементу перечисление `ScrollToPosition` указывает, где будет отображаться элемент:

- **Центр** &ndash; прокручивает элемент до центра видимой части представления.
- **End** &ndash; прокручивает элемент до конца видимой части представления.
- **Макевисибле** &ndash; прокручивает элемент таким образом, чтобы он был виден в представлении.
- **Start** &ndash; прокручивает элемент до начала видимой части представления.

Свойство `IsAnimated` указывает, как будет прокручиваться представление. Если задано значение `true`, будет использоваться плавная анимация, а не мгновенное перемещение содержимого в представление.

## <a name="events"></a>события

[`ScrollView`](xref:Xamarin.Forms.ScrollView) определяет только одно событие, `Scrolled`. `Scrolled` возникает после завершения прокрутки представления. Обработчик событий для `Scrolled` принимает `ScrolledEventArgs`, который имеет свойства `ScrollX` и `ScrollY`. Ниже показано, как обновить метку с текущей позицией прокрутки `ScrollView`.

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

Обратите внимание, что позиции прокрутки могут быть отрицательными из-за того, что при прокрутке в конце списка будет действовать стрелка.

## <a name="related-links"></a>Связанные ссылки

- [Макет (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [Пример Бусинесстумбле (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
