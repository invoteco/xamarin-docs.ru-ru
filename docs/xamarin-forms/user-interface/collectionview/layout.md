---
title: Задать макет CollectionView Xamarin.Forms
description: По умолчанию CollectionView будет отображения элементов в вертикальном списке. Тем не менее можно указать вертикальные и горизонтальные списки и сеток.
ms.prod: xamarin
ms.assetid: 5FE78207-1BD6-4706-91EF-B13932321FC9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/15/2019
ms.openlocfilehash: 8ed365ed41ac31c66d41f1a32a7a16929cdc6770
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61367690"
---
# <a name="specify-xamarinforms-collectionview-layout"></a>Задать макет CollectionView Xamarin.Forms

![Предварительный просмотр](~/media/shared/preview.png)

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

> [!IMPORTANT]
> `CollectionView` В настоящее время доступна в предварительной версии, а не имеет части плановой функциональных возможностей. Кроме того API может измениться, как реализация завершилась.

`CollectionView` определяет следующие свойства, которые управляют макета:

- `ItemsLayout`, типа `IItemsLayout`, определяющий макет для использования.
- `ItemSizingStrategy`, типа `ItemSizingStrategy`, указывает используемая стратегия элемента измерения.

Эти свойства поддерживаются [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) объектов, что означает, что свойства могут быть целями привязки данных.

По умолчанию `CollectionView` будет отображения элементов в вертикальном списке. Тем не менее любой из следующих типов можно использовать:

- Вертикальный список — список один столбец, по мере добавления новых элементов увеличивается по вертикали.
- Горизонтальный список — список одну строку, по мере добавления новых элементов увеличивается по горизонтали.
- Вертикальная сетка — сетка нескольких столбцов, по мере добавления новых элементов увеличивается по вертикали.
- Горизонтальная сетка — сетка нескольких строк, по мере добавления новых элементов увеличивается по горизонтали.

Эти макеты можно задать, присвоив `ItemsLayout` свойство в класс, который является производным от `ItemsLayout` класса. Этот класс определяет следующие свойства:

- `Orientation`, типа `ItemsLayoutOrientation`, указывает направление, в котором `CollectionView` увеличивается по мере добавления элементов.
- `SnapPointsAlignment`, типа `SnapPointsAlignment`, выравнивание точек прикрепления с элементами.
- `SnapPointsType`, типа `SnapPointsType`, определяет поведение точки прикрепления во время прокрутки.

Эти свойства поддерживаются [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) объектов, что означает, что свойства могут быть целями привязки данных. Дополнительные сведения о точки прикрепления, см. в разделе [привязать точки](scrolling.md#snap-points) в [прокрутки элемента в представлении](scrolling.md) руководства.

`ItemsLayoutOrientation` Перечисление определяет следующие члены:

- `Vertical` Указывает, что `CollectionView` расширяется по вертикали, так как элементы добавляются.
- `Horizontal` Указывает, что `CollectionView` расширяются горизонтально при добавлении элементов.

`ListItemsLayout` Класс наследует от `ItemsLayout` класса и определяет статические `VerticalList` и `HorizontalList` членов. Эти элементы можно создать вертикальные или горизонтальные списки, соответственно. Кроме того `ListItemsLayout` объект может быть создан, указав `ItemsLayoutOrientation` член перечисления в качестве аргумента.

`GridItemsLayout` Класс наследует от `ItemsLayout` класса и определяет `Span` свойство типа `int`, который представляет количество столбцов или строк, отображаемых в сетке. Значение по умолчанию `Span` свойства-1, и его значение всегда должно быть больше или равно 1.

> [!NOTE]
> `CollectionView` использует собственный макет ядер для выполнения макета.

## <a name="vertical-list"></a>Вертикальный список

По умолчанию `CollectionView` будет отображения элементов в вертикальном списке макета. Таким образом, нет необходимости задать `ItemsLayout` свойства, используемого для этой структуры:

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

Однако для полноты информации `CollectionView` можно задать для отображения элементов в вертикальном списке, задав его `ItemsLayout` статического `ListItemsLayout.VerticalList` член:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="{x:Static ListItemsLayout.VerticalList}">
    ...
</CollectionView>
```

Кроме того, это можно также сделать, задав `ItemsLayout` свойство в объект `ListItemsLayout` класса, указав `Vertical` `ItemsLayoutOrientation` член перечисления в качестве аргумента:

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

Ниже приведен аналогичный код C#:

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

`CollectionView` можно отобразить элементы в список по горизонтали, задав его `ItemsLayout` свойство статического `ListItemsLayout.HorizontalList` член:

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

Кроме того, это можно также сделать, задав `ItemsLayout` свойства `ListItemsLayout` объекта, указав `Horizontal` `ItemsLayoutOrientation` член перечисления в качестве аргумента:

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

Ниже приведен аналогичный код C#:

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

`CollectionView` можно отобразить элементы в вертикальную сетку, задав его `ItemsLayout` свойства `GridItemsLayout` которого `Orientation` свойству `Vertical`:

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

Ниже приведен аналогичный код C#:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(2, ItemsLayoutOrientation.Vertical)
};
```

По умолчанию вертикальный `GridItemsLayout` будет отображать элементы в одном столбце. Тем не менее, в этом примере устанавливается `GridItemsLayout.Span` значение 2. Это приводит к таблице два столбца, которая по мере добавления новых элементов увеличивается по вертикали:

[![Снимок экрана: макет CollectionView вертикальной сетки, в iOS и Android](layout-images/vertical-grid.png "CollectionView вертикальную сетку макета")](layout-images/vertical-grid-large.png#lightbox "CollectionView вертикальной сетки макета")

## <a name="horizontal-grid"></a>Горизонтальная сетка

`CollectionView` можно отобразить элементы в виде горизонтальной сетки, задав его `ItemsLayout` свойства `GridItemsLayout` которого `Orientation` свойству `Horizontal`:

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

Ниже приведен аналогичный код C#:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(4, ItemsLayoutOrientation.Horizontal)
};
```

По умолчанию горизонтального `GridItemsLayout` будут доступны элементы в одну строку. Тем не менее, в этом примере устанавливается `GridItemsLayout.Span` значение 4. Это приводит к сетке четыре строки по мере добавления новых элементов увеличивается по горизонтали:

[![Снимок экрана: макет CollectionView горизонтальной сетки, в iOS и Android](layout-images/horizontal-grid.png "CollectionView горизонтальной сетки макета")](layout-images/horizontal-grid-large.png#lightbox "CollectionView горизонтальной сетки макета")

## <a name="right-to-left-layout"></a>Макет справа налево

`CollectionView` возможность размещать его содержимое в направлении справа налево, задав его [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) свойства [ `RightToLeft` ](xref:Xamarin.Forms.FlowDirection.RightToLeft). Тем не менее `FlowDirection` свойство в идеале задается на странице или корневой макет, который вызывает все элементы внутри страницы или корневого макета, реагировать на направление потока:

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

Значение по умолчанию [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) для элемента с родительским [ `MatchParent` ](xref:Xamarin.Forms.FlowDirection.MatchParent). Таким образом `CollectionView` наследует `FlowDirection` значение свойства из [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), который в свою очередь наследует `FlowDirection` значение свойства из [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) . Это приводит к макет справа налево, показано на следующем снимке экрана:

[![Снимок экрана: макет справа налево вертикальный список CollectionView, в iOS и Android](layout-images/vertical-list-rtl.png "макет справа налево вертикальный список CollectionView")](layout-images/vertical-list-rtl-large.png#lightbox "CollectionView справа налево по вертикали Макет списка")

Дополнительные сведения о направлении, см. в разделе [справа налево локализации](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

## <a name="item-sizing"></a>Изменение размера элемента

По умолчанию каждый элемент в `CollectionView` не каждый измеряется и размера, при условии, что элементы пользовательского интерфейса в [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) не указать фиксированных размеров. Это поведение, которое может быть изменена, определяется `CollectionView.ItemSizingStrategy` значение свойства. Значение этого свойства может быть присвоено одно из `ItemSizingStrategy` членов перечисления:

- `MeasureAllItems` — Каждый элемент измеряется по отдельности. Это значение по умолчанию.
- `MeasureFirstItem` — только первый элемент основана на числах, все последующие элементы, получает тот же размер, что первый элемент.

> [!IMPORTANT]
> `MeasureFirstItem` Изменения размера стратегии следует использовать в ситуациях, когда размер элемента должен быть однородными для всех элементов и приведет к увеличению производительности.

В следующем примере кода показана установка `ItemSizingStrategy` свойство:

```xaml
<CollectionView ...
                ItemSizingStrategy="MeasureFirstItem">
    ...
</CollectionView>
```

Ниже приведен аналогичный код C#:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemSizingStrategy = ItemSizingStrategy.MeasureFirstItem
};
```

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Локализация справа налево](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [Прокрутки элемента в представлении](scrolling.md)
