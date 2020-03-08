---
title: Режимы наложения Duff Портер
description: Используйте режимы Портер Duff blend для создания сцены на основе образов источника и назначения.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 57F172F8-BA03-43EC-A215-ED6B78696BB5
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: c15dd4606a75cc3cdffbad71f15299568157213a
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/07/2020
ms.locfileid: "78915829"
---
# <a name="porter-duff-blend-modes"></a>Режимы наложения Duff Портер

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Режимы наложения Портер Duff названы Томас Портер и Tom Duff, являющейся разработчиком алгебры компоновки, работая в Lucasfilm. [_Цифровые изображения_](https://graphics.pixar.com/library/Compositing/paper.pdf) , составленные на бумаге, были опубликованы в июле 1984 _, а также_на страницах 253 – 259. Эти режимы смешения крайне важны для композиции, в которой сборка различных изображений составного сцены:

![Портер — пример Дуфф](porter-duff-images/PorterDuffSample.png "Портер — пример Дуфф")

## <a name="porter-duff-concepts"></a>Основные понятия Duff Портер

Предположим, что прямоугольник коричневом занимает левом и верхнем две трети поверхность отображения:

![Портер — назначение Дуфф](porter-duff-images/PorterDuffDst.png "Портер — назначение Дуфф")

Эта область называется _назначением_ или иногда _фоном_ или _заднем_.

Требуется нарисовать следующие прямоугольник, который соответствует размеру массива назначения. Прямоугольник является прозрачным за исключением голубовато область, которая занимает вправо и вниз на две трети:

![Портер — источник Дуфф](porter-duff-images/PorterDuffSrc.png "Портер — источник Дуфф")

Это называется _источником_ или иногда _передним планом_.

При отображении источник на сервере назначения, вот что нужно:

![Портер — источник Дуфф](porter-duff-images/PorterDuffSrcOver.png "Портер — источник Дуфф")

Прозрачных точек источника разрешить фона видны сквозь, хотя голубовато исходные точки скрывать фона. Это обычная ситуация, и она упоминается в SkiaSharp как `SKBlendMode.SrcOver`. Это значение является значением по умолчанию свойства `BlendMode` при первом создании объекта `SKPaint`.

Тем не менее можно указать режим разных наложения для другой эффект. Если указать `SKBlendMode.DstOver`, то в области пересечения источника и назначения вместо источника появляется место назначения.

![Портер-Дуфф назначение](porter-duff-images/PorterDuffDstOver.png "Портер-Дуфф назначение")

В режиме `SKBlendMode.DstIn` Blend отображается только область, в которой назначение и источник пересекается с цветом назначения.

![Портер — назначение Дуфф в](porter-duff-images/PorterDuffDstIn.png "Портер — назначение Дуфф в")

Режим смешения `SKBlendMode.Xor` (исключающее или) приводит к тому, что ничего не отображается в месте пересечения двух областей:

![Портер-Дуфф Exclusive или](porter-duff-images/PorterDuffXor.png "Портер-Дуфф Exclusive или")

Цветных прямоугольников источника и назначения эффективно разделить поверхность отображения четыре уникальных областей, которые можно окрасить различными способами, соответствующий наличие прямоугольников источника и назначения:

![Портер — Дуфф](porter-duff-images/PorterDuff.png "Duff Портер")

Прямоугольники верхний правый и левый нижний всегда будут пустыми, так как в исходном и целевом прозрачны в этих областях. Конечный цвет занимает область верхнего левого, таким образом, чтобы область можно окрасить, либо с цветом назначения или вообще не. Аналогичным образом исходного цвета занимает область нижний правый, таким образом, чтобы область будут выделены цветом с цветом источника или вообще не. Пересечение источника в середине и назначения будут выделены цветом с цветом назначения, цвет источника или вообще не.

Общее число комбинаций — 2 (для левом верхнем углу), время 2 (внизу справа) раз 3 (для центра) или 12. Ниже приведены 12 основных режима Портер Duff композиции.

В конце _компоновки цифровых изображений_ (стр. 256) Портер и Дуфф добавляют более 13-й режим с названием _плюс_ (соответствующий элементу SkiaSharp `SKBlendMode.Plus` и режиму W3C, который не следует путать _с режимом_ " _осветлить_ W3C".) Этот `Plus`ный режим добавляет цвета назначения и источника, процесс, который будет описан более подробно в скором времени.

СКИА добавляет режим 14 с именем `Modulate`, который очень похож на `Plus` за исключением того, что целевой цвет и цвета источника умножаются. Он может рассматриваться как дополнительный режим наложения Duff Портер.

Ниже приведены режимы 14 Duff Портер, как определено в SkiaSharp. В таблице показано, как они цвет каждого из трех областей непустой на схеме выше:

| Режим       | Назначение | Пересечение | Источник |
| ---------- |:-----------:|:------------:|:------:|
| `Clear`    |             |              |        |
| `Src`      |             | Источник       | X      |
| `Dst`      | X           | Назначение  |        |
| `SrcOver`  | X           | Источник       | X      |
| `DstOver`  | X           | Назначение  | X      |
| `SrcIn`    |             | Источник       |        |
| `DstIn`    |             | Назначение  |        |
| `SrcOut`   |             |              | X      |
| `DstOut`   | X           |              |        |
| `SrcATop`  | X           | Источник       |        |
| `DstATop`  |             | Назначение  | X      |
| `Xor`      | X           |              | X      |
| `Plus`     | X           | SUM          | X      |
| `Modulate` |             | Продукт      |        | 

Эти режимы смешения являются симметричными. Может быть передано источник и назначение и все режимы по-прежнему доступны.

Соглашение об именовании из режимов следует за несколько простых правил:

- **Src** или **DST** само по себе означает, что видимы только исходные или конечные Пиксели.
- Суффикс **over** указывает, что отображается в пересечениех. Исходных или целевых рисуется «по».
- Суффикс **in** означает, что окрашено только пересечение. Выходные данные ограничено только часть источнике или назначении «в» другой.
- Суффикс **out** означает, что пересечение не окрашено. Выводится только часть источника или назначения, «out» пересечения.
- Суффикс **поверх** — это объединение **в** и из **.** Он включает область, где источник или назначение — это «поверх» другого.

Обратите внимание на разницу с режимами `Plus` и `Modulate`. Эти режимы выполнении пиксели исходного и целевого другой тип вычисления. Они описаны более подробно чуть ниже.

На странице **сетки Портер-Дуфф** показаны все 14 режимов на одном экране в виде сетки. Каждый режим является отдельным экземпляром `SKCanvasView`. По этой причине класс является производным от `SKCanvasView` с именем `PorterDuffCanvasView`. Статический конструктор создает два точечные рисунки такого же размера, один коричневом прямоугольник, в его левом верхнем области, а другой голубовато прямоугольником.

```csharp
class PorterDuffCanvasView : SKCanvasView
{
    static SKBitmap srcBitmap, dstBitmap;

    static PorterDuffCanvasView()
    {
        dstBitmap = new SKBitmap(300, 300);
        srcBitmap = new SKBitmap(300, 300);

        using (SKPaint paint = new SKPaint())
        {
            using (SKCanvas canvas = new SKCanvas(dstBitmap))
            {
                canvas.Clear();
                paint.Color = new SKColor(0xC0, 0x80, 0x00);
                canvas.DrawRect(new SKRect(0, 0, 200, 200), paint);
            }
            using (SKCanvas canvas = new SKCanvas(srcBitmap))
            {
                canvas.Clear();
                paint.Color = new SKColor(0x00, 0x80, 0xC0);
                canvas.DrawRect(new SKRect(100, 100, 300, 300), paint);
            }
        }
    }
    ···
}
```

Конструктор экземпляра имеет параметр типа `SKBlendMode`. Она сохраняет этот параметр в поле. 

```csharp
class PorterDuffCanvasView : SKCanvasView
{
    ···
    SKBlendMode blendMode;

    public PorterDuffCanvasView(SKBlendMode blendMode)
    {
        this.blendMode = blendMode;
    }

    protected override void OnPaintSurface(SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find largest square that fits
        float rectSize = Math.Min(info.Width, info.Height);
        float x = (info.Width - rectSize) / 2;
        float y = (info.Height - rectSize) / 2;
        SKRect rect = new SKRect(x, y, x + rectSize, y + rectSize);

        // Draw destination bitmap
        canvas.DrawBitmap(dstBitmap, rect);

        // Draw source bitmap
        using (SKPaint paint = new SKPaint())
        {
            paint.BlendMode = blendMode;
            canvas.DrawBitmap(srcBitmap, rect, paint);
        }

        // Draw outline
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Black;
            paint.StrokeWidth = 2;
            rect.Inflate(-1, -1);
            canvas.DrawRect(rect, paint);
        }
    }
}
```

Переопределение `OnPaintSurface` выводит два растровых изображения. Первый рисуется обычно:

```csharp
canvas.DrawBitmap(dstBitmap, rect);
```

Второй объект рисуется с помощью объекта `SKPaint`, в котором для свойства `BlendMode` было задано значение аргумента конструктора:

```csharp
using (SKPaint paint = new SKPaint())
{
    paint.BlendMode = blendMode;
    canvas.DrawBitmap(srcBitmap, rect, paint);
}
```

Оставшаяся часть `OnPaintSurface` переопределяет рисование прямоугольника вокруг точечного рисунка, чтобы показать их размеры.

Класс `PorterDuffGridPage` создает экземпляры четырнадцать `PorterDurffCanvasView`, по одному для каждого элемента массива `blendModes`. Порядок элементов `SKBlendModes` в массиве немного отличается от таблицы для позиционирования схожих режимов, смежных друг с другом. 14 экземпляров `PorterDuffCanvasView` организованы вместе с метками в `Grid`:

```csharp
public class PorterDuffGridPage : ContentPage
{
    public PorterDuffGridPage()
    {
        Title = "Porter-Duff Grid";

        SKBlendMode[] blendModes =
        {
            SKBlendMode.Src, SKBlendMode.Dst, SKBlendMode.SrcOver, SKBlendMode.DstOver,
            SKBlendMode.SrcIn, SKBlendMode.DstIn, SKBlendMode.SrcOut, SKBlendMode.DstOut,
            SKBlendMode.SrcATop, SKBlendMode.DstATop, SKBlendMode.Xor, SKBlendMode.Plus,
            SKBlendMode.Modulate, SKBlendMode.Clear
        };

        Grid grid = new Grid
        {
            Margin = new Thickness(5)
        };

        for (int row = 0; row < 4; row++)
        {
            grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
            grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Star });
        }

        for (int col = 0; col < 3; col++)
        {
            grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Star });
        }

        for (int i = 0; i < blendModes.Length; i++)
        {
            SKBlendMode blendMode = blendModes[i];
            int row = 2 * (i / 4);
            int col = i % 4;

            Label label = new Label
            {
                Text = blendMode.ToString(),
                HorizontalTextAlignment = TextAlignment.Center
            };
            Grid.SetRow(label, row);
            Grid.SetColumn(label, col);
            grid.Children.Add(label);

            PorterDuffCanvasView canvasView = new PorterDuffCanvasView(blendMode);

            Grid.SetRow(canvasView, row + 1);
            Grid.SetColumn(canvasView, col);
            grid.Children.Add(canvasView);
        }

        Content = grid;
    }
}
```

Ниже приведен результат:

[![Портер-Дуфф, сетка](porter-duff-images/PorterDuffGrid.png "Портер-Дуфф, сетка")](porter-duff-images/PorterDuffGrid-Large.png#lightbox)

Необходимо убедиться, что прозрачность крайне важны для правильной работы режимов смешения Портер Duff. Класс `PorterDuffCanvasView` содержит три вызова метода `Canvas.Clear`. Все из них используют метод без параметров, который задает все пиксели прозрачным:

```csharp
canvas.Clear();
```

Попробуйте изменении любого из этих вызовов, таким образом, пиксели непрозрачный белый.

```csharp
canvas.Clear(SKColors.White);
```

После этого изменения некоторые из режимов смешения будет казаться, что для работы, а другие нет. Если задать для фона исходного точечного рисунка белый цвет, то режим `SrcOver` не будет работать, так как в исходном точечном рисунке нет прозрачных пикселов, позволяющих отобразить назначение. Если задать фон конечного точечного рисунка или холста как белый, `DstOver` не будет работать, так как в месте назначения отсутствуют прозрачные пиксели.

Может существовать искушения для замены точечных рисунков на странице **сетки Портер-Дуфф** простыми вызовами `DrawRect`. Который будет работать для целевого прямоугольника, но не для исходного прямоугольника. Исходный прямоугольник должен охватывать не только области сине цветные. Исходный прямоугольник должен включать прозрачной области, соответствующий цветная область назначения. Только после этого они будут blend режимы работы.

## <a name="using-mattes-with-porter-duff"></a>Использование при задании с Duff Портер

На странице "композиция" в области **композиции** отображается пример классической задачи компоновки: необходимо собрать изображение из нескольких частей, включая точечный рисунок с фоновым рисунком, который необходимо устранить. Ниже приведено изображение **сеатедмонкэй. jpg** с проблемным фоном:

![Заданная обезьяна](porter-duff-images/SeatedMonkey.jpg "Заданная обезьяна")

При подготовке к композиции создается соответствующая _матовая_ рамка, которая представляет собой еще один черный рисунок, где изображение должно отображаться и прозрачно в противном случае. Этот файл называется **сеатедмонкэйматте. png** и относится к ресурсам в папке **Media** в примере [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) :

![Матовая обезьяна](porter-duff-images/SeatedMonkeyMatte.png "Матовая обезьяна")

Это _не_ слишком экспертное создание. Лучше всего подложки должна содержать частично прозрачных точек вокруг области черных пикселей, и этот матовой — нет.

XAML-файл для страницы "композиция" на **основе кирпича** создает экземпляр `SKCanvasView` и `Button`, который помогает пользователю создать окончательный образ.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.BrickWallCompositingPage"
             Title="Brick-Wall Compositing">

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Button Text="Show sitting monkey"
                HorizontalOptions="Center"
                Margin="0, 10"
                Clicked="OnButtonClicked" />

    </StackLayout>
</ContentPage>
```

Файл кода программной части загружает два необходимых точечных рисунка и обрабатывает событие `Clicked` `Button`. Для каждого `Button` щелкните `step` поле увеличивается, и для `Button`задается новое свойство `Text`. Когда `step` достигает 5, возвращается значение 0:

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    SKBitmap monkeyBitmap = BitmapExtensions.LoadBitmapResource(
        typeof(BrickWallCompositingPage), 
        "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    SKBitmap matteBitmap = BitmapExtensions.LoadBitmapResource(
        typeof(BrickWallCompositingPage), 
        "SkiaSharpFormsDemos.Media.SeatedMonkeyMatte.png");

    int step = 0;

    public BrickWallCompositingPage ()
    {
        InitializeComponent ();
    }

    void OnButtonClicked(object sender, EventArgs args)
    {
        Button btn = (Button)sender;
        step = (step + 1) % 5;

        switch (step)
        {
            case 0: btn.Text = "Show sitting monkey"; break;
            case 1: btn.Text = "Draw matte with DstIn"; break;
            case 2: btn.Text = "Draw sidewalk with DstOver"; break;
            case 3: btn.Text = "Draw brick wall with DstOver"; break;
            case 4: btn.Text = "Reset"; break;
        }

        canvasView.InvalidateSurface();
    }
    
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        ···
    }
}
```

При первом запуске программы ничего не отображается, кроме `Button`:

[![Шаг 0 при компоновке кирпича](porter-duff-images/BrickWallCompositing0.png "Шаг 0 при компоновке кирпича")](porter-duff-images/BrickWallCompositing0-Large.png#lightbox)

Нажатие `Button` однократно приводит к тому, что `step` увеличивается до 1, а обработчик `PaintSurface` теперь отображает **сеатедмонкэй. jpg**:

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        float x = (info.Width - monkeyBitmap.Width) / 2;
        float y = info.Height - monkeyBitmap.Height;

        // Draw monkey bitmap
        if (step >= 1)
        {
            canvas.DrawBitmap(monkeyBitmap, x, y);
        }
        ···
    }
}
```

Нет `SKPaint` объекта и, следовательно, режима смешения. Точечный рисунок отображается в нижней части экрана:

[![Действие 1 для компоновки кирпича](porter-duff-images/BrickWallCompositing1.png "Действие 1 для компоновки кирпича")](porter-duff-images/BrickWallCompositing1-Large.png#lightbox)

Снова нажмите `Button` и `step` шаг с шагом 2. Это важный шаг при отображении файла **сеатедмонкэйматте. png** :

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        // Draw matte to exclude monkey's surroundings
        if (step >= 2)
        {
            using (SKPaint paint = new SKPaint())
            {
                paint.BlendMode = SKBlendMode.DstIn;
                canvas.DrawBitmap(matteBitmap, x, y, paint);
            }
        }
        ···
    }
}
```

Режим наложения `SKBlendMode.DstIn`, что означает, что место назначения будет сохранено в областях, соответствующих непрозрачным областям источника. В оставшейся части прямоугольник назначения, соответствующего исходного растрового изображения становится прозрачной:

[![Этап 2 при компоновке кирпича](porter-duff-images/BrickWallCompositing2.png "Этап 2 при компоновке кирпича")](porter-duff-images/BrickWallCompositing2-Large.png#lightbox)

Фон был удален. 

Следующим шагом является рисование прямоугольника, похожий на тротуара, на тебя monkey. Внешний вид этого тротуара основан на сочетание двух шейдеры: шейдера сплошным цветом и шейдера шума Перлина:

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        const float sidewalkHeight = 80;
        SKRect rect = new SKRect(info.Rect.Left, info.Rect.Bottom - sidewalkHeight,
                                 info.Rect.Right, info.Rect.Bottom);

        // Draw gravel sidewalk for monkey to sit on
        if (step >= 3)
        {
            using (SKPaint paint = new SKPaint())
            {
                paint.Shader = SKShader.CreateCompose(
                                    SKShader.CreateColor(SKColors.SandyBrown),
                                    SKShader.CreatePerlinNoiseTurbulence(0.1f, 0.3f, 1, 9));

                paint.BlendMode = SKBlendMode.DstOver;
                canvas.DrawRect(rect, paint);
            }
        }
        ···
    }
}
```

Так как этот тротуара должен идти позади самой обезьяны, режим смешения `DstOver`. Назначение появляется, только когда фон будет прозрачным:

[![Шаг 3. Компоновка модуля-стена](porter-duff-images/BrickWallCompositing3.png "Шаг 3. Компоновка модуля-стена")](porter-duff-images/BrickWallCompositing3-Large.png#lightbox)

Последним шагом является добавление упираетесь в стену. Программа использует плитку точечного рисунка на стороне модуля, доступную в качестве статического свойства `BrickWallTile` в классе `AlgorithmicBrickWallPage`. Преобразование перевода добавляется к `SKShader.CreateBitmap` вызову для сдвига плиток, чтобы нижняя строка была полной плиткой:

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        // Draw bitmap tiled brick wall behind monkey
        if (step >= 4)
        {
            using (SKPaint paint = new SKPaint())
            {
                SKBitmap bitmap = AlgorithmicBrickWallPage.BrickWallTile;
                float yAdjust = (info.Height - sidewalkHeight) % bitmap.Height;

                paint.Shader = SKShader.CreateBitmap(bitmap,
                                                     SKShaderTileMode.Repeat,
                                                     SKShaderTileMode.Repeat,
                                                     SKMatrix.MakeTranslation(0, yAdjust));
                paint.BlendMode = SKBlendMode.DstOver;
                canvas.DrawRect(info.Rect, paint);
            }
        }
    }
}
```

Для удобства `DrawRect` вызов отображает этот шейдер на всем холсте, но режим `DstOver` ограничивает вывод только областью холста, которая все еще прозрачна:

[![Этап 4 при компоновке кирпича](porter-duff-images/BrickWallCompositing4.png "Этап 4 при компоновке кирпича")](porter-duff-images/BrickWallCompositing4-Large.png#lightbox)

Очевидно, что существуют другие способы создания этой сценой. Его можно построить и запуск в фоновом режиме и выполняется на переднем плане. Но использование режимов blend обеспечивает большую гибкость. В частности использование подложки позволяет фона растрового изображения должны быть исключены из составных сцены.

Как вы узнали из статьи, [вырезая с помощью путей и регионов](../../curves/clipping.md), класс `SKCanvas` определяет три типа обрезки, соответствующие методам [`ClipRect`](xref:SkiaSharp.SKCanvas.ClipRect*), [`ClipPath`](xref:SkiaSharp.SKCanvas.ClipPath*)и [`ClipRegion`](xref:SkiaSharp.SKCanvas.ClipRegion*) . Режимы наложения Портер Duff добавьте еще один тип обрезки, что позволяет ограничить изображения для все, что можно рисовать, включая точечные рисунки. Мэтт, используемый при **компоновке кирпичного модуля** , по сути определяет область обрезки.

## <a name="gradient-transparency-and-transitions"></a>Градиента прозрачности и переходы

Примеры Портер-Duff blend режимы, показанный ранее в этой статье все использовали образы, которые состоял из непрозрачными и прозрачных точек, но не частично прозрачных точек. Для этих пикселей, а также определения функций режим наложения. Следующая таблица является более формальным определением режимов смешения Портер-Дуфф, в которых используется нотация, найденная в [**ссылке СКИА скблендмоде**](https://skia.org/user/api/SkBlendMode_Reference). (Так как **ссылка скблендмоде** является ссылкой на СКИА C++ , используется синтаксис.)

По существу красного, зеленого, синего и альфа-компоненты каждого пикселя преобразуются из байтов в числа с плавающей запятой в диапазоне от 0 до 1. Для альфа-канала 0 — полностью прозрачное, а 1 — полностью непрозрачный

В следующей таблице используются следующие сокращения:

- **Da** — это альфа-канал назначения
- **Контроллер домена** является ЦЕЛЕВЫМ цветом RGB
- **SA** является исходным альфа-каналом
- **SC** — цвет исходного RGB

Цвета RGB предварительно умноженное альфа-значение. Например, если **SC** представляет чисто красный, а **SA** — 0X80, то цвет RGB — **(0x80, 0, 0)** . Если **SA** равно 0, то все компоненты RGB также равны нулю.

Результат отображается в квадратных скобках с альфа-каналом, а цвет RGB отделяется запятой: **[альфа, Color]** . Цвета Расчет выполняется отдельно для красного, зеленого и синего компонентов:

| Режим       | Операция |
| ---------- | --------- |
| `Clear`    | [0, 0]    |
| `Src`      | [Sa, Sc]  |
| `Dst`      | [Da, контроллер домена]  |
| `SrcOver`  | [Sa + Da· (1 — Sa), Sc + Dc· (1 — Sa) | 
| `DstOver`  | [Da + Sa· (1 — Да), Dc + Sc· (1 — Da) |
| `SrcIn`    | [Sa· DA Sc· DA] |
| `DstIn`    | [Da· SA, Dc· SA] |
| `SrcOut`   | [Sa· (1 — Да), Sc· (1 — Da)] |
| `DstOut`   | [Da· (1 — Sa), Dc· (1 — Sa)] |
| `SrcATop`  | [Da, Sc· DA + Dc· (1 — Sa)] |
| `DstATop`  | [Sa, Dc· SA + Sc· (1 — Da)] |
| `Xor`      | [Sa + Da — 2· SA· DA Sc· (1 — Da) + Dc· (1 — Sa)] |
| `Plus`     | [Sa + Da, Sc + Dc] |
| `Modulate` | [Sa· DA Sc· Контроллер домена] | 

Эти операции проще анализировать, если **Da** и **SA** имеют значение 0 или 1. Например, для режима `SrcOver` по умолчанию, если **SA** имеет значение 0, то **SC** также имеет значение 0, а результатом будет **[Da, DC]** , Alpha и Color назначения. Если **SA** имеет значение 1, то результатом будет **[SA, SC]** , альфа-канал и цвет, или **[1, SC]** .

Режимы `Plus` и `Modulate` немного отличаются от других в том, что новые цвета могут быть результатом сочетания источника и назначения. Режим `Plus` может интерпретироваться как с байт-компонентами, так и с компонентами с плавающей запятой. На странице **сетки Портер-Дуфф** , показанной ранее, целевой цвет — **(0xC0, 0x80, 0x00)** , а исходный цвет — **(0x00, 0x80, 0xC0)** . Каждая пара компонентов добавляется, но сумма присваивается в 0xFF. Результатом является цвет **(0xC0, 0xFF, 0xC0)** . Это показано на пересечении цвет.

В режиме `Modulate` значения RGB необходимо преобразовать в тип с плавающей запятой. Целевой цвет — **(0,75, 0,5, 0)** , а источник — **(0, 0,5, 0,75)** . Каждый из компонентов RGB умножается вместе, а результат равен **(0, 0,25, 0)** . Это цвет, который отображается на пересечении на странице **сетки Портер-Дуфф** для этого режима.

Страница **прозрачность Портер-Дуфф** позволяет исследовать, как режимы наложения Портер-Дуфф работают с графическими объектами, частично прозрачными. Файл XAML включает `Picker` с режимами Портер-Дуфф:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaviews="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.PorterDuffTransparencyPage"
             Title="Porter-Duff Transparency">

    <StackLayout>
        <skiaviews:SKCanvasView x:Name="canvasView"
                                VerticalOptions="FillAndExpand"
                                PaintSurface="OnCanvasViewPaintSurface" />

        <Picker x:Name="blendModePicker"
                Title="Blend Mode"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKBlendMode}">
                    <x:Static Member="skia:SKBlendMode.Clear" />
                    <x:Static Member="skia:SKBlendMode.Src" />
                    <x:Static Member="skia:SKBlendMode.Dst" />
                    <x:Static Member="skia:SKBlendMode.SrcOver" />
                    <x:Static Member="skia:SKBlendMode.DstOver" />
                    <x:Static Member="skia:SKBlendMode.SrcIn" />
                    <x:Static Member="skia:SKBlendMode.DstIn" />
                    <x:Static Member="skia:SKBlendMode.SrcOut" />
                    <x:Static Member="skia:SKBlendMode.DstOut" />
                    <x:Static Member="skia:SKBlendMode.SrcATop" />
                    <x:Static Member="skia:SKBlendMode.DstATop" />
                    <x:Static Member="skia:SKBlendMode.Xor" />
                    <x:Static Member="skia:SKBlendMode.Plus" />
                    <x:Static Member="skia:SKBlendMode.Modulate" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                3
            </Picker.SelectedIndex>
        </Picker>
    </StackLayout>
</ContentPage>
```

Файл с выделенным кодом заполняет два прямоугольника одного размера с помощью линейного градиента. Градиент назначения представляет собой из верхней прямо в нижнем левом углу. Коричневом в правом верхнем углу, но затем к центру начинается постепенное исчезновение прозрачным и прозрачно в левом нижнем углу. 

Исходного прямоугольника имеет градиент от левого верхнего угла до правого нижнего угла. Верхнего левого угла голубовато, но снова выцветает до прозрачным и прозрачно в правом нижнем углу. 

```csharp
public partial class PorterDuffTransparencyPage : ContentPage
{
    public PorterDuffTransparencyPage()
    {
        InitializeComponent();
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

        // Make square display rectangle smaller than canvas
        float size = 0.9f * Math.Min(info.Width, info.Height);
        float x = (info.Width - size) / 2;
        float y = (info.Height - size) / 2;
        SKRect rect = new SKRect(x, y, x + size, y + size);

        using (SKPaint paint = new SKPaint())
        {
            // Draw destination
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(rect.Right, rect.Top),
                                new SKPoint(rect.Left, rect.Bottom),
                                new SKColor[] { new SKColor(0xC0, 0x80, 0x00),
                                                new SKColor(0xC0, 0x80, 0x00, 0) },
                                new float[] { 0.4f, 0.6f },
                                SKShaderTileMode.Clamp);

            canvas.DrawRect(rect, paint);

            // Draw source
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(rect.Left, rect.Top),
                                new SKPoint(rect.Right, rect.Bottom),
                                new SKColor[] { new SKColor(0x00, 0x80, 0xC0), 
                                                new SKColor(0x00, 0x80, 0xC0, 0) },
                                new float[] { 0.4f, 0.6f },
                                SKShaderTileMode.Clamp);

            // Get the blend mode from the picker
            paint.BlendMode = blendModePicker.SelectedIndex == -1 ? 0 :
                                    (SKBlendMode)blendModePicker.SelectedItem;

            canvas.DrawRect(rect, paint);

            // Stroke surrounding rectangle
            paint.Shader = null;
            paint.BlendMode = SKBlendMode.SrcOver;
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Black;
            paint.StrokeWidth = 3;
            canvas.DrawRect(rect, paint);
        }
    }
}
```

В этой программе демонстрируется использование режимов смешения Портер Duff со графических объектов, отличных от растровых изображений. Тем не менее источник должен включать прозрачной области. Это обусловлено тем, в данном случае градиентные заливки прямоугольника, но часть градиента является прозрачным.

Ниже приведены три примера:

[![Прозрачность Портер-Дуфф](porter-duff-images/PorterDuffTransparency.png "Прозрачность Портер-Дуфф")](porter-duff-images/PorterDuffTransparency-Large.png#lightbox)

Конфигурация назначения и источника очень похожа на схемы, показанные на странице 255 исходной бумаги для создания [_цифровых изображений_](https://graphics.pixar.com/library/Compositing/paper.pdf) Портер-Дуфф, но на этой странице показано, что режимы наложения хорошо работают для областей частичной прозрачности.

Для некоторых различные эффекты можно использовать прозрачный градиентов. Одна из возможных вариантов — это маскировка, аналогичная методике, показанной в разделе [**Радиальные градиенты для маскирования**](../shaders/circular-gradients.md#radial-gradients-for-masking) **страницы круговые градиенты SkiaSharp**. Большая часть страницы **маски компоновки** аналогична предыдущей программе. Он загружает ресурса точечного рисунка и определяет прямоугольник, в котором для его отображения. Радиальный градиент будет создан на основе предварительно определенный центра и радиус:

```csharp
public class CompositingMaskPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
        typeof(CompositingMaskPage),
        "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

    static readonly SKPoint CENTER = new SKPoint(180, 300);
    static readonly float RADIUS = 120;

    public CompositingMaskPage ()
    {
        Title = "Compositing Mask";

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

        // Find rectangle to display bitmap
        float scale = Math.Min((float)info.Width / bitmap.Width,
                               (float)info.Height / bitmap.Height);

        SKRect rect = SKRect.Create(scale * bitmap.Width, scale * bitmap.Height);

        float x = (info.Width - rect.Width) / 2;
        float y = (info.Height - rect.Height) / 2;
        rect.Offset(x, y);

        // Display bitmap in rectangle
        canvas.DrawBitmap(bitmap, rect);

        // Adjust center and radius for scaled and offset bitmap
        SKPoint center = new SKPoint(scale * CENTER.X + x,
                                        scale * CENTER.Y + y);
        float radius = scale * RADIUS;

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateRadialGradient(
                                center,
                                radius,
                                new SKColor[] { SKColors.Black,
                                                SKColors.Transparent },
                                new float[] { 0.6f, 1 },
                                SKShaderTileMode.Clamp);

            paint.BlendMode = SKBlendMode.DstIn;

            // Display rectangle using that gradient and blend mode
            canvas.DrawRect(rect, paint);
        }

        canvas.DrawColor(SKColors.Pink, SKBlendMode.DstOver);
    }
}
```

С этой программой разница в том что начинается с черный цвет в центре градиента и заканчивается прозрачности. Он отображается на точечном рисунке с режимом смешения `DstIn`, который показывает назначение только в тех областях источника, которые не являются прозрачными.

После вызова `DrawRect` вся поверхность холста прозрачна, за исключением круга, определенного радиальным градиентом. Последний вызов:

```csharp
canvas.DrawColor(SKColors.Pink, SKBlendMode.DstOver);
```

Прозрачные области холста розовым:

[![Маска компоновки](porter-duff-images/CompositingMask.png "Маска компоновки")](porter-duff-images/CompositingMask-Large.png#lightbox)

Также можно Портер Duff режимы и частично прозрачный градиентов для переходов из одного образа на другой. Страница « **переходы градиента** » содержит `Slider` для указания уровня хода выполнения в переходе от 0 к 1 и `Picker` для выбора требуемого типа перехода:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.GradientTransitionsPage"
             Title="Gradient Transitions">
    
    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="progressSlider"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference progressSlider},
                              Path=Value,
                              StringFormat='Progress = {0:F2}'}"
               HorizontalTextAlignment="Center" />

        <Picker x:Name="transitionPicker" 
                Title="Transition" 
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged" />
        
    </StackLayout>
</ContentPage>
```

Файл с выделенным кодом загружает два ресурса точечного рисунка для демонстрации перехода. Это те же два изображения, которые используются на странице **устранения точечных рисунков** , приведенной выше в этой статье. Код также определяет перечисление с тремя элементами, соответствующими трем типам градиентов &mdash; линейный, радиальный и поворот. Эти значения загружаются в `Picker`:

```csharp
public partial class GradientTransitionsPage : ContentPage
{
    SKBitmap bitmap1 = BitmapExtensions.LoadBitmapResource(
        typeof(GradientTransitionsPage),
        "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    SKBitmap bitmap2 = BitmapExtensions.LoadBitmapResource(
        typeof(GradientTransitionsPage),
        "SkiaSharpFormsDemos.Media.FacePalm.jpg");

    enum TransitionMode
    {
        Linear,
        Radial,
        Sweep
    };

    public GradientTransitionsPage ()
    {
        InitializeComponent ();

        foreach (TransitionMode mode in Enum.GetValues(typeof(TransitionMode)))
        {
            transitionPicker.Items.Add(mode.ToString());
        }

        transitionPicker.SelectedIndex = 0;
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }
    ···
}
```

Файл кода программной части создает три объекта `SKPaint`. Объект `paint0` не использует режим смешения. Этот объект Paint используется для рисования прямоугольника с градиентом, который переходит от черного к прозрачному, как указано в массиве `colors`. Массив `positions` основан на положении `Slider`, но был скорректирован несколько. Если `Slider` имеет минимальное или максимальное значение, `progress` значения 0 или 1, а одно из двух растровых изображений должно быть полностью видимым. Для этих значений необходимо задать соответствующие массивы `positions`.

Если `progress` значение равно 0, массив `positions` содержит значения-0,1 и 0. Первое значение равно 0, это означает, что градиент представляет собой черный только с 0 и прозрачный настроит SkiaSharp в противном случае. Если `progress` равен 0,5, массив содержит значения 0,45 и 0,55. Градиента черный от 0 к 0,45, а затем переходит к прозрачности, а полностью прозрачный 0,55 1. Если `progress` равен 1, массив `positions` равен 1 и 1,1, что означает, что градиент имеет черный цвет от 0 до 1.

Массивы `colors` и `position` используются в трех методах `SKShader`, которые создают градиент. На основе выбора `Picker` создается только один из этих шейдеров: 

```csharp
public partial class GradientTransitionsPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Assume both bitmaps are square for display rectangle
        float size = Math.Min(info.Width, info.Height);
        SKRect rect = SKRect.Create(size, size);
        float x = (info.Width - size) / 2;
        float y = (info.Height - size) / 2;
        rect.Offset(x, y);

        using (SKPaint paint0 = new SKPaint())
        using (SKPaint paint1 = new SKPaint())
        using (SKPaint paint2 = new SKPaint())
        {
            SKColor[] colors = new SKColor[] { SKColors.Black,
                                               SKColors.Transparent };

            float progress = (float)progressSlider.Value;

            float[] positions = new float[]{ 1.1f * progress - 0.1f,
                                             1.1f * progress };

            switch ((TransitionMode)transitionPicker.SelectedIndex)
            {
                case TransitionMode.Linear:
                    paint0.Shader = SKShader.CreateLinearGradient(
                                        new SKPoint(rect.Left, 0),
                                        new SKPoint(rect.Right, 0),
                                        colors,
                                        positions,
                                        SKShaderTileMode.Clamp);
                    break;

                case TransitionMode.Radial:
                    paint0.Shader = SKShader.CreateRadialGradient(
                                        new SKPoint(rect.MidX, rect.MidY),
                                        (float)Math.Sqrt(Math.Pow(rect.Width / 2, 2) +
                                                         Math.Pow(rect.Height / 2, 2)),
                                        colors,
                                        positions,
                                        SKShaderTileMode.Clamp);
                    break;

                case TransitionMode.Sweep:
                    paint0.Shader = SKShader.CreateSweepGradient(
                                        new SKPoint(rect.MidX, rect.MidY),
                                        colors,
                                        positions);
                    break;
            }

            canvas.DrawRect(rect, paint0);

            paint1.BlendMode = SKBlendMode.SrcOut;
            canvas.DrawBitmap(bitmap1, rect, paint1);

            paint2.BlendMode = SKBlendMode.DstOver;
            canvas.DrawBitmap(bitmap2, rect, paint2);
        }
    }
}
```

Этот градиент отображается в прямоугольнике без режима наложения. После того, как этот вызов `DrawRect`, холст просто содержит градиент от черного к прозрачному. Количество черновых увеличений с более высоким значением `Slider`.

В последних четырех инструкциях обработчика `PaintSurface` отображаются два растровых изображения. Режим смешения `SrcOut` означает, что первое растровое изображение отображается только в прозрачных областях фона. Режим `DstOver` для второго точечного рисунка означает, что второй точечный рисунок отображается только в тех областях, где первое растровое изображение не отображается.

На следующих снимках экрана показано три типа различных переходов, каждый на отметке 50%:

[![Градиентные переходы](porter-duff-images/GradientTransitions.png "Градиентные переходы")](porter-duff-images/GradientTransitions-Large.png#lightbox)

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
