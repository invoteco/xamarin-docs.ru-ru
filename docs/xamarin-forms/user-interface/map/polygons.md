---
title: Геомногоугольники и ломаные линии на карте Xamarin. Forms
description: В этой статье объясняется, как создать многоугольники и ломаные линии на экземпляре Map на карте Xamarin. Forms.
ms.prod: xamarin
ms.assetid: CDAF0B02-1AA8-4AD6-94A7-ABFC18006A2D
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 09/20/2019
ms.openlocfilehash: 42c00a060e0477aff4ea02f6213fa751b2adf4dd
ms.sourcegitcommit: 5c22097bed2a8d51ecaf6ca197bf4d449dfe1377
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2019
ms.locfileid: "72810495"
---
# <a name="xamarinforms-map-polygons-and-polylines"></a>Геомногоугольники и ломаные линии на карте Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

[!["демонстрация многоугольников и ломаных линий в iOS и Android"](polygons-images/polygon-app-cropped.png)](polygons-images/polygon-app.png#lightbox)

элементы `Polygon` и `Polyline` позволяют выделять определенные области на карте. @No__t_0 является полностью замкнутой фигурой, которая может иметь цвет обводки и заливки. @No__t_0 — это линия, которая не полностью заключает в себя область.

> [!NOTE]
> Примеры `Polygon` и `Polyline` находятся в **полигонспаже** в примере проекта.

Классы `Polygon` и `Polyline` являются производными от `MapElement`, которые предоставляют следующие свойства [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) :

- `StrokeColor` — это свойство `Color`, определяющее цвет линии.
- `StrokeWidth` — это `float` свойство, определяющее толщину линии.
- `Geopath` определяется как в `Polygon`, так и в `Polyline` и представляет собой список объектов [`Position`](xref:Xamarin.Forms.Maps.Position) , указывающих точки фигуры.

Класс `Polygon` определяет дополнительное свойство:

- `FillColor` — это свойство `Color`, определяющее цвет фона многоугольника.

> [!NOTE]
> Если свойство `StrokeColor` не задано, штрих будет по умолчанию черным. Если свойство `FillColor` не задано, заливка по умолчанию будет прозрачной. Таким образом, если ни одно из свойств не указано, фигура будет иметь черный контур без заливки.

## <a name="create-a-polygon"></a>Создание многоугольника

Объект `Polygon` можно добавить в карту, создав его экземпляр и добавив его в коллекцию `MapElements` Map. Это можно сделать в XAML следующим образом:

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
     <maps:Map>
         <maps:Map.MapElements>
             <maps:Polygon StrokeColor="#FF9900"
                           StrokeWidth="8"
                           FillColor="#88FF9900">
                 <maps:Polygon.Geopath>
                     <maps:Position>
                         <x:Arguments>
                             <x:Double>47.6368678</x:Double>
                             <x:Double>-122.137305</x:Double>
                         </x:Arguments>
                     </maps:Position>
                     ...
                 </maps:Polygon.Geopath>
             </maps:Polygon>
         </maps:Map.MapElements>
     </maps:Map>
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
using Xamarin.Forms.Maps;
// ...
Map map = new Map
{
  // ...
};

// instantiate a polygon
Polygon polygon = new Polygon
{
    StrokeWidth = 8,
    StrokeColor = Color.FromHex("#1BA1E2"),
    FillColor = Color.FromHex("#881BA1E2"),
    Geopath =
    {
        new Position(47.6368678, -122.137305),
        new Position(47.6368894, -122.134655),
        new Position(47.6359424, -122.134655),
        new Position(47.6359496, -122.1325521),
        new Position(47.6424124, -122.1325199),
        new Position(47.642463,  -122.1338932),
        new Position(47.6406414, -122.1344833),
        new Position(47.6384943, -122.1361248),
        new Position(47.6372943, -122.1376912)
    }
};

// add the polygon to the map's MapElements collection
map.MapElements.Add(polygon);
```

Свойства `StrokeColor` и `StrokeWidth` указываются для настройки контура многоугольника. Значение свойства `FillColor` соответствует значению свойства `StrokeColor`, но имеет заданное значение альфа, чтобы сделать его прозрачным, позволяя отображать базовую карту через фигуру. Свойство `GeoPath` содержит список объектов `Position`, определяющих географические координаты точек многоугольника. @No__t_0 объект отображается на карте после добавления в коллекцию `MapElements` `Map`.

> [!NOTE]
> @No__t_0 является полностью замкнутой фигурой. Первая и последняя точки будут автоматически подключены, если они не совпадают.

## <a name="create-a-polyline"></a>Создание ломаной линии

Объект `Polyline` можно добавить в карту, создав его экземпляр и добавив его в коллекцию `MapElements` Map. Это можно сделать в XAML следующим образом:

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
     <maps:Map>
         <maps:Map.MapElements>
             <maps:Polyline StrokeColor="Blue"
                            StrokeWidth="12">
                 <maps:Polyline.Geopath>
                     <maps:Position>
                         <x:Arguments>
                             <x:Double>47.6381401</x:Double>
                             <x:Double>-122.1317367</x:Double>
                         </x:Arguments>
                     </maps:Position>
                     ...
                 </maps:Polyline.Geopath>
             </maps:Polyline>
         </maps:Map.MapElements>
     </maps:Map>
</ContentPage>
```

```csharp
using Xamarin.Forms.Maps;
// ...
Map map = new Map
{
  // ...
};
// instantiate a polyline
Polyline polyline = new Polyline
{
    StrokeColor = Color.Blue,
    StrokeWidth = 12,
    Geopath =
    {
        new Position(47.6381401, -122.1317367),
        new Position(47.6381473, -122.1350841),
        new Position(47.6382847, -122.1353094),
        new Position(47.6384582, -122.1354703),
        new Position(47.6401136, -122.1360819),
        new Position(47.6403883, -122.1364681),
        new Position(47.6407426, -122.1377019),
        new Position(47.6412558, -122.1404056),
        new Position(47.6414148, -122.1418647),
        new Position(47.6414654, -122.1432702)
    }
};

// add the polyline to the map's MapElements collection
map.MapElements.Add(polyline);
```

Свойства `StrokeColor` и `StrokeWidth` указываются для настройки линии. Свойство `GeoPath` содержит список объектов `Position`, определяющих географические координаты точек ломаной линии. @No__t_0 объект отображается на карте после добавления в коллекцию `MapElements` `Map`.

## <a name="related-links"></a>Связанные ссылки

- [Пример Maps](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
