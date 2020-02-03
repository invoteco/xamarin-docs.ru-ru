---
title: Преобразование масштаба
description: Thhis статья исследует преобразование SkiaSharp масштабирования для масштабирования объектов для различных размеров и демонстрирует это с помощью примера кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 54A43F3D-9DA8-44A7-9AE4-7E3025129A0B
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2017
ms.openlocfilehash: 7dc7a2faabef86aa63b52739edda696efcb54aba
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724250"
---
# <a name="the-scale-transform"></a>Преобразование масштаба

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Обнаружение преобразования масштабирования SkiaSharp для масштабирования объектов до различных размеров_

Как видно из статьи [**Преобразование преобразований**](translate.md) , преобразование «миграция» может перемещать графический объект из одного расположения в другое. Напротив преобразование масштаба изменяет размер графического объекта:

![](scale-images/scaleexample.png "A tall word scaled in size")

Преобразование масштаба также часто вызывает ошибки в координаты графики для перемещения, так как они были сделаны большего размера.

Ранее вы видели две формулы преобразования, описывающие влияние коэффициентов перевода `dx` и `dy`:

x' = x + dx

y "= y + dy

Масштабные коэффициенты `sx` и `sy` являются мультипликативные, а не аддитивными:

x' = [Default] sx x

y "= sy [Default] y

Значения по умолчанию факторов translate равны 0; коэффициенты масштабирования значения по умолчанию — 1.

Класс `SKCanvas` определяет четыре метода `Scale`. Первый метод [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single)) предназначен для случаев, когда требуется один и тот же коэффициент горизонтального и вертикального масштабирования:

```csharp
public void Scale (Single s)
```

Это называется *исотропик* масштабирование &mdash; масштабирование в обоих направлениях. Масштабирование мощности сохраняет пропорции объекта.

Второй метод [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single)) позволяет указать различные значения горизонтального и вертикального масштабирования:

```csharp
public void Scale (Single sx, Single sy)
```

Это приводит к некоторому *масштабированию* .
Третий метод [`Scale`](xref:SkiaSharp.SKCanvas.Scale(SkiaSharp.SKPoint)) сочетает два фактора масштабирования в одном `SKPoint`ном значении:

```csharp
public void Scale (SKPoint size)
```

Четвертый метод `Scale` будет описан в ближайшее время.

На странице " **базовый масштаб** " показан метод `Scale`. Файл [**басикскалепаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml) содержит два элемента `Slider`, которые позволяют выбрать коэффициенты горизонтального и вертикального масштабирования от 0 до 10. Файл кода программной части [**BasicScalePage.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml.cs) использует эти значения для вызова `Scale` перед отображением скругленного прямоугольника, обозначенного пунктирной линией, и размера в соответствии с текстом в левом верхнем углу холста:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear(SKColors.SkyBlue);

    using (SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] {  7, 7 }, 0)
    })
    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 50
    })
    {
        canvas.Scale((float)xScaleSlider.Value,
                     (float)yScaleSlider.Value);

        SKRect textBounds = new SKRect();
        textPaint.MeasureText(Title, ref textBounds);

        float margin = 10;
        SKRect borderRect = SKRect.Create(new SKPoint(margin, margin), textBounds.Size);
        canvas.DrawRoundRect(borderRect, 20, 20, strokePaint);
        canvas.DrawText(Title, margin, -textBounds.Top + margin, textPaint);
    }
}
```

Может возникнуть вопрос: как коэффициенты масштабирования влияют на значение, возвращаемое методом `MeasureText` `SKPaint`? Ответ: вообще не. `Scale` является методом `SKCanvas`. Он не влияет на то, что вы выполняете с объектом `SKPaint`, пока вы не используете этот объект для визуализации чего-либо на холсте.

Как видите, все, что рисуется после вызова `Scale`, увеличивается пропорционально:

[![](scale-images/basicscale-small.png "Triple screenshot of the Basic Scale page")](scale-images/basicscale-large.png#lightbox "Triple screenshot of the Basic Scale page")

Текст, ширина пунктирная линия, длина дефисы в этой строке, округление углов и полями шириной 10 пикселей между краями левом и верхнем полотна и скругленный прямоугольник подвержены все же коэффициенты масштабирования.

> [!IMPORTANT]
> Универсальная платформа Windows не отображаться должным образом anisotropicly масштабированного текста.

Анизотропная масштабирование причины ширина мазков станет разных линий, согласован с горизонтальной и вертикальной осей. (Это также очевидно из первого изображения на этой странице.) Если коэффициенты масштабирования не должны влиять на толщину штриха, задайте для него значение 0, и это всегда будет иметь ширину в один пиксель, независимо от значения параметра `Scale`.

Масштабирование — это относительно левого верхнего угла холста. Это может оказаться именно то, что нужно, но не всегда. Предположим, что нужно поместить текст и прямоугольник в другое место на холсте для его масштабирования относительно ее центра. В этом случае можно использовать четвертую версию метода [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single)) , который включает два дополнительных параметра для указания центра масштабирования:

```csharp
public void Scale (Single sx, Single sy, Single px, Single py)
```

Параметры `px` и `py` определяют точку, которая иногда называется *центром масштабирования* , но в документации SkiaSharp называется *точкой вращения*. Это точка относительно левого верхнего угла холста, который не зависит от масштабирования. Все операции масштабирования происходит относительно этого центра.

На странице [**центрированный масштаб**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/CenteredScalePage.xaml.cs) показано, как это работает. Обработчик `PaintSurface` похож на **обычную программу масштабирования** , за исключением того, что `margin` значение вычисляется для центрирования текста по горизонтали, что означает, что программа лучше работает в книжной ориентации:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear(SKColors.SkyBlue);

    using (SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] { 7, 7 }, 0)
    })
    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 50
    })
    {
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(Title, ref textBounds);
        float margin = (info.Width - textBounds.Width) / 2;

        float sx = (float)xScaleSlider.Value;
        float sy = (float)yScaleSlider.Value;
        float px = margin + textBounds.Width / 2;
        float py = margin + textBounds.Height / 2;

        canvas.Scale(sx, sy, px, py);

        SKRect borderRect = SKRect.Create(new SKPoint(margin, margin), textBounds.Size);
        canvas.DrawRoundRect(borderRect, 20, 20, strokePaint);
        canvas.DrawText(Title, margin, -textBounds.Top + margin, textPaint);
    }
}
```

Левый верхний угол скругленного прямоугольника размещается `margin` пикселях слева от холста и `margin` пикселях сверху. Два последних аргумента для метода `Scale` задаются для этих значений плюс ширину и высоту текста, а также ширину и высоту скругленного прямоугольника. Это означает, что все операции масштабирования относительно центра этого прямоугольника:

[![](scale-images/centeredscale-small.png "Triple screenshot of the Centered Scale page")](scale-images/centeredscale-large.png#lightbox "Triple screenshot of the Centered Scale page")

Элементы `Slider` в этой программе имеют диапазон от &ndash;10 до 10. Как вы видите, отрицательные значения вертикальное масштабирование (например, в Android, экран в центре) вызвать объекты для зеркального отображения относительно горизонтальной оси, проходящих через Центр масштабирования. Отрицательные значения по горизонтали, масштабирование (например, как показано на экране UWP справа) приводят к перевернуть вокруг вертикальной оси, проходящих через Центр масштабирования объектов.

Версия метода [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single)) с точками вращения является ярлыком для ряда из трех `Translate` и `Scale` вызовов. Возможно, вы захотите увидеть, как это работает, заменив метод `Scale` на странице **центрированный масштаб** следующим образом:

```csharp
canvas.Translate(-px, -py);
```

Это отрицательных координат точек pivot.

Запустите программу повторно. Вы увидите, что прямоугольник и текстовый сдвигаются, чтобы центр находится в левом верхнем углу холста. Его можно практически невозможно просматривать. Ползунки не работают. конечно Теперь программа не масштабируется вообще.

Теперь добавьте базовый `Scale` вызов (без центра масштабирования) *перед* вызовом `Translate`:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Если вы знакомы с этого упражнения в других системах программирования графики, может показаться, что это не так, но не завершает работу. Skia обрабатывает вызовы последовательных преобразование немного по-разному из что вы можете быть знакомы с.

После последовательного вызова `Scale` и `Translate` центр скругленного прямоугольника остается в левом верхнем углу, но теперь его можно масштабировать относительно верхнего левого угла холста, который также является центром скругленного прямоугольника.

Теперь перед вызовом `Scale` добавьте еще один `Translate` вызов с центральными значениями:

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Это перемещает масштабированный результат обратно в исходное положение. Эти три вызовы эквивалентны:

```csharp
canvas.Scale(sx, sy, px, py);
```

Преобразования отдельных актуальны и таким образом, формула Общее преобразование:

 x' = [Default] sx (x – пкс) + px

 y "= sy [Default] (y — py) + py

Помните, что значения по умолчанию `sx` и `sy` равны 1. Это просто чтобы убедиться, что точки вращения (px, py), эти формулы не применяется. Она остается в том же расположении относительно холста.

При объединении вызовов `Translate` и `Scale` порядок важен. Если `Translate` поступает после `Scale`, коэффициенты перевода эффективно масштабируются по коэффициентам масштабирования. Если `Translate` предшествует `Scale`, коэффициенты перевода не масштабируются. Этот процесс несколько яснее (хотя и более математические) Если вводится предметом матрицы преобразования.

Класс `SKPath` определяет свойство [`Bounds`](xref:SkiaSharp.SKPath.Bounds) только для чтения, возвращающее `SKRect` определяющее экстент координат в пути. Например, если свойство `Bounds` получено из ранее созданного пути хендекаграм, свойства `Left` и `Top` прямоугольника приблизительно – 100, свойства `Right` и `Bottom` — приблизительно 100, а свойства `Width` и `Height` — приблизительно 200. (Большинство фактические значения, немного меньше, так как точки звездочек определяются круг с радиусом 100, но только верхней точки — параллельное выполнение с помощью горизонтальной или вертикальной оси.)

Доступность этих сведений подразумевает, что она должна быть возможность являются производными масштабирования и перевод факторов, подходящий для масштабирования путь к размер холста. На странице [**анизотропное масштабирование**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicScalingPage.cs) это показано с помощью 11-дюймовой звезды. *Анизотропная* шкала означает, что она не равна в горизонтальном и вертикальном направлениях, что означает, что звездочка не будет сохранять исходные пропорции. Ниже приведен соответствующий код в обработчике `PaintSurface`.

```csharp
SKPath path = HendecagramPage.HendecagramPath;
SKRect pathBounds = path.Bounds;

using (SKPaint fillPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Pink
})
using (SKPaint strokePaint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    Color = SKColors.Blue,
    StrokeWidth = 3,
    StrokeJoin = SKStrokeJoin.Round
})
{
    canvas.Scale(info.Width / pathBounds.Width,
                 info.Height / pathBounds.Height);
    canvas.Translate(-pathBounds.Left, -pathBounds.Top);

    canvas.DrawPath(path, fillPaint);
    canvas.DrawPath(path, strokePaint);
}
```

Прямоугольник `pathBounds` получается в верхней части этого кода, а затем используется позже с шириной и высотой холста в `Scale` вызове. Этот вызов будет масштабировать координаты пути, когда он визуализируется `DrawPath`ным вызовом, но звезда будет центрирована в правом верхнем углу холста. Он должен быть перемещен вниз и влево. Это задание вызова `Translate`. Эти два свойства `pathBounds` приблизительно – 100, поэтому коэффициенты перевода — около 100. Поскольку `Translate` вызов происходит после вызова `Scale`, эти значения эффективно масштабируются по коэффициентам масштабирования, поэтому они перемещают центр звезды в центр холста:

[![](scale-images/anisotropicscaling-small.png "Triple screenshot of the Anisotropic Scaling page")](scale-images/anisotropicscaling-large.png#lightbox "Triple screenshot of the Anisotropic Scaling page")

Кроме того, вы можете подумать о `Scale` и `Translate`ных вызовах, чтобы определить воздействие на обратную последовательность: вызов `Translate` сдвигает путь так, чтобы он стал полностью видимым, но ориентированным в левом верхнем углу холста. Затем метод `Scale` делает эту звезду больше относительно левого верхнего угла.

На самом деле похоже, что звездочки немного больше, чем холста. Проблема заключается в ширину штриха. Свойство `Bounds` `SKPath` указывает размеры координат, закодированных в пути, и именно то, что программа использует для масштабирования. При подготовке к просмотру путь с определенной толщина, готовый для просмотра путь больше, чем холста.

Чтобы устранить эту проблему необходимо компенсировать. Одним из простых подходов в этой программе является добавление следующей инструкции перед вызовом `Scale`:

```csharp
pathBounds.Inflate(strokePaint.StrokeWidth / 2,
                   strokePaint.StrokeWidth / 2);
```

Это увеличивает `pathBounds`ный прямоугольник на 1,5 единиц на всех четырех сторонах. Это разумное решение, только в том случае, когда соединение штриха округляется. Фацетное соединение может быть больше времени и трудно вычислить.

Аналогичную методику также можно использовать с текстом, как показано на странице **анизотропный текст** . Ниже приведена соответствующая часть обработчика `PaintSurface` из класса [`AnisotropicTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicTextPage.cs) .

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    Color = SKColors.Blue,
    StrokeWidth = 0.1f,
    StrokeJoin = SKStrokeJoin.Round
})
{
    SKRect textBounds = new SKRect();
    textPaint.MeasureText("HELLO", ref textBounds);

    // Inflate bounds by the stroke width
    textBounds.Inflate(textPaint.StrokeWidth / 2,
                       textPaint.StrokeWidth / 2);

    canvas.Scale(info.Width / textBounds.Width,
                 info.Height / textBounds.Height);
    canvas.Translate(-textBounds.Left, -textBounds.Top);

    canvas.DrawText("HELLO", 0, 0, textPaint);
}
```

Это аналогичная логика, а текст расширяется до размера страницы на основе границ прямоугольника, возвращаемых из `MeasureText` (что немного больше, чем фактический текст):

[![](scale-images/anisotropictext-small.png "Triple screenshot of the Anisotropic Test page")](scale-images/anisotropictext-large.png#lightbox "Triple screenshot of the Anisotropic Test page")

Если необходимо выполнить сохранение пропорций графических объектов, стоит использовать масштабирование мощности. На странице **масштабирование исотропик** показано это для 11-ориентированной звезды. По существу приведены шаги для отображения графического объекта в центре страницы мощности масштабирования.

- Перевод центр графического объекта в левом верхнем углу.
- Изменять масштаб объекта, на основании минимального размеры страницы горизонтальные и вертикальные разделить по измерениям графического объекта.
- Перевод center масштабируемого объекта относительно центральной части страницы.

[`IsotropicScalingPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/IsotropicScalingPage.cs) выполняет эти действия в обратную последовательность перед отображением звездочки:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPath path = HendecagramArrayPage.HendecagramPath;
    SKRect pathBounds = path.Bounds;

    using (SKPaint fillPaint = new SKPaint())
    {
        fillPaint.Style = SKPaintStyle.Fill;

        float scale = Math.Min(info.Width / pathBounds.Width,
                               info.Height / pathBounds.Height);

        for (int i = 0; i <= 10; i++)
        {
            fillPaint.Color = new SKColor((byte)(255 * (10 - i) / 10),
                                          0,
                                          (byte)(255 * i / 10));
            canvas.Save();
            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(scale);
            canvas.Translate(-pathBounds.MidX, -pathBounds.MidY);
            canvas.DrawPath(path, fillPaint);
            canvas.Restore();

            scale *= 0.9f;
        }
    }
}
```

Код также отображает 10 типа "звезда" несколько раз, каждый раз, уменьшение масштабирования factor, 10% и постепенно изменения цвета от красного к синему:

[![](scale-images/isotropicscaling-small.png "Triple screenshot of the Isotropic Scaling page")](scale-images/isotropicscaling-large.png#lightbox "Triple screenshot of the Isotropic Scaling page")

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
