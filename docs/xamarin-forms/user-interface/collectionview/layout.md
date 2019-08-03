---
title: Макет CollectionView Xamarin. Forms
description: По умолчанию элементы CollectionView будут отображаться в вертикальном списке. Однако можно указать вертикальные и горизонтальные списки и сетки.
ms.prod: xamarin
ms.assetid: 5FE78207-1BD6-4706-91EF-B13932321FC9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2019
ms.openlocfilehash: 5fb92882f443007e5b3dd693f54e582757db1905
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739026"
---
# <a name="xamarinforms-collectionview-layout"></a>Макет CollectionView Xamarin. Forms

![](~/media/shared/preview.png "Этот API в настоящее время предоставляется в режиме предварительной версии")

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)определяет следующие свойства, управляющие макетом:

- [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)Тип [`IItemsLayout`](xref:Xamarin.Forms.IItemsLayout)— указывает макет для использования.
- [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemsView.ItemSizingStrategy)Тип [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemSizingStrategy)— определяет применяемую стратегию мер элементов.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что свойства могут быть целевыми объектами привязок данных.

По умолчанию [`CollectionView`](xref:Xamarin.Forms.CollectionView) элементы отображаются в вертикальном списке. Однако можно использовать любой из следующих макетов:

- Вертикальный список — список из одного столбца, который увеличивается по вертикали при добавлении новых элементов.
- Горизонтальный список — один список строк, который увеличивается горизонтально по мере добавления новых элементов.
- Вертикальная сетка — сетка с несколькими столбцами, которая растет вертикально по мере добавления новых элементов.
- Горизонтальная сетка — многострочная сетка, которая растет горизонтально по мере добавления новых элементов.

Эти макеты можно указать, задав [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) для свойства класс, производный [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) от класса. Этот класс определяет следующие свойства:

- [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation)Тип [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)задает направление, в [`CollectionView`](xref:Xamarin.Forms.CollectionView) котором добавляются элементы расширения по мере добавления.
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment), типа [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment), задает способ выравнивания точек привязки по элементам.
- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)Тип [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)— задает поведение точек привязки при прокрутке.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что свойства могут быть целевыми объектами привязок данных. Дополнительные сведения о точках привязки см. в разделе [точки привязки](scrolling.md#snap-points) в [CollectionViewе прокрутки Xamarin. Forms](scrolling.md) .

[`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) Перечисление определяет следующие члены:

- `Vertical`Указывает, что [`CollectionView`](xref:Xamarin.Forms.CollectionView) будет расширяться по вертикали по мере добавления элементов.
- `Horizontal`Указывает, что [`CollectionView`](xref:Xamarin.Forms.CollectionView) будет разворачиваться горизонтально по мере добавления элементов.

Класс наследует [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) `double` от`ItemSpacing` класса и определяет свойство типа, представляющее пустое пространство вокруг каждого элемента. [`ListItemsLayout`](xref:Xamarin.Forms.ListItemsLayout) Значение этого свойства по умолчанию равно 0, а его значение всегда должно быть больше или равно 0. Класс также определяет статические `Vertical` члены `Horizontal`и. `ListItemsLayout` Эти члены можно использовать для создания вертикальных или горизонтальных списков соответственно. Кроме того, `ListItemsLayout` можно создать объект, [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) указав в качестве аргумента член перечисления.

Класс наследует от класса и определяет следующие свойства: [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout)

- `VerticalItemSpacing`Тип `double`, представляющий вертикальное пустое пространство вокруг каждого элемента. Значение этого свойства по умолчанию равно 0, а его значение всегда должно быть больше или равно 0.
- `HorizontalItemSpacing`Тип `double`, представляющий горизонтальное пустое пространство вокруг каждого элемента. Значение этого свойства по умолчанию равно 0, а его значение всегда должно быть больше или равно 0.
- `Span`Тип `int`, который представляет число столбцов или строк, отображаемых в сетке. Значение этого свойства по умолчанию равно 1, а его значение всегда должно быть больше или равно 1.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что свойства могут быть целевыми объектами привязок данных.

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)использует собственные обработчики макетов для выполнения макета.

## <a name="vertical-list"></a>Вертикальный список

По умолчанию [`CollectionView`](xref:Xamarin.Forms.CollectionView) отображает свои элементы в вертикальном макете списка. Поэтому нет необходимости задавать [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) свойство для использования этого макета:

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

Однако для полноты [`CollectionView`](xref:Xamarin.Forms.CollectionView) можно задать отображение элементов в вертикальном списке, задав для `VerticalList`его [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) свойства значение:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="VerticalList">
    ...
</CollectionView>
```

Кроме того, это [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) можно сделать, задав для свойства объект [`ListItemsLayout`](xref:Xamarin.Forms.ListItemsLayout) класса, указав `Vertical` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) член перечисления в качестве аргумента:

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
    ItemsLayout = ListItemsLayout.Vertical
};
```

В результате получается список из одного столбца, который по вертикали увеличивается по мере добавления новых элементов:

[ ![Снимок экрана с вертикальным макетом списка CollectionView на вертикальном списке в iOS и Android](layout-images/vertical-list.png "CollectionView") ] (layout-images/vertical-list-large.png#lightbox "Вертикальный макет списка CollectionView")

## <a name="horizontal-list"></a>Горизонтальный список

[`CollectionView`](xref:Xamarin.Forms.CollectionView)может отображать свои элементы в горизонтальном списке, присвоив [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) `HorizontalList`свойству значение:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="HorizontalList">
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

Кроме того, это [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) можно сделать, задав [`ListItemsLayout`](xref:Xamarin.Forms.ListItemsLayout) для свойства объект, указав `Horizontal` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) член перечисления в качестве аргумента:

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
    ItemsLayout = ListItemsLayout.Horizontal
};
```

В результате получается один список строк, который увеличивается горизонтально по мере добавления новых элементов:

[ ![Снимок экрана с горизонтальным макетом списка CollectionView на горизонтальном рисунке списка в iOS и Android](layout-images/horizontal-list.png "CollectionView") ] (layout-images/horizontal-list-large.png#lightbox "CollectionView горизонтальный макет списка")

## <a name="vertical-grid"></a>Вертикальная сетка

[`CollectionView`](xref:Xamarin.Forms.CollectionView)может отображать [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) свои элементы в вертикальной сетке, присвоив его свойству [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) объект [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation) `Vertical`, свойство которого имеет значение:

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

По умолчанию элементы будут [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) отображаться в одном столбце по вертикали. Однако в этом примере `GridItemsLayout.Span` свойству присваивается значение 2. В результате получается сетка из двух столбцов, которая увеличивается по вертикали при добавлении новых элементов:

[ ![Снимок экрана: вертикальный макет сетки CollectionView, в iOS и Android](layout-images/vertical-grid.png "CollectionView вертикальный макет сетки") ] (layout-images/vertical-grid-large.png#lightbox "Макет вертикальной сетки CollectionView")

## <a name="horizontal-grid"></a>Горизонтальная сетка

[`CollectionView`](xref:Xamarin.Forms.CollectionView)может отображать свои элементы в горизонтальной сетке, [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) присвоив свойству [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) значение объекта[`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation) `Horizontal`, свойство которого имеет значение:

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

По умолчанию элементы по [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) горизонтали будут отображаться в одной строке. Однако в `GridItemsLayout.Span` этом примере свойству присваивается значение 4. В результате получается сетка из четырех строк, которая увеличивается горизонтально по мере добавления новых элементов:

[ ![Снимок экрана с горизонтальным макетом сетки CollectionView на](layout-images/horizontal-grid.png "горизонтальном макете сетки") в iOS и Android CollectionView] (layout-images/horizontal-grid-large.png#lightbox "Горизонтальный макет сетки CollectionView")

## <a name="item-spacing"></a>Промежуток между элементами

По умолчанию каждый элемент в [`CollectionView`](xref:Xamarin.Forms.CollectionView) не имеет пустого пространства вокруг него. Это поведение можно изменить, задав свойства в макете элементов, который `CollectionView`используется.

[`ListItemsLayout`](xref:Xamarin.Forms.ListItemsLayout) `ListItemsLayout.ItemSpacing` Когда свойство присваивает свойству объект`double` , свойству может быть присвоено значение, представляющее пустое пространство вокруг каждого элемента: [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout)

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout ItemSpacing="20">
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>    
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

> [!NOTE]
> `ListItemsLayout.ItemSpacing` Свойство имеет набор обратного вызова проверки, который гарантирует, что значение свойства всегда больше или равно 0.

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new ListItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        ItemSpacing = 20
    }
};
```

Этот код приводит к постановке в вертикальный список с одним столбцом с отступом 20 вокруг каждого элемента:

[ ![Снимок экрана CollectionView с промежутком между элементами на устройствах iOS и Android](layout-images/vertical-list-spacing.png "CollectionView") ] (layout-images/vertical-list-spacing-large.png#lightbox "CollectionView между элементами")

[`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) `GridItemsLayout.VerticalItemSpacing` `GridItemsLayout.HorizontalItemSpacing` `double` Когда свойство [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) присваивает свойству объект, свойства и могут быть установлены на значения, представляющие пустое пространство по вертикали и по горизонтали вокруг каждого элемента:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Vertical"
                        Span="2"
                        VerticalItemSpacing="20"
                        HorizontalItemSpacing="30" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

> [!NOTE]
> Свойства `GridItemsLayout.VerticalItemSpacing` и`GridItemsLayout.HorizontalItemSpacing` имеют обратные вызовы проверки, которые гарантируют, что значения свойств всегда больше или равны 0.

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(2, ItemsLayoutOrientation.Vertical)
    {
        VerticalItemSpacing = 20,
        HorizontalItemSpacing = 30
    }
};
```

Этот код приводит к вертикальной сетке с двумя столбцами с вертикальным расстоянием от 20 вокруг каждого элемента, а расстояние по горизонтали — 30 вокруг каждого элемента:

[ ![Снимок экрана CollectionView с промежутком между элементами на устройствах iOS и Android](layout-images/vertical-grid-spacing.png "CollectionView") ] (layout-images/vertical-grid-spacing-large.png#lightbox "CollectionView между элементами")

## <a name="item-sizing"></a>Размер элемента

По умолчанию каждый элемент в [`CollectionView`](xref:Xamarin.Forms.CollectionView) принимает отдельное измерение и размер, при условии, что элементы пользовательского интерфейса [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) в не задают фиксированные размеры. Это поведение, которое можно изменить, задается [`CollectionView.ItemSizingStrategy`](xref:Xamarin.Forms.ItemsView.ItemSizingStrategy) значением свойства. Это значение свойства может быть задано одним из [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemSizingStrategy) членов перечисления:

- `MeasureAllItems`— Каждый элемент по отдельности измеряется. Это значение по умолчанию.
- `MeasureFirstItem`— измеряется только первый элемент, и все последующие элементы получают тот же размер, что и первый элемент.

> [!IMPORTANT]
> Стратегия `MeasureFirstItem` изменения размера приводит к повышению производительности при использовании в ситуациях, когда размер элемента должен быть одинаковым для всех элементов.

В следующем примере кода показано, как [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemsView.ItemSizingStrategy) задать свойство:

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

## <a name="dynamic-resizing-of-items"></a>Динамическое изменение размера элементов

В среде выполнения [`CollectionView`](xref:Xamarin.Forms.CollectionView) элементы в можно динамически изменять размер, изменяя связанные с макетом свойства элементов [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)в. Например, в следующем примере кода изменяются [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) свойства [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) [`Image`](xref:Xamarin.Forms.Image) и объекта:

```csharp
void OnImageTapped(object sender, EventArgs e)
{
    Image image = sender as Image;
    image.HeightRequest = image.WidthRequest = image.HeightRequest.Equals(60) ? 100 : 60;
}
```

Обработчик событий выполняется в ответ [`Image`](xref:Xamarin.Forms.Image) на касание объекта и изменяет размеры изображения, чтобы его было проще Просмотреть: `OnImageTapped`

[ ![Снимок экрана CollectionView с динамическим изменением размера элемента в iOS и Android](layout-images/runtime-resizing.png "CollectionView изменение размера динамического элемента") ] (layout-images/runtime-resizing-large.png#lightbox "Изменение размера динамического элемента CollectionView")

## <a name="right-to-left-layout"></a>Макет с письмом справа налево

[`CollectionView`](xref:Xamarin.Forms.CollectionView)может Разметка содержимого в направлении справа налево путем присвоения [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)свойству значения. Однако в идеале это `FlowDirection` свойство должно быть установлено на странице или в корневом макете, что приводит к тому, что все элементы на странице или в корневом макете реагируют на направление потока:

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

По умолчанию [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) для элемента с родительским элементом [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)имеет значение. `FlowDirection` `FlowDirection` [`StackLayout`](xref:Xamarin.Forms.StackLayout)Таким образом, класс [`ContentPage`](xref:Xamarin.Forms.ContentPage)наследует значение свойства от, которое, в свою очередь, наследует значение свойства от. [`CollectionView`](xref:Xamarin.Forms.CollectionView) В результате отображается макет справа налево, показанный на следующих снимках экрана:

[ ![Снимок экрана с вертикальным макетом списка справа налево на CollectionView в iOS и Android](layout-images/vertical-list-rtl.png "CollectionView вертикальный макет списка справа") ] налево (layout-images/vertical-list-rtl-large.png#lightbox "CollectionView вертикальный макет списка справа налево")

Дополнительные сведения о направлении потока см. [в разделе локализация справа налево](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Локализация справа налево](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [Прокрутка Xamarin. Forms CollectionView](scrolling.md)
