---
title: Макет CollectionView Xamarin.Forms
description: По умолчанию CollectionView будет отображения элементов в вертикальном списке. Тем не менее можно указать вертикальные и горизонтальные списки и сеток.
ms.prod: xamarin
ms.assetid: 5FE78207-1BD6-4706-91EF-B13932321FC9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 74cfade0cd872ae107f74ca86d6bc5fffa22e1f6
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/21/2019
ms.locfileid: "65971285"
---
# <a name="xamarinforms-collectionview-layout"></a>Макет CollectionView Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) определяет следующие свойства, которые управляют макета:

- [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout), типа [ `IItemsLayout` ](xref:Xamarin.Forms.IItemsLayout), определяющий макет для использования.
- [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemsView.ItemSizingStrategy), типа [ `ItemSizingStrategy` ](xref:Xamarin.Forms.ItemSizingStrategy), указывает используемая стратегия элемента измерения.

Эти свойства поддерживаются [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) объектов, что означает, что свойства могут быть целями привязки данных.

По умолчанию [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) будет отображения элементов в вертикальном списке. Тем не менее любой из следующих типов можно использовать:

- Вертикальный список — список один столбец, по мере добавления новых элементов увеличивается по вертикали.
- Горизонтальный список — список одну строку, по мере добавления новых элементов увеличивается по горизонтали.
- Вертикальная сетка — сетка нескольких столбцов, по мере добавления новых элементов увеличивается по вертикали.
- Горизонтальная сетка — сетка нескольких строк, по мере добавления новых элементов увеличивается по горизонтали.

Эти макеты можно задать, присвоив [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) свойство в класс, который является производным от [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsLayout) класса. Этот класс определяет следующие свойства:

- [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation), типа [ `ItemsLayoutOrientation` ](xref:Xamarin.Forms.ItemsLayoutOrientation), указывает направление, в котором [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) увеличивается по мере добавления элементов.
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment), типа [ `SnapPointsAlignment` ](xref:Xamarin.Forms.SnapPointsAlignment), выравнивание точек прикрепления с элементами.
- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType), типа [ `SnapPointsType` ](xref:Xamarin.Forms.SnapPointsType), определяет поведение точки прикрепления во время прокрутки.

Эти свойства поддерживаются [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) объектов, что означает, что свойства могут быть целями привязки данных. Дополнительные сведения о точки прикрепления, см. в разделе [привязать точки](scrolling.md#snap-points) в [прокрутка CollectionView Xamarin.Forms](scrolling.md) руководства.

[ `ItemsLayoutOrientation` ](xref:Xamarin.Forms.ItemsLayoutOrientation) Перечисление определяет следующие члены:

- `Vertical` Указывает, что [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) расширяется по вертикали, так как элементы добавляются.
- `Horizontal` Указывает, что [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) расширяются горизонтально при добавлении элементов.

[ `ListItemsLayout` ](xref:Xamarin.Forms.ListItemsLayout) Класс наследует от [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsLayout) класса и определяет статические `VerticalList` и `HorizontalList` членов. Эти элементы можно создать вертикальные или горизонтальные списки, соответственно. Кроме того `ListItemsLayout` объект может быть создан, указав [ `ItemsLayoutOrientation` ](xref:Xamarin.Forms.ItemsLayoutOrientation) член перечисления в качестве аргумента.

[ `GridItemsLayout` ](xref:Xamarin.Forms.GridItemsLayout) Класс наследует от [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsLayout) класса и определяет `Span` свойство типа `int`, который представляет количество столбцов или строк, отображаемых в сетке. Значение по умолчанию `Span` свойства-1, и его значение всегда должно быть больше или равно 1.

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) использует собственный макет ядер для выполнения макета.

## <a name="vertical-list"></a>Вертикальный список

По умолчанию [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) будет отображения элементов в вертикальном списке макета. Таким образом, нет необходимости задать [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) свойства, используемого для этой структуры:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Однако для полноты информации [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) можно задать для отображения элементов в вертикальном списке, задав его [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) свойство статического `ListItemsLayout.VerticalList` член:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="{x:Static ListItemsLayout.VerticalList}">
    ...
</CollectionView>
```

Кроме того, это можно также сделать, задав [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) свойство в объект [ `ListItemsLayout` ](xref:Xamarin.Forms.ListItemsLayout) класса, указав `Vertical` [ `ItemsLayoutOrientation` ](xref:Xamarin.Forms.ItemsLayoutOrientation) член перечисления в качестве аргумента:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout>
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>    
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = ListItemsLayout.VerticalList
};
```

Это приводит список один столбец, который увеличивается по вертикали добавляются новые элементы:

[![Снимок экрана: макет вертикальный список CollectionView, iOS и Android](layout-images/vertical-list.png "макета вертикальный список CollectionView")](layout-images/vertical-list-large.png#lightbox "CollectionView вертикальный список макета")

## <a name="horizontal-list"></a>Горизонтальный список

[`CollectionView`](xref:Xamarin.Forms.CollectionView) можно отобразить элементы в список по горизонтали, задав его [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) свойство статического `ListItemsLayout.HorizontalList` член:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="{x:Static ListItemsLayout.HorizontalList}">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="140" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Кроме того, это можно также сделать, задав [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) свойства [ `ListItemsLayout` ](xref:Xamarin.Forms.ListItemsLayout) объекта, указав `Horizontal` [ `ItemsLayoutOrientation` ](xref:Xamarin.Forms.ItemsLayoutOrientation) член перечисления в качестве аргумента:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout>
            <x:Arguments>
                <ItemsLayoutOrientation>Horizontal</ItemsLayoutOrientation>    
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = ListItemsLayout.HorizontalList
};
```

Это приводит список одной строки, который увеличивается по горизонтали как добавляются новые элементы:

[![Снимок экрана: макет горизонтальный список CollectionView, iOS и Android](layout-images/horizontal-list.png "макет горизонтальный список CollectionView")](layout-images/horizontal-list-large.png#lightbox "CollectionView макет горизонтальный список")

## <a name="vertical-grid"></a>Вертикальную сетку

[`CollectionView`](xref:Xamarin.Forms.CollectionView) можно отобразить элементы в вертикальную сетку, задав его [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) свойства [ `GridItemsLayout` ](xref:Xamarin.Forms.GridItemsLayout) которого [ `Orientation` ](xref:Xamarin.Forms.ItemsLayout.Orientation) свойству `Vertical`:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Vertical"
                        Span="2" />
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="80" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(2, ItemsLayoutOrientation.Vertical)
};
```

По умолчанию вертикальный [ `GridItemsLayout` ](xref:Xamarin.Forms.GridItemsLayout) будет отображать элементы в одном столбце. Тем не менее, в этом примере устанавливается `GridItemsLayout.Span` значение 2. Это приводит к таблице два столбца, которая по мере добавления новых элементов увеличивается по вертикали:

[![Снимок экрана: макет CollectionView вертикальной сетки, в iOS и Android](layout-images/vertical-grid.png "CollectionView вертикальную сетку макета")](layout-images/vertical-grid-large.png#lightbox "CollectionView вертикальной сетки макета")

## <a name="horizontal-grid"></a>Горизонтальная сетка

[`CollectionView`](xref:Xamarin.Forms.CollectionView) можно отобразить элементы в виде горизонтальной сетки, задав его [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) свойства [ `GridItemsLayout` ](xref:Xamarin.Forms.GridItemsLayout) которого[ `Orientation` ](xref:Xamarin.Forms.ItemsLayout.Orientation) свойство имеет значение `Horizontal`:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Horizontal"
                        Span="4" />
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="140" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(4, ItemsLayoutOrientation.Horizontal)
};
```

По умолчанию горизонтального [ `GridItemsLayout` ](xref:Xamarin.Forms.GridItemsLayout) будут доступны элементы в одну строку. Тем не менее, в этом примере устанавливается `GridItemsLayout.Span` значение 4. Это приводит к сетке четыре строки по мере добавления новых элементов увеличивается по горизонтали:

[![Снимок экрана: макет CollectionView горизонтальной сетки, в iOS и Android](layout-images/horizontal-grid.png "CollectionView горизонтальной сетки макета")](layout-images/horizontal-grid-large.png#lightbox "CollectionView горизонтальной сетки макета")

## <a name="item-sizing"></a>Изменение размера элемента

По умолчанию каждый элемент в [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) не каждый измеряется и размера, при условии, что элементы пользовательского интерфейса в [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) не указать фиксированных размеров. Это поведение, которое может быть изменена, определяется [ `CollectionView.ItemSizingStrategy` ](xref:Xamarin.Forms.ItemsView.ItemSizingStrategy) значение свойства. Значение этого свойства может быть присвоено одно из [ `ItemSizingStrategy` ](xref:Xamarin.Forms.ItemSizingStrategy) членов перечисления:

- `MeasureAllItems` — Каждый элемент измеряется по отдельности. Это значение по умолчанию.
- `MeasureFirstItem` — только первый элемент основана на числах, все последующие элементы, получает тот же размер, что первый элемент.

> [!IMPORTANT]
> `MeasureFirstItem` Изменения размера стратегии приведет к увеличению производительности при использовании в ситуациях, где размер элемента должен быть универсальный код для всех элементов.

В следующем примере кода показана установка [ `ItemSizingStrategy` ](xref:Xamarin.Forms.ItemsView.ItemSizingStrategy) свойство:

```xaml
<CollectionView ...
                ItemSizingStrategy="MeasureFirstItem">
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemSizingStrategy = ItemSizingStrategy.MeasureFirstItem
};
```

> [!NOTE]
> Элемент, изменение размера стратегии в настоящее время является только реализуется на iOS.

## <a name="dynamic-resizing-of-items"></a>Динамическое изменение размера элементов

Элементы в [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) могут динамически изменяться во время выполнения, изменив свойства элементов, связанные с макета [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Например, следующий код изменяет пример [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) и [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) свойства [ `Image` ](xref:Xamarin.Forms.Image) объекта:

```csharp
void OnImageTapped(object sender, EventArgs e)
{
    Image image = sender as Image;
    image.HeightRequest = image.WidthRequest = image.HeightRequest.Equals(60) ? 100 : 60;
}
```

`OnImageTapped` Обработчик событий выполняется в ответ на [ `Image` ](xref:Xamarin.Forms.Image) касание, а также изменяет размеры изображения, чтобы его проще просмотра:

[![Снимок экрана CollectionView с изменения размера динамического элемента, в iOS и Android](layout-images/runtime-resizing.png "изменения размера динамического элемента CollectionView")](layout-images/runtime-resizing-large.png#lightbox "изменения размера динамического элемента CollectionView")

## <a name="right-to-left-layout"></a>Макет справа налево

[`CollectionView`](xref:Xamarin.Forms.CollectionView) возможность размещать его содержимое в направлении справа налево, задав его [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) свойства [ `RightToLeft` ](xref:Xamarin.Forms.FlowDirection.RightToLeft). Тем не менее `FlowDirection` свойство в идеале задается на странице или корневой макет, который вызывает все элементы внутри страницы или корневого макета, реагировать на направление потока:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CollectionViewDemos.Views.VerticalListFlowDirectionPage"
             Title="Vertical list (RTL FlowDirection)"
             FlowDirection="RightToLeft">
    <StackLayout Margin="20">
        <CollectionView ItemsSource="{Binding Monkeys}">
            ...
        </CollectionView>
    </StackLayout>
</ContentPage>
```

Значение по умолчанию [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) для элемента с родительским [ `MatchParent` ](xref:Xamarin.Forms.FlowDirection.MatchParent). Таким образом [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) наследует `FlowDirection` значение свойства из [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), который в свою очередь наследует `FlowDirection` значение свойства из [ `ContentPage`](xref:Xamarin.Forms.ContentPage). Это приводит к макет справа налево, показано на следующем снимке экрана:

[![Снимок экрана: макет справа налево вертикальный список CollectionView, в iOS и Android](layout-images/vertical-list-rtl.png "макет справа налево вертикальный список CollectionView")](layout-images/vertical-list-rtl-large.png#lightbox "CollectionView справа налево по вертикали Макет списка")

Дополнительные сведения о направлении, см. в разделе [справа налево локализации](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Локализация справа налево](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [Прокрутка Xamarin.Forms CollectionView](scrolling.md)
