---
title: Линейный градиент SkiaSharp
description: Узнайте, как для вычерчивания линий или Заливка области с градиентом, состоящие из постепенное сочетание двух цветов.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 20A2A8C4-FEB7-478D-BF57-C92E26117B6A
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 290e533e54b2ee150b94d9fb6b0f5119324f9cf0
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306503"
---
# <a name="the-skiasharp-linear-gradient"></a>Линейный градиент SkiaSharp

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Класс [`SKPaint`](xref:SkiaSharp.SKPaint) определяет свойство [`Color`](xref:SkiaSharp.SKPaint.Color) , которое используется для обводки линий или заливки областей сплошным цветом. Можно также обводка линий или заливки с помощью _градиентов_, которые являются постепенным смешением цветов.

![Пример линейного градиента](linear-gradient-images/LinearGradientSample.png "Пример линейного градиента")

Самый простой тип градиента — _линейный_ градиент. Смешение цветов выполняется в строке ( _линия градиента_) с одной точки на другую. Одинаковый цвет у линии, перпендикулярной линии градиента. Линейный градиент можно создать с помощью одного из двух статических [`SKShader.CreateLinearGradient`](xref:SkiaSharp.SKShader.CreateLinearGradient*) методов. Разница между двумя перегрузками является один входит преобразование матрицы и другой — нет. 

Эти методы возвращают объект типа [`SKShader`](xref:SkiaSharp.SKShader) , установленного для свойства [`Shader`](xref:SkiaSharp.SKPaint.Shader) `SKPaint`. Если свойство `Shader` не равно null, оно переопределяет свойство `Color`. Любая линия с обводкой или любая область, заполненная с помощью этого `SKPaint` объекта, основана на градиенте, а не на сплошном цвете.

> [!NOTE]
> Свойство `Shader` игнорируется при включении объекта `SKPaint` в вызов `DrawBitmap`. Можно использовать свойство `Color` `SKPaint`, чтобы задать уровень прозрачности для отображения точечного рисунка (как описано в статье, [отображающей точечные рисунки SkiaSharp](../../bitmaps/displaying.md#displaying-in-pixel-dimensions)), но нельзя использовать свойство `Shader` для отображения растрового изображения с прозрачностью градиента. Для отображения растровых изображений с прозрачными градиентами можно использовать другие методы. они описаны в статьях [SkiaSharp циклические градиенты](circular-gradients.md#radial-gradients-for-masking) и [SkiaSharp компоновки и режимы смешения](../blend-modes/porter-duff.md#gradient-transparency-and-transitions).

## <a name="corner-to-corner-gradients"></a>Градиенты углу верхним углом

Часто линейный градиент расширяется от один из углов прямоугольника в другой. Если начальная точка верхнего левого угла прямоугольника, можно расширить градиента:

- по вертикали относительно левого нижнего угла
- по горизонтали в правом верхнем углу
- по диагонали, чтобы нижний правый угол

Диагональный линейный градиент показан на первой странице в разделе **SkiaSharp шейдеры и другие эффекты** в примере [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) . Страница **градиента в угловом** углу создает `SKCanvasView` в своем конструкторе. Обработчик `PaintSurface` создает объект `SKPaint` в операторе `using`, а затем определяет прямоугольный прямоугольник размером 300 пикселей в центре холста:

```csharp
public class CornerToCornerGradientPage : ContentPage
{
    ···
    public CornerToCornerGradientPage ()
    {
        Title = "Corner-to-Corner Gradient";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
        ···
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            // Create 300-pixel square centered rectangle
            float x = (info.Width - 300) / 2;
            float y = (info.Height - 300) / 2;
            SKRect rect = new SKRect(x, y, x + 300, y + 300);

            // Create linear gradient from upper-left to lower-right
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(rect.Left, rect.Top),
                                new SKPoint(rect.Right, rect.Bottom),
                                new SKColor[] { SKColors.Red, SKColors.Blue },
                                new float[] { 0, 1 },
                                SKShaderTileMode.Repeat);

            // Draw the gradient on the rectangle
            canvas.DrawRect(rect, paint);
            ···
        }
    }
}
```

Свойству `Shader` `SKPaint` присваивается возвращаемое `SKShader` значение из статического метода `SKShader.CreateLinearGradient`. Ниже приведены пять аргументов.

- Начальную точку градиента, заданные в верхний левый угол прямоугольника
- Конечную точку градиента, заданные в правый нижний угол прямоугольника
- Массив несколько цветов, способствующих градиента
- Массив значений `float`, указывающих относительное расположение цветов в линии градиента
- Элемент перечисления [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) , указывающий, как градиент ведет себя за концом линии градиента

После создания объекта градиента `DrawRect` метод рисует квадратный прямоугольник размером 300 пикселей, используя объект `SKPaint`, включающий шейдер. Здесь он работает в iOS, Android и универсальной платформы Windows (UWP):

[![Градиентный угол в углу](linear-gradient-images/CornerToCornerGradient.png "Градиентный угол в углу")](linear-gradient-images/CornerToCornerGradient-Large.png#lightbox)

Линии градиента определяется двумя точками, заданный как первые два аргумента. Обратите внимание, что эти точки находятся относительно _холста_ , а _не_ для графического объекта, отображаемого с градиентом. Вдоль линии градиента цвет постепенно переходит от красного в левом верхнем углу на синий в правом нижнем углу. Любую строку, перпендикулярно линии градиента имеет постоянный цвет.

Массив значений `float`, указанных в качестве четвертого аргумента, имеет корреспонденцию "один к одному" с массивом цветов. Значения указывают относительное положение вдоль линии градиента возникновения этих цветов. Здесь 0 означает, что `Red` встречается в начале линии градиента, а значение 1 означает, что `Blue` встречается в конце строки. Цифры должны быть отсортированы по возрастанию и должно быть в диапазоне от 0 до 1. Если их нет в этом диапазоне, они будут корректироваться должны находиться в диапазоне.

Два значения в массиве можно задать на что-нибудь, отличный от 0 до 1. Попробуйте выполните следующее.

```csharp
new float[] { 0.25f, 0.75f }
```

Теперь совершенно первого квартала линии градиента является чисто red, которая последний квартал чисто синий цвет. Сочетание красного и синего предоставляется только в центральной части линии градиента.

Как правило вы захотите пространства эти значения позиции одинаково от 0 до 1. Если это так, можно просто указать `null` в качестве четвертого аргумента для `CreateLinearGradient`.

Несмотря на то, что этот градиента определяется между двумя углов прямоугольника square 300 пикселей, он не ограничен заполнения этого прямоугольника. Страница **градиента в угловых** скобках содержит дополнительный код, реагирующий на касания или щелчки мышью на странице. Поле `drawBackground` переключается между `true` и `false` при каждом касании. Если значение равно `true`, то обработчик `PaintSurface` использует тот же `SKPaint` объект для заполнения всего холста, а затем рисует черный прямоугольник, указывающий на меньший прямоугольник: 

```csharp
public class CornerToCornerGradientPage : ContentPage
{
    bool drawBackground;

    public CornerToCornerGradientPage ()
    {
        ···
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
        ···
        using (SKPaint paint = new SKPaint())
        {
            ···
            if (drawBackground)
            {
                // Draw the gradient on the whole canvas
                canvas.DrawRect(info.Rect, paint);

                // Outline the smaller rectangle
                paint.Shader = null;
                paint.Style = SKPaintStyle.Stroke;
                paint.Color = SKColors.Black;
                canvas.DrawRect(rect, paint);
            }
        }
    }
}
```

Вот, что вы увидите после нажатия на экране:

[![Заполнение градиента в угловом углу](linear-gradient-images/CornerToCornerGradientFull.png "Заполнение градиента в угловом углу")](linear-gradient-images/CornerToCornerGradientFull-Large.png#lightbox)

Обратите внимание на то, что градиент повторяется в один и тот же шаблон за пределами точки, определяющие линии градиента. Это повторение происходит потому, что последний аргумент для `CreateLinearGradient` `SKShaderTileMode.Repeat`. (Вскоре вы увидите другие варианты.)

Также Обратите внимание на то, что точки, которые позволяют указать линии градиента не уникальным. Линии, перпендикулярной линии градиента имеют один и тот же цвет, поэтому существует бесконечное число градиента строки, которые можно указать для тот же эффект. Например при заполнении прямоугольника с помощью горизонтальный градиент, можно указать углов верхнего левого и правого верхнего или нижнего левого и правого нижнего углов или двух точках, даже с и параллельно эти строки.

## <a name="interactively-experiment"></a>Интерактивно экспериментов

Вы можете интерактивно экспериментировать с линейными градиентами с помощью страницы « **интерактивная линейный градиент** ». На этой странице используется класс `InteractivePage`, представленный в статье [**три способа рисования дуги**](../../curves/arcs.md). `InteractivePage` обрабатывает события [`TouchEffect`](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md) , чтобы поддерживать коллекцию объектов `TouchPoint`, которые можно перемещать с помощью пальцев или мыши.

XAML-файл присоединяет `TouchEffect` к родительскому элементу `SKCanvasView`, а также включает `Picker`, позволяющую выбрать один из трех членов перечисления [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) :

```xaml
<local:InteractivePage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       xmlns:local="clr-namespace:SkiaSharpFormsDemos"
                       xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
                       xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
                       xmlns:tt="clr-namespace:TouchTracking"
                       x:Class="SkiaSharpFormsDemos.Effects.InteractiveLinearGradientPage"
                       Title="Interactive Linear Gradient">
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

Конструктор в файле кода программной части создает два `TouchPoint` объектов для начальной и конечной точек линейного градиента. Обработчик `PaintSurface` определяет массив из трех цветов (для градиента от красного к зеленому и синего) и получает текущий `SKShaderTileMode` из `Picker`:

```csharp
public partial class InteractiveLinearGradientPage : InteractivePage
{
    public InteractiveLinearGradientPage ()
    {
        InitializeComponent ();

        touchPoints = new TouchPoint[2];

        for (int i = 0; i < 2; i++)
        { 
            touchPoints[i] = new TouchPoint
            {
                Center = new SKPoint(100 + i * 200, 100 + i * 200)
            };
        }

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
            paint.Shader = SKShader.CreateLinearGradient(touchPoints[0].Center,
                                                         touchPoints[1].Center,
                                                         colors,
                                                         null,
                                                         tileMode);
            canvas.DrawRect(info.Rect, paint);
        }
        ···
    }
}
```

Обработчик `PaintSurface` создает объект `SKShader` из всех этих данных и использует его для выделения цветом всего холста. Массив значений `float` имеет значение `null`. В противном случае для одинаковые расстояния между трех цветов, этому параметру будет присвоено массив, содержащий значения 0, 0.5 и 1.

Основная часть обработчика `PaintSurface` предназначена для отображения нескольких объектов: сенсорные точки в виде контуров, линии градиента и линий, перпендикулярных линиям градиента в сенсорных точках:

```csharp
public partial class InteractiveLinearGradientPage : InteractivePage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
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

            // Draw gradient line connecting touchpoints
            canvas.DrawLine(touchPoints[0].Center, touchPoints[1].Center, paint);

            // Draw lines perpendicular to the gradient line
            SKPoint vector = touchPoints[1].Center - touchPoints[0].Center;
            float length = (float)Math.Sqrt(Math.Pow(vector.X, 2) +
                                            Math.Pow(vector.Y, 2));
            vector.X /= length;
            vector.Y /= length;
            SKPoint rotate90 = new SKPoint(-vector.Y, vector.X);
            rotate90.X *= 200;
            rotate90.Y *= 200;

            canvas.DrawLine(touchPoints[0].Center, 
                            touchPoints[0].Center + rotate90, 
                            paint);

            canvas.DrawLine(touchPoints[0].Center,
                            touchPoints[0].Center - rotate90,
                            paint);

            canvas.DrawLine(touchPoints[1].Center,
                            touchPoints[1].Center + rotate90,
                            paint);

            canvas.DrawLine(touchPoints[1].Center,
                            touchPoints[1].Center - rotate90,
                            paint);
        }
    }
}
```

Легко для рисования градиента линию, соединяющую две точки соприкосновения, но перпендикулярной линии требуют дополнительной работы. Линии градиента преобразуется в вектор, нормализованы, чтобы длина за одну единицу и затем повернут на 90 градусов. Этот вектор предоставляется длина составляет 200 пикселей. Он используется для рисования четыре строки, которые исходят из точки касания с градиентной линией.

Перпендикулярной линии совпадают с начало и конец градиента. Что происходит за пределами этих строк, зависит от значения перечисления `SKShaderTileMode`:

[![Интерактивный линейный градиент](linear-gradient-images/InteractiveLinearGradient.png "Интерактивный линейный градиент")](linear-gradient-images/InteractiveLinearGradient-Large.png#lightbox)

На трех снимках экрана показаны результаты трех разных значений [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode). На снимке экрана iOS отображается `SKShaderTileMode.Clamp`, который просто расширяет цвета на границе градиента. Параметр `SKShaderTileMode.Repeat` на снимке экрана Android показывает, как повторяется шаблон градиента. Параметр `SKShaderTileMode.Mirror` на снимке экрана UWP также повторяет шаблон, но шаблон отменяется каждый раз, что приводит к ненепрерывности цветов.

## <a name="gradients-on-gradients"></a>Градиенты на градиенты

Класс `SKShader` не определяет открытые свойства или методы, кроме `Dispose`. В связи с этим объекты `SKShader`, созданные статическими методами, являются неизменяемыми. Даже если вы используете этот же градиент для двух разных объектов, вполне вероятно, вы захотите могут незначительно отличаться в градиенте. Для этого необходимо создать новый объект `SKShader`.

На странице **текста градиента** отображается текст и фоновом, которые окрашены с одинаковыми градиентами:

[![Текст градиента](linear-gradient-images/GradientText.png "Текст градиента")](linear-gradient-images/GradientText-Large.png#lightbox)

Единственное различие градиенты, начальную и конечную точки. Градиента, используемого для отображения текста основана на двух точек на углах ограничивающего прямоугольника для текста. Для фона две точки основаны на весь холст. Вот этот код:

```csharp
public class GradientTextPage : ContentPage
{
    const string TEXT = "GRADIENT";

    public GradientTextPage ()
    {
        Title = "Gradient Text";

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
            // Create gradient for background
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(0, 0),
                                new SKPoint(info.Width, info.Height),
                                new SKColor[] { new SKColor(0x40, 0x40, 0x40),
                                                new SKColor(0xC0, 0xC0, 0xC0) },
                                null,
                                SKShaderTileMode.Clamp);

            // Draw background
            canvas.DrawRect(info.Rect, paint);

            // Set TextSize to fill 90% of width
            paint.TextSize = 100;
            float width = paint.MeasureText(TEXT);
            float scale = 0.9f * info.Width / width;
            paint.TextSize *= scale;

            // Get text bounds
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);

            // Calculate offsets to center the text on the screen
            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2 - textBounds.MidY;

            // Shift textBounds by that amount
            textBounds.Offset(xText, yText);

            // Create gradient for text
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(textBounds.Left, textBounds.Top),
                                new SKPoint(textBounds.Right, textBounds.Bottom),
                                new SKColor[] { new SKColor(0x40, 0x40, 0x40),
                                                new SKColor(0xC0, 0xC0, 0xC0) },
                                null,
                                SKShaderTileMode.Clamp);

            // Draw text
            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

Свойство `Shader` объекта `SKPaint` сначала устанавливается для вывода градиента, охватывающего фон. Точки градиента задаются для верхнего левого и правого нижнего углов части холста.

Код задает свойство `TextSize` объекта `SKPaint`, чтобы текст отображался в 90% от ширины холста. Границы текста используются для вычисления значений `xText` и `yText`, которые передаются методу `DrawText` для центрирования текста.

Однако точки градиента для второго вызова `CreateLinearGradient` должны ссылаться на верхний левый и правый нижний угол текста относительно холста при его отображении. Это достигается путем сдвига `textBounds` прямоугольника на одинаковые значения `xText` и `yText`:

```csharp
textBounds.Offset(xText, yText);
```

Теперь верхнего левого и правого нижнего углов прямоугольника можно задать начальную и конечную точки градиента.

## <a name="animating-a-gradient"></a>Анимация градиент

Для анимации градиент несколькими способами. Один подход заключается в том, чтобы анимировать начальную и конечную точки. Страница « **анимация градиента** » перемещает две точки вокруг окружности, расположенной по центру холста. Радиус этот круг является половины ширины или высоты полотна, какое значение меньше. Начальная и конечная точки отменяются друг от друга в этом круге, а градиент переходит от белого к черному с `Mirror`ным режимом плитки:

[![Градиентная анимация](linear-gradient-images/GradientAnimation.png "Градиентная анимация")](linear-gradient-images/GradientAnimation-Large.png#lightbox)

Конструктор создает `SKCanvasView`. Методы `OnAppearing` и `OnDisappearing` обрабатывали логику анимации:

```csharp
public class GradientAnimationPage : ContentPage
{
    SKCanvasView canvasView;
    bool isAnimating;
    double angle;
    Stopwatch stopwatch = new Stopwatch();

    public GradientAnimationPage()
    {
        Title = "Gradient Animation";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();

        isAnimating = true;
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();

        stopwatch.Stop();
        isAnimating = false;
    }

    bool OnTimerTick()
    {
        const int duration = 3000;
        angle = 2 * Math.PI * (stopwatch.ElapsedMilliseconds % duration) / duration;
        canvasView.InvalidateSurface();

        return isAnimating;
    }
    ···
}
```

Метод `OnTimerTick` вычисляет значение `angle`, которое анимируется от 0 до 2π каждые 3 секунды. 

Вот один из способов, для которого требуется вычислить две точки градиента. Значение `SKPoint` с именем `vector` вычисляется для расширения от центра холста до точки на радиусе круга. Направление данного вектора, зависит от значения синуса и косинуса угла. Затем вычисляется двумя точками противоположной градиента: одна точка вычисляется путем вычитания, вектор из центральной точки, а другие точки получается путем прибавления вектор к центральной точке:

```csharp
public class GradientAnimationPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            SKPoint center = new SKPoint(info.Rect.MidX, info.Rect.MidY);
            int radius = Math.Min(info.Width, info.Height) / 2;
            SKPoint vector = new SKPoint((float)(radius * Math.Cos(angle)),
                                         (float)(radius * Math.Sin(angle)));

            paint.Shader = SKShader.CreateLinearGradient(
                                center - vector,
                                center + vector,
                                new SKColor[] { SKColors.White, SKColors.Black },
                                null,
                                SKShaderTileMode.Mirror);

            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

Немного другой подход требует меньшего объема кода. При таком подходе используется метод перегрузки [`SKShader.CreateLinearGradient`](xref:SkiaSharp.SKShader.CreateLinearGradient(SkiaSharp.SKPoint,SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) с матричным преобразованием в качестве последнего аргумента. Этот подход является версией в примере [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) :

```csharp
public class GradientAnimationPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(0, 0),
                                info.Width < info.Height ? new SKPoint(info.Width, 0) : 
                                                           new SKPoint(0, info.Height),
                                new SKColor[] { SKColors.White, SKColors.Black },
                                new float[] { 0, 1 },
                                SKShaderTileMode.Mirror,
                                SKMatrix.MakeRotation((float)angle, info.Rect.MidX, info.Rect.MidY));

            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

Если ширина холста меньше, чем высота, то для двух точек градиента устанавливается значение (0, 0) и (`info.Width`, 0). Преобразование поворота, передаваемое в качестве последнего аргумента, `CreateLinearGradient` эффективно поворачивает эти две точки вокруг центра экрана.

Обратите внимание, что если угол равен 0, отсутствие поворота, и двумя моментами градиента являются верхних левом и правом углов части холста. Эти точки не являются одними и теми точками градиента, как показано в предыдущем вызове `CreateLinearGradient`. Но эти точки _параллельны_ линии горизонтального градиента, бисектс центр холста, и они приводят к идентичному градиенту.

**Градиент Радуга**

Страница **градиента Радуга** рисует Радуга от левого верхнего угла холста до правого нижнего угла. Но этот градиент радуги не как реальные радуги. Это прямой, а не криволинейные, но она основана на восемь цвета HSL (оттенок насыщенность яркость), которые определяются циклически оттенок значения от 0 до 360:

```csharp
SKColor[] colors = new SKColor[8];

for (int i = 0; i < colors.Length; i++)
{
    colors[i] = SKColor.FromHsl(i * 360f / (colors.Length - 1), 100, 50);
}
```

Этот код является частью обработчика `PaintSurface`, показанного ниже. Обработчик начинается с создания путь, который определяет шести стороны многоугольника, начиная с верхнего левого угла холста в правом нижнем углу:

```csharp
public class RainbowGradientPage : ContentPage
{
    public RainbowGradientPage ()
    {
        Title = "Rainbow Gradient";

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

        using (SKPath path = new SKPath())
        {
            float rainbowWidth = Math.Min(info.Width, info.Height) / 2f;

            // Create path from upper-left to lower-right corner
            path.MoveTo(0, 0);
            path.LineTo(rainbowWidth / 2, 0);
            path.LineTo(info.Width, info.Height - rainbowWidth / 2);
            path.LineTo(info.Width, info.Height);
            path.LineTo(info.Width - rainbowWidth / 2, info.Height);
            path.LineTo(0, rainbowWidth / 2);
            path.Close();

            using (SKPaint paint = new SKPaint())
            {
                SKColor[] colors = new SKColor[8];

                for (int i = 0; i < colors.Length; i++)
                {
                    colors[i] = SKColor.FromHsl(i * 360f / (colors.Length - 1), 100, 50);
                }

                paint.Shader = SKShader.CreateLinearGradient(
                                    new SKPoint(0, rainbowWidth / 2), 
                                    new SKPoint(rainbowWidth / 2, 0),
                                    colors,
                                    null,
                                    SKShaderTileMode.Repeat);

                canvas.DrawPath(path, paint);
            }
        }
    }
}
```

Две точки градиента в методе `CreateLinearGradient` основаны на двух точках, определяющих этот путь: обе точки находятся ближе к левому верхнему углу. Во-первых, на верхней границе холста, а второй — в левой части холста. Ниже приведен результат:

[![Сбой градиента Радуга](linear-gradient-images/RainbowGradientFaulty.png "Сбой градиента Радуга")](linear-gradient-images/RainbowGradientFaulty-Large.png#lightbox)

Это интересный образ, но это не совсем цель. Проблема в том, что при создании линейный градиент, строки постоянный цвет перпендикулярная линии градиента. Линии градиента зависит от точки, где рисунке касается верхней и левой границ, и эту строку не перпендикулярной к краям фигуры, которые отображают в нижнем правом углу. Этот подход будет работать только в том случае, если холст были square.

Для создания правильного радуги градиента, должен быть перпендикулярной к краю радуги линии градиента. Это более сложного вычисления. Вектор должен быть определен, параллельное длинной стороны фигуры. Вектор — Повернуть на 90 градусов, так как это перпендикулярной к этой стороне. Затем она становится шириной фигуры, умноженной на `rainbowWidth`. Две точки градиента вычисляются на основе точки на стороне рисунке, и, которые указывают плюс вектора. Ниже приведен код, отображаемый на странице " **градиентная шкала** " в образце [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) :

```csharp
public class RainbowGradientPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        using (SKPath path = new SKPath())
        {
            ···
            using (SKPaint paint = new SKPaint())
            {
                ···
                // Vector on lower-left edge, from top to bottom 
                SKPoint edgeVector = new SKPoint(info.Width - rainbowWidth / 2, info.Height) - 
                                     new SKPoint(0, rainbowWidth / 2);

                // Rotate 90 degrees counter-clockwise:
                SKPoint gradientVector = new SKPoint(edgeVector.Y, -edgeVector.X);

                // Normalize
                float length = (float)Math.Sqrt(Math.Pow(gradientVector.X, 2) +
                                                Math.Pow(gradientVector.Y, 2));
                gradientVector.X /= length;
                gradientVector.Y /= length;

                // Make it the width of the rainbow
                gradientVector.X *= rainbowWidth;
                gradientVector.Y *= rainbowWidth;

                // Calculate the two points
                SKPoint point1 = new SKPoint(0, rainbowWidth / 2);
                SKPoint point2 = point1 + gradientVector;

                paint.Shader = SKShader.CreateLinearGradient(point1,
                                                             point2,
                                                             colors,
                                                             null,
                                                             SKShaderTileMode.Repeat);

                canvas.DrawPath(path, paint);
            }
        }
    }
}
```

Теперь цвета радуги выравнивается рисунке:

[![Градиент Радуга](linear-gradient-images/RainbowGradient.png "Градиент Радуга")](linear-gradient-images/RainbowGradient-Large.png#lightbox)

**Цвета бесконечности**

Градиент Радуга также используется на странице **цвета бесконечности** . На этой странице рисуется знак бесконечности с использованием объекта пути, описанного в статье [**три типа кривых Безье**](../../curves/beziers.md#bezier-curve-approximation-to-circular-arcs). Затем изображение окрашивается градиентом анимированных радуги, которая постоянно выполняет очистку по изображению.

Конструктор создает объект `SKPath`, описывающий знак бесконечности. После создания пути конструктор можно также получить прямоугольник, ограничивающий путь. Затем вычисляется значение с именем `gradientCycleLength`. Если градиент основан на верхнем левом и нижнем правом углу `pathBounds` прямоугольника, это `gradientCycleLength` значение является общей горизонтальной шириной шаблона градиента:

```csharp
public class InfinityColorsPage : ContentPage
{
    ···
    SKCanvasView canvasView;

    // Path information 
    SKPath infinityPath;
    SKRect pathBounds;
    float gradientCycleLength;

    // Gradient information
    SKColor[] colors = new SKColor[8];
    ···

    public InfinityColorsPage ()
    {
        Title = "Infinity Colors";

        // Create path for infinity sign
        infinityPath = new SKPath();
        infinityPath.MoveTo(0, 0);                                  // Center
        infinityPath.CubicTo(  50,  -50,   95, -100,  150, -100);   // To top of right loop
        infinityPath.CubicTo( 205, -100,  250,  -55,  250,    0);   // To far right of right loop
        infinityPath.CubicTo( 250,   55,  205,  100,  150,  100);   // To bottom of right loop
        infinityPath.CubicTo(  95,  100,   50,   50,    0,    0);   // Back to center  
        infinityPath.CubicTo( -50,  -50,  -95, -100, -150, -100);   // To top of left loop
        infinityPath.CubicTo(-205, -100, -250,  -55, -250,    0);   // To far left of left loop
        infinityPath.CubicTo(-250,   55, -205,  100, -150,  100);   // To bottom of left loop
        infinityPath.CubicTo( -95,  100, - 50,   50,    0,    0);   // Back to center
        infinityPath.Close();

        // Calculate path information 
        pathBounds = infinityPath.Bounds;
        gradientCycleLength = pathBounds.Width +
            pathBounds.Height * pathBounds.Height / pathBounds.Width;

        // Create SKColor array for gradient
        for (int i = 0; i < colors.Length; i++)
        {
            colors[i] = SKColor.FromHsl(i * 360f / (colors.Length - 1), 100, 50);
        }

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ···
}
```

Конструктор также создает массив `colors` для Радуга и объект `SKCanvasView`.

Переопределения методов `OnAppearing` и `OnDisappearing` выполняют дополнительную нагрузку на анимацию. Метод `OnTimerTick` анимируется поле `offset` от 0 до `gradientCycleLength` каждые две секунды:

```csharp
public class InfinityColorsPage : ContentPage
{
    ···
    // For animation
    bool isAnimating;
    float offset;
    Stopwatch stopwatch = new Stopwatch();
    ···

    protected override void OnAppearing()
    {
        base.OnAppearing();

        isAnimating = true;
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();

        stopwatch.Stop();
        isAnimating = false;
    }

    bool OnTimerTick()
    {
        const int duration = 2;     // seconds
        double progress = stopwatch.Elapsed.TotalSeconds % duration / duration;
        offset = (float)(gradientCycleLength * progress);
        canvasView.InvalidateSurface();

        return isAnimating;
    }
    ···
}
```

Наконец, обработчик `PaintSurface` отображает знак бесконечности. Поскольку контур содержит отрицательные и положительные координаты вокруг центральной точки (0, 0), для сдвига в центр используется `Translate`ное преобразование на холсте. За преобразованием преобразования следует преобразование `Scale`, которое применяет коэффициент масштабирования, который делает знак бесконечности максимально большим, при этом продолжая оставаться в пределах 95% ширины и высоты холста. 

Обратите внимание, что `STROKE_WIDTH` константа добавляется к ширине и высоте ограничивающего прямоугольника контура. Путь будет быть нарисован с использованием строки эта ширина, готовый для просмотра бесконечность размер увеличивается на половинной ширины, что все четыре стороны:

```csharp
public class InfinityColorsPage : ContentPage
{
    const int STROKE_WIDTH = 50;
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Set transforms to shift path to center and scale to canvas size
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(0.95f * 
            Math.Min(info.Width / (pathBounds.Width + STROKE_WIDTH),
                     info.Height / (pathBounds.Height + STROKE_WIDTH)));

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.StrokeWidth = STROKE_WIDTH;
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(pathBounds.Left, pathBounds.Top),
                                new SKPoint(pathBounds.Right, pathBounds.Bottom),
                                colors,
                                null,
                                SKShaderTileMode.Repeat,
                                SKMatrix.MakeTranslation(offset, 0));

            canvas.DrawPath(infinityPath, paint);
        }
    }
}
```

Взгляните на точки, передаваемые в качестве первых двух аргументов `SKShader.CreateLinearGradient`. Эти точки основаны на исходный путь, ограничивающий прямоугольник. Первая точка — (&ndash;250, &ndash;100), а вторая — (250, 100). Внутри SkiaSharp, эти точки них распространяется текущего преобразования canvas, они были согласованы правильно входа отображается бесконечность.

Без последнего аргумента для `CreateLinearGradient`вы увидите градиент Радуга, который находится в левом верхнем углу знака бесконечности в нижнем правом углу. (На самом деле, градиента расширяется от верхнего левого угла в правый нижний угол ограничивающего прямоугольника. Отображаемый знак бесконечности больше, чем ограничивающий прямоугольник, с половиной `STROKE_WIDTH` значения на всех сторонах. Так как градиент выделяется красным цветом как в начале, так и в конце, а градиент создается с помощью `SKShaderTileMode.Repeat`, разница не заметна.)

Используя последний аргумент для `CreateLinearGradient`, шаблон градиента непрерывно вращается по изображению:

[![Цвета бесконечности](linear-gradient-images/InfinityColors.png "Цвета бесконечности")](linear-gradient-images/InfinityColors-Large.png#lightbox)

## <a name="transparency-and-gradients"></a>Прозрачности и градиентов

Цвета, способствующих градиента можно включить прозрачности. Вместо градиент, который уменьшает от одного цвета к другому градиента можно скрывать из цвета прозрачным. 

Этот метод можно использовать для некоторых интересных эффектов. Один из примеров классической показано графического объекта с помощью его отражения:

[![Градиентное отражение](linear-gradient-images/ReflectionGradient.png "Градиентное отражение")](linear-gradient-images/ReflectionGradient-Large.png#lightbox)

Текст, который ногами окрашивается с градиентом, 50%, прозрачного вверху, чтобы полностью прозрачный внизу. Эти уровни прозрачности связаны с альфа-значения 0x80 и 0.

Обработчик `PaintSurface` на странице **градиента отражения** масштабирует размер текста до 90% от ширины холста. Затем он вычисляет `xText` и `yText` значения, чтобы расположить текст горизонтально по центру, но находящийся на базовом плане, соответствующем вертикальной центральной части страницы:

```csharp
public class ReflectionGradientPage : ContentPage
{
    const string TEXT = "Reflection";

    public ReflectionGradientPage ()
    {
        Title = "Reflection Gradient";

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
            // Set text color to blue
            paint.Color = SKColors.Blue;

            // Set text size to fill 90% of width
            paint.TextSize = 100;
            float width = paint.MeasureText(TEXT);
            float scale = 0.9f * info.Width / width;
            paint.TextSize *= scale;

            // Get text bounds
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);

            // Calculate offsets to position text above center
            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2;

            // Draw unreflected text
            canvas.DrawText(TEXT, xText, yText, paint);

            // Shift textBounds to match displayed text
            textBounds.Offset(xText, yText);

            // Use those offsets to create a gradient for the reflected text
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(0, textBounds.Top),
                                new SKPoint(0, textBounds.Bottom),
                                new SKColor[] { paint.Color.WithAlpha(0),
                                                paint.Color.WithAlpha(0x80) },
                                null,
                                SKShaderTileMode.Clamp);

            // Scale the canvas to flip upside-down around the vertical center
            canvas.Scale(1, -1, 0, yText);

            // Draw reflected text
            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

Эти значения `xText` и `yText` те же значения, которые используются для отображения отраженного текста в `DrawText` вызове в нижней части обработчика `PaintSurface`. Однако перед этим кодом вы увидите вызов метода `Scale` `SKCanvas`. Этот `Scale` метод масштабируется горизонтально на 1 (что не делает ничего), но по вертикали на &ndash;1, что фактически переворачивает все перевернутые элементы. Центр вращения устанавливается в точке (0, `yText`), где `yText` является вертикальной центральной областью холста, первоначально вычисленная как `info.Height` деленная на 2.

Имейте в виду, что Skia использует градиента для цветового графических объектов до преобразования canvas. После прорисовки неотраженного текста `textBounds` прямоугольник смещается так, чтобы он соответствовал отображаемому тексту:

```csharp
textBounds.Offset(xText, yText);
```

`CreateLinearGradient` вызов определяет градиент от верхнего края этого прямоугольника до конца. Градиент находится от полностью прозрачного синего (`paint.Color.WithAlpha(0)`) до 50% прозрачного синего (`paint.Color.WithAlpha(0x80)`). Преобразование canvas зеркальное отражение текста вверх ногами, прозрачного синюю 50% начинается с базового плана, поэтому становится прозрачной в верхней части текста.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
