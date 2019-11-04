---
title: ПИН-коды карт Xamarin. Forms
description: В этой статье объясняется, как создавать ПИН-коды на карте Xamarin. Forms.
ms.prod: xamarin
ms.assetid: F8FC081B-A811-4FBB-B8F8-30D6FD36BD40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/23/2019
ms.openlocfilehash: 930d2dcc701f88e2a350ec1011405bb18b86de6e
ms.sourcegitcommit: 3ea19e3a51515b30349d03c70a5b3acd7eca7fe7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/01/2019
ms.locfileid: "73425569"
---
# <a name="xamarinforms-map-pins"></a>ПИН-коды карт Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Элемент управления Xamarin. Forms [`Map`](xref:Xamarin.Forms.Maps.Map) позволяет помечать расположения объектами [`Pin`](xref:Xamarin.Forms.Maps.Pin) . `Pin` — это маркер Map, открывающий информационное окно при касании:

[![Снимок экрана: ПИН-код и информационное окно на устройстве iOS и Android](pins-images/pin-and-information-window.png "Отображение ПИН-кода с помощью окна сведений")](pins-images/pin-and-information-window-large.png#lightbox "Отображение ПИН-кода с помощью окна сведений")

При добавлении [`Pin`](xref:Xamarin.Forms.Maps.Pin) объекта в коллекцию [`Map.Pins`](xref:Xamarin.Forms.Maps.Pin) на карте отображается ПИН-код.

Класс [`Pin`](xref:Xamarin.Forms.Maps.Pin) имеет следующие свойства.

- [`Address`](xref:Xamarin.Forms.Maps.Pin.Address)типа `string`, который обычно представляет адрес для расположения ПИН-кода. Однако это может быть любое `string`ное содержимое, а не только адрес.
- [`Label`](xref:Xamarin.Forms.Maps.Pin.Label), типа `string`, который обычно представляет заголовок ПИН-кода.
- [`Position`](xref:Xamarin.Forms.Maps.Pin.Position), типа [`Position`](xref:Xamarin.Forms.Maps.Position), который представляет широту и долготу ПИН-кода.
- [`Type`](xref:Xamarin.Forms.Maps.Pin.Type), типа [`PinType`](xref:Xamarin.Forms.Maps.PinType), который представляет тип ПИН-кода.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что `Pin` может быть целевым объектом привязок данных. Дополнительные сведения о привязке данных `Pin` объектов см. [в разделе Отображение коллекции закрепления](#display-a-pin-collection).

Кроме того, класс [`Pin`](xref:Xamarin.Forms.Maps.Pin) определяет события `MarkerClicked` и `InfoWindowClicked`. Событие `MarkerClicked` срабатывает при касании ПИН-кода, а событие `InfoWindowClicked` возникает при касании информационного окна. Объект `PinClickedEventArgs`, сопровождающий оба события, имеет одно свойство `HideInfoWindow` типа `bool`.

## <a name="display-a-pin"></a>Отображение ПИН-кода

[`Pin`](xref:Xamarin.Forms.Maps.Pin) можно добавить в [`Map`](xref:Xamarin.Forms.Maps.Map) в XAML:

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
     <maps:Map x:Name="map"
               IsShowingUser="True"
               MoveToLastRegionOnLayoutChange="False">
         <x:Arguments>
             <maps:MapSpan>
                 <x:Arguments>
                     <maps:Position>
                         <x:Arguments>
                             <x:Double>36.9628066</x:Double>
                             <x:Double>-122.0194722</x:Double>
                         </x:Arguments>
                     </maps:Position>
                     <x:Double>0.01</x:Double>
                     <x:Double>0.01</x:Double>
                 </x:Arguments>
             </maps:MapSpan>
         </x:Arguments>
         <maps:Map.Pins>
             <maps:Pin Label="Santa Cruz"
                       Address="The city with a boardwalk"
                       Type="Place">
                 <maps:Pin.Position>
                     <maps:Position>
                         <x:Arguments>
                             <x:Double>36.9628066</x:Double>
                             <x:Double>-122.0194722</x:Double>
                         </x:Arguments>
                     </maps:Position>
                 </maps:Pin.Position>
             </maps:Pin>
         </maps:Map.Pins>
     </maps:Map>
</ContentPage>
```

Этот код XAML создает объект [`Map`](xref:Xamarin.Forms.Maps.Map) , который показывает область, указанную объектом [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) . Объект `MapSpan` выравнивается по центру широты и долготы, представленные объектом [`Position`](xref:Xamarin.Forms.Maps.Position) , который расширяет 0,01 широты и долготы. Объект [`Pin`](xref:Xamarin.Forms.Maps.Pin) добавляется в коллекцию [`Map.Pins`](xref:Xamarin.Forms.Maps.Pin) и нарисовывается на `Map` в расположении, указанном свойством [`Position`](xref:Xamarin.Forms.Maps.Pin.Position) . Сведения о структуре [`Position`](xref:Xamarin.Forms.Maps.Position) см. в разделе [Map Disposition and Distance](position-distance.md). Сведения о передаче аргументов в XAML в объекты, у которых отсутствуют конструкторы по умолчанию, см. [в разделе Передача аргументов в XAML](~/xamarin-forms/xaml/passing-arguments.md).

Эквивалентный код на C# выглядит так:

```csharp
using Xamarin.Forms.Maps;
// ...
Map map = new Map
{
  // ...
};
Pin pin = new Pin
{
  Label = "Santa Cruz",
  Address = "The city with a boardwalk",
  Type = PinType.Place,
  Position = new Position(36.9628066, -122.0194722)
};
map.Pins.Add(pin);
```

> [!WARNING]
> Сбой установки свойства [`Pin.Label`](xref:Xamarin.Forms.Maps.Pin.Label) приведет к появлению `ArgumentException` при добавлении [`Pin`](xref:Xamarin.Forms.Maps.Pin) в [`Map`](xref:Xamarin.Forms.Maps.Map).

Этот пример кода приводит к отображению одного ПИН-кода на карте:

[![Снимок экрана с ПИН-кодом для карт на iOS и Android](pins-images/pin-only.png "ПИН-код Map")](pins-images/pin-only-large.png#lightbox "ПИН-код Map")

## <a name="interact-with-a-pin"></a>Взаимодействие с ПИН-кодом

По умолчанию при касании [`Pin`](xref:Xamarin.Forms.Maps.Pin) отображается информационное окно:

[![Снимок экрана: ПИН-код и информационное окно на устройстве iOS и Android](pins-images/pin-and-information-window.png "Отображение ПИН-кода с помощью окна сведений")](pins-images/pin-and-information-window-large.png#lightbox "Отображение ПИН-кода с помощью окна сведений")

Если коснуться на карте, окно сведений закрывается.

Класс [`Pin`](xref:Xamarin.Forms.Maps.Pin) определяет событие `MarkerClicked`, которое срабатывает при касании `Pin`. Нет необходимости в обработке этого события для вывода информационного окна. Вместо этого это событие должно быть обработано при наличии требования, чтобы получать уведомления о касании определенного ПИН-кода.

Класс [`Pin`](xref:Xamarin.Forms.Maps.Pin) также определяет событие `InfoWindowClicked`, которое возникает при касании информационного окна. Это событие должно быть обработано при наличии требования, чтобы получать уведомления о касании конкретного информационного окна.

В следующем коде показан пример обработки этих событий:

```csharp
using Xamarin.Forms.Maps;
// ...
Pin boardwalkPin = new Pin
{
    Position = new Position(36.9641949, -122.0177232),
    Label = "Boardwalk",
    Address = "Santa Cruz",
    Type = PinType.Place
};
boardwalkPin.MarkerClicked += async (s, args) =>
{
    args.HideInfoWindow = true;
    string pinName = ((Pin)s).Label;
    await DisplayAlert("Pin Clicked", $"{pinName} was clicked.", "Ok");
};

Pin wharfPin = new Pin
{
    Position = new Position(36.9571571, -122.0173544),
    Label = "Wharf",
    Address = "Santa Cruz",
    Type = PinType.Place
};
wharfPin.InfoWindowClicked += async (s, args) =>
{
    string pinName = ((Pin)s).Label;
    await DisplayAlert("Info Window Clicked", $"The info window was clicked for {pinName}.", "Ok");
};
```

Объект `PinClickedEventArgs`, сопровождающий оба события, имеет одно свойство `HideInfoWindow` типа `bool`. Если это свойство имеет значение `true` в обработчике событий, информационное окно будет скрыто.

## <a name="pin-types"></a>Типы ПИН-кодов

[`Pin`](xref:Xamarin.Forms.Maps.Pin) объекты включают свойство [`Type`](xref:Xamarin.Forms.Maps.Pin.Type) типа [`PinType`](xref:Xamarin.Forms.Maps.PinType), представляющее тип ПИН-кода. Перечисление `PinType` определяет следующие члены:

- `Generic`представляет универсальный ПИН-код.
- `Place`, представляет ПИН-код для места.
- `SavedPin`представляет ПИН-код для сохраненного расположения.
- `SearchResult`представляет ПИН-код для результата поиска.

Однако установка свойства [`Pin.Type`](xref:Xamarin.Forms.Maps.Pin.Type) для любого элемента [`PinType`](xref:Xamarin.Forms.Maps.PinType) не изменяет внешний вид отображаемого ПИН-кода. Вместо этого необходимо создать пользовательский модуль подготовки отчетов для настройки внешнего вида ПИН-кода. Дополнительные сведения см. [в разделе Настройка ПИН-кода Map](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

## <a name="display-a-pin-collection"></a>Отображение коллекции закрепления

Класс [`Map`](xref:Xamarin.Forms.Maps.Map) определяет следующие свойства:

- [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource), типа `IEnumerable`, который указывает коллекцию элементов `IEnumerable` для отображения.
- [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate), типа [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), который указывает [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , применяемые к каждому элементу в коллекции отображаемых элементов.
- `ItemTemplateSelector`, типа [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector), который указывает [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) , которое будет использоваться для выбора [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) для элемента во время выполнения.

> [!IMPORTANT]
> Свойство [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate) имеет приоритет, если заданы свойства `ItemTemplate` и `ItemTemplateSelector`.

[`Map`](xref:Xamarin.Forms.Maps.Map) можно заполнить ПИН-кодов с помощью привязки данных, чтобы привязать его свойство [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource) к коллекции `IEnumerable`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
             x:Class="WorkingWithMaps.PinItemsSourcePage">
    <Grid>
        ...
        <maps:Map x:Name="map"
                  ItemsSource="{Binding Locations}">
            <maps:Map.ItemTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="{Binding Description}" />
                </DataTemplate>
            </maps:Map.ItemTemplate>
        </maps:Map>
        ...
    </Grid>
</ContentPage>
```

Данные свойства [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource) привязываются к свойству `Locations` подключенного ViewModel, который возвращает `ObservableCollection` объектов `Location`, которые являются пользовательским типом. Каждый объект `Location` определяет свойства `Address` и `Description`, тип `string` и свойство `Position` типа [`Position`](xref:Xamarin.Forms.Maps.Position).

Внешний вид каждого элемента в коллекции `IEnumerable` определяется путем присвоения свойству [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate) значения [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , содержащего объект [`Pin`](xref:Xamarin.Forms.Maps.Pin) , который привязывает данные к соответствующим свойствам.

На следующих снимках экрана показан [`Map`](xref:Xamarin.Forms.Maps.Map) , отображающий коллекцию [`Pin`](xref:Xamarin.Forms.Maps.Pin) с помощью привязки данных:

[![Снимок экрана с привязками к данным в iOS и Android](pins-images/pins-itemsource.png "Сопоставьте с закрепленными данными")](pins-images/pins-itemsource-large.png#lightbox "Сопоставьте с закрепленными данными")

### <a name="choose-item-appearance-at-runtime"></a>Выбор внешнего вида элемента во время выполнения

Внешний вид каждого элемента в `IEnumerable` коллекции можно выбрать во время выполнения на основе значения элемента, установив для свойства `ItemTemplateSelector` значение [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector).

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:WorkingWithMaps"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
    <ContentPage.Resources>
        <local:MapItemTemplateSelector x:Key="MapItemTemplateSelector">
            <local:MapItemTemplateSelector.DefaultTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="{Binding Description}" />
                </DataTemplate>
            </local:MapItemTemplateSelector.DefaultTemplate>
            <local:MapItemTemplateSelector.XamarinTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="Xamarin!" />
                </DataTemplate>
            </local:MapItemTemplateSelector.XamarinTemplate>    
        </local:MapItemTemplateSelector>
    </ContentPage.Resources>

    <Grid>
        ...
        <maps:Map x:Name="map"
                  ItemsSource="{Binding Locations}"
                  ItemTemplateSelector="{StaticResource MapItemTemplateSelector}" />
        ...
    </Grid>
</ContentPage>
```

В следующем примере показан класс `MapItemTemplateSelector`:

```csharp
public class MapItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate XamarinTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return ((Location)item).Address.Contains("San Francisco") ? XamarinTemplate : DefaultTemplate;
    }
}
```

Класс `MapItemTemplateSelector` определяет `DefaultTemplate` и `XamarinTemplate` свойства [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , для которых заданы разные шаблоны данных. Метод `OnSelectTemplate` возвращает `XamarinTemplate`, который отображает Xamarin в качестве метки при касании `Pin`, если элемент имеет адрес, содержащий "Сан-Франциско". Если у элемента нет адреса, содержащего "Сан Франциско", метод `OnSelectTemplate` возвращает `DefaultTemplate`.

Дополнительные сведения о селекторах шаблонов данных см. [в разделе Создание DataTemplateSelector Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Связанные ссылки

- [Пример Maps](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Преобразование пользовательского модуля подготовки отчетов](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Передача аргументов в XAML](~/xamarin-forms/xaml/passing-arguments.md)
- [Создание DataTemplateSelector Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
