---
title: Анимации точечных рисунков SkiaSharp
description: Узнайте, как выполнить анимацию растрового изображения, последовательно отображение ряда точечных рисунков и Подготовка к просмотру анимированные GIF-файлы.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 97142ADC-E2FD-418C-8A09-9C561AEE5BFD
author: davidbritch
ms.author: dabritch
ms.date: 07/12/2018
ms.openlocfilehash: 33e17a01d8a13fcdaee27e5857c554a4a232c534
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305675"
---
# <a name="animating-skiasharp-bitmaps"></a>Анимации точечных рисунков SkiaSharp

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Приложения, которые анимированы SkiaSharp Graphics, обычно вызывают `InvalidateSurface` на `SKCanvasView` с фиксированной частотой, часто каждые 16 миллисекунд. При аннулировании поверхности вызывается обработчик `PaintSurface` для перерисовки отображения. Как визуальные элементы перерисовываются 60 раз в секунду, они отображаются плавно анимировать.

Тем не менее если графики слишком сложны для отображения в 16 миллисекунд, анимация может стать перебои управления. Программист может потребоваться уменьшить частоту обновления до 30 раз или 15 раз в секунду, но иногда этого недостаточно. Иногда графика — настолько сложны, что они просто не может быть отображен в режиме реального времени.

Одним из решений является заранее подготовить для анимации с визуализации отдельных кадров анимации на ряд точечных рисунков. Для отображения анимации, необходим только для отображения растровых изображения последовательно 60 раз в секунду.

Само собой, это потенциально много точечных рисунков, но это как большой бюджет 3D вносятся анимированных фильмов. 3D-графики гораздо слишком сложны для отображения в режиме реального времени. Требуется много времени обработки для отрисовки каждого кадра. Что видят при просмотре фильма является по сути ряд точечных рисунков.

Это можно сделать нечто подобное в SkiaSharp. В этой статье показано два типа анимации растрового изображения. Первый пример выполняется анимация Мандельброта:

![Пример анимации](animating-images/AnimatingSample.png "Пример анимации")

Во втором примере показано, как использовать SkiaSharp для подготовки к просмотру анимированный GIF-файл.

## <a name="bitmap-animation"></a>Анимации для точечных рисунков

Мандельброта визуально интересно, но computionally длинными. (Описание набора фрактала Мандельброта и математических операций, использованных здесь, см. в [главе 20 _Создание мобильных приложений с помощью Xamarin. Forms_ ](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf) , начиная со страницы 666. Со следующим описанием предполагается, что фоновые знания.)

Пример [**анимации фрактала Мандельброта**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-mandelanima) использует растровую анимацию для имитации непрерывного масштабирования фиксированной точки в наборе Мандельброта. Увеличение следуют масштаба изображения, и затем цикл повторяется неограниченное время или до окончания работы программы.

Программа подготавливает этой анимации путем создания до 50 растровые изображения, хранящиеся в локальном хранилище приложения. Каждый рисунок охватывает половины ширины и высоты на комплексной плоскости как предыдущих битовую карту. (В программе эти точечные рисунки говорят, что представляют собой целочисленные _уровни масштабирования_.) Растровые изображения отображаются последовательно. Масштабирование растрового изображения каждого анимируется для предоставления smooth последовательность от одного точечного рисунка в другой.

Как и в финальной программе, описанной в главе 20 _Создание мобильных приложений с помощью Xamarin. Forms_, вычисление фрактала Мандельброта в **анимации фрактала Мандельброта** — это асинхронный метод с восемью параметрами. Параметры включают сложные центральной точки и ширину и высоту на комплексной плоскости, вокруг этой центральной точки. Следующие три параметра: ширина в пикселях и Высота растрового изображения должен быть создан и максимальное количество итераций для рекурсивных вычислений. Параметр `progress` используется для показа хода выполнения этого вычисления. Параметр `cancelToken` не используется в этой программе:

```csharp
static class Mandelbrot
{
    public static Task<BitmapInfo> CalculateAsync(Complex center,
                                                  double width, double height,
                                                  int pixelWidth, int pixelHeight,
                                                  int iterations,
                                                  IProgress<double> progress,
                                                  CancellationToken cancelToken)
    {
        return Task.Run(() =>
        {
            int[] iterationCounts = new int[pixelWidth * pixelHeight];
            int index = 0;

            for (int row = 0; row < pixelHeight; row++)
            {
                progress.Report((double)row / pixelHeight);
                cancelToken.ThrowIfCancellationRequested();

                double y = center.Imaginary + height / 2 - row * height / pixelHeight;

                for (int col = 0; col < pixelWidth; col++)
                {
                    double x = center.Real - width / 2 + col * width / pixelWidth;
                    Complex c = new Complex(x, y);

                    if ((c - new Complex(-1, 0)).Magnitude < 1.0 / 4)
                    {
                        iterationCounts[index++] = -1;
                    }
                    // http://www.reenigne.org/blog/algorithm-for-mandelbrot-cardioid/
                    else if (c.Magnitude * c.Magnitude * (8 * c.Magnitude * c.Magnitude - 3) < 3.0 / 32 - c.Real)
                    {
                        iterationCounts[index++] = -1;
                    }
                    else
                    {
                        Complex z = 0;
                        int iteration = 0;

                        do
                        {
                            z = z * z + c;
                            iteration++;
                        }
                        while (iteration < iterations && z.Magnitude < 2);

                        if (iteration == iterations)
                        {
                            iterationCounts[index++] = -1;
                        }
                        else
                        {
                            iterationCounts[index++] = iteration;
                        }
                    }
                }
            }
            return new BitmapInfo(pixelWidth, pixelHeight, iterationCounts);
        }, cancelToken);
    }
}
```

Метод возвращает объект типа `BitmapInfo`, который предоставляет сведения для создания точечного рисунка.

```csharp
class BitmapInfo
{
    public BitmapInfo(int pixelWidth, int pixelHeight, int[] iterationCounts)
    {
        PixelWidth = pixelWidth;
        PixelHeight = pixelHeight;
        IterationCounts = iterationCounts;
    }

    public int PixelWidth { private set; get; }

    public int PixelHeight { private set; get; }

    public int[] IterationCounts { private set; get; }
}
```

XAML-файл **анимации фрактала Мандельброта** включает в себя два `Label` представлений, `ProgressBar`и `Button`, а также `SKCanvasView`:

```csharp
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="MandelAnima.MainPage"
             Title="Mandelbrot Animation">

    <StackLayout>
        <Label x:Name="statusLabel"
               HorizontalTextAlignment="Center" />
        <ProgressBar x:Name="progressBar" />

        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <StackLayout Orientation="Horizontal"
                     Padding="5">
            <Label x:Name="storageLabel"
                   VerticalOptions="Center" />

            <Button x:Name="deleteButton"
                    Text="Delete All"
                    HorizontalOptions="EndAndExpand"
                    Clicked="OnDeleteButtonClicked" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Файл с выделенным кодом начинается с определения трех решающее значение константы, а также массив растровых изображений:

```csharp
public partial class MainPage : ContentPage
{
    const int COUNT = 10;           // The number of bitmaps in the animation.
                                    // This can go up to 50!

    const int BITMAP_SIZE = 1000;   // Program uses square bitmaps exclusively

    // Uncomment just one of these, or define your own
    static readonly Complex center = new Complex(-1.17651152924355, 0.298520986549558);
    //   static readonly Complex center = new Complex(-0.774693089457127, 0.124226621261617);
    //   static readonly Complex center = new Complex(-0.556624880053304, 0.634696788141351);

    SKBitmap[] bitmaps = new SKBitmap[COUNT];   // array of bitmaps
    ···
}
```

В некоторый момент вы, вероятно, захотите изменить значение `COUNT` на 50, чтобы увидеть весь диапазон анимации. Значения выше 50 не используются. Вокруг уровень масштабирования — 48, или около того разрешение чисел с плавающей запятой двойной точности становится недостаточным для расчета Мандельброта. Эта проблема обсуждается на странице 684, посвященной _созданию мобильных приложений с помощью Xamarin. Forms_.

Значение `center` очень важно. Это цель анимации масштабирования. Три значения в файле — это те, которые используются в трех последних снимках экрана в главе 20 _создания мобильных приложений с помощью Xamarin. Forms_ на странице 684, но вы можете поэкспериментировать с программой в этой главе, чтобы получить одно из собственных значений.

Пример **анимации фрактала Мандельброта** сохраняет эти `COUNT` точечные рисунки в локальном хранилище приложений. Пятьдесят точечные рисунки требуют более чем 20 мегабайт памяти на устройстве, поэтому полезно знать, какой объем хранилища, занимаемого эти точечные рисунки, а в определенный момент может потребоваться удалить их все. Это назначение этих двух методов в нижней части класса `MainPage`:

```csharp
public partial class MainPage : ContentPage
{
    ···
    void TallyBitmapSizes()
    {
        long fileSize = 0;

        foreach (string filename in Directory.EnumerateFiles(FolderPath()))
        {
            fileSize += new FileInfo(filename).Length;
        }

        storageLabel.Text = $"Total storage: {fileSize:N0} bytes";
    }

    void OnDeleteButtonClicked(object sender, EventArgs args)
    {
        foreach (string filepath in Directory.EnumerateFiles(FolderPath()))
        {
            File.Delete(filepath);
        }

        TallyBitmapSizes();
    }
}
```

Точечные рисунки в локальном хранилище можно удалить, пока программа выполняет анимацию этих же растровые изображения, так как программа сохраняет их в памяти. Но следующий раз при запуске программы, его нужно будет заново создать точечные рисунки.

Точечные рисунки, хранящиеся в локальном хранилище приложений, включают в себя `center` значение в именах файлов, поэтому при изменении параметра `center` существующие растровые изображения не будут заменены в хранилище и будут по-прежнему занимать место.

Ниже приведены методы, которые `MainPage` используют для создания имен файлов, а также метод `MakePixel` для определения значения пикселя на основе цветовых компонентов:

```csharp
public partial class MainPage : ContentPage
{
    ···
    // File path for storing each bitmap in local storage
    string FolderPath() =>
        Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData);

    string FilePath(int zoomLevel) =>
        Path.Combine(FolderPath(),
                     String.Format("R{0}I{1}Z{2:D2}.png", center.Real, center.Imaginary, zoomLevel));

    // Form bitmap pixel for Rgba8888 format
    uint MakePixel(byte alpha, byte red, byte green, byte blue) =>
        (uint)((alpha << 24) | (blue << 16) | (green << 8) | red);
    ···
}
```

Параметр `zoomLevel` для `FilePath` диапазоны от 0 до `COUNT` константы минус 1.

Конструктор `MainPage` вызывает метод `LoadAndStartAnimation`:

```csharp
public partial class MainPage : ContentPage
{
    ···
    public MainPage()
    {
        InitializeComponent();

        LoadAndStartAnimation();
    }
    ···
}
```

Метод `LoadAndStartAnimation` отвечает за доступ к локальному хранилищу приложения для загрузки любых растровых изображений, которые могли быть созданы при выполнении программы ранее. Он просматривает `zoomLevel` значения от 0 до `COUNT`. Если файл существует, он загружается в массив `bitmaps`. В противном случае необходимо создать точечный рисунок для конкретного `center` и `zoomLevel` значений путем вызова `Mandelbrot.CalculateAsync`. Этот метод получает результатов итерации для каждой точки, который этот метод преобразует в цвета:

```csharp
public partial class MainPage : ContentPage
{
    ···
    async void LoadAndStartAnimation()
    {
        // Show total bitmap storage
        TallyBitmapSizes();

        // Create progressReporter for async operation
        Progress<double> progressReporter =
            new Progress<double>((double progress) => progressBar.Progress = progress);

        // Create (unused) CancellationTokenSource for async operation
        CancellationTokenSource cancelTokenSource = new CancellationTokenSource();

        // Loop through all the zoom levels
        for (int zoomLevel = 0; zoomLevel < COUNT; zoomLevel++)
        {
            // If the file exists, load it
            if (File.Exists(FilePath(zoomLevel)))
            {
                statusLabel.Text = $"Loading bitmap for zoom level {zoomLevel}";

                using (Stream stream = File.OpenRead(FilePath(zoomLevel)))
                {
                    bitmaps[zoomLevel] = SKBitmap.Decode(stream);
                }
            }
            // Otherwise, create a new bitmap
            else
            {
                statusLabel.Text = $"Creating bitmap for zoom level {zoomLevel}";

                CancellationToken cancelToken = cancelTokenSource.Token;

                // Do the (generally lengthy) Mandelbrot calculation
                BitmapInfo bitmapInfo =
                    await Mandelbrot.CalculateAsync(center,
                                                    4 / Math.Pow(2, zoomLevel),
                                                    4 / Math.Pow(2, zoomLevel),
                                                    BITMAP_SIZE, BITMAP_SIZE,
                                                    (int)Math.Pow(2, 10), progressReporter, cancelToken);

                // Create bitmap & get pointer to the pixel bits
                SKBitmap bitmap = new SKBitmap(BITMAP_SIZE, BITMAP_SIZE, SKColorType.Rgba8888, SKAlphaType.Opaque);
                IntPtr basePtr = bitmap.GetPixels();

                // Set pixel bits to color based on iteration count
                for (int row = 0; row < bitmap.Width; row++)
                    for (int col = 0; col < bitmap.Height; col++)
                    {
                        int iterationCount = bitmapInfo.IterationCounts[row * bitmap.Width + col];
                        uint pixel = 0xFF000000;            // black

                        if (iterationCount != -1)
                        {
                            double proportion = (iterationCount / 32.0) % 1;
                            byte red = 0, green = 0, blue = 0;

                            if (proportion < 0.5)
                            {
                                red = (byte)(255 * (1 - 2 * proportion));
                                blue = (byte)(255 * 2 * proportion);
                            }
                            else
                            {
                                proportion = 2 * (proportion - 0.5);
                                green = (byte)(255 * proportion);
                                blue = (byte)(255 * (1 - proportion));
                            }

                            pixel = MakePixel(0xFF, red, green, blue);
                        }

                        // Calculate pointer to pixel
                        IntPtr pixelPtr = basePtr + 4 * (row * bitmap.Width + col);

                        unsafe     // requires compiling with unsafe flag
                        {
                            *(uint*)pixelPtr.ToPointer() = pixel;
                        }
                    }

                // Save as PNG file
                SKData data = SKImage.FromBitmap(bitmap).Encode();

                try
                {
                    File.WriteAllBytes(FilePath(zoomLevel), data.ToArray());
                }
                catch
                {
                    // Probably out of space, but just ignore
                }

                // Store in array
                bitmaps[zoomLevel] = bitmap;

                // Show new bitmap sizes
                TallyBitmapSizes();
            }

            // Display the bitmap
            bitmapIndex = zoomLevel;
            canvasView.InvalidateSurface();
        }

        // Now start the animation
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }
    ···
}
```

Обратите внимание на то, что программа сохраняет эти точечные рисунки в хранилище локального приложения, а не в библиотеке фотографий устройства. Библиотека .NET Standard 2,0 позволяет использовать знакомые методы `File.OpenRead` и `File.WriteAllBytes` для этой задачи.

После того как все точечные рисунки созданы или загружены в память, метод запускает объект `Stopwatch` и вызывает `Device.StartTimer`. Метод `OnTimerTick` вызывается каждые 16 миллисекунд.

`OnTimerTick` вычисляет значение `time` в миллисекундах в диапазоне от 0 до 6000 раз `COUNT`, что приводит к отображению каждого растрового изображения на шесть секунд. Значение `progress` использует значение `Math.Sin`, чтобы создать анимацию синусоидальной, которая будет выполняться медленнее в начале цикла, и медленнее в конце, так как в обратном направлении.

Значение `progress` в диапазоне от 0 до `COUNT`. Это означает, что целая часть `progress` является индексом в массиве `bitmaps`, а дробная часть `progress` указывает уровень масштаба для этого конкретного точечного рисунка. Эти значения хранятся в полях `bitmapIndex` и `bitmapProgress` и отображаются `Label` и `Slider` в файле XAML. `SKCanvasView` становится недействительным для обновления экрана растрового изображения:

```csharp
public partial class MainPage : ContentPage
{
    ···
    Stopwatch stopwatch = new Stopwatch();      // for the animation
    int bitmapIndex;
    double bitmapProgress = 0;
    ···
    bool OnTimerTick()
    {
        int cycle = 6000 * COUNT;       // total cycle length in milliseconds

        // Time in milliseconds from 0 to cycle
        int time = (int)(stopwatch.ElapsedMilliseconds % cycle);

        // Make it sinusoidal, including bitmap index and gradation between bitmaps
        double progress = COUNT * 0.5 * (1 + Math.Sin(2 * Math.PI * time / cycle - Math.PI / 2));

        // These are the field values that the PaintSurface handler uses
        bitmapIndex = (int)progress;
        bitmapProgress = progress - bitmapIndex;

        // It doesn't often happen that we get up to COUNT, but an exception would be raised
        if (bitmapIndex < COUNT)
        {
            // Show progress in UI
            statusLabel.Text = $"Displaying bitmap for zoom level {bitmapIndex}";
            progressBar.Progress = bitmapProgress;

            // Update the canvas
            canvasView.InvalidateSurface();
        }

        return true;
    }
    ···
}
```

Наконец, обработчик `PaintSurface` `SKCanvasView` вычисляет прямоугольник назначения, чтобы отображать точечный рисунок максимально крупным образом при сохранении пропорций. Исходный прямоугольник основан на значении `bitmapProgress`. Значение `fraction`, вычисленное здесь, находится в диапазоне от 0, если `bitmapProgress` равно 0, чтобы отобразить всю битовую карту, до 0,25, если `bitmapProgress` равен 1, чтобы отобразить половину ширины и высоты точечного рисунка, эффективно масштабирование:

```csharp
public partial class MainPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        if (bitmaps[bitmapIndex] != null)
        {
            // Determine destination rect as square in canvas
            int dimension = Math.Min(info.Width, info.Height);
            float x = (info.Width - dimension) / 2;
            float y = (info.Height - dimension) / 2;
            SKRect destRect = new SKRect(x, y, x + dimension, y + dimension);

            // Calculate source rectangle based on fraction:
            //  bitmapProgress == 0: full bitmap
            //  bitmapProgress == 1: half of length and width of bitmap
            float fraction = 0.5f * (1 - (float)Math.Pow(2, -bitmapProgress));
            SKBitmap bitmap = bitmaps[bitmapIndex];
            int width = bitmap.Width;
            int height = bitmap.Height;
            SKRect sourceRect = new SKRect(fraction * width, fraction * height,
                                           (1 - fraction) * width, (1 - fraction) * height);

            // Display the bitmap
            canvas.DrawBitmap(bitmap, sourceRect, destRect);
        }
    }
    ···
}
```

Вот работающая программа:

[![Анимация фрактала Мандельброта](animating-images/MandelbrotAnimation.png "Анимация фрактала Мандельброта")](animating-images/MandelbrotAnimation-Large.png#lightbox)

## <a name="gif-animation"></a>Анимированный GIF

Спецификация формата GIF (Graphics Interchange) включает компонент, позволяющий единый GIF-файл должен содержать несколько последовательных кадров сцены, который может быть отображен подряд, часто в цикле. Эти файлы называются _анимированными GIF_. Веб-браузеры могут играть анимированные изображения формата GIF и SkiaSharp позволяет приложению для извлечения из анимированный GIF-файл кадры и отобразить их последовательно.

Образец [скиашарпформсдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) включает в себя анимированный ресурс GIF с именем **Newtons_cradle_animation_book_2. gif** , созданный демонделуксе и скачанный на странице [док Ньютона](https://en.wikipedia.org/wiki/Newton%27s_cradle) в Википедии. **Анимированная страница GIF** содержит файл XAML, который предоставляет эти сведения и создает экземпляр `SKCanvasView`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.AnimatedGifPage"
             Title="Animated GIF">

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="0"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Label Text="GIF file by DemonDeLuxe from Wikipedia Newton's Cradle page"
               Grid.Row="1"
               Margin="0, 5"
               HorizontalTextAlignment="Center" />
    </Grid>
</ContentPage>
```

Файл с выделенным кодом не обобщается для воспроизведения анимированный GIF-файл. Он игнорирует большая часть данных, который доступен, в частности, число повторений и просто играет анимационный GIF в цикле.

Использование SkisSharp для извлечения кадры анимированный GIF-файл не документироваться в любой точке мира, поэтому описания приведенный ниже код — более подробные, чем обычно:

Декодирование анимированного GIF-файла происходит в конструкторе страницы и требует, чтобы объект `Stream`, ссылающийся на точечный рисунок, использовался для создания объекта `SKManagedStream`, а затем объект [`SKCodec`](xref:SkiaSharp.SKCodec) . Свойство [`FrameCount`](xref:SkiaSharp.SKCodec.FrameCount) указывает количество кадров, которые составляют анимацию.

В конечном итоге эти кадры сохраняются как отдельные точечные рисунки, поэтому конструктор использует `FrameCount` для выделения массива типа `SKBitmap`, а также двух `int` массивов в течение каждого кадра и (для упрощения логики анимации) накопленные длительности.

Свойство [`FrameInfo`](xref:SkiaSharp.SKCodec.FrameInfo) класса `SKCodec` является массивом значений [`SKCodecFrameInfo`](xref:SkiaSharp.SKCodecFrameInfo) , по одному для каждого кадра, но единственное, что эта программа получает из этой структуры, — [`Duration`](xref:SkiaSharp.SKCodecFrameInfo.Duration) кадра в миллисекундах.

`SKCodec` определяет свойство с именем [`Info`](xref:SkiaSharp.SKCodec.Info) типа [`SKImageInfo`](xref:SkiaSharp.SKImageInfo), но это значение `SKImageInfo` указывает (по крайней мере для этого изображения), что тип цвета — `SKColorType.Index8`. Это означает, что каждый пиксель является индексом типа цвета. Чтобы не тратиться на использование таблиц цветов, программа использует [`Width`](xref:SkiaSharp.SKImageInfo.Width) и [`Height`](xref:SkiaSharp.SKImageInfo.Height) сведения из этой структуры для создания собственного значения `ImageInfo` полного цвета. Каждый `SKBitmap` создается из этого.

Метод `GetPixels` `SKBitmap` возвращает `IntPtr` ссылающийся на пиксельные биты этого растрового изображения. Эти биты пикселов еще не задано. `IntPtr` передается одному из методов [`GetPixels`](xref:SkiaSharp.SKCodec.GetPixels(SkiaSharp.SKImageInfo,System.IntPtr,SkiaSharp.SKCodecOptions)) `SKCodec`. Этот метод копирует кадр из GIF-файла в пространство памяти, на которое ссылается `IntPtr`. Конструктор [`SKCodecOptions`](xref:SkiaSharp.SKCodecOptions) указывает номер кадра:

```csharp
public partial class AnimatedGifPage : ContentPage
{
    SKBitmap[] bitmaps;
    int[] durations;
    int[] accumulatedDurations;
    int totalDuration;
    ···

    public AnimatedGifPage ()
    {
        InitializeComponent ();

        string resourceID = "SkiaSharpFormsDemos.Media.Newtons_cradle_animation_book_2.gif";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        using (SKCodec codec = SKCodec.Create(skStream))
        {
            // Get frame count and allocate bitmaps
            int frameCount = codec.FrameCount;
            bitmaps = new SKBitmap[frameCount];
            durations = new int[frameCount];
            accumulatedDurations = new int[frameCount];

            // Note: There's also a RepetitionCount property of SKCodec not used here

            // Loop through the frames
            for (int frame = 0; frame < frameCount; frame++)
            {
                // From the FrameInfo collection, get the duration of each frame
                durations[frame] = codec.FrameInfo[frame].Duration;

                // Create a full-color bitmap for each frame
                SKImageInfo imageInfo = code.new SKImageInfo(codec.Info.Width, codec.Info.Height);
                bitmaps[frame] = new SKBitmap(imageInfo);

                // Get the address of the pixels in that bitmap
                IntPtr pointer = bitmaps[frame].GetPixels();

                // Create an SKCodecOptions value to specify the frame
                SKCodecOptions codecOptions = new SKCodecOptions(frame, false);

                // Copy pixels from the frame into the bitmap
                codec.GetPixels(imageInfo, pointer, codecOptions);
            }

            // Sum up the total duration
            for (int frame = 0; frame < durations.Length; frame++)
            {
                totalDuration += durations[frame];
            }

            // Calculate the accumulated durations
            for (int frame = 0; frame < durations.Length; frame++)
            {
                accumulatedDurations[frame] = durations[frame] +
                    (frame == 0 ? 0 : accumulatedDurations[frame - 1]);
            }
        }
    }
    ···
}
```

Несмотря на значение `IntPtr`, код `unsafe` не требуется, так как `IntPtr` никогда не преобразуется в значение C# указателя.

После извлечения каждого кадра, конструктор итогов вверх продолжительность всех кадров, а затем инициализирует другой массив с накопленные длительности.

В оставшейся части файла кода, предназначенный для анимации. Метод `Device.StartTimer` используется для запуска таймера, а обратный вызов `OnTimerTick` использует объект `Stopwatch` для определения затраченного времени в миллисекундах. Перебор массива накопленные длительности достаточно найти текущего кадра:

```csharp
public partial class AnimatedGifPage : ContentPage
{
    SKBitmap[] bitmaps;
    int[] durations;
    int[] accumulatedDurations;
    int totalDuration;

    Stopwatch stopwatch = new Stopwatch();
    bool isAnimating;

    int currentFrame;
    ···
    protected override void OnAppearing()
    {
        base.OnAppearing();

        isAnimating = true;
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();

        stopwatch.Stop();
        isAnimating = false;
    }

    bool OnTimerTick()
    {
        int msec = (int)(stopwatch.ElapsedMilliseconds % totalDuration);
        int frame = 0;

        // Find the frame based on the elapsed time
        for (frame = 0; frame < accumulatedDurations.Length; frame++)
        {
            if (msec < accumulatedDurations[frame])
            {
                break;
            }
        }

        // Save in a field and invalidate the SKCanvasView.
        if (currentFrame != frame)
        {
            currentFrame = frame;
            canvasView.InvalidateSurface();
        }

        return isAnimating;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);

        // Get the bitmap and center it
        SKBitmap bitmap = bitmaps[currentFrame];
        canvas.DrawBitmap(bitmap,info.Rect, BitmapStretch.Uniform);
    }
}
```

При каждом изменении `currentframe` переменной `SKCanvasView` становится недействительным и отображается новый кадр:

[![Анимированный GIF](animating-images/AnimatedGif.png "Анимированный GIF")](animating-images/AnimatedGif-Large.png#lightbox)

Само собой, необходимо запустить программу самостоятельно, чтобы просмотреть анимацию.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [Анимация фрактала Мандельброта (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-mandelanima)
