---
title: Данные о пути SVG в SkiaSharp
description: В этой статье объясняется, как определить SkiaSharp пути с помощью текстовых строк в формате масштабируемой векторной графики и демонстрируется пример кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 1D53067B-3502-4D74-B89D-7EC496901AE2
author: davidbritch
ms.author: dabritch
ms.date: 05/24/2017
ms.openlocfilehash: 809bcd8288c4c4205b3110418aeae0e08bf21dd6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029466"
---
# <a name="svg-path-data-in-skiasharp"></a>Данные о пути SVG в SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Определение путей с помощью текстовых строк в формате масштабируемой векторной графики_

Класс [`SKPath`](xref:SkiaSharp.SKPath) поддерживает определение всех объектов Path из текстовых строк в формате, установленном спецификацией масштабируемой векторной графики (SVG). Далее в этой статье показано, как можно представить весь путь, например, в текстовой строке:

![](path-data-images/pathdatasample.png "A sample path defined with SVG path data")

SVG — это язык программирования графики на основе XML для веб-страниц. Поскольку в языке SVG необходимо разрешить определение путей в разметке, а не в ряде вызовов функций, стандарт SVG включает очень краткий способ указания всего графического контура в виде текстовой строки.

В SkiaSharp этот формат называется "SVG Path-Data". Формат также поддерживается в средах программирования на основе XAML в Windows, включая Windows Presentation Foundation и универсальная платформа Windows, где он называется [синтаксисом разметки пути](/dotnet/framework/wpf/graphics-multimedia/path-markup-syntax) или [синтаксисом команд Move и Draw](/windows/uwp/xaml-platform/move-draw-commands-syntax/). Он также может использоваться в качестве формата обмена для векторных изображений, особенно в текстовых файлах, таких как XML.

Класс [`SKPath`](xref:SkiaSharp.SKPath) определяет два метода с словами, `SvgPathData` в их именах:

```csharp
public static SKPath ParseSvgPathData(string svgPath)

public string ToSvgPathData()
```

Статический [`ParseSvgPathData`](xref:SkiaSharp.SKPath.ParseSvgPathData(System.String)) метод преобразует строку в объект `SKPath`, а [`ToSvgPathData`](xref:SkiaSharp.SKPath.ToSvgPathData) преобразует объект `SKPath` в строку.

Ниже приведена SVG-строка для пяти звезд, центрированных в точке (0, 0) с радиусом 100:

```
"M 0 -100 L 58.8 90.9, -95.1 -30.9, 95.1 -30.9, -58.8 80.9 Z"
```

Буквы — это команды, создающие объект `SKPath`: `M` обозначает `MoveTo` вызов, `L` — `LineTo`, `Z` — `Close`, чтобы закрыть профиль. Каждая пара чисел предоставляет координату X и Y точки. Обратите внимание, что за командой `L` следуют несколько точек, разделенных запятыми. В ряде координат и точек запятые и пробелы обрабатываются одинаково. Некоторые программисты предпочитают добавлять запятые между координатами X и Y, а не между точками, но запятые или пробелы необходимы только для того, чтобы избежать неоднозначности. Это вполне допустимо:

```
"M0-100L58.8 90.9-95.1-30.9 95.1-30.9-58.8 80.9Z"
```

Синтаксис данных пути SVG формально описан в [разделе 8,3 спецификации SVG](https://www.w3.org/TR/SVG11/paths.html#PathData). Ниже приведено краткое описание.

## <a name="moveto"></a>**MoveTo**

```
M x y
```

Он начинает новый профиль в пути, устанавливая текущую точку. Данные пути должны всегда начинаться с `M` команды.

## <a name="lineto"></a>**LineTo**

```
L x y ...
```

Эта команда добавляет прямую линию (или линии) к пути и устанавливает новую текущую точку в конец последней строки. Можно выполнить команду `L` с несколькими парами координат *x* и *y* .

## <a name="horizontal-lineto"></a>**Горизонтально LineTo**

```
H x ...
```

Эта команда добавляет горизонтальную линию к контуру и устанавливает новую текущую точку в конец строки. Можно выполнить команду `H` с несколькими координатами *x* , но это не имеет смысла.

## <a name="vertical-line"></a>**Вертикальная линия**

```
V y ...
```

Эта команда добавляет вертикальную линию к контуру и устанавливает новую текущую точку в конец строки.

## <a name="close"></a>**Закрыть**

```
Z
```

Команда `C` закрывает профиль, добавляя прямую линию с текущей позицией в начало контура.

## <a name="arcto"></a>**аркто**

Команда для добавления эллиптической дуги к профилю — это самая сложная команда во всей спецификации данных Path SVG. Это единственная команда, в которой числа могут представлять нечто, отличное от значений координат.

```
A rx ry rotation-angle large-arc-flag sweep-flag x y ...
```

Параметры *RX* и *дые* — это горизонтальные и вертикальные Радиусы эллипса. *Угол поворота* — по часовой стрелке в градусах.

Установите *флаг Large-Arc* в значение 1 для большой дуги или значение 0 для небольшой дуги.

Установите *флаг поворота* равным 1 в качестве значения по часовой стрелке и 0 для счетчика — по часовой стрелке.

Дуга рисуется до точки (*x*, *y*), которая становится новой текущей позицией.

## <a name="cubicto"></a>**кубикто**

```
C x1 y1 x2 y2 x3 y3 ...
```

Эта команда добавляет кривую Безье третьего порядка к текущему положению (*X3*, *Y3*), которая станет новой текущей позицией. Точки (*x1*, *Y1*) и (*x2*, *Y2*) являются контрольными точками.

Несколько кривых Безье можно указать с помощью одной команды `C`. Число точек должно быть кратным 3.

Имеется также гладкая команда кривой Безье:

```
S x2 y2 x3 y3 ...
```

Эта команда должна следовать обычной команде Безье (хотя это не является обязательным требованием). Команда Smooth Безье вычисляет первую контрольную точку таким образом, что она является отражением второй контрольной точки предыдущей Безье вокруг своей взаимной точки. Таким образом, эти три точки являются колинейными, а соединение между двумя кривыми Безье является гладким.

## <a name="quadto"></a>**куадто**

```
Q x1 y1 x2 y2 ...
```

Для квадратичных кривых Безье количество точек должно быть кратным 2. Контрольная точка — (*x1*, *Y1*), конечная точка (и Новая текущая позиции) — (*x2*, *Y2*).

Также существует команда гладкого квадратичного изгиба:

```
T x2 y2 ...
```

Контрольная точка вычисляется на основе контрольной точки предыдущей квадратичной кривой.

Все эти команды также доступны в относительных версиях, где точки координат зависят от текущей позиции. Эти относительные команды начинаются с строчных букв, например `c`, а не `C` для относительной версии команды кубической Безье.

Это экстент определения данных SVG Path. Не существует средства для повторяющихся групп команд или для выполнения вычислений любого типа. Команды для `ConicTo` или другие типы спецификаций Arc недоступны.

Статический метод [`SKPath.ParseSvgPathData`](xref:SkiaSharp.SKPath.ParseSvgPathData(System.String)) принимает допустимую строку команд SVG. При обнаружении любой синтаксической ошибки метод возвращает `null`. Это единственное обозначение ошибки.

Метод [`ToSvgPathData`](xref:SkiaSharp.SKPath.ToSvgPathData) удобен для получения данных о контуре SVG из существующего объекта `SKPath` для перемещения в другую программу или для хранения в текстовом формате файла, например XML. (Метод `ToSvgPathData` не показан в примере кода в этой статье.) *Не* следует рассчитывать `ToSvgPathData` возвращать строку, соответствующую только вызовам методов, создавшим путь. В частности, вы обнаружите, что дуги преобразуются в несколько `QuadTo` команд, и вот как они отображаются в данных пути, возвращаемых из `ToSvgPathData`.

Страница « **Hello Data Path** » записывает слово «Hello» с использованием данных о пути SVG. Объекты `SKPath` и `SKPaint` определены как поля в классе [`PathDataHelloPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataHelloPage.cs) :

```csharp
public class PathDataHelloPage : ContentPage
{
    SKPath helloPath = SKPath.ParseSvgPathData(
        "M 0 0 L 0 100 M 0 50 L 50 50 M 50 0 L 50 100" +                // H
        "M 125 0 C 60 -10, 60 60, 125 50, 60 40, 60 110, 125 100" +     // E
        "M 150 0 L 150 100, 200 100" +                                  // L
        "M 225 0 L 225 100, 275 100" +                                  // L
        "M 300 50 A 25 50 0 1 0 300 49.9 Z");                           // O

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };
    ...
}
```

Путь, определяющий текстовую строку, начинается с левого верхнего угла в точке (0, 0). Каждая буква имеет ширину 50 единиц и 100 единицы измерения, а буквы разделяются на 25 единиц, что означает, что весь путь имеет ширину 350 единиц.

"H" для "Hello" состоит из 3 1-линейных демонстраций, а "E" — две соединенные кривые Безье третьего порядка. Обратите внимание, что за командой `C` следуют шесть точек, а две контрольных точки имеют координаты Y — 10 и 110, что помещает их за пределы диапазона координат Y других букв. "L" — две соединенные линии, а "O" — эллипс, отображаемый с помощью команды `A`.

Обратите внимание, что команда `M`, которая начинает последний профиль, задает позицию в точке (350, 50), которая является вертикальной центральной точкой слева от "O". Как указано в первых цифрах после команды `A`, эллипс имеет горизонтальный радиус 25 и вертикальный радиус 50. Конечная точка обозначается последней парой чисел в команде `A`, которая представляет точку (300, 49,9). Это намеренно немного отличается от начальной точки. Если конечная точка установлена равна начальной точке, дуга не будет подготовлена к просмотру. Чтобы нарисовать полный эллипс, необходимо установить конечную точку в значение (но не равно) начальной точке или использовать две или более `A` команды, каждая для части полного эллипса.

Может потребоваться добавить следующую инструкцию в конструктор страницы, а затем задать точку останова для проверки результирующей строки:

```csharp
string str = helloPath.ToSvgPathData();
```

Вы обнаружите, что дуга заменена длинной серией `Q` команд для поэтапного приближения дуги с использованием квадратичных кривых Безье.

Обработчик `PaintSurface` получает ограниченные границы пути, которые не включают контрольные точки для кривых "E" и "O". Три преобразования перемещают центр пути к точке (0, 0), масштабируют путь до размера холста (но при этом учитывается ширина штриха), а затем перемещается центр контура в центр холста:

```csharp
public class PathDataHelloPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect bounds;
        helloPath.GetTightBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);

        canvas.Scale(info.Width / (bounds.Width + paint.StrokeWidth),
                     info.Height / (bounds.Height + paint.StrokeWidth));

        canvas.Translate(-bounds.MidX, -bounds.MidY);

        canvas.DrawPath(helloPath, paint);
    }
}
```

Путь заполняет холст, который выглядит более разумным при просмотре в альбомном режиме:

[![](path-data-images/pathdatahello-small.png "Triple screenshot of the Path Data Hello page")](path-data-images/pathdatahello-large.png#lightbox "Triple screenshot of the Path Data Hello page")

Страница **Data Cat** выглядит аналогично. Объекты Path и Paint определяются как поля в классе [`PathDataCatPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) :

```csharp
public class PathDataCatPage : ContentPage
{
    SKPath catPath = SKPath.ParseSvgPathData(
        "M 160 140 L 150 50 220 103" +              // Left ear
        "M 320 140 L 330 50 260 103" +              // Right ear
        "M 215 230 L 40 200" +                      // Left whiskers
        "M 215 240 L 40 240" +
        "M 215 250 L 40 280" +
        "M 265 230 L 440 200" +                     // Right whiskers
        "M 265 240 L 440 240" +
        "M 265 250 L 440 280" +
        "M 240 100" +                               // Head
        "A 100 100 0 0 1 240 300" +
        "A 100 100 0 0 1 240 100 Z" +
        "M 180 170" +                               // Left eye
        "A 40 40 0 0 1 220 170" +
        "A 40 40 0 0 1 180 170 Z" +
        "M 300 170" +                               // Right eye
        "A 40 40 0 0 1 260 170" +
        "A 40 40 0 0 1 300 170 Z");

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 5
    };
    ...
}
```

В качестве заголовка Cat используется окружность, и в нем отображаются две команды `A`, каждая из которых рисует полукруг. Обе `A` команды для Head определяют горизонтальные и вертикальные радиусы 100. Первая Дуга начинается в (240, 100) и заканчивается в (240, 300), которая станет начальной точкой для второй дуги, которая заканчивается обратно (240, 100).

Эти два глаза также отображаются с двумя командами `A`, как и в головке Cat, вторая `A`ная команда заканчивается в той же точке, что и начало первой команды `A`. Однако эти пары `A` команд не определяют эллипс. Для каждой дуги используется 40 единиц, а радиус — также 40 единиц. Это означает, что эти дуги не являются полными полукругами.

Обработчик `PaintSurface` выполняет аналогичные преобразования, как в предыдущем примере, но устанавливает один `Scale` фактор для сохранения пропорций и предоставляет небольшое поле, чтобы блочнойа не потратила на границы экрана.

```csharp
public class PathDataCatPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);

        SKRect bounds;
        catPath.GetBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);

        canvas.Scale(0.9f * Math.Min(info.Width / bounds.Width,
                                     info.Height / bounds.Height));

        canvas.Translate(-bounds.MidX, -bounds.MidY);

        canvas.DrawPath(catPath, paint);
    }
}
```

Вот работающая программа:

[![](path-data-images/pathdatacat-small.png "Triple screenshot of the Path Data Cat page")](path-data-images/pathdatacat-large.png#lightbox "Triple screenshot of the Path Data Cat page")

Как правило, если объект `SKPath` определен как поле, то в конструкторе или другом методе должны быть определены контуры пути. Однако при использовании данных контура SVG вы видели, что путь может быть указан полностью в определении поля.

В примере ранее **некрасивого аналогового часов** в статье [**вращение преобразования**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md) показаны руки часов в виде простых линий. Программа **довольно аналоговых часов** , приведенная ниже, заменяет эти строки `SKPath`ными объектами, определенными в [`PrettyAnalogClockPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PrettyAnalogClockPage.cs) классе вместе с объектами `SKPaint`:

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    // Clock hands pointing straight up
    SKPath hourHandPath = SKPath.ParseSvgPathData(
        "M 0 -60 C   0 -30 20 -30  5 -20 L  5   0" +
                "C   5 7.5 -5 7.5 -5   0 L -5 -20" +
                "C -20 -30  0 -30  0 -60 Z");

    SKPath minuteHandPath = SKPath.ParseSvgPathData(
        "M 0 -80 C   0 -75  0 -70  2.5 -60 L  2.5   0" +
                "C   2.5 5 -2.5 5 -2.5   0 L -2.5 -60" +
                "C 0 -70  0 -75  0 -80 Z");

    SKPath secondHandPath = SKPath.ParseSvgPathData(
        "M 0 10 L 0 -80");

    // SKPaint objects
    SKPaint handStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2,
        StrokeCap = SKStrokeCap.Round
    };

    SKPaint handFillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Gray
    };
    ...
}
```

В час и минуту у руки теперь есть заключенные в них области. Чтобы сделать эти руки разными, они рисуются черным контуром и серой заливкой с помощью объектов `handStrokePaint` и `handFillPaint`.

В предыдущем примере **некрасивого аналогового времени** небольшие круги, помеченные часами и минутами, были прорисованы в цикле. В этом **простом** примере используется совершенно другой подход: часы и минуты — это пунктирные линии, нарисованные с помощью `minuteMarkPaint` и `hourMarkPaint` объектов:

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    SKPaint minuteMarkPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3,
        StrokeCap = SKStrokeCap.Round,
        PathEffect = SKPathEffect.CreateDash(new float[] { 0, 3 * 3.14159f }, 0)
    };

    SKPaint hourMarkPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 6,
        StrokeCap = SKStrokeCap.Round,
        PathEffect = SKPathEffect.CreateDash(new float[] { 0, 15 * 3.14159f }, 0)
    };
    ...
}
```

В статье " [**точки и тире**](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md) " обсуждалось, как можно использовать метод [`SKPathEffect.CreateDash`](xref:SkiaSharp.SKPathEffect.CreateDash*) для создания пунктирной линии. Первый аргумент — это `float` массив, который обычно содержит два элемента: первый элемент — это длина дефисов, второй элемент — разрыв между тире. Если свойство `StrokeCap` имеет значение `SKStrokeCap.Round`, то скругленные концы штриха фактически дополняют длину тире на толщину штриха на обеих сторонах тире. Поэтому при установке первого элемента массива равным 0 создается пунктирная линия.

Расстояние между этими точками регулируется вторым элементом массива. Как вы увидите чуть позже, эти два объекта `SKPaint` используются для рисования кругов с радиусом 90 единиц. Таким образом, длина окружности для этого круга составляет 180 π, что означает, что 60 минут должны отображаться в каждой единице 3π, которая является вторым значением в массиве `float` в `minuteMarkPaint`. 12-часовые метки должны находиться в каждой единице 15π, которая является значением во втором массиве `float`.

Класс `PrettyAnalogClockPage` задает таймеру на недействительность поверхности каждые 16 миллисекунд, а обработчик `PaintSurface` вызывается с такой частотой. Более ранние определения объектов `SKPath` и `SKPaint` позволяют выполнять очень чистый код рисования:

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Transform for 100-radius circle in center
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(Math.Min(info.Width / 200, info.Height / 200));

        // Draw circles for hour and minute marks
        SKRect rect = new SKRect(-90, -90, 90, 90);
        canvas.DrawOval(rect, minuteMarkPaint);
        canvas.DrawOval(rect, hourMarkPaint);

        // Get time
        DateTime dateTime = DateTime.Now;

        // Draw hour hand
        canvas.Save();
        canvas.RotateDegrees(30 * dateTime.Hour + dateTime.Minute / 2f);
        canvas.DrawPath(hourHandPath, handStrokePaint);
        canvas.DrawPath(hourHandPath, handFillPaint);
        canvas.Restore();

        // Draw minute hand
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Minute + dateTime.Second / 10f);
        canvas.DrawPath(minuteHandPath, handStrokePaint);
        canvas.DrawPath(minuteHandPath, handFillPaint);
        canvas.Restore();

        // Draw second hand
        double t = dateTime.Millisecond / 1000.0;

        if (t < 0.5)
        {
            t = 0.5 * Easing.SpringIn.Ease(t / 0.5);
        }
        else
        {
            t = 0.5 * (1 + Easing.SpringOut.Ease((t - 0.5) / 0.5));
        }

        canvas.Save();
        canvas.RotateDegrees(6 * (dateTime.Second + (float)t));
        canvas.DrawPath(secondHandPath, handStrokePaint);
        canvas.Restore();
    }
}
```

Однако что-то особенно сделано с другой рукой. Так как часы обновляются каждые 16 миллисекунд, свойство `Millisecond` значения `DateTime` может быть использовано для анимации второй руки, а не для перемещения в дискретных переходах с секунды на секунду. Но этот код не допускает плавность перемещения. Вместо этого он использует Xamarin. Forms [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) и [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut) функции плавности анимации для другого вида движения. Эти функции плавности приводят к тому, что вторая рука перемещается жеркиерным способом &mdash; немного перед перемещением, а затем немного переводится в другое место, что не может быть воспроизведено в этих статических снимках экрана:

[![](path-data-images/prettyanalogclock-small.png "Triple screenshot of the Pretty Analog Clock page")](path-data-images/prettyanalogclock-large.png#lightbox "Triple screenshot of the Pretty Analog Clock page")

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
