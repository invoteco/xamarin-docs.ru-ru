---
title: Xamarin.Forms CollectionView Data
description: CollectionView заполняется данными, установив его свойство ItemsSource к любой коллекции, реализующий интерфейс IEnumerable.
ms.prod: xamarin
ms.assetid: E1783E34-1C0F-401A-80D5-B2BE5508F5F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: d2729250c0f991564ae70ddf6a15b40425ed6c46
ms.sourcegitcommit: 0596004d4a0e599c1da1ddd75a6ac928f21191c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/22/2019
ms.locfileid: "66005269"
---
# <a name="xamarinforms-collectionview-data"></a>Xamarin.Forms CollectionView Data

![](~/media/shared/preview.png "Этот API в настоящее время предоставляется в режиме предварительной версии")

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) определяет следующие свойства, которые определяют данные для отображения и его внешний вид:

- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource), типа `IEnumerable`, указывает коллекцию элементов для отображения, и имеет значение по умолчанию `null`.
- [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate), типа [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), определяет шаблон, применяемый к каждому элементу в коллекции элементов для отображения.

Эти свойства поддерживаются [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) объектов, что означает, что свойства могут быть целями привязки данных.

## <a name="populate-a-collectionview-with-data"></a>Заполнение CollectionView с данными

Объект [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) заполняется данными, задав его [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView.ItemsSource) свойство к любой коллекции, реализующий `IEnumerable`. Элементы могут быть добавлены в XAML путем инициализации `ItemsSource` свойства из массива строк:

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
> Если [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) — требуется обновить, как элементы будут добавлены, удалены или изменены в базовой коллекции, должен быть базовой коллекции `IEnumerable` коллекции, который отправляет свойства уведомления об изменении, такие как `ObservableCollection`.

По умолчанию [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) элементы отображаются в вертикальном списке, как показано на следующем снимке экрана:

[![Снимок экрана из CollectionView содержащий текстовые элементы, в iOS и Android](populate-data-images/text.png "текстовых элементов в CollectionView")](populate-data-images/text-large.png#lightbox "текстовых элементов в CollectionView")

Сведения о том, как изменить [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) макета, см. в разделе [указывается макет](layout.md). Сведения о том, как определить внешний вид каждого элемента в `CollectionView`, см. в разделе [определения внешнего вида элемента](#define-item-appearance).

### <a name="data-binding"></a>Привязка данных

[`CollectionView`](xref:Xamarin.Forms.CollectionView) с помощью привязки данных для привязки может заполняться данными его [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView.ItemsSource) свойства `IEnumerable` коллекции. В XAML, это достигается за счет `Binding` расширение разметки:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

В этом примере [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView.ItemsSource) свойство данных привязывает к `Monkeys` связанное представление модели.

> [!NOTE]
> Скомпилированный привязки можно включить для повышения производительности привязки данных в приложениях Xamarin.Forms. Дополнительные сведения см. в разделе [компиляции привязки](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md).

Дополнительные сведения о привязке данных см. в разделе [Привязки данных в Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md).

## <a name="define-item-appearance"></a>Определить внешний вид элемента

Внешний вид каждого элемента в [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) может быть определена путем задания [ `CollectionView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView.ItemTemplate) свойства [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate):

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

Элементы, указанные в [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) определяют внешний вид каждого элемента в списке. В примере, компоновка внутри `DataTemplate` управляется [ `Grid` ](xref:Xamarin.Forms.Grid). `Grid` Содержит [ `Image` ](xref:Xamarin.Forms.Image) объекта, а также два [ `Label` ](xref:Xamarin.Forms.Label) объектов, что все привязки к свойствам объекта `Monkey` класса:

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

На следующих снимках экрана показано результат шаблонов каждого элемента в списке:

[![Снимок экрана из CollectionView где каждый элемент является шаблоном, в iOS и Android](populate-data-images/datatemplate.png "шаблонных элементов в CollectionView")](populate-data-images/datatemplate-large.png#lightbox "шаблонных элементов в CollectionView")

Дополнительные сведения о шаблонах данных см. в разделе [Шаблоны данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="choose-item-appearance-at-runtime"></a>Выберите внешний вид элемента во время выполнения

Внешний вид каждого элемента в [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) можно выбрать во время выполнения в зависимости значения элемента, задав [ `CollectionView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView.ItemTemplate) свойства [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector)объекта:

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

[ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView.ItemTemplate) Свойству `MonkeyDataTemplateSelector` объекта. В следующем примере показан `MonkeyDataTemplateSelector` класса:

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

`MonkeyDataTemplateSelector` Класс определяет `AmericanMonkey` и `OtherMonkey` [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) свойства, заданные для различные шаблоны данных. `OnSelectTemplate` Переопределить возвращает `AmericanMonkey` шаблон, который отображает monkey имя и расположение в сине-зеленый, если имя monkey содержит «Америка». Когда monkey не может содержать «Америка», `OnSelectTemplate` переопределить возвращает `OtherMonkey` шаблон, который отображает monkey имя и расположение в silver:

[![Снимок экрана из CollectionView среды выполнения элемента выбора шаблона, в iOS и Android](populate-data-images/datatemplateselector.png "Выбор шаблона элементов среды выполнения в CollectionView")](populate-data-images/datatemplateselector-large.png#lightbox "Выбор шаблона элементов среды выполнения в CollectionView")

Дополнительные сведения о селекторах шаблон данных, см. в разделе [создать Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)
- [Привязка данных в Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Шаблоны Xamarin.Forms данных](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Создание Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
