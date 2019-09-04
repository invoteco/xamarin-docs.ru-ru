---
title: Три способа нарисовать дугу
description: В этой статье описывается использование SkiaSharp для определения дуги тремя различными способами и демонстрирует это с помощью примера кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: F1DA55E4-0182-4388-863C-5C340213BF3C
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2017
ms.openlocfilehash: ada7ce8fc9365ab4133ddf439353e97e640f39d6
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/03/2019
ms.locfileid: "70228111"
---
# <a name="three-ways-to-draw-an-arc"></a>Три способа нарисовать дугу

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Сведения об использовании для определения дуги в три разных способа SkiaSharp_

Дуга — кривую на длины окружности эллипса, например скругленными части это знак бесконечности:

![Знак бесконечности](arcs-images/arcsample.png)

Несмотря на простоту этого определения, не существует способа определить функцию рисования дуги, удовлетворяющий все потребности, и, следовательно, не консенсус между системами графики о наилучшем способе нарисовать дугу. По этой причине `SKPath` класса не ограничивает сам лишь одним из подходов.

`SKPath` Определяет [ `AddArc` ](xref:SkiaSharp.SKPath.AddArc*) метод, пять различных [ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo*) методов и два относительно [ `RArcTo` ](xref:SkiaSharp.SKPath.RArcTo*) методы. Эти методы можно разделить на три категории, представляющая три различных подхода к указанию дуги. Используемый тот, который зависит от сведений, доступных для определения дуги и как это arc дополняют другие графические, рисовании.

## <a name="the-angle-arc"></a>Угол дуги

Угол дуги способ рисования дуги, необходимо указать прямоугольник, ограничивающий эллипс. Дугу на длины окружности этого эллипса обозначается углы от центра эллипса, указывающие начало дуги и его длины. Два различных способа рисования угол дуги. Это [ `AddArc` ](xref:SkiaSharp.SKPath.AddArc(SkiaSharp.SKRect,System.Single,System.Single)) метод и [ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKRect,System.Single,System.Single,System.Boolean)) метод:

```csharp
public void AddArc (SKRect oval, Single startAngle, Single sweepAngle)

public void ArcTo (SKRect oval, Single startAngle, Single sweepAngle, Boolean forceMoveTo)
```

Эти методы идентичны методам Android [`AddArc`](xref:Android.Graphics.Path.AddArc*) и [`ArcTo`] XREF: Android. Graphics. Path. аркто *). IOS [ `AddArc` ](xref:CoreGraphics.CGPath.AddArc(System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.Boolean)) метод аналогичен, но будет ограничен дуг на длину окружности круга, а не обобщить как эллипс.

Оба метода начинаются с `SKRect` значение, которое определяет расположение и размер эллипса:

![Овал, начинающий дугу угла](arcs-images/anglearcoval.png)

Дуга является частью длины окружности этого эллипса.

`startAngle` Аргумент — по часовой стрелке в градусах относительно горизонтальной линии, рисуемой из центра эллипса справа. `sweepAngle` Аргумент задается по отношению к `startAngle`. Ниже приведены `startAngle` и `sweepAngle` значения 60 градусов и 100 градусов, соответственно:

![Углы, определяющие дугу угла](arcs-images/anglearcangles.png)

Начальный угол дуги начинается. Его длина регулируется угол поворота. Ниже приведен дуги красным цветом.

![Выделенная дуга угла](arcs-images/anglearchighlight.png)

Кривая, добавляется к пути с `AddArc` или `ArcTo` метод — просто это часть эллипса окружности:

![Дуга угла по себе](arcs-images/anglearc.png)

Аргументы `startAngle` или`sweepAngle` могут быть отрицательными: Дуга по часовой стрелке используется для положительных `sweepAngle` значений и против часовой стрелки для отрицательных значений.

Тем не менее `AddArc` does *не* определить закрытый профиль. При вызове метода `LineTo` после `AddArc`, линия берет начало от конца дуги к моменту `LineTo` метода и это справедливо для `ArcTo`.

`AddArc` автоматически запускает новый профиль и функционально эквивалентен вызов `ArcTo` с последний аргумент `true`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, true);
```

Что вызывается последним аргументом `forceMoveTo`, и фактически вызывает `MoveTo` вызвать в начале дуги. Новый профиль, который начинается. Это не так с последним аргументом `false`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, false);
```

Эта версия `ArcTo` проводит линию из текущей позиции в начале дуги. Это означает, что дуги может быть где-то посередине профиль большего размера.

**Угол дуги** страница позволяет указать начало и углы поворота с помощью двух ползунков. Файл XAML создает два `Slider` элементов и `SKCanvasView`. `PaintCanvas` Обработчик в [ **AngleArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AngleArcPage.xaml.cs) файл рисует Овал и дуги, с использованием двух `SKPaint` объекты, определенные как поля:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKRect rect = new SKRect(100, 100, info.Width - 100, info.Height - 100);
    float startAngle = (float)startAngleSlider.Value;
    float sweepAngle = (float)sweepAngleSlider.Value;

    canvas.DrawOval(rect, outlinePaint);

    using (SKPath path = new SKPath())
    {
        path.AddArc(rect, startAngle, sweepAngle);
        canvas.DrawPath(path, arcPaint);
    }
}
```

Как вы видите, начальный угол и угол поворота, выполняемых с отрицательными значениями:

[![Тройной снимок экрана со страницей угла дуги](arcs-images/anglearc-small.png)](arcs-images/anglearc-large.png#lightbox)

Этот подход к созданию дугу является алгоритмически наиболее простым и легко являются производными параметрические уравнения, которые описывают дуги. Зная, размер и расположение элемента эллипса и угла начала и поворота, начальную и конечную точки дуги можно вычислить по простой тригонометрических вычислений:

`x = oval.MidX + (oval.Width / 2) * cos(angle)`

`y = oval.MidY + (oval.Height / 2) * sin(angle)`

`angle` Значение `startAngle` или `startAngle + sweepAngle`.

Использование двух углов для определения дугу является наилучшим образом подходит для случаев, когда вы знаете angular длина окружности, необходимый для рисования, например, чтобы убедиться в круговой диаграмме. **Разрезанная круговая диаграмма** страница демонстрирует это. [ `ExplodedPieChartPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ExplodedPieChartPage.cs) Класс использует внутренний класс для определения некоторых данных создано и цвета:

```csharp
class ChartData
{
    public ChartData(int value, SKColor color)
    {
        Value = value;
        Color = color;
    }

    public int Value { private set; get; }

    public SKColor Color { private set; get; }
}

ChartData[] chartData =
{
    new ChartData(45, SKColors.Red),
    new ChartData(13, SKColors.Green),
    new ChartData(27, SKColors.Blue),
    new ChartData(19, SKColors.Magenta),
    new ChartData(40, SKColors.Cyan),
    new ChartData(22, SKColors.Brown),
    new ChartData(29, SKColors.Gray)
};

```

`PaintSurface` Обработчик сначала перебирает элементы для вычисления `totalValues` номер. Таким образом его можно определить размер каждого элемента долях от общего количества и преобразовать их значение угла:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int totalValues = 0;

    foreach (ChartData item in chartData)
    {
        totalValues += item.Value;
    }

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float explodeOffset = 50;
    float radius = Math.Min(info.Width / 2, info.Height / 2) - 2 * explodeOffset;
    SKRect rect = new SKRect(center.X - radius, center.Y - radius,
                             center.X + radius, center.Y + radius);

    float startAngle = 0;

    foreach (ChartData item in chartData)
    {
        float sweepAngle = 360f * item.Value / totalValues;

        using (SKPath path = new SKPath())
        using (SKPaint fillPaint = new SKPaint())
        using (SKPaint outlinePaint = new SKPaint())
        {
            path.MoveTo(center);
            path.ArcTo(rect, startAngle, sweepAngle, false);
            path.Close();

            fillPaint.Style = SKPaintStyle.Fill;
            fillPaint.Color = item.Color;

            outlinePaint.Style = SKPaintStyle.Stroke;
            outlinePaint.StrokeWidth = 5;
            outlinePaint.Color = SKColors.Black;

            // Calculate "explode" transform
            float angle = startAngle + 0.5f * sweepAngle;
            float x = explodeOffset * (float)Math.Cos(Math.PI * angle / 180);
            float y = explodeOffset * (float)Math.Sin(Math.PI * angle / 180);

            canvas.Save();
            canvas.Translate(x, y);

            // Fill and stroke the path
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, outlinePaint);
            canvas.Restore();
        }

        startAngle += sweepAngle;
    }
}
```

Новый `SKPath` объекта создается для каждого среза круговой диаграммы. Путь состоит из строки из центра, то `ArcTo` для рисования дуги и еще одну строку обратно в центр результаты из `Close` вызова. Эта программа отображает «Разрезанная» секторов диаграммы, перемещая их все в центре на 50 пикселей. Эта задача требует вектор в направлении среднюю угол поворота для каждого среза:

[![Тройной снимок экрана страницы разрезанной круговой диаграммы](arcs-images/explodedpiechart-small.png)](arcs-images/explodedpiechart-large.png#lightbox)

Чтобы увидеть, как выглядит без «развертывание», просто задокомментируем `Translate` вызова:

[![Тройной снимок экрана развернутой страницы круговой диаграммы без развертывания](arcs-images/explodedpiechartunexploded-small.png)](arcs-images/explodedpiechartunexploded-large.png#lightbox)

## <a name="the-tangent-arc"></a>Касательной дуги

Второй тип дуги, поддерживаемых `SKPath` — *касательных arc*, так называемых поскольку дуги окружности круга, была касательной к две соединенные линии.

Касательной arc добавляется путь с помощью вызова [ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,System.Single)) метод с двумя `SKPoint` параметры, или [ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo(System.Single,System.Single,System.Single,System.Single,System.Single)) перегрузка с помощью отдельных `Single` параметры для следующие моменты.

```csharp
public void ArcTo (SKPoint point1, SKPoint point2, Single radius)

public void ArcTo (Single x1, Single y1, Single x2, Single y2, Single radius)
```

Это `ArcTo` метод аналогичен методу PostScript [ `arct` ](https://www.adobe.com/products/postscript/pdfs/PLRM.pdf) функции (страницы 532) и iOS [ `AddArcToPoint` ](xref:CoreGraphics.CGPath.AddArcToPoint(System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.nfloat)) метод.

`ArcTo` Метод включает в себя три точки:

- Текущий точек контура или точки (0, 0), если `MoveTo` не был вызван
- Первый аргумент точки `ArcTo` метод, именуемый *углу точки*
- Второй аргумент точки `ArcTo`, который называется *конечная точка*:

![Три точки, начинающиеся с дуги по касательной](arcs-images/tangentarcthreepoints.png)

Эти три точки определяют две соединенные линии:

![Линии, соединяющие три точки дуги тангенса](arcs-images/tangentarcconnectinglines.png)

Если colinear три точки &mdash; то есть если они находятся в той же строке прямой &mdash; рисуется дуга.

`ArcTo` Также включает `radius` параметра. Определяет радиус круга:

![Окружность дуги тангенса](arcs-images/tangentarccircle.png)

Касательной дуги для эллипс не генерализована.

Если две строки соответствует под любым углом, которым могут вставляться между эти строки, чтобы она была касательной к обе строки:

![Окружность дуги между двумя линиями](arcs-images/tangentarctangentcircle.png)

Кривую, которая добавляется в профиль не затрагивает любой из точки, указанные в `ArcTo` метод. Она состоит из прямую линию с текущего момента первой точке касания и дугу, которая заканчивается на второй точке касания, здесь показано красным цветом:

![Выделенная дуга тангенса между двумя линиями](arcs-images/tangentarchighlight.png)

Ниже приведен окончательный прямой линии и дуги, который добавляется в профиль.

![Выделенная дуга тангенса между двумя линиями](arcs-images/tangentarc.png)

Профиль может быть продолжен из второй точке касания.

**Дуги тангенс** страница позволяет экспериментировать с касательной дуги. Это первое из нескольких страниц, которые являются производными от [ `InteractivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/InteractivePage.cs), который определяет некоторые удобные `SKPaint` объектов и выполняет `TouchPoint` обработки:

```csharp
public class InteractivePage : ContentPage
{
    protected SKCanvasView baseCanvasView;
    protected TouchPoint[] touchPoints;

    protected SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3
    };

    protected SKPaint redStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 15
    };

    protected SKPaint dottedStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] { 7, 7 }, 0)
    };

    protected void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        bool touchPointMoved = false;

        foreach (TouchPoint touchPoint in touchPoints)
        {
            float scale = baseCanvasView.CanvasSize.Width / (float)baseCanvasView.Width;
            SKPoint point = new SKPoint(scale * (float)args.Location.X,
                                        scale * (float)args.Location.Y);
            touchPointMoved |= touchPoint.ProcessTouchEvent(args.Id, args.Type, point);
        }

        if (touchPointMoved)
        {
            baseCanvasView.InvalidateSurface();
        }
    }
}
```

Класс `TangentArcPage` является производным от класса `InteractivePage`. Конструктор в [ **TangentArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml.cs) файл отвечает за создание и инициализация `touchPoints` массива, а параметр `baseCanvasView` (в `InteractivePage`) для `SKCanvasView` создать экземпляр объекта в [ **TangentArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml) файла:

```csharp
public partial class TangentArcPage : InteractivePage
{
    public TangentArcPage()
    {
        touchPoints = new TouchPoint[3];

        for (int i = 0; i < 3; i++)
        {
            TouchPoint touchPoint = new TouchPoint
            {
                Center = new SKPoint(i == 0 ? 100 : 500,
                                     i != 2 ? 100 : 500)
            };
            touchPoints[i] = touchPoint;
        }

        InitializeComponent();

        baseCanvasView = canvasView;
        radiusSlider.Value = 100;
    }

    void sliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

`PaintSurface` Обработчик использует `ArcTo` на основе методов, чтобы нарисовать дугу на точки касания и `Slider`, но также алгоритмически вычисляет круг, угол основан на:

```csharp
public partial class TangentArcPage : InteractivePage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Draw the two lines that meet at an angle
        using (SKPath path = new SKPath())
        {
            path.MoveTo(touchPoints[0].Center);
            path.LineTo(touchPoints[1].Center);
            path.LineTo(touchPoints[2].Center);
            canvas.DrawPath(path, dottedStrokePaint);
        }

        // Draw the circle that the arc wraps around
        float radius = (float)radiusSlider.Value;

        SKPoint v1 = Normalize(touchPoints[0].Center - touchPoints[1].Center);
        SKPoint v2 = Normalize(touchPoints[2].Center - touchPoints[1].Center);

        double dotProduct = v1.X * v2.X + v1.Y * v2.Y;
        double angleBetween = Math.Acos(dotProduct);
        float hypotenuse = radius / (float)Math.Sin(angleBetween / 2);
        SKPoint vMid = Normalize(new SKPoint((v1.X + v2.X) / 2, (v1.Y + v2.Y) / 2));
        SKPoint center = new SKPoint(touchPoints[1].Center.X + vMid.X * hypotenuse,
                                     touchPoints[1].Center.Y + vMid.Y * hypotenuse);

        canvas.DrawCircle(center.X, center.Y, radius, this.strokePaint);

        // Draw the tangent arc
        using (SKPath path = new SKPath())
        {
            path.MoveTo(touchPoints[0].Center);
            path.ArcTo(touchPoints[1].Center, touchPoints[2].Center, radius);
            canvas.DrawPath(path, redStrokePaint);
        }

        foreach (TouchPoint touchPoint in touchPoints)
        {
            touchPoint.Paint(canvas);
        }
    }

    // Vector methods
    SKPoint Normalize(SKPoint v)
    {
        float magnitude = Magnitude(v);
        return new SKPoint(v.X / magnitude, v.Y / magnitude);
    }

    float Magnitude(SKPoint v)
    {
        return (float)Math.Sqrt(v.X * v.X + v.Y * v.Y);
    }
}
```

Вот **дуги тангенс** странице под управлением:

[![Тройной снимок экрана со страницей дуги на Тангенсе](arcs-images/tangentarc-small.png)](arcs-images/tangentarc-large.png#lightbox)

Касательной дуги идеально подходит для создания скругленных углов, например прямоугольник с закругленными углами. Так как `SKPath` уже включает в себя `AddRoundedRect` метод, **округленное Heptagon** странице рассказывается, как использовать `ArcTo` для скругление углов семь стороны многоугольника. (Код обобщается для любого регулярных многоугольника.)

`PaintSurface` Обработчик [ `RoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RoundedHeptagonPage.cs) класса содержит один `for` цикла, для которого требуется вычислить координаты семь вершины heptagon, во-вторых, для которого требуется вычислить средние точки этих семи сторон вершины. Затем эти средние точки используются для создания пути:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float cornerRadius = 100;
    int numVertices = 7;
    float radius = 0.45f * Math.Min(info.Width, info.Height);

    SKPoint[] vertices = new SKPoint[numVertices];
    SKPoint[] midPoints = new SKPoint[numVertices];

    double vertexAngle = -0.5f * Math.PI;       // straight up

    // Coordinates of the vertices of the polygon
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        vertices[vertex] = new SKPoint(radius * (float)Math.Cos(vertexAngle),
                                       radius * (float)Math.Sin(vertexAngle));
        vertexAngle += 2 * Math.PI / numVertices;
    }

    // Coordinates of the midpoints of the sides connecting the vertices
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        int prevVertex = (vertex + numVertices - 1) % numVertices;
        midPoints[vertex] = new SKPoint((vertices[prevVertex].X + vertices[vertex].X) / 2,
                                        (vertices[prevVertex].Y + vertices[vertex].Y) / 2);
    }

    // Create the path
    using (SKPath path = new SKPath())
    {
        // Begin at the first midpoint
        path.MoveTo(midPoints[0]);

        for (int vertex = 0; vertex < numVertices; vertex++)
        {
            SKPoint nextMidPoint = midPoints[(vertex + 1) % numVertices];

            // Draws a line from the current point, and then the arc
            path.ArcTo(vertices[vertex], nextMidPoint, cornerRadius);

            // Connect the arc with the next midpoint
            path.LineTo(nextMidPoint);
        }
        path.Close();

        // Render the path in the center of the screen
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 10;

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.DrawPath(path, paint);
        }
    }
}

```

Вот работающая программа:

[![Тройной снимок экрана со скругленной Хептагон страницей](arcs-images/roundedheptagon-small.png)](arcs-images/roundedheptagon-large.png#lightbox)

## <a name="the-elliptical-arc"></a>Эллиптической дуги

Эллиптической дуги добавляется к пути с помощью вызова [ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPathArcSize,SkiaSharp.SKPathDirection,SkiaSharp.SKPoint)) метод, который имеет два `SKPoint` параметры, или [ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo(System.Single,System.Single,System.Single,SkiaSharp.SKPathArcSize,SkiaSharp.SKPathDirection,System.Single,System.Single)) перегрузка с отдельной X и Y координаты:

```csharp
public void ArcTo (SKPoint r, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, SKPoint xy)

public void ArcTo (Single rx, Single ry, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, Single x, Single y)
```

Эллиптической дуги согласуется с [эллиптической дуги](http://www.w3.org/TR/SVG11/paths.html#PathDataEllipticalArcCommands) входящих в масштабируемый векторный рисунок (SVG) и универсальной платформы Windows [ `ArcSegment` ](/uwp/api/Windows.UI.Xaml.Media.ArcSegment/) класса.

Эти `ArcTo` методы нарисовать дугу между двумя точками, которые являются текущей точки контура, а последний параметр `ArcTo` метод ( `xy` параметр или отдельные `x` и `y` параметров):

![Две точки, которые задают эллиптическую дугу](arcs-images/ellipticalarcpoints.png)

Первый параметр точки для `ArcTo` метод (`r`, или `rx` и `ry`) вообще не является точкой, но вместо этого задает горизонтальный и вертикальный радиусы эллипса;

![Эллипс, который определил эллиптическую дугу](arcs-images/ellipticalarcellipse.png)

`xAxisRotate` Параметр — это число градусов по часовой стрелке для поворота этого эллипса:

![Наклон эллипса, который определил эллиптическую дугу](arcs-images/ellipticalarctiltedellipse.png)

Если это мозаичный эллипс находится затем таким образом, чтобы он соприкасается двумя точками, точки соединяются два разных дуги:

![Первый набор эллиптических дуг](arcs-images/ellipticalarcellipse1.png)

Эти две дуги могут различаться двумя способами: Верхняя дуга больше, чем нижняя дуга, и по мере того, как дуга рисуется слева направо, верхняя дуга рисуется в направлении по часовой стрелке, а нижняя дуга рисуется в направлении по часовой стрелке.

Можно также в соответствии с эллипса между двумя точками другим способом:

![Второй набор эллиптических дуг](arcs-images/ellipticalarcellipse2.png)

Теперь имеется меньшего размера дуги в верхней части, которая рисуется по часовой стрелке и большего размера дуги внизу, рисуется против часовой стрелки.

Этих двух точек может быть подключено дугой определением мозаичный эллипс в общей сложности состоит из четырех способов:

![Все четыре эллиптических дуги](arcs-images/ellipticalarccolors.png)

Эти четыре дуги различаются по четыре комбинации [ `SKPathArcSize` ](xref:SkiaSharp.SKPathArcSize) и [ `SKPathDirection` ](xref:SkiaSharp.SKPathDirection) аргументов типа перечисления в `ArcTo` метод:

- отмечен Скпасарксизе. Large и Скпасдиректион. по часовой стрелке
- зеленого Скпасарксизе. Малый и Скпасдиректион. по часовой стрелке
- выделен Скпасарксизе. Малый и Скпасдиректион. против часовой стрелки
- цвет Скпасарксизе. Large и Скпасдиректион. против часовой стрелки

Если мозаичный эллипс не достаточное для размещения между двумя точками, затем он равномерно масштабируется, пока она не будет достаточно большим. Только два уникальных дуги в этом случае подключиться двумя точками. Их можно отличить с `SKPathDirection` параметра.

Несмотря на то, что этот подход к определению дугу звучит на первом возникновении сложных, это единственный подход, который позволяет определять дуги с повернутый эллипс, и это часто самый простой подход, если вам нужно провести интеграцию с другими частями компании contour дуги.

**Эллиптической дуги** страница позволяет интерактивно установить две точки и размер и поворот эллипса. `EllipticalArcPage` Класс является производным от `InteractivePage`и `PaintSurface` обработчик в [ **EllipticalArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/EllipticalArcPage.xaml.cs) файл с выделенным кодом рисует четыре дуги:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        int colorIndex = 0;
        SKPoint ellipseSize = new SKPoint((float)xRadiusSlider.Value,
                                          (float)yRadiusSlider.Value);
        float rotation = (float)rotationSlider.Value;

        foreach (SKPathArcSize arcSize in Enum.GetValues(typeof(SKPathArcSize)))
            foreach (SKPathDirection direction in Enum.GetValues(typeof(SKPathDirection)))
            {
                path.MoveTo(touchPoints[0].Center);
                path.ArcTo(ellipseSize, rotation,
                           arcSize, direction,
                           touchPoints[1].Center);

                strokePaint.Color = colors[colorIndex++];
                canvas.DrawPath(path, strokePaint);
                path.Reset();
            }
    }

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}

```

Здесь выполняется:

[![Тройной снимок экрана эллиптической страницы дуги](arcs-images/ellipticalarc-small.png)](arcs-images/ellipticalarc-large.png#lightbox)

**Бесконечность Arc** страница использует эллиптической дуги для рисования знак бесконечности. Знак бесконечности основана на два круга с радиусы 100 единиц, разделенных 100 единиц.

![Два круга](arcs-images/infinitycircles.png)

Две строки, пересекающих друг с другом приведены касательной оба окружности.

![Два круга с касательными линиями](arcs-images/infinitycircleslines.png)

Знак бесконечности состоит из части этих кругах и две строки. Чтобы использовать эллиптической дуги для рисования знак бесконечности, необходимо определить координаты, где две строки представляют касательной кругов.

Создайте прямоугольник справа в один из кругов.

![Два круга с касательными и внедренными кругами](arcs-images/infinitytriangle.png)

Радиус круга составляет 100 единиц и гипотенузы треугольника составляет 150 единиц, поэтому угол α арксинус 100 на 150, или 41,8 градусов (обратный синус). Длина другой стороне треугольник — 150 раз косинус 41,8 градусов или 112, который также можно вычислить по необходимости знать теорему Пифагора.

Координаты точки касания, затем может быть вычислено с помощью этой информации:

`x = 112·cos(41.8) = 83`

`y = 112·sin(41.8) = 75`

Четыре касательных приведены все необходимое для рисования знак бесконечности по центру в точке (0, 0) с радиусы круг 100.

![Два круга с касательными и координатами](arcs-images/infinitycoordinates.png)

`PaintSurface` Обработчик в [ `ArcInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ArcInfinityPage.cs) класс размещает знак бесконечности, чтобы (0, 0) точка располагается по центру страницы и масштабирует путь, по размеру экрана:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        path.LineTo(83, 75);
        path.ArcTo(100, 100, 0, SKPathArcSize.Large, SKPathDirection.CounterClockwise, 83, -75);
        path.LineTo(-83, 75);
        path.ArcTo(100, 100, 0, SKPathArcSize.Large, SKPathDirection.Clockwise, -83, -75);
        path.Close();

        // Use path.TightBounds for coordinates without control points
        SKRect pathBounds = path.Bounds;

        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(Math.Min(info.Width / pathBounds.Width,
                              info.Height / pathBounds.Height));

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 5;

            canvas.DrawPath(path, paint);
        }
    }
}
```

Код использует `Bounds` свойство `SKPath` определяет размеры бесконечность синус для его масштабирования размер холста:

[![Тройной снимок экрана со страницей "бесконечность" для дуги](arcs-images/arcinfinity-small.png)](arcs-images/arcinfinity-large.png#lightbox)

Результат выглядит немного мало, поэтому, предположительно, `Bounds` свойство `SKPath` сообщает, размер, превышающий путь.

На внутреннем уровне Skia аппроксимирует дуги, с помощью нескольких кривых Безье второго порядка. Эти кривые (как можно будет увидеть в следующем разделе) содержат контрольные точки, которые определяют, каким образом кривой, но не являются частью отображаемой кривой. `Bounds` Свойство включает эти контрольные точки.

Чтобы получить более тесную подходит, используйте `TightBounds` свойство, которое исключает контрольные точки. Вот программа работает в альбомном режиме и с помощью `TightBounds` свойство, чтобы получить границы путей:

[![Тройной снимок экрана страницы с жесткими границами на странице "бесконечность"](arcs-images/arcinfinitytightbounds-small.png)](arcs-images/arcinfinitytightbounds-large.png#lightbox)

Несмотря на то, что подключения между дуги, а также прямые линии — математически smooth, может показаться немного внезапные изменение дугу на прямую линию. Знак бесконечности лучше представлены в следующей статье по [ **три типа кривых Безье**](beziers.md).

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
