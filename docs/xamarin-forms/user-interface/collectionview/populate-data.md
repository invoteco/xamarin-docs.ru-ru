---
title: Заполнение Xamarin.Forms CollectionView с данными
description: CollectionView заполняется данными, установив его свойство ItemsSource к любой коллекции, реализующий интерфейс IEnumerable.
ms.prod: xamarin
ms.assetid: E1783E34-1C0F-401A-80D5-B2BE5508F5F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/15/2019
ms.openlocfilehash: 57012202d981b96dba42f3017a19f2e32e4982ec
ms.sourcegitcommit: a7170494e1975f0f1be547a45444752fd8e57819
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2019
ms.locfileid: "58507192"
---
# <a name="populate-xamarinforms-collectionview-with-data"></a>Заполнение Xamarin.Forms CollectionView с данными

![Предварительный просмотр](~/media/shared/preview.png)

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

> [!IMPORTANT]
> `CollectionView` В настоящее время доступна в предварительной версии, а не имеет части плановой функциональных возможностей. Кроме того API может измениться, как реализация завершилась.

`CollectionView` определяет следующие свойства, которые определяют данные для отображения и его внешний вид:

- `ItemsSource`, типа `IEnumerable`, указывает коллекцию элементов для отображения, и имеет значение по умолчанию `null`.
- `ItemTemplate`, типа [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), определяет шаблон, применяемый к каждому элементу в коллекции элементов для отображения.

Эти свойства поддерживаются [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) объектов, что означает, что свойства могут быть целями привязки данных.

## <a name="populate-a-collectionview-with-data"></a>Заполнение CollectionView с данными

Объект `CollectionView` заполняется данными, задав его `ItemsSource` свойство к любой коллекции, реализующий `IEnumerable`. Элементы могут быть добавлены в XAML путем инициализации `ItemsSource` свойства из массива строк:

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

Ниже приведен аналогичный код C#:

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
> Если `CollectionView` — требуется обновить, как элементы будут добавлены, удалены или изменены в базовой коллекции, должен быть базовой коллекции `IEnumerable` коллекции, который отправляет свойства уведомления об изменении, например `ObservableCollection`.

По умолчанию `CollectionView` элементы отображаются в вертикальном списке, как показано на следующем снимке экрана:

[![Снимок экрана из CollectionView содержащий текстовые элементы, в iOS и Android](populate-data-images/text.png "текстовых элементов в CollectionView")](populate-data-images/text-large.png#lightbox "текстовых элементов в CollectionView")

Сведения о том, как изменить `CollectionView` макета, см. в разделе [указывается макет](layout.md). Сведения о том, как определить внешний вид каждого элемента в `CollectionView`, см. в разделе [определения внешнего вида элемента](#define-item-appearance).

### <a name="data-binding"></a>Привязка данных

`CollectionView` с помощью привязки данных для привязки может заполняться данными его `ItemsSource` свойства `IEnumerable` коллекции. В XAML, это достигается за счет `Binding` расширение разметки:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

Ниже приведен аналогичный код C#:

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

В этом примере `ItemsSource` свойство данных привязывает к `Monkeys` связанное представление модели.

> [!NOTE]
> Скомпилированный привязки можно включить для повышения производительности привязки данных в приложениях Xamarin.Forms. Дополнительные сведения см. в разделе [компиляции привязки](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md).

Дополнительные сведения о привязке данных см. в разделе [Привязки данных в Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md).

## <a name="define-item-appearance"></a>Определить внешний вид элемента

Внешний вид каждого элемента в `CollectionView` может быть определена путем задания `CollectionView.ItemTemplate` свойства [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate):

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

Ниже приведен аналогичный код C#:

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

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Привязка данных в Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Шаблоны Xamarin.Forms данных](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
