---
title: Цвета в Xamarin.Forms
description: Xamarin.Forms предоставляет гибкий класс кросс платформенных цвет. В этой статье содержатся сведения о функциях, предоставляемых класса цвета и способы ее использования.
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2019
ms.openlocfilehash: 2a17b037803d1ca6e54000ea7ba3f05c8ce6034f
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305759"
---
# <a name="colors-in-xamarinforms"></a>Цвета в Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)

_Xamarin. Forms предоставляет гибкий класс цветового кросс-платформенного цвета._

В этой статье описаны различные способы использования класса [`Color`](xref:Xamarin.Forms.Color) в Xamarin. Forms.

Класс [`Color`](xref:Xamarin.Forms.Color) предоставляет ряд методов для создания экземпляра цвета:

- **Именованные цвета** — коллекция общих именованных цветов, включая `Red`, `Green`и `Blue`.
- **Фромхекс** — строковое значение, аналогичное синтаксису, используемому в HTML, например "00FF00". При необходимости в качестве первой пары символов ("CC00FF00") можно указать альфа-канал.
- **Фромхсла** — оттенок, насыщенность и яркость `double` значения с необязательным альфа-значением (0,0 – 1,0).
- **Фромргб** — красный, зеленый и синий `int` значения (0-255).
- **Фромргба** — красные, зеленые, синие и альфа-`int` значения (0-255).
- **Фромуинт** — задает одно значение `double`, представляющее **ARGB**.

Ниже приведено несколько примеров цветов, присвоенных `BackgroundColor`у некоторых меток с использованием различных вариантов разрешенного синтаксиса:

```csharp
var red    = new Label { Text = "Red",   BackgroundColor = Color.Red };
var orange = new Label { Text = "Orange",BackgroundColor = Color.FromHex("FF6A00") };
var yellow = new Label { Text = "Yellow",BackgroundColor = Color.FromHsla(0.167, 1.0, 0.5, 1.0) };
var green  = new Label { Text = "Green", BackgroundColor = Color.FromRgb (38, 127, 0) };
var blue   = new Label { Text = "Blue",  BackgroundColor = Color.FromRgba(0, 38, 255, 255) };
var indigo = new Label { Text = "Indigo",BackgroundColor = Color.FromRgb (0, 72, 255) };
var violet = new Label { Text = "Violet",BackgroundColor = Color.FromHsla(0.82, 1, 0.25, 1) };

var transparent = new Label { Text = "Transparent",BackgroundColor = Color.Transparent };
var @default = new Label    { Text = "Default",    BackgroundColor = Color.Default };
var accent = new Label      { Text = "Accent",     BackgroundColor = Color.Accent };
```

Эти цвета отображаются на каждой из платформ. Обратите внимание, что окончательный цвет — `Accent` — это синий длинных цвет для iOS и Android; Это значение определяется Xamarin. Forms.

 [![Демонстрационный цвет](colors-images/colors-sml.png "Демонстрационный цвет")](colors-images/colors.png#lightbox "Демонстрационный цвет")

## <a name="colordefault"></a>Color.Default

Используйте `Default`, чтобы установить (или восстановить) значение цвета обратно на платформу по умолчанию (понимание того, что оно представляет отдельный основной цвет на каждой платформе для каждого свойства).

Разработчики могут использовать это значение для задания `Color` свойства, но **не** должны запрашивать этот экземпляр для его значений RGB (все они имеют значение-1).

## <a name="colortransparent"></a>Color.Transparent

Задайте цвет для очистки.

## <a name="coloraccent"></a>Color.Accent

В iOS и Android контрастный цвет, который является видимым в фоновом режиме по умолчанию, но не так же, как цвета текста по умолчанию имеет значение данного экземпляра.

## <a name="additional-methods"></a>Дополнительные методы

[`Color`](xref:Xamarin.Forms.Color) экземпляры включают следующие дополнительные методы:

- **Аддлуминосити** — возвращает `Color`, изменяя яркость на заданную разницу.
- **Мултиплялфа** — возвращает `Color`, изменяя альфа-канал, умножая его на заданное альфа-значение.
- **Тохекс** — возвращает шестнадцатеричное `string` представление `Color`.
- **Висхуе** — возвращает `Color`, заменяя оттенок на заданное значение.
- **Вислуминосити** — возвращает `Color`, заменяя яркость на предоставляемое значение.
- **Виссатуратион** — возвращает `Color`, заменяя насыщенность заданным значением.

## <a name="implicit-conversions"></a>Неявные преобразования

Неявное преобразование между типами `Xamarin.Forms.Color` и `System.Drawing.Color` может быть выполнено:

```csharp
Xamarin.Forms.Color xfColor = Xamarin.Forms.Color.FromRgb(0, 72, 255);
System.Drawing.Color sdColor = System.Drawing.Color.FromArgb(38, 127, 0);

// Implicity convert from a Xamarin.Forms.Color to a System.Drawing.Color
System.Drawing.Color sdColor2 = xfColor;

// Implicitly convert from a System.Drawing.Color to a Xamarin.Forms.Color
Xamarin.Forms.Color xfColor2 = sdColor;
```

## <a name="deviceruntimeplatform"></a>Device.RuntimePlatform

В этом фрагменте кода используется свойство `Device.RuntimePlatform` для выборочного задания цвета `ActivityIndicator`.

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator
{
    Color = Device.RuntimePlatform == Device.iOS ? Color.Black : Color.Default,
    IsRunning = true
};
```

## <a name="using-from-xaml"></a>С помощью из XAML

К цветам также можно обращаться в XAML, используя определенные имена цветов или шестнадцатеричные представления, показанные ниже:

```xaml
<Label Text="Sea color" BackgroundColor="Aqua" />
<Label Text="RGB" BackgroundColor="#00FF00" />
<Label Text="Alpha plus RGB" BackgroundColor="#CC00FF00" />
<Label Text="Tiny RGB" BackgroundColor="#0F0" />
<Label Text="Tiny Alpha plus RGB" BackgroundColor="#C0F0" />
```

> [!NOTE]
> При использовании компиляции XAML, названия цветов нечувствительны к регистру и таким образом, могут быть записаны в нижнем регистре. Дополнительные сведения о компиляции XAML см. в статье [Компиляция XAML](~/xamarin-forms/xaml/xamlc.md).

## <a name="related-links"></a>Связанные ссылки

- [колорссампле](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)
- [Средство выбора с возможностью привязки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablepicker)
