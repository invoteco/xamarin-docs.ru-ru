---
title: SkiaSharp шум и составление
description: Создание шейдеров шума в Perl и объединение с другими шейдерами.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 90C2D00A-2876-43EA-A836-538C3318CF93
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: c1e500936b89f2ec8dc17279a7ed878dc7f5cbb3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029439"
---
# <a name="skiasharp-noise-and-composing"></a>SkiaSharp шум и составление

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Простая векторная графика, как правило, выглядит неестественным. Прямые линии, гладкие кривые и сплошные цвета не похожи на реальные объекты. Работая с созданными компьютером графическими изображениями для 1982 Movie _трон_, компьютер анализу Алексей Perl начал разрабатывать алгоритмы, использующие случайные процессы для предоставления этих изображений более реалистичным текстурам. В 1997 Алексей Perl выиграл Academy награду за технические достижения. Его работа известна как шум Perl и поддерживается в SkiaSharp. Ниже приведен пример:

![Пример шума на Perl](noise-images/NoiseSample.png "Пример шума на Perl")

Как видите, каждый пиксель не является случайным значением цвета. Непрерывность в результате от пикселя до пикселя приводит к возникновению случайных фигур.

Поддержка шума в Perl в СКИА основана на спецификации W3C для CSS и SVG. Раздел 8,20 из [**модуля эффектов фильтра уровень 1**](https://www.w3.org/TR/filter-effects-1/#feTurbulenceElement) включает в себя базовые алгоритмы для Perl в коде на языке C.

## <a name="exploring-perlin-noise"></a>Исследование шума в Perl

Класс [`SKShader`](xref:SkiaSharp.SKShader) определяет два разных статических метода для создания шума perl: [`CreatePerlinNoiseFractalNoise`](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise*) и [`CreatePerlinNoiseTurbulence`](xref:SkiaSharp.SKShader.CreatePerlinNoiseTurbulence*). Параметры идентичны:

```csharp
public static SkiaSharp CreatePerlinNoiseFractalNoise (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed);

public static SkiaSharp.SKShader CreatePerlinNoiseTurbulence (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed);
```

Оба метода также существуют в перегруженных версиях с дополнительным параметром `SKPointI`. Эти перегрузки обсуждаются в разделе [**мозаичный шум на Perl**](#tiling-perlin-noise) .

Два аргумента `baseFrequency` являются положительными значениями, определенными в документации SkiaSharp, как в диапазоне от 0 до 1, но для них также можно задать более высокие значения. Чем выше значение, тем больше изменение в случайном изображении в горизонтальном и вертикальном направлениях.

Значение `numOctaves` является целым числом, равному 1 или выше. Он связан с коэффициентом итерации в алгоритмах. Каждый дополнительный Октава повлияет на пополовину предыдущего Октава, поэтому этот результат сокращается с помощью более высоких значений Октава.

Параметр `seed` является отправной точкой генератора случайных чисел. Хотя в качестве значения с плавающей запятой, дробная часть усекается до ее использования, а значение 0 равно 1.

На странице « **шум Perl** » в [ **скиашарпформсдемос**)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) можно экспериментировать с различными значениями `baseFrequency` и `numOctaves` аргументов. Вот файл XAML:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.PerlinNoisePage"
             Title="Perlin Noise">

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="baseFrequencyXSlider"
                Maximum="4"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="baseFrequencyXText"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="baseFrequencyYSlider"
                Maximum="4"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="baseFrequencyYText"
               HorizontalTextAlignment="Center" />

        <StackLayout Orientation="Horizontal"
                     HorizontalOptions="Center"
                     Margin="10">

            <Label Text="{Binding Source={x:Reference octavesStepper},
                                  Path=Value,
                                  StringFormat='Number of Octaves: {0:F0}'}"
                   VerticalOptions="Center" />

            <Stepper x:Name="octavesStepper"
                     Minimum="1"
                     ValueChanged="OnStepperValueChanged" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

В нем используются два представления `Slider` для двух `baseFrequency` аргументов. Чтобы увеличить диапазон нижних значений, ползунки являются логарифмами. Файл кода программной части вычисляет аргументы для `SKShader`методов от степеней значений `Slider`. В представлениях `Label` отображаются вычисляемые значения:

```csharp
float baseFreqX = (float)Math.Pow(10, baseFrequencyXSlider.Value - 4);
baseFrequencyXText.Text = String.Format("Base Frequency X = {0:F4}", baseFreqX);

float baseFreqY = (float)Math.Pow(10, baseFrequencyYSlider.Value - 4);
baseFrequencyYText.Text = String.Format("Base Frequency Y = {0:F4}", baseFreqY);
```

`Slider` значение 1 соответствует 0,001, `Slider` значение OS 2 соответствует 0,01, `Slider` значения 3 соответствует 0,1, а `Slider` значение 4 соответствует 1.

Вот файл кода программной части, включающий этот код:

```csharp
public partial class PerlinNoisePage : ContentPage
{
    public PerlinNoisePage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnStepperValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Get values from sliders and stepper
        float baseFreqX = (float)Math.Pow(10, baseFrequencyXSlider.Value - 4);
        baseFrequencyXText.Text = String.Format("Base Frequency X = {0:F4}", baseFreqX);

        float baseFreqY = (float)Math.Pow(10, baseFrequencyYSlider.Value - 4);
        baseFrequencyYText.Text = String.Format("Base Frequency Y = {0:F4}", baseFreqY);

        int numOctaves = (int)octavesStepper.Value;

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader =
                SKShader.CreatePerlinNoiseFractalNoise(baseFreqX,
                                                       baseFreqY,
                                                       numOctaves,
                                                       0);

            SKRect rect = new SKRect(0, 0, info.Width, info.Height / 2);
            canvas.DrawRect(rect, paint);

            paint.Shader =
                SKShader.CreatePerlinNoiseTurbulence(baseFreqX,
                                                     baseFreqY,
                                                     numOctaves,
                                                     0);

            rect = new SKRect(0, info.Height / 2, info.Width, info.Height);
            canvas.DrawRect(rect, paint);
        }
    }
}
```

Вот программа, выполняемая на устройствах iOS, Android и универсальная платформа Windows (UWP). Шум фрактала отображается в верхней половине холста. Шум турбуленце находится в нижней половине:

[![Шум Perl](noise-images/PerlinNoise.png "Шум Perl")](noise-images/PerlinNoise-Large.png#lightbox)

Те же аргументы всегда создают тот же шаблон, который начинается в левом верхнем углу. Такая согласованность очевидна при изменении ширины и высоты окна UWP. Когда Windows 10 перерисовывает экран, шаблон в верхней половине холста остается неизменным.

Шаблон шума включает различные степени прозрачности. Прозрачность станет очевидной, если задать цвет в вызове `canvas.Clear()`. Этот цвет станет заметным в шаблоне. Вы также увидите этот результат в разделе [**Объединение нескольких шейдеров**](#combining-multiple-shaders).

Эти шаблоны шума Perl редко используются сами по себе. Часто они накладываются на режимы наложения и фильтры цветов, обсуждаемые в последующих статьях.

## <a name="tiling-perlin-noise"></a>Разбиение шума на Perl

Два статических `SKShader` методов для создания шума в Perl также существуют в перегрузках версий. Перегрузки [`CreatePerlinNoiseFractalNoise`](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise(System.Single,System.Single,System.Int32,System.Single,SkiaSharp.SKPointI)) и [`CreatePerlinNoiseTurbulence`](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise(System.Single,System.Single,System.Int32,System.Single,SkiaSharp.SKPointI)) имеют дополнительный параметр `SKPointI`:

```csharp
public static SKShader CreatePerlinNoiseFractalNoise (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed, SKPointI tileSize);

public static SKShader CreatePerlinNoiseTurbulence (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed, SKPointI tileSize);
```

Структура [`SKPointI`](xref:SkiaSharp.SKPointI) — это целочисленная версия знакомой структуры [`SKPoint`](xref:SkiaSharp.SKPoint) . `SKPointI` определяет свойства `X` и `Y` типа `int`, а не `float`.

Эти методы создают Повторяющийся шаблон указанного размера. В каждой плитке правый край совпадает с левым ребром, а верхний край — с нижней границей. Эта характеристика показана на странице « **мозаичные помехи Perl** ». XAML-файл подобен предыдущему примеру, но он имеет только `Stepper` представление для изменения аргумента `seed`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.TiledPerlinNoisePage"
             Title="Tiled Perlin Noise">

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <StackLayout Orientation="Horizontal"
                     HorizontalOptions="Center"
                     Margin="10">

            <Label Text="{Binding Source={x:Reference seedStepper},
                                  Path=Value,
                                  StringFormat='Seed: {0:F0}'}"
                   VerticalOptions="Center" />

            <Stepper x:Name="seedStepper"
                     Minimum="1"
                     ValueChanged="OnStepperValueChanged" />

        </StackLayout>
    </StackLayout>
</ContentPage>
```

Файл кода программной части определяет константу для размера плитки. Обработчик `PaintSurface` создает точечный рисунок этого размера и `SKCanvas` для рисования в этом растровом изображении. Метод `SKShader.CreatePerlinNoiseTurbulence` создает шейдер с размером плитки. Этот шейдер рисуется на точечном рисунке:

```csharp
public partial class TiledPerlinNoisePage : ContentPage
{
    const int TILE_SIZE = 200;

    public TiledPerlinNoisePage()
    {
        InitializeComponent();
    }

    void OnStepperValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Get seed value from stepper
        float seed = (float)seedStepper.Value;

        SKRect tileRect = new SKRect(0, 0, TILE_SIZE, TILE_SIZE);

        using (SKBitmap bitmap = new SKBitmap(TILE_SIZE, TILE_SIZE))
        {
            using (SKCanvas bitmapCanvas = new SKCanvas(bitmap))
            {
                bitmapCanvas.Clear();

                // Draw tiled turbulence noise on bitmap
                using (SKPaint paint = new SKPaint())
                {
                    paint.Shader = SKShader.CreatePerlinNoiseTurbulence(
                                        0.02f, 0.02f, 1, seed,
                                        new SKPointI(TILE_SIZE, TILE_SIZE));

                    bitmapCanvas.DrawRect(tileRect, paint);
                }
            }

            // Draw tiled bitmap shader on canvas
            using (SKPaint paint = new SKPaint())
            {
                paint.Shader = SKShader.CreateBitmap(bitmap,
                                                     SKShaderTileMode.Repeat,
                                                     SKShaderTileMode.Repeat);
                canvas.DrawRect(info.Rect, paint);
            }

            // Draw rectangle showing tile
            using (SKPaint paint = new SKPaint())
            {
                paint.Style = SKPaintStyle.Stroke;
                paint.Color = SKColors.Black;
                paint.StrokeWidth = 2;

                canvas.DrawRect(tileRect, paint);
            }
        }
    }
}
```

После создания точечного рисунка используется другой объект `SKPaint` для создания шаблона мозаичного точечного рисунка путем вызова `SKShader.CreateBitmap`. Обратите внимание на два аргумента `SKShaderTileMode.Repeat`:

```csharp
paint.Shader = SKShader.CreateBitmap(bitmap,
                                     SKShaderTileMode.Repeat,
                                     SKShaderTileMode.Repeat);
```

Этот шейдер используется для покрытия холста. Наконец, другой объект `SKPaint` используется для обводки прямоугольника, показывающего размер исходного растрового изображения.

Из пользовательского интерфейса выбирается только параметр `seed`. Если на каждой платформе используется один и тот же шаблон `seed`, они будут показывать один и тот же шаблон. Разные значения `seed` приводят к различным шаблонам:

[![Мозаичный шум в Perl](noise-images/TiledPerlinNoise.png "Мозаичный шум в Perl")](noise-images/TiledPerlinNoise-Large.png#lightbox)

Шаблон 200-пиксельного квадрата в левом верхнем углу полностью переносится в другие плитки.

## <a name="combining-multiple-shaders"></a>Объединение нескольких шейдеров

Класс `SKShader` включает метод [`CreateColor`](xref:SkiaSharp.SKShader.CreateColor*) , который создает шейдер с заданным сплошным цветом. Этот шейдер не очень полезен, так как его можно просто установить в качестве значения свойства `Color` объекта `SKPaint` и установить свойство `Shader` в значение null.

Этот метод `CreateColor` будет полезен в другом методе, который `SKShader` определяет. Этот метод является [`CreateCompose`ным ](xref:SkiaSharp.SKShader.CreateCompose(SkiaSharp.SKShader,SkiaSharp.SKShader)), объединяющим два шейдера. Ниже приведен синтаксис.

```csharp
public static SKShader CreateCompose (SKShader dstShader, SKShader srcShader);
```

`srcShader` (исходный шейдер) фактически нарисовывается поверх `dstShader` (целевой шейдер). Если исходный шейдер является сплошным цветом или градиентом без прозрачности, целевой шейдер будет полностью скрыт.

Шейдер шума Perl содержит прозрачность. Если этот шейдер является источником, шейдер назначения будет отображаться через прозрачные области.

На странице **составной страницы Perl** имеется XAML-файл, который практически идентичен первой странице **шума Perl** . Файл кода программной части также аналогичен. Но исходная страница **Perl «шум** » устанавливает свойство `Shader` `SKPaint` в шейдер, возвращаемый статическими методами `CreatePerlinNoiseFractalNoise` и `CreatePerlinNoiseTurbulence`. Эта страница, **состоящая из Perl** , вызывает `CreateCompose` для комбинированного шейдера. Назначение — это сплошной синий шейдер, созданный с помощью `CreateColor`. Источником является шейдер шума Perl:

```csharp
public partial class ComposedPerlinNoisePage : ContentPage
{
    public ComposedPerlinNoisePage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnStepperValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Get values from sliders and stepper
        float baseFreqX = (float)Math.Pow(10, baseFrequencyXSlider.Value - 4);
        baseFrequencyXText.Text = String.Format("Base Frequency X = {0:F4}", baseFreqX);

        float baseFreqY = (float)Math.Pow(10, baseFrequencyYSlider.Value - 4);
        baseFrequencyYText.Text = String.Format("Base Frequency Y = {0:F4}", baseFreqY);

        int numOctaves = (int)octavesStepper.Value;

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateCompose(
                SKShader.CreateColor(SKColors.Blue),
                SKShader.CreatePerlinNoiseFractalNoise(baseFreqX,
                                                       baseFreqY,
                                                       numOctaves,
                                                       0));

            SKRect rect = new SKRect(0, 0, info.Width, info.Height / 2);
            canvas.DrawRect(rect, paint);

            paint.Shader = SKShader.CreateCompose(
                SKShader.CreateColor(SKColors.Blue),
                SKShader.CreatePerlinNoiseTurbulence(baseFreqX,
                                                     baseFreqY,
                                                     numOctaves,
                                                     0));

            rect = new SKRect(0, info.Height / 2, info.Width, info.Height);
            canvas.DrawRect(rect, paint);
        }
    }
}
```

Шейдер шума фрактала Мандельброта находится в верхней части; Шейдер турбуленце находится в нижней части:

[![Состоялся шум Perl](noise-images/ComposedPerlinNoise.png "Состоялся шум Perl")](noise-images/ComposedPerlinNoise-Large.png#lightbox)

Обратите внимание на то, насколько синим являются эти шейдеры, отличные от тех, которые отображаются на странице **помехи Perl** . Разница показывает степень прозрачности в шейдерах шума.

Существует также перегрузка метода [`CreateCompose`](xref:SkiaSharp.SKShader.CreateCompose(SkiaSharp.SKShader,SkiaSharp.SKShader,SkiaSharp.SKBlendMode)) :

```csharp
public static SKShader CreateCompose (SKShader dstShader, SKShader srcShader, SKBlendMode blendMode);
```

Последний параметр является членом перечисления `SKBlendMode` — перечисление с 29 членами, которое обсуждается в следующей серии статей по [**SkiaSharp композиции и режимам смешения**](../blend-modes/index.md).

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
