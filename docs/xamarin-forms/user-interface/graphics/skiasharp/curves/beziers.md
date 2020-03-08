---
title: Три типа кривых Безье
description: В этой статье описывается использование SkiaSharp для подготовки к просмотру кривых Безье третьего порядка, квадратичных и conic приложений Xamarin.Forms и демонстрирует это с помощью примера кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8FE0F6DC-16BC-435F-9626-DD1790C0145A
author: davidbritch
ms.author: dabritch
ms.date: 05/25/2017
ms.openlocfilehash: 1cf061f2ff27720ad78567bc26f00d99c5456f04
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/07/2020
ms.locfileid: "78916410"
---
# <a name="three-types-of-bzier-curves"></a>Три типа кривых Безье

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Узнайте, как использовать SkiaSharp для отрисовки кривых Безье в кубических, квадратичных и коникях_

Кривая Безье именуется после Безье Пьер (1910 – 1999 г.), инженер французский автомобильные компании альянс Renault, кто использовал кривой для проектирования автоматизированных тел автомобиля.

Кривые Безье хорошо подходят для интерактивного проектирования: они хорошо работают &mdash; другими словами, нет единственных случаев, которые приводят к тому, что кривая становится бесконечной или неудобной &mdash; и обычно визуально привлекательный:

![Пример кривой Безье](beziers-images/beziersample.png)

Контурам символов на компьютере шрифтов, обычно определяются с помощью кривых Безье.

Статья Википедии по [**кривой Безье**](https://en.wikipedia.org/wiki/B%C3%A9zier_curve) содержит некоторые полезные фундаментальные сведения. Термин « *Кривая Безье* » фактически относится к семейству схожих кривых. SkiaSharp поддерживает три типа кривых Безье, которые называются *кубическими*, *квадратными*и *Коник*. Коник также называется *рациональным квадратичным*.

## <a name="the-cubic-bzier-curve"></a>Кривая Безье третьего порядка

Кривой является тип кривой Безье, большинство разработчиков считают когда предметом кривых Безье.

Кривую Безье третьего порядка можно добавить к `SKPath` объекту с помощью метода [`CubicTo`](xref:SkiaSharp.SKPath.CubicTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,SkiaSharp.SKPoint)) с тремя параметрами `SKPoint` или перегрузкой [`CubicTo`](xref:SkiaSharp.SKPath.CubicTo(System.Single,System.Single,System.Single,System.Single,System.Single,System.Single)) с отдельными `x` и `y` параметрами:

```csharp
public void CubicTo (SKPoint point1, SKPoint point2, SKPoint point3)

public void CubicTo (Single x1, Single y1, Single x2, Single y2, Single x3, Single y3)
```

Кривая начинается в текущей точке контур. Полный кривую Безье третьего порядка определяется четырьмя точками:

- Начальная точка: текущая точка в контуре или (0, 0), если `MoveTo` не был вызван
- Первая контрольная точка: `point1` в вызове `CubicTo`
- Вторая контрольная точка: `point2` в вызове `CubicTo`
- Конечная точка: `point3` в вызове `CubicTo`

Итоговый кривой начинается с начальной точки и заканчивается в конечной точке. Кривая обычно не проходит через две контрольные точки; Вместо этого элемент управления указывает функции, как магниты для извлечения кривой к их.

Лучший способ почувствовать, кривая Безье третьего порядка, службы "Экспериментирование". Это цель страницы **кривой Безье** , которая является производной от `InteractivePage`. Файл [**безиеркурвепаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml) создает экземпляр `SKCanvasView` и `TouchEffect`. Файл кода программной части [**BezierCurvePage.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml.cs) создает четыре объекта `TouchPoint` в своем конструкторе. Обработчик событий `PaintSurface` создает `SKPath` для отрисовки кривой Безье на основе четырех `TouchPoint`ных объектов, а также рисует линии с точками по касательной от контрольных точек к конечным точкам:

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
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[2].Center.X,
                    touchPoints[2].Center.Y,
                    touchPoints[3].Center.X,
                    touchPoints[3].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
       touchPoint.Paint(canvas);
    }
}
```

Здесь выполняется:

[![тройной снимок экрана со страницей кривой Безье](beziers-images/beziercurve-small.png)](beziers-images/beziercurve-large.png#lightbox)

Математически кривая является третьего полинома. Кривая максимум пересекает прямую линию в трех местах. Начальной точки кривой всегда является касательной и в направлении, прямую линию с самого начала пункты первой контрольной точки. В конечной точке, кривая является всегда точки касания и в направлении, прямую линию от второго элемента управления в конечную точку.

Кривая Безье третьего порядка всегда связывает выпуклая четырехугольника осуществляется плавный переход подключении четырех точек. Это называется *выпуклой поверхности*. Если контрольные точки находятся на прямую линию между начальной и конечной точек, кривая Безье визуализацию как прямую линию. Но кривой также может пересекать, как показано на третьем снимке экрана.

Профиль путь может содержать несколько соединенных кривых Безье третьего, но соединение между двумя кривых Безье третьего порядка, будет smooth, только в том случае, если три аспекта colinear (то есть, находящихся на прямую линию):

- Вторая контрольная точка кривой первый
- Конечная точка первого кривой, которая также является точкой запуска второго кривой
- Первая контрольная точка кривой, вторая

В следующей статье по [**данным о пути SVG**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)вы обнаружите средство для упрощения определения гладко Соединенных кривых Безье.

Иногда бывает полезно знать, базовой параметрические уравнения, которые отображаются в кривую Безье третьего порядка. Для *t* в диапазоне от 0 до 1 значения параметрической формулы выглядят следующим образом:

x(t) = (1 – t) ³x₀ + 3t (1 – t) ²x₁ + 3t² (1 – t) x₂ + t³x₃

y(t) = (1 – t) ³y₀ + 3t (1 – t) ²y₁ + 3t² (1 – t) y₂ + t³y₃

Самый высокий показатель степени 3 подтверждает, что они третьего polynomials. Можно легко проверить, что если `t` равно 0, то точка имеет значение (x ₀, y ₀), то есть начальная точка, а если `t` равно 1, то точка имеет значение (x ₃, y ₃), то есть конечная точка. Рядом с начальной точкой (для младших значений `t`) Первая контрольная точка (x ₁, y ₁) имеет высокий результат, а ближе к конечной точке (большие значения 't ') вторая контрольная точка (x ₂, y ₂) имеет высокий результат.

## <a name="bezier-curve-approximation-to-circular-arcs"></a>Приближения кривой Безье в дуги

Иногда удобно использовать кривую Безье для визуализации дуги окружности. Кривая Безье третьего порядка может приблизительно оценить дугу окружности до четвертого круга, поэтому четыре Соединенные кривые Безье могут определять целую окружность. Такое приближение рассматривается в две статьи, опубликованные более 25 лет назад:

> Tor Доккен, et al, «хорошее приближение кругов по кривизне непрерывных кривых Безье», « *компьютерно-геометрическое проектирование 7* (1990), 33-41.

> Майкл Голдапп, "приближение круглых дуг на значение полинома в кубических," *компьютерно-геометрическое проектирование 8* (1991), 227-238.

На следующей диаграмме показаны четыре точки, помеченные как `pto`, `pt1`, `pt2`и `pt3` определяющие кривую Безье (показанную красным цветом), которая приблизительно соответствует кругу дуги:

![Приближение дуги окружности к кривой Безье](beziers-images/bezierarc45.png)

Строки начальной и конечной точек указываются касательно к окружности и кривой Безье и имеют длину *L*. Первая статья, упомянутая выше, указывает, что кривая Безье лучше всего приближена к окружности, если эта длина *L* вычисляется следующим образом:

L = 4 × tan(α / 4) / 3

На рисунке угла 45 градусов, поэтому L равно 0.265. В коде это значение будет умножено с требуемой радиус круга.

**Круглая страница дуги Безье** позволяет поэкспериментировать с определением кривой Безье, чтобы приблизительно оценить круг дуги для углов в диапазоне от до 180 градусов. Файл [**безиерЦиркулараркпаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml) создает экземпляр `SKCanvasView` и `Slider` для выбора угла. Обработчик событий `PaintSurface` в файле кода программной части [**BezierCircularArgPage.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml.cs) использует преобразование, чтобы задать точку (0, 0) в центре холста. Он рисует окружность центром в точке, для сравнения, а затем вычисляет две контрольные точки кривой Безье:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    // Draw the circle
    float radius = Math.Min(info.Width, info.Height) / 3;
    canvas.DrawCircle(0, 0, radius, blackStroke);

    // Get the value of the Slider
    float angle = (float)angleSlider.Value;

    // Calculate length of control point line
    float length = radius * 4 * (float)Math.Tan(Math.PI * angle / 180 / 4) / 3;

    // Calculate sin and cosine for half that angle
    float sin = (float)Math.Sin(Math.PI * angle / 180 / 2);
    float cos = (float)Math.Cos(Math.PI * angle / 180 / 2);

    // Find the end points
    SKPoint point0 = new SKPoint(-radius * sin, radius * cos);
    SKPoint point3 = new SKPoint(radius * sin, radius * cos);

    // Find the control points
    SKPoint point0Normalized = Normalize(point0);
    SKPoint point1 = point0 + new SKPoint(length * point0Normalized.Y,
                                          -length * point0Normalized.X);

    SKPoint point3Normalized = Normalize(point3);
    SKPoint point2 = point3 + new SKPoint(-length * point3Normalized.Y,
                                          length * point3Normalized.X);

    // Draw the points
    canvas.DrawCircle(point0.X, point0.Y, 10, blackFill);
    canvas.DrawCircle(point1.X, point1.Y, 10, blackFill);
    canvas.DrawCircle(point2.X, point2.Y, 10, blackFill);
    canvas.DrawCircle(point3.X, point3.Y, 10, blackFill);

    // Draw the tangent lines
    canvas.DrawLine(point0.X, point0.Y, point1.X, point1.Y, dottedStroke);
    canvas.DrawLine(point3.X, point3.Y, point2.X, point2.Y, dottedStroke);

    // Draw the Bezier curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(point0);
        path.CubicTo(point1, point2, point3);
        canvas.DrawPath(path, redStroke);
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

```

Начальная и конечная точки (`point0` и `point3`) рассчитываются на основе обычных математических уравнений для окружности. Так как элемент управления circle центрируется в (0, 0), эти точки могут также рассматриваться как радиального векторов в центре круга для длины окружности. Контрольные точки, в строках, которые являются касательной к элементу управления circle, поэтому они находятся под прямым углом в следующие радиального векторы. Вектор под прямым углом в другой — просто исходного вектора с местами координаты X и Y и один из них внесены отрицательное.

Вот приложение, выполняющееся с различными углами зрения:

[![тройной снимок экрана круговой страницы дуги Безье](beziers-images/beziercirculararc-small.png)](beziers-images/beziercirculararc-large.png#lightbox)

Внимательно посмотрите на третий снимок экрана, и вы увидите, что кривая Безье особенно отличается от полукруга угол равен 180 градусов, когда на экране iOS показывает, что кажется под просто прекрасно квартал круг угол равен 90 градусов.

Вычисление координаты две контрольные точки достаточно прост, когда четверть круга ориентирован следующим образом:

![Приближение окружности круга с кривой Безье](beziers-images/bezierarc90.png)

Если радиус окружности — 100, то *L* — 55, и это просто число, которое нужно запомнить.

Возведение в круг на странице **«окружность** » анимируется на фигуре между кругом и квадратом. Окружность приближена к четырем кривым Безье, координаты которых показаны в первом столбце определения массива в классе [`SquaringTheCirclePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/SquaringTheCirclePage.cs) :

```csharp
public class SquaringTheCirclePage : ContentPage
{
    SKPoint[,] points =
    {
        { new SKPoint(   0,  100), new SKPoint(     0,    125), new SKPoint() },
        { new SKPoint(  55,  100), new SKPoint( 62.5f,  62.5f), new SKPoint() },
        { new SKPoint( 100,   55), new SKPoint( 62.5f,  62.5f), new SKPoint() },
        { new SKPoint( 100,    0), new SKPoint(   125,      0), new SKPoint() },
        { new SKPoint( 100,  -55), new SKPoint( 62.5f, -62.5f), new SKPoint() },
        { new SKPoint(  55, -100), new SKPoint( 62.5f, -62.5f), new SKPoint() },
        { new SKPoint(   0, -100), new SKPoint(     0,   -125), new SKPoint() },
        { new SKPoint( -55, -100), new SKPoint(-62.5f, -62.5f), new SKPoint() },
        { new SKPoint(-100,  -55), new SKPoint(-62.5f, -62.5f), new SKPoint() },
        { new SKPoint(-100,    0), new SKPoint(  -125,      0), new SKPoint() },
        { new SKPoint(-100,   55), new SKPoint(-62.5f,  62.5f), new SKPoint() },
        { new SKPoint( -55,  100), new SKPoint(-62.5f,  62.5f), new SKPoint() },
        { new SKPoint(   0,  100), new SKPoint(     0,    125), new SKPoint() }
    };
    ...
}
```

Второй столбец содержит координаты четырех кривых Безье, которые определяют квадрат которого области составляет приблизительно так же, как площади круга. (Рисование квадрата с *точной* областью в виде заданной окружности является классической неразрешимой геометрической проблемой [возведения в круг](https://en.wikipedia.org/wiki/Squaring_the_circle).) Для отрисовки квадрата с кривыми Безье две контрольные точки для каждой кривой одинаковы, и они линейно связаны с начальной и конечной точками, поэтому кривая Безье визуализируется как прямая линия.

Третий столбец массива предназначен для интерполированные значения анимации. На странице устанавливается таймер для 16 миллисекунд, а обработчик `PaintSurface` вызывается по этой ставке:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    canvas.Translate(info.Width / 2, info.Height / 2);
    canvas.Scale(Math.Min(info.Width / 300, info.Height / 300));

    // Interpolate
    TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
    float t = (float)(timeSpan.TotalSeconds % 3 / 3);   // 0 to 1 every 3 seconds
    t = (1 + (float)Math.Sin(2 * Math.PI * t)) / 2;     // 0 to 1 to 0 sinusoidally

    for (int i = 0; i < 13; i++)
    {
        points[i, 2] = new SKPoint(
            (1 - t) * points[i, 0].X + t * points[i, 1].X,
            (1 - t) * points[i, 0].Y + t * points[i, 1].Y);
    }

    // Create the path and draw it
    using (SKPath path = new SKPath())
    {
        path.MoveTo(points[0, 2]);

        for (int i = 1; i < 13; i += 3)
        {
            path.CubicTo(points[i, 2], points[i + 1, 2], points[i + 2, 2]);
        }
        path.Close();

        canvas.DrawPath(path, cyanFill);
        canvas.DrawPath(path, blueStroke);
    }
}
```

Точки интерполируются на основе значения синусоидалли осЦиллатинг, равного `t`. Интерполированные точки затем используются для создания ряда из четырех соединенных кривых Безье. Ниже приведен запущенная анимация.

[![тройной снимок экрана возведения в круг на странице "окружность"](beziers-images/squaringthecircle-small.png)](beziers-images/squaringthecircle-large.png#lightbox)

Такой анимации бы невозможно без кривых, которые алгоритмически достаточно гибки для воспроизведения в виде прямых линий и дуг.

Страница « **бесконечность** » также использует преимущества кривой Безье для приблизительной окружности дуги. Ниже приведен обработчик `PaintSurface` из класса [`BezierInfinityPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierInfinityPage.cs) :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        path.MoveTo(0, 0);                                // Center
        path.CubicTo(  50,  -50,   95, -100,  150, -100); // To top of right loop
        path.CubicTo( 205, -100,  250,  -55,  250,    0); // To far right of right loop
        path.CubicTo( 250,   55,  205,  100,  150,  100); // To bottom of right loop
        path.CubicTo(  95,  100,   50,   50,    0,    0); // Back to center  
        path.CubicTo( -50,  -50,  -95, -100, -150, -100); // To top of left loop
        path.CubicTo(-205, -100, -250,  -55, -250,    0); // To far left of left loop
        path.CubicTo(-250,   55, -205,  100, -150,  100); // To bottom of left loop
        path.CubicTo( -95,  100,  -50,   50,    0,    0); // Back to center
        path.Close();

        SKRect pathBounds = path.Bounds;
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(0.9f * Math.Min(info.Width / pathBounds.Width,
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

Это может быть хорошее упражнение для построения эти координаты на бумаге graph, чтобы увидеть, как они связаны. Знак бесконечности центрируется относительно точки (0, 0), а два цикла иметь центров (–150, 0) и (150, 0) и радиусы 100. В серии `CubicTo`ных команд можно видеть координаты X точек управления, принимающих значения от – 95 до – 205 (эти значения – 150 Plus и минус 55), 205 и 95 (150 Plus и минус 55), а также 250 и-250 для прав и левых сторон. Единственное исключение — когда знак бесконечности пересекает самого в центре. В этом случае контрольные точки имеют координаты вместе с 50 и от – 50, чтобы выровнять кривой рядом с центром.

Ниже приведен знак бесконечности.

[![тройной снимок экрана страницы бесконечной Безье](beziers-images/bezierinfinity-small.png)](beziers-images/bezierinfinity-large.png#lightbox)

Это несколько ближе к центру, чем знак бесконечности, отображаемый на странице **Arc Infinity** , из [**трех способов рисования статьи дуги**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) .

## <a name="the-quadratic-bzier-curve"></a>Квадратичная кривая Безье

Квадратичная кривая Безье есть только одна контрольная точка и кривой определяется только трех точек: начальной точки, контрольная точка и конечную точку. Параметрические уравнения очень похожи на кривая Безье третьего порядка, за исключением того, что самый высокий показатель степени — 2, поэтому кривая является квадратичной полинома:

x(t) = (1 – t) ²x₀ + 2t (1 – t) x₁ + t²x₂

y(t) = (1 – t) ²y₀ + 2t (1 – t) y₁ + t²y₂

Чтобы добавить в контур квадратичную кривую Безье, используйте метод [`QuadTo`](xref:SkiaSharp.SKPath.QuadTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint)) или перегрузку [`QuadTo`](xref:SkiaSharp.SKPath.QuadTo(System.Single,System.Single,System.Single,System.Single)) с отдельными координатами `x` и `y`:

```csharp
public void QuadTo (SKPoint point1, SKPoint point2)

public void QuadTo (Single x1, Single y1, Single x2, Single y2)
```

Методы добавляют кривую из текущей позиции в `point2` с `point1` в качестве контрольной точки.

Вы можете поэкспериментировать с квадратичными кривыми Безье с помощью страницы **квадратичной кривой** , которая очень похожа на страницу **кривой Безье** , за исключением трех точек касания. Ниже приведен обработчик `PaintSurface` в файле кода программной части [**QuadraticCurve.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/QuadraticCurvePage.xaml.cs) :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with quadratic Bezier
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.QuadTo(touchPoints[1].Center,
                    touchPoints[2].Center);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[1].Center.X,
                    touchPoints[1].Center.Y,
                    touchPoints[2].Center.X,
                    touchPoints[2].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}
```

И здесь он выполняется:

[![тройной снимок экрана со страницей квадратичной кривой](beziers-images/quadraticcurve-small.png)](beziers-images/quadraticcurve-large.png#lightbox)

Пунктирные линии располагаются по касательной к кривой в начальной и конечной точки и соответствовать в точке управления.

Квадратичных сплайнов подходит в том случае, если вам нужны кривую общие фигуры, но вы предпочитаете удобство точки только один элемент управления, а не два. Отображает более эффективно, чем любой другой кривой, именно он используется системой в Skia для подготовки к просмотру эллиптические дуги квадратичных сплайнов.

Однако форма квадратичной кривой Безье не является эллиптической, поэтому для приблизительной дуги Бéзиерс требуется несколько квадратичных кривых. Квадратичная Безье является сегментом парабола.

## <a name="the-conic-bzier-curve"></a>Conic кривую Безье

Кривая Безье Коник &mdash; также известна как рациональная квадратичная кривая Безье &mdash; является относительно недавно добавленной к семейству кривых Безье. Как квадратичная кривая Безье rational квадратичная кривая Безье включает в себя начальную точку, конечная точка и точка управления. Но рациональная кривая Безье — также требуется значение *веса* . Это называется *рациональным* квадратичным квадратом, так как непараметрической формулы подразумевают соотношение.

Параметрические уравнения для X и Y, коэффициенты, которые совместно используют же знаменатель. Ниже приведено уравнение для знаменателя *в диапазоне от* 0 до 1 и значения веса *w*:

d(t) = (1 – t) ² + 2wt(1 – t) + t²

В теории rational квадратичная может включать в себя три значения отдельный вес, по одной для каждого из трех условий, но их может быть упрощен до единственного значение веса в середине термина.

Параметрические уравнения для координат X и Y, аналогичны параметрические уравнения для квадратичных сплайнов, за исключением того, что термин среднего также включает в себя значение веса, и выражение делится на делитель:

x(t) = ((1 – t) ²x₀ + 2wt (1 – t) x₁ + t²x₂)) ÷ d(t)

y(t) = ((1 – t) ²y₀ + 2wt (1 – t) y₁ + t²y₂)) ÷ d(t)

Рациональные квадратичные кривые Безье также называются *коникс* , поскольку они могут точно представлять сегменты любого коник раздела &mdash; хиперболас, параболас, эллипсы и круги.

Чтобы добавить рациональную квадратичную кривую Безье к контуру, используйте метод [`ConicTo`](xref:SkiaSharp.SKPath.ConicTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,System.Single)) или перегрузку [`ConicTo`](xref:SkiaSharp.SKPath.ConicTo(System.Single,System.Single,System.Single,System.Single,System.Single)) с отдельными координатами `x` и `y`:

```csharp
public void ConicTo (SKPoint point1, SKPoint point2, Single weight)

public void ConicTo (Single x1, Single y1, Single x2, Single y2, Single weight)
```

Обратите внимание на последний параметр `weight`.

Страница **кривой Коник** позволяет экспериментировать с этими кривыми. Класс `ConicCurvePage` является производным от класса `InteractivePage`. Файл [**кониккурвепаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml) создает экземпляр `Slider`, чтобы выбрать значение веса от – 2 до 2. Файл кода программной части [**ConicCurvePage.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml.cs) создает три объекта `TouchPoint`, а обработчик `PaintSurface` просто выводит результирующую кривую с касательными к контрольным точкам:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with conic curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.ConicTo(touchPoints[1].Center,
                     touchPoints[2].Center,
                     (float)weightSlider.Value);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[1].Center.X,
                    touchPoints[1].Center.Y,
                    touchPoints[2].Center.X,
                    touchPoints[2].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}
```

Здесь выполняется:

[![тройной снимок экрана со страницей кривой Коник](beziers-images/coniccurve-small.png)](beziers-images/coniccurve-large.png#lightbox)

Как вы видите, контрольная точка похоже на включение внесенных изменений кривой к более, если вес выше. Если вес равен нулю, кривая становится прямую линию от начальной точки в конечную точку.

Теоретически возможны отрицательные веса, и кривая *отбрасывается* от контрольной точки. Однако весовые коэффициенты – 1 или ниже приводят к тому, что знаменатель в параметрической уравнениях становится отрицательным для определенных значений *t*. Возможно, по этой причине отрицательные веса игнорируются в методах `ConicTo`. Программа **кривой Коник** позволяет задать отрицательные веса, но, как можно увидеть, эксперименты, отрицательные весовые коэффициенты имеют тот же результат, что и нулевой вес, и приводят к отрисовке прямой линии.

Очень легко получить контрольную точку и вес, чтобы использовать метод `ConicTo` для отрисовки дуги окружности вверх (но не включая) полукруга. На следующей схеме касательные из начальной и конечной точек соответствовать в точке управления.

![Коникная дуга, Визуализация дуги окружности](beziers-images/conicarc.png)

Тригонометрические функции можно использовать для определения расстояния контрольной точки из центра: это радиус круга, деленное косинус угла половину α. Чтобы нарисовать дугу между начальной и конечной точек, присвойте весу этой же косинус половину угла. Обратите внимание на то, что если угол в 180 градусов, затем касательные никогда не соответствуют и вес равен нулю. Но для углов меньше, чем 180 градусов, математические работает нормально.

Это показано на странице **дуги окружности Коник** . Файл [**коникЦиркуларарк. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml) создает `Slider` для выбора угла. Обработчик `PaintSurface` в файле кода программной части [**ConicCircularArc.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml.cs) вычисляет контрольную точку и вес:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    // Draw the circle
    float radius = Math.Min(info.Width, info.Height) / 4;
    canvas.DrawCircle(0, 0, radius, blackStroke);

    // Get the value of the Slider
    float angle = (float)angleSlider.Value;

    // Calculate sin and cosine for half that angle
    float sin = (float)Math.Sin(Math.PI * angle / 180 / 2);
    float cos = (float)Math.Cos(Math.PI * angle / 180 / 2);

    // Find the points and weight
    SKPoint point0 = new SKPoint(-radius * sin, radius * cos);
    SKPoint point1 = new SKPoint(0, radius / cos);
    SKPoint point2 = new SKPoint(radius * sin, radius * cos);
    float weight = cos;

    // Draw the points
    canvas.DrawCircle(point0.X, point0.Y, 10, blackFill);
    canvas.DrawCircle(point1.X, point1.Y, 10, blackFill);
    canvas.DrawCircle(point2.X, point2.Y, 10, blackFill);

    // Draw the tangent lines
    canvas.DrawLine(point0.X, point0.Y, point1.X, point1.Y, dottedStroke);
    canvas.DrawLine(point2.X, point2.Y, point1.X, point1.Y, dottedStroke);

    // Draw the conic
    using (SKPath path = new SKPath())
    {
        path.MoveTo(point0);
        path.ConicTo(point1, point2, weight);
        canvas.DrawPath(path, redStroke);
    }
}
```

Как видите, нет визуальной разницы между `ConicTo`ным путем, показанным красным цветом, и базовым кружком, отображаемым для справки:

[![тройной снимок страницы дуги окружности Коник](beziers-images/coniccirculararc-small.png)](beziers-images/coniccirculararc-large.png#lightbox)

Но задать угол в 180 градусов, а также математика "fail".

В этом случае `ConicTo` не поддерживает отрицательные веса, так как в теории (основанных на параметрической уравнениях) окружность может быть выполнена с помощью другого вызова `ConicTo` с теми же точками, но с отрицательным значением веса. Это позволит создать весь круг с двумя `ConicTo` кривыми на основе любого угла между (но не включительно) нулями и 180 градусами.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
