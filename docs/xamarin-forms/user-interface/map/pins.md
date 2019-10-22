---
title: ПИН-коды карт Xamarin. Forms
description: В этой статье объясняется, как создавать ПИН-коды на карте Xamarin. Forms.
ms.prod: xamarin
ms.assetid: F8FC081B-A811-4FBB-B8F8-30D6FD36BD40
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 09/23/2019
ms.openlocfilehash: 76535f9c31a9dc138e132a3e582b986daf89bdb0
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697671"
---
# <a name="xamarinforms-map-pins"></a>ПИН-коды карт Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Элемент управления Xamarin. Forms `Maps` позволяет помечать расположения объектами `Pin`. @No__t_0 — это маркер Map, открывающий информационное окно при нажатии или касании.

Класс `Pin` имеет следующие свойства.

- `Type` — это `PinType` перечислимое значение: Generic, Place, Саведпин или SearchResult.
- `Position` — это `Position` экземпляр, содержащий широту и долготу ПИН-кода.
- `Label` — это `string`, который обычно отображается в качестве заголовка ПИН-кода.
- `Address` — это `string`, который будет отображаться в окне сведений. Это может быть любое `string`ное содержимое, а не только адрес.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что `Pin` может быть целевым объектом привязок данных. Дополнительные сведения см. в разделе [Создание закрепления с помощью привязки данных](#create-pins-with-data-binding).

## <a name="create-map-pins"></a>Создание ПИН-кодов карт

Экземпляр `Pin` можно создать в коде и добавить на карту:

```csharp
Pin pin1 = new Pin
{
    Type = PinType.Place,
    Position = new Position(47.6368678, -122.137305),
    Label = "Example Pin 1",
    Address = "Example custom details..."
};
map.Pins.Add(pin1);
```

> [!NOTE]
> Значение `PinType` влияет на отображение ПИН-кодов в зависимости от платформы. Чтобы настроить внешний вид ПИН-кода, необходимо создать пользовательский модуль подготовки отчетов. Дополнительные сведения см. [в разделе Настройка ПИН-кода Map](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

## <a name="create-pins-with-data-binding"></a>Создание ПИН-кодов с помощью привязки данных

Класс [`Map`](xref:Xamarin.Forms.Maps.Map) предоставляет следующие свойства:

- `ItemsSource` — указывает коллекцию отображаемых элементов `IEnumerable`.
- `ItemTemplate` — указывает [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , применяемый к каждому элементу в коллекции отображаемых элементов.
- `ItemTemplateSelector` — указывает [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) , который будет использоваться для выбора [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) для элемента во время выполнения.

> [!NOTE]
> Свойство `ItemTemplate` имеет приоритет, если заданы свойства `ItemTemplate` и `ItemTemplateSelector`.

[@No__t_1](xref:Xamarin.Forms.Maps.Map) можно заполнить данными с помощью привязки данных, чтобы привязать его свойство `ItemsSource` к коллекции `IEnumerable`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
             x:Class="WorkingWithMaps.PinItemsSourcePage">
    <Grid>
        ...
        <maps:Map x:Name="map"
                  MoveToLastRegionOnLayoutChange="false"
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

Данные свойства `ItemsSource` привязываются к свойству `Locations` подключенной модели представления, которая возвращает `ObservableCollection` объектов `Location`, которые являются пользовательским типом. Каждый объект `Location` определяет свойства `Address` и `Description`, тип `string` и свойство `Position` типа [`Position`](xref:Xamarin.Forms.Maps.Position).

Внешний вид каждого элемента в коллекции `IEnumerable` определяется путем присвоения свойству `ItemTemplate` значения [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , содержащего объект [`Pin`](xref:Xamarin.Forms.Maps.Pin) , который привязывает данные к соответствующим свойствам.

На следующих снимках экрана показан [`Map`](xref:Xamarin.Forms.Maps.Map) , отображающий коллекцию [`Pin`](xref:Xamarin.Forms.Maps.Pin) с помощью привязки данных:

[![Снимок экрана с привязками к данным в iOS и Android](map-images/pins-itemssource.png "Сопоставьте с закрепленными данными")](map-images/pins-itemssource-large.png#lightbox "Сопоставьте с закрепленными данными")

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

## <a name="pin-events"></a>Закрепление событий

Класс `Pin` предоставляет два события:

- `MarkerClicked` срабатывает при нажатии или касании ПИН-кода.
- `InfoWindowClicked` срабатывает при нажатии или касании окна info.

Обработчики событий можно прикрепить к ПИН-коду в коде:

```csharp
public class PinEvents: ContentPage
{
    public PinEvents()
    {
        // ...

        pin1.MarkerClicked += OnMarkerClickedAsync;
        pin1.InfoWindowClicked += OnInfoWindowClickedAsync;
    }

    async void OnMarkerClickedAsync(object sender, PinClickedEventArgs e)
    {
        string pinName = ((Pin)sender).Label;
        await DisplayAlert("Pin Clicked", $"{pinName} was clicked.", "Ok");
    }

    async void OnInfoWindowClickedAsync(object sender, PinClickedEventArgs e)
    {
        string pinName = ((Pin)sender).Label;
        await DisplayAlert("Info Window Clicked", $"The info window was clicked for {pinName}.", "Ok");
    }
}
```

## <a name="related-links"></a>Связанные ссылки

- [Пример Maps](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Преобразование пользовательского модуля подготовки отчетов](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Создание DataTemplateSelector Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
