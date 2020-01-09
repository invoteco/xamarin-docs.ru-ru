---
title: Макет Карауселвиев Xamarin. Forms
description: По умолчанию Карауселвиев отображает элементы по горизонтали. Однако также возможна вертикальная ориентация.
ms.prod: xamarin
ms.assetid: fede0382-c972-4023-a4ea-fe5cadec91a6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/14/2019
ms.openlocfilehash: 0149a66fedd98a94f1c9d96bf8e7e57715d1b90b
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75488261"
---
# <a name="xamarinforms-carouselview-layout"></a>Макет Карауселвиев Xamarin. Forms

![](~/media/shared/preview.png "This API is currently pre-release")

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView) определяет следующие свойства, управляющие макетом.

- [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)типа `LinearItemsLayout`указывает используемый макет.
- `PeekAreaInsets`типа [`Thickness`](xref:Xamarin.Forms.Thickness)указывает, какой объем смежных элементов должен быть частично видимым.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами. Это означает, что свойства могут быть целевыми объектами привязок данных.

По умолчанию элементы отображаются в [`CarouselView`](xref:Xamarin.Forms.CarouselView) горизонтальной ориентации. На экране будет отображаться один элемент с жестами прокрутки, приводящими к пересылке и обратной навигации по коллекции элементов. Однако также возможна вертикальная ориентация. Это обусловлено тем, что свойство [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) имеет тип `LinearItemsLayout`, который наследуется от класса [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) . Класс `ItemsLayout` определяет следующие свойства:

- [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation)типа [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)задает направление, в котором [`CarouselView`](xref:Xamarin.Forms.CarouselView) расширяется по мере добавления элементов.
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)типа [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment)задает способ выравнивания точек привязки по элементам.
- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)типа [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)задает поведение точек привязки при прокрутке.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами. Это означает, что свойства могут быть целевыми объектами привязок данных. Дополнительные сведения о точках привязки см. в разделе [точки привязки](scrolling.md#snap-points) в [CollectionViewе прокрутки Xamarin. Forms](scrolling.md) .

Перечисление [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) определяет следующие члены:

- `Vertical` указывает, что [`CarouselView`](xref:Xamarin.Forms.CarouselView) будет расширяться по вертикали при добавлении элементов.
- `Horizontal` указывает, что [`CarouselView`](xref:Xamarin.Forms.CarouselView) будет расширяться по горизонтали по мере добавления элементов.

Класс `LinearItemsLayout` наследует от класса [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) и определяет свойство `ItemSpacing` типа `double`, представляющее пустое пространство вокруг каждого элемента. Значение этого свойства по умолчанию равно 0, а его значение всегда должно быть больше или равно 0. Класс `LinearItemsLayout` также определяет статические `Vertical` и элементы `Horizontal`. Эти члены можно использовать для создания вертикальных или горизонтальных списков соответственно. Кроме того, можно создать объект `LinearItemsLayout`, указав в качестве аргумента член перечисления [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) .

> [!NOTE]
> для выполнения макета [`CarouselView`](xref:Xamarin.Forms.CarouselView) использует собственные обработчики макетов.

## <a name="horizontal-layout"></a>Горизонтальное расположение

По умолчанию [`CarouselView`](xref:Xamarin.Forms.CarouselView) будет отображать элементы по горизонтали. Поэтому нет необходимости устанавливать свойство [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) для использования этого макета:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                <Frame HasShadow="True"
                       BorderColor="DarkGray"
                       CornerRadius="5"
                       Margin="20"
                       HeightRequest="300"
                       HorizontalOptions="Center"
                       VerticalOptions="CenterAndExpand">
                    <StackLayout>
                        <Label Text="{Binding Name}"
                               FontAttributes="Bold"
                               FontSize="Large"
                               HorizontalOptions="Center"
                               VerticalOptions="Center" />
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="150"
                               WidthRequest="150"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Location}"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Details}"
                               FontAttributes="Italic"
                               HorizontalOptions="Center"
                               MaxLines="5"
                               LineBreakMode="TailTruncation" />
                    </StackLayout>
                </Frame>
            </StackLayout>
        </DataTemplate>
    </CarouselView.ItemTemplate>
</CarouselView>
```

Кроме того, этот макет можно также выполнить, задав для свойства [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) `LinearItemsLayout` объект, указав в качестве значения свойства `Orientation` элемент перечисления `Horizontal` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) .

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    ItemsLayout = LinearItemsLayout.Horizontal
};
```

Это приводит к увеличению макета по горизонтали по мере добавления новых элементов.

## <a name="vertical-layout"></a>Вертикальное расположение

[`CarouselView`](xref:Xamarin.Forms.CarouselView) могут отображать свои элементы по вертикали, присвоив свойству [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) значение `LinearItemsLayout` объекта, указав в качестве значения свойства`ItemsLayoutOrientation`элемент перечисления `Vertical` [`Orientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) :

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical" />
    </CarouselView.ItemsLayout>
    <CarouselView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                <Frame HasShadow="True"
                       BorderColor="DarkGray"
                       CornerRadius="5"
                       Margin="20"
                       HeightRequest="300"
                       HorizontalOptions="Center"
                       VerticalOptions="CenterAndExpand">
                    <StackLayout>
                        <Label Text="{Binding Name}"
                               FontAttributes="Bold"
                               FontSize="Large"
                               HorizontalOptions="Center"
                               VerticalOptions="Center" />
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="150"
                               WidthRequest="150"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Location}"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Details}"
                               FontAttributes="Italic"
                               HorizontalOptions="Center"
                               MaxLines="5"
                               LineBreakMode="TailTruncation" />
                    </StackLayout>
                </Frame>
            </StackLayout>
        </DataTemplate>
    </CarouselView.ItemTemplate>
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    ItemsLayout = LinearItemsLayout.Vertical
};
```

Это приводит к увеличению макета по вертикали по мере добавления новых элементов.

## <a name="partially-visible-adjacent-items"></a>Частично видимые смежные элементы

По умолчанию [`CarouselView`](xref:Xamarin.Forms.CarouselView) отображает все элементы одновременно. Однако это поведение можно изменить, присвоив свойству `PeekAreaInsets` значение `Thickness`, которое указывает, какой объем смежных элементов должен быть частично видимым. Это может быть полезно для того, чтобы пользователи могли просматривать дополнительные элементы. В следующем коде XAML показан пример задания этого свойства:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    PeekAreaInsets = new Thickness(100)
};
```

В результате смежные элементы будут частично видны на экране.

## <a name="item-spacing"></a>Промежуток между элементами

По умолчанию каждый элемент в [`CarouselView`](xref:Xamarin.Forms.CarouselView) не имеет пустого пространства вокруг него. Это поведение можно изменить, задав свойства в макете элементов, который используется `CarouselView`.

Когда [`CarouselView`](xref:Xamarin.Forms.CarouselView) задает для свойства [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) объект `LinearItemsLayout`, свойству `LinearItemsLayout.ItemSpacing` может быть присвоено значение `double`, представляющее пустое пространство вокруг каждого элемента:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           ItemSpacing="20" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

> [!NOTE]
> Свойство `LinearItemsLayout.ItemSpacing` имеет набор обратного вызова проверки, который гарантирует, что значение свойства всегда больше или равно 0.

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        ItemSpacing = 20
    }
};
```

Этот код приводит к вертикальному макету с отступом 20 вокруг каждого элемента.

## <a name="dynamic-resizing-of-items"></a>Динамическое изменение размера элементов

Элементы в [`CarouselView`](xref:Xamarin.Forms.CarouselView) могут динамически изменяться во время выполнения путем изменения связанных с макетом свойств элементов в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate). Например, в следующем примере кода изменяются свойства [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) и [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) объекта [`Image`](xref:Xamarin.Forms.Image) , а также свойство `HeightRequest` родительского [`Frame`](xref:Xamarin.Forms.Frame):

```csharp
void OnImageTapped(object sender, EventArgs e)
{
    Image image = sender as Image;
    image.HeightRequest = image.WidthRequest = image.HeightRequest.Equals(150) ? 200 : 150;
    Frame frame = ((Frame)image.Parent.Parent);
    frame.HeightRequest = frame.HeightRequest.Equals(300) ? 350 : 300;
}
```

Обработчик событий `OnImageTapped` выполняется в ответ на касание объекта [`Image`](xref:Xamarin.Forms.Image) и изменяет размеры изображения (и его родительского фрейма), чтобы его было проще просмотреть.

## <a name="right-to-left-layout"></a>Макет с письмом справа налево

[`CarouselView`](xref:Xamarin.Forms.CarouselView) можно разформатировать содержимое в направлении с направлением справа налево, задав для свойства [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) значение [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft). Однако в идеале свойство `FlowDirection` должно быть установлено на странице или в корневом макете, что приводит к тому, что все элементы на странице или в корневом макете отвечают на направление потока:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CarouselViewDemos.Views.HorizontalTemplateLayoutRTLPage"
             Title="Horizontal layout (RTL FlowDirection)"
             FlowDirection="RightToLeft">    
    <CarouselView ItemsSource="{Binding Monkeys}">
        ...
    </CarouselView>
</ContentPage>
```

[`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) по умолчанию для элемента с родительским элементом является [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent). Таким образом, [`CarouselView`](xref:Xamarin.Forms.CarouselView) наследует значение свойства `FlowDirection` из [`ContentPage`](xref:Xamarin.Forms.ContentPage).

Дополнительные сведения о направлении потока см. [в разделе локализация справа налево](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

## <a name="related-links"></a>Связанные ссылки

- [Карауселвиев (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [Локализация справа налево](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [Прокрутка Xamarin. Forms Карауселвиев](scrolling.md)
