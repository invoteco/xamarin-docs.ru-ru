---
title: Доступ к биты пикселов точечного рисунка SkiaSharp
description: Обнаружить различные методы для доступа и изменения битов пикселов точечных рисунков SkiaSharp.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: DBB58522-F816-4A8C-96A5-E0236F16A5C6
author: davidbritch
ms.author: dabritch
ms.date: 07/11/2018
ms.openlocfilehash: 6c066f89dc8f558a9154138bf38ad4326fe21291
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306461"
---
# <a name="accessing-skiasharp-bitmap-pixel-bits"></a>Доступ к биты пикселов точечного рисунка SkiaSharp

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Как было показано в статье [**Сохранение точечных рисунков SkiaSharp в файлах**](saving.md), точечные рисунки обычно хранятся в файлах в сжатом формате, таком как JPEG или PNG. В отличие от SkiaSharp растрового изображения, хранящиеся в памяти не сжимаются. Они хранятся как ряд последовательных точках. Этот формат без сжатия упрощает перенос растровые изображения на поверхность отображения.

Блок памяти, занятый Битовая карта SkiaSharp организована очень простым способом: он начинается с первой строки пикселей слева направо, а затем продолжает операции со второй строки. Для full цветных точечных рисунков каждый пиксель состоит из четырех байтов это означает, что общий объем памяти пространства, необходимого растрового изображения четыре раза произведение высоту и ширину.

В этой статье описывается, как приложения могут получить доступ к пикселов, либо непосредственно открыть блок памяти пикселей растрового изображения, или косвенно. В некоторых случаях программа может потребоваться проанализировать пикселей изображения и создать гистограмму какого-либо рода. Обычно приложения можно создать уникальных образов алгоритмически, создав пикселей растрового изображения:

![Примеры битов пикселей](pixel-bits-images/PixelBitsSample.png "Пример точечного бита")

## <a name="the-techniques"></a>Методы

SkiaSharp предоставляет несколько методов для доступа к биты пикселов точечного рисунка. Какой из них выбран обычно связано с неким объектой удобства (что относится к обслуживанию и Простота отладки) и производительности. В большинстве случаев для доступа к пикселям растрового изображения используется один из следующих методов и свойств `SKBitmap`.

- Методы `GetPixel` и `SetPixel` позволяют получить или задать цвет одного пикселя.
- Свойство `Pixels` получает массив цветов пикселей для всего растрового изображения или задает массив цветов.
- `GetPixels` возвращает адрес пиксельной памяти, используемой точечным рисунком.
- `SetPixels` заменяет адрес памяти пикселя, используемый точечным рисунком.

Можно представить первые две методики, как «высокий уровень» и Вторые два как «низкий» уровень. Некоторые методы и свойства, которые можно использовать, но они наиболее полезны.

Чтобы увидеть различия в производительности между этими методами, приложение [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) содержит страницу с именем **градиентного растрового изображения** , которая создает точечный рисунок с пикселами, которые объединяют красные и синие тени для создания градиента. Программа создает восемь разных копий растрового изображения, все с помощью различных методов для настройки пиксели растрового изображения. Каждый из этих восьми точечные рисунки создается в отдельный метод, который также задает краткое описание этого метода и вычисляет время, необходимое для установки всех пикселей. Каждый метод перебирает логики пикселях 100 раз получить лучшую оценку производительности.

### <a name="the-setpixel-method"></a>Метод SetPixel

Если необходимо задать или получить несколько отдельных пикселов, то методы [`SetPixel`](xref:SkiaSharp.SKBitmap.SetPixel(System.Int32,System.Int32,SkiaSharp.SKColor)) и [`GetPixel`](xref:SkiaSharp.SKBitmap.GetPixel(System.Int32,System.Int32)) являются идеальными. Для каждого из этих двух методов Укажите целое число столбцов и строк. Независимо от формата пикселей эти два метода позволяют получить или задать пиксель в качестве значения `SKColor`:

```csharp
bitmap.SetPixel(col, row, color);

SKColor color = bitmap.GetPixel(col, row);
```

Аргумент `col` должен находиться в диапазоне от 0 до меньшего значения свойства `Width` растрового изображения, а `row` диапазоны от 0 до значения, меньшего свойства `Height`.

Ниже приведен метод в **битовой карте градиента** , который задает содержимое растрового изображения с помощью метода `SetPixel`. Битовая карта 256 на 256 пикселей, а циклы `for` жестко запрограммированы диапазоном значений:

```csharp
public class GradientBitmapPage : ContentPage
{
    const int REPS = 100;

    Stopwatch stopwatch = new Stopwatch();
    ···
    SKBitmap FillBitmapSetPixel(out string description, out int milliseconds)
    {
        description = "SetPixel";
        SKBitmap bitmap = new SKBitmap(256, 256);

        stopwatch.Restart();

        for (int rep = 0; rep < REPS; rep++)
            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    bitmap.SetPixel(col, row, new SKColor((byte)col, 0, (byte)row));
                }

        milliseconds = (int)stopwatch.ElapsedMilliseconds;
        return bitmap;
    }
    ···
}
```

Набор для каждого пикселя цветов имеет красный компонент которой равен столбцу растрового изображения и синего компонента, равным строке. Получающийся точечный рисунок черным в левом верхнем углу, красным в правом верхнем углу, синий в левом нижнем и пурпурный в правом нижнем углу, с градиентом в другом месте.

Метод `SetPixel` вызывается 65 536 раз и, независимо от того, насколько эффективно этот метод, обычно не рекомендуется делать это множество вызовов API, если доступна альтернатива. К счастью существует несколько вариантов.

### <a name="the-pixels-property"></a>Свойство пикселей

`SKBitmap` определяет свойство [`Pixels`](xref:SkiaSharp.SKBitmap.Pixels) , которое возвращает массив значений `SKColor` для всего растрового изображения. Можно также использовать `Pixels`, чтобы задать массив цветовых значений для точечного рисунка:

```csharp
SKColor[] pixels = bitmap.Pixels;

bitmap.Pixels = pixels;
```

Пиксели, упорядочены в массиве, начиная с первой строки, слева направо, затем второй строки и так далее. Общее число цветов в массиве равно произведению высоту и ширину растрового изображения.

Хотя это свойство является эффективным, помните, что Пиксели копируются из растрового изображения в массив, а затем из массива обратно в точечный рисунок, а пикселы преобразуются в `SKColor` значения.

Ниже приведен метод в классе `GradientBitmapPage`, который задает точечный рисунок с помощью свойства `Pixels`. Метод выделяет массив `SKColor` требуемого размера, но может использовать свойство `Pixels` для создания этого массива:

```csharp
SKBitmap FillBitmapPixelsProp(out string description, out int milliseconds)
{
    description = "Pixels property";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    SKColor[] pixels = new SKColor[256 * 256];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                pixels[256 * row + col] = new SKColor((byte)col, 0, (byte)row);
            }

    bitmap.Pixels = pixels;

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Обратите внимание, что индекс массива `pixels` необходимо вычислить на основе переменных `row` и `col`. Строки умножается на количество точек в каждой строке (256 в данном случае), а затем будет добавлен столбец.

`SKBitmap` также определяет аналогичное свойство `Bytes`, которое возвращает массив байтов для всего растрового изображения, но это более громоздки для полных точечных рисунков.

### <a name="the-getpixels-pointer"></a>Указатель GetPixels

Возможно, самым мощным способом доступа к растровым пикселам является [`GetPixels`](xref:SkiaSharp.SKBitmap.GetPixels), который не следует путать с методом `GetPixel` или свойством `Pixels`. Вы сразу же заметите разницу с `GetPixels` в том, что она возвращает нечто, C# не очень распространенное в программировании:

```csharp
IntPtr pixelsAddr = bitmap.GetPixels();
```

Тип [`IntPtr`](xref:System.IntPtr) .NET представляет указатель. Он называется `IntPtr`, так как это длина целого числа на машинном процессоре компьютера, на котором выполняется программа, как правило, 32 бит или 64 бит. `IntPtr`, который `GetPixels` возвращает, является адресом фактического блока памяти, который используется объектом Bitmap для хранения пикселов.

Можно преобразовать `IntPtr` в тип C# указателя с помощью метода [`ToPointer`](xref:System.IntPtr.ToPointer) . Указатель синтаксис C# является таким же, как C и C++:

```csharp
byte* ptr = (byte*)pixelsAddr.ToPointer();
```

Переменная `ptr` имеет тип Byte- _pointer_. Эта переменная `ptr` позволяет получить доступ к отдельным байтам памяти, используемым для хранения пикселей растрового изображения. Считывает байт из памяти или запись байта в память использовать следующий код:

```csharp
byte pixelComponent = *ptr;

*ptr = pixelComponent;
```

В этом контексте Звездочка является C# _оператором косвенного обращения_ и используется для ссылки на содержимое памяти, на которое указывает `ptr`. Изначально `ptr` указывает на первый байт первой строки точечного рисунка, но можно выполнять арифметические действия над переменной `ptr`, чтобы переместить ее в другие расположения в точечном рисунке.

Недостаток заключается в том, что эту `ptr`ную переменную можно использовать только в блоке кода, помеченном ключевым словом `unsafe`. Кроме того сборки должен быть помечен как допускающий небезопасные блоки. Для этого в свойствах проекта.

С использованием указателей в C# — очень мощный, но также очень опасно. Необходимо соблюдать осторожность, что не доступа к памяти за то, что указатель должен ссылаться на. Именно по этой причине используйте указатель связан со словом «небезопасный».

Ниже приведен метод в классе `GradientBitmapPage`, который использует метод `GetPixels`. Обратите внимание на блок `unsafe`, охватывающий весь код с помощью указателя Byte:

```csharp
SKBitmap FillBitmapBytePtr(out string description, out int milliseconds)
{
    description = "GetPixels byte ptr";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    IntPtr pixelsAddr = bitmap.GetPixels();

    unsafe
    {
        for (int rep = 0; rep < REPS; rep++)
        {
            byte* ptr = (byte*)pixelsAddr.ToPointer();

            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    *ptr++ = (byte)(col);   // red
                    *ptr++ = 0;             // green
                    *ptr++ = (byte)(row);   // blue
                    *ptr++ = 0xFF;          // alpha
                }
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Когда переменная `ptr` сначала получается из метода `ToPointer`, она указывает на первый байт крайнего левого пикселя первой строки точечного рисунка. `for` циклы для `row` и `col` настраиваются таким образом, чтобы `ptr` можно было увеличивать с помощью оператора `++` после того, как задан каждый байт каждого пикселя. Для остальных 99 просматривает Пиксели, `ptr` должны быть возвращены к началу точечного рисунка.

Каждый пиксель — четыре числа байтов памяти, поэтому каждый байт должен устанавливаться отдельно. В коде предполагается, что байты находятся в порядке красного, зеленого, синего и альфа-канала, который согласуется с типом цвета `SKColorType.Rgba8888`. Как вы помните, что это тип цвета по умолчанию для iOS и Android, но не для универсальной платформы Windows. По умолчанию UWP создает точечные рисунки с типом цвета `SKColorType.Bgra8888`. По этой причине ожидать получение различных результатов на этой платформе!

Можно привести значение, возвращаемое из `ToPointer`, к `uint`ному указателю, а не `byte` указателю. Это позволяет пиксель для доступа в одной инструкции. Применение оператора `++` к этому указателю увеличивает его на четыре байта, чтобы указать на следующий пиксель:

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    SKBitmap FillBitmapUintPtr(out string description, out int milliseconds)
    {
        description = "GetPixels uint ptr";
        SKBitmap bitmap = new SKBitmap(256, 256);

        stopwatch.Restart();

        IntPtr pixelsAddr = bitmap.GetPixels();

        unsafe
        {
            for (int rep = 0; rep < REPS; rep++)
            {
                uint* ptr = (uint*)pixelsAddr.ToPointer();

                for (int row = 0; row < 256; row++)
                    for (int col = 0; col < 256; col++)
                    {
                        *ptr++ = MakePixel((byte)col, 0, (byte)row, 0xFF);
                    }
            }
        }

        milliseconds = (int)stopwatch.ElapsedMilliseconds;
        return bitmap;
    }
    ···
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    uint MakePixel(byte red, byte green, byte blue, byte alpha) =>
            (uint)((alpha << 24) | (blue << 16) | (green << 8) | red);
    ···
}
```

Пиксел задается с помощью метода `MakePixel`, который строит целочисленный пиксел от красного, зеленого, синего и альфа-компонентов. Помните, что формат `SKColorType.Rgba8888` имеет примерно пиксельный порядок следования байтов:

RR, GG BB AA

Но целое число, соответствующее этих байтов:

AABBGGRR

Соответствии с архитектурой с прямым порядком байтов первым сохраняется наименее значащим байтом целого числа. Этот метод `MakePixel` не будет правильно работать для точечных рисунков с типом цвета `Bgra8888`.

Метод `MakePixel` помечается параметром [`MethodImplOptions.AggressiveInlining`](xref:System.Runtime.CompilerServices.MethodImplOptions) , чтобы обеспечить компилятору возможность избегать создания отдельного метода, но вместо этого для компиляции кода, в котором вызывается метод. Это должно повысить производительность.

Интересно, что структура `SKColor` определяет явное преобразование из `SKColor` в целое число без знака, что означает, что можно создать `SKColor`ное значение, а преобразование в `uint` можно использовать вместо `MakePixel`:

```csharp
SKBitmap FillBitmapUintPtrColor(out string description, out int milliseconds)
{
    description = "GetPixels SKColor";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    IntPtr pixelsAddr = bitmap.GetPixels();

    unsafe
    {
        for (int rep = 0; rep < REPS; rep++)
        {
            uint* ptr = (uint*)pixelsAddr.ToPointer();

            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    *ptr++ = (uint)new SKColor((byte)col, 0, (byte)row);
                }
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Единственный вопрос: является целочисленным форматом `SKColor` значения в порядке `SKColorType.Rgba8888`ого типа цвета, типа `SKColorType.Bgra8888`ого цвета или того, что что-то другое? Ответ на этот вопрос должны стать доступными чуть ниже.

### <a name="the-setpixels-method"></a>Метод SetPixels

`SKBitmap` также определяет метод с именем [`SetPixels`](xref:SkiaSharp.SKBitmap.SetPixels(System.IntPtr)), который вызывается следующим образом:

```csharp
bitmap.SetPixels(intPtr);
```

Помните, что `GetPixels` получает `IntPtr` ссылающийся на блок памяти, используемый точечным рисунком для хранения пикселов. `SetPixels` вызов _заменяет_ этот блок памяти на блок памяти, на который ссылается `IntPtr`, заданный в качестве аргумента `SetPixels`. Точечный рисунок освобождает блок памяти, который использовался ранее. При следующем вызове `GetPixels` он получает набор блоков памяти с `SetPixels`.

На первый взгляд кажется, что `SetPixels` больше энергии и производительности, чем `GetPixels` хотя и менее удобным. С `GetPixels` получить блок памяти Bitmap и получить к нему доступ. В `SetPixels` вы выделяете и обращаетесь к некоторой памяти, а затем устанавливаете это как блок памяти Bitmap.

Но использование `SetPixels` предлагает отдельное синтаксические преимущества: оно позволяет обращаться к битовым точкам растрового изображения с помощью массива. Ниже приведен метод в `GradientBitmapPage`, демонстрирующий этот прием. Метод сначала определяет многомерный массив, соответствующих байтам пиксели. Первого измерения является строкой, во втором измерении — столбец и соответствует третьему измерению для каждого пикселя четырех компонентов:

```csharp
SKBitmap FillBitmapByteBuffer(out string description, out int milliseconds)
{
    description = "SetPixels byte buffer";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    byte[,,] buffer = new byte[256, 256, 4];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col, 0] = (byte)col;   // red
                buffer[row, col, 1] = 0;           // green
                buffer[row, col, 2] = (byte)row;   // blue
                buffer[row, col, 3] = 0xFF;        // alpha
            }

    unsafe
    {
        fixed (byte* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Затем, после заполнения массива пикселами, блок `unsafe` и инструкция `fixed` используются для получения указателя на байт, указывающего на этот массив. Затем этот указатель байтов можно привести к `IntPtr` для передачи в `SetPixels`.

Массив, созданный не должен быть массивом байтов. Это может быть массив целых чисел с помощью только двух измерений для строк и столбцов:

```csharp
SKBitmap FillBitmapUintBuffer(out string description, out int milliseconds)
{
    description = "SetPixels uint buffer";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    uint[,] buffer = new uint[256, 256];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col] = MakePixel((byte)col, 0, (byte)row, 0xFF);
            }

    unsafe
    {
        fixed (uint* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Метод `MakePixel` снова используется для объединения цветовых компонентов в 32-разрядный пиксель.

Для полноты вот тот же код, но с `SKColor` значением, которое преобразуется в целое число без знака:

```csharp
SKBitmap FillBitmapUintBufferColor(out string description, out int milliseconds)
{
    description = "SetPixels SKColor";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    uint[,] buffer = new uint[256, 256];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col] = (uint)new SKColor((byte)col, 0, (byte)row);
            }

    unsafe
    {
        fixed (uint* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

### <a name="comparing-the-techniques"></a>Сравнение методов

Конструктор страницы **цвета градиента** вызывает все восемь показанных выше методов и сохраняет результаты:

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    string[] descriptions = new string[8];
    SKBitmap[] bitmaps = new SKBitmap[8];
    int[] elapsedTimes = new int[8];

    SKCanvasView canvasView;

    public GradientBitmapPage ()
    {
        Title = "Gradient Bitmap";

        bitmaps[0] = FillBitmapSetPixel(out descriptions[0], out elapsedTimes[0]);
        bitmaps[1] = FillBitmapPixelsProp(out descriptions[1], out elapsedTimes[1]);
        bitmaps[2] = FillBitmapBytePtr(out descriptions[2], out elapsedTimes[2]);
        bitmaps[4] = FillBitmapUintPtr(out descriptions[4], out elapsedTimes[4]);
        bitmaps[6] = FillBitmapUintPtrColor(out descriptions[6], out elapsedTimes[6]);
        bitmaps[3] = FillBitmapByteBuffer(out descriptions[3], out elapsedTimes[3]);
        bitmaps[5] = FillBitmapUintBuffer(out descriptions[5], out elapsedTimes[5]);
        bitmaps[7] = FillBitmapUintBufferColor(out descriptions[7], out elapsedTimes[7]);

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ···
}
```

Конструктор завершается созданием `SKCanvasView` для вывода итоговых точечных рисунков. Обработчик `PaintSurface` делит свою поверхность на восемь прямоугольников и вызывает `Display` для отображения каждого из них:

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        int width = info.Width;
        int height = info.Height;

        canvas.Clear();

        Display(canvas, 0, new SKRect(0, 0, width / 2, height / 4));
        Display(canvas, 1, new SKRect(width / 2, 0, width, height / 4));
        Display(canvas, 2, new SKRect(0, height / 4, width / 2, 2 * height / 4));
        Display(canvas, 3, new SKRect(width / 2, height / 4, width, 2 * height / 4));
        Display(canvas, 4, new SKRect(0, 2 * height / 4, width / 2, 3 * height / 4));
        Display(canvas, 5, new SKRect(width / 2, 2 * height / 4, width, 3 * height / 4));
        Display(canvas, 6, new SKRect(0, 3 * height / 4, width / 2, height));
        Display(canvas, 7, new SKRect(width / 2, 3 * height / 4, width, height));
    }

    void Display(SKCanvas canvas, int index, SKRect rect)
    {
        string text = String.Format("{0}: {1:F1} msec", descriptions[index],
                                    (double)elapsedTimes[index] / REPS);

        SKRect bounds = new SKRect();

        using (SKPaint textPaint = new SKPaint())
        {
            textPaint.TextSize = (float)(12 * canvasView.CanvasSize.Width / canvasView.Width);
            textPaint.TextAlign = SKTextAlign.Center;
            textPaint.MeasureText("Tly", ref bounds);

            canvas.DrawText(text, new SKPoint(rect.MidX, rect.Bottom - bounds.Bottom), textPaint);
            rect.Bottom -= bounds.Height;
            canvas.DrawBitmap(bitmaps[index], rect, BitmapStretch.Uniform);
        }
    }
}
```

Чтобы разрешить компилятору оптимизировать код, эта страница была запущена в режиме **выпуска** . Вот эту страницу, на эмулятор iPhone 8 MacBook Pro, телефоне Nexus 5 Android и Surface Pro 3 под управлением Windows 10. Из-за различия оборудования избежать сравнения временных показателей производительности между устройствами, но вместо этого посмотреть относительное время на каждом устройстве:

[![Растровое изображение градиента](pixel-bits-images/GradientBitmap.png "Растровое изображение градиента")](pixel-bits-images/GradientBitmap-Large.png#lightbox)

Ниже приведена таблица, объединяющий времени выполнения в миллисекундах:

| API       | Тип данных | iOS  | Android | UWP  |
| --------- | --------- | ----:| -------:| ----:|
| SetPixel  |           | 3,17 |   10.77 | 3.49 |
| Перекрытых    |           | 0,32 |    1,23 | 0,07 |
| GetPixels | byte      | 0,09 |    0.24 | 0,10 |
|           | uint      | 0,06 |    0,26 | 0,05 |
|           | SKColor   | 0.29 |    0.99 | 0,07 |
| SetPixels | byte      | 1.33 |    6.78 | 0,11 |
|           | uint      | 0,14 |    0,69 | 0,06 |
|           | SKColor   | 0,35 |    1,93 | 0,10 |

Как и ожидалось, вызов `SetPixel` 65 536 раз является наименьшим еффеиЦиент способом установки пикселей растрового изображения. Заполнение массива `SKColor` и установка свойства `Pixels` гораздо лучше, и даже сравнение с некоторыми из `GetPixels` и `SetPixels` методов. Работа с `uint`ными значениями пикселей обычно выполняется быстрее, чем установка отдельных компонентов `byte` и преобразование значения `SKColor` в целое число без знака увеличивает нагрузку на процесс.

Это также интересно сравнить различные градиенты: верхних строк для каждой платформы совпадают и Показать градиента, как предполагалось. Это означает, что метод `SetPixel` и свойство `Pixels` правильно создают Пиксели из цветов независимо от базового формата пикселей.

Следующие две строки снимков экрана iOS и Android также одинаковы, что подтверждает, что небольшой `MakePixel` правильно определен для `Rgba8888`ого формата пикселей по умолчанию для этих платформ.

Нижняя строка снимков экрана iOS и Android находится в обратном направлении, что означает, что целое число без знака, полученное путем приведения `SKColor` значения, имеет вид:

AARRGGBB

Байты располагаются в порядке:

AA BB GG RR

Это `Bgra8888` порядок, а не порядок `Rgba8888`. Формат `Brga8888` используется по умолчанию для универсальной платформы Windows, поэтому градиенты в последней строке этого снимка экрана совпадают с первой строкой. Но средние две строки неверны, так как код, создающий эти точечные рисунки, предполагает `Rgba8888` упорядочения.

Если вы хотите использовать один и тот же код для доступа к битовым точкам на каждой платформе, можно явно создать `SKBitmap` с помощью формата `Rgba8888` или `Bgra8888`. Если необходимо привести `SKColor` значений к растровым пикселам, используйте `Bgra8888`.

## <a name="random-access-of-pixels"></a>Произвольный доступ пикселей

Методы `FillBitmapBytePtr` и `FillBitmapUintPtr` на странице **растрового изображения градиента** выгодны `for` циклов, предназначенных для последовательного заполнения растрового изображения, от верхней строки до нижней строки и в каждой строке слева направо. Пиксель может быть установлена с той же инструкции, которые увеличивается указатель.

Иногда бывает необходимо получить доступ к пиксели случайным образом, а не последовательно. Если вы используете `GetPixels` подход, необходимо вычислить указатели на основе строки и столбца. Это показано на странице **синуса** , которая создает растровое изображение, показывающее Радуга в виде одного цикла синуса.

Цвета радуги — проще всего создать с помощью модели цвета HSL (оттенок, насыщенность, яркость). Метод `SKColor.FromHsl` создает `SKColor` значение, используя значения оттенков в диапазоне от 0 до 360 (например, углы круга, но от красного и синего до зеленого и голубого цвета), а также значения насыщенности и яркости в диапазоне от 0 до 100. Для цветов радуги насыщенность должно быть присвоено более 100 и яркость к промежуточной точке 50.

**Радуга синуса** создает это изображение, перепроходя по строкам точечного рисунка, а затем прокручивать значения оттенков 360. Из каждого значения оттенка он вычисляет столбец точечного рисунка, который также зависит от значения синуса:

```csharp
public class RainbowSinePage : ContentPage
{
    SKBitmap bitmap;

    public RainbowSinePage()
    {
        Title = "Rainbow Sine";

        bitmap = new SKBitmap(360 * 3, 1024, SKColorType.Bgra8888, SKAlphaType.Unpremul);

        unsafe
        {
            // Pointer to first pixel of bitmap
            uint* basePtr = (uint*)bitmap.GetPixels().ToPointer();

            // Loop through the rows
            for (int row = 0; row < bitmap.Height; row++)
            {
                // Calculate the sine curve angle and the sine value
                double angle = 2 * Math.PI * row / bitmap.Height;
                double sine = Math.Sin(angle);

                // Loop through the hues
                for (int hue = 0; hue < 360; hue++)
                {
                    // Calculate the column
                    int col = (int)(360 + 360 * sine + hue);

                    // Calculate the address
                    uint* ptr = basePtr + bitmap.Width * row + col;

                    // Store the color value
                    *ptr = (uint)SKColor.FromHsl(hue, 100, 50);
                }
            }
        }

        // Create the SKCanvasView
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

Обратите внимание, что конструктор создает точечный рисунок на основе формата `SKColorType.Bgra8888`:

```csharp
bitmap = new SKBitmap(360 * 3, 1024, SKColorType.Bgra8888, SKAlphaType.Unpremul);
```

Это позволяет программе использовать преобразование значений `SKColor` в `uint` пикселях без необходимости беспокоиться. Хотя она не воспроизводит роль в этой конкретной программе, всякий раз при использовании преобразования `SKColor` для установки пикселей необходимо также указать `SKAlphaType.Unpremul`, так как `SKColor` не выполняет преобразование компонентов цвета по альфа-значению.

Затем конструктор использует метод `GetPixels` для получения указателя на первый пиксель точечного рисунка:

```csharp
uint* basePtr = (uint*)bitmap.GetPixels().ToPointer();
```

Для каждой конкретной строки и столбца значение offset должно быть добавлено в `basePtr`. Это смещение является строкой времени Ширина растрового изображения, а также столбец:

```csharp
uint* ptr = basePtr + bitmap.Width * row + col;
```

Значение `SKColor` хранится в памяти с помощью этого указателя:

```csharp
*ptr = (uint)SKColor.FromHsl(hue, 100, 50);
```

В обработчике `PaintSurface` `SKCanvasView`растровое изображение растягивается для заполнения отображаемой области:

[![Синус Радуга](pixel-bits-images/RainbowSine.png "Синус Радуга")](pixel-bits-images/RainbowSine-Large.png#lightbox)

## <a name="from-one-bitmap-to-another"></a>Из одного точечного рисунка в другой

Очень многие задачи обработки изображений в том числе изменить пикселей, так как они передаются из одного точечного рисунка в другую. Этот метод показан на странице **настройки цвета** . Страница загружает один из ресурсов точечного рисунка, а затем позволяет изменить образ с помощью трех `Slider` представлений:

[![Настройка цвета](pixel-bits-images/ColorAdjustment.png "Настройка цвета")](pixel-bits-images/ColorAdjustment-Large.png#lightbox)

Для каждого цвета пикселя первый `Slider` добавляет значение от 0 до 360 к оттенок, а затем использует оператор остатка от деления для сохранения результата между 0 и 360, фактически сдвигя цвета вдоль спектра (как показано на снимке экрана UWP). Вторая `Slider` позволяет выбрать мультипликативные коэффициент между 0,5 и 2 для применения к насыщенности, а третий `Slider` для яркости, как показано на снимке экрана Android.

Программа поддерживает два точечных рисунка: исходный точечный рисунок с именем `srcBitmap` и скорректированный точечный рисунок назначения с именем `dstBitmap`. При каждом перемещении `Slider` программа вычисляет все новые Пиксели в `dstBitmap`. Конечно, пользователи будут экспериментировать, перемещая представления `Slider` очень быстро, так что вы хотите обеспечить максимальную производительность, которую вы можете управлять. Это включает метод `GetPixels` как для исходного, так и для целевого растрового изображения.

Страница « **Коррекция цвета** » не управляет форматом цвета исходного и конечного точечных рисунков. Вместо этого он содержит немного другую логику для `SKColorType.Rgba8888` и `SKColorType.Bgra8888` форматов. Источник и назначение могут быть различные форматы и программа по-прежнему будет работать.

Ниже приведена программа, за исключением крайне важного метода `TransferPixels`, который передает Пиксели источник в место назначения. Конструктор задает `dstBitmap` равное `srcBitmap`. Обработчик `PaintSurface` отображает `dstBitmap`:

```csharp
public partial class ColorAdjustmentPage : ContentPage
{
    SKBitmap srcBitmap =
        BitmapExtensions.LoadBitmapResource(typeof(FillRectanglePage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    SKBitmap dstBitmap;

    public ColorAdjustmentPage()
    {
        InitializeComponent();

        dstBitmap = new SKBitmap(srcBitmap.Width, srcBitmap.Height);
        OnSliderValueChanged(null, null);
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        float hueAdjust = (float)hueSlider.Value;
        hueLabel.Text = $"Hue Adjustment: {hueAdjust:F0}";

        float saturationAdjust = (float)Math.Pow(2, saturationSlider.Value);
        saturationLabel.Text = $"Saturation Adjustment: {saturationAdjust:F2}";

        float luminosityAdjust = (float)Math.Pow(2, luminositySlider.Value);
        luminosityLabel.Text = $"Luminosity Adjustment: {luminosityAdjust:F2}";

        TransferPixels(hueAdjust, saturationAdjust, luminosityAdjust);
        canvasView.InvalidateSurface();
    }
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(dstBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

Обработчик `ValueChanged` для представлений `Slider` вычисляет значения корректировки и вызывает `TransferPixels`.

Весь метод `TransferPixels` помечается как `unsafe`. Он начинает с получения байтовые указатели для битов пикселов точечных рисунков, оба и затем циклически проходит все строки и столбцы. Из исходного растрового изображения метод получает четыре байта для каждого пикселя. Они могут находиться либо в `Rgba8888`, либо в порядке `Bgra8888`. Проверка типа цвета позволяет создать значение `SKColor`. Затем компоненты HSL извлекаются, корректируются и используются для повторного создания значения `SKColor`. В зависимости от того, является ли точечный рисунок назначения `Rgba8888` или `Bgra8888`, байты хранятся в целевом битмп:

```csharp
public partial class ColorAdjustmentPage : ContentPage
{
    ···
    unsafe void TransferPixels(float hueAdjust, float saturationAdjust, float luminosityAdjust)
    {
        byte* srcPtr = (byte*)srcBitmap.GetPixels().ToPointer();
        byte* dstPtr = (byte*)dstBitmap.GetPixels().ToPointer();

        int width = srcBitmap.Width;       // same for both bitmaps
        int height = srcBitmap.Height;

        SKColorType typeOrg = srcBitmap.ColorType;
        SKColorType typeAdj = dstBitmap.ColorType;

        for (int row = 0; row < height; row++)
        {
            for (int col = 0; col < width; col++)
            {
                // Get color from original bitmap
                byte byte1 = *srcPtr++;         // red or blue
                byte byte2 = *srcPtr++;         // green
                byte byte3 = *srcPtr++;         // blue or red
                byte byte4 = *srcPtr++;         // alpha

                SKColor color = new SKColor();

                if (typeOrg == SKColorType.Rgba8888)
                {
                    color = new SKColor(byte1, byte2, byte3, byte4);
                }
                else if (typeOrg == SKColorType.Bgra8888)
                {
                    color = new SKColor(byte3, byte2, byte1, byte4);
                }

                // Get HSL components
                color.ToHsl(out float hue, out float saturation, out float luminosity);

                // Adjust HSL components based on adjustments
                hue = (hue + hueAdjust) % 360;
                saturation = Math.Max(0, Math.Min(100, saturationAdjust * saturation));
                luminosity = Math.Max(0, Math.Min(100, luminosityAdjust * luminosity));

                // Recreate color from HSL components
                color = SKColor.FromHsl(hue, saturation, luminosity);

                // Store the bytes in the adjusted bitmap
                if (typeAdj == SKColorType.Rgba8888)
                {
                    *dstPtr++ = color.Red;
                    *dstPtr++ = color.Green;
                    *dstPtr++ = color.Blue;
                    *dstPtr++ = color.Alpha;
                }
                else if (typeAdj == SKColorType.Bgra8888)
                {
                    *dstPtr++ = color.Blue;
                    *dstPtr++ = color.Green;
                    *dstPtr++ = color.Red;
                    *dstPtr++ = color.Alpha;
                }
            }
        }
    }
    ···
}
```

Вполне вероятно, производительность этого метода может быть еще более повысить, создавая отдельные методы для различных сочетаний значений типы цвета исходного и конечного точечных рисунков что избежать проверки типа для каждого пикселя. Другой вариант — использовать несколько циклов `for` для переменной `col` в зависимости от типа цвета.

## <a name="posterization"></a>Постеризации

Другим распространенным заданием, включающим доступ к битам пикселей, является _Афиша_. Номер, если цвета кодируются в пикселей растрового изображения уменьшается таким образом, результат имеет примерно плакат нарисованные от руки с помощью ограниченной цветовую палитру.

На странице **постеризе** этот процесс выполняется на одном из образов обезьян:

```csharp
public class PosterizePage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(FillRectanglePage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public PosterizePage()
    {
        Title = "Posterize";

        unsafe
        {
            uint* ptr = (uint*)bitmap.GetPixels().ToPointer();
            int pixelCount = bitmap.Width * bitmap.Height;

            for (int i = 0; i < pixelCount; i++)
            {
                *ptr++ &= 0xE0E0E0FF;
            }
        }

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
        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform;
    }
}
```

Код в конструктор обращается к каждого пикселя, выполняет побитовую операцию AND со значением 0xE0E0E0FF и затем сохраняет результат обратно в битовой карте. Значения 0xE0E0E0FF отслеживает старшие 3 разряда каждого компонента цвета и задает младшие 5 битов 0. Вместо 2<sup>24</sup> или 16 777 216 цветов битовая карта сокращается до 2<sup>9</sup> или 512 цветов:

[![постеризе](pixel-bits-images/Posterize.png "Постеризация")](pixel-bits-images/Posterize-Large.png#lightbox)

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
