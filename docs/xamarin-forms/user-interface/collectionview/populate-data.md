---
title: Данные CollectionView в Xamarin. Forms
description: CollectionView заполняется данными путем присвоения свойству ItemsSource любой коллекции, реализующей IEnumerable.
ms.prod: xamarin
ms.assetid: E1783E34-1C0F-401A-80D5-B2BE5508F5F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: ce745109ea2852b597de3a8a5922a171ad83e289
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2019
ms.locfileid: "68738920"
---
# <a name="xamarinforms-collectionview-data"></a>Данные CollectionView в Xamarin. Forms

![](~/media/shared/preview.png "Этот API в настоящее время предоставляется в режиме предварительной версии")

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)определяет следующие свойства, определяющие отображаемые данные, и его внешний вид:

- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource), типа `IEnumerable`, задает коллекцию элементов для отображения и имеет `null`значение по умолчанию.
- [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)Тип [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)— указывает шаблон, применяемый к каждому элементу в коллекции отображаемых элементов.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что свойства могут быть целевыми объектами привязок данных.

## <a name="populate-a-collectionview-with-data"></a>Заполнение CollectionView данными

Заполняется данными путем установки его [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) свойства в любую коллекцию, реализующую `IEnumerable`. [`CollectionView`](xref:Xamarin.Forms.CollectionView) Элементы можно добавлять в XAML путем инициализации `ItemsSource` свойства из массива строк:

```xaml
<CollectionView>
  <CollectionView.ItemsSource>
    <x:Array Type="{x:Type x:String}">
      <x:String>Baboon</x:String>
      <x:String>Capuchin Monkey</x:String>
      <x:String>Blue Monkey</x:String>
      <x:String>Squirrel Monkey</x:String>
      <x:String>Golden Lion Tamarin</x:String>
      <x:String>Howler Monkey</x:String>
      <x:String>Japanese Macaque</x:String>
    </x:Array>
  </CollectionView.ItemsSource>
</CollectionView>
```

> [!NOTE]
> Обратите внимание на то, что для элемента `x:Array` требуется атрибут `Type`, указывающий тип элементов в массиве.

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView();
collectionView.ItemsSource = new string[]
{
    "Baboon",
    "Capuchin Monkey",
    "Blue Monkey",
    "Squirrel Monkey",
    "Golden Lion Tamarin",
    "Howler Monkey",
    "Japanese Macaque"
};
```

> [!IMPORTANT]
> Если требуется обновление при добавлении, удалении или изменении элементов в базовой коллекции, то базовая коллекция должна `IEnumerable` быть коллекцией, которая отправляет уведомления об `ObservableCollection`изменении свойств, например. [`CollectionView`](xref:Xamarin.Forms.CollectionView)

По умолчанию [`CollectionView`](xref:Xamarin.Forms.CollectionView) отображает элементы в вертикальном списке, как показано на следующих снимках экрана:

[ ![Снимок экрана CollectionView, содержащий текстовые элементы, в текстовых элементах iOS и Android](populate-data-images/text.png "в CollectionView") ] (populate-data-images/text-large.png#lightbox "Текстовые элементы в CollectionView")

Сведения о том, как изменить [`CollectionView`](xref:Xamarin.Forms.CollectionView) макет, см. в разделе [Определение макета](layout.md). Сведения о том `CollectionView`, как определить внешний вид каждого элемента в, см. в разделе [Определение внешнего вида элемента](#define-item-appearance).

### <a name="data-binding"></a>Привязка данных

[`CollectionView`](xref:Xamarin.Forms.CollectionView)может заполняться данными с помощью привязки данных для привязки своего [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) свойства `IEnumerable` к коллекции. В XAML это достигается с помощью `Binding` расширения разметки:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

В этом примере [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) данные свойства привязываются `Monkeys` к свойству подключенной модели представления.

> [!NOTE]
> Скомпилированные привязки можно включить для повышения производительности привязки данных в приложениях Xamarin. Forms. Дополнительные сведения см. в разделе [скомпилированные привязки](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md).

Дополнительные сведения о привязке данных см. в разделе [Привязки данных в Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md).

## <a name="define-item-appearance"></a>Определение внешнего вида элемента

Внешний вид каждого элемента в [`CollectionView`](xref:Xamarin.Forms.CollectionView) может быть определен путем [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) присвоения свойству [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)значения.

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
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

collectionView.ItemTemplate = new DataTemplate(() =>
{
    Grid grid = new Grid { Padding = 10 };
    grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
    grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
    grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Auto });
    grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Auto });

    Image image = new Image { Aspect = Aspect.AspectFill, HeightRequest = 60, WidthRequest = 60 };
    image.SetBinding(Image.SourceProperty, "ImageUrl");

    Label nameLabel = new Label { FontAttributes = FontAttributes.Bold };
    nameLabel.SetBinding(Label.TextProperty, "Name");

    Label locationLabel = new Label { FontAttributes = FontAttributes.Italic, VerticalOptions = LayoutOptions.End };
    locationLabel.SetBinding(Label.TextProperty, "Location");

    Grid.SetRowSpan(image, 2);

    grid.Children.Add(image);
    grid.Children.Add(nameLabel, 1, 0);
    grid.Children.Add(locationLabel, 1, 1);

    return grid;
});
```

Элементы, указанные в параметре, [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) определяют внешний вид каждого элемента в списке. В этом примере макет `DataTemplate` в управляется [`Grid`](xref:Xamarin.Forms.Grid)с помощью. Содержит объект и [два`Label`](xref:Xamarin.Forms.Label) объекта`Monkey` , которые привязываются к свойствам класса: [`Image`](xref:Xamarin.Forms.Image) `Grid`

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

На следующих снимках экрана показан результат создания шаблонов каждого элемента в списке:

[ ![Снимок экрана CollectionView, где размещается каждый элемент, в шаблонных элементах iOS и Android](populate-data-images/datatemplate.png "в CollectionView") ] (populate-data-images/datatemplate-large.png#lightbox "Элементы шаблона в CollectionView")

Дополнительные сведения о шаблонах данных см. в разделе [Шаблоны данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="choose-item-appearance-at-runtime"></a>Выбор внешнего вида элемента во время выполнения

Внешний вид каждого элемента в [`CollectionView`](xref:Xamarin.Forms.CollectionView) можно выбрать во время выполнения на основе значения элемента, [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) задав для [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) свойства объект.

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CollectionViewDemos.Controls">
    <ContentPage.Resources>
        <DataTemplate x:Key="AmericanMonkeyTemplate">
            ...
        </DataTemplate>

        <DataTemplate x:Key="OtherMonkeyTemplate">
            ...
        </DataTemplate>

        <controls:MonkeyDataTemplateSelector x:Key="MonkeySelector"
                                             AmericanMonkey="{StaticResource AmericanMonkeyTemplate}"
                                             OtherMonkey="{StaticResource OtherMonkeyTemplate}" />
    </ContentPage.Resources>

    <CollectionView ItemsSource="{Binding Monkeys}"
                    ItemTemplate="{StaticResource MonkeySelector}" />
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemTemplate = new MonkeyDataTemplateSelector { ... }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Для свойства задается объект. `MonkeyDataTemplateSelector` [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) В следующем примере показан `MonkeyDataTemplateSelector` класс:

```csharp
public class MonkeyDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate AmericanMonkey { get; set; }
    public DataTemplate OtherMonkey { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return ((Monkey)item).Location.Contains("America") ? AmericanMonkey : OtherMonkey;
    }
}
```

Класс определяет `AmericanMonkey` свойства и`OtherMonkey` , [длякоторыхустановленыразныешаблоныданных.`DataTemplate`](xref:Xamarin.Forms.DataTemplate) `MonkeyDataTemplateSelector` Переопределение`AmericanMonkey` Возвращает шаблон, который отображает имя и расположение обезьяны в синем, если имя обезьяны содержит "America". `OnSelectTemplate` Если имя обезьяны не содержит "America", то `OnSelectTemplate` переопределение `OtherMonkey` Возвращает шаблон, который отображает имя и расположение обезьяны в серебристом виде:

[ ![Снимок экрана: Выбор шаблона элемента среды выполнения CollectionView,](populate-data-images/datatemplateselector.png "Выбор шаблона элемента среды выполнения") iOS и Android в CollectionView] (populate-data-images/datatemplateselector-large.png#lightbox "Выбор шаблона элемента среды выполнения в CollectionView")

Дополнительные сведения о селекторах шаблонов данных см. [в разделе Создание DataTemplateSelector Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

> [!IMPORTANT]
> При использовании [`CollectionView`](xref:Xamarin.Forms.CollectionView)никогда не устанавливайте для `ViewCell`корневого элемента [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) объектов значение. Это приведет к возникновению исключения, так как `CollectionView` не содержит концепцию ячеек.

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Привязка данных Xamarin. Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Шаблоны данных Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Создание DataTemplateSelector Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
