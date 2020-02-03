---
title: Простая анимация в SkiaSharp
description: В этой статье объясняется, как анимация графики SkiaSharp в Xamarin.Forms приложений и демонстрирует это с помощью примера кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 31C96FD6-07E4-4473-A551-24753A5118C3
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 80de16a0cf9b601ac3795085b638b9d62812f4d9
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725554"
---
# <a name="basic-animation-in-skiasharp"></a>Простая анимация в SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Узнайте, как анимировать график SkiaSharp_

Вы можете анимировать график SkiaSharp в Xamarin. Forms, вызывая периодический вызов метода `PaintSurface`, каждый раз рисуя графику немного иначе. Ниже приведен анимацию, описанных далее в этой статье с концентрических окружностей, казалось бы, разверните в центре.

![](animation-images/animationexample.png "Several concentric circles seemingly expanding from the center")

Страница **эллипса пулсатинг** в программе [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) анимирует две оси эллипса, чтобы они были пулсатинг, и вы даже можете контролировать скорость этого пулсатион. Файл [**пулсатинжеллипсепаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml) создает экземпляр Xamarin. forms `Slider` и `Label` для вывода текущего значения ползунка. Это распространенный способ интеграции `SKCanvasView` с другими представлениями Xamarin. Forms:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.PulsatingEllipsePage"
             Title="Pulsating Ellipse">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Slider x:Name="slider"
                Grid.Row="0"
                Maximum="10"
                Minimum="0.1"
                Value="5"
                Margin="20, 0" />

        <Label Grid.Row="1"
               Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='Cycle time = {0:F1} seconds'}"
               HorizontalTextAlignment="Center" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="2"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Файл кода программной части создает экземпляр объекта `Stopwatch`, который служит часами высокой точности. Переопределение `OnAppearing` задает для поля `pageIsActive` значение `true` и вызывает метод с именем `AnimationLoop`. `OnDisappearing` переопределяет, `pageIsActive` поле `false`:

```csharp
Stopwatch stopwatch = new Stopwatch();
bool pageIsActive;
float scale;            // ranges from 0 to 1 to 0

public PulsatingEllipsePage()
{
    InitializeComponent();
}

protected override void OnAppearing()
{
    base.OnAppearing();
    pageIsActive = true;
    AnimationLoop();
}

protected override void OnDisappearing()
{
    base.OnDisappearing();
    pageIsActive = false;
}
```

Метод `AnimationLoop` запускает `Stopwatch`, а затем циклы, пока `pageIsActive` `true`. По сути это «бесконечный цикл», когда страница активна, но это не приводит к зависанию программы, так как цикл завершается вызовом `Task.Delay` с помощью оператора `await`, который позволяет другим частям программной функции. Аргумент для `Task.Delay` приводит к его завершению через 1/30-й секунды. Этот параметр определяет частоту кадров анимации.

```csharp
async Task AnimationLoop()
{
    stopwatch.Start();

    while (pageIsActive)
    {
        double cycleTime = slider.Value;
        double t = stopwatch.Elapsed.TotalSeconds % cycleTime / cycleTime;
        scale = (1 + (float)Math.Sin(2 * Math.PI * t)) / 2;
        canvasView.InvalidateSurface();
        await Task.Delay(TimeSpan.FromSeconds(1.0 / 30));
    }

    stopwatch.Stop();
}

```

Цикл `while` начинается с получения времени цикла из `Slider`. Это время в секундах, например, 5. Вторая инструкция вычисляет значение `t` для *времени*. Для `cycleTime` 5 `t` увеличивается от 0 до 1 каждые 5 секунд. Аргумент функции `Math.Sin` во второй инструкции находится в диапазоне от 0 до 2π каждые 5 секунд. Функция `Math.Sin` возвращает значение в диапазоне от 0 до 1 до 0, а затем в &ndash;1 и 0 каждые 5 секунд, но со значениями, которые меняются медленнее, если значение приближается к 1 или – 1. Значение 1 добавляется, поэтому значения всегда имеют положительное значение, а затем он делится на 2, поэтому диапазон значений: от ½ значение 1, чтобы ½ равным 0, чтобы ½, но медленнее, если значение равно приблизительно 1 и 0. Он хранится в поле `scale`, а `SKCanvasView` становится недействительным.

Метод `PaintSurface` использует это `scale` значение для вычисления двух осей эллипса:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float maxRadius = 0.75f * Math.Min(info.Width, info.Height) / 2;
    float minRadius = 0.25f * maxRadius;

    float xRadius = minRadius * scale + maxRadius * (1 - scale);
    float yRadius = maxRadius * scale + minRadius * (1 - scale);

    using (SKPaint paint = new SKPaint())
    {
        paint.Style = SKPaintStyle.Stroke;
        paint.Color = SKColors.Blue;
        paint.StrokeWidth = 50;
        canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);

        paint.Style = SKPaintStyle.Fill;
        paint.Color = SKColors.SkyBlue;
        canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);
    }
}
```

Метод вычисляет максимальное radius, в зависимости от размера области отображения и минимальный радиус, в зависимости от максимального радиуса. Значение `scale` анимируется от 0 до 1 и возвращается к 0, поэтому метод использует его для вычислений `xRadius` и `yRadius`, которые находятся между `minRadius` и `maxRadius`. Эти значения используются для рисования и заливка эллипса:

[![](animation-images/pulsatingellipse-small.png "Triple screenshot of the Pulsating Ellipse page")](animation-images/pulsatingellipse-large.png#lightbox "Triple screenshot of the Pulsating Ellipse page")

Обратите внимание, что объект `SKPaint` создается в блоке `using`. Как и многие SkiaSharp классы `SKPaint` являются производными от `SKObject`, который является производным от `SKNativeObject`, который реализует интерфейс [`IDisposable`](xref:System.IDisposable) . `SKPaint` переопределяет метод `Dispose`, чтобы освободить неуправляемые ресурсы.

 Помещение `SKPaint` в блок `using` гарантирует, что `Dispose` будет вызван в конце блока, чтобы освободить эти неуправляемые ресурсы. Это происходит в любом случае, когда память, используемая объектом `SKPaint`, освобождается сборщиком мусора .NET, но в коде анимации лучше проактивно освободить память более упорядоченным образом.

 Лучшим решением в этом случае будет создание двух `SKPaint` объектов один раз и сохранение их в виде полей.

Это именно то, что делает **расширяемая круговая** анимация. Класс [`ExpandingCirclesPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ExpandingCirclesPage.cs) начинается с определения нескольких полей, включая объект `SKPaint`:

```csharp
public class ExpandingCirclesPage : ContentPage
{
    const double cycleTime = 1000;       // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float t;
    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke
    };

    public ExpandingCirclesPage()
    {
        Title = "Expanding Circles";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ...
}
```

Эта программа использует другой подход к анимации на основе метода Xamarin. Forms `Device.StartTimer`. Поле `t` анимируется от 0 до 1 каждые `cycleTime` миллисекунд:

```csharp
public class ExpandingCirclesPage : ContentPage
{
    ...
    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;
        stopwatch.Start();

        Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
        {
            t = (float)(stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime);
            canvasView.InvalidateSurface();

            if (!pageIsActive)
            {
                stopwatch.Stop();
            }
            return pageIsActive;
        });
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        pageIsActive = false;
    }
    ...
}
```

Обработчик `PaintSurface` формирует пять концентрических кругов с анимированными радиусами. Если `baseRadius` переменная вычисляется как 100, то `t` анимируется от 0 до 1, радиусы пяти кругов увеличиваются с 0 до 100, 100 — до 200, от 200 до 300, от 300 до 400 и от 400 до 500. Для большей части кругов `strokeWidth` равен 50, но для первого круга `strokeWidth` анимируется от 0 до 50. Для большинства кругов синий цвет, но для последнего окружности, цвет анимируется от синего к прозрачности. Обратите внимание на четвертый аргумент конструктора `SKColor`, который задает непрозрачность.

```csharp
public class ExpandingCirclesPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
        float baseRadius = Math.Min(info.Width, info.Height) / 12;

        for (int circle = 0; circle < 5; circle++)
        {
            float radius = baseRadius * (circle + t);

            paint.StrokeWidth = baseRadius / 2 * (circle == 0 ? t : 1);
            paint.Color = new SKColor(0, 0, 255,
                (byte)(255 * (circle == 4 ? (1 - t) : 1)));

            canvas.DrawCircle(center.X, center.Y, radius, paint);
        }
    }
}
```

В результате изображение выглядит одинаково, когда `t` равно 0, как если `t` равно 1, а окружности продолжают развертываться непрерывно:

[![](animation-images/expandingcircles-small.png "Triple screenshot of the Expanding Circles page")](animation-images/expandingcircles-large.png#lightbox "Triple screenshot of the Expanding Circles page")

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
