---
title: Xamarin.Forms ScrollView
description: В этой статье объясняется, как использовать класс Xamarin.Forms ScrollView для представления макеты, не может поместиться на экран только один, и которые имеют содержимое освободить место для клавиатуры.
ms.prod: xamarin
ms.assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/18/2019
ms.openlocfilehash: bb10cda7c9899f176861ceee712cc876984c56ef
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75488274"
---
# <a name="xamarinforms-scrollview"></a>Xamarin.Forms ScrollView

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

[`ScrollView`](xref:Xamarin.Forms.ScrollView) содержит макеты и позволяет им прокрутки вне экрана. `ScrollView` также позволяет разрешить представления автоматически перейти на видимой области экрана, когда отображается клавиатуры.

[![](scroll-view-images/layouts-sml.png "Xamarin.Forms Layouts")](scroll-view-images/layouts.png#lightbox "Xamarin.Forms Layouts")

## <a name="purpose"></a>Цель

[`ScrollView`](xref:Xamarin.Forms.ScrollView) можно использовать для того, чтобы более крупные представления хорошо отображались на небольших телефонах. Например макет, который работает на устройстве iPhone 6s могут быть обрезаны на iPhone 4s. С помощью `ScrollView` позволит усеченные части макет для отображения на экране меньшего размера.

## <a name="usage"></a>Метрики

> [!NOTE]
> [`ScrollView`](xref:Xamarin.Forms.ScrollView) объекты не должны быть вложенными. Кроме того `ScrollView`s не должен быть вложенным с другими элементами управления, которые предоставляют прокрутка, подобно `ListView` и `WebView`.

[`ScrollView`](xref:Xamarin.Forms.ScrollView) предоставляет свойство `Content`, для которого можно задать одно представление или макет. Рассмотрим следующий пример макета с очень больших boxView, за которым следует `Entry`:

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
public class ScrollingDemoCode : ContentPage
{
    public ScrollingDemoCode()
    {
        StackLayout stackLayout = new StackLayout();
        stackLayout.Children.Add(new BoxView { BackgroundColor = Color.Red, HeightRequest = 600, WidthRequest = 150 });
        stackLayout.Children.Add(new Entry());
        ScrollView scrollView = new ScrollView();
        scrollView.Content = stackLayout;
        Content = scrollView;
    }
}
```

Прежде чем пользователь выполняет прокрутку вниз, только `BoxView` отображается:

![](scroll-view-images/scroll-start.png "BoxView in ScrollView")

Обратите внимание, что, когда пользователь начинает вводить текст в `Entry`, представление прокручивается, чтобы закрепить на экране:

![](scroll-view-images/scroll-end.png "Entry in ScrollView")

## <a name="properties"></a>Свойства

[`ScrollView`](xref:Xamarin.Forms.ScrollView) определяет следующие свойства:

- [`ContentSize`](xref:Xamarin.Forms.ScrollView.ContentSizeProperty) Получает [ `Size` ](xref:Xamarin.Forms.Size) значение, представляющее размер содержимого.
- [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) Возвращает или задает [ `ScrollOrientation` ](xref:Xamarin.Forms.ScrollOrientation) значение перечисления, представляющее направление прокрутки `ScrollView`.
- [`ScrollX`](xref:Xamarin.Forms.ScrollView.ScrollXProperty) Возвращает `double` , представляющий текущий позиция прокрутки по оси Х.
- [`ScrollY`](xref:Xamarin.Forms.ScrollView.ScrollYProperty) Возвращает `double` , представляющий текущую позицию прокрутки Y.
- [`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibilityProperty) Возвращает или задает [ `ScrollBarVisibility` ](xref:Xamarin.Forms.ScrollBarVisibility) значение, представляющее горизонтальная полоса прокрутки видима.
- [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.VerticalScrollBarVisibilityProperty) Возвращает или задает [ `ScrollBarVisibility` ](xref:Xamarin.Forms.ScrollBarVisibility) значение, представляющее вертикальная полоса прокрутки видима.

> [!NOTE]
> Прокрутку можно отключить, задав для свойства [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) значение `Neither`.

## <a name="methods"></a>Методы

[`ScrollView`](xref:Xamarin.Forms.ScrollView) предоставляет метод `ScrollToAsync`, который можно использовать для прокрутки представления либо с помощью координат, либо путем указания конкретного представления, которое следует сделать видимым.

При использовании координаты, указать `x` и `y` координаты, а также логическое значение, указывающее, является ли прокрутка анимации:

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

> [!IMPORTANT]
> Метод `ScrollToAsync` не приведет к прокрутке, если свойство [`ScrollView.Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) имеет значение `Neither`.

При прокрутке к определенному элементу перечисление `ScrollToPosition` указывает, где будет отображаться элемент:

- **Center** &ndash; прокручивает элемент относительно центральной части видимой части представления.
- **Конец** &ndash; прокручивает элемент в конец видимой части представления.
- **MakeVisible** &ndash; прокручивает элемент, чтобы он отображается в представлении.
- **Запуск** &ndash; прокручивает элемент в начало видимой части представления.

`IsAnimated` Свойство определяет, как представление прокручивается. Если задано значение `true`, будет использоваться плавная анимация, а не мгновенное перемещение содержимого в представление.

## <a name="events"></a>события

[`ScrollView`](xref:Xamarin.Forms.ScrollView) определяет только одно событие, `Scrolled`. `Scrolled` вызывается после завершения прокрутки представления. Обработчик событий для `Scrolled` принимает `ScrolledEventArgs`, который имеет `ScrollX` и `ScrollY` свойства. Следующий подход демонстрирует обновление метки с использованием текущей позиции прокрутки `ScrollView`:

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

Обратите внимание, что позиции прокрутки может быть отрицательным, из-за эффекта bounce при прокрутке в конце списка.

## <a name="related-links"></a>Связанные ссылки

- [Макет (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [Пример BusinessTumble (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
