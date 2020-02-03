---
title: Точки и тире в SkiaSharp
description: В этой статье описано, как к Освойте тонкости механизмов рисования в SkiaSharp пунктирная и пунктирные линии и демонстрирует это с помощью примера кода.
ms.prod: xamarin
ms.assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 229f60cbb96058454a1c634e53a7bb00ec725bcf
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723758"
---
# <a name="dots-and-dashes-in-skiasharp"></a>Точки и тире в SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Основные сложности рисования пунктирных и пунктирных линий в SkiaSharp_

SkiaSharp позволяет рисовать линии, которые не сплошной, но вместо этого состоят из точки и тире.

![](dots-images/dottedlinesample.png "Dotted line")

Это делается с помощью *результата пути*, который является экземпляром класса [`SKPathEffect`](xref:SkiaSharp.SKPathEffect) , установленного для свойства [`PathEffect`](xref:SkiaSharp.SKPaint.PathEffect) `SKPaint`. Можно создать эффект пути (или объединить эффекты пути), используя один из статических методов создания, определенных `SKPathEffect`. (`SKPathEffect` является одним из шести эффектов, поддерживаемых SkiaSharp. другие описаны в разделе [**SkiaSharp Effect**](../effects/index.md).)

Для рисования пунктирных или пунктирных линий используется статический метод [`SKPathEffect.CreateDash`](xref:SkiaSharp.SKPathEffect.CreateDash(System.Single[],System.Single)) . Существует два аргумента: сначала это массив `float` значений, указывающий длину точек и штрихов, а также длину пробелов между ними. Этот массив должен иметь четное число элементов и должно быть по крайней мере два элемента. (В массиве могут быть нулевые элементы, но это приводит к сплошной линии.) Если имеется два элемента, первый — это длина точки или тире, а вторая — длину разрыва перед следующей точкой или тире. Если имеется более двух элементов, то они находятся в следующем порядке: тире длины, длина временного промежутка, тире, длина временного промежутка и т. д.

Как правило нужно будет сделать длинами тире и свойство gap кратно ширина штриха. Если ширина мазков 10 точек, например, массив {10, 10} будет нарисуйте пунктирной линией, точек и пробелов которых же длины, толщина штриха.

Однако параметр `StrokeCap` объекта `SKPaint` также влияет на эти точки и тире. Как вы вскоре увидите, что оказывает влияние на элементы исходного массива.

Пунктирные и пунктирные линии показаны на странице « **точки и тире** ». В файле [**дотсанддашеспаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/DotsAndDashesPage.xaml) создаются два представления `Picker`, одно для выбора наконечника обводки, а второй — для выбора массива тире:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Paths.DotsAndDashesPage"
             Title="Dots and Dashes">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKStrokeCap}">
                    <x:Static Member="skia:SKStrokeCap.Butt" />
                    <x:Static Member="skia:SKStrokeCap.Round" />
                    <x:Static Member="skia:SKStrokeCap.Square" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="dashArrayPicker"
                Title="Dash Array"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>10, 10</x:String>
                    <x:String>30, 10</x:String>
                    <x:String>10, 10, 30, 10</x:String>
                    <x:String>0, 20</x:String>
                    <x:String>20, 20</x:String>
                    <x:String>0, 20, 20, 20</x:String>
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skiaforms:SKCanvasView x:Name="canvasView"
                                PaintSurface="OnCanvasViewPaintSurface"
                                Grid.Row="1"
                                Grid.Column="0"
                                Grid.ColumnSpan="2" />
    </Grid>
</ContentPage>
```

 Первые три элемента в `dashArrayPicker` предполагают, что ширина штриха равна 10 пикселям. {10, 10} массив предназначен для пунктирной линией {30, 10} — пунктирная линия и {10, 10, 30, 10} для точки и штриховой линией. (Другие будут рассмотрены чуть позже.)

[`DotsAndDashesPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/DotsAndDashesPage.xaml.cs) файл кода программной части содержит обработчик событий `PaintSurface` и пару вспомогательных подпрограмм для доступа к представлениям `Picker`:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = (SKStrokeCap)strokeCapPicker.SelectedItem,
        PathEffect = SKPathEffect.CreateDash(GetPickerArray(dashArrayPicker), 20)
    };

    SKPath path = new SKPath();
    path.MoveTo(0.2f * info.Width, 0.2f * info.Height);
    path.LineTo(0.8f * info.Width, 0.8f * info.Height);
    path.LineTo(0.2f * info.Width, 0.8f * info.Height);
    path.LineTo(0.8f * info.Width, 0.2f * info.Height);

    canvas.DrawPath(path, paint);
}

float[] GetPickerArray(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return new float[0];
    }

    string str = (string)picker.SelectedItem;
    string[] strs = str.Split(new char[] { ' ', ',' }, StringSplitOptions.RemoveEmptyEntries);
    float[] array = new float[strs.Length];

    for (int i = 0; i < strs.Length; i++)
    {
        array[i] = Convert.ToSingle(strs[i]);
    }
    return array;
}
```

На следующих снимках экрана на экране iOS слева показано пунктирной линией.

[![](dots-images/dotsanddashes-small.png "Triple screenshot of the Dots and Dashes page")](dots-images/dotsanddashes-large.png#lightbox "Triple screenshot of the Dots and Dashes page")

Тем не менее экран Android также должна показать пунктирной линией, с помощью массива {10, 10}, но вместо этого строки является сплошная заливка. Что произошло? Проблема заключается в том, что на экране Android также задан параметр штриха "`Square`". При этом расширяется все дефисы на половину толщины штриха, они заполняют пробелы.

Чтобы обойти эту проблему при использовании наконечника рукописного ввода `Square` или `Round`, необходимо уменьшить размер штриха в массиве на длину штриха (иногда это приводит к нулевой длине штриха) и увеличить длину зазора на величину штриха. Вот как вычисляются последние три массива штрихов в `Picker` в файле XAML:

- {10, 10} становится {0, 20} пунктирная линия
- {30, 10} становится {20, 20} для пунктирная линия
- {10, 10, 30, 10} становится {0, 20, 20, 20} для пунктирная и пунктирные линии

На экране UWP отображается пунктирная и пунктирная линии для наконечника штриха `Round`. `Round`ная линия обводки часто обеспечивает лучший вид точек и штрихов в толстых линиях.

До сих пор нет упоминания о втором параметре метода `SKPathEffect.CreateDash`. Этот параметр называется `phase` и ссылается на смещение в шаблоне с точкой и тире для начала строки. Например, если массив тире имеет тип {10, 10}, а `phase` равен 10, то строка начинается с пробела, а не с точки.

Одно интересное приложение параметра `phase` находится в анимации. **Анимированная страница с анимацией** похожа на страницу **арчимедеана** , за исключением того, что класс [`AnimatedSpiralPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/AnimatedSpiralPage.cs) анимирует параметр `phase` с помощью метода `Device.Timer` Xamarin. Forms:

```csharp
public class AnimatedSpiralPage : ContentPage
{
    const double cycleTime = 250;       // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float dashPhase;

    public AnimatedSpiralPage()
    {
        Title = "Animated Spiral";

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
            dashPhase = (float)(10 * t);
            canvasView.InvalidateSurface();

            if (!pageIsActive)
            {
                stopwatch.Stop();
            }

            return pageIsActive;
        });
    }
    ···  
}
```

Конечно вам придется фактически работающих с программой для анимации см. в разделе:

[![](dots-images/animatedspiral-small.png "Triple screenshot of the Animated Spiral page")](dots-images/animatedspiral-large.png#lightbox "Triple screenshot of the Animated Spiral page")

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
