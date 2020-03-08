---
title: Циклическая градиенты SkiaSharp
description: Дополнительные сведения о различных типах градиентов, исходя из кругов.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 400AE23A-6A0B-4FA8-BD6B-DE4146B04732
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: d56cc499112a937cd1a22664adeedd54c4397341
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/07/2020
ms.locfileid: "78916386"
---
# <a name="the-skiasharp-circular-gradients"></a>Циклическая градиенты SkiaSharp

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Класс [`SKShader`](xref:SkiaSharp.SKShader) определяет статические методы для создания четырех различных типов градиентов. В статье о [**линейном градиенте SkiaSharp**](linear-gradient.md) обсуждается метод [`CreateLinearGradient`](xref:SkiaSharp.SKShader.CreateLinearGradient*) . В этой статье рассматриваются три других типов градиентов, которые основаны на кругов.

Метод [`CreateRadialGradient`](xref:SkiaSharp.SKShader.CreateRadialGradient*) создает градиент, основывается от центра окружности:

![Пример радиального градиента](circular-gradients-images/RadialGradientSample.png)

Метод [`CreateSweepGradient`](xref:SkiaSharp.SKShader.CreateSweepGradient*) создает градиент, который вращается вокруг центра окружности:

![Очистка образца градиента](circular-gradients-images/SweepGradientSample.png)

Третий тип градиента довольно необычен. Он называется градиентом с двумя точками и определяется методом [`CreateTwoPointConicalGradient`](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient*) . Градиент начиная с позиции один круг на другой:

![Пример коническую градиента](circular-gradients-images/ConicalGradientSample.png)

Если два круга различных размеров, градиента принимает форме конуса.

В этой статье рассматриваются эти градиентов, более подробно.

## <a name="the-radial-gradient"></a>Радиальный градиент

Метод [`CreateRadialGradient`](xref:SkiaSharp.SKShader.CreateRadialGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode)) имеет следующий синтаксис:

```csharp
public static SKShader CreateRadialGradient (SKPoint center, 
                                             Single radius, 
                                             SKColor[] colors, 
                                             Single[] colorPos, 
                                             SKShaderTileMode mode)
```

Перегрузка [`CreateRadialGradient`](xref:SkiaSharp.SKShader.CreateRadialGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) также включает параметр матрицы преобразования.

Первые два аргумента укажите центр круга и radius. Градиент начинается в этом центре и распространяется наружу для `radius` пикселей. Что происходит вне `radius` зависит от аргумента [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) . Параметр `colors` — это массив из двух или более цветов (как в методах линейного градиента), а `colorPos` — массив целых чисел в диапазоне от 0 до 1. Эти целые числа указывают относительное положение цветов, а именно `radius` линии. Вы можете задать для этого аргумента значение, `null`, чтобы в равной степени пробельные цвета.

Если для заполнения окружности используется `CreateRadialGradient`, можно задать центр градиента в центре окружности, а радиус градиента — радиусу круга. В этом случае аргумент `SKShaderTileMode` не влияет на отрисовку градиента. Но если область, заполненная градиентом, больше круга, определенного градиентом, то аргумент `SKShaderTileMode` имеет более глубокое воздействие на то, что происходит вне окружности.

Результат `SKShaderMode` показан на странице **радиальный градиент** в примере [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) . XAML-файл для этой страницы создает экземпляр `Picker`, который позволяет выбрать один из трех членов перечисления `SKShaderTileMode`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.RadialGradientPage"
             Title="Radial Gradient">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <skiaforms:SKCanvasView x:Name="canvasView"
                                Grid.Row="0"
                                PaintSurface="OnCanvasViewPaintSurface" />

        <Picker x:Name="tileModePicker" 
                Grid.Row="1"
                Title="Shader Tile Mode" 
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKShaderTileMode}">
                    <x:Static Member="skia:SKShaderTileMode.Clamp" />
                    <x:Static Member="skia:SKShaderTileMode.Repeat" />
                    <x:Static Member="skia:SKShaderTileMode.Mirror" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>
    </Grid>
</ContentPage>
```

Файл с выделенным кодом цвета весь холст с применением радиального градиента. Центр градиента задано относительно центральной части холста и радиус присваивается 100 пикселей. Градиент состоит из двух цветов, черно-белый.

```csharp
public partial class RadialGradientPage : ContentPage
{
    public RadialGradientPage ()
    {
        InitializeComponent ();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKShaderTileMode tileMode =
            (SKShaderTileMode)(tileModePicker.SelectedIndex == -1 ?
                                        0 : tileModePicker.SelectedItem);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateRadialGradient(
                                new SKPoint(info.Rect.MidX, info.Rect.MidY),
                                100,
                                new SKColor[] { SKColors.Black, SKColors.White },
                                null,
                                tileMode);

            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

Этот код создает градиент с черным в центре, постепенно плавный переход белому 100 пикселей в центре. Что происходит за пределами радиуса, зависит от аргумента `SKShaderTileMode`:

[![Радиальный градиент](circular-gradients-images/RadialGradient.png "Радиальный градиент")](circular-gradients-images/RadialGradient-Large.png#lightbox)

Во всех трех случаях градиента заполнит весь холст. На экране iOS слева градиента за пределами радиус продолжается с последний цвет, являющийся белый. Это результат `SKShaderTileMode.Clamp`. Экран Android показывает результат `SKShaderTileMode.Repeat`: в 100 пикселей от центра градиент начинается снова с первого цвета, который является черным. Градиент повторяется каждые 100 пикселей radius. 

На универсальная платформа Windows экране справа показано, как `SKShaderTileMode.Mirror` приводит градиент к альтернативным направлениям. Первый градиент — от черного в центре белым радиус 100 пикселей. Следующему — белый из радиус 100 пикселей на черный в radius 200 пикселей, а далее градиента зеркально отображается снова.

Можно использовать более двух цветов в радиальном градиенте. Образец **градиента дуги на Радуга** создает массив из восьми цветов, соответствующих цветам Радуга и заканчивая красным, а также массивом из восьми значений позиции:

```csharp
public class RainbowArcGradientPage : ContentPage
{
    public RainbowArcGradientPage ()
    {
        Title = "Rainbow Arc Gradient";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            float rainbowWidth = Math.Min(info.Width, info.Height) / 4f;

            // Center of arc and gradient is lower-right corner
            SKPoint center = new SKPoint(info.Width, info.Height);

            // Find outer, inner, and middle radius
            float outerRadius = Math.Min(info.Width, info.Height);
            float innerRadius = outerRadius - rainbowWidth;
            float radius = outerRadius - rainbowWidth / 2;

            // Calculate the colors and positions
            SKColor[] colors = new SKColor[8];
            float[] positions = new float[8];

            for (int i = 0; i < colors.Length; i++)
            {
                colors[i] = SKColor.FromHsl(i * 360f / 7, 100, 50);
                positions[i] = (i + (7f - i) * innerRadius / outerRadius) / 7f;
            }

            // Create sweep gradient based on center and outer radius
            paint.Shader = SKShader.CreateRadialGradient(center, 
                                                         outerRadius, 
                                                         colors, 
                                                         positions, 
                                                         SKShaderTileMode.Clamp);
            // Draw a circle with a wide line
            paint.Style = SKPaintStyle.Stroke;
            paint.StrokeWidth = rainbowWidth;

            canvas.DrawCircle(center, radius, paint);
        }
    }
}
```

Предположим, что минимальное значение ширины и высоты холста — 1000. Это означает, что `rainbowWidth` значением является 250. Значения `outerRadius` и `innerRadius` имеют значение 1000 и 750 соответственно. Эти значения используются для вычисления массива `positions`; восемь значений находятся в диапазоне от 0,75 f до 1. Значение `radius` используется для обводки круга. Значение 875 средства, предоставляемые между radius 750 пикселей и радиус точек 1000 ширина мазков 250 пикселей:

[![Градиент дуги (Радуга)](circular-gradients-images/RainbowArcGradient.png "Градиент дуги (Радуга)")](circular-gradients-images/RainbowArcGradient-Large.png#lightbox)

С этой градиента заполнения весь холст, вы увидите сообщение о том, что это красный в внутренний радиус. Это обусловлено тем, что массив `positions` не начинается с 0. Первый цвет используется для смещения от 0 до первого значения массива. Градиент — тоже красный за пределы внешним. Это результат режима плитки `Clamp`. Поскольку градиента используется для Обводка толстой линией, эти red области не отображаются.

## <a name="radial-gradients-for-masking"></a>Радиальные градиенты для маски

Как линейные градиенты радиальным градиентом можно включить цвета прозрачным или полупрозрачным. Эта функция полезна для процесса, называемого _маскированием_, который скрывает часть изображения для акцентирования другой части изображения.

Пример показан на странице **маски радиального градиента** . Программа загружает один ресурс точечных рисунков. Поля `CENTER` и `RADIUS` были определены с помощью проверки растрового изображения и ссылаются на область, которую следует выделить. Обработчик `PaintSurface` начинает с вычисления прямоугольника для отображения точечного рисунка, а затем отображает его в этом прямоугольнике:

```csharp
public class RadialGradientMaskPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
        typeof(RadialGradientMaskPage),
        "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

    static readonly SKPoint CENTER = new SKPoint(180, 300);
    static readonly float RADIUS = 120;

    public RadialGradientMaskPage ()
    {
        Title = "Radial Gradient Mask";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find rectangle to display bitmap
        float scale = Math.Min((float)info.Width / bitmap.Width,
                                (float)info.Height / bitmap.Height);

        SKRect rect = SKRect.Create(scale * bitmap.Width, scale * bitmap.Height);

        float x = (info.Width - rect.Width) / 2;
        float y = (info.Height - rect.Height) / 2;
        rect.Offset(x, y);

        // Display bitmap in rectangle
        canvas.DrawBitmap(bitmap, rect);

        // Adjust center and radius for scaled and offset bitmap
        SKPoint center = new SKPoint(scale * CENTER.X + x,
                                     scale * CENTER.Y + y);
        float radius = scale * RADIUS;

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateRadialGradient(
                                center,
                                radius,
                                new SKColor[] { SKColors.Transparent,
                                                SKColors.White },
                                new float[] { 0.6f, 1 },
                                SKShaderTileMode.Clamp);

            // Display rectangle using that gradient
            canvas.DrawRect(rect, paint);
        }
    }
}
```

После рисования растрового изображения некоторые простые в коде преобразования `CENTER` и `RADIUS` в `center` и `radius`, которые ссылаются на выделенную область точечного рисунка, которая была масштабирована и смещена для отображения. Эти значения используются для создания этого центра и радиус радиального градиента. Два цвета начинаются с прозрачным, в центре, а также для первого 60% от радиуса. Градиент, затем выцветает до белого:

[![Маска радиального градиента](circular-gradients-images/RadialGradientMask.png "Маска радиального градиента")](circular-gradients-images/RadialGradientMask-Large.png#lightbox)

Этот подход не является лучшим способом для маскировки растрового изображения. Проблема в том, что маска главным образом имеет цвет белый цвет, который был выбран в соответствии с фоном части холста. Если фон является другим цветом &mdash; или, возможно, градиентный &mdash; он не соответствует. Лучший подход к маскировке показан в статье [SkiaSharp Портер-Дуфф Blend modes](../blend-modes/porter-duff.md).

## <a name="radial-gradients-for-specular-highlights"></a>Радиальные градиенты для зеркального отражения

При света на сервере со скругленными поверхность она отражает свет в разных направлениях, но некоторые света будет передаваться непосредственно в средстве просмотра глаз. Это часто создает впечатление нечеткой белой области на поверхности, называемой _зеркальным выделением_.

В трехмерной графики отражающие яркие часто результатом алгоритмы, которые используются для определения света пути и заливка. В двухмерной графики зеркального отражения иногда добавляются предложить внешний вид трехмерной поверхности. Отражающий выделения можно преобразовать плоский красный кружок в round красный шар.

На странице **радиальное отраженное выделение** для точного использования используется радиальный градиент. Обработчик `PaintSurface` существ, вычисляя радиус для окружности и два `SKPoint`ых значения &mdash; `center` и `offCenter`, наполовину от центра к левому верхнему краю круга:

```csharp
public class RadialSpecularHighlightPage : ContentPage
{
    public RadialSpecularHighlightPage()
    {
        Title = "Radial Specular Highlight";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        float radius = 0.4f * Math.Min(info.Width, info.Height);
        SKPoint center = new SKPoint(info.Rect.MidX, info.Rect.MidY);
        SKPoint offCenter = center - new SKPoint(radius / 2, radius / 2);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateRadialGradient(
                                offCenter,
                                radius / 2,
                                new SKColor[] { SKColors.White, SKColors.Red },
                                null,
                                SKShaderTileMode.Clamp);

            canvas.DrawCircle(center, radius, paint);
        }
    }
}
```

Вызов `CreateRadialGradient` создает градиент, который начинается с этого `offCenter` наведите указатель на белый цвет и заканчивается красным на расстоянии половины радиуса. Вот как оно выглядит:

[![Радиальное отраженное выделение](circular-gradients-images/RadialSpecularHighlight.png "Радиальное отраженное выделение")](circular-gradients-images/RadialSpecularHighlight-Large.png#lightbox)

Если вы внимательно посмотрите на этот градиент, можно решить, что – порочен. Градиента центрируется по состоянию на определенный момент, и вы можете его были немного меньше симметричное в соответствии с рабочей области со скругленными. В этом случае вы можете предпочесть отраженному изображению, показанному ниже, в разделе [**градиенты для отражения отраженных**](#conical-gradients-for-specular-highlights)светов.

## <a name="the-sweep-gradient"></a>Очистка градиента

Метод [`CreateSweepGradient`](xref:SkiaSharp.SKShader.CreateSweepGradient(SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[])) имеет самый простой синтаксис для всех методов создания градиента:

```csharp
public static SKShader CreateSweepGradient (SKPoint center, 
                                            SKColor[] colors, 
                                            Single[] colorPos)
```

Это просто center, массив цветов и позиций цвет. Начинается в правой части центральной точки и выполняет очистку 360 градусов по часовой стрелке вокруг центра. Обратите внимание, что отсутствует параметр `SKShaderTileMode`.

Также доступен перегруженный метод [`CreateSweepGradient`](xref:SkiaSharp.SKShader.CreateSweepGradient(SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKMatrix)) с параметром преобразования матрицы. Чтобы изменить начальную точку градиента можно применить преобразование поворота. Также можно применить преобразование масштаба, чтобы изменить направление по часовой стрелке на против часовой стрелки.

Страница « **градиентная очистка** » использует градиент поворота для цвета круга с толщиной штриха 50 пикселей.

[![Градиентная очистка](circular-gradients-images/SweepGradient.png "Градиентная очистка")](circular-gradients-images/SweepGradient-Large.png#lightbox)

Класс `SweepGradientPage` определяет массив из восьми цветов с разными значениями оттенка. Обратите внимание на то, что массив начинается и заканчивается red (hue значение 0 или 360), которая отображается в правом на снимках экрана:

```csharp
public class SweepGradientPage : ContentPage
{
    bool drawBackground;

    public SweepGradientPage ()
    {
        Title = "Sweep Gradient";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        TapGestureRecognizer tap = new TapGestureRecognizer();
        tap.Tapped += (sender, args) =>
        {
            drawBackground ^= true;
            canvasView.InvalidateSurface();
        };
        canvasView.GestureRecognizers.Add(tap);
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            // Define an array of rainbow colors
            SKColor[] colors = new SKColor[8];

            for (int i = 0; i < colors.Length; i++)
            {
                colors[i] = SKColor.FromHsl(i * 360f / 7, 100, 50);
            }

            SKPoint center = new SKPoint(info.Rect.MidX, info.Rect.MidY);

            // Create sweep gradient based on center of canvas
            paint.Shader = SKShader.CreateSweepGradient(center, colors, null);

            // Draw a circle with a wide line
            const int strokeWidth = 50;
            paint.Style = SKPaintStyle.Stroke;
            paint.StrokeWidth = strokeWidth;

            float radius = (Math.Min(info.Width, info.Height) - strokeWidth) / 2;
            canvas.DrawCircle(center, radius, paint);

            if (drawBackground)
            {
                // Draw the gradient on the whole canvas
                paint.Style = SKPaintStyle.Fill;
                canvas.DrawRect(info.Rect, paint);
            }
        }
    }
}
```

Программа также реализует `TapGestureRecognizer`, которая позволяет выполнить некоторый код в конце обработчика `PaintSurface`. Этот код использует этот же градиент для заполнения полотна.

[![Полная очистка градиента](circular-gradients-images/SweepGradientFull.png "Полная очистка градиента")](circular-gradients-images/SweepGradientFull-Large.png#lightbox)

Эти снимки экрана показано, что градиентных заливок независимо от области окрашивается цветом по его. Если градиента не начинаться и заканчиваться тот же цвет, будет существовать разрыв справа от центральной точки.

## <a name="the-two-point-conical-gradient"></a>Две точки коническую градиента

Метод [`CreateTwoPointConicalGradient`](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode)) имеет следующий синтаксис:

```csharp
public static SKShader CreateTwoPointConicalGradient (SKPoint startCenter, 
                                                      Single startRadius, 
                                                      SKPoint endCenter, 
                                                      Single endRadius, 
                                                      SKColor[] colors, 
                                                      Single[] colorPos, 
                                                      SKShaderTileMode mode)
```

Параметры начинаются с центральных точек и радиусов для двух кругов, которые называются _начальным_ и _конечным_ кругом. Остальные три параметра те же, что и для `CreateLinearGradient` и `CreateRadialGradient`. Перегрузка [`CreateTwoPointConicalGradient`](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) включает преобразование матрицы.

Градиента начинается с начала круг и заканчивается в круг end. Параметр `SKShaderTileMode` управляет тем, что происходит за пределами двух кругов. Две точки градиента коническую является только градиент, который не полностью заполняет область. Если два круги одного радиуса, градиента ограничен прямоугольник, ширина которого совпадает со значением диаметр кругов. Если два круга различные радиусы, градиента forms конуса.

Скорее всего, вы захотите поэкспериментировать с градиентом с двумя точками, так что страница **градиента** в реальном направлении является производной от `InteractivePage`, чтобы разрешить перемещение двух точек касания для двух круговых радиусов:

```xaml
<local:InteractivePage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       xmlns:local="clr-namespace:SkiaSharpFormsDemos"
                       xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
                       xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
                       xmlns:tt="clr-namespace:TouchTracking"
                       x:Class="SkiaSharpFormsDemos.Effects.ConicalGradientPage"
                       Title="Conical Gradient">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        
        <Grid BackgroundColor="White"
              Grid.Row="0">
            <skiaforms:SKCanvasView x:Name="canvasView"
                                    PaintSurface="OnCanvasViewPaintSurface" />
            <Grid.Effects>
                <tt:TouchEffect Capture="True"
                                TouchAction="OnTouchEffectAction" />
            </Grid.Effects>
        </Grid>

        <Picker x:Name="tileModePicker" 
                Grid.Row="1"
                Title="Shader Tile Mode" 
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKShaderTileMode}">
                    <x:Static Member="skia:SKShaderTileMode.Clamp" />
                    <x:Static Member="skia:SKShaderTileMode.Repeat" />
                    <x:Static Member="skia:SKShaderTileMode.Mirror" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>
    </Grid>
</local:InteractivePage>
```

Файл кода программной части определяет два `TouchPoint` объектов с фиксированными радиусами 50 и 100:

```csharp
public partial class ConicalGradientPage : InteractivePage
{
    const int RADIUS1 = 50;
    const int RADIUS2 = 100;

    public ConicalGradientPage ()
    {
        touchPoints = new TouchPoint[2];

        touchPoints[0] = new TouchPoint
        {
            Center = new SKPoint(100, 100),
            Radius = RADIUS1
        };

        touchPoints[1] = new TouchPoint
        {
            Center = new SKPoint(300, 300),
            Radius = RADIUS2
        };

        InitializeComponent();
        baseCanvasView = canvasView;
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKColor[] colors = { SKColors.Red, SKColors.Green, SKColors.Blue };
        SKShaderTileMode tileMode = 
            (SKShaderTileMode)(tileModePicker.SelectedIndex == -1 ? 
                                        0 : tileModePicker.SelectedItem);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateTwoPointConicalGradient(touchPoints[0].Center,
                                                                  RADIUS1,
                                                                  touchPoints[1].Center,
                                                                  RADIUS2,
                                                                  colors,
                                                                  null,
                                                                  tileMode);
            canvas.DrawRect(info.Rect, paint);
        }

        // Display the touch points here rather than by TouchPoint
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Black;
            paint.StrokeWidth = 3;

            foreach (TouchPoint touchPoint in touchPoints)
            {
                canvas.DrawCircle(touchPoint.Center, touchPoint.Radius, paint);
            }
        }
    }
}
```

`colors` массив — красный, зеленый и синий. Код, расположенный в нижней части обработчика `PaintSurface`, рисует две сенсорные точки как черные кружки, чтобы они не переходили на градиент.

Обратите внимание, что `DrawRect` вызов использует градиент для выделения цветом всего холста. В общем случае Однако большая часть холста остается бесцветных по градиента. Вот программа, показывающая три варианта конфигурации:

[![Градиентное применение в виде конуса](circular-gradients-images/ConicalGradient.png "Градиентное применение в виде конуса")](circular-gradients-images/ConicalGradient-Large.png#lightbox)

На экране iOS в левой части показано действие параметра `SKShaderTileMode` `Clamp`. Градиент начинается с red внутри края меньшего размера окружности, который противоположной стороны, ближайший к второго круга. `Clamp` значение также приводит к тому, что красный цвет переходит к точке конуса. Градиент заканчивается синего цвета на вешней границе круга большего размера, наиболее близкое к первый круг, но по-прежнему синий круг и за его пределами.

Экран Android аналогичен, но с `SKShaderTileMode` `Repeat`. Теперь это яснее, что начинается внутри круга, первый и оканчивается за пределами второго круга. Параметр `Repeat` приводит к тому, что градиент повторяется красным цветом внутри большего круга.

На экране универсальной платформы Windows показывает, что происходит при перемещении небольших круга полностью внутри круга большего размера. Градиент перестает быть конус и вместо этого заполняет все пространство. Результат аналогичен радиального градиента, но ассиметричные круг меньшего размера не точно центрируется в круг большего размера.

Практические полезность градиента может сомневаюсь, когда один круг вложен в другой, но он идеально подходит для зеркального отражения выделения.

## <a name="conical-gradients-for-specular-highlights"></a>Коническую градиент для зеркального отражения

Ранее в этой статье вы узнали, как использовать Радиальный градиент для создания зеркального отражения выделения. Две точки коническую градиента можно также использовать для этой цели, и может потребоваться его внешний вид:

[![Отраженное выделение в виде конусов](circular-gradients-images/ConicalSpecularHighlight.png "Отраженное выделение в виде конусов")](circular-gradients-images/ConicalSpecularHighlight-Large.png#lightbox)

Асимметричное внешний лучше предлагает скругленными поверхности объекта. 

Код рисования на странице с **отраженным** отражением в виде конусов совпадает со страницей **радиального выделения** , за исключением шейдера:

```csharp
public class ConicalSpecularHighlightPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateTwoPointConicalGradient(
                                offCenter,
                                1,
                                center,
                                radius,
                                new SKColor[] { SKColors.White, SKColors.Red },
                                null,
                                SKShaderTileMode.Clamp);

            canvas.DrawCircle(center, radius, paint);
        }
    }
}
```

У двух кругов есть центры `offCenter` и `center`. Кружок, центрированный по `center`, связан с радиусом, охватывающим весь шар, но окружность в центре `offCenter` имеет радиус всего одного пикселя. Градиент фактически начинается в этот момент и заканчивается на границе шара.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
