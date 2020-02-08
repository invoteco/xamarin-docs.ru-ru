---
title: Сведения о пути и перечисление
description: В этой статье объясняется, как получить сведения о пути SkiaSharp и перечисление содержимого и демонстрирует это с помощью примера кода.
ms.prod: xamarin
ms.assetid: 8E8C5C6A-F324-4155-8652-7A77D231B3E5
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 09/12/2017
ms.openlocfilehash: 6f4f4e6253c14d86e2057f13d6232a07a83b4d26
ms.sourcegitcommit: ae5557c5024d4b7bd52b2f33cb96114ce2b8e086
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/06/2020
ms.locfileid: "77045083"
---
# <a name="path-information-and-enumeration"></a>Сведения о пути и перечисление

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Получение сведений о путях и перечисление содержимого_

Класс [`SKPath`](xref:SkiaSharp.SKPath) определяет несколько свойств и методов, позволяющих получать сведения о пути. Свойства [`Bounds`](xref:SkiaSharp.SKPath.Bounds) и [`TightBounds`](xref:SkiaSharp.SKPath.TightBounds) (и связанные методы) получают метрики пути. Метод [`Contains`](xref:SkiaSharp.SKPath.Contains(System.Single,System.Single)) позволяет определить, находится ли конкретная точка в пределах пути.

Иногда полезно определить, общая длина всех линий и кривых линий, составляющих путь. Вычисление этой длины не является алгоритмически простой задачей, поэтому на нее помещается весь класс с именем [`PathMeasure`](xref:SkiaSharp.SKPathMeasure) .

Полезно также иногда для получения графических операций и точки, составляющие путь. Поначалу это средство может показаться ненужной: Если приложение уже создано путь, программа уже знает содержимое. Однако вы видели, что пути можно также создавать с помощью [эффектов пути](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) и путем преобразования [текстовых строк в пути](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md). Можно также получить рисования операции и точки, составляющие эти пути. Один из вариантов — применить алгоритмического преобразование для всех точек, например, чтобы обтекание полушария:

![](information-images/pathenumerationsample.png "Text wrapped on a hemisphere")

## <a name="getting-the-path-length"></a>Получение длины пути

В статье « [**пути и текст**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md) » вы узнали, как использовать метод [`DrawTextOnPath`](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint)) для отрисовки текстовой строки, базовая папка которой соответствует пути. Но что делать, если вы хотите размер текста, таким образом, чтобы он точно соответствует путь? Рисование текста вокруг круга очень просто, так как длина окружности круга прост, для которого требуется вычислить. Но длины окружности эллипса или длина параметра кривую Безье не так просто.

Класс [`SKPathMeasure`](xref:SkiaSharp.SKPathMeasure) может помочь. [Конструктор](xref:SkiaSharp.SKPathMeasure.%23ctor(SkiaSharp.SKPath,System.Boolean,System.Single)) принимает аргумент `SKPath`, а свойство [`Length`](xref:SkiaSharp.SKPathMeasure.Length) отображает его длину.

Этот класс показан в примере **длины пути** , который основан на странице **кривой Безье** . Файл [**пасленгспаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml) является производным от `InteractivePage` и включает в себя сенсорный интерфейс:

```xaml
<local:InteractivePage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       xmlns:local="clr-namespace:SkiaSharpFormsDemos"
                       xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
                       xmlns:tt="clr-namespace:TouchTracking"
                       x:Class="SkiaSharpFormsDemos.Curves.PathLengthPage"
                       Title="Path Length">
    <Grid BackgroundColor="White">
        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction" />
        </Grid.Effects>
    </Grid>
</local:InteractivePage>
```

Файл кода программной части [**PathLengthPage.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml.cs) позволяет перемещать четыре сенсорные точки для определения конечных точек и контрольных точек кривой Безье третьего порядка. Три поля определяют текстовую строку, объект `SKPaint` и вычисляемую ширину текста:

```csharp
public partial class PathLengthPage : InteractivePage
{
    const string text = "Compute length of path";

    static SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Black,
        TextSize = 10,
    };

    static readonly float baseTextWidth = textPaint.MeasureText(text);
    ...
}
```

`baseTextWidth` поле — это ширина текста в зависимости от значения параметра `TextSize`, равного 10.

Обработчик `PaintSurface` Рисует кривую Безье, а затем изменяет размер текста в соответствии с его полной длиной:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with cubic Bezier curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.CubicTo(touchPoints[1].Center,
                     touchPoints[2].Center,
                     touchPoints[3].Center);

        canvas.DrawPath(path, strokePaint);

        // Get path length
        SKPathMeasure pathMeasure = new SKPathMeasure(path, false, 1);

        // Find new text size
        textPaint.TextSize = pathMeasure.Length / baseTextWidth * 10;

        // Draw text on path
        canvas.DrawTextOnPath(text, path, 0, 0, textPaint);
    }
    ...
}
```

Свойство `Length` созданного объекта `SKPathMeasure` получает длину пути. Длина пути делится на `baseTextWidth` значение (ширина текста зависит от размера текста, равного 10), а затем умножается на базовый размер текста, равный 10. Результатом является новый размер текста для отображения текста по этому пути:

[![](information-images/pathlength-small.png "Triple screenshot of the Path Length page")](information-images/pathlength-large.png#lightbox "Triple screenshot of the Path Length page")

Как для кривой Безье возвращает длинный или короткий, вы увидите изменение размера текста.

## <a name="traversing-the-path"></a>Обход путь

`SKPathMeasure` может делать больше, чем просто измерять длину пути. Для любого значения от нуля до длины пути объект `SKPathMeasure` может получить позицию по контуру, а тангенс — к кривой пути в этой точке. Тангенс доступен в виде вектора в виде объекта `SKPoint` или вращения, инкапсулированного в объекте `SKMatrix`. Ниже приведены методы `SKPathMeasure`, которые получают эти сведения различными и гибкими способами.

```csharp
Boolean GetPosition (Single distance, out SKPoint position)

Boolean GetTangent (Single distance, out SKPoint tangent)

Boolean GetPositionAndTangent (Single distance, out SKPoint position, out SKPoint tangent)

Boolean GetMatrix (Single distance, out SKMatrix matrix, SKPathMeasureMatrixFlags flag)
```

Члены перечисления [`SKPathMeasureMatrixFlags`](xref:SkiaSharp.SKPathMeasureMatrixFlags) :

- `GetPosition`
- `GetTangent`
- `GetPositionAndTangent`

Страница **половинной черты уницикле** анимирует фигуру на уницикле, который кажется пере в кривую Безье третьего порядка:

[![](information-images/unicyclehalfpipe-small.png "Triple screenshot of the Unicycle Half-Pipe page")](information-images/unicyclehalfpipe-large.png#lightbox "Triple screenshot of the Unicycle Half-Pipe page")

Объект `SKPaint`, используемый для обводки половинной черты и уницикле, определяется как поле в классе `UnicycleHalfPipePage`. Также определяется объект `SKPath` для уницикле:

```csharp
public class UnicycleHalfPipePage : ContentPage
{
    ...
    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 3,
        Color = SKColors.Black
    };

    SKPath unicyclePath = SKPath.ParseSvgPathData(
        "M 0 0" +
        "A 25 25 0 0 0 0 -50" +
        "A 25 25 0 0 0 0 0 Z" +
        "M 0 -25 L 0 -100" +
        "A 15 15 0 0 0 0 -130" +
        "A 15 15 0 0 0 0 -100 Z" +
        "M -25 -85 L 25 -85");
    ...
}
```

Класс содержит стандартные переопределения методов `OnAppearing` и `OnDisappearing` для анимации. Обработчик `PaintSurface` создает путь для половины канала и затем рисует его. Затем создается объект `SKPathMeasure` на основе этого пути:

```csharp
public class UnicycleHalfPipePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath pipePath = new SKPath())
        {
            pipePath.MoveTo(50, 50);
            pipePath.CubicTo(0, 1.25f * info.Height,
                             info.Width - 0, 1.25f * info.Height,
                             info.Width - 50, 50);

            canvas.DrawPath(pipePath, strokePaint);

            using (SKPathMeasure pathMeasure = new SKPathMeasure(pipePath))
            {
                float length = pathMeasure.Length;

                // Animate t from 0 to 1 every three seconds
                TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
                float t = (float)(timeSpan.TotalSeconds % 5 / 5);

                // t from 0 to 1 to 0 but slower at beginning and end
                t = (float)((1 - Math.Cos(t * 2 * Math.PI)) / 2);

                SKMatrix matrix;
                pathMeasure.GetMatrix(t * length, out matrix,
                                      SKPathMeasureMatrixFlags.GetPositionAndTangent);

                canvas.SetMatrix(matrix);
                canvas.DrawPath(unicyclePath, strokePaint);
            }
        }
    }
}
```

Обработчик `PaintSurface` вычисляет значение `t`, которое переходит от 0 к 1 каждые пять секунд. Затем она использует функцию `Math.Cos` для преобразования этого значения в значение `t` в диапазоне от 0 до 1 и обратно в 0, где 0 соответствует уницикле в начале сверху слева, а 1 соответствует уницикле в правом верхнем углу. Косинус вызывает скорость медленных в верхней части канала и быстрым в нижней.

Обратите внимание, что это значение `t` должно быть умножено на длину пути для первого аргумента, чтобы `GetMatrix`. Затем матрица применяется к объекту `SKCanvas` для рисования пути уницикле.

## <a name="enumerating-the-path"></a>Перечисление путь

Два встроенных класса `SKPath` позволяют перечислить содержимое пути. Эти классы [`SKPath.Iterator`](xref:SkiaSharp.SKPath.Iterator) и [`SKPath.RawIterator`](xref:SkiaSharp.SKPath.RawIterator). Эти два класса очень похожи, но `SKPath.Iterator` могут исключить элементы в пути с нулевой длиной или близкие к нулевой длине. `RawIterator` используется в примере ниже.

Объект типа `SKPath.RawIterator` можно получить, вызвав метод [`CreateRawIterator`](xref:SkiaSharp.SKPath.CreateRawIterator) `SKPath`. Перечисление по пути достигается путем многократного вызова метода [`Next`](xref:SkiaSharp.SKPath.RawIterator.Next*) . Передайте ему массив из четырех `SKPoint` значений:

```csharp
SKPoint[] points = new SKPoint[4];
...
SKPathVerb pathVerb = rawIterator.Next(points);
```

Метод `Next` возвращает член типа перечисления [`SKPathVerb`](xref:SkiaSharp.SKPathVerb) . Эти значения указывают определенной команды рисования в пути. Количество допустимых точек, вставлены в массиве зависит от этой команды:

- `Move` с одной точкой
- `Line` с двумя точками
- `Cubic` с четырьмя точками
- `Quad` с тремя точками
- `Conic` с тремя точками (а также вызов метода [`ConicWeight`](xref:SkiaSharp.SKPath.RawIterator.ConicWeight*) для веса)
- `Close` с одной точкой
- `Done`

Команда `Done` указывает, что перечисление пути завершено.

Обратите внимание, что команд `Arc` не существует. Это означает, что все дуги преобразуются в кривых Безье при добавлении к пути.

Некоторые сведения в массиве `SKPoint` являются избыточными. Например, если за глаголом `Move` следует команда `Line`, то первая из двух точек, сопровождающих `Line`, совпадает с точкой `Move`. На практике эта избыточность очень полезно. При получении команды `Cubic` она сопровождается всеми четырьмя точками, определяющими кривую Безье третьего порядка. Сохранить текущую позицию установленные предыдущей команды не нужно.

Однако команда, вызывающая проблемы, `Close`. Эта команда рисует прямую линию от текущего расположения до начала контура, установленного ранее командой `Move`. В идеале команда `Close` должна предоставлять эти две точки, а не только одну точку. Что еще хуже, точка, сопровождающая глагол `Close`, всегда равна (0, 0). При перечислении по пути, вероятно, потребуется хранить `Move`ную точку и текущую позицию.

## <a name="enumerating-flattening-and-malforming"></a>Перечисление, выравнивание и Malforming

Для применения алгоритма иногда рекомендуется преобразовать путь к malform их каким-либо образом:

![](information-images/pathenumerationsample.png "Text wrapped on a hemisphere")

Большинство этих букв состоят из прямых линий, но эти прямые линии было очевидно преминет кривых. Как такое возможно?

Ключом является то, что исходные прямые линии разбиваются на ряд небольших прямых линий. Затем эти меньшего размера отдельных прямых линий можно управлять различными способами, в одну кривую.

Чтобы упростить этот процесс, пример [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) содержит статический класс [`PathExtensions`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathExtensions.cs) с методом `Interpolate`, который разделяет прямую линию на несколько коротких строк, которые имеют длину только одну единицу. Кроме того класс содержит несколько методов, которые преобразуют три типа кривых Безье на ряд мелких прямых линий, которые аппроксимируют кривой. (Параметрической формулы представлены в статье [**три типа кривых Безье**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md).) Этот процесс называется _плоской_ кривой:

```csharp
static class PathExtensions
{
    ...
    static SKPoint[] Interpolate(SKPoint pt0, SKPoint pt1)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * pt0.X + t * pt1.X;
            float y = (1 - t) * pt0.Y + t * pt1.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenCubic(SKPoint pt0, SKPoint pt1, SKPoint pt2, SKPoint pt3)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2) + Length(pt2, pt3));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * (1 - t) * (1 - t) * pt0.X +
                        3 * t * (1 - t) * (1 - t) * pt1.X +
                        3 * t * t * (1 - t) * pt2.X +
                        t * t * t * pt3.X;
            float y = (1 - t) * (1 - t) * (1 - t) * pt0.Y +
                        3 * t * (1 - t) * (1 - t) * pt1.Y +
                        3 * t * t * (1 - t) * pt2.Y +
                        t * t * t * pt3.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenQuadratic(SKPoint pt0, SKPoint pt1, SKPoint pt2)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * (1 - t) * pt0.X + 2 * t * (1 - t) * pt1.X + t * t * pt2.X;
            float y = (1 - t) * (1 - t) * pt0.Y + 2 * t * (1 - t) * pt1.Y + t * t * pt2.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenConic(SKPoint pt0, SKPoint pt1, SKPoint pt2, float weight)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float denominator = (1 - t) * (1 - t) + 2 * weight * t * (1 - t) + t * t;
            float x = (1 - t) * (1 - t) * pt0.X + 2 * weight * t * (1 - t) * pt1.X + t * t * pt2.X;
            float y = (1 - t) * (1 - t) * pt0.Y + 2 * weight * t * (1 - t) * pt1.Y + t * t * pt2.Y;
            x /= denominator;
            y /= denominator;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static double Length(SKPoint pt0, SKPoint pt1)
    {
        return Math.Sqrt(Math.Pow(pt1.X - pt0.X, 2) + Math.Pow(pt1.Y - pt0.Y, 2));
    }
}
```

На все эти методы ссылается метод расширения `CloneWithTransform` также включены в этот класс и показан ниже. Этот метод создает точную копию пути, перечисление команды пути и создав новый путь на основе данных. Однако новый путь состоит только из `MoveTo` и `LineTo` вызовов. Все прямые линии и кривые уменьшаются на ряд мелких строк.

При вызове `CloneWithTransform`в метод передается `Func<SKPoint, SKPoint>`, который является функцией с параметром `SKPaint`, возвращающим значение `SKPoint`. Эта функция вызывается для каждой точки для применения пользовательского алгоритма преобразования:

```csharp
static class PathExtensions
{
    public static SKPath CloneWithTransform(this SKPath pathIn, Func<SKPoint, SKPoint> transform)
    {
        SKPath pathOut = new SKPath();

        using (SKPath.RawIterator iterator = pathIn.CreateRawIterator())
        {
            SKPoint[] points = new SKPoint[4];
            SKPathVerb pathVerb = SKPathVerb.Move;
            SKPoint firstPoint = new SKPoint();
            SKPoint lastPoint = new SKPoint();

            while ((pathVerb = iterator.Next(points)) != SKPathVerb.Done)
            {
                switch (pathVerb)
                {
                    case SKPathVerb.Move:
                        pathOut.MoveTo(transform(points[0]));
                        firstPoint = lastPoint = points[0];
                        break;

                    case SKPathVerb.Line:
                        SKPoint[] linePoints = Interpolate(points[0], points[1]);

                        foreach (SKPoint pt in linePoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[1];
                        break;

                    case SKPathVerb.Cubic:
                        SKPoint[] cubicPoints = FlattenCubic(points[0], points[1], points[2], points[3]);

                        foreach (SKPoint pt in cubicPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[3];
                        break;

                    case SKPathVerb.Quad:
                        SKPoint[] quadPoints = FlattenQuadratic(points[0], points[1], points[2]);

                        foreach (SKPoint pt in quadPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[2];
                        break;

                    case SKPathVerb.Conic:
                        SKPoint[] conicPoints = FlattenConic(points[0], points[1], points[2], iterator.ConicWeight());

                        foreach (SKPoint pt in conicPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[2];
                        break;

                    case SKPathVerb.Close:
                        SKPoint[] closePoints = Interpolate(lastPoint, firstPoint);

                        foreach (SKPoint pt in closePoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        firstPoint = lastPoint = new SKPoint(0, 0);
                        pathOut.Close();
                        break;
                }
            }
        }
        return pathOut;
    }
    ...
}
```

Так как путь клонированный уменьшается до мелких прямых линий, функция преобразования имеет возможность преобразуется прямых линий и кривых.

Обратите внимание, что метод удерживает первую точку каждого профиля в переменной, именуемой `firstPoint`, и текущую позицию после каждой команды рисования в переменной `lastPoint`. Эти переменные необходимы для создания окончательной закрывающей строки при обнаружении глагола `Close`.

В примере **глобулартекст** этот метод расширения используется для заключения текста вокруг полушария в трехмерном эффекте:

[![](information-images/globulartext-small.png "Triple screenshot of the Globular Text page")](information-images/globulartext-large.png#lightbox "Triple screenshot of the Globular Text page")

Конструктор класса [`GlobularTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/GlobularTextPage.cs) выполняет это преобразование. Он создает объект `SKPaint` для текста, а затем получает объект `SKPath` из метода `GetTextPath`. Это путь, передаваемый методу расширения `CloneWithTransform` вместе с функцией преобразования:

```csharp
public class GlobularTextPage : ContentPage
{
    SKPath globePath;

    public GlobularTextPage()
    {
        Title = "Globular Text";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        using (SKPaint textPaint = new SKPaint())
        {
            textPaint.Typeface = SKTypeface.FromFamilyName("Times New Roman");
            textPaint.TextSize = 100;

            using (SKPath textPath = textPaint.GetTextPath("HELLO", 0, 0))
            {
                SKRect textPathBounds;
                textPath.GetBounds(out textPathBounds);

                globePath = textPath.CloneWithTransform((SKPoint pt) =>
                {
                    double longitude = (Math.PI / textPathBounds.Width) *
                                            (pt.X - textPathBounds.Left) - Math.PI / 2;
                    double latitude = (Math.PI / textPathBounds.Height) *
                                            (pt.Y - textPathBounds.Top) - Math.PI / 2;

                    longitude *= 0.75;
                    latitude *= 0.75;

                    float x = (float)(Math.Cos(latitude) * Math.Sin(longitude));
                    float y = (float)Math.Sin(latitude);

                    return new SKPoint(x, y);
                });
            }
        }
    }
    ...
}
```

Функция преобразования сначала вычисляет два значения с именем `longitude` и `latitude` в диапазоне от – π/2 в верхней и левой части текста до π/2 в правой и нижней части текста. Диапазон этих значений не визуально удовлетворительные, поэтому они сокращаются путем умножения 0,75. (Попробуйте выполнить код без в настройках. Текст станет слишком запутанным на северном и Южной полюсам и слишком тонкий на сторонах.) Эти трехмерные сферические координаты преобразуются в двухмерные `x` и `y` координаты стандартными формулами.

Новый путь хранится в виде поля. Затем обработчик `PaintSurface` просто должен центрировать и масштабировать путь, чтобы отобразить его на экране:

```csharp
public class GlobularTextPage : ContentPage
{
    SKPath globePath;
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint pathPaint = new SKPaint())
        {
            pathPaint.Style = SKPaintStyle.Fill;
            pathPaint.Color = SKColors.Blue;
            pathPaint.StrokeWidth = 3;
            pathPaint.IsAntialias = true;

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(0.45f * Math.Min(info.Width, info.Height));     // radius
            canvas.DrawPath(globePath, pathPaint);
        }
    }
}
```

Это очень универсальный метод. Если массив эффектов пути, описанных в статье [**эффекты пути**](effects.md) , не включает в себя что-то, что вы намерены добавить, это способ заполнять пропуски.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
