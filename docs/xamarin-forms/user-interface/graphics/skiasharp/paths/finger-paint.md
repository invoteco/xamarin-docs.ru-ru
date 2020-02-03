---
title: Рисование пальцами в SkiaSharp
description: В этой статье объясняется, как использовать пальцы для рисования на холсте SkiaSharp в приложении Xamarin.Forms и демонстрирует это с помощью примера кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 56929D74-8F2C-44C6-90E6-3FBABCDC0A4B
author: davidbritch
ms.author: dabritch
ms.date: 04/05/2017
ms.openlocfilehash: d5cf0927c64732d6d0a44204db9509fae77f0d1d
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724077"
---
# <a name="finger-painting-in-skiasharp"></a>Рисование пальцами в SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Используйте пальцы для рисования на холсте._

Объект `SKPath` может быть постоянно обновлен и отображен. Эта функция позволяет путь используется для интерактивного рисунком, таких как в программе красочное.

![](finger-paint-images/fingerpaintsample.png "An exercise in finger painting")

Поддержка сенсорного ввода в Xamarin.Forms не позволяет отслеживания отдельных пальцев на экране, чтобы эффект Xamarin.Forms touch отслеживания был разработан для обеспечения поддержки дополнительных сенсорного ввода. Этот эффект описан в статье [**вызов событий из эффектов**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md). Демонстрационные видеоролики программы с примерами с [**поддержкой сенсорного экрана**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/) включают две страницы, использующие SkiaSharp, в том числе программу рисования пальца.

Решение [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) включает в себя это событие отслеживания касаний. Проект библиотеки .NET Standard включает класс `TouchEffect`, перечисление `TouchActionType`, делегат `TouchActionEventHandler` и класс `TouchActionEventArgs`. Каждый проект платформы включает класс `TouchEffect` для этой платформы; проект iOS также содержит класс `TouchRecognizer`.

Страница **прорисовки пальца** в **скиашарпформсдемос** — это упрощенная реализация рисования пальца. Не разрешить выбор цвет и ширина штриха, он никак не может очистить холст и Конечно не удается сохранить иллюстрации.

Файл [**финжерпаинтпаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/FingerPaintPage.xaml) помещает `SKCanvasView` в `Grid` одной ячейки и присоединяет `TouchEffect` к этому `Grid`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Paths.FingerPaintPage"
             Title="Finger Paint">

    <Grid BackgroundColor="White">
        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction" />
        </Grid.Effects>
    </Grid>
</ContentPage>
```

Присоединение `TouchEffect` непосредственно к `SKCanvasView` не работает на всех платформах.

Файл кода программной части [**FingerPaintPage.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/FingerPaintPage.xaml.cs) определяет две коллекции для хранения `SKPath` объектов, а также объект `SKPaint` для отрисовки этих путей:

```csharp
public partial class FingerPaintPage : ContentPage
{
    Dictionary<long, SKPath> inProgressPaths = new Dictionary<long, SKPath>();
    List<SKPath> completedPaths = new List<SKPath>();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };

    public FingerPaintPage()
    {
        InitializeComponent();
    }
    ...
}
```

Как видно из названия, в словаре `inProgressPaths` хранятся пути, которые в данный момент рисуются одним или несколькими пальцами. Ключ словаря — идентификатор сенсорного ввода, прилагаемый к события касания. Поле `completedPaths` представляет собой коллекцию путей, которые были завершены, когда палец, рисующий контур, ликвидируется с экрана.

Обработчик `TouchAction` управляет этими двумя коллекциями. Когда палец впервые касается экрана, в `inProgressPaths`добавляется новый `SKPath`. При перемещении пальца, дополнительные точки добавляются к пути. При отпускании пальца путь передается в коллекцию `completedPaths`. Можно рисовать с помощью нескольких пальцев одновременно. После каждого изменения в одном из путей или коллекций `SKCanvasView` становится недействительным.

```csharp
public partial class FingerPaintPage : ContentPage
{
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = new SKPath();
                    path.MoveTo(ConvertToPixel(args.Location));
                    inProgressPaths.Add(args.Id, path);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Moved:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = inProgressPaths[args.Id];
                    path.LineTo(ConvertToPixel(args.Location));
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    completedPaths.Add(inProgressPaths[args.Id]);
                    inProgressPaths.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Cancelled:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    inProgressPaths.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
    SKPoint ConvertToPixel(Point pt)
    {
        return new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                           (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));
    }
}
```

Точки, сопровождающие события отслеживания сенсорного ввода являются координаты Xamarin.Forms; они должны преобразовываться в SkiaSharp координаты, которые являются пиксели. Это назначение метода `ConvertToPixel`.

Затем обработчик `PaintSurface` просто визуализирует обе коллекции путей. Ранее завершенного пути отображаются под путей в ход выполнения:

```csharp
public partial class FingerPaintPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKCanvas canvas = args.Surface.Canvas;
        canvas.Clear();

        foreach (SKPath path in completedPaths)
        {
            canvas.DrawPath(path, paint);
        }

        foreach (SKPath path in inProgressPaths.Values)
        {
            canvas.DrawPath(path, paint);
        }
    }
    ...
}
```

Ваш картин палец ограничиваются только свои таланты:

[![](finger-paint-images/fingerpaint-small.png "Triple screenshot of the Finger Paint page")](finger-paint-images/fingerpaint-large.png#lightbox "Triple screenshot of the Finger Paint page")

Теперь вы узнали, как для рисования линий и кривых, с помощью параметрические уравнения определения. В следующем разделе [**SkiaSharp кривых и paths**](../curves/index.md) рассматриваются различные типы кривых, которые `SKPath` поддерживаются. Но полезным условием является исследование [**преобразований SkiaSharp**](../transforms/index.md).

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [Демонстрации эффектов для отслеживания касаний (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/)
- [Вызов событий из эффектов](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
