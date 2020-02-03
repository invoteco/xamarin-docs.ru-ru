---
title: Линии и концы штрихов
description: В этой статье описывается использование SkiaSharp для рисования линий с концы штрихов различных приложений Xamarin.Forms и демонстрирует это с помощью примера кода.
ms.prod: xamarin
ms.assetid: 1F854DDD-5D1B-4DE4-BD2D-584439429FDB
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 9aaecb8c63ff28111097dce81954f523b4c7731b
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725216"
---
# <a name="lines-and-stroke-caps"></a>Линии и концы штрихов

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Сведения об использовании SkiaSharp для рисования линий с различными наконечниками штриха_

В SkiaSharp Подготовка к просмотру на одной строке существенно отличается от отрисовки ряд соединенных прямых линий. Даже в том случае, если рисования отдельных строк, тем не менее, часто бывает необходимо предоставить определенной Толщина линий. Как эти строки становятся шире, внешний вид концы строк также становится важным. Внешний вид конца линии называется *наконечником обводки*.

![](lines-images/strokecapsexample.png "The three stroke caps options")

Для рисования отдельных строк `SKCanvas` определяет простой метод [`DrawLine`](xref:SkiaSharp.SKCanvas.DrawLine(System.Single,System.Single,System.Single,System.Single,SkiaSharp.SKPaint)) , аргументы которого указывают начальную и конечную координаты линии с объектом `SKPaint`:

```csharp
canvas.DrawLine (x0, y0, x1, y1, paint);
```

По умолчанию свойство [`StrokeWidth`](xref:SkiaSharp.SKPaint.StrokeWidth) недавно созданного объекта `SKPaint` имеет значение 0, что аналогично значению 1 в отрисовке линии размером один пиксель в толщину. Это выглядит очень тонко на устройствах с высоким разрешением, например на телефонах, поэтому вы, вероятно, захотите задать для `StrokeWidth` большее значение. Но как только вы начнете Рисование линий расширяемой толщины, вызывающего событие еще одна проблема: как должны начала и окончания этих толстых строк отображаться?

Внешний вид линий начала и конца линии называется *концом строки* или, в СКИА, *наконечником обводки*. Слово «Cap» в этом контексте относится к разновидности Hat &mdash; что-то, что находится в конце строки. Свойству [`StrokeCap`](xref:SkiaSharp.SKPaint.StrokeCap) объекта `SKPaint` задается один из следующих членов перечисления [`SKStrokeCap`](xref:SkiaSharp.SKStrokeCap) :

- `Butt` (по умолчанию)
- `Square`
- `Round`

Лучше всего они показаны с помощью примера программы. Раздел **SkiaSharp Lines and paths** программы [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) начинается со страницы, Наименованной с **прописными буквами** , на основе класса [`StrokeCapsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/StrokeCapsPage.cs) . На этой странице определяется обработчик событий `PaintSurface`, который циклически проходит по трем элементам перечисления `SKStrokeCap`, отображая как имя члена перечисления, так и рисование линии с помощью этого конца штриха:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 75,
        TextAlign = SKTextAlign.Center
    };

    SKPaint thickLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 50
    };

    SKPaint thinLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2
    };

    float xText = info.Width / 2;
    float xLine1 = 100;
    float xLine2 = info.Width - xLine1;
    float y = textPaint.FontSpacing;

    foreach (SKStrokeCap strokeCap in Enum.GetValues(typeof(SKStrokeCap)))
    {
        // Display text
        canvas.DrawText(strokeCap.ToString(), xText, y, textPaint);
        y += textPaint.FontSpacing;

        // Display thick line
        thickLinePaint.StrokeCap = strokeCap;
        canvas.DrawLine(xLine1, y, xLine2, y, thickLinePaint);

        // Display thin line
        canvas.DrawLine(xLine1, y, xLine2, y, thinLinePaint);
        y += 2 * textPaint.FontSpacing;
    }
}
```

Для каждого члена перечисления `SKStrokeCap` обработчик рисует две строки: одну с толщиной штриха 50 пикселей, а другую — с толщиной обводки, равной двум пикселям. Это вторая строка предназначен для демонстрации геометрические начало и конец строки, независимо от толщины линии и наконечник штриха:

[![](lines-images/strokecaps-small.png "Triple screenshot of the Stroke Caps page")](lines-images/strokecaps-large.png#lightbox "Triple screenshot of the Stroke Caps page")

Как видите, `Square` и `Round`ные концы обводки эффективно дополняют длину линии на половину толщины штриха в начале строки и снова в конце. Это расширение важно в тех случаях, когда это необходимо для определения измерений, готовый для просмотра графического объекта.

Класс `SKCanvas` также содержит еще один метод для рисования нескольких строк, которые несколько довольно необычная:

```csharp
DrawPoints (SKPointMode mode, points, paint)
```

Параметр `points` является массивом `SKPoint` значений и `mode` является членом перечисления [`SKPointMode`](xref:SkiaSharp.SKPointMode) , который имеет три члена:

- `Points` для отображения отдельных точек
- `Lines` для соединения каждой пары точек
- `Polygon` для подключения всех последовательных точек

Этот метод показан на странице с **несколькими строками** . В файле [**мултиплелинеспаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/MultipleLinesPage.xaml) создаются два представления `Picker`, которые позволяют выбрать член перечисления `SKPointMode` и член перечисления `SKStrokeCap`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Paths.MultipleLinesPage"
             Title="Multiple Lines">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker x:Name="pointModePicker"
                Title="Point Mode"
                Grid.Row="0"
                Grid.Column="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKPointMode}">
                    <x:Static Member="skia:SKPointMode.Points" />
                    <x:Static Member="skia:SKPointMode.Lines" />
                    <x:Static Member="skia:SKPointMode.Polygon" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKStrokeCap}">
                    <x:Static Member="skia:SKStrokeCap.Butt" />
                    <x:Static Member="skia:SKStrokeCap.Round" />
                    <x:Static Member="skia:SKStrokeCap.Square" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skiaforms:SKCanvasView x:Name="canvasView"
                                PaintSurface="OnCanvasViewPaintSurface"
                                Grid.Row="1"
                                Grid.Column="0"
                                Grid.ColumnSpan="2" />
    </Grid>
</ContentPage>
```

Обратите внимание, что объявления пространства имен SkiaSharp немного отличаются, так как пространство имен `SkiaSharp` требуется для ссылки на элементы перечислений `SKPointMode` и `SKStrokeCap`. Обработчик `SelectedIndexChanged` для `Picker` представлений просто делает недействительным объект `SKCanvasView`:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs args)
{
    if (canvasView != null)
    {
        canvasView.InvalidateSurface();
    }
}
```

Этот обработчик должен проверить наличие объекта `SKCanvasView`, так как обработчик событий сначала вызывается, когда свойство `SelectedIndex` `Picker` имеет значение 0 в файле XAML и происходит до создания экземпляра `SKCanvasView`.

Обработчик `PaintSurface` получает два значения перечисления из представлений `Picker`:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Create an array of points scattered through the page
    SKPoint[] points = new SKPoint[10];

    for (int i = 0; i < 2; i++)
    {
        float x = (0.1f + 0.8f * i) * info.Width;

        for (int j = 0; j < 5; j++)
        {
            float y = (0.1f + 0.2f * j) * info.Height;
            points[2 * j + i] = new SKPoint(x, y);
        }
    }

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.DarkOrchid,
        StrokeWidth = 50,
        StrokeCap = (SKStrokeCap)strokeCapPicker.SelectedItem
    };

    // Render the points by calling DrawPoints
    SKPointMode pointMode = (SKPointMode)pointModePicker.SelectedItem;
    canvas.DrawPoints(pointMode, points, paint);
}
```

На снимках экрана показаны различные `Picker` выборки:

[![](lines-images/multiplelines-small.png "Triple screenshot of the Multiple Lines page")](lines-images/multiplelines-large.png#lightbox "Triple screenshot of the Multiple Lines page")

В левой части устройства iPhone показано, как элемент перечисления `SKPointMode.Points` приводит `DrawPoints` к отображению каждой точки в `SKPoint` массиве в виде квадрата, если конец строки `Butt` или `Square`. Круги подготавливаются к просмотру, если конец строки `Round`.

На снимке экрана Android показан результат `SKPointMode.Lines`. Метод `DrawPoints` рисует линию между каждой парой значений `SKPoint`, используя заданное окончание строки, в данном случае `Round`.

Если вместо этого используется `SKPointMode.Polygon`, линия рисуется между последовательными точками в массиве, но если смотреть очень близко, вы увидите, что эти строки не подключены. Каждая из этих отдельных строк начинается и заканчивается указанного отрезка. Если выбрать `Round` с прописными буквами, строки могут быть подключены, но на самом деле они не подключены.

Являются ли строки, подключен или не подключен является важным аспектом работа с контурами графики.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
