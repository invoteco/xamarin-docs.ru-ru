---
title: Map Control Xamarin. Forms
description: Элемент управления картой представляет собой кросс-платформенное представление для отображения и аннотирования карт. Он использует собственный элемент управления картой для каждой платформы, обеспечивая быстрый и знакомый интерфейс карт для пользователей.
ms.prod: xamarin
ms.assetid: 22C99029-0B16-43A6-BF58-26B48C4AED38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/29/2019
ms.openlocfilehash: 1cfda90360557af1160d421f18807f8b534967a8
ms.sourcegitcommit: 3ea19e3a51515b30349d03c70a5b3acd7eca7fe7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/01/2019
ms.locfileid: "73426351"
---
# <a name="xamarinforms-map-control"></a>Map Control Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Элемент управления [`Map`](xref:Xamarin.Forms.Maps.Map) представляет собой кросс-платформенное представление для отображения и аннотирования карт. Он использует собственный элемент управления картой для каждой платформы, обеспечивая быстрый и знакомый интерфейс карт для пользователей:

[![Снимок экрана элемента управления картой в iOS и Android](map-images/map-default.png "Элемент управления картой")](map-images/map-default-large.png#lightbox "Элемент управления картой")

Класс [`Map`](xref:Xamarin.Forms.Maps.Map) определяет следующие свойства, которые управляют внешним видом и поведением карт.

- [`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser)типа `bool`указывает, отображается ли на карте текущее расположение пользователя.
- [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource), типа `IEnumerable`, который указывает коллекцию элементов `IEnumerable` для отображения.
- [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate), типа [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), который указывает [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , применяемые к каждому элементу в коллекции отображаемых элементов.
- `ItemTemplateSelector`, типа [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector), который указывает [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) , которое будет использоваться для выбора [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) для элемента во время выполнения.
- [`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled)типа `bool`определяет, разрешена ли на карте прокрутка.
- [`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled)типа `bool`определяет, разрешено ли на карте возможность масштабирования.
- `MapElements`типа `IList<MapElement>`представляет список элементов на карте, например многоугольники и ломаные линии.
- [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)типа [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)указывает стиль отображения для схемы.
- `MoveToLastRegionOnLayoutChange`типа `bool`определяет, будет ли передвигаться отображаемая область карт из текущей области в ее ранее заданную область при изменении макета.
- [`Pins`](xref:Xamarin.Forms.Maps.Map.Pins)типа `IList<Pin>`представляет список ПИН-кодов на карте.
- [`VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion)типа [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)Возвращает отображаемую в данный момент область на карте.

Эти свойства, за исключением свойств `MapElements`, `Pins`и `VisibleRegion`, поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) , что означает, что они могут быть целями привязок данных.

Класс [`Map`](xref:Xamarin.Forms.Maps.Map) также определяет событие `MapClicked`, которое возникает при нажатии на карту. Объект `MapClickedEventArgs`, сопровождающий событие, имеет одно свойство с именем `Position`типа [`Position`](xref:Xamarin.Forms.Maps.Position). При срабатывании события для свойства `Position` задается выбранное расположение на карте. Сведения о структуре [`Position`](xref:Xamarin.Forms.Maps.Position) см. в разделе [Map Disposition and Distance](position-distance.md).

Дополнительные сведения о свойствах [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource), [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)и `ItemTemplateSelector` см. [в разделе Отображение коллекции закрепления](pins.md#display-a-pin-collection).

## <a name="display-a-map"></a>Отображение схемы

[`Map`](xref:Xamarin.Forms.Maps.Map) можно отобразить, добавив его в макет или страницу:

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
    <maps:Map x:Name="map" />
</ContentPage>
```

> [!NOTE]
> Для ссылки на элементы управления Xamarin. Forms. Maps требуется дополнительное определение пространства имен `xmlns`. В предыдущем примере ссылка на пространство имен `Xamarin.Forms.Maps` указывается с помощью ключевого слова `maps`.

Эквивалентный код на C# выглядит так:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Maps;

namespace WorkingWithMaps
{
    public class MapTypesPageCode : ContentPage
    {
        public MapTypesPageCode()
        {
            Map map = new Map();
            Content = map;
        }
    }
}
```

В этом примере вызывается конструктор по умолчанию [`Map`](xref:Xamarin.Forms.Maps.Map) , который выравнивает карту по Рим:

[![Снимок экрана элемента управления Map с расположением по умолчанию в iOS и Android](map-images/map-default.png "Элемент управления Map с расположением по умолчанию")](map-images/map-default-large.png#lightbox "Элемент управления Map с расположением по умолчанию")

Кроме того, [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) аргумент может быть передан в конструктор [`Map`](xref:Xamarin.Forms.Maps.Map) , чтобы задать центральную точку и уровень масштабирования для схемы при ее загрузке. Дополнительные сведения см. в разделе [Отображение определенного расположения на карте](#display-a-specific-location-on-a-map).

## <a name="map-types"></a>Типы карт

Свойству [`Map.MapType`](xref:Xamarin.Forms.Maps.Map.MapType) может быть присвоено [`MapType`](xref:Xamarin.Forms.Maps.MapType) элемент перечисления, определяющий стиль отображения для схемы. Перечисление `MapType` определяет следующие члены:

- `Street` указывает, что будет отображаться улица.
- `Satellite` указывает, что будет отображена схема, содержащая сопутствующие изображения.
- `Hybrid` указывает, что будут отображены комбинированные сведения о улицы и спутниковом сопоставлении.

По умолчанию [`Map`](xref:Xamarin.Forms.Maps.Map) отображает карту улицы, если свойство [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) не определено. Кроме того, свойству `MapType` может быть присвоено одно из членов перечисления [`MapType`](xref:Xamarin.Forms.Maps.MapType) :

```xaml
<maps:Map MapType="Satellite" />
```

Эквивалентный код на C# выглядит так:

```csharp
Map map = new Map
{
    MapType = MapType.Satellite
};
```

На следующих снимках экрана показано [`Map`](xref:Xamarin.Forms.Maps.Map) , если свойство [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) имеет значение `Street`.

[![Снимок экрана элемента управления Map с типом схемы улицы в iOS и Android](map-images/maptype-street.png "Сопоставьте элемент управления с маптипе улицы")](map-images/maptype-street-large.png#lightbox "Map control with the street map type")

На следующих снимках экрана показано [`Map`](xref:Xamarin.Forms.Maps.Map) , если свойство [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) имеет значение `Satellite`.

[![Снимок экрана элемента управления Map с типом вспомогательной схемы в iOS и Android](map-images/maptype-satellite.png "Отображение элемента управления с помощью вспомогательной маптипе")](map-images/maptype-satellite-large.png#lightbox "Map control with the satellite map type")

На следующих снимках экрана показано [`Map`](xref:Xamarin.Forms.Maps.Map) , если свойство [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) имеет значение `Hybrid`.

[![Снимок экрана элемента управления Map с типом гибридной схемы в iOS и Android](map-images/maptype-hybrid.png "Управление картой с помощью гибридного маптипе")](map-images/maptype-hybrid-large.png#lightbox "Map control with the hybrid map type")

## <a name="display-a-specific-location-on-a-map"></a>Отображение определенного расположения на карте

Область отображения, отображаемая при загрузке схемы, может быть задана путем передачи аргумента [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) в конструктор [`Map`](xref:Xamarin.Forms.Maps.Map) :

```xaml
<maps:Map>
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
</maps:Map>
```

Эквивалентный код на C# выглядит так:

```csharp
Position position = new Position(36.9628066, -122.0194722);
MapSpan mapSpan = new MapSpan(position, 0.01, 0.01);
Map map = new Map(mapSpan);
```

В этом примере создается объект [`Map`](xref:Xamarin.Forms.Maps.Map) , который показывает область, указанную объектом [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) . Объект `MapSpan` выравнивается по центру широты и долготы, представленные объектом [`Position`](xref:Xamarin.Forms.Maps.Position) , и охватывает 0,01 широты и 0,01 долготы. Сведения о структуре [`Position`](xref:Xamarin.Forms.Maps.Position) см. в разделе [Map Disposition and Distance](position-distance.md). Дополнительные сведения о передаче аргументов в XAML см. в разделе [Передача аргументов в XAML](~/xamarin-forms/xaml/passing-arguments.md).

Результат заключается в том, что при отображении карт она выравнивается по центру в определенном месте и охватывает определенное количество широт и долготы:

[![Снимок экрана элемента управления Map с указанным расположением в iOS и Android](map-images/map-region.png "Элемент управления Map с указанным расположением")](map-images/map-region-large.png#lightbox "Элемент управления Map с указанным расположением")

## <a name="create-a-mapspan-object"></a>Создание объекта Мапспан

Существует ряд подходов к созданию [`MapSpan`ных](xref:Xamarin.Forms.Maps.MapSpan) объектов. Распространенным подходом является предоставление требуемых аргументов конструктору `MapSpan`. Это Широта и Долгота, представленная объектом [`Position`](xref:Xamarin.Forms.Maps.Position) , и `double` значения, представляющие градусы широты и долготы, которые являются частью `MapSpan`. Сведения о структуре [`Position`](xref:Xamarin.Forms.Maps.Position) см. в разделе [Map Disposition and Distance](position-distance.md).

Кроме того, в классе [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) есть три метода, которые возвращают новые объекты `MapSpan`:

1. [`ClampLatitude`](xref:Xamarin.Forms.Maps.MapSpan.ClampLatitude*) возвращает `MapSpan` с тем же `LongitudeDegrees`, что и экземпляр класса метода, и радиус, определяемый его аргументами `north` и `south`.
1. [`FromCenterAndRadius`](xref:Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius*) возвращает `MapSpan`, определяемый его [`Position`](xref:Xamarin.Forms.Maps.Position) и [`Distance`](xref:Xamarin.Forms.Maps.Distance) аргументами.
1. [`WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom*) возвращает `MapSpan` с тем же центром, что и экземпляр класса метода, но с радиусом, умноженным на аргумент `double`.

Сведения о структуре [`Distance`](xref:Xamarin.Forms.Maps.Distance) см. в разделе [Map Disposition and Distance](position-distance.md).

После создания [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) можно получить доступ к следующим свойствам, чтобы получить данные о нем:

- [`Center`](xref:Xamarin.Forms.Maps.MapSpan.Center), представляющий [`Position`](xref:Xamarin.Forms.Maps.Position) в географическом центре `MapSpan`.
- [`LatitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LatitudeDegrees), представляющий градусы широты, охваченные `MapSpan`.
- [`LongitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LongitudeDegrees), представляющий градусы долготы, охваченные `MapSpan`.
- [`Radius`](xref:Xamarin.Forms.Maps.MapSpan.Radius), представляющий `MapSpan` RADIUS.

## <a name="move-the-map"></a>Перемещение схемы

Метод [`Map.MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) может быть вызван для изменения расположения и масштаба на карте. Этот метод принимает [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) аргумент, определяющий область отображаемой схемы и ее масштаб.

В следующем коде показан пример перемещения отображаемой области на карте:

```csharp
MapSpan mapSpan = MapSpan.FromCenterAndRadius(position, Distance.FromKilometers(0.444));
map.MoveToRegion(mapSpan);
```

## <a name="zoom-the-map"></a>Изменить масштаб схемы

Уровень масштаба [`Map`](xref:Xamarin.Forms.Maps.Map) можно изменить без изменения его расположения. Это можно сделать с помощью пользовательского интерфейса Map или программным путем, вызвав метод [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) с аргументом [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) , который использует текущее расположение в качестве аргумента [`Position`](xref:Xamarin.Forms.Maps.Position) :

```csharp
double zoomLevel = 0.5;
double latlongDegrees = 360 / (Math.Pow(2, zoomLevel));
if (map.VisibleRegion != null)
{
    map.MoveToRegion(new MapSpan(map.VisibleRegion.Center, latlongDegrees, latlongDegrees));
}
```

В этом примере метод [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) вызывается с аргументом [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) , который задает текущее расположение map, через свойство [`Map.VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion) и уровень масштабирования в градусах широты и долготы. Общий результат заключается в том, что уровень масштабирования в карте изменяется, но его расположение — нет. Альтернативный подход к реализации масштабирования на карте заключается в использовании метода [`MapSpan.WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom*) для управления коэффициентом масштабирования.

> [!IMPORTANT]
> При изменении масштаба схемы (с помощью пользовательского интерфейса Map или программным путем) требуется, чтобы свойство [`Map.HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) было `true`. Дополнительные сведения об этом свойстве см. в разделе [Отключение масштабирования](#disable-zoom).

## <a name="customize-map-behavior"></a>Настройка поведения карт

Поведение [`Map`](xref:Xamarin.Forms.Maps.Map) может быть настроено путем установки некоторых его свойств и обработки события `MapClicked`.

> [!NOTE]
> Дополнительное поведение карт кустомизатин можно достичь, создав пользовательский модуль подготовки отчетов. Дополнительные сведения см. в разделе [Настройка Map в Xamarin. Forms](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).

### <a name="disable-scroll"></a>Отключить прокрутку

Класс [`Map`](xref:Xamarin.Forms.Maps.Map) определяет свойство [`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled) типа `bool`. По умолчанию это свойство имеет значение `true`, которое указывает, что карте разрешено прокручивать. Если это свойство имеет значение `false`, то эта схема не прокручивается. В следующем примере показано задание этого свойства:

```xaml
<maps:Map HasScrollEnabled="false" />
```

Эквивалентный код на C# выглядит так:

```csharp
Map map = new Map
{
    HasScrollEnabled = false
};
```

### <a name="disable-zoom"></a>Отключить масштаб

Класс [`Map`](xref:Xamarin.Forms.Maps.Map) определяет свойство [`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) типа `bool`. По умолчанию это свойство имеет значение `true`, которое указывает на то, что на карте можно выполнить масштаб. Если это свойство имеет значение `false`, то карту невозможно изменить. В следующем примере показано задание этого свойства:

```xaml
<maps:Map HasZoomEnabled="false" />
```

Эквивалентный код на C# выглядит так:

```csharp
Map map = new Map
{
    HasZoomEnabled = false
};
```

### <a name="show-the-users-location"></a>Отображение расположения пользователя

Класс [`Map`](xref:Xamarin.Forms.Maps.Map) определяет свойство [`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser) типа `bool`. По умолчанию это свойство имеет значение `false`, которое указывает, что на карте не отображается текущее расположение пользователя. Если это свойство имеет значение `true`, то на карте отображается текущее расположение пользователя. В следующем примере показано задание этого свойства:

```xaml
<maps:Map IsShowingUser="true" />
```

Эквивалентный код на C# выглядит так:

```csharp
Map map = new Map
{
    IsShowingUser = true
};
```

> [!IMPORTANT]
> В iOS, Android и универсальная платформа Windows для доступа к расположению пользователя требуются разрешения на расположение, предоставленные приложению. Дополнительные сведения см. в разделе [Конфигурация платформы](setup.md#platform-configuration).

### <a name="maintain-map-region-on-layout-change"></a>Сохранить регион на карте при изменении макета

Класс [`Map`](xref:Xamarin.Forms.Maps.Map) определяет свойство `MoveToLastRegionOnLayoutChange` типа `bool`. По умолчанию это свойство имеет значение `true`, которое указывает, что отображаемая область отображения будет перемещена из текущей области в ее ранее заданную область при изменении макета, например при смене устройства. Если для этого свойства задано значение `false`, отображаемая область отображения останется в центре при изменении макета. В следующем примере показано задание этого свойства:

```xaml
<maps:Map MoveToLastRegionOnLayoutChange="false" />
```

Эквивалентный код на C# выглядит так:

```csharp
Map map = new Map
{
    MoveToLastRegionOnLayoutChange = false
};
```

### <a name="map-clicks"></a>Кнопки карт

Класс [`Map`](xref:Xamarin.Forms.Maps.Map) определяет событие `MapClicked`, возникающее при нажатии на карту. Объект `MapClickedEventArgs`, сопровождающий событие, имеет одно свойство с именем `Position`типа [`Position`](xref:Xamarin.Forms.Maps.Position). При срабатывании события для свойства `Position` задается выбранное расположение на карте. Сведения о структуре [`Position`](xref:Xamarin.Forms.Maps.Position) см. в разделе [Map Disposition and Distance](position-distance.md).

В следующем примере кода показан обработчик событий для события `MapClicked`:

```csharp
void OnMapClicked(object sender, MapClickedEventArgs e)
{
    System.Diagnostics.Debug.WriteLine($"MapClick: {e.Position.Latitude}, {e.Position.Longitude}");
}
```

В этом примере обработчик событий `OnMapClicked` выводит широту и долготу, представляющую расположение касания. Обработчик событий можно зарегистрировать с помощью события `MapClicked` следующим образом:

```xaml
<maps:Map MapClicked="OnMapClicked" />
```

Эквивалентный код на C# выглядит так:

```csharp
Map map = new Map();
map.MapClicked += OnMapClicked;
```

## <a name="related-links"></a>Связанные ссылки

- [Пример Maps](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Расположение и расстояние на карте](position-distance.md)
- [Настройка схемы Xamarin. Forms](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Передача аргументов в XAML](~/xamarin-forms/xaml/passing-arguments.md)
