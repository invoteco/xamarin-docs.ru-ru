---
title: Основные сведения о пути в SkiaSharp
description: В этой статье исследует объект SkiaSharp SKPath объединения соединенных линий и кривых и демонстрирует это с помощью примера кода.
ms.prod: xamarin
ms.assetid: A7EDA6C2-3921-4021-89F3-211551E430F1
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: c892adf2f75ec00c4a9ee171ded78f79bb8227e9
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725198"
---
# <a name="path-basics-in-skiasharp"></a>Основные сведения о пути в SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Изучите объект SkiaSharp Скпас для объединения Соединенных линий и кривых_

Одной из самых важных особенностей графический контур является возможность определить, когда должны быть подключены несколько строк, и когда они не должны быть подключены. Разница может быть значительной, как верхние края этих двух треугольниках показывают:

![](paths-images/connectedlinesexample.png "Two triangles showing the difference between connected and disconnected lines")

Графический путь инкапсулируется объектом [`SKPath`](xref:SkiaSharp.SKPath) . Путь — это коллекция из одного или нескольких *контуров*. Каждый профиль является коллекцией *Соединенных* прямых линий и кривых. Профили не подключены друг к другу, но они могут визуально перекрываться. Иногда один профиль может перекрывать сам себя.

Обычно контур начинается с вызова следующего метода `SKPath`:

- [`MoveTo`](xref:SkiaSharp.SKPath.MoveTo*) для начала нового профиля

Аргументом для этого метода является единственная точка, которую можно выразить как `SKPoint` значение или как отдельные координаты X и Y. `MoveTo` вызов устанавливает точку в начале контура и начальную *текущую точку*. Можно вызвать следующие методы для продолжения профиль с линий или кривых из текущего расположения на момент, указанный в методе, которая становится новой текущей точки:

- [`LineTo`](xref:SkiaSharp.SKPath.LineTo*) Добавление прямой линии к контуру
- [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo*) для добавления дуги, которая является линией окружности круга или эллипса
- [`CubicTo`](xref:SkiaSharp.SKPath.CubicTo*) для добавления сплайна Безье третьего порядка
- [`QuadTo`](xref:SkiaSharp.SKPath.QuadTo*) добавить кривую Безье второго квадратичного сплайна
- [`ConicTo`](xref:SkiaSharp.SKPath.ConicTo*) для добавления рационального сплайна Безье квадратичных кривых, который может точно визуализировать Коник разделы (многоточия, параболас и хиперболас)

Эти пять методов не содержат все сведения, необходимые для описания линий или кривых. Каждый из этих пяти методов работает совместно с текущей точкой установить непосредственно перед вызовом метода. Например, метод `LineTo` добавляет прямую линию к контуру на основе текущей точки, поэтому параметр для `LineTo` является единственной точкой.

Класс `SKPath` также определяет методы, имена которых совпадают с именами этих шести методов, но с `R` в начале:

- [`RMoveTo`](xref:SkiaSharp.SKPath.RMoveTo*)
- [`RLineTo`](xref:SkiaSharp.SKPath.RLineTo*)
- [`RArcTo`](xref:SkiaSharp.SKPath.RArcTo*)
- [`RCubicTo`](xref:SkiaSharp.SKPath.RCubicTo*)
- [`RQuadTo`](xref:SkiaSharp.SKPath.RQuadTo*)
- [`RConicTo`](xref:SkiaSharp.SKPath.RConicTo*)

`R` обозначает *относительную*. Эти методы имеют тот же синтаксис, что и соответствующие методы без `R`, но они относительны по отношению к текущей точке. Это удобно для рисования аналогичные части пути в методе, который можно вызывать несколько раз.

Контур заканчивается другим вызовом `MoveTo` или `RMoveTo`, который начинает новый профиль, или вызовом `Close`, который закрывает профиль. Метод `Close` автоматически добавляет прямую линию от текущей точки к первой точке контура и помечает путь как закрытый, что означает, что он будет визуализирован без наконечников обводки.

Разница между открытыми и закрытыми контурами показана на **двух треугольных** страницах, где для отрисовки двух треугольников используется объект `SKPath` с двумя контурами. Первый профиль является открытым и закрывается второй. Ниже приведен класс [`TwoTriangleContoursPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/TwoTriangleContoursPage.cs) :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Create the path
    SKPath path = new SKPath();

    // Define the first contour
    path.MoveTo(0.5f * info.Width, 0.1f * info.Height);
    path.LineTo(0.2f * info.Width, 0.4f * info.Height);
    path.LineTo(0.8f * info.Width, 0.4f * info.Height);
    path.LineTo(0.5f * info.Width, 0.1f * info.Height);

    // Define the second contour
    path.MoveTo(0.5f * info.Width, 0.6f * info.Height);
    path.LineTo(0.2f * info.Width, 0.9f * info.Height);
    path.LineTo(0.8f * info.Width, 0.9f * info.Height);
    path.Close();

    // Create two SKPaint objects
    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Magenta,
        StrokeWidth = 50
    };

    SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Cyan
    };

    // Fill and stroke the path
    canvas.DrawPath(path, fillPaint);
    canvas.DrawPath(path, strokePaint);
}
```

Первый профиль состоит из вызова [`MoveTo`](xref:SkiaSharp.SKPath.MoveTo(System.Single,System.Single)) с использованием координат X и Y, а не `SKPoint` значения, за которым следуют три вызова [`LineTo`](xref:SkiaSharp.SKPath.LineTo(System.Single,System.Single)) для рисования трех сторон треугольника. Второй профиль имеет только два вызова `LineTo` но завершает профиль с вызовом [`Close`](xref:SkiaSharp.SKPath.Close), который закрывает профиль. Различие важно:

[![](paths-images/twotrianglecontours-small.png "Triple screenshot of the Two Triangle Contours page")](paths-images/twotrianglecontours-large.png#lightbox "Triple screenshot of the Two Triangle Contours page")

Как вы видите, первый профиль очевидно, что представляет собой ряд три соединенных линий, но окончания не сможет подключиться с самого начала. В верхней перекрываться две строки. Второй контур, очевидно, закрыт и был выполнен с одним меньшим числом `LineTo`ных вызовов, так как метод `Close` автоматически добавляет завершающую строку для закрытия контура.

`SKCanvas` определяет только один метод [`DrawPath`](xref:SkiaSharp.SKCanvas.DrawPath(SkiaSharp.SKPath,SkiaSharp.SKPaint)) , который в этой демонстрации вызывается дважды для заполнения и обводки контура. Все профили заполняются, даже те, которые не закрыты. В целях заполнения незамкнутых прямой предполагается, что существует между начальной и конечной точек из контуров. Если удалить последний `LineTo` из первого профиля или удалить вызов `Close` из второго контура, каждый профиль будет иметь только две стороны, но будет заполнен так, как если бы он был треугольником.

`SKPath` определяет множество других методов и свойств. Следующие методы позволяют добавлять контуров весь путь, который может быть закрыт или не закрывается в зависимости от метода:

- [`AddRect`](xref:SkiaSharp.SKPath.AddRect*)
- [`AddRoundedRect`](xref:SkiaSharp.SKPath.AddRoundedRect(SkiaSharp.SKRect,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddCircle`](xref:SkiaSharp.SKPath.AddCircle(System.Single,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddOval`](xref:SkiaSharp.SKPath.AddOval(SkiaSharp.SKRect,SkiaSharp.SKPathDirection))
- [`AddArc`](xref:SkiaSharp.SKPath.AddArc(SkiaSharp.SKRect,System.Single,System.Single)) добавить кривую для окружности эллипса
- [`AddPath`](xref:SkiaSharp.SKPath.AddPath*) добавить еще один путь к текущему пути
- [`AddPathReverse`](xref:SkiaSharp.SKPath.AddPathReverse(SkiaSharp.SKPath)) , чтобы добавить еще один путь в обратную

Помните, что объект `SKPath` определяет только геометрию, &mdash; ряд точек и соединений. Только если `SKPath` объединяется с `SKPaint`ным объектом, то контур отображается с определенным цветом, шириной штрихов и т. д. Кроме того, помните, что объект `SKPaint`, передаваемый методу `DrawPath`, определяет характеристики всего пути. Если вы хотите нарисуйте что-нибудь, требующих несколько цветов, необходимо использовать отдельный путь для каждого цвета.

Точно так же, как внешний вид начала и конца линии определяется наконечником обводки, внешний вид соединения между двумя строками определяется *соединением с обводкой*. Это можно указать, задав для свойства [`StrokeJoin`](xref:SkiaSharp.SKPaint.StrokeJoin) `SKPaint` элемент перечисления [`SKStrokeJoin`](xref:SkiaSharp.SKStrokeJoin) .

- `Miter` для точки подключения
- `Round` для скругленного объединения
- `Bevel` для неровного объединения

На странице « **Присоединение штрихов** » показаны три объединения штрихов с кодом, похожим на страницу « **штрихи** ». Это `PaintSurface` обработчик событий в классе [`StrokeJoinsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/StrokeJoinsPage.cs) :

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
        TextAlign = SKTextAlign.Right
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

    float xText = info.Width - 100;
    float xLine1 = 100;
    float xLine2 = info.Width - xLine1;
    float y = 2 * textPaint.FontSpacing;
    string[] strStrokeJoins = { "Miter", "Round", "Bevel" };

    foreach (string strStrokeJoin in strStrokeJoins)
    {
        // Display text
        canvas.DrawText(strStrokeJoin, xText, y, textPaint);

        // Get stroke-join value
        SKStrokeJoin strokeJoin;
        Enum.TryParse(strStrokeJoin, out strokeJoin);

        // Create path
        SKPath path = new SKPath();
        path.MoveTo(xLine1, y - 80);
        path.LineTo(xLine1, y + 80);
        path.LineTo(xLine2, y + 80);

        // Display thick line
        thickLinePaint.StrokeJoin = strokeJoin;
        canvas.DrawPath(path, thickLinePaint);

        // Display thin line
        canvas.DrawPath(path, thinLinePaint);
        y += 3 * textPaint.FontSpacing;
    }
}
```

Вот работающая программа:

[![](paths-images/strokejoins-small.png "Triple screenshot of the Stroke Joins page")](paths-images/strokejoins-large.png#lightbox "Triple screenshot of the Stroke Joins page")

Фацетное соединение состоит из sharp точки, где строки подключения. Если две строки присоединяются к небольшой углом, фацетное соединение может быть довольно длинными. Чтобы избежать чрезмерно длинных срезов соединения, длина углового соединения ограничена значением свойства [`StrokeMiter`](xref:SkiaSharp.SKPaint.StrokeMiter) `SKPaint`. Фацетного соединения, в которых превышает длину исчезает станет скошенное соединение.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
