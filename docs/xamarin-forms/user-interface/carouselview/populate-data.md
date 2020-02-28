---
title: Данные Карауселвиев в Xamarin. Forms
description: Карауселвиев заполняется данными путем присвоения свойству ItemsSource любой коллекции, реализующей IEnumerable.
ms.prod: xamarin
ms.assetid: 20DB2C57-CE3A-4D91-80DC-73AE361A3CB0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/27/2019
ms.openlocfilehash: 154d039e95ccc2de28e09a7162a32a19f8f84656
ms.sourcegitcommit: 5d22f37dfc358678df52a4d17c57261056a72cb7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2020
ms.locfileid: "77674540"
---
# <a name="xamarinforms-carouselview-data"></a>Данные Карауселвиев в Xamarin. Forms

![](~/media/shared/preview.png "This API is currently pre-release")

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView) содержит следующие свойства, которые определяют отображаемые данные и его внешний вид:

- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource), типа `IEnumerable`, указывает коллекцию отображаемых элементов и значение по умолчанию `null`.
- [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)типа [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)указывает шаблон, применяемый к каждому элементу в коллекции отображаемых элементов.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами. Это означает, что свойства могут быть целевыми объектами привязок данных.

> [!NOTE]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView) определяет свойство `ItemsUpdatingScrollMode`, представляющее поведение прокрутки `CarouselView` при добавлении к нему новых элементов. Дополнительные сведения об этом свойстве см. в разделе [управление позицией прокрутки при добавлении новых элементов](scrolling.md#control-scroll-position-when-new-items-are-added).

[`CarouselView`](xref:Xamarin.Forms.CarouselView) также может загружать данные постепенно по мере прокрутки пользователем. Дополнительные сведения см. в статье [добавочная загрузка данных](#load-data-incrementally).

## <a name="populate-a-carouselview-with-data"></a>Заполнение Карауселвиев данными

[`CarouselView`](xref:Xamarin.Forms.CarouselView) заполняется данными путем присвоения свойству [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) любой коллекции, реализующей `IEnumerable`. Элементы можно добавить в XAML, инициализируя свойство `ItemsSource` из массива строк:

```xaml
<CarouselView>
    <CarouselView.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Baboon</x:String>
            <x:String>Capuchin Monkey</x:String>
            <x:String>Blue Monkey</x:String>
            <x:String>Squirrel Monkey</x:String>
            <x:String>Golden Lion Tamarin</x:String>
            <x:String>Howler Monkey</x:String>
            <x:String>Japanese Macaque</x:String>
        </x:Array>
    </CarouselView.ItemsSource>
</CarouselView>
```

> [!NOTE]
> Обратите внимание на то, что для элемента `x:Array` требуется атрибут `Type`, указывающий тип элементов в массиве.

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.ItemsSource = new string[]
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
> Если [`CarouselView`](xref:Xamarin.Forms.CarouselView) требуется обновлять при добавлении, удалении или изменении элементов в базовой коллекции, базовая коллекция должна быть коллекцией `IEnumerable`, которая отправляет уведомления об изменении свойств, например `ObservableCollection`.

По умолчанию [`CarouselView`](xref:Xamarin.Forms.CarouselView) отображает элементы по горизонтали. На следующих снимках экрана показано `CarouselView` отображения различных строковых элементов в iOS и Android:

[![Снимок экрана Карауселвиев, содержащий текстовые элементы, в iOS и Android](populate-data-images/text.png "Текстовые элементы в Карауселвиев")](populate-data-images/text-large.png#lightbox "Текстовые элементы в Карауселвиев")

Сведения о том, как изменить ориентацию [`CarouselView`](xref:Xamarin.Forms.CarouselView) , см. в разделе [карауселвиевing The Xamarin. Forms](layout.md). Сведения о том, как определить внешний вид каждого элемента в `CarouselView`, см. в разделе [Определение внешнего вида элемента](#define-item-appearance).

### <a name="data-binding"></a>привязка данных,

[`CarouselView`](xref:Xamarin.Forms.CarouselView) можно заполнить данными с помощью привязки данных, чтобы привязать его свойство [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) к коллекции `IEnumerable`. В XAML это достигается с помощью расширения разметки `Binding`:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}" />
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

В этом примере данные свойства [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) привязываются к свойству `Monkeys` подключенного ViewModel.

> [!NOTE]
> Скомпилированные привязки можно включить для повышения производительности привязки данных в приложениях Xamarin. Forms. Дополнительные сведения см. в статье [Скомпилированные привязки](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md).

Дополнительные сведения о привязке данных см. в разделе [Привязки данных в Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md).

## <a name="define-item-appearance"></a>Определение внешнего вида элемента

Внешний вид каждого элемента в [`CarouselView`](xref:Xamarin.Forms.CarouselView) можно определить, присвоив свойству [`CarouselView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate).

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

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

carouselView.ItemTemplate = new DataTemplate(() =>
{
    Label nameLabel = new Label { ... };
    nameLabel.SetBinding(Label.TextProperty, "Name");

    Image image = new Image { ... };
    image.SetBinding(Image.SourceProperty, "ImageUrl");

    Label locationLabel = new Label { ... };
    locationLabel.SetBinding(Label.TextProperty, "Location");

    Label detailsLabel = new Label { ... };
    detailsLabel.SetBinding(Label.TextProperty, "Details");

    StackLayout stackLayout = new StackLayout
    {
        Children = { nameLabel, image, locationLabel, detailsLabel }
    };

    Frame frame = new Frame { ... };
    StackLayout rootStackLayout = new StackLayout
    {
        Children = { frame }
    };

    return rootStackLayout;
});
```

Элементы, указанные в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) определяют внешний вид каждого элемента в `CarouselView`. В этом примере макет в `DataTemplate` управляется [`StackLayout`](xref:Xamarin.Forms.StackLayout), а данные отображаются с помощью [`Image`](xref:Xamarin.Forms.Image) объекта и трех [`Label`](xref:Xamarin.Forms.Label) объектов, которые привязываются к свойствам класса `Monkey`:

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

На следующих снимках экрана показан результат создания шаблонов каждого элемента:

[![Снимок экрана Карауселвиев, где для каждого элемента используется шаблон, в iOS и Android](populate-data-images/datatemplate.png "Элементы шаблона в Карауселвиев")](populate-data-images/datatemplate-large.png#lightbox "Элементы шаблона в Карауселвиев")

Дополнительные сведения о шаблонах данных см. в разделе [Шаблоны данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="choose-item-appearance-at-runtime"></a>Выбор внешнего вида элемента во время выполнения

Внешний вид каждого элемента в [`CarouselView`](xref:Xamarin.Forms.CarouselView) можно выбрать во время выполнения на основе значения элемента, установив для свойства [`CarouselView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) значение [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) объект:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CarouselViewDemos.Controls"
             x:Class="CarouselViewDemos.Views.HorizontalLayoutDataTemplateSelectorPage">
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

    <CarouselView ItemsSource="{Binding Monkeys}"
                  ItemTemplate="{StaticResource MonkeySelector}" />
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView
{
    ItemTemplate = new MonkeyDataTemplateSelector { ... }
};
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
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

Класс `MonkeyDataTemplateSelector` определяет `AmericanMonkey` и `OtherMonkey` свойства [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , для которых заданы разные шаблоны данных. Переопределение `OnSelectTemplate` Возвращает шаблон `AmericanMonkey`, если имя обезьяны содержит "America". Если имя обезьяны не содержит "America", то переопределение `OnSelectTemplate` Возвращает шаблон `OtherMonkey`, в котором данные отображаются серым цветом:

[![Снимок экрана выбора шаблона элемента среды выполнения Карауселвиев в iOS и Android](populate-data-images/datatemplateselector.png "Выбор шаблона элемента среды выполнения в Карауселвиев")](populate-data-images/datatemplateselector-large.png#lightbox "Выбор шаблона элемента среды выполнения в Карауселвиев")

Дополнительные сведения о селекторах шаблонов данных см. [в разделе Создание DataTemplateSelector Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

> [!IMPORTANT]
> При использовании [`CarouselView`](xref:Xamarin.Forms.CarouselView)никогда не устанавливайте в качестве корневого элемента [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) объектов `ViewCell`. Это приведет к возникновению исключения, поскольку `CarouselView` не имеет концепции ячеек.

## <a name="display-indicators"></a>Отображение индикаторов

Индикаторы, представляющие количество элементов и текущую позиции в `CarouselView`, могут отображаться рядом с `CarouselView`. Это можно сделать с помощью элемента управления `IndicatorView`:

```xaml
<StackLayout>
    <CarouselView ItemsSource="{Binding Monkeys}"
                  IndicatorView="indicatorView">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView x:Name="indicatorView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="DarkGray"
                   HorizontalOptions="Center" />
</StackLayout>
```

В этом примере `IndicatorView` отображается под `CarouselView`, с индикатором для каждого элемента в `CarouselView`. `IndicatorView` заполняется данными путем установки свойства `CarouselView.IndicatorView` в объект `IndicatorView`. Каждый индикатор представляет собой светло-серый круг, а индикатор, представляющий текущий элемент в `CarouselView`, темно-серый:

[![Снимок экрана Карауселвиев и Индикаторвиев на iOS и Android](populate-data-images/indicators.png "Индикаторвиев круги")](populate-data-images/indicators-large.png#lightbox "Индикаторвиев круги")

> [!IMPORTANT]
> Установка свойства `CarouselView.IndicatorView` приводит к привязке свойства `IndicatorView.Position` к свойству `CarouselView.Position`, а также к привязке свойства `IndicatorView.ItemsSource` к свойству `CarouselView.ItemsSource`.

Дополнительные сведения о индикаторах см. в разделе [Xamarin. Forms индикаторвиев](~/xamarin-forms/user-interface/indicatorview.md).

## <a name="pull-to-refresh"></a>Извлечь для обновления

[`CarouselView`](xref:Xamarin.Forms.CarouselView) поддерживает функцию Pull для обновления с помощью `RefreshView`, что позволяет обновлять отображаемые данные путем извлечения элементов. `RefreshView` — это контейнерный элемент управления, предоставляющий функции обновления для своего дочернего элемента, при условии, что дочерний объект поддерживает прокручиваемое содержимое. Таким образом, запрос на обновление реализуется для `CarouselView`, настроив его как дочерний элемент `RefreshView`.

```xaml
<RefreshView IsRefreshing="{Binding IsRefreshing}"
             Command="{Binding RefreshCommand}">
    <CarouselView ItemsSource="{Binding Animals}">
        ...
    </CarouselView>
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

CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
refreshView.Content = carouselView;
// ...
```

Когда пользователь инициирует обновление, выполняется `ICommand`, определяемое свойством `Command`, которое должно обновлять отображаемые элементы. Во время обновления отображается визуализация обновления, которая состоит из анимированного круга хода выполнения:

[![Снимок экрана Карауселвиев Pull-to-Refresh, в iOS и Android](populate-data-images/pull-to-refresh.png "Карауселвиев по запросу на обновление")](populate-data-images/pull-to-refresh-large.png#lightbox "Карауселвиев по запросу на обновление")

Значение свойства `RefreshView.IsRefreshing` указывает текущее состояние `RefreshView`. При активации обновления пользователем это свойство автоматически переходит в `true`. После завершения обновления следует сбросить свойство на `false`.

Дополнительные сведения о `RefreshView`см. в разделе [Xamarin. Forms рефрешвиев](~/xamarin-forms/user-interface/refreshview.md).

## <a name="load-data-incrementally"></a>Добавочная загрузка данных

[`CarouselView`](xref:Xamarin.Forms.CarouselView) поддерживает добавочную загрузку данных по мере прокрутки элементов пользователями. Это позволяет выполнять такие сценарии, как асинхронная загрузка страницы данных из веб-службы при прокрутке пользователем. Кроме того, точка, в которой загружаются дополнительные данные, настраивается таким образом, что пользователи не видят пустое пространство или останавливаются при прокрутке.

[`CarouselView`](xref:Xamarin.Forms.CarouselView) определяет следующие свойства для управления добавочной загрузкой данных:

- `RemainingItemsThreshold`типа `int`пороговое значение элементов, которые еще не отображаются в списке, в котором будет запущено событие `RemainingItemsThresholdReached`.
- `RemainingItemsThresholdReachedCommand`, типа `ICommand`, который выполняется при достижении `RemainingItemsThreshold`.
- `RemainingItemsThresholdReachedCommandParameter` с типом `object`, который передается как параметр в `RemainingItemsThresholdReachedCommand`.

[`CarouselView`](xref:Xamarin.Forms.CarouselView) также определяет `RemainingItemsThresholdReached` событие, которое возникает, когда `CarouselView` прокручивается достаточно, чтобы `RemainingItemsThreshold` элементы не отображались. Это событие может быть обработано для загрузки дополнительных элементов. Кроме того, при срабатывании события `RemainingItemsThresholdReached` выполняется `RemainingItemsThresholdReachedCommand`, что позволяет выполнять добавочную загрузку данных в ViewModel.

Значение по умолчанию для свойства `RemainingItemsThreshold` равно-1, что означает, что событие `RemainingItemsThresholdReached` не будет запущено. Если значение свойства равно 0, то при отображении последнего элемента в [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) будет вызвано событие `RemainingItemsThresholdReached`. Для значений больше 0 событие `RemainingItemsThresholdReached` будет вызвано, когда `ItemsSource` содержит количество элементов, которые еще не прокручены.

> [!NOTE]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView) проверяет свойство `RemainingItemsThreshold`, чтобы его значение всегда было больше или равно-1.

В следующем примере XAML показан [`CarouselView`](xref:Xamarin.Forms.CarouselView) , который выполняет добавочную загрузку данных:

```xaml
<CarouselView ItemsSource="{Binding Animals}"
              RemainingItemsThreshold="2"
              RemainingItemsThresholdReached="OnCarouselViewRemainingItemsThresholdReached"
              RemainingItemsThresholdReachedCommand="{Binding LoadMoreDataCommand}">
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView
{
    RemainingItemsThreshold = 2
};
carouselView.RemainingItemsThresholdReached += OnCollectionViewRemainingItemsThresholdReached;
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
```

В этом примере кода событие `RemainingItemsThresholdReached` срабатывает, когда еще не прокручивается 2 элемента, а в ответе выполняется `OnCollectionViewRemainingItemsThresholdReached` обработчик событий:

```csharp
void OnCollectionViewRemainingItemsThresholdReached(object sender, EventArgs e)
{
    // Retrieve more data here and add it to the CollectionView's ItemsSource collection.
}
```

> [!NOTE]
> Данные также могут быть загружены постепенно путем привязки `RemainingItemsThresholdReachedCommand` к реализации `ICommand` в ViewModel.

## <a name="related-links"></a>Связанные ссылки

- [Карауселвиев (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [Индикаторвиев Xamarin. Forms](~/xamarin-forms/user-interface/indicatorview.md)
- [Рефрешвиев Xamarin. Forms](~/xamarin-forms/user-interface/refreshview.md)
- [Привязка данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Шаблоны данных Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Создание DataTemplateSelector Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
