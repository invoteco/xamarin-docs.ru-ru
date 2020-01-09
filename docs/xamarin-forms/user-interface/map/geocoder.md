---
title: Геокодирование карт Xamarin. Forms
description: В этой статье объясняется, как выполнять геокод и реверсировать данные карты геокодирования с помощью класса геокодирования Xamarin. Forms. Maps.
ms.prod: xamarin
ms.assetid: DE7DB31A-8921-4614-8B49-DAEF1E7B03B3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/22/2019
ms.openlocfilehash: 6df7a2e0056cd2d448206b56e1e9d2600b342b3b
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75490263"
---
# <a name="xamarinforms-map-geocoding"></a>Геокодирование карт Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Пространство имен [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) предоставляет класс [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) , который выполняет преобразование между строковыми адресами и координатами широты и долготы, хранящимися в [`Position`](xref:Xamarin.Forms.Maps.Position) объектах. Дополнительные сведения о структуре [`Position`](xref:Xamarin.Forms.Maps.Position) см. в разделе [Map Disposition and Distance](position-distance.md).

## <a name="geocode-an-address"></a>Геокодирование адреса

Адрес улицы может быть геокодирован в координаты широты и долготы путем создания экземпляра [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) и вызова метода [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*) в экземпляре `Geocoder`:

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder = new Geocoder();

IEnumerable<Position> approximateLocations = await geoCoder.GetPositionsForAddressAsync("Pacific Ave, San Francisco, California");
Position position = approximateLocations.FirstOrDefault();
string coordinates = $"{position.Latitude}, {position.Longitude}";
```

Метод [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*) принимает `string` аргумент, представляющий адрес, и асинхронно Возвращает коллекцию объектов [`Position`](xref:Xamarin.Forms.Maps.Position) , которые могут представлять адрес.

## <a name="reverse-geocode-an-address"></a>Обратный геокодировании адреса

Координаты широты и долготы могут быть обратными геокодированными в адрес улицы путем создания [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) экземпляра и вызова метода [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*) в экземпляре `Geocoder`:

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder = new Geocoder();

Position position = new Position(37.8044866, -122.4324132);
IEnumerable<string> possibleAddresses = await geoCoder.GetAddressesForPositionAsync(position);
string address = possibleAddresses.FirstOrDefault();
```

Метод [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*) принимает [`Position`](xref:Xamarin.Forms.Maps.Position) аргумент, состоящий из координат широты и долготы, и асинхронно Возвращает коллекцию строк, представляющих адреса, расположенные рядом с позицией.

## <a name="related-links"></a>Связанные ссылки

- [Пример Maps](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Расположение и расстояние карт Xamarin. Forms](position-distance.md)
- [API-интерфейс геокодирования](xref:Xamarin.Forms.Maps.Geocoder)
