---
title: Основы точечного рисунка в SkiaSharp
description: В этой статье объясняется, как загружать точечные рисунки в SkiaSharp из различных источников и отображать их в приложениях Xamarin. Forms, а также демонстрируется пример кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 32C95DFF-9065-42D7-966C-D3DBD16906B3
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
ms.openlocfilehash: 4aa73e1cba3f970396e5a52679372a160f47f7af
ms.sourcegitcommit: 3bf02ecac9a8855779e07eb1e7e27abb9fc38934
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/29/2019
ms.locfileid: "74658272"
---
# <a name="bitmap-basics-in-skiasharp"></a>Основы точечного рисунка в SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Загрузка растровых изображений из различных источников и их отображение._

Поддержка точечных рисунков в SkiaSharp довольно обширна. В этой статье рассматриваются только основные сведения &mdash; загрузке растровых изображений и способах их отображения:

![](bitmaps-images/basicbitmaps-small.png "The display of two bitmaps")

Более глубокое изучение точечных рисунков можно найти в разделе [SkiaSharp Bitmaps](../bitmaps/index.md).

Точечный рисунок SkiaSharp — это объект типа [`SKBitmap`](xref:SkiaSharp.SKBitmap). Существует множество способов создания точечного рисунка, но эта статья ограничена методом [`SKBitmap.Decode`](xref:SkiaSharp.SKBitmap.Decode(System.IO.Stream)) , который загружает точечный рисунок из объекта .NET `Stream`.

На странице **основные растровые изображения** в программе **скиашарпформсдемос** показано, как загружать точечные рисунки из трех различных источников:

- Из Интернета
- Из ресурса, внедренного в исполняемый файл
- Из библиотеки фотографий пользователя

Три `SKBitmap` объектов для этих трех источников определены как поля в классе [`BasicBitmapsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/BasicBitmapsPage.cs) :

```csharp
public class BasicBitmapsPage : ContentPage
{
    SKCanvasView canvasView;
    SKBitmap webBitmap;
    SKBitmap resourceBitmap;
    SKBitmap libraryBitmap;

    public BasicBitmapsPage()
    {
        Title = "Basic Bitmaps";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
        ...
    }
    ...
}
```

## <a name="loading-a-bitmap-from-the-web"></a>Загрузка точечного рисунка из Интернета

Чтобы загрузить точечный рисунок на основе URL-адреса, можно использовать класс [`HttpClient`](/dotnet/api/system.net.http.httpclient?view=netstandard-2.0) . Следует создать только один экземпляр `HttpClient` и использовать его повторно, поэтому сохраните его как поле:

```csharp
HttpClient httpClient = new HttpClient();
```

При использовании `HttpClient` с приложениями iOS и Android необходимо задать свойства проекта, как описано в документах по **[протоколу tls 1,2](~/cross-platform/app-fundamentals/transport-layer-security.md)** .

Так как лучше использовать оператор `await` с `HttpClient`, код не может быть выполнен в конструкторе `BasicBitmapsPage`. Вместо этого он является частью переопределения `OnAppearing`. Здесь URL-адрес указывает на область на веб-сайте Xamarin с некоторыми примерами точечных рисунков. Пакет на веб-сайте допускает добавление спецификации для изменения размера точечного рисунка до определенной ширины:

```csharp
protected override async void OnAppearing()
{
    base.OnAppearing();

    // Load web bitmap.
    string url = "https://developer.xamarin.com/demo/IMG_3256.JPG?width=480";

    try
    {
        using (Stream stream = await httpClient.GetStreamAsync(url))
        using (MemoryStream memStream = new MemoryStream())
        {
            await stream.CopyToAsync(memStream);
            memStream.Seek(0, SeekOrigin.Begin);

            webBitmap = SKBitmap.Decode(memStream);
            canvasView.InvalidateSurface();
        };
    }
    catch
    {
    }
}
```

Операционная система Android создает исключение при использовании `Stream`, возвращаемого из `GetStreamAsync` в методе `SKBitmap.Decode`, так как он выполняет длительную операцию в основном потоке. По этой причине содержимое файла точечного рисунка копируется в объект `MemoryStream` с помощью `CopyToAsync`.

Статический метод `SKBitmap.Decode` отвечает за декодирование растровых файлов. Он работает с форматами битов JPEG, PNG и GIF и сохраняет результаты во внутреннем формате SkiaSharp. На этом этапе `SKCanvasView` необходимо сделать недействительным, чтобы обработчик `PaintSurface` может обновить отображение.

## <a name="loading-a-bitmap-resource"></a>Загрузка ресурса точечного рисунка

С точки зрения кода самым простым подходом к загрузке точечных рисунков является включение ресурса точечного рисунка непосредственно в приложение. Программа **скиашарпформсдемос** включает папку с именем **Media** , содержащую несколько файлов точечных рисунков, включая один файл **обезьян. png**. Для точечных рисунков, хранящихся как ресурсы программы, необходимо использовать диалоговое окно **свойств** , чтобы дать файлу **действие сборки** **внедренного ресурса**!

Каждый внедренный ресурс имеет *идентификатор ресурса* , состоящий из имени проекта, папки и имени файла, Соединенных точками: **скиашарпформсдемос. Media. обезьян. png**. Доступ к этому ресурсу можно получить, указав идентификатор ресурса в качестве аргумента метода [`GetManifestResourceStream`](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String)) класса [`Assembly`](xref:System.Reflection.Assembly) :

```csharp
string resourceID = "SkiaSharpFormsDemos.Media.monkey.png";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
{
    resourceBitmap = SKBitmap.Decode(stream);
}
```

Этот объект `Stream` может быть передан непосредственно методу `SKBitmap.Decode`.

## <a name="loading-a-bitmap-from-the-photo-library"></a>Загрузка точечного рисунка из библиотеки фотографий

Кроме того, пользователь может загрузить фотографию из библиотеки изображений устройства. Это средство не предоставляется в Xamarin. Forms. Для задания требуется служба зависимостей, например, описанная в статье о [выборе фотографии из библиотеки рисунков](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md).

Файл **IPhotoLibrary.CS** в проекте **скиашарпформсдемос** и три файла **PhotoLibrary.CS** в проектах платформы были адаптированы из этой статьи. Кроме того, файл **MainActivity.CS** для Android был изменен, как описано в статье, и проекту iOS предоставлено разрешение на доступ к библиотеке фотографий с двумя строками в нижней части файла **info. plist** .

Конструктор `BasicBitmapsPage` добавляет `TapGestureRecognizer` в `SKCanvasView`, чтобы получать уведомления об отводах. При получении касания обработчик `Tapped` получает доступ к службе зависимостей средства выбора изображений и вызывает `PickPhotoAsync`. Если возвращается объект `Stream`, он передается методу `SKBitmap.Decode`:

```csharp
// Add tap gesture recognizer
TapGestureRecognizer tapRecognizer = new TapGestureRecognizer();
tapRecognizer.Tapped += async (sender, args) =>
{
    // Load bitmap from photo library
    IPhotoLibrary photoLibrary = DependencyService.Get<IPhotoLibrary>();

    using (Stream stream = await photoLibrary.PickPhotoAsync())
    {
        if (stream != null)
        {
            libraryBitmap = SKBitmap.Decode(stream);
            canvasView.InvalidateSurface();
        }
    }
};
canvasView.GestureRecognizers.Add(tapRecognizer);
```

Обратите внимание, что обработчик `Tapped` также вызывает метод `InvalidateSurface` объекта `SKCanvasView`. При этом создается новый вызов обработчика `PaintSurface`.

## <a name="displaying-the-bitmaps"></a>Отображение растровых изображений

Обработчику `PaintSurface` необходимо отобразить три точечных рисунка. Обработчик предполагает, что телефон находится в книжной ориентации и разделяет холст по вертикали на три равных части.

Первое растровое изображение отображается с простейшим методом [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,System.Single,System.Single,SkiaSharp.SKPaint)) . Необходимо указать только координаты X и Y, где расположен верхний левый угол точечного рисунка:

```csharp
public void DrawBitmap (SKBitmap bitmap, Single x, Single y, SKPaint paint = null)
```

Хотя параметр `SKPaint` определен, он имеет значение по умолчанию `null` и его можно игнорировать. Пикселы точечного рисунка просто передаются в Пиксели области отображения с сопоставлением «один к одному». Вы увидите приложение для этого `SKPaint` аргумента в следующем разделе, посвященном [**прозрачности SkiaSharp**](transparency.md).

Программа может получить размеры растрового изображения в пикселях с помощью свойств [`Width`](xref:SkiaSharp.SKBitmap.Width) и [`Height`](xref:SkiaSharp.SKBitmap.Height) . Эти свойства позволяют программе вычислять координаты для позиционирования точечного рисунка в центре верхнего левого края холста:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    if (webBitmap != null)
    {
        float x = (info.Width - webBitmap.Width) / 2;
        float y = (info.Height / 3 - webBitmap.Height) / 2;
        canvas.DrawBitmap(webBitmap, x, y);
    }
    ...
}
```

Два других растровых изображения отображаются с версией [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKPaint)) с параметром `SKRect`:

```csharp
public void DrawBitmap (SKBitmap bitmap, SKRect dest, SKPaint paint = null)
```

Третья версия [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKRect,SkiaSharp.SKPaint)) имеет два аргумента `SKRect` для указания прямоугольного подмножества отображаемого точечного рисунка, но эта версия не используется в этой статье.

Ниже приведен код для вывода растрового изображения, загруженного из битовой карты внедренного ресурса.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    if (resourceBitmap != null)
    {
        canvas.DrawBitmap(resourceBitmap,
            new SKRect(0, info.Height / 3, info.Width, 2 * info.Height / 3));
    }
    ...
}
```

Точечный рисунок растягивается по размерам прямоугольника, поэтому обезьяна будет горизонтально растянута на этих снимках экрана:

[![](bitmaps-images/basicbitmaps-small.png "A triple screenshot of the Basic Bitmaps page")](bitmaps-images/basicbitmaps-large.png#lightbox "A triple screenshot of the Basic Bitmaps page")

Третий образ &mdash;, который можно увидеть, только если вы запускаете программу и загрузили фотографию из собственной библиотеки изображений &mdash; также отображается внутри прямоугольника, но его расположение и размер изменяются для поддержания пропорций растрового изображения. Это вычисление немного сложнее, поскольку для него требуется Вычисление коэффициента масштабирования на основе размера точечного рисунка и прямоугольника назначения, а также центрирование прямоугольника в этой области:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    if (libraryBitmap != null)
    {
        float scale = Math.Min((float)info.Width / libraryBitmap.Width,
                               info.Height / 3f / libraryBitmap.Height);

        float left = (info.Width - scale * libraryBitmap.Width) / 2;
        float top = (info.Height / 3 - scale * libraryBitmap.Height) / 2;
        float right = left + scale * libraryBitmap.Width;
        float bottom = top + scale * libraryBitmap.Height;
        SKRect rect = new SKRect(left, top, right, bottom);
        rect.Offset(0, 2 * info.Height / 3);

        canvas.DrawBitmap(libraryBitmap, rect);
    }
    else
    {
        using (SKPaint paint = new SKPaint())
        {
            paint.Color = SKColors.Blue;
            paint.TextAlign = SKTextAlign.Center;
            paint.TextSize = 48;

            canvas.DrawText("Tap to load bitmap",
                info.Width / 2, 5 * info.Height / 6, paint);
        }
    }
}
```

Если растровое изображение из библиотеки рисунков еще не загружено, то блок `else` отображает некоторый текст, предлагающий пользователю выбрать экран.

Вы можете отображать точечные рисунки с различными степенями прозрачности, а следующая статья по [**прозрачности SkiaSharp**](transparency.md) описывает, как это делать.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [Выбор фотографии в библиотеке рисунков](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
