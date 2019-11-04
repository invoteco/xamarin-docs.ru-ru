---
title: Схема Xamarin. Forms
description: Элемент управления картой отображает карту и требует пакет NuGet Xamarin. Forms. Maps.
ms.prod: xamarin
ms.assetid: B669B5EE-D24C-4C69-93E1-2CA5CC9108B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/29/2019
ms.openlocfilehash: 013e126b76de08442327707cd0502f207826dad8
ms.sourcegitcommit: 3ea19e3a51515b30349d03c70a5b3acd7eca7fe7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/01/2019
ms.locfileid: "73425597"
---
# <a name="xamarinforms-map"></a>Схема Xamarin. Forms

## <a name="initialization-and-configurationsetupmd"></a>[Инициализация и настройка](setup.md)

Для использования функций карт в приложении требуется пакет NuGet [Xamarin. Forms. Maps](https://www.nuget.org/packages/Xamarin.Forms.Maps/) . Кроме того, для доступа к расположению пользователя требуются разрешения на расположение, предоставленные приложению.

## <a name="map-controlmapmd"></a>[Map Control](map.md)

Элемент управления [`Map`](xref:Xamarin.Forms.Maps.Map) представляет собой кросс-платформенное представление для отображения и аннотирования карт. Он использует собственный элемент управления картой для каждой платформы, обеспечивая быстрый и знакомый интерфейс карт для пользователей.

## <a name="position-and-distanceposition-distancemd"></a>[Расположение и расстояние](position-distance.md)

Структура [`Position`](xref:Xamarin.Forms.Maps.Position) обычно используется при размещении схемы и ее ПИН-кодов, а также структуры [`Distance`](xref:Xamarin.Forms.Maps.Distance) , которая при необходимости может быть использована при размещении схемы.

## <a name="pinspinsmd"></a>[Закрепления](pins.md)

Элемент управления [`Map`](xref:Xamarin.Forms.Maps.Map) позволяет помечать расположения [`Pin`](xref:Xamarin.Forms.Maps.Pin) объектами. `Pin` — это маркер Map, открывающий информационное окно при касании.

## <a name="polygons-and-polylinespolygonsmd"></a>[Многоугольники и ломаные линии](polygons.md)

элементы `Polygon` и `Polyline` позволяют выделять определенные области на карте. `Polygon` является полностью замкнутой фигурой, которая может иметь цвет обводки и заливки. `Polyline` — это линия, которая не полностью заключает в себя область.

## <a name="geocodinggeocodermd"></a>[Геокодирование](geocoder.md)

Класс [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) выполняет преобразование между строковыми адресами и координатами широты и долготы, хранящимися в [`Position`](xref:Xamarin.Forms.Maps.Position) объектах.

## <a name="launch-the-native-map-appnative-map-appmd"></a>[Запуск собственного приложения для карт](native-map-app.md)

Собственное приложение Map на каждой платформе можно запустить из приложения Xamarin. Forms с помощью `Launcher` класса Xamarin. Essentials.
