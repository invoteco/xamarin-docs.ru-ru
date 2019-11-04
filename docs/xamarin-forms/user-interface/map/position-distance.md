---
title: Расположение и расстояние карт Xamarin. Forms
description: Пространство имен Xamarin. Forms. Maps содержит структуру Position, которая обычно используется при размещении карты и ее ПИН-кодов, а также структуры расстояния, которая при необходимости может использоваться при размещении карты.
ms.prod: xamarin
ms.assetid: 2F4EA3D2-1351-40AD-A71D-CF7F1F18F1E8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/30/2019
ms.openlocfilehash: a68eab7bb3e6da764f5a35af4461d6af2be50ebe
ms.sourcegitcommit: 3ea19e3a51515b30349d03c70a5b3acd7eca7fe7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/01/2019
ms.locfileid: "73426285"
---
# <a name="xamarinforms-map-position-and-distance"></a>Расположение и расстояние карт Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Пространство имен [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) содержит структуру [`Position`](xref:Xamarin.Forms.Maps.Position) , которая обычно используется при размещении схемы и ее ПИН-кодов, а также структуры [`Distance`](xref:Xamarin.Forms.Maps.Distance) , которую при необходимости можно использовать при размещении схемы.

## <a name="position"></a>Положение

Структура [`Position`](xref:Xamarin.Forms.Maps.Position) инкапсулирует расположение, хранящееся в виде значений широты и долготы. Эта структура определяет два свойства только для чтения:

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude), типа `double`, представляющего широту расположения в десятичных градусах.
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude), типа `double`, представляющего долготу расположения в десятичных градусах.

[`Position`](xref:Xamarin.Forms.Maps.Position) объекты создаются с помощью конструктора `Position`, для которого требуются аргументы широты и долготы, указанные в качестве `double` значений:

```csharp
Position position = new Position(36.9628066, -122.0194722);
```

> [!NOTE]
> При создании объекта `Position` значение широты будет относиться между-90,0 и 90,0, а значение долготы будет относиться между-180,0 и 180,0.

## <a name="distance"></a>Друг

Структура [`Distance`](xref:Xamarin.Forms.Maps.Distance) инкапсулирует расстояние, хранящееся в виде `double` значения, представляющего расстояние в метрах. Эта структура определяет три свойства только для чтения:

- [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers), типа `double`, который представляет расстояние в километрах, занимаемое `Distance`.
- [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters)типа `double`, который представляет расстояние в метрах, занимаемое `Distance`.
- [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles)типа `double`, который представляет расстояние в милях, занимаемое `Distance`.

[`Distance`](xref:Xamarin.Forms.Maps.Distance) объекты можно создать с помощью конструктора `Distance`, для которого требуется аргумент метрах, заданный в качестве `double`:

```csharp
Distance distance = new Distance(1450.5);
```

Кроме того, [`Distance`](xref:Xamarin.Forms.Maps.Distance) объекты можно создавать с помощью методов фабрики [`FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers*), [`FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters*)и [`FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles*) :

```csharp
Distance distance1 = Distance.FromKilometers(1.45); // argument represents the number of kilometers
Distance distance2 = Distance.FromMeters(1450.5);   // argument represents the number of meters
Distance distance3 = Distance.FromMiles(0.969);     // argument represents the number of miles
```

## <a name="related-links"></a>Связанные ссылки

- [Пример Maps](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
