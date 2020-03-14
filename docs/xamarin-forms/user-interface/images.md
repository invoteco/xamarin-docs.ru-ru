---
title: Образы в Xamarin.Forms
description: Образы могут совместно использоваться платформах с помощью Xamarin.Forms, могут быть загружены специально для каждой платформы, или их можно загрузить для отображения.
ms.prod: xamarin
ms.assetid: C025AB53-05CC-49BA-9815-75D6DF9E40B7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/04/2019
ms.openlocfilehash: 255d3f2f532e4899b1a890405af942a7ca2da8ea
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305711"
---
# <a name="images-in-xamarinforms"></a>Образы в Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages)

_Образы можно совместно использовать на разных платформах с помощью Xamarin. Forms, они могут быть загружены специально для каждой платформы, а также могут быть загружены для просмотра._

Образы являются одной из важнейших приложений навигации, удобство использования и фирменной символики. Приложения Xamarin.Forms должны иметь возможность совместного использования образов на всех платформах, но также может отображать различные изображения на каждой платформе.

Образы платформы также требуются для значки и экраны-заставки; они должны быть настроены отдельно для каждой платформы.

## <a name="display-images"></a>Отображение изображений

Xamarin. Forms использует представление [`Image`](xref:Xamarin.Forms.Image) для отображения изображений на странице. Он имеет два важных свойства:

- [`Source`](xref:Xamarin.Forms.Image.Source) — экземпляр [`ImageSource`](xref:Xamarin.Forms.ImageSource) , файл, URI или ресурс, который задает отображаемое изображение.
- [`Aspect`](xref:Xamarin.Forms.Image.Aspect) — как задать размер изображения в пределах границ, в которых он отображается (растяжение, кадрирование или леттербокс).

[`ImageSource`](xref:Xamarin.Forms.ImageSource) экземпляры могут быть получены с помощью статических методов для каждого типа источника изображения:

- [`FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String)) — требуется имя файла или путь к файлу, который можно разрешить на каждой платформе.
- для [`FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) требуется объект URI, например.  `new Uri("http://server.com/image.jpg")`.
- [`FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) — требуется идентификатор ресурса для файла изображения, внедренного в проект библиотеки приложения или .NET Standard, с **действием сборки: EmbeddedResource**.
- [`FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream})) — требуется поток, поставляющий данные изображения.

Свойство [`Aspect`](xref:Xamarin.Forms.Image.Aspect) определяет, как изображение будет масштабироваться в соответствии с отображаемой областью:

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) — растягивает изображение, чтобы полностью и точно заполнить отображаемую область. Это может привести в образе искажен.
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) — обрезает изображение таким образом, чтобы оно заполнило область экрана при сохранении аспекта (IE. без искажений).
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) -леттербоксес изображение (при необходимости), чтобы весь изображение поместилось в область экрана, с добавлением пробела в верхнюю или нижнюю часть или на сторону в зависимости от того, является ли изображение широким или максимальным.

Образы могут загружаться из [локального файла](#local-images), [внедренного ресурса](#embedded-images), [скачанного](#download-images)или загруженного из потока. Кроме того, значки шрифтов могут отображаться [`Image`ным](xref:Xamarin.Forms.Image) представлением путем указания данных значка шрифта в объекте `FontImageSource`. Дополнительные сведения см. в разделе [Отображение значков шрифтов](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons) в пошаговом окне " [шрифты](~/xamarin-forms/user-interface/text/fonts.md) ".

## <a name="local-images"></a>Локальные образы

Можно добавить в каждый проект приложения и ссылок в коде Xamarin.Forms общих файлов изображений. Этот метод распространения является обязательным для изображений, специфических для платформы, например при использовании разных разрешений на различных платформах или немного разных вариантах изображения.

Чтобы использовать один образ во всех приложениях, *на каждой платформе необходимо использовать*одно и то же имя файла, которое должно быть допустимым именем ресурса Android (только строчные буквы, цифры, символ подчеркивания и период).

- **iOS** — предпочтительный способ управления и поддержки образов с iOS 9 — использование **наборов образов каталога активов**, которые должны содержать все версии образа, необходимые для поддержки различных устройств и коэффициентов масштабирования для приложения. Дополнительные сведения см. [в разделе Добавление образов в набор изображений каталога активов](~/ios/app-fundamentals/images-icons/displaying-an-image.md).
- Образы на устройстве **Android** в каталоге **Resources/Draw** с **действием сборки: AndroidResource**. Также можно указать версии изображения с высоким и низким разрешением (в подкаталогах **ресурсов** с соответствующим именем, например, **Draw-лдпи**, **Draw-HDPI**и **Draw-xhdpi**).
- **Универсальная платформа Windows (UWP)** — размещение изображений в корневом каталоге приложения с **действием сборки: содержимое**.

> [!IMPORTANT]
> До версии iOS 9 образы обычно помещаются в папку **Resources** с **действием сборки: BundleResource**. Тем не менее этот метод работы с изображениями в приложение iOS является устаревшим компанией Apple. Дополнительные сведения см. в разделе [размеры и имена файлов образов](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Выполнение этих правил именования файлов и размещения предоставляет следующий XAML для загрузки и отображения изображения на всех платформах:

```xaml
<Image Source="waterfront.jpg" />
```

Эквивалент C# код выглядит следующим образом:

```csharp
var image = new Image { Source = "waterfront.jpg" };
```

На следующих снимках экрана показано результат отображения локальный образ на каждой платформе:

[![пример приложения, отображающий локальный образ](images-images/local-sml.png)](images-images/local.png#lightbox)

Для большей гибкости свойство `Device.RuntimePlatform` можно использовать для выбора другого файла изображения или пути для некоторых или всех платформ, как показано в следующем примере кода:

```csharp
image.Source = Device.RuntimePlatform == Device.Android 
                ? ImageSource.FromFile("waterfront.jpg") 
                : ImageSource.FromFile("Images/waterfront.jpg");
```

> [!IMPORTANT]
> Чтобы использовать это имя образа на всех платформах имя должно быть допустимым на всех платформах. Android прорисовываемых ресурсов имеют ограничения по именованию – допускаются только строчные буквы, цифры, подчеркивания и период — и для кросс платформенной совместимости это необходимо выполнить на всех остальных платформах слишком. Например, имя файла **набережная. png** соответствует правилам, но примеры недопустимых имен файлов: "вода Front. png", "набережная. png", "Ватер-фронт. png" и "вåтерфронт. png".

### <a name="native-resolutions-retina-and-high-dpi"></a>Собственные разрешения (Retina и высокое разрешение)

для Android, iOS и универсальной платформы Windows включают поддержку разрешение другой образ, где операционная система выбирает подходящий образ во время выполнения в зависимости от возможностей устройства. Xamarin.Forms использует интерфейсы API собственных платформ для загрузки локальных образов, поэтому он автоматически поддерживает альтернативные способы их устранения, если файлы правильно с именем и находится в проекте.

Предпочтительный способ управления образами, поскольку iOS 9, перетащите изображения для каждого разрешения, требуется набор изображений каталога соответствующие средства. Дополнительные сведения см. [в разделе Добавление образов в набор изображений каталога активов](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

До выпуска iOS 9 версии образа Retina можно поместить в папку **Resources (ресурсы** ) два и три раза в разрешение с **@2x** или **@3x** суффиксами по имени файла перед расширением файла (например, **myimage@2x.png** ). Тем не менее этот метод работы с изображениями в приложение iOS является устаревшим компанией Apple. Дополнительные сведения см. в разделе [размеры и имена файлов образов](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Образы альтернативного разрешения Android должны размещаться в [каталогах с особыми именами](https://developer.android.com/guide/practices/screens_support.html) в проекте Android, как показано на следующем снимке экрана:

[![расположение образа с несколькими разрешениями для Android](images-images/xs-highdpisolution-sml.png)](images-images/xs-highdpisolution.png#lightbox)

Имена файлов образов UWP можно дополнить [суффиксом `.scale-xxx` перед расширением файла](https://docs.microsoft.com/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast), где `xxx` — это процент масштабирования, применяемого к ресурсу, например **MyImage. Scale-200. png**. На образы можно ссылаться в коде или коде XAML без модификатора Scale, например, с помощью **MyImage. png**. Платформа выберет ближайшего масштаб соответствующего ресурса, исходя из текущего отображения точек на ДЮЙМ.

### <a name="additional-controls-that-display-images"></a>Дополнительные элементы управления, отображающие изображения

Некоторые элементы управления имеют свойства, которые отображают изображения, такие как:

- [`Button`](xref:Xamarin.Forms.Button) имеет свойство [`ImageSource`](xref:Xamarin.Forms.Button.ImageSource) , для которого можно задать точечный рисунок, отображаемый на `Button`. Дополнительные сведения см. в разделе [Использование точечных рисунков с кнопками](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons).
- [`ImageButton`](xref:Xamarin.Forms.Button) имеет свойство [`Source`](xref:Xamarin.Forms.ImageButton.Source) , которое может быть задано для изображения, отображаемого в `ImageButton`. Дополнительные сведения см. [в разделе Установка источника изображения](~/xamarin-forms/user-interface/imagebutton.md#setting-the-image-source).
- [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) имеет свойство [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource) , которое можно задать для изображения, загружаемого из файла, внедренного ресурса, URI или потока.
- [`ImageCell`](xref:Xamarin.Forms.ImageCell) имеет свойство [`ImageSource`](xref:Xamarin.Forms.ImageCell.ImageSource) , для которого можно задать изображение, полученное из файла, внедренного ресурса, URI или потока.
- [`Page`](xref:Xamarin.Forms.Page). Любой тип страницы, производный от `Page`, имеет [`IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource) и [`BackgroundImageSource`](xref:Xamarin.Forms.Page.BackgroundImageSource) свойства, которым можно назначить файл, внедренный ресурс, URI или поток. При определенных обстоятельствах, например, когда [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) отображает [`ContentPage`](xref:Xamarin.Forms.ContentPage), значок будет отображаться, если он поддерживается платформой.

  > [!IMPORTANT]
  > В iOS свойство [`Page.IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource) не может быть заполнено изображением в наборе изображений каталога активов. Вместо этого Загрузите изображения значков для свойства `Page.IconImageSource` из файла, внедренного ресурса, URI или потока.

## <a name="embedded-images"></a>Внедренные изображения

Внедренные изображения также поставляются с приложением (например, локальных образов), но вместо копирования образа в структуре файлов каждого приложения изображение файл внедряется в сборку как ресурс. Этот метод распространения образов рекомендуется использовать при идентичных образы используются на каждой платформе и хорошо подходит для создания компонентов, как изображение входит в состав код.

Внедрение изображения в проекте, щелкните правой кнопкой для добавления новых элементов и выберите образ/s, который вы хотите добавить. По умолчанию для образа будет задано **действие сборки: нет**; необходимо задать для **действия сборки: EmbeddedResource**.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![задать для действия сборки внедренный ресурс](images-images/vs-buildaction-sml.png)](images-images/vs-buildaction.png#lightbox)

**Действие сборки** можно просмотреть и изменить в окне **свойств** файла.

В этом примере ИДЕНТИФИКАТОРом ресурса является **воркингвисимажес. пляже. jpg**.
Интегрированная среда разработки создала это значение по умолчанию путем сцепления **пространства имен по умолчанию** для этого проекта с именем файла, используя точку (.) между каждым значением.
<!-- https://msdn.microsoft.com/library/ms950960.aspx -->

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

![](images-images/xs-buildaction.png "Set Build Action: EmbeddedResource")

**Действие сборки** также можно просмотреть и изменить на панели **свойств** файла.
На этой панели отображается **идентификатор ресурса** , используемый для ссылки на ресурс в коде. На следующем снимке экрана **идентификатор ресурса** — **воркингвисимажес. пляже. jpg**.
Интегрированная среда разработки создала это значение по умолчанию путем сцепления **пространства имен по умолчанию** для этого проекта с именем файла, используя точку (.) между каждым значением.
Этот идентификатор можно изменить на панели **свойств** , но для этих примеров будет использоваться значение **воркингвисимажес. пляже. jpg** .

[Панель свойств ![внедренных ресурсов](images-images/xs-embeddedproperties-sml.png)](images-images/xs-embeddedproperties.png#lightbox)

-----

Если поместить внедренные изображения в папки в проекте, имена папок также разделенных точками (.) в идентификатор ресурса. Перемещение образа **пляже. jpg** в папку с именем **MyImages** приведет к появлению идентификатора ресурса **воркингвисимажес. MyImages. пляже. jpg** .

Код для загрузки внедренного изображения просто передает **идентификатор ресурса** в метод [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) , как показано ниже:

```csharp
var embeddedImage = new Image { 
      Source = ImageSource.FromResource(
        "WorkingWithImages.beach.jpg", 
        typeof(EmbeddedImages).GetTypeInfo().Assembly
      ) };
```

> [!NOTE]
> Для поддержки отображения внедренных изображений в режиме выпуска на универсальная платформа Windows необходимо использовать перегрузку `ImageSource.FromResource`, указывающую исходную сборку для поиска изображения.

В настоящее время отсутствует неявное преобразование для идентификаторов ресурсов. Вместо этого для загрузки внедренных изображений необходимо использовать [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) или `new ResourceImageSource()`.

На следующих снимках экрана показано результат отображения внедренного изображения на каждой платформе:

[![пример приложения, отображающий внедренное изображение](images-images/resource-sml.png)](images-images/resource.png#lightbox)

### <a name="xaml"></a>XAML

Поскольку для `ResourceImageSource`не существует встроенного преобразователя типов `string`, эти типы изображений не могут быть загружены в собственном коде XAML. Вместо этого для загрузки изображений с помощью **идентификатора ресурса** , УКАЗАННОГО в XAML, можно написать простое расширение разметки XAML.

```csharp
[ContentProperty (nameof(Source))]
public class ImageResourceExtension : IMarkupExtension
{
 public string Source { get; set; }

 public object ProvideValue (IServiceProvider serviceProvider)
 {
   if (Source == null)
   {
     return null;
   }

   // Do your translation lookup here, using whatever method you require
   var imageSource = ImageSource.FromResource(Source, typeof(ImageResourceExtension).GetTypeInfo().Assembly);

   return imageSource;
 }
}
```

> [!NOTE]
> Для поддержки отображения внедренных изображений в режиме выпуска на универсальная платформа Windows необходимо использовать перегрузку `ImageSource.FromResource`, указывающую исходную сборку для поиска изображения.

Чтобы использовать это расширение, добавьте в XAML настраиваемый `xmlns`, используя правильное пространство имен и значения сборки для проекта. Затем можно задать источник изображения, используя следующий синтаксис: `{local:ImageResource WorkingWithImages.beach.jpg}`. Ниже приведен полный пример XAML:

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage
   xmlns="http://xamarin.com/schemas/2014/forms"
   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
   xmlns:local="clr-namespace:WorkingWithImages;assembly=WorkingWithImages"
   x:Class="WorkingWithImages.EmbeddedImagesXaml">
 <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
   <!-- use a custom Markup Extension -->
   <Image Source="{local:ImageResource WorkingWithImages.beach.jpg}" />
 </StackLayout>
</ContentPage>
```

### <a name="troubleshoot-embedded-images"></a>Устранение неполадок внедренных образов

#### <a name="debug-code"></a>Отладка кода

Так как иногда сложно понять, почему не загружаемых ресурсов определенного образа, следующий код отладки можно временно добавляется приложения чтобы убедиться, что ресурсы настроены правильно. Он выводит все известные ресурсы, внедренные в данную сборку, в **консоль** , чтобы помочь в отладке проблем загрузки ресурсов.

```csharp
using System.Reflection;
// ...
// NOTE: use for debugging, not in released app code!
var assembly = typeof(EmbeddedImages).GetTypeInfo().Assembly;
foreach (var res in assembly.GetManifestResourceNames())
{
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

#### <a name="images-embedded-in-other-projects"></a>Изображения, внедренные в другие проекты

По умолчанию метод `ImageSource.FromResource` ищет только изображения в той же сборке, что и код, вызывающий метод `ImageSource.FromResource`. С помощью приведенного выше кода отладки можно определить, какие сборки содержат определенный ресурс, изменив `typeof()` инструкцию на `Type`, которая указана в каждой сборке.

Однако исходная сборка, в которой выполняется поиск внедренного изображения, может быть указана в качестве аргумента для метода `ImageSource.FromResource`:

```csharp
var imageSource = ImageSource.FromResource("filename.png", 
            typeof(MyClass).GetTypeInfo().Assembly);
```

## <a name="download-images"></a>Загрузка образов

Образы могут автоматически загружаться для отображения, как показано в следующем XAML:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
       x:Class="WorkingWithImages.DownloadImagesXaml">
  <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
    <Label Text="Image UriSource Xaml" />
    <Image Source="https://xamarin.com/content/images/pages/forms/example-app.png" />
    <Label Text="example-app.png gets downloaded from xamarin.com" />
  </StackLayout>
</ContentPage>
```

Эквивалент C# код выглядит следующим образом:

```csharp
var webImage = new Image { 
     Source = ImageSource.FromUri(
        new Uri("https://xamarin.com/content/images/pages/forms/example-app.png")
     ) };
```

Для метода [`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) требуется объект `Uri` и возвращается новый [`UriImageSource`](xref:Xamarin.Forms.UriImageSource) , считывающий из `Uri`.

Имеется также неявное преобразование для строки URI, поэтому следующий пример также будет работать:

```csharp
webImage.Source = "https://xamarin.com/content/images/pages/forms/example-app.png";
```

На следующих снимках экрана показано результат отображения удаленного изображения на каждой платформе:

[![пример приложения, отображающий скачанный образ](images-images/download-sml.png)](images-images/download.png#lightbox)

### <a name="downloaded-image-caching"></a>Кэширование скачанных образов

[`UriImageSource`](xref:Xamarin.Forms.UriImageSource) также поддерживает кэширование скачанных образов, настроенных с помощью следующих свойств:

- [`CachingEnabled`](xref:Xamarin.Forms.UriImageSource.CachingEnabled) — включено ли кэширование (по умолчанию`true`).
- [`CacheValidity`](xref:Xamarin.Forms.UriImageSource.CacheValidity) — `TimeSpan`, который определяет, как долго образ будет храниться локально.

Кэширование включено по умолчанию и сохраняется локально на 24 часа. Чтобы отключить кэширование для конкретного изображения, создать экземпляр источник изображения следующим образом:

```csharp
image.Source = new UriImageSource { CachingEnabled = false, Uri = new Uri("http://server.com/image") };
```

Чтобы задать периодом указанным кэшем (например, 5 дней) следует создать источник изображения следующим образом:

```csharp
webImage.Source = new UriImageSource
{
    Uri = new Uri("https://xamarin.com/content/images/pages/forms/example-app.png"),
    CachingEnabled = true,
    CacheValidity = new TimeSpan(5,0,0,0)
};
```

Встроенные кэширование упрощает для поддержки сценариев, таких как прокрутка списки изображений, где можно задать (или привязка) изображение в каждой ячейке и встроенные кэша берем на себя повторной загрузке образа, когда ячейка прокручена обратно в представление.

## <a name="animated-gifs"></a>Анимированные GIF

Xamarin. Forms включает поддержку отображения мелких анимированных GIF. Это достигается путем присвоения свойству [`Image.Source`](xref:Xamarin.Forms.Image.Source) АНИМИРОВАНного GIF-файла.

```xaml
<Image Source="demo.gif" />
```

> [!IMPORTANT]
> Хотя поддержка анимированных рисунков GIF в Xamarin. Forms включает возможность загрузки файлов, она не поддерживает кэширование или потоковую передачу анимированных GIF.

По умолчанию, когда загружается анимированный GIF-файл, он не будет воспроизводиться. Это обусловлено тем, что свойство `IsAnimationPlaying`, которое управляет воспроизведением или остановкой анимированного GIF-файла, имеет значение по умолчанию `false`. Это свойство типа `bool`поддерживается объектом [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) , что означает, что он может быть целевым объектом привязки данных и имеет стиль.

Таким образом, когда загружается анимированный GIF-файл, он не будет воспроизводиться до тех пор, пока свойство `IsAnimationPlaying` не будет установлено в `true`. Затем можно остановить воспроизведение, задав для свойства `IsAnimationPlaying` значение `false`. Обратите внимание, что это свойство не действует при отображении источника изображения, отличного от GIF.

> [!NOTE]
> В Android поддержка анимированных GIF-рисунков требует, чтобы приложение использовало быстрые модули подготовки отчетов и не работало, если вы выбрали использование модулей подготовки отчетов прежних версий.
> В UWP поддержка анимированных GIF-рисунков требует наличия минимального выпуска обновления для Windows 10 (версия 1607).

## <a name="icons-and-splash-screens"></a>Значки и экраны-заставки

Несмотря на то, что они не связаны с представлением [`Image`](xref:Xamarin.Forms.Image) , значки приложений и экраны-заставки также являются важным использованием изображений в проектах Xamarin. Forms.

Настройка значков и экранов-заставок для приложений Xamarin.Forms выполняется в каждом из проектов приложений. Это означает, что создание правильно размера изображения для iOS, Android и универсальной платформы Windows. Эти образы должны с именем и находится в соответствии с требованиями каждой платформы.

## <a name="icons"></a>Значки

Дополнительные сведения о создании этих ресурсов приложений см. в руководстве по [работе с образами](~/ios/app-fundamentals/images-icons/index.md), [Google Иконографи](https://developer.android.com/design/style/iconography.html)и [UWP руководства для плиток и ресурсов значков](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/) .

Кроме того, значки шрифтов могут отображаться [`Image`ным](xref:Xamarin.Forms.Image) представлением путем указания данных значка шрифта в объекте `FontImageSource`. Дополнительные сведения см. в разделе [Отображение значков шрифтов](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons) в пошаговом окне " [шрифты](~/xamarin-forms/user-interface/text/fonts.md) ".

## <a name="splash-screens"></a>Экраны-заставки

Только приложения iOS и универсальной платформы Windows требуют экран-заставка (также называемый образ запуска экрана или по умолчанию).

См. документацию по [iOS, которая работает с изображениями](~/ios/app-fundamentals/images-icons/index.md) и [экранами-заставками](/windows/uwp/launch-resume/splash-screens/) в центре разработки для Windows.

## <a name="related-links"></a>Связанные ссылки

- [Воркингвисимажес (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages)
- [Работа iOS с изображениями](~/ios/app-fundamentals/images-icons/index.md)
- [Иконографи Android](https://developer.android.com/design/style/iconography.html)
- [Рекомендации по ресурсам плитки и значков](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)
