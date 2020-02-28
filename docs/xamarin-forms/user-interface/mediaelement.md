---
title: Элемент для Xamarin. Forms MediaElement
description: В этой статье объясняется, как использовать MediaElement для воспроизведения видео и аудио в приложении Xamarin. Forms.
ms.prod: xamarin
ms.assetid: e65f1e56-a80d-46c7-9ff4-7ae6650a3165
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/18/2020
ms.openlocfilehash: 601a2884cb9ca90ab6681e48afda4c9f1f467932
ms.sourcegitcommit: 5d22f37dfc358678df52a4d17c57261056a72cb7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2020
ms.locfileid: "77674550"
---
# <a name="xamarinforms-mediaelement"></a>Элемент для Xamarin. Forms MediaElement

![](~/media/shared/preview.png "This API is currently pre-release")

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/MediaElementDemos)

[`MediaElement`](xref:Xamarin.Forms.MediaElement) — это представление для воспроизведения видео и аудио. Носители, поддерживаемые базовой платформой, можно воспроизводить из следующих источников:

- Веб-узел с использованием URI (HTTP или HTTPS).
- Ресурс, внедренный в приложение платформы, используя `ms-appx:///` схему URI.
- Файлы, полученные из локальных и временных папок приложения с использованием `ms-appdata:///` схемы URI.
- Библиотека устройства.

[`MediaElement`](xref:Xamarin.Forms.MediaElement) могут использовать элементы управления воспроизведением платформы, которые называются элементами управления транспортом. Однако по умолчанию они отключены и могут быть заменены собственными элементами управления транспорта. На следующих снимках экрана показано, `MediaElement` воспроизводит видео с помощью элементов управления транспортной платформы:

[![Снимок экрана элемента MediaElement: воспроизведение видео в iOS и Android](mediaelement-images/playback-controls.png "Элемент MediaElement воспроизводит видео")](mediaelement-images/playback-controls-large.png#lightbox "Элемент MediaElement воспроизводит видео")

[`MediaElement`](xref:Xamarin.Forms.MediaElement) доступен в Xamarin. forms 4,5. Однако в настоящее время он экспериментальен и может использоваться только путем добавления следующей строки кода в файл *app.XAML.CS* :

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

> [!NOTE]
> [`MediaElement`](xref:Xamarin.Forms.MediaElement) доступен на устройствах iOS, Android, универсальная платформа Windows (UWP) и на дополнительных платформах.

[`MediaElement`](xref:Xamarin.Forms.MediaElement) определяет следующие свойства:

- [`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect)типа [`Aspect`](xref:Xamarin.Forms.Aspect)определяет, как будет масштабироваться носитель в соответствии с отображаемой областью. Значение по умолчанию этого свойства равно `AspectFit`.
- [`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay)типа `bool`указывает, начнется ли воспроизведение мультимедиа автоматически при установке свойства [`Source`](xref:Xamarin.Forms.MediaElement.Source) . Значение по умолчанию этого свойства равно `true`.
- [`BufferingProgress`](xref:Xamarin.Forms.MediaElement.BufferingProgress)типа `double`указывает текущий ход выполнения буферизации. Значение этого свойства по умолчанию равно 0,0.
- [`CanSeek`](xref:Xamarin.Forms.MediaElement.CanSeek)типа `bool`указывает, можно ли переместить носитель, задав значение свойства [`Position`](xref:Xamarin.Forms.MediaElement.Position) . Это свойство доступно только для чтения.
- [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState)типа [`MediaElementState`](xref:Xamarin.Forms.MediaElementState)указывает текущее состояние элемента управления. Это свойство только для чтения, для которого значение по умолчанию — `MediaElementState.Closed`.
- [`Duration`](xref:Xamarin.Forms.MediaElement.Duration)типа `TimeSpan?`указывает длительность текущего открытого носителя. Это свойство только для чтения, значением по умолчанию которого является `null`.
- [`IsLooping`](xref:Xamarin.Forms.MediaElement.IsLooping)типа `bool`описывает, должен ли текущий загруженный источник мультимедиа возобновить воспроизведение с начала после достижения конца. Значение по умолчанию этого свойства равно `false`.
- [`KeepScreenOn`](xref:Xamarin.Forms.MediaElement.KeepScreenOn)типа `bool`определяет, сохраняется ли экран устройства во время воспроизведения мультимедиа. Значение по умолчанию этого свойства равно `false`.
- [`Position`](xref:Xamarin.Forms.MediaElement.Position), типа `TimeSpan`, описывает текущий ход выполнения воспроизведения мультимедиа. Значение по умолчанию этого свойства равно `TimeSpan.Zero`.
- [`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls)типа `bool`определяет, отображаются ли элементы управления воспроизведением платформ. Значение по умолчанию этого свойства равно `false`.
- [`Source`](xref:Xamarin.Forms.MediaElement.Source)типа [`MediaSource`](xref:Xamarin.Forms.MediaSource)указывает источник мультимедиа, загруженного в элемент управления.
- [`VideoHeight`](xref:Xamarin.Forms.MediaElement.VideoHeight)типа `int`указывает высоту элемента управления. Это свойство доступно только для чтения.
- [`VideoWidth`](xref:Xamarin.Forms.MediaElement.VideoWidth)типа `int`указывает ширину элемента управления. Это свойство доступно только для чтения.
- [`Volume`](xref:Xamarin.Forms.MediaElement.Volume)типа `double`определяет громкость носителя, которая представлена в линейном масштабе от 0 до 1. Это свойство использует привязку `TwoWay`, а значение по умолчанию — 1.

Эти свойства, за исключением свойства `CanSeek`, поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

Класс [`MediaElement`](xref:Xamarin.Forms.MediaElement) также определяет четыре события:

- [`MediaOpened`](xref:Xamarin.Forms.MediaElement.MediaOpened) возникает при проверке и открытии потока мультимедиа.
- [`MediaEnded`](xref:Xamarin.Forms.MediaElement.MediaEnded) срабатывает, когда `MediaElement` завершает воспроизведение носителя.
- [`MediaFailed`](xref:Xamarin.Forms.MediaElement.MediaFailed) запускается при возникновении ошибки, связанной с источником мультимедиа.
- [`SeekCompleted`](xref:Xamarin.Forms.MediaElement.SeekCompleted) срабатывает, когда точка поиска запрошенной операции поиска готова к воспроизведению.

Кроме того, [`MediaElement`](xref:Xamarin.Forms.MediaElement) включает методы [`Play`](xref:Xamarin.Forms.MediaElement.Play), [`Pause`](xref:Xamarin.Forms.MediaElement.Pause)и [`Stop`](xref:Xamarin.Forms.MediaElement.Stop) .

Сведения о поддерживаемых форматах мультимедиа в Android см. в статье [Поддерживаемые форматы мультимедиа](https://developer.android.com/guide/topics/media/media-formats) в Developer.Android.com. Сведения о поддерживаемых форматах мультимедиа в универсальная платформа Windows (UWP) см. в разделе [Поддерживаемые кодеки](/windows/uwp/audio-video-camera/supported-codecs).

## <a name="play-remote-media"></a>Воспроизвести Удаленный носитель

[`MediaElement`](xref:Xamarin.Forms.MediaElement) может воспроизводить удаленные файлы мультимедиа с помощью схем URI HTTP и HTTPS. Это достигается путем присвоения свойству [`Source`](xref:Xamarin.Forms.MediaElement.Source) универсального кода ресурса (URI) файла мультимедиа:

```xaml
<MediaElement Source="https://sec.ch9.ms/ch9/5d93/a1eab4bf-3288-4faf-81c4-294402a85d93/XamarinShow_mid.mp4"
              ShowsPlaybackControls="True" />
```

По умолчанию носитель, определенный свойством [`Source`](xref:Xamarin.Forms.MediaElement.Source) , играет роль сразу после открытия носителя. Чтобы отключить автоматическое воспроизведение мультимедиа, присвойте свойству [`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay) значение `false`.

Элементы управления воспроизведением мультимедиа по умолчанию отключены и включаются путем присвоения свойству [`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls) значения `true`. [`MediaElement`](xref:Xamarin.Forms.MediaElement) будет использовать элементы управления воспроизведением платформы.

## <a name="play-local-media"></a>Воспроизвести локальный носитель

Локальный носитель можно воспроизвести из следующих источников:

- Ресурс, внедренный в приложение платформы, используя `ms-appx:///` схему URI.
- Файлы, полученные из локальных и временных папок приложения с использованием `ms-appdata:///` схемы URI.
- Библиотека устройства.

Дополнительные сведения об этих схемах URI см. в разделе [схемы URI](/windows/uwp/app-resources/uri-schemes).

### <a name="play-media-embedded-in-the-app-package"></a>Воспроизведение мультимедиа, внедренного в пакет приложения

[`MediaElement`](xref:Xamarin.Forms.MediaElement) может воспроизводить файлы мультимедиа, внедренные в пакет приложения, используя `ms-appx:///` схему URI. Файлы мультимедиа внедряются в пакет приложения путем помещения их в проект платформы.

Хранение файла мультимедиа в проекте платформы отличается для каждой платформы:

- В iOS файлы мультимедиа должны храниться в папке **Resources** или во вложенной папке папки **Resources** . Файл мультимедиа должен иметь `Build Action` `BundleResource`.
- В Android файлы мультимедиа должны храниться во вложенной папке **ресурсов** с именем **RAW**. В папке **raw** не может быть вложенных папок. Файл мультимедиа должен иметь `Build Action` `AndroidResource`.
- В UWP файлы мультимедиа могут храниться в любой папке проекта. Файл мультимедиа должен иметь `BuildAction` `Content`.

Затем можно воспроизводить файлы мультимедиа, соответствующие этим критериям, с помощью `ms-appx:///` схеме URI:

```xaml
<MediaElement Source="ms-appx:///XamarinForms101UsingEmbeddedImages.mp4"
              ShowsPlaybackControls="True" />
```

При использовании привязки данных можно использовать преобразователь значений для применения этой схемы URI:

```csharp
public class VideoSourceConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value == null)
            return null;

        if (string.IsNullOrWhiteSpace(value.ToString()))
            return null;

        if (Device.RuntimePlatform == Device.UWP)
            return new Uri($"ms-appx:///Assets/{value}");
        else
            return new Uri($"ms-appx:///{value}");
    }
    // ...
}
```

Экземпляр `VideoSourceConverter` можно использовать для применения схемы URI `ms-appx:///` к внедренному файлу мультимедиа:

```xaml
<MediaElement Source="{Binding MediaSource, Converter={StaticResource VideoSourceConverter}}"
              ShowsPlaybackControls="True" />
```

Дополнительные сведения о схеме URI ms-appx см. в статье [MS-Appx и ms-appx-web](/windows/uwp/app-resources/uri-schemes#ms-appx-and-ms-appx-web).

### <a name="play-media-from-the-apps-local-and-temporary-folders"></a>Воспроизведение мультимедиа из локальных и временных папок приложения

[`MediaElement`](xref:Xamarin.Forms.MediaElement) может воспроизводить файлы мультимедиа, которые копируются в папки локальных или временных данных приложения, используя `ms-appdata:///` схему URI.

В следующем примере показано, как свойство [`Source`](xref:Xamarin.Forms.MediaElement.Source) задано для файла мультимедиа, хранящегося в папке локальных данных приложения:

```xaml
<MediaElement Source="ms-appdata:///local/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

В следующем примере показано свойство [`Source`](xref:Xamarin.Forms.MediaElement.Source) для файла мультимедиа, хранящегося в папке временных данных приложения.

```xaml
<MediaElement Source="ms-appdata:///temp/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

> [!IMPORTANT]
> Кроме воспроизведения файлов мультимедиа, хранящихся в локальных или временных папках данных приложения, UWP также может воспроизводить файлы мультимедиа, расположенные в перемещаемой папке приложения. Это можно сделать, добавив в файл мультимедиа `ms-appdata:///roaming/`.

При использовании привязки данных можно использовать преобразователь значений для применения этой схемы URI:

```csharp
public class VideoSourceConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value == null)
            return null;

        if (string.IsNullOrWhiteSpace(value.ToString()))
            return null;

        return new Uri($"ms-appdata:///{value}");
    }
    // ...
}
```

Экземпляр `VideoSourceConverter` можно использовать для применения схемы URI `ms-appdata:///` к файлу мультимедиа в локальной или временной папке данных приложения.

```xaml
<MediaElement Source="{Binding MediaSource, Converter={StaticResource VideoSourceConverter}}"
              ShowsPlaybackControls="True" />
```

Дополнительные сведения о схеме URI MS-AppData см. в статье [MS-AppData](/windows/uwp/app-resources/uri-schemes#ms-appdata).

#### <a name="copying-a-media-file-to-the-apps-local-or-temporary-data-folder"></a>Копирование файла мультимедиа в локальную или временную папку приложения

Воспроизведение файла мультимедиа, хранящегося в папке локальных или временных данных приложения, требует, чтобы файл мультимедиа был скопирован в него приложением. Это можно сделать, например, скопировав файл мультимедиа из пакета приложения:

```csharp
// This method copies the video from the app package to the app data
// directory for your app. To copy the video to the temp directory
// for your app, comment out the first line of code, and uncomment
// the second line of code.
public static async Task CopyVideoIfNotExists(string filename)
{
    string folder = FileSystem.AppDataDirectory;
    //string folder = Path.GetTempPath();
    string videoFile = Path.Combine(folder, "XamarinVideo.mp4");

    if (!File.Exists(videoFile))
    {
        using (Stream inputStream = await FileSystem.OpenAppPackageFileAsync(filename))
        {
            using (FileStream outputStream = File.Create(videoFile))
            {
                await inputStream.CopyToAsync(outputStream);
            }
        }
    }
}
```

> [!NOTE]
> В приведенном выше примере кода используется класс `FileSystem`, входящий в Xamarin. Essentials. Дополнительные сведения см. в разделе [вспомогательные функции файловой системы Xamarin. Essentials](~/essentials/file-system-helpers.md?context=xamarin%2Fxamarin-forms&tabs=android).

### <a name="play-media-from-the-device-library"></a>Воспроизведение мультимедиа из библиотеки устройств

Большинство современных мобильных устройств и настольных компьютеров могут записывать видео и аудио с помощью камеры и микрофона устройства. Затем созданный носитель сохраняется в виде файлов на устройстве. Эти файлы можно извлечь из библиотеки и воспроизвести [`MediaElement`](xref:Xamarin.Forms.MediaElement).

Каждая из платформ включает в себя средство, позволяющее пользователю выбирать носитель из библиотеки устройства. В Xamarin. Forms проекты платформы могут вызывать эту функцию, и ее можно вызывать с помощью класса [`DependencyService`](xref:Xamarin.Forms.DependencyService) .

Служба зависимости видео, используемая в образце приложения, очень похожа на ту, которая определена при [выборе фотографии из библиотеки рисунков](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md), за исключением того, что средство выбора возвращает имя файла, а не объект `Stream`. Проект общего кода определяет интерфейс с именем `IVideoPicker`, который определяет один метод с именем `GetVideoFileAsync`. Затем каждая платформа реализует этот интерфейс в `VideoPicker`ном классе.

В следующем примере кода показано, как получить файл мультимедиа из библиотеки устройств.

```csharp
string filename = await DependencyService.Get<IVideoPicker>().GetVideoFileAsync();
if (!string.IsNullOrWhiteSpace(filename))
{
    mediaElement.Source = new FileMediaSource
    {
        File = filename
    };
}
```

Служба зависимости видео вызывается путем вызова метода `DependencyService.Get` для получения реализации интерфейса `IVideoPicker` в проекте платформы. Затем в этом экземпляре вызывается метод `GetVideoFileAsync`, а возвращаемое имя файла используется для создания [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) объекта и задания его свойству [`Source`](xref:Xamarin.Forms.MediaElement.Source) [`MediaElement`](xref:Xamarin.Forms.MediaElement).

## <a name="change-video-aspect-ratio"></a>Изменение пропорций видео

Свойство [`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect) определяет способ масштабирования видео мультимедиа в соответствии с отображаемой областью. По умолчанию этому свойству присваивается элемент перечисления `AspectFit`, но его можно задать любому из [`Aspect`](xref:Xamarin.Forms.Aspect) членов перечисления:

- `AspectFit` указывает, что видео будет леттербоксед (при необходимости) для размещения в области экрана, сохраняя пропорции.
- `AspectFill` указывает, что видео будет обрезано, чтобы заполнить область экрана, сохраняя пропорции.
- `Fill` указывает, что видео будет растянуто для заполнения отображаемой области.

## <a name="poll-for-position-data"></a>Опрос данных о положении

Уведомление об изменении свойства для свойства, допускающего привязку [`Position`](xref:Xamarin.Forms.MediaElement.Position) , срабатывает только в течение ключа, например с начала и окончания воспроизведения, а также при приостановке. Поэтому привязка данных к свойству `Position` не будет давать точные данные о положении. Вместо этого необходимо настроить таймер и опросить свойство.

Хорошим местом для этого является переопределение `OnAppearing` для страницы, требующей, чтобы данные о положении были воспроизведены как носители:

```csharp
bool polling = true;

protected override void OnAppearing()
{
    base.OnAppearing();

    Device.StartTimer(TimeSpan.FromMilliseconds(1000), () =>
    {
        Device.BeginInvokeOnMainThread(() =>
        {
            positionLabel.Text = mediaElement.Position.ToString("hh\\:mm\\:ss");
        });
        return polling;
    });
}

protected override void OnDisappearing()
{
    base.OnDisappearing();
    polling = false;
}
```

В этом примере переопределение `OnAppearing` запускает таймер, который обновляет `positionLabel` со значением `Position` каждую секунду. Обратный вызов таймера вызывается каждую секунду, пока обратный вызов не возвратит `false`. Когда происходит переход на страницу, выполняется переопределение `OnDisappearing`, которое останавливает вызов таймера.

## <a name="understand-mediasource-types"></a>Общие сведения о типах Медиасаурце

[`MediaElement`](xref:Xamarin.Forms.MediaElement) может воспроизводить мультимедиа, присвоив его свойству [`Source`](xref:Xamarin.Forms.MediaElement.Source) удаленный или локальный файл мультимедиа. Свойство `Source` имеет тип [`MediaSource`](xref:Xamarin.Forms.MediaSource), и этот класс определяет два статических метода:

- [`FromFile`](xref:Xamarin.Forms.MediaSource.FromFile*)возвращает экземпляр [`MediaSource`](xref:Xamarin.Forms.MediaSource) из аргумента `string`.
- [`FromUri`](xref:Xamarin.Forms.MediaSource.FromUri*)возвращает экземпляр [`MediaSource`](xref:Xamarin.Forms.MediaSource) из аргумента `Uri`.

Кроме того, класс [`MediaSource`](xref:Xamarin.Forms.MediaSource) также имеет неявные операторы, возвращающие `MediaSource` экземпляры из `string` и `Uri` аргументов.

> [!NOTE]
> Если свойство [`Source`](xref:Xamarin.Forms.MediaElement.Source) задано в XAML, вызывается преобразователь типов, возвращающий экземпляр [`MediaSource`](xref:Xamarin.Forms.MediaSource) из `string` или `Uri`.

Класс [`MediaSource`](xref:Xamarin.Forms.MediaSource) также имеет два производных класса:

- [`UriMediaSource`](xref:Xamarin.Forms.UriMediaSource), который используется для указания удаленного файла мультимедиа из универсального кода ресурса (URI). Этот класс имеет [`Uri`](xref:Xamarin.Forms.UriMediaSource.Uri) свойство, для которого можно задать `Uri`.
- [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource), который используется для указания локального файла мультимедиа из `string`. Этот класс имеет [`File`](xref:Xamarin.Forms.FileMediaSource.File) свойство, для которого можно задать `string`. Кроме того, этот класс содержит неявные операторы для преобразования `string` в `FileMediaSource` объект и объект `FileMediaSource` в `string`.

> [!NOTE]
> При создании [`FileMediaSource`ного](xref:Xamarin.Forms.FileMediaSource) объекта в XAML вызывается преобразователь типов, возвращающий экземпляр [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) из `string`.

## <a name="determine-mediaelement-status"></a>Определение состояния MediaElement

Класс [`MediaElement`](xref:Xamarin.Forms.MediaElement) определяет доступное только для чтения свойство с именем [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState)типа [`MediaElementState`](xref:Xamarin.Forms.MediaElementState). Это свойство указывает текущее состояние элемента управления, например, воспроизведение или приостановка мультимедиа или еще не готово к воспроизведению мультимедиа.

Перечисление [`MediaElementState`](xref:Xamarin.Forms.MediaElementState) определяет следующие члены:

- `Closed` указывает, что `MediaElement` не содержит носитель.
- `Opening` указывает, что `MediaElement` проверяется и пытается загрузить указанный источник.
- `Buffering` указывает, что `MediaElement` загружает носитель для воспроизведения. Его свойство [`Position`](xref:Xamarin.Forms.MediaElement.Position) не переходит в это состояние. Если `MediaElement` воспроизводил видео, он по экрану продолжит отображать последний отображаемый кадр.
- `Playing` указывает, что `MediaElement` воспроизводит источник мультимедиа.
- `Paused` указывает, что `MediaElement` не переместит свойство [`Position`](xref:Xamarin.Forms.MediaElement.Position) . Если `MediaElement` воспроизводил видео, текущий кадр будет отображаться.
- `Stopped` указывает, что `MediaElement` содержит носитель, но он не воспроизводится или приостанавливается. Его свойство [`Position`](xref:Xamarin.Forms.MediaElement.Position) имеет значение 0 и не переходит к нему. Если загруженный носитель является видео, `MediaElement` отображает первый кадр.

Обычно не требуется проверять свойство [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState) при использовании элементов управления транспорта [`MediaElement`](xref:Xamarin.Forms.MediaElement) . Однако это свойство очень важно при реализации собственных элементов управления транспортом.

## <a name="implement-custom-transport-controls"></a>Реализация пользовательских элементов управления транспортом

Элементы управления транспорта проигрывателя мультимедиа включают кнопки, выполняющие функции **воспроизведения**, **приостановки**и **остановки**. Эти кнопки обычно определяются по знакомым значкам, а не тексту. Как правило, функции **Воспроизведение** и **Пауза** объединены в одну кнопку.

По умолчанию элементы управления воспроизведением [`MediaElement`](xref:Xamarin.Forms.MediaElement) отключены. Это позволяет управлять `MediaElement` программным способом или путем предоставления собственных элементов управления транспорта. В поддержку этого `MediaElement` включает методы [`Play`](xref:Xamarin.Forms.MediaElement.Play), [`Pause`](xref:Xamarin.Forms.MediaElement.Pause)и [`Stop`](xref:Xamarin.Forms.MediaElement.Stop) .

В следующем примере XAML показана страница, содержащая [`MediaElement`](xref:Xamarin.Forms.MediaElement) и пользовательские элементы управления транспорта:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MediaElementDemos.CustomTransportPage"
             Title="Custom transport">
    <Grid>
        ...
        <MediaElement x:Name="mediaElement"
                      AutoPlay="False"
                      ... />
        <StackLayout BindingContext="{x:Reference mediaElement}"
                     ...>
            <Button Text="&#x25B6;&#xFE0F; Play"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnPlayPauseButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding CurrentState}"
                                 Value="{x:Static MediaElementState.Playing}">
                        <Setter Property="Text"
                                Value="&#x23F8; Pause" />
                    </DataTrigger>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding CurrentState}"
                                 Value="{x:Static MediaElementState.Buffering}">
                        <Setter Property="IsEnabled"
                                Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>
            <Button Text="&#x23F9; Stop"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnStopButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding CurrentState}"
                                 Value="{x:Static MediaElementState.Stopped}">
                        <Setter Property="IsEnabled"
                                Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>
        </StackLayout>
    </Grid>
</ContentPage>
```

В этом примере пользовательские элементы управления транспорта определяются как объекты [`Button`](xref:Xamarin.Forms.Button) . Однако существует только два `Button` объектов, первая `Button` представляющая **Воспроизведение** и **Пауза**, а вторая `Button` представляющая **остановку**. [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) объекты используются для включения и отключения кнопок, а также для переключения первой кнопки между **воспроизведением** и **паузой**. Дополнительные сведения о триггерах данных см. в разделе [триггеры Xamarin. Forms](~/xamarin-forms/app-fundamentals/triggers.md).

Файл кода программной части содержит обработчики событий [`Clicked`](xref:Xamarin.Forms.Button.Clicked) :

```csharp
void OnPlayPauseButtonClicked(object sender, EventArgs args)
{
    if (mediaElement.CurrentState == MediaElementState.Stopped ||
        mediaElement.CurrentState == MediaElementState.Paused)
    {
        mediaElement.Play();
    }
    else if (mediaElement.CurrentState == MediaElementState.Playing)
    {
        mediaElement.Pause();
    }
}

void OnStopButtonClicked(object sender, EventArgs args)
{
    mediaElement.Stop();
}
```

Кнопка **воспроизведения** может быть нажата, после того как она будет включена, чтобы начать воспроизведение:

[![Снимок экрана элемента MediaElement с пользовательскими элементами управления транспортом в iOS и Android](mediaelement-images/custom-transport-playback.png "Элемент MediaElement воспроизводит видео")](mediaelement-images/custom-transport-playback-large.png#lightbox "Элемент MediaElement воспроизводит видео")

Нажатие кнопки **приостановить** приводит к паузе воспроизведения:

[![Снимок экрана элемента MediaElement с приостановленным воспроизведением в iOS и Android](mediaelement-images/custom-transport-paused.png "MediaElement с приостановленным видео")](mediaelement-images/custom-transport-paused-large.png#lightbox "MediaElement с приостановленным видео")

Нажатие кнопки **остановить** останавливает воспроизведение и возвращает расположение файла мультимедиа в начало.

## <a name="implement-a-custom-position-bar"></a>Реализация пользовательской панели позиций

На каждой платформе реализуются элементы управления транспортировкой, в том числе строка позиционирования. Эта панель напоминает ползунок и показывает текущее расположение носителя в течение его общей продолжительности. Кроме того, можно управлять панелью позиций, чтобы перемещаться вперед или назад к новой должности в видео.

Для реализации пользовательской панели должно знать длительность мультимедиа и текущую точку воспроизведения. Эти данные доступны в свойствах [`Duration`](xref:Xamarin.Forms.MediaElement.Duration) и [`Position`](xref:Xamarin.Forms.MediaElement.Position) .

> [!IMPORTANT]
> Чтобы получить точные данные о положении, [`Position`](xref:Xamarin.Forms.MediaElement.Position) необходимо опросить. Дополнительные сведения см. в разделе [опрос данных о положении](#poll-for-position-data).

Настраиваемую строку позиционирования можно реализовать с помощью [`Slider`](xref:Xamarin.Forms.Slider), как показано в следующем примере:

```csharp
public class PositionSlider : Slider
{
    public static readonly BindableProperty DurationProperty =
        BindableProperty.Create(nameof(Duration), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(1),
                                propertyChanged: (bindable, oldValue, newValue) =>
                                {
                                    ((PositionSlider)bindable).SetTimeToEnd();
                                    double seconds = ((TimeSpan)newValue).TotalSeconds;
                                    ((Slider)bindable).Maximum = seconds <= 0 ? 1 : seconds;
                                });

    public TimeSpan Duration
    {
        get { return (TimeSpan)GetValue(DurationProperty); }
        set { SetValue(DurationProperty, value); }
    }

    public static readonly BindableProperty PositionProperty =
        BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(0),
                                propertyChanged: (bindable, oldValue, newValue) => ((PositionSlider)bindable).SetTimeToEnd());

    public TimeSpan Position
    {
        get { return (TimeSpan)GetValue(PositionProperty); }
        set { SetValue(PositionProperty, value); }
    }

    static readonly BindablePropertyKey TimeToEndPropertyKey =
        BindableProperty.CreateReadOnly(nameof(TimeToEnd), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan());

    public static readonly BindableProperty TimeToEndProperty = TimeToEndPropertyKey.BindableProperty;

    public TimeSpan TimeToEnd
    {
        get { return (TimeSpan)GetValue(TimeToEndProperty); }
        private set { SetValue(TimeToEndPropertyKey, value); }
    }

    public PositionSlider()
    {
        PropertyChanged += (sender, args) =>
        {
            if (args.PropertyName == "Value")
            {
                TimeSpan newPosition = TimeSpan.FromSeconds(Value);
                if (Math.Abs(newPosition.TotalSeconds - Position.TotalSeconds) / Duration.TotalSeconds > 0.01)
                {
                    Position = newPosition;
                }
            }
        };
    }

    void SetTimeToEnd()
    {
        TimeToEnd = Duration - Position;
    }
}
```

Класс `PositionSlider` определяет собственные `Duration` и `Position` связываемые свойства, а также свойство с возможностью привязки `TimeToEnd`. Все три свойства имеют тип `TimeSpan`. Обработчик изменения свойства для свойства `Duration` задает для свойства `Maximum` [`Slider`](xref:Xamarin.Forms.Slider) свойство `TotalSeconds` значения `TimeSpan`. Свойство `TimeToEnd` вычисляется на основе изменений в свойствах `Duration` и `Position` и начинается с длительности носителя и уменьшается до нуля по мере того, как воспроизведение продолжается.

`PositionSlider` обновляется с базовой [`Slider`](xref:Xamarin.Forms.Slider) при перемещении `Slider`, чтобы указать, что носитель должен быть расширен или реверсирован до новой должности. Это обнаруживается в обработчике `PropertyChanged` конструктора `PositionSlider`. Обработчик проверяет наличие изменений в свойстве `Value`. Если оно отличается от свойства `Position`, то свойству `Position` присваивается значение свойства `Value`. Дополнительные сведения об использовании [`Slider`](xref:Xamarin.Forms.Slider) см. в разделе [ползунок Xamarin. Forms.](~/xamarin-forms/user-interface/slider.md)

> [!NOTE]
> В Android [`Slider`](xref:Xamarin.Forms.Slider) содержит только 1000 дискретных шага, независимо от параметров `Minimum` и `Maximum`. Если длина носителя превышает 1000 секунд, то два разных `Position` значения будут соответствовать одному и тому же `Value` `Slider`. Именно поэтому вышеприведенный код проверяет, что новая и существующая должности больше одной сотый доли общей длительности.

В следующем примере показано использование `PositionSlider` на странице:

```xaml
<controls:PositionSlider x:Name="positionSlider"
                         BindingContext="{x:Reference mediaElement}"
                         Duration="{Binding Duration}"
                         ValueChanged="OnPositionSliderValueChanged">
    <controls:PositionSlider.Triggers>
        <DataTrigger TargetType="controls:PositionSlider"
                     Binding="{Binding CurrentState}"
                     Value="{x:Static MediaElementState.Buffering}">
            <Setter Property="IsEnabled" Value="False" />
        </DataTrigger>
    </controls:PositionSlider.Triggers>
</controls:PositionSlider>
```

В этом примере свойство `Duration` `PositionSlider` привязано к данным свойства [`Duration`](xref:Xamarin.Forms.MediaElement.Duration) [`MediaElement`](xref:Xamarin.Forms.MediaElement). При изменении свойства [`Value`](xref:Xamarin.Forms.Slider.Value) [`Slider`](xref:Xamarin.Forms.Slider) вызывается событие `ValueChanged` и выполняется обработчик `OnPositionSliderValueChanged`. Этот обработчик задает свойству [`MediaElement.Position`](xref:Xamarin.Forms.MediaElement.Position) значение свойства `PositionSlider.Position`. Таким образом, перетаскивание `Slider` приведет к изменению расположения воспроизведения мультимедиа:

[![Снимок экрана элемента MediaElement с настраиваемой строкой расположения в iOS и Android](mediaelement-images/custom-position-bar.png "MediaElement с настраиваемой строкой позиционирования")](mediaelement-images/custom-position-bar-large.png#lightbox "MediaElement с настраиваемой строкой позиционирования")

Кроме того, объект [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) используется для отключения `PositionSlider` при буферизации мультимедиа. Дополнительные сведения о триггерах данных см. в разделе [триггеры Xamarin. Forms](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="implement-a-custom-volume-control"></a>Реализация пользовательского элемента управления "Громкость"

Элементы управления воспроизведением мультимедиа, реализованные на каждой платформе, включают панель томов. Эта панель напоминает ползунок и показывает объем носителя. Кроме того, можно управлять панелью громкости, чтобы увеличить или уменьшить объем тома.

Настраиваемую панель томов можно реализовать с помощью [`Slider`](xref:Xamarin.Forms.Slider), как показано в следующем примере:

```xaml
<StackLayout>
    <MediaElement AutoPlay="False"
                  Source="{StaticResource AdvancedAsync}" />
    <Slider Maximum="1.0"
            Minimum="0.0"
            Value="{Binding Volume}"
            Rotation="270"
            WidthRequest="100" />
</StackLayout>
```

В этом примере [`Slider`](xref:Xamarin.Forms.Slider) данные привязывают свойство `Value` к свойству [`Volume`](xref:Xamarin.Forms.MediaElement.Volume) [`MediaElement`](xref:Xamarin.Forms.MediaElement). Это возможно потому, что свойство `Volume` использует привязку `TwoWay`. Поэтому изменение свойства `Value` приведет к изменению свойства `Volume`.

> [!NOTE]
> Свойство [`Volume`](xref:Xamarin.Forms.MediaElement.Volume) имеет обратный вызов влидатион, который гарантирует, что его значение больше или равно 0,0 и меньше или равно 1,0.

Дополнительные сведения об использовании [`Slider`](xref:Xamarin.Forms.Slider) см. в разделе [ползунок Xamarin. Forms.](~/xamarin-forms/user-interface/slider.md)

## <a name="related-links"></a>Связанные ссылки

- [Медиаелементдемос (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/MediaElementDemos)
- [Схемы URI](/windows/uwp/app-resources/uri-schemes)
- [Триггеры Xamarin. Forms](~/xamarin-forms/app-fundamentals/triggers.md)
- [Ползунок Xamarin. Forms](~/xamarin-forms/user-interface/slider.md)
- [Android: Поддерживаемые форматы мультимедиа](https://developer.android.com/guide/topics/media/media-formats)
- [UWP: Поддерживаемые кодеки](/windows/uwp/audio-video-camera/supported-codecs)
