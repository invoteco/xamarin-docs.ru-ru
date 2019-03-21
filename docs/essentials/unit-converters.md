---
title: Преобразователи единиц в Xamarin.Essentials
description: Класс UnitConverters в Xamarin.Essentials предоставляет несколько преобразователей единиц, чтобы помочь разработчикам при использовании Xamarin.Essentials.
ms.assetid: 35DE2704-E730-4337-9476-66CD53376943
author: jamesmontemagno
ms.author: jamont
ms.date: 03/13/2019
ms.openlocfilehash: 23126359c3b5e1c7e3562177b82f12596d2893a4
ms.sourcegitcommit: 64d6da88bb6ba222ab2decd2fdc8e95d377438a6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/18/2019
ms.locfileid: "58176077"
---
# <a name="xamarinessentials-unit-converters"></a>Xamarin.Essentials: Преобразователи единиц

Класс **UnitConverters** предоставляет несколько преобразователей единиц, чтобы помочь разработчикам при использовании Xamarin.Essentials.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-unit-converters"></a>Использование преобразователей единиц

Добавьте в свой класс ссылку на Xamarin.Essentials:

```csharp
using Xamarin.Essentials;
```

Все преобразователи единиц доступны с помощью статического класса `UnitConverters` в Xamarin.Essentials. Например, можно легко преобразовать градусы Фаренгейта в градусы Цельсия.

```csharp
var celcius = UnitConverters.FahrenheitToCelsius(32.0);
```

Ниже приведен список доступных преобразований:

* FahrenheitToCelsius
* CelsiusToFahrenheit
* CelsiusToKelvin
* KelvinToCelsius
* MilesToMeters
* MilesToMeters
* KilometersToMiles
* DegreesToRadians
* RadiansToDegrees
* DegreesPerSecondToRadiansPerSecond
* RadiansPerSecondToDegreesPerSecond
* DegreesPerSecondToHertz
* DegreesPerSecondToHertz
* HertzToDegreesPerSecond
* HertzToDegreesPerSecond
* KilopascalsToHectopascals
* HectopascalsToKilopascals
* KilopascalsToHectopascals
* HectopascalsToKilopascals
* AtmospheresToPascals
* PascalsToAtmospheres
* CoordinatesToMiles
* CoordinatesToMiles

## <a name="api"></a>API

- [Исходный код преобразователей единиц](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/UnitConverters.shared.cs)
- [Документация по API преобразователей единиц](xref:Xamarin.Essentials.UnitConverters)
