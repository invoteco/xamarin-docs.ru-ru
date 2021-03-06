---
title: Преобразователи цвета в Xamarin.Essentials
description: Класс ColorConverters в Xamarin.Essentials предоставляет несколько вспомогательных методов и методов расширения для работы с System.Drawing.Color.
ms.assetid: B10428D6-89E2-4714-A39F-7E6E626391B2
author: jamesmontemagno
ms.author: jamont
ms.date: 01/06/2020
ms.openlocfilehash: 5d64967dfaa6ce7ef746a97f739cac67f5102fc2
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "77545163"
---
# <a name="xamarinessentials-color-converters"></a>Xamarin.Essentials: Преобразователи цвета

Класс **ColorConverters** в Xamarin.Essentials предоставляет несколько вспомогательных методов для System.Drawing.Color.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-color-converters"></a>Использование преобразователей цвета

Добавьте в свой класс ссылку на Xamarin.Essentials:

```csharp
using Xamarin.Essentials;
```

При работе с `System.Drawing.Color` можно использовать встроенные преобразователи Xamarin.Forms для создания цвета из Hsl, Hex или UInt.

```csharp
var blueHex = ColorConverters.FromHex("#3498db");
var blueHsl = ColorConverters.FromHsl(204, 70, 53);
var blueUInt = ColorConverters.FromUInt(3447003);
```

## <a name="using-color-extensions"></a>Использование расширений цвета

Методы расширения на `System.Drawing.Color` позволяют применять разные свойства:

```csharp
var blue = ColorConverters.FromHex("#3498db");

// Multiplies the current alpha by 50%
var blueWithAlpha = blue.MultiplyAlpha(.5f);
```

Существует несколько других методов расширения, в том числе:

- GetComplementary
- MultiplyAlpha
- ToUInt
- WithAlpha
- WithHue
- WithLuminosity
- WithSaturation

## <a name="using-platform-extensions"></a>Использование расширений платформы

Кроме того, можно преобразовать System.Drawing.Color в структуре цвета для определенной платформы. Эти методы могут вызываться только из проектов iOS, Android и UWP.

```csharp
var system = System.Drawing.Color.FromArgb(255, 52, 152, 219);

// Extension to convert to Android.Graphics.Color, UIKit.UIColor, or Windows.UI.Color
var platform = system.ToPlatformColor();
```

```csharp
var platform = new Android.Graphics.Color(52, 152, 219, 255);

// Back to System.Drawing.Color
var system = platform.ToSystemColor();
```

Метод `ToSystemColor` применим к Android.Graphics.Color UIKit.UIColor и Windows.UI.Color.

## <a name="api"></a>API

- [Исходный код преобразователей цвета](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/ColorConverters.shared.cs)
- [Документация по API преобразователей цвета](xref:Xamarin.Essentials.ColorConverters)
- [Исходный код расширений цвета](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/ColorConverters.shared.cs)
- [Документация по API расширений цвета](xref:Xamarin.Essentials.ColorExtensions)
