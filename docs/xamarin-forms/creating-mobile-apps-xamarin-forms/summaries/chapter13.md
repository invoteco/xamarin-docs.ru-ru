---
title: Сводка главе 13. Битовые карты
description: 'Создание мобильных приложений с помощью Xamarin.Forms: Сводка главе 13. Битовые карты'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5D153857-B6B7-4A14-8FB9-067DE198C2C7
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: e4746ed94a008d382ce15bb9cd7c52365d9ba574
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725532"
---
# <a name="summary-of-chapter-13-bitmaps"></a>Сводка главе 13. Битовые карты

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)

> [!NOTE]
> Заметки на этой странице указывать области, где различаются Xamarin.Forms материал, представленный в книге.

Элемент Xamarin. Forms [`Image`](xref:Xamarin.Forms.Image) отображает точечный рисунок. Все платформы Xamarin.Forms поддерживают форматы файлов JPEG, PNG, GIF и BMP.

Растровые изображения в Xamarin.Forms поступать из четырех местах:

- В Интернете в соответствии с URL-адрес
- Внедрены в качестве ресурса в общей библиотеки
- Внедренного как ресурс в проектах приложений платформы
- Из любого места, на который может ссылаться объект `Stream` .NET, включая `MemoryStream`

Ресурсы растрового изображения в общей библиотеке являются независимыми от платформы, пока ресурсы точечного рисунка в проектах платформы зависят от платформы.

> [!NOTE]
> Текст книги делает ссылки на переносимые библиотеки классов, в которой были заменены библиотеки .NET Standard. Все примеры кода из книги будет преобразована использовать стандартные библиотеки .NET.

Точечный рисунок задается путем присвоения свойству [`Source`](xref:Xamarin.Forms.Image.Source) `Image` объекту типа [`ImageSource`](xref:Xamarin.Forms.ImageSource), абстрактному классу с тремя производными:

- [`UriImageSource`](xref:Xamarin.Forms.UriImageSource) для доступа к точечному рисунку через Интернет на основе объекта `Uri`, для которого задано свойство [`Uri`](xref:Xamarin.Forms.UriImageSource.Uri)
- [`FileImageSource`](xref:Xamarin.Forms.FileImageSource) для доступа к точечному рисунку, хранящемуся в проекте приложения платформы на основе папки и пути к файлу, для которого задано свойство [`File`](xref:Xamarin.Forms.FileImageSource.File)
- [`StreamImageSource`](xref:Xamarin.Forms.StreamImageSource) для загрузки точечного рисунка с помощью объекта `Stream` .NET, который возвращает `Stream` из `Func`, установленного в его свойство [`Stream`](xref:Xamarin.Forms.StreamImageSource.Stream)

Кроме того, можно использовать следующие статические методы класса `ImageSource`, каждый из которых возвращает `ImageSource` объекты:

- [`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) для доступа к точечному рисунку через Интернет на основе объекта `Uri`
- [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) для доступа к точечному рисунку, сохраненному в качестве внедренного ресурса в PCL приложения; [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Type)) или [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Reflection.Assembly)) для доступа к точечному рисунку в другой исходной сборке
- [`ImageSource.FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String)) для доступа к точечному рисунку из проекта приложения платформы
- [`ImageSource.FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream})) загрузки точечного рисунка на основе объекта `Stream`

Класс, эквивалентный методам `Image.FromResource`, отсутствует. Класс `UriImageSource` полезен, если требуется управлять кэшированием. Класс `FileImageSource` полезен в XAML. `StreamImageSource` удобно использовать для асинхронной загрузки `Stream` объектов, а `ImageSource.FromStream` — синхронно.

## <a name="platform-independent-bitmaps"></a>Точечные рисунки независимо от платформы

Проект [**веббитмапкоде**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode) загружает точечный рисунок через Интернет с помощью `ImageSource.FromUri`. Элементу `Image` присваивается свойство `Content` `ContentPage`, поэтому оно ограничено размером страницы. Независимо от размера точечного рисунка элемент с ограничением `Image` растягивается до размера своего контейнера, а размер точечного рисунка отображается в его максимальном размере в элементе `Image` при сохранении пропорций изображения. Области `Image` за пределами точечного рисунка могут быть окрашены [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor).

Пример [**веббитмапксамл**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml) похож, но просто устанавливает для свойства `Source` URL-адрес. Преобразование обрабатывается классом [`ImageSourceConverter`](xref:Xamarin.Forms.ImageSourceConverter) .

### <a name="fit-and-fill"></a>По размеру и заливки

Способ растяжения растрового изображения можно контролировать, присвоив свойству [`Aspect`](xref:Xamarin.Forms.Image.Aspect) `Image` один из следующих элементов перечисления [`Aspect`](xref:Xamarin.Forms.Aspect) :

- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit): учитывается пропорции (по умолчанию)
- [`Fill`](xref:Xamarin.Forms.Aspect.Fill): область заполнения, не учитывается пропорции
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill): область заполнения, но учитывает пропорции, которая выполняется путем обрезки части точечного рисунка

### <a name="embedded-resources"></a>Внедренные ресурсы

Можно добавить к файлу точечного рисунка, PCL или папки в переносимой библиотеке Классов. Присвойте ему **действие сборки** **EmbeddedResource**. В примере [**ресаурцебитмапкоде**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode) показано, как использовать `ImageSource.FromResource` для загрузки файла. Имя ресурса, передаваемые методу состоит из имени сборки, за которым следует точка, за которым следует имя необязательная папка и точкой, а затем имя файла.

Программа задает для свойств `VerticalOptions` и `HorizontalOptions` `Image` `LayoutOptions.Center`, что делает элемент `Image` неограниченным. `Image` и точечный рисунок имеют одинаковый размер:

- В iOS и Android `Image` представляет собой размер точечного рисунка в пикселях. Имеется однозначное сопоставление между пикселов точечного рисунка и точек.
- В универсальная платформа Windows `Image` — это размер точечного рисунка в единицах, не зависящих от устройства. На большинстве устройств каждый пиксел точечный рисунок занимает несколько точек.

Пример [**стаккедбитмап**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap) помещает `Image` в вертикальную `StackLayout` в XAML. Расширение разметки с именем [`ImageResourceExtension`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs) помогает ссылаться на внедренный ресурс в XAML. Этот класс только загружает ресурсы из сборки, в котором она размещается, поэтому он не может быть помещен в библиотеку.

### <a name="more-on-sizing"></a>Дополнительные сведения об изменения размера

Часто желательно, чтобы точечные рисунки размером согласованно среди всех платформ.
Экспериментируя с **стаккедбитмап**, можно установить `WidthRequest` в элементе `Image` вертикальной `StackLayout`, чтобы обеспечить согласованность размера на всех платформах, но можно уменьшить размер только с помощью этого метода.

Можно также задать `HeightRequest`, чтобы обеспечить единообразие размеров образов на платформах, но ограничение ширины точечного рисунка будет ограничивать гибкость этой методики. Для изображения в вертикальном `StackLayout``HeightRequest` следует избегать.

Лучший подход — начать с точечного рисунка, ширина которого превышает ширину телефона в единицах, не зависящих от устройства, и присвоить `WidthRequest` желаемую ширину в единицах, не зависящих от устройства. Это продемонстрировано в примере [**девицеиндбитмапсизе**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize) .

[**Мадтеапарти**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty) отображает главу 7 из Льюис Кэрролл *Алисы приключения в вондерланд* с исходными иллюстрациями Джон Тенниел:

[![Тройной снимок экрана с вызываемой стороной Mad](images/ch13fg16-small.png "Текст книги о Хаттерсной стороне Mad")](images/ch13fg16-large.png#lightbox "Текст книги о Хаттерсной стороне Mad")

### <a name="browsing-and-waiting"></a>Обзор и ожидания

Пример [**имажебровсер**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser) позволяет пользователю просматривать стандартные образы, хранящиеся на веб-сайте Xamarin. Для скачивания JSON-файла со списком точечных рисунков используется класс [`WebRequest`](xref:System.Net.WebRequest) .NET.

> [!NOTE]
> Программы Xamarin. Forms должны использовать [`HttpClient`](xref:System.Net.Http.HttpClient) , а не [`WebRequest`](xref:System.Net.WebRequest) для доступа к файлам через Интернет.

Программа использует [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) , чтобы указать, что что-то происходит. При загрузке каждого растрового изображения [`IsLoading`](xref:Xamarin.Forms.Image.IsLoading) свойство `Image` только для чтения имеет значение `true`. Свойство `IsLoading` поддерживается связываемым свойством, поэтому при изменении этого свойства возникает событие `PropertyChanged`. Программа присоединяет обработчик к этому событию и использует текущее значение `IsLoaded`, чтобы задать свойство [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning) `ActivityIndicator`.

## <a name="streaming-bitmaps"></a>Потоковая передача точечных рисунков

Метод `ImageSource.FromStream` создает `ImageSource` на основе `Stream`.NET. Методу должен быть передан объект `Func`, возвращающий объект `Stream`.

### <a name="accessing-the-streams"></a>Доступ к потоки

В примере [**битмапстреамс**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams) показано, как использовать метод `ImaageSource.FromStream` для загрузки растрового изображения, хранящегося в виде внедренного ресурса, и для загрузки точечного рисунка через Интернет.

### <a name="generating-bitmaps-at-run-time"></a>Создание растровых изображений во время выполнения

Все платформы Xamarin. Forms поддерживают формат файлов BMP без сжатия, который легко создается в коде и затем сохраняется в `MemoryStream`. Этот метод позволяет алгоритмически создавать растровые изображения во время выполнения, как реализовано в классе [`BmpMaker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) в библиотеке **Xamrin. формсбук. Toolkit** .

В примере [**дийградиентбитмап**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap) демонстрируется использование `BmpMaker` для создания точечного рисунка с градиентным изображением.

## <a name="platform-specific-bitmaps"></a>Растровые изображения для конкретных платформ

Все платформы Xamarin.Forms Разрешить сохранение растровых изображений в сборки платформы приложений. При получении приложением Xamarin. Forms эти точечные рисунки платформы имеют тип [`FileImageSource`](xref:Xamarin.Forms.FileImageSource). Они используются для:

- Свойство [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) [`MenuItem`](xref:Xamarin.Forms.MenuItem)
- Свойство [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)
- Свойство [`Image`](xref:Xamarin.Forms.Button) `Button`

Сборки платформы уже содержат точечные рисунки, значки и экраны-заставки:

- В проекте iOS в папке **ресурсов**
- В проекте Android в подпапках папки **ресурсов**
- В проектах Windows в папке **Assets** (несмотря на то, что платформы Windows не ограничивают точечные рисунки для этой папки)

В примере [**платформбитмапс**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps) используется код для вывода значка из проектов приложений платформы.

### <a name="bitmap-resolutions"></a>Разрешение растрового изображения

Все платформы позволяют хранить несколько версий точечных рисунков для разрешений на другое устройство. Во время выполнения правильную версию загружается в зависимости от разрешения экрана устройства.

В iOS эти точечные рисунки, различаются по суффикс к имени файла:

- Без суффикса для устройств 160 точек на ДЮЙМ (1 пиксель в аппаратно независимая единица)
- суффикс "@2x" для устройств с 320 DPI (2 пиксела до Диу)
- суффикс "@3x" для устройств с 480 DPI (3 пиксела до Диу)

Растровое изображение, предназначенное для отображения как квадрат 1 дюйм будет существовать в трех версиях:

- MyImage.jpg 160 точек square
- MyImage@2x.jpg с квадратом 320 пикселей
- MyImage@3x.jpg с квадратом 480 пикселей

Программа ссылаетесь на дополнительное этим растровым изображением, как MyImage.jpg, но правильную версию извлекается во время выполнения на основе разрешения экрана. При отсутствии ограничений точечный рисунок всегда будет отображаться на 160 аппаратно независимых единицах.

Для Android точечные рисунки хранятся в различных вложенных папках папки **Resources** :

- drawable-ldpi (низкий точек на ДЮЙМ) для устройств 120 точек на ДЮЙМ (0,75 точек в DIU)
- drawable-mdpi (средний) для устройств 160 точек на ДЮЙМ (1 пиксель в DIU)
- drawable-hdpi (высокий) для устройств 240 точек на ДЮЙМ (1,5 точек в DIU)
- drawable-xhdpi (очень высокое) для устройств 320 точек на ДЮЙМ (2 точек в DIU)
- drawable-xxhdpi (очень очень высокий уровень) для устройств 480 точек на ДЮЙМ (3 точек в DIU)
- drawable-xxxhdpi (три дополнительных Удачи) для устройств 640 точек на ДЮЙМ (4 точек в DIU)

Для точечного рисунка, который предназначен для отображения на одной квадратный дюйм в различных версиях растровое изображение будет тем же именем, но другого размера и хранящиеся в этих папках:

- drawable-ldpi/MyImage.jpg на 120 пикселей ниже square
- drawable-mdpi/MyImage.jpg 160 точек square
- drawable-hdpi/MyImage.jpg в квадрат составляет 240 пикселей
- drawable-xhdpi/MyImage.jpg 320 пикселов в квадрат
- drawable-xxhdpi/MyImage.jpg 480 точек square
- drawable-xxxhdpi/MyImage.jpg в квадрат 640 пикселей

Точечный рисунок всегда будет отображаться на 160 аппаратно независимых единицах. (Стандартный шаблон решения Xamarin.Forms включает только hdpi, xhdpi и xxhdpi папок.)

Проект универсальной платформы Windows поддерживает схему именования точечного рисунка, состоящий из коэффициент масштабирования в пикселях на аппаратно независимые единицы в процентах, например:

- MyImage.scale-200.jpg 320 пикселов в квадрат

Только некоторые процентные показатели являются допустимыми. Примеры программ для этой книги включают только образы с суффиксами **Scale-200** , но текущие шаблоны решений Xamarin. Forms включают **масштабируемые-100**, **Scale-125**, **Scale-150**и **Scale-400**.

При добавлении точечных рисунков в проекты платформы **действие сборки** должно быть следующим:

- iOS: **BundleResource**
- Android: **AndroidResource**
- UWP: **содержимое**

Образец [**имажетап**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap) создает два объекта, наподобие кнопок, состоящих из `Image` элементов с установленным `TapGestureRecognizer`. Предполагается, что объекты были квадрат 1 дюйм. Свойство `Source` `Image` задается с помощью `OnPlatform` и `On` объектов для ссылки на потенциально отличающиеся имена файлов на платформах. Растровые изображения включают значения указывают их размер в пикселях, чтобы можно было видеть, какой размер растрового изображения извлекается и подготовке к просмотру.

### <a name="toolbars-and-their-icons"></a>Панели инструментов и соответствующие значки

Одним из основных способов использования точечных рисунков, зависящих от платформы, является панель инструментов Xamarin. Forms, которая создается путем добавления [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) объектов в коллекцию [`ToolbarItems`](xref:Xamarin.Forms.Page.ToolbarItems) , определенную `Page`. `ToobarItem` является производным от [`MenuItem`](xref:Xamarin.Forms.MenuItem) , от которого он наследует некоторые свойства.

Наиболее важные свойства `ToolbarItem`:

- [`Text`](xref:Xamarin.Forms.MenuItem.Text) текста, который может отображаться в зависимости от платформы и `Order`
- [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) типа `FileImageSource` для образа, который может отображаться в зависимости от платформы и `Order`
- [`Order`](xref:Xamarin.Forms.ToolbarItem.Order) типа [`ToolbarItemOrder`](xref:Xamarin.Forms.ToolbarItemOrder), перечисления с тремя элементами, [`Default`](xref:Xamarin.Forms.ToolbarItemOrder.Default), [`Primary`](xref:Xamarin.Forms.ToolbarItemOrder.Primary)и [`Secondary`](xref:Xamarin.Forms.ToolbarItemOrder.Secondary).

Количество `Primary` элементов должно быть ограничено тремя или четырьмя. Необходимо включить параметр `Text` для всех элементов. Для большинства платформ только `Primary`ным элементам требуется `Icon`, но для Windows 8.1 требуется `Icon` для всех элементов. Значки должны иметь 32 аппаратно независимых единицах square. Тип `FileImageSource` указывает, что они относятся к конкретной платформе.

`ToolbarItem` запускает событие [`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked) при касании, во многом похоже на `Button`. `ToolbarItem` также поддерживает свойства [`Command`](xref:Xamarin.Forms.MenuItem.Command) и [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter) , часто используемые в соединении с MVVM. (См. [главу 18, MVVM](chapter18.md)).

Как для iOS, так и для Android требуется, чтобы страница, на которой отображается панель инструментов, была [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) или страницей, на которую помещается `NavigationPage`. Программа [**тулбардемо**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo) устанавливает свойство `MainPage` класса `App` в [конструктор`NavigationPage`](xref:Xamarin.Forms.NavigationPage.%23ctor(Xamarin.Forms.Page)) с аргументом `ContentPage`, а также демонстрирует создание обработчика событий панели инструментов.

### <a name="button-images"></a>Изображения кнопки

Можно также использовать точечные рисунки для конкретной платформы, чтобы задать для свойства [`Image`](xref:Xamarin.Forms.Button.Image) `Button` точечный рисунок 32 единиц, не зависящих от устройства, как показано в примере [**буттонимаже**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage) .

> [!NOTE]
> Использование изображений на кнопках была улучшена. См. раздел [Использование растровых изображений с кнопками](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons).

## <a name="related-links"></a>Связанные ссылки

- [Глава 13. полный текст (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [Глава 13. примеры](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [Работа с образами](~/xamarin-forms/user-interface/images.md)
- [Использование точечных рисунков с кнопками](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)
