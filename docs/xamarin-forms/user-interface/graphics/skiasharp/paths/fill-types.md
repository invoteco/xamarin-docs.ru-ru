---
title: Типы заполнения пути
description: В этой статье рассматриваются различные возможные эффекты с типы заполнения пути SkiaSharp и демонстрирует это с помощью примера кода.
ms.prod: xamarin
ms.assetid: 57103A7A-49A2-46AE-894C-7C2664682644
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 98081ed1a9aef1260150671d4fd026dd64c20b62
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723643"
---
# <a name="the-path-fill-types"></a>Типы заполнения пути

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Узнайте о различных эффектах, возможных для типов заливки пути SkiaSharp_

Возможно перекрытие двух контуров в пути и строки, которые составляют единый профиль могут перекрывать друг друга. Любой замкнутой области потенциально может быть заполнен, но может не потребоваться заполнить все замкнутые области. Ниже приведен пример:

![](fill-types-images/filltypeexample.png "Five-pointed star partially filles")

Вы располагаете полным контролем над это. Алгоритм заполнения регулируется свойством [`SKFillType`](xref:SkiaSharp.SKPath.FillType) `SKPath`, которое задается членом перечисления [`SKPathFillType`](xref:SkiaSharp.SKPathFillType) :

- `Winding`, значение по умолчанию
- `EvenOdd`
- `InverseWinding`
- `InverseEvenOdd`

Поворота и четный алгоритмы определить, является любой замкнутой области заполнения или не заполнено на основании гипотетической линию от этой области до бесконечности. Эту строку пересекает одну или несколько границ строк, составляющих путь. В режиме поворота Если число границ линий, нарисованных в одном направлении баланс out количество линий, рисуемых в обратном направлении, а затем области не заполнены. В противном случае область заполняется. Алгоритм четный Заливка области, если нечетное число границ строк.

С помощью многих стандартных путей поворота алгоритма часто заполняет все замкнутые области пути. Алгоритм четный обычно получаются более интересные результаты.

Классический пример — это 5-направленная звезда, как показано на **5-** элементной странице звезды. Файл [**фивепоинтедстарпаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/FivePointedStarPage.xaml) создает два представления `Picker` для выбора типа заливки пути, а также указывает, является ли контур обводкым или заполненным или обоими, и в каком порядке:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Paths.FivePointedStarPage"
             Title="Five-Pointed Star">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Picker x:Name="fillTypePicker"
                Title="Path Fill Type"
                Grid.Row="0"
                Grid.Column="0"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKPathFillType}">
                    <x:Static Member="skia:SKPathFillType.Winding" />
                    <x:Static Member="skia:SKPathFillType.EvenOdd" />
                    <x:Static Member="skia:SKPathFillType.InverseWinding" />
                    <x:Static Member="skia:SKPathFillType.InverseEvenOdd" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="drawingModePicker"
                Title="Drawing Mode"
                Grid.Row="0"
                Grid.Column="1"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>Fill only</x:String>
                    <x:String>Stroke only</x:String>
                    <x:String>Stroke then Fill</x:String>
                    <x:String>Fill then Stroke</x:String>
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skiaforms:SKCanvasView x:Name="canvasView"
                                Grid.Row="1"
                                Grid.Column="0"
                                Grid.ColumnSpan="2"
                                PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Файл кода программной части использует оба значения `Picker`, чтобы нарисовать пять звезд:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = 0.45f * Math.Min(info.Width, info.Height);

    SKPath path = new SKPath
    {
        FillType = (SKPathFillType)fillTypePicker.SelectedItem
    };
    path.MoveTo(info.Width / 2, info.Height / 2 - radius);

    for (int i = 1; i < 5; i++)
    {
        // angle from vertical
        double angle = i * 4 * Math.PI / 5;
        path.LineTo(center + new SKPoint(radius * (float)Math.Sin(angle),
                                        -radius * (float)Math.Cos(angle)));
    }
    path.Close();

    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 50,
        StrokeJoin = SKStrokeJoin.Round
    };

    SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue
    };

    switch ((string)drawingModePicker.SelectedItem)
    {
        case "Fill only":
            canvas.DrawPath(path, fillPaint);
            break;

        case "Stroke only":
            canvas.DrawPath(path, strokePaint);
            break;

        case "Stroke then Fill":
            canvas.DrawPath(path, strokePaint);
            canvas.DrawPath(path, fillPaint);
            break;

        case "Fill then Stroke":
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, strokePaint);
            break;
    }
}
```

Как правило, тип заливки пути должен влиять только на заливки и не штрихи, но два режима `Inverse` влияют как на заливку, так и на штрихи. Для заливок два типа `Inverse` заполняют области в обратном направлении, чтобы заполнить область за пределами звезды. Для штрихов в двух типах `Inverse` цвета все, кроме штриха. Использование этих типов обратный заполнения может привести некоторые нечетное эффекты, как показано на снимке экрана iOS:

[![](fill-types-images/fivepointedstar-small.png "Triple screenshot of the Five-Pointed Star page")](fill-types-images/fivepointedstar-large.png#lightbox "Triple screenshot of the Five-Pointed Star page")

На снимке экрана Android показаны типичные эффекты «четный-нечетный» и «обмотка», но порядок штрихов и заливки также влияет на результаты.

Алгоритм поворота зависит от направления, что линии. Обычно при создании пути, вы можете управлять этом направлении, как указано, что линии из одной точки в другую. Однако класс `SKPath` также определяет такие методы, как `AddRect` и `AddCircle`, рисующие целые контуры. Для управления обрисовкой этих объектов методы включают параметр типа [`SKPathDirection`](xref:SkiaSharp.SKPathDirection), который имеет два члена:

- `Clockwise`
- `CounterClockwise`

Методы в `SKPath`, которые включают параметр `SKPathDirection`, присвойте ему значение по умолчанию `Clockwise`.

На **перекрывающейся странице с кружками** создается контур с четырьмя перекрывающимися круговыми типами заливки пути с четными нечетными путями:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = Math.Min(info.Width, info.Height) / 4;

    SKPath path = new SKPath
    {
        FillType = SKPathFillType.EvenOdd
    };

    path.AddCircle(center.X - radius / 2, center.Y - radius / 2, radius);
    path.AddCircle(center.X - radius / 2, center.Y + radius / 2, radius);
    path.AddCircle(center.X + radius / 2, center.Y - radius / 2, radius);
    path.AddCircle(center.X + radius / 2, center.Y + radius / 2, radius);

    SKPaint paint = new SKPaint()
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Cyan
    };

    canvas.DrawPath(path, paint);

    paint.Style = SKPaintStyle.Stroke;
    paint.StrokeWidth = 10;
    paint.Color = SKColors.Magenta;

    canvas.DrawPath(path, paint);
}
```

Это интересный образ создается с минимальным объемом кода:

[![](fill-types-images/overlappingcircles-small.png "Triple screenshot of the Overlapping Circles page")](fill-types-images/overlappingcircles-large.png#lightbox "Triple screenshot of the Overlapping Circles page")

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
