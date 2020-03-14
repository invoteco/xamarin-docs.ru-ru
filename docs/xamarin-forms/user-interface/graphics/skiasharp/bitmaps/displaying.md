---
title: Отображение точечных рисунков SkiaSharp
description: Узнайте, как отобразить SkiaSharp растровые изображения в пикселях, размеры и расширены для заполнения прямоугольника с сохранением пропорций.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8E074F8D-4715-4146-8CC0-FD7A8290EDE9
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
ms.openlocfilehash: 9955b68346c74435a3a141c69d02e1bec5856bd3
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305657"
---
# <a name="displaying-skiasharp-bitmaps"></a>Отображение точечных рисунков SkiaSharp

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Тема точечных рисунков SkiaSharp была представлена в статье **[основы битовой карты в SkiaSharp](../basics/bitmaps.md)** . В этой статье показали три способа загрузки точечные рисунки и три способа отображения растровых изображений. В этой статье рассматриваются методы загрузки растровых изображений и более подробное использование `DrawBitmap` методов `SKCanvas`.

![Отображение образца](displaying-images/DisplayingSample.png "Отображение образца")

Методы `DrawBitmapLattice` и `DrawBitmapNinePatch` обсуждаются в статье **[Сегментированное отображение растровых изображений SkiaSharp](segmented.md)** .

Примеры на этой странице относятся к приложению **[скиашарпформсдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** . На домашней странице этого приложения выберите **точечные рисунки SkiaSharp**, а затем перейдите к разделу **отображение растровых изображений** .

## <a name="loading-a-bitmap"></a>Загрузка растрового изображения

Растровое изображение, используемое приложением SkiaSharp, обычно берутся из одного из трех различных источников:

- Из Интернета
- Из ресурсов, внедренных в исполняемый файл
- В библиотеке фотографий пользователя

Можно также для приложения SkiaSharp, чтобы создать новое растровое изображение, а затем нарисовать на нем или алгоритмически задать биты растрового изображения. Эти методы обсуждаются в статьях **[Создание и рисование растровых изображений SkiaSharp](drawing.md)** и **[доступ к SkiaSharp растровых пикселов](pixel-bits.md)** .

В следующих трех примерах кода для загрузки точечного рисунка предполагается, что класс содержит поле типа `SKBitmap`:

```csharp
SKBitmap bitmap;
```

Как и в **[SkiaSharp](../basics/bitmaps.md)** , для загрузки точечного рисунка в Интернете лучше всего использовать класс [`HttpClient`](xref:System.Net.Http.HttpClient) . Один экземпляр класса может быть определена как поле:

```csharp
HttpClient httpClient = new HttpClient();
```

При использовании `HttpClient` с приложениями iOS и Android необходимо задать свойства проекта, как описано в документах по **[протоколу tls 1,2](~/cross-platform/app-fundamentals/transport-layer-security.md)** .

Код, использующий `HttpClient`, часто включает оператор `await`, поэтому он должен находиться в методе `async`:

```csharp
try
{
    using (Stream stream = await httpClient.GetStreamAsync("https:// ··· "))
    using (MemoryStream memStream = new MemoryStream())
    {
        await stream.CopyToAsync(memStream);
        memStream.Seek(0, SeekOrigin.Begin);

        bitmap = SKBitmap.Decode(stream);
        ···
    };
}
catch
{
    ···
}
```

Обратите внимание, что объект `Stream`, полученный из `GetStreamAsync`, копируется в `MemoryStream`. Android не позволяет обрабатывать `Stream` `HttpClient` основным потоком, за исключением асинхронных методов. 

[`SKBitmap.Decode`](xref:SkiaSharp.SKBitmap.Decode(System.IO.Stream)) выполняет массу работы: объект `Stream`, переданный в него, ссылается на блок памяти, содержащий все точечные рисунки в одном из стандартных форматов файлов точечных рисунков, обычно JPEG, PNG или GIF. Метод `Decode` должен определить формат, а затем декодировать файл растрового изображения в собственный внутренний точечный формат SkiaSharp.

После того как код вызовет `SKBitmap.Decode`, он, вероятно, сделает недействительным `CanvasView`, чтобы обработчик `PaintSurface` мог отобразить только что загруженный точечный рисунок.

Второй способ загрузки точечного рисунка, включив точечного рисунка в качестве внедренного ресурса в библиотеке .NET Standard ссылаются проекты для отдельных платформ. Идентификатор ресурса передается методу [`GetManifestResourceStream`](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String)) . Этот идентификатор ресурса состоит из имени сборки, имя папки и имя файла ресурсов, разделенных точками.

```csharp
string resourceID = "assemblyName.folderName.fileName";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
{
    bitmap = SKBitmap.Decode(stream);
    ···
}
```

Файлы растровых изображений также могут храниться как ресурсы в проект отдельные платформы iOS, Android и универсальной платформы Windows (UWP). Однако загрузка этих маленьких точечных изображений требуется код, который находится в проекте платформы.

Третий подход для получения растрового изображения является из библиотеки рисунков пользователя. В следующем коде используется служба зависимостей, включенная в приложение **[скиашарпформсдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** . Библиотека .NET Standard **скиашарпформсдемо** включает интерфейс `IPhotoLibrary`, а каждый проект платформы содержит класс `PhotoLibrary`, реализующий этот интерфейс.

```csharp
IPhotoicturePicker picturePicker = DependencyService.Get<IPhotoLibrary>();

using (Stream stream = await picturePicker.GetImageStreamAsync())
{
    if (stream != null)
    {
        bitmap = SKBitmap.Decode(stream);
        ···
    }
}
```

Как правило, такой код также делает недействительным `CanvasView`, чтобы обработчик `PaintSurface` мог отобразить новое растровое изображение.

Класс `SKBitmap` определяет несколько полезных свойств, включая [`Width`](xref:SkiaSharp.SKBitmap.Width) и [`Height`](xref:SkiaSharp.SKBitmap.Height), которые раскрывают размеры точечного рисунка в пикселях, а также многие методы, включая методы для создания точечных рисунков, их копирования и для представления битов пикселов. 

## <a name="displaying-in-pixel-dimensions"></a>Отображение в пикселах

Класс [`Canvas`](xref:SkiaSharp.SKCanvas) SkiaSharp определяет четыре метода `DrawBitmap`. Эти методы позволяют растровые изображения для отображения двух фундаментально различными способами: 

- Указание `SKPoint` значения (или отдельных `x` и `y`) отображает точечный рисунок в его размерах в пикселях. Пиксели точечного рисунка непосредственно сопоставляются пикселей экрана.
- Задание прямоугольник приводит растровое изображение растягиваться по размер и форму элемента управления rectangle. 

Вы отображаете точечный рисунок в его размерах в пикселах, используя [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKPoint,SkiaSharp.SKPaint)) с параметром `SKPoint` или [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,System.Single,System.Single,SkiaSharp.SKPaint)) с отдельными параметрами `x` и `y`:

```csharp
DrawBitmap(SKBitmap bitmap, SKPoint pt, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, float x, float y, SKPaint paint = null)
```

Эти два метода функционально идентичны. Указанная точка указывает на расположение верхнего левого угла растрового изображения относительно холста. Так как на разрешение мобильных устройств, настолько велико, меньшего размера растровых изображений обычно отображаются очень маленькими на этих устройствах.

Необязательный параметр `SKPaint` позволяет отображать растровое изображение с помощью прозрачности. Для этого создайте объект `SKPaint` и задайте для свойства `Color` любое `SKColor` значение с альфа-каналом меньше 1. Пример:

```csharp
paint.Color = new SKColor(0, 0, 0, 0x80);
```

0x80, переданный в качестве последнего аргумента указывает 50% прозрачности. Можно также задать альфа-канал на один из предварительно определенных цветов:

```csharp
paint.Color = SKColors.Red.WithAlpha(0x80);
```

Тем не менее цвет, сам не имеет значения. Только альфа-канал проверяется при использовании объекта `SKPaint` в вызове `DrawBitmap`.

Объект `SKPaint` также играет роль при отображении растровых изображений с помощью режимов смешения или фильтров. Они показаны в статьях [SkiaSharp компоновки и режимы смешения](../effects/blend-modes/index.md) и [Фильтры изображений SkiaSharp](../effects/image-filters.md).

На странице **измерения в пикселах** образца программы **[скиашарпформсдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** отображается ресурс точечного рисунка размером 320 пикселей в ширину на 240 пикселей в высоком уровне:

```csharp
public class PixelDimensionsPage : ContentPage
{
    SKBitmap bitmap;

    public PixelDimensionsPage()
    {
        Title = "Pixel Dimensions";

        // Load the bitmap from a resource
        string resourceID = "SkiaSharpFormsDemos.Media.Banana.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }

        // Create the SKCanvasView and set the PaintSurface handler
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

        float x = (info.Width - bitmap.Width) / 2;
        float y = (info.Height - bitmap.Height) / 2;

        canvas.DrawBitmap(bitmap, x, y);
    }
}
```

Обработчик `PaintSurface` выравнивает точечный рисунок по центру, вычисляя `x` и `y` значения на основе размеров пикселов отображаемой области и размеров точечного рисунка в пикселях.

[![Размеры в пикселях](displaying-images/PixelDimensions.png "Размеры в пикселях")](displaying-images/PixelDimensions-Large.png#lightbox)

Если необходимо отобразить растровое изображение в его левом верхнем углу приложения, он бы передал просто координаты (0, 0). 

## <a name="a-method-for-loading-resource-bitmaps"></a>Метод для загрузки ресурсов растровых изображений

Многие из примеров, дальше необходимо загрузить ресурсы растрового изображения. Статический класс `BitmapExtensions` в решении **[скиашарпформсдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** содержит метод, помогающий:

```csharp
static class BitmapExtensions
{
    public static SKBitmap LoadBitmapResource(Type type, string resourceID)
    {
        Assembly assembly = type.GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            return SKBitmap.Decode(stream);
        }
    }
    ···
}
```

Обратите внимание на параметр `Type`. Это может быть `Type` объект, связанный с любым типом в сборке, в которой хранится ресурс Bitmap.

Этот метод `LoadBitmapResource` будет использоваться во всех последующих примерах, требующих ресурсов точечного рисунка.

## <a name="stretching-to-fill-a-rectangle"></a>Растяжение для заливки прямоугольника

Класс `SKCanvas` также определяет метод [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKPaint)) , который визуализирует растровое изображение в прямоугольник, а другой метод [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKRect,SkiaSharp.SKPaint)) , который визуализирует прямоугольное подмножество точечного рисунка с прямоугольником:

```
DrawBitmap(SKBitmap bitmap, SKRect dest, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, SKRect source, SKRect dest, SKPaint paint = null)
```

В обоих случаях точечный рисунок растягивается для заполнения прямоугольника с именем `dest`. Во втором методе `source` прямоугольник позволяет выбрать подмножество точечного рисунка. `dest` прямоугольник задается относительно выходного устройства; прямоугольник `source` задается относительно точечного рисунка.

На странице « **прямоугольник заливки** » показано первое из этих двух методов, отображая тот же рисунок, который использовался в предыдущем примере в прямоугольнике того же размера, что и холст: 

```csharp
public class FillRectanglePage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(FillRectanglePage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public FillRectanglePage ()
    {
        Title = "Fill Rectangle";

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

        canvas.DrawBitmap(bitmap, info.Rect);
    }
}
```

Обратите внимание на использование нового метода `BitmapExtensions.LoadBitmapResource` для задания поля `SKBitmap`. Прямоугольник назначения получается из свойства [`Rect`](xref:SkiaSharp.SKImageInfo.Rect) `SKImageInfo`, которое описывает размер отображаемой поверхности:

[![Заливка прямоугольника](displaying-images/FillRectangle.png "Заливка прямоугольника")](displaying-images/FillRectangle-Large.png#lightbox)

Обычно это _не_ то, что нужно. Изображение искажается растягивается по-разному в горизонтальном и вертикальном направлениях. При отображении растрового изображения в отличные от его размер в пикселях, обычно нужно сохранить пропорции исходного растрового изображения.

## <a name="stretching-while-preserving-the-aspect-ratio"></a>Растяжение при сохранении пропорций

Растяжение растрового изображения с сохранением пропорций является процессом, также известным как _однородное масштабирование_. Этот термин предлагаются алгоритма. Одно из возможных решений показано на странице **Равномерное масштабирование** :

```csharp
public class UniformScalingPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(UniformScalingPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public UniformScalingPage()
    {
        Title = "Uniform Scaling";

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

        float scale = Math.Min((float)info.Width / bitmap.Width, 
                               (float)info.Height / bitmap.Height);
        float x = (info.Width - scale * bitmap.Width) / 2;
        float y = (info.Height - scale * bitmap.Height) / 2;
        SKRect destRect = new SKRect(x, y, x + scale * bitmap.Width, 
                                           y + scale * bitmap.Height);

        canvas.DrawBitmap(bitmap, destRect);
    }
}
```

Обработчик `PaintSurface` вычисляет коэффициент `scale`, который является минимальным отношением ширины и высоты отображаемого значения к ширине и высоте растрового изображения. Затем можно вычислить значения `x` и `y` для центрирования масштабированного растрового изображения в пределах отображаемой ширины и высоты. Прямоугольник назначения содержит левый верхний угол `x` и `y` и правый нижний угол этих значений плюс масштабированную ширину и высоту точечного рисунка:

[![Унифицированное масштабирование](displaying-images/UniformScaling.png "Унифицированное масштабирование")](displaying-images/UniformScaling-Large.png#lightbox)

Повернуть телефон набок, чтобы увидеть растровое изображение растягивается для этой области:

[![Ландшафт равномерного масштабирования](displaying-images/UniformScaling-Landscape.png "Ландшафт равномерного масштабирования")](displaying-images/UniformScaling-Landscape-Large.png#lightbox)

Преимущества использования этого `scale` фактора становятся очевидными, если требуется реализовать немного другой алгоритм. Предположим, что вы хотите сохранить соотношение сторон растрового изображения, но также заполняет прямоугольник назначения. Единственное, что можно сделать, — обрезать часть изображения, но можно реализовать этот алгоритм просто путем изменения `Math.Min` для `Math.Max` в приведенном выше коде. Ниже приведен результат: 

[![Альтернатива унифицированного масштабирования](displaying-images/UniformScaling-Alternative.png "Альтернатива унифицированного масштабирования")](displaying-images/UniformScaling-Alternative-Large.png#lightbox)

Растрового изображения пропорции сохраняются, но обрезаются области слева и справа от растрового изображения.

## <a name="a-versatile-bitmap-display-function"></a>Функция отображения универсальный растрового изображения

Программирования среды на основе XAML (например, UWP и Xamarin.Forms) имеют возможность увеличивать и уменьшать размер точечных рисунков, сохраняя их пропорций. Несмотря на то, что SkiaSharp не поддерживает эту функцию, вы можете реализовать его самостоятельно. Класс `BitmapExtensions`, входящий в приложение [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) , показывает, как это делать. Класс определяет два новых `DrawBitmap` методов, выполняющих вычисление пропорций. Эти новые методы являются методами расширения `SKCanvas`.

Новые методы `DrawBitmap` включают параметр типа `BitmapStretch`, перечисление, определенное в файле **BitmapExtensions.CS** :

```csharp
public enum BitmapStretch
{
    None,
    Fill,
    Uniform,
    UniformToFill,
    AspectFit = Uniform,
    AspectFill = UniformToFill
}
```

Члены `None`, `Fill`, `Uniform`и `UniformToFill` совпадают с элементами перечисления UWP [`Stretch`](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.stretch.aspx) . Аналогичное перечисление [`Aspect`](xref:Xamarin.Forms.Aspect) Xamarin. Forms определяет члены `Fill`, `AspectFit`и `AspectFill`.

Показанная выше страница с **равномерным масштабированием** выравнивает точечный рисунок внутри прямоугольника, но могут потребоваться другие параметры, например размещение растрового изображения в левой или правой части прямоугольника, сверху или снизу. Это назначение перечисления `BitmapAlignment`:

```csharp
public enum BitmapAlignment
{
    Start,
    Center,
    End
}
```

Параметры выравнивания не действуют при использовании с `BitmapStretch.Fill`.

Первая `DrawBitmap`ная функция расширения содержит прямоугольник назначения, но не имеет исходного прямоугольника. Значения по умолчанию определяются таким образом, чтобы при необходимости центрировать точечный рисунок необходимо указать только элемент `BitmapStretch`:

```csharp
static class BitmapExtensions
{
    ···
    public static void DrawBitmap(this SKCanvas canvas, SKBitmap bitmap, SKRect dest, 
                                  BitmapStretch stretch, 
                                  BitmapAlignment horizontal = BitmapAlignment.Center, 
                                  BitmapAlignment vertical = BitmapAlignment.Center, 
                                  SKPaint paint = null)
    {
        if (stretch == BitmapStretch.Fill)
        {
            canvas.DrawBitmap(bitmap, dest, paint);
        }
        else
        {
            float scale = 1;

            switch (stretch)
            {
                case BitmapStretch.None:
                    break;

                case BitmapStretch.Uniform:
                    scale = Math.Min(dest.Width / bitmap.Width, dest.Height / bitmap.Height);
                    break;

                case BitmapStretch.UniformToFill:
                    scale = Math.Max(dest.Width / bitmap.Width, dest.Height / bitmap.Height);
                    break;
            }

            SKRect display = CalculateDisplayRect(dest, scale * bitmap.Width, scale * bitmap.Height, 
                                                  horizontal, vertical);

            canvas.DrawBitmap(bitmap, display, paint);
        }
    }
    ···
}
```

Основным назначением этого метода является вычисление коэффициента масштабирования с именем `scale`, который затем применяется к ширине и высоте растрового изображения при вызове метода `CalculateDisplayRect`. Это метод, который вычисляет прямоугольник для отображения растрового изображения, на основании выравнивания по горизонтали и вертикали:

```csharp
static class BitmapExtensions
{
    ···
    static SKRect CalculateDisplayRect(SKRect dest, float bmpWidth, float bmpHeight, 
                                       BitmapAlignment horizontal, BitmapAlignment vertical)
    {
        float x = 0;
        float y = 0;

        switch (horizontal)
        {
            case BitmapAlignment.Center:
                x = (dest.Width - bmpWidth) / 2;
                break;

            case BitmapAlignment.Start:
                break;

            case BitmapAlignment.End:
                x = dest.Width - bmpWidth;
                break;
        }

        switch (vertical)
        {
            case BitmapAlignment.Center:
                y = (dest.Height - bmpHeight) / 2;
                break;

            case BitmapAlignment.Start:
                break;

            case BitmapAlignment.End:
                y = dest.Height - bmpHeight;
                break;
        }

        x += dest.Left;
        y += dest.Top;

        return new SKRect(x, y, x + bmpWidth, y + bmpHeight);
    }
}
```

Класс `BitmapExtensions` содержит дополнительный метод `DrawBitmap` с исходным прямоугольником для указания подмножества точечного рисунка. Этот метод аналогичен первому за исключением того, что коэффициент масштабирования вычисляется на основе `source` прямоугольника, а затем применяется к `source` прямоугольнику в вызове `CalculateDisplayRect`:

```csharp
static class BitmapExtensions
{
    ···
    public static void DrawBitmap(this SKCanvas canvas, SKBitmap bitmap, SKRect source, SKRect dest,
                                  BitmapStretch stretch,
                                  BitmapAlignment horizontal = BitmapAlignment.Center,
                                  BitmapAlignment vertical = BitmapAlignment.Center,
                                  SKPaint paint = null)
    {
        if (stretch == BitmapStretch.Fill)
        {
            canvas.DrawBitmap(bitmap, source, dest, paint);
        }
        else
        {
            float scale = 1;

            switch (stretch)
            {
                case BitmapStretch.None:
                    break;

                case BitmapStretch.Uniform:
                    scale = Math.Min(dest.Width / source.Width, dest.Height / source.Height);
                    break;

                case BitmapStretch.UniformToFill:
                    scale = Math.Max(dest.Width / source.Width, dest.Height / source.Height);
                    break;
            }

            SKRect display = CalculateDisplayRect(dest, scale * source.Width, scale * source.Height, 
                                                  horizontal, vertical);

            canvas.DrawBitmap(bitmap, source, display, paint);
        }
    }
    ···
}
```

Первый из этих двух новых методов `DrawBitmap` показан на странице **режимы масштабирования** . Файл XAML содержит три элемента `Picker`, которые позволяют выбрать элементы перечислений `BitmapStretch` и `BitmapAlignment`:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:SkiaSharpFormsDemos"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.ScalingModesPage"
             Title="Scaling Modes">

    <Grid Padding="10">
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <skia:SKCanvasView x:Name="canvasView" 
                           Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Label Text="Stretch:"
               Grid.Row="1" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="stretchPicker"
                Grid.Row="1" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapStretch}">
                    <x:Static Member="local:BitmapStretch.None" />
                    <x:Static Member="local:BitmapStretch.Fill" />
                    <x:Static Member="local:BitmapStretch.Uniform" />
                    <x:Static Member="local:BitmapStretch.UniformToFill" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Label Text="Horizontal Alignment:"
               Grid.Row="2" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="horizontalPicker" 
                Grid.Row="2" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapAlignment}">
                    <x:Static Member="local:BitmapAlignment.Start" />
                    <x:Static Member="local:BitmapAlignment.Center" />
                    <x:Static Member="local:BitmapAlignment.End" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Label Text="Vertical Alignment:"
               Grid.Row="3" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="verticalPicker" 
                Grid.Row="3" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapAlignment}">
                    <x:Static Member="local:BitmapAlignment.Start" />
                    <x:Static Member="local:BitmapAlignment.Center" />
                    <x:Static Member="local:BitmapAlignment.End" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>
    </Grid>
</ContentPage>
```

Файл кода программной части просто делает недействительным `CanvasView` при изменении любого элемента `Picker`. Обработчик `PaintSurface` обращается к трем представлениям `Picker` для вызова метода расширения `DrawBitmap`:

```csharp
public partial class ScalingModesPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(ScalingModesPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public ScalingModesPage()
    {
        InitializeComponent();
    }

    private void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect dest = new SKRect(0, 0, info.Width, info.Height);

        BitmapStretch stretch = (BitmapStretch)stretchPicker.SelectedItem;
        BitmapAlignment horizontal = (BitmapAlignment)horizontalPicker.SelectedItem;
        BitmapAlignment vertical = (BitmapAlignment)verticalPicker.SelectedItem;

        canvas.DrawBitmap(bitmap, dest, stretch, horizontal, vertical);
    }
}
```

Ниже приведены некоторые сочетания параметров.

[![Режимы масштабирования](displaying-images/ScalingModes.png "Режимы масштабирования")](displaying-images/ScalingModes-Large.png#lightbox)

Страница **подмножества прямоугольников** имеет практически тот же XAML-файл, что и **режимы масштабирования**, но файл кода программной части определяет прямоугольное подмножество точечного рисунка, заданное `SOURCE` полем: 

```csharp
public partial class ScalingModesPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(ScalingModesPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");

    static readonly SKRect SOURCE = new SKRect(94, 12, 212, 118);

    public RectangleSubsetPage()
    {
        InitializeComponent();
    }

    private void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect dest = new SKRect(0, 0, info.Width, info.Height);

        BitmapStretch stretch = (BitmapStretch)stretchPicker.SelectedItem;
        BitmapAlignment horizontal = (BitmapAlignment)horizontalPicker.SelectedItem;
        BitmapAlignment vertical = (BitmapAlignment)verticalPicker.SelectedItem;

        canvas.DrawBitmap(bitmap, SOURCE, dest, stretch, horizontal, vertical);
    }
}
```

Этот источник прямоугольник изолирует monkey head, как показано на эти снимки экрана:

[![Подмножество прямоугольников](displaying-images/RectangleSubset.png "Подмножество прямоугольников")](displaying-images/RectangleSubset-Large.png#lightbox)

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
