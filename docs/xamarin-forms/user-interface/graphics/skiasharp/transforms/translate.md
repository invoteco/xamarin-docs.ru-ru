---
title: Преобразование переноса
description: В этой статье рассматривается использование преобразования переноса необходимо сдвинуть графики SkiaSharp в приложениях Xamarin.Forms и демонстрирует это с помощью примера кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: BD28ADA1-49F9-44E2-A548-46024A29882F
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: f1efd7610b32e6a3903d34fc2f8b5a6e20c9da8a
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723607"
---
# <a name="the-translate-transform"></a>Преобразование переноса

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Сведения об использовании преобразования "преобразование" для сдвига SkiaSharp графики_

Простейший тип преобразования в SkiaSharp — *Преобразование или* *перевод* . Это преобразование сдвигает графическими объектами в горизонтальном и вертикальном направлениях. В некотором смысле перевод — наиболее ненужные преобразования, так как обычно это можно выполнить тот же эффект, просто изменив координаты, которые вы используете в функции рисования. При отображении контура, тем не менее, все координаты инкапсулированы в пути, поэтому гораздо проще применения преобразования переноса необходимо сдвинуть весь путь.

Преобразование удобно использовать для анимации, а также для простых текстовых эффектов:

![](translate-images/translateexample.png "Text shadow, engraving, and embossing with translation")

Метод [`Translate`](xref:SkiaSharp.SKCanvas.Translate(System.Single,System.Single)) в `SKCanvas` имеет два параметра, которые приводят к тому, что графические объекты будут перемещаться по горизонтали и по вертикали:

```csharp
public void Translate (Single dx, Single dy)
```

Эти аргументы может быть отрицательным. Второй метод [`Translate`](xref:SkiaSharp.SKCanvas.Translate(SkiaSharp.SKPoint)) объединяет два значения перевода в одно `SKPoint` значение:

```csharp
public void Translate (SKPoint point)
```

На странице **накопленный перевод** примера программы [**скиашарпформс**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) показано, что несколько вызовов метода `Translate` являются кумулятивными. Класс [`AccumulatedTranslatePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AccumulatedTranslatePage.cs) отображает 20 версий одного и того же прямоугольника, каждое смещение от предыдущего прямоугольника достаточно просто для растяжения по диагонали. Обработчик событий `PaintSurface`:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint strokePaint = new SKPaint())
    {
        strokePaint.Color = SKColors.Black;
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.StrokeWidth = 3;

        int rectangleCount = 20;
        SKRect rect = new SKRect(0, 0, 250, 250);
        float xTranslate = (info.Width - rect.Width) / (rectangleCount - 1);
        float yTranslate = (info.Height - rect.Height) / (rectangleCount - 1);

        for (int i = 0; i < rectangleCount; i++)
        {
            canvas.DrawRect(rect, strokePaint);
            canvas.Translate(xTranslate, yTranslate);
        }
    }
}
```

Последовательные прямоугольников унаследованы от страницы:

[![](translate-images/accumulatedtranslate-small.png "Triple screenshot of the Accumulated Translate page")](translate-images/accumulatedtranslate-large.png#lightbox "Triple screenshot of the Accumulated Translate page")

Если накопленные коэффициенты перевода имеют `dx` и `dy`, а точка, указанная в функции рисования, имеет значение (`x`, `y`), то графический объект визуализируется в точке (`x'`, `y'`), где:

x' = x + dx

y "= y + dy

Они называются *формулами преобразования* для перевода. Значения `dx` и `dy` по умолчанию для нового `SKCanvas` равны 0.

Обычно используется преобразование «миграция» для эффектов тени и аналогичных методов, как показано на странице « **перевод текстовых эффектов** ». Ниже приведена соответствующая часть обработчика `PaintSurface` в классе [`TranslateTextEffectsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TranslateTextEffectsPage.cs) :

```csharp
float textSize = 150;

using (SKPaint textPaint = new SKPaint())
{
    textPaint.Style = SKPaintStyle.Fill;
    textPaint.TextSize = textSize;
    textPaint.FakeBoldText = true;

    float x = 10;
    float y = textSize;

    // Shadow
    canvas.Translate(10, 10);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("SHADOW", x, y, textPaint);
    canvas.Translate(-10, -10);
    textPaint.Color = SKColors.Pink;
    canvas.DrawText("SHADOW", x, y, textPaint);

    y += 2 * textSize;

    // Engrave
    canvas.Translate(-5, -5);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("ENGRAVE", x, y, textPaint);
    canvas.ResetMatrix();
    textPaint.Color = SKColors.White;
    canvas.DrawText("ENGRAVE", x, y, textPaint);

    y += 2 * textSize;

    // Emboss
    canvas.Save();
    canvas.Translate(5, 5);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("EMBOSS", x, y, textPaint);
    canvas.Restore();
    textPaint.Color = SKColors.White;
    canvas.DrawText("EMBOSS", x, y, textPaint);
}
```

В каждом из трех примеров вызывается `Translate` для отображения текста, который нужно сдвинуть, из расположения, заданного переменными `x` и `y`. Затем текст снова отображается другим цветом, без влияния перевода:

[![](translate-images/translatetexteffects-small.png "Triple screenshot of the Translate Text Effects page")](translate-images/translatetexteffects-large.png#lightbox "Triple screenshot of the Translate Text Effects page")

В каждом из трех примеров показан другой способ отрицания `Translate`ного вызова:

В первом примере просто вызывается `Translate`, но с отрицательными значениями. Так как вызовы `Translate` являются кумулятивными, эта последовательность вызовов просто восстанавливает Общий перевод до значений по умолчанию, равных нулю.

Во втором примере вызывается [`ResetMatrix`](xref:SkiaSharp.SKCanvas.ResetMatrix). В результате всех операций преобразования, чтобы вернуться к состоянию по умолчанию.

В третьем примере состояние объекта `SKCanvas` сохраняется с вызовом в [`Save`](xref:SkiaSharp.SKCanvas.Save) , а затем восстанавливается состояние с вызовом [`Restore`](xref:SkiaSharp.SKCanvas.Restore). Это наиболее универсальный способ управления преобразований для ряда операций рисования. Эти `Save` и `Restore` вызывают функцию, подобную стеку: можно вызвать `Save` несколько раз, а затем вызвать `Restore` в обратной последовательности, чтобы вернуться к предыдущим состояниям. Метод `Save` возвращает целое число, и вы можете передать это целое число в [`RestoreToCount`](xref:SkiaSharp.SKCanvas.RestoreToCount*) для эффективного вызова `Restore` несколько раз. Свойство [`SaveCount`](xref:SkiaSharp.SKCanvas.SaveCount) возвращает число состояний, сохраненных в стеке в данный момент.

Можно также использовать класс [`SKAutoCanvasRestore`](xref:SkiaSharp.SKAutoCanvasRestore) для восстановления состояния холста. Конструктор этого класса предназначен для вызова в операторе `using`; состояние холста автоматически восстанавливается в конце блока `using`.

Однако нет необходимости беспокоиться о преобразованиях, отсущих один вызов обработчика `PaintSurface` далее. Каждый новый вызов `PaintSurface` предоставляет свежий объект `SKCanvas` с преобразованиями по умолчанию.

Другим распространенным применением преобразования `Translate` является визуализация визуального объекта, который изначально был создан с помощью координат, удобных для рисования. Например может потребоваться указать координаты аналогично часам со стрелками с центром в точке (0, 0). Затем можно использовать преобразования для отображения часы где вам удобно. Этот метод показан на странице [**Хендекаграм Array**]. Класс [`HendecagramArrayPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramArrayPage.cs) начинается с создания объекта `SKPath` для оси, указывающей на 11 звезд. Объект `HendecagramPath` определен как открытый, статический и доступный только для чтения, поэтому доступ к нему можно получить из других демонстрационных программ. Она создается в статическом конструкторе:

```csharp
public class HendecagramArrayPage : ContentPage
{
    ...
    public static readonly SKPath HendecagramPath;

    static HendecagramArrayPage()
    {
        // Create 11-pointed star
        HendecagramPath = new SKPath();
        for (int i = 0; i < 11; i++)
        {
            double angle = 5 * i * 2 * Math.PI / 11;
            SKPoint pt = new SKPoint(100 * (float)Math.Sin(angle),
                                    -100 * (float)Math.Cos(angle));
            if (i == 0)
            {
                HendecagramPath.MoveTo(pt);
            }
            else
            {
                HendecagramPath.LineTo(pt);
            }
        }
        HendecagramPath.Close();
    }
}
```

Если центре звезды точки (0, 0), все точки, звезды, круг вокруг этой точки. Каждая точка представляет собой сочетание значения синуса и косинуса угла, увеличивающейся на 5/11ths 360 градусов. (Можно также создать 11-дюймовую звезду, увеличив угол на 2/11, 3/11 или 4/11 в кружке.) Радиус этого круга устанавливается как 100.

Если этот путь визуализируется без преобразований, центр будет расположен в левом верхнем углу `SKCanvas`, и будет отображаться только квартал. Обработчик `PaintSurface` `HendecagramPage` вместо этого использует `Translate` для мозаичного заполнения холста несколькими копиями звезды, каждый из которых имеет случайное цветное значение:

```csharp
public class HendecagramArrayPage : ContentPage
{
    Random random = new Random();
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            for (int x = 100; x < info.Width + 100; x += 200)
                for (int y = 100; y < info.Height + 100; y += 200)
                {
                    // Set random color
                    byte[] bytes = new byte[3];
                    random.NextBytes(bytes);
                    paint.Color = new SKColor(bytes[0], bytes[1], bytes[2]);

                    // Display the hendecagram
                    canvas.Save();
                    canvas.Translate(x, y);
                    canvas.DrawPath(HendecagramPath, paint);
                    canvas.Restore();
                }
        }
    }
}

```

Ниже приведен результат:

[![](translate-images/hendecagramarray-small.png "Triple screenshot of the Hendecagram Array page")](translate-images/hendecagramarray-large.png#lightbox "Triple screenshot of the Hendecagram Array page")

Анимации часто включают в себя преобразований. Страница **анимации хендекаграм** перемещает звезду, указывающую на 11, вокруг круга. Класс [`HendecagramAnimationPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramAnimationPage.cs) начинается с некоторых полей и переопределений методов `OnAppearing` и `OnDisappearing` для запуска и завершения таймера Xamarin. Forms:

```csharp
public class HendecagramAnimationPage : ContentPage
{
    const double cycleTime = 5000;      // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float angle;

    public HendecagramAnimationPage()
    {
        Title = "Hedecagram Animation";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;
        stopwatch.Start();

        Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
        {
            double t = stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime;
            angle = (float)(360 * t);
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

Поле `angle` анимируется от 0 градусов до 360 градусов каждые 5 секунд. Обработчик `PaintSurface` использует свойство `angle` двумя способами: для указания оттенок цвета в методе `SKColor.FromHsl`, а также в качестве аргумента для методов `Math.Sin` и `Math.Cos`, управляющих расположением звезды:

```csharp
public class HendecagramAnimationPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.Translate(info.Width / 2, info.Height / 2);
        float radius = (float)Math.Min(info.Width, info.Height) / 2 - 100;

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Fill;
            paint.Color = SKColor.FromHsl(angle, 100, 50);

            float x = radius * (float)Math.Sin(Math.PI * angle / 180);
            float y = -radius * (float)Math.Cos(Math.PI * angle / 180);
            canvas.Translate(x, y);
            canvas.DrawPath(HendecagramPage.HendecagramPath, paint);
        }
    }
}
```

Обработчик `PaintSurface` дважды вызывает метод `Translate`, сначала переводит его в центр холста, а затем преобразуется к окружности окружности вокруг (0, 0). Радиус круга задано быть как можно большего размера, сохраняя при этом типа "звезда" в пределах страницы:

[![](translate-images/hendecagramanimation-small.png "Triple screenshot of the Hendecagram Animation page")](translate-images/hendecagramanimation-large.png#lightbox "Triple screenshot of the Hendecagram Animation page")

Обратите внимание на то, что типа "звезда" сохраняет ориентацию же, как он вращается вокруг центра страницы. Объект не поворачивается вообще. Это задание для преобразования вращения.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
