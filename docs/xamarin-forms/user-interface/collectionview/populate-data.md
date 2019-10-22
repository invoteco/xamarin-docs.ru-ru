---
title: Данные CollectionView в Xamarin. Forms
description: CollectionView заполняется данными путем присвоения свойству ItemsSource любой коллекции, реализующей IEnumerable.
ms.prod: xamarin
ms.assetid: E1783E34-1C0F-401A-80D5-B2BE5508F5F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2019
ms.openlocfilehash: 5afdaa9afa4c5ced39498a1cb45de07fe4bf4195
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696710"
---
# <a name="xamarinforms-collectionview-data"></a>Данные CollectionView в Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) содержит следующие свойства, которые определяют отображаемые данные и его внешний вид:

- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource), типа `IEnumerable`, указывает коллекцию отображаемых элементов и значение по умолчанию `null`.
- [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)типа [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)указывает шаблон, применяемый к каждому элементу в коллекции отображаемых элементов.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами. Это означает, что свойства могут быть целевыми объектами привязок данных.

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) определяет свойство `ItemsUpdatingScrollMode`, представляющее поведение прокрутки `CollectionView` при добавлении к нему новых элементов. Дополнительные сведения об этом свойстве см. в разделе [управление позицией прокрутки при добавлении новых элементов](scrolling.md#control-scroll-position-when-new-items-are-added).

[`CollectionView`](xref:Xamarin.Forms.CollectionView) также может загружать данные постепенно по мере прокрутки пользователем. Дополнительные сведения см. в статье [добавочная загрузка данных](#load-data-incrementally).

## <a name="populate-a-collectionview-with-data"></a>Заполнение CollectionView данными

[@No__t_1](xref:Xamarin.Forms.CollectionView) заполняется данными путем присвоения свойству [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) любой коллекции, реализующей `IEnumerable`. Элементы можно добавить в XAML, инициализируя свойство `ItemsSource` из массива строк:

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
> Если [`CollectionView`](xref:Xamarin.Forms.CollectionView) требуется обновлять при добавлении, удалении или изменении элементов в базовой коллекции, базовая коллекция должна быть коллекцией `IEnumerable`, которая отправляет уведомления об изменении свойств, например `ObservableCollection`.

По умолчанию [`CollectionView`](xref:Xamarin.Forms.CollectionView) отображает элементы в вертикальном списке, как показано на следующих снимках экрана:

[![Снимок экрана CollectionView, содержащий текстовые элементы, в iOS и Android](populate-data-images/text.png "Текстовые элементы в CollectionView")](populate-data-images/text-large.png#lightbox "Текстовые элементы в CollectionView")

Сведения о том, как изменить макет [`CollectionView`](xref:Xamarin.Forms.CollectionView) , см. в разделе [CollectionViewing The Xamarin. Forms](layout.md). Сведения о том, как определить внешний вид каждого элемента в `CollectionView`, см. в разделе [Определение внешнего вида элемента](#define-item-appearance).

### <a name="data-binding"></a>привязка данных,

[`CollectionView`](xref:Xamarin.Forms.CollectionView) можно заполнить данными с помощью привязки данных, чтобы привязать его свойство [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) к коллекции `IEnumerable`. В XAML это достигается с помощью расширения разметки `Binding`:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

В этом примере данные свойства [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) привязываются к свойству `Monkeys` подключенного ViewModel.

> [!NOTE]
> Скомпилированные привязки можно включить для повышения производительности привязки данных в приложениях Xamarin. Forms. Дополнительные сведения см. в статье [Скомпилированные привязки](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md).

Дополнительные сведения о привязке данных см. в разделе [Привязки данных в Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md).

## <a name="define-item-appearance"></a>Определение внешнего вида элемента

Внешний вид каждого элемента в [`CollectionView`](xref:Xamarin.Forms.CollectionView) можно определить, присвоив свойству [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate).

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

Элементы, указанные в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , определяют внешний вид каждого элемента в списке. В этом примере макет в `DataTemplate` управляется [`Grid`](xref:Xamarin.Forms.Grid). @No__t_0 содержит объект [`Image`](xref:Xamarin.Forms.Image) и два [`Label`](xref:Xamarin.Forms.Label) объектов, которые привязываются к свойствам класса `Monkey`:

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

[![Снимок экрана CollectionView, где для каждого элемента используется шаблон, в iOS и Android](populate-data-images/datatemplate.png "Элементы шаблона в CollectionView")](populate-data-images/datatemplate-large.png#lightbox "Элементы шаблона в CollectionView")

Дополнительные сведения о шаблонах данных см. в разделе [Шаблоны данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="choose-item-appearance-at-runtime"></a>Выбор внешнего вида элемента во время выполнения

Внешний вид каждого элемента в [`CollectionView`](xref:Xamarin.Forms.CollectionView) можно выбрать во время выполнения на основе значения элемента, установив для свойства [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) значение [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) объект:

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

Для свойства [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) задан объект `MonkeyDataTemplateSelector`. В следующем примере показан класс `MonkeyDataTemplateSelector`:

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

Класс `MonkeyDataTemplateSelector` определяет `AmericanMonkey` и `OtherMonkey` свойства [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , для которых заданы разные шаблоны данных. Переопределение `OnSelectTemplate` Возвращает шаблон `AmericanMonkey`, который отображает имя и расположение обезьяны в синем, если имя обезьяны содержит "America". Если имя обезьяны не содержит "America", то переопределение `OnSelectTemplate` Возвращает шаблон `OtherMonkey`, который отображает имя и расположение обезьяны в серебристом виде:

[![Снимок экрана выбора шаблона элемента среды выполнения CollectionView в iOS и Android](populate-data-images/datatemplateselector.png "Выбор шаблона элемента среды выполнения в CollectionView")](populate-data-images/datatemplateselector-large.png#lightbox "Выбор шаблона элемента среды выполнения в CollectionView")

Дополнительные сведения о селекторах шаблонов данных см. [в разделе Создание DataTemplateSelector Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

> [!IMPORTANT]
> При использовании [`CollectionView`](xref:Xamarin.Forms.CollectionView)никогда не устанавливайте в качестве корневого элемента [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) объектов `ViewCell`. Это приведет к возникновению исключения, поскольку `CollectionView` не имеет концепции ячеек.

## <a name="pull-to-refresh"></a>Извлечь для обновления

[`CollectionView`](xref:Xamarin.Forms.CollectionView) поддерживает функцию Pull для обновления с помощью `RefreshView`, что позволяет обновлять отображаемые данные путем вывода списка элементов. @No__t_0 — это контейнерный элемент управления, предоставляющий функции обновления для своего дочернего элемента, при условии, что дочерний объект поддерживает прокручиваемое содержимое. Таким образом, запрос на обновление реализуется для `CollectionView`, настроив его как дочерний элемент `RefreshView`.

```xaml
<RefreshView IsRefreshing="{Binding IsRefreshing}"
             Command="{Binding RefreshCommand}">
    <CollectionView ItemsSource="{Binding Animals}">
        ...
    </CollectionView>
</RefreshView>
```

Эквивалентный код на C# выглядит так:

```csharp
RefreshView refreshView = new RefreshView();
ICommand refreshCommand = new Command(() =>
{
    // IsRefreshing is true
    // Refresh data here
    refreshView.IsRefreshing = false;
});
refreshView.Command = refreshCommand;

CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
refreshView.Content = collectionView;
// ...
```

Когда пользователь инициирует обновление, выполняется `ICommand`, определяемое свойством `Command`, которое должно обновлять отображаемые элементы. Во время обновления отображается визуализация обновления, которая состоит из анимированного круга хода выполнения:

[![Снимок экрана CollectionView Pull-to-Refresh, в iOS и Android](populate-data-images/pull-to-refresh.png "CollectionView по запросу на обновление")](populate-data-images/pull-to-refresh-large.png#lightbox "CollectionView по запросу на обновление")

Значение свойства `RefreshView.IsRefreshing` указывает текущее состояние `RefreshView`. При активации обновления пользователем это свойство автоматически переходит в `true`. После завершения обновления следует сбросить свойство на `false`.

Дополнительные сведения о `RefreshView` см. в разделе [Xamarin. Forms рефрешвиев](~/xamarin-forms/user-interface/refreshview.md).

## <a name="load-data-incrementally"></a>Добавочная загрузка данных

[`CollectionView`](xref:Xamarin.Forms.CollectionView) поддерживает добавочную загрузку данных по мере прокрутки элементов пользователями. Это позволяет выполнять такие сценарии, как асинхронная загрузка страницы данных из веб-службы при прокрутке пользователем. Кроме того, точка, в которой загружаются дополнительные данные, настраивается таким образом, что пользователи не видят пустое пространство или останавливаются при прокрутке.

[`CollectionView`](xref:Xamarin.Forms.CollectionView) определяет следующие свойства для управления добавочной загрузкой данных:

- `RemainingItemsThreshold` типа `int` пороговое значение элементов, которые еще не отображаются в списке, в котором будет запущено событие `RemainingItemsThresholdReached`.
- `RemainingItemsThresholdReachedCommand`, типа `ICommand`, который выполняется при достижении `RemainingItemsThreshold`.
- `RemainingItemsThresholdReachedCommandParameter` с типом `object`, который передается как параметр в `RemainingItemsThresholdReachedCommand`.

[`CollectionView`](xref:Xamarin.Forms.CollectionView) также определяет `RemainingItemsThresholdReached` событие, которое возникает, когда `CollectionView` прокручивается достаточно, чтобы `RemainingItemsThreshold` элементы не отображались. Это событие может быть обработано для загрузки дополнительных элементов. Кроме того, при срабатывании события `RemainingItemsThresholdReached` выполняется `RemainingItemsThresholdReachedCommand`, что позволяет выполнять добавочную загрузку данных в ViewModel.

Значение по умолчанию для свойства `RemainingItemsThreshold` равно-1, что означает, что событие `RemainingItemsThresholdReached` не будет запущено. Если значение свойства равно 0, то при отображении последнего элемента в [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) будет вызвано событие `RemainingItemsThresholdReached`. Для значений больше 0 событие `RemainingItemsThresholdReached` будет вызвано, когда `ItemsSource` содержит количество элементов, которые еще не прокручены.

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) проверяет свойство `RemainingItemsThreshold`, чтобы его значение всегда было больше или равно-1.

В следующем примере XAML показан [`CollectionView`](xref:Xamarin.Forms.CollectionView) , который выполняет добавочную загрузку данных:

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                RemainingItemsThreshold="5"
                RemainingItemsThresholdReached="OnCollectionViewRemainingItemsThresholdReached">
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    RemainingItemsThreshold = 5
};
collectionView.RemainingItemsThresholdReached += OnCollectionViewRemainingItemsThresholdReached;
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
```

В этом примере кода событие `RemainingItemsThresholdReached` срабатывает при наличии 5 элементов, которые еще не прокручены, а в ответ выполняет обработчик событий `OnCollectionViewRemainingItemsThresholdReached`:

```csharp
void OnCollectionViewRemainingItemsThresholdReached(object sender, EventArgs e)
{
    // Retrieve more data here and add it to the CollectionView's ItemsSource collection.
}
```

> [!NOTE]
> Данные также могут быть загружены постепенно путем привязки `RemainingItemsThresholdReachedCommand` к реализации `ICommand` в ViewModel.

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Рефрешвиев Xamarin. Forms](~/xamarin-forms/user-interface/refreshview.md)
- [Привязка данных Xamarin. Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Шаблоны данных Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Создание DataTemplateSelector Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
