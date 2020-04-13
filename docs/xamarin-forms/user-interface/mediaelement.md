---
title: Xamarin.Forms MediaElement
description: В этой статье объясняется, как использовать MediaElement для воспроизведения видео и аудио в приложении Xamarin.Forms.
ms.prod: xamarin
ms.assetid: e65f1e56-a80d-46c7-9ff4-7ae6650a3165
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/18/2020
ms.openlocfilehash: 6f6c51c428de569ceb09ed6a26cfc36881c86dc5
ms.sourcegitcommit: b93754b220fca3d6e3d131341e3cfbe233d10f84
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2020
ms.locfileid: "80628340"
---
# <a name="xamarinforms-mediaelement"></a>Xamarin.Forms MediaElement

![](~/media/shared/preview.png "This API is currently pre-release")

[![Скачать](~/media/shared/download.png) образец Скачать образец](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos/)

[`MediaElement`](xref:Xamarin.Forms.MediaElement)это вид для воспроизведения видео и аудио. Средства массовой информации, поддерживаемые базовой платформой, могут воспроизводиться из следующих источников:

- Веб, используя URI (HTTP или HTTPS).
- Ресурс, встроенный в приложение `ms-appx:///` платформы, используя схему URI.
- Файлы, которые поступают из локальных и временных папок данных приложения, используя схему `ms-appdata:///` URI.
- Библиотека устройства.

[`MediaElement`](xref:Xamarin.Forms.MediaElement)может использовать элементы управления воспроизведения платформы, которые называются транспортными элементами управления. Тем не менее, они отключены по умолчанию и могут быть заменены на ваш собственный транспортный контроль. На следующих `MediaElement` скриншотах показана игра видео с управлением транспортом платформы:

[![Скриншот воспроизведения видео MediaElement на iOS и Android](mediaelement-images/playback-controls.png "MediaElement воскнет видео")](mediaelement-images/playback-controls-large.png#lightbox "MediaElement воскнет видео")

[`MediaElement`](xref:Xamarin.Forms.MediaElement)доступен в Xamarin.Forms 4.5. Тем не менее, в настоящее время он является экспериментальным и может быть использован только путем добавления следующей строки кода в файл *App.xaml.cs:*

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

> [!NOTE]
> [`MediaElement`](xref:Xamarin.Forms.MediaElement)доступна на iOS, Android, универсальной платформе Windows (UWP), macOS, Windows Presentation Foundation и Tizen.

[`MediaElement`](xref:Xamarin.Forms.MediaElement)определяет следующие свойства:

- [`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect), типа, [`Aspect`](xref:Xamarin.Forms.Aspect)определяет, как средства массовой информации будут масштабироваться в соответствии с областью отображения. Значение по умолчанию этого свойства равно `AspectFit`.
- [`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay), типа, `bool`указывает, начнется ли воспроизведение мультимедиа автоматически при установке [`Source`](xref:Xamarin.Forms.MediaElement.Source) свойства. Значение по умолчанию этого свойства равно `true`.
- [`BufferingProgress`](xref:Xamarin.Forms.MediaElement.BufferingProgress), типа, `double`указывает на текущий прогресс буферизации. Значение этого свойства по умолчанию составляет 0,0.
- [`CanSeek`](xref:Xamarin.Forms.MediaElement.CanSeek), типа, `bool`указывает, можно ли переместить мультимедиа, [`Position`](xref:Xamarin.Forms.MediaElement.Position) установив значение свойства. Это свойство доступно только для чтения.
- [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState), типа, [`MediaElementState`](xref:Xamarin.Forms.MediaElementState)указывает текущее состояние элемента управления. Это свойство только для чтения, `MediaElementState.Closed`значение по умолчанию которого .
- [`Duration`](xref:Xamarin.Forms.MediaElement.Duration), типа, `TimeSpan?`указывает продолжительность в настоящее время открытых носителей. Это свойство только для чтения, `null`значение по умолчанию которого .
- [`IsLooping`](xref:Xamarin.Forms.MediaElement.IsLooping), типа `bool`, описывает ли в настоящее время загруженный источник мультимедиа должен возобновить воспроизведение с самого начала после достижения его конца. Значение по умолчанию этого свойства равно `false`.
- [`KeepScreenOn`](xref:Xamarin.Forms.MediaElement.KeepScreenOn), типа, `bool`определяет, должен ли экран устройства оставаться на во время воспроизведения мультимедиа. Значение по умолчанию этого свойства равно `false`.
- [`Position`](xref:Xamarin.Forms.MediaElement.Position), типа `TimeSpan`, описывает текущий прогресс через время воспроизведения средств массовой информации. Значение по умолчанию этого свойства равно `TimeSpan.Zero`.
- [`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls), типа, `bool`определяет, отображаются ли элементы управления воспроизведения платформ. Значение по умолчанию этого свойства равно `false`.
- [`Source`](xref:Xamarin.Forms.MediaElement.Source), типа, [`MediaSource`](xref:Xamarin.Forms.MediaSource)указывает источник носителя, загруженного в элемент управления.
- [`VideoHeight`](xref:Xamarin.Forms.MediaElement.VideoHeight), типа, `int`указывает на высоту элемента управления. Это свойство доступно только для чтения.
- [`VideoWidth`](xref:Xamarin.Forms.MediaElement.VideoWidth), типа, `int`указывает на ширину элемента управления. Это свойство доступно только для чтения.
- [`Volume`](xref:Xamarin.Forms.MediaElement.Volume), типа, `double`определяет объем носителя, который представлен по линейной шкале между 0 и 1. Это свойство `TwoWay` использует привязку, а его значение по умолчанию составляет 1.

Эти свойства, за исключением `CanSeek` свойства, [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) поддерживаются объектами, что означает, что они могут быть мишенями привязки данных и стилизованы.

Класс [`MediaElement`](xref:Xamarin.Forms.MediaElement) также определяет четыре события:

- [`MediaOpened`](xref:Xamarin.Forms.MediaElement.MediaOpened)увольняется, когда поток мультимедиа был проверен и открыт.
- [`MediaEnded`](xref:Xamarin.Forms.MediaElement.MediaEnded)увольняется, `MediaElement` когда заканчивается играть свои средства массовой информации.
- [`MediaFailed`](xref:Xamarin.Forms.MediaElement.MediaFailed)увольняется при ошибке, связанной с источником мультимедиа.
- [`SeekCompleted`](xref:Xamarin.Forms.MediaElement.SeekCompleted)увольняется, когда точка поиска запрашиваемых операций поиска готова к воспроизведению.

Кроме [`MediaElement`](xref:Xamarin.Forms.MediaElement) того, [`Play`](xref:Xamarin.Forms.MediaElement.Play) [`Pause`](xref:Xamarin.Forms.MediaElement.Pause)включает [`Stop`](xref:Xamarin.Forms.MediaElement.Stop) в себя, и методы.

Информацию о поддерживаемых медиаформатах на Android [можно](https://developer.android.com/guide/topics/media/media-formats) узнать на developer.android.com. Для получения информации о поддерживаемых форматах мультимедиа [Supported codecs](/windows/uwp/audio-video-camera/supported-codecs)на универсальной платформе Windows (UWP) см.

## <a name="play-remote-media"></a>Воспроизведение удаленных носителей

A [`MediaElement`](xref:Xamarin.Forms.MediaElement) может воспроизводить удаленные медиафайлы, используя схемы HTTP и HTTPS URI. Это достигается путем [`Source`](xref:Xamarin.Forms.MediaElement.Source) установки свойства uri медиафайла:

```xaml
<MediaElement Source="https://sec.ch9.ms/ch9/5d93/a1eab4bf-3288-4faf-81c4-294402a85d93/XamarinShow_mid.mp4"
              ShowsPlaybackControls="True" />
```

По умолчанию средства массовой [`Source`](xref:Xamarin.Forms.MediaElement.Source) информации, определяемые свойством, воспроизводится сразу после открытия носителя. Чтобы подавить автоматическое воспроизведение мультимедиа, установите свойство [`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay) на `false`.

Элементы воспроизведения мультимедиа отключены по [`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls) умолчанию `true`и включены при настройке свойства для. [`MediaElement`](xref:Xamarin.Forms.MediaElement)затем будет использовать элементы управления воспроизведения платформы.

## <a name="play-local-media"></a>Воспроизведение местных сми

Местные средства массовой информации могут быть воспроизведены из следующих источников:

- Ресурс, встроенный в приложение `ms-appx:///` платформы, используя схему URI.
- Файлы, которые поступают из локальных и временных папок данных приложения, используя схему `ms-appdata:///` URI.
- Библиотека устройства.

Для получения дополнительной информации об [URI schemes](/windows/uwp/app-resources/uri-schemes)этих схемах URI см.

### <a name="play-media-embedded-in-the-app-package"></a>Воспроизведение мультимедиа, встроенных в пакет приложений

A [`MediaElement`](xref:Xamarin.Forms.MediaElement) может воспроизводить медиафайлы, встроенные `ms-appx:///` в пакет приложений, используя схему URI. Медиафайлы встраиваются в пакет приложений, размещая их в проекте платформы.

Хранение медиафайла в проекте платформы отличается для каждой платформы:

- На iOS файлы мультимедиа должны храниться в папке **Ресурсов** или в подфедере папки **Ресурсов.** Файл средств массовой `Build Action` `BundleResource`информации должен иметь .
- На Android медиафайлы должны храниться в подфайле **Ресурсов,** названных **необработанными.** В папке **raw** не может быть вложенных папок. Файл средств массовой `Build Action` `AndroidResource`информации должен иметь .
- На UWP файлы мультимедиа могут храниться в любой папке проекта. Файл средств массовой `BuildAction` `Content`информации должен иметь .

Файлы мультимедиа, отвечающие этим критериям, могут быть воспроизведены с помощью схемы `ms-appx:///` URI:

```xaml
<MediaElement Source="ms-appx:///XamarinForms101UsingEmbeddedImages.mp4"
              ShowsPlaybackControls="True" />
```

При использовании связывания данных для применения этой схемы URI можно использовать преобразователь значений:

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

Экземпляр `VideoSourceConverter` может быть использован для применения `ms-appx:///` схемы URI к встроенного файла мультимедиа:

```xaml
<MediaElement Source="{Binding MediaSource, Converter={StaticResource VideoSourceConverter}}"
              ShowsPlaybackControls="True" />
```

Для получения дополнительной информации о схеме ms-appx URI [см.](/windows/uwp/app-resources/uri-schemes#ms-appx-and-ms-appx-web)

### <a name="play-media-from-the-apps-local-and-temporary-folders"></a>Воспроизведение мультимедиа из локальных и временных папок приложения

A [`MediaElement`](xref:Xamarin.Forms.MediaElement) может воспроизводить файлы мультимедиа, которые копируются в `ms-appdata:///` локальные или временные папки данных приложения, используя схему URI.

Ниже приводится [`Source`](xref:Xamarin.Forms.MediaElement.Source) приведение набора свойств в файл мультимедиа, который хранится в локальной папке данных приложения:

```xaml
<MediaElement Source="ms-appdata:///local/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

Ниже приводится [`Source`](xref:Xamarin.Forms.MediaElement.Source) свойство к файлу мультимедиа, который хранится во временной папке данных приложения:

```xaml
<MediaElement Source="ms-appdata:///temp/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

> [!IMPORTANT]
> В дополнение к воспроизведению файлов мультимедиа, которые хранятся в локальных или временных папках данных приложения, UWP также может воспроизводить медиафайлы, которые находятся в папке роуминга приложения. Это может быть достигнуто путем `ms-appdata:///roaming/`предварительной фиксации файла мультимедиа с .

При использовании связывания данных для применения этой схемы URI можно использовать преобразователь значений:

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

Экземпляр `VideoSourceConverter` может быть использован для применения `ms-appdata:///` схемы URI к файлу мультимедиа в локальной или временной папке данных приложения:

```xaml
<MediaElement Source="{Binding MediaSource, Converter={StaticResource VideoSourceConverter}}"
              ShowsPlaybackControls="True" />
```

Для получения дополнительной информации о схеме ms-appdata URI [см.](/windows/uwp/app-resources/uri-schemes#ms-appdata)

#### <a name="copying-a-media-file-to-the-apps-local-or-temporary-data-folder"></a>Копирование файла мультимедиа в локальную или временную папку данных приложения

Воспроизведение файла мультимедиа, хранящегося в локальной или временной папке данных приложения, требует, чтобы файл мультимедиа был скопирован там приложением. Это может быть достигнуто, например, путем копирования медиафайла из пакета приложений:

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
> В приведенном выше `FileSystem` примере кода используется класс, включенный в Xamarin.Essentials. Для получения дополнительной информации [см.](~/essentials/file-system-helpers.md?context=xamarin%2Fxamarin-forms&tabs=android)

### <a name="play-media-from-the-device-library"></a>Воспроизведение мультимедиа из библиотеки устройств

Большинство современных мобильных устройств и настольных компьютеров имеют возможность записывать видео и аудио с помощью камеры и микрофона устройства. Создаваемые средства массовой информации затем хранятся в виде файлов на устройстве. Эти файлы могут быть извлечены из [`MediaElement`](xref:Xamarin.Forms.MediaElement)библиотеки и воспроизведены .

Каждая из платформ включает в себя средство, которое позволяет пользователю выбирать мультимедиа из библиотеки устройства. В Xamarin.Forms проекты платформмогут вызывать эту функциональность, и [`DependencyService`](xref:Xamarin.Forms.DependencyService) ее можно вызывать по классу.

Служба подбора видео, используемая в примере приложения, очень похожа на услугу, определенную при [выборе фотографии из библиотеки изображений,](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)за исключением того, что сборщик возвращает имя файла, а не `Stream` объект. Общий код проекта определяет `IVideoPicker`интерфейс под названием , `GetVideoFileAsync`который определяет один метод под названием . Затем каждая платформа реализует `VideoPicker` этот интерфейс в классе.

Следующий пример кода показывает, как извлечь файл мультимедиа из библиотеки устройств:

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

Служба подбора видео-зависимости вызывается, вызывая `DependencyService.Get` метод `IVideoPicker` для получения реализации интерфейса в проекте платформы. Затем `GetVideoFileAsync` метод вызывается на этом экземпляре, и возвращенное имя файла используется для создания [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) объекта и установки его в [`Source`](xref:Xamarin.Forms.MediaElement.Source) свойство [`MediaElement`](xref:Xamarin.Forms.MediaElement).

## <a name="change-video-aspect-ratio"></a>Изменение соотношения видеоаспектов

Свойство [`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect) определяет, как видео-медиа будут масштабироваться в соответствии с областью отображения. По умолчанию это свойство `AspectFit` устанавливается для участника перечисления, но [`Aspect`](xref:Xamarin.Forms.Aspect) может быть установлено любому из участников перечисления:

- `AspectFit`указывает на то, что видео будет иметь почтовые ящики, если это необходимо, чтобы вписаться в область дисплея, сохраняя при этом соотношение сторон.
- `AspectFill`указывает на то, что видео будет обрезано так, что оно заполняет область дисплея, сохраняя при этом соотношение сторон.
- `Fill`указывает на то, что видео будет растянуто для заполнения зоны дисплея.

## <a name="poll-for-position-data"></a>Опрос данных о позиции

Уведомление об изменении [`Position`](xref:Xamarin.Forms.MediaElement.Position) свойства для связываемого свойства только в ключевые моменты, такие как начало и окончание воспроизведения и происходит пауза. Таким образом, привязка данных к свойству не даст точных данных о положении. `Position` Вместо этого необходимо настроить таймер и опросить свойство.

Хорошее место для этого `OnAppearing` находится в переопределение для страницы, которая требует данных о позиции, как средства массовой информации играет:

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

В этом примере `OnAppearing` переопределение запускает таймер, который обновляется `positionLabel` со значением каждую секунду. `Position` Обратный вызов таймер вызывается каждую секунду, пока обратный вызов не вернется. `false` При навигации `OnDisappearing` страницы выполняется переопределение, что останавливает вызов обратного вызова таймера.

## <a name="understand-mediasource-types"></a>Понимание типов MediaSource

A [`MediaElement`](xref:Xamarin.Forms.MediaElement) может воспроизводить [`Source`](xref:Xamarin.Forms.MediaElement.Source) мультимедиа, установив свое свойство в удаленном или локальном файле мультимедиа. Свойство `Source` типа, [`MediaSource`](xref:Xamarin.Forms.MediaSource)и этот класс определяет два статических метода:

- [`FromFile`](xref:Xamarin.Forms.MediaSource.FromFile*), возвращает [`MediaSource`](xref:Xamarin.Forms.MediaSource) экземпляр `string` из аргумента.
- [`FromUri`](xref:Xamarin.Forms.MediaSource.FromUri*), возвращает [`MediaSource`](xref:Xamarin.Forms.MediaSource) экземпляр `Uri` из аргумента.

Кроме того, [`MediaSource`](xref:Xamarin.Forms.MediaSource) в классе также `MediaSource` есть неявные операторы, которые возвращают экземпляры и `string` `Uri` аргументы.

> [!NOTE]
> При [`Source`](xref:Xamarin.Forms.MediaElement.Source) установке свойства в XAML используется преобразователь [`MediaSource`](xref:Xamarin.Forms.MediaSource) типа `string` для `Uri`возврата экземпляра из или .

Класс [`MediaSource`](xref:Xamarin.Forms.MediaSource) также имеет два класса, полученных:

- [`UriMediaSource`](xref:Xamarin.Forms.UriMediaSource), который используется для указания удаленного файла мультимедиа из URI. Этот класс [`Uri`](xref:Xamarin.Forms.UriMediaSource.Uri) имеет свойство, которое может быть установлено на `Uri`.
- [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource), который используется для указания локального `string`файла мультимедиа из . Этот класс [`File`](xref:Xamarin.Forms.FileMediaSource.File) имеет свойство, которое может быть установлено на `string`. Кроме того, этот класс имеет `string` неявных операторов для преобразования в `FileMediaSource` объект, и `FileMediaSource` объект в `string`.

> [!NOTE]
> При [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) создании объекта в XAML используется преобразователь [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) типа для `string`возвращения экземпляра из .

## <a name="determine-mediaelement-status"></a>Определение статуса MediaElement

Класс [`MediaElement`](xref:Xamarin.Forms.MediaElement) определяет передевоприми только [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState)для чтения [`MediaElementState`](xref:Xamarin.Forms.MediaElementState)свойство с именем типа. Это свойство указывает текущее состояние элемента управления, например, играет ли носители или приостанавливается, или если оно еще не готово к воспроизведению носителя.

Перечисление [`MediaElementState`](xref:Xamarin.Forms.MediaElementState) определяет следующих членов:

- `Closed`указывает на `MediaElement` то, что носители не содержат средств массовой информации.
- `Opening`указывает на `MediaElement` то, что проверка и попытка загрузки указанного источника.
- `Buffering`указывает на `MediaElement` загрузку носителя для воспроизведения. Его [`Position`](xref:Xamarin.Forms.MediaElement.Position) собственность не продвигается в течение этого состояния. Если `MediaElement` воспроизведенное видео, оно продолжает отображать последнюю отображаемую рамку.
- `Playing`указывает на `MediaElement` то, что играет источник мультимедиа.
- `Paused`указывает на `MediaElement` то, [`Position`](xref:Xamarin.Forms.MediaElement.Position) что не продвигает свое имущество. Если `MediaElement` воспроизведенное видео, оно продолжает отображать текущий кадр.
- `Stopped`указывает на `MediaElement` то, что носит средства массовой информации, но он не воспроизводится или не приостанавливается. Его [`Position`](xref:Xamarin.Forms.MediaElement.Position) собственность 0 и не заранее. Если загруженные носители `MediaElement` — это видео, отображается первый кадр.

Как правило, нет необходимости [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState) осматривать [`MediaElement`](xref:Xamarin.Forms.MediaElement) свойство при использовании транспортного контроля. Однако это свойство становится важным при реализации собственного управления транспортом.

## <a name="implement-custom-transport-controls"></a>Внедрение пользовательских транспортных средств управления

Управление транспортом медиаплеера включает кнопки, **Play**выполняющие функции Воспроизведение, **Пауза**и **Остановка.** Эти кнопки обычно определяются по знакомым значкам, а не тексту. Как правило, функции **Воспроизведение** и **Пауза** объединены в одну кнопку.

По умолчанию [`MediaElement`](xref:Xamarin.Forms.MediaElement) элементы управления воспроизведения отключены. Это позволяет управлять `MediaElement` программным управлением или поставлять собственный транспортный контроль. В поддержку `MediaElement` этого, [`Pause`](xref:Xamarin.Forms.MediaElement.Pause)включает [`Stop`](xref:Xamarin.Forms.MediaElement.Stop) в себя [`Play`](xref:Xamarin.Forms.MediaElement.Play), и методы.

На следующем примере XAML показана страница, содержащая пользовательские элементы управления транспортом: [`MediaElement`](xref:Xamarin.Forms.MediaElement)

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

В этом примере пользовательские элементы управления транспортом определяются как [`Button`](xref:Xamarin.Forms.Button) объекты. Однако, Есть `Button` только два объекта, с `Button` первым представляющим `Button` **Play** и **паузы**, а второй представляющих **Стоп**. [`DataTrigger`](xref:Xamarin.Forms.DataTrigger)объекты используются для включения и отключения кнопок, а также для переключения первой кнопки между **Воспроизведением** и **Паузой.** Для получения дополнительной информации о триггерах данных, [см.](~/xamarin-forms/app-fundamentals/triggers.md)

В файле, закодированном [`Clicked`](xref:Xamarin.Forms.Button.Clicked) файле есть обработчики событий:

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

Кнопка **Воспроизведения** может быть нажата, как только она становится включена, чтобы начать воспроизведение:

[![Скриншот MediaElement с пользовательским транспортным элементом на iOS и Android](mediaelement-images/custom-transport-playback.png "MediaElement воскнет видео")](mediaelement-images/custom-transport-playback-large.png#lightbox "MediaElement воскнет видео")

Нажатие кнопки **«Пауза»** приводит к приостановке воспроизведения:

[![Скриншот MediaElement с приостановкой воспроизведения на iOS и Android](mediaelement-images/custom-transport-paused.png "MediaElement с приостановленным видео")](mediaelement-images/custom-transport-paused-large.png#lightbox "MediaElement с приостановленным видео")

Нажатие кнопки **Стоп** останавливает воспроизведение и возвращает положение файла мультимедиа к началу.

## <a name="implement-a-custom-position-bar"></a>Реализация пользовательской панели позиции

На каждой платформе реализуются элементы управления транспортировкой, в том числе строка позиционирования. Этот бар напоминает ползунок и показывает текущее местоположение носителя в пределах его общей продолжительности. Кроме того, вы можете манипулировать позиционным баром, чтобы двигаться вперед или назад в новое положение в видео.

Реализация пользовательской панели позиции требует знания продолжительности носителя и текущей позиции воспроизведения. Эти данные доступны [`Duration`](xref:Xamarin.Forms.MediaElement.Duration) [`Position`](xref:Xamarin.Forms.MediaElement.Position) в свойствах.

> [!IMPORTANT]
> Необходимо [`Position`](xref:Xamarin.Forms.MediaElement.Position) опросить, чтобы получить точные данные о положении. Для получения дополнительной информации, см [Опрос для позиции данных](#poll-for-position-data).

Пользовательская панель положения может [`Slider`](xref:Xamarin.Forms.Slider)быть реализована с помощью , как показано в следующем примере:

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

Класс `PositionSlider` определяет свои `Duration` собственные `Position` и связываемые свойства, а также `TimeToEnd` связываемое свойство. Все три свойства `TimeSpan`типа. Обработчик свойства, `Duration` измененный `Maximum` для свойства, устанавливает свойство [`Slider`](xref:Xamarin.Forms.Slider) свойства к свойству `TotalSeconds` стоимости. `TimeSpan` Свойство `TimeToEnd` рассчитывается на основе `Duration` `Position` изменений в свойствах и начинается с длительности носителя и уменьшается до нуля по мере продолжения воспроизведения.

Обновление `PositionSlider` от базового [`Slider`](xref:Xamarin.Forms.Slider) при `Slider` перемещении, чтобы указать, что средства массовой информации должны быть расширены или обращены в новую позицию. Это обнаруживается в `PropertyChanged` обработчике в конструкторе. `PositionSlider` Обработчик проверяет наличие изменений в свойстве `Value`. Если оно отличается от свойства `Position`, то свойству `Position` присваивается значение свойства `Value`. Для получения дополнительной [`Slider`](xref:Xamarin.Forms.Slider) информации об использовании см., [Xamarin.Forms Slider](~/xamarin-forms/user-interface/slider.md)

> [!NOTE]
> На Android, [`Slider`](xref:Xamarin.Forms.Slider) только имеет 1000 дискретных `Minimum` шагов, независимо от `Maximum` и настроек. Если длина носителя превышает 1000 секунд, `Position` то два разных `Value` значения `Slider`будут соответствовать тому же . Вот почему приведенный выше код проверяет, что новое положение и существующее положение превышают одну сотую от общей продолжительности.

Ниже приводится `PositionSlider` следующий пример, повысиваемый на странице:

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

В `Duration` этом примере свойство `PositionSlider` данных связано с свойством [`Duration`](xref:Xamarin.Forms.MediaElement.Duration) [`MediaElement`](xref:Xamarin.Forms.MediaElement). При [`Value`](xref:Xamarin.Forms.Slider.Value) [`Slider`](xref:Xamarin.Forms.Slider) изменении свойства `ValueChanged` событие сражается, и `OnPositionSliderValueChanged` обработчик выполняется. Этот обработчик устанавливает [`MediaElement.Position`](xref:Xamarin.Forms.MediaElement.Position) свойство `PositionSlider.Position` к значению свойства. Таким образом, `Slider` перетаскивание результатов в позиции воспроизведения мультимедиа меняется:

[![Скриншот MediaElement с пользовательской позиционной панелью на iOS и Android](mediaelement-images/custom-position-bar.png "MediaElement с пользовательской позиционной панелью")](mediaelement-images/custom-position-bar-large.png#lightbox "MediaElement с пользовательской позиционной панелью")

Кроме того, [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) объект используется для отсутствования `PositionSlider` буферизации носителя. Для получения дополнительной информации о триггерах данных, [см.](~/xamarin-forms/app-fundamentals/triggers.md)

## <a name="implement-a-custom-volume-control"></a>Внедрение пользовательского управления громкости

Элементы воспроизведения мультимедиа, реализуемые каждой платформой, включают в себя панель громкости. Этот бар напоминает ползунок и показывает объем носителя. Кроме того, можно манипулировать панелью громкости, чтобы увеличить или уменьшить громкость.

Пользовательская панель громкости может [`Slider`](xref:Xamarin.Forms.Slider)быть реализована с помощью , как показано в следующем примере:

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

В этом примере [`Slider`](xref:Xamarin.Forms.Slider) данные `Value` связывают [`Volume`](xref:Xamarin.Forms.MediaElement.Volume) свое свойство с собственностью [`MediaElement`](xref:Xamarin.Forms.MediaElement). Это возможно, `Volume` поскольку `TwoWay` в свойстве используется связывание. Таким образом, `Value` изменение свойства `Volume` приведет к изменению свойства.

> [!NOTE]
> Свойство [`Volume`](xref:Xamarin.Forms.MediaElement.Volume) имеет vlidation callback, который гарантирует, что его значение больше или равно 0,0, и меньше, чем или равна 1,0.

Для получения дополнительной [`Slider`](xref:Xamarin.Forms.Slider) информации об использовании см., [Xamarin.Forms Slider](~/xamarin-forms/user-interface/slider.md)

## <a name="related-links"></a>Связанные ссылки

- [MediaElementDemos (образец)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos/)
- [Схемы URI](/windows/uwp/app-resources/uri-schemes)
- [Триггеры Xamarin.Forms](~/xamarin-forms/app-fundamentals/triggers.md)
- [Слайдер Xamarin.Forms](~/xamarin-forms/user-interface/slider.md)
- [Android: Поддерживаемые медиа форматы](https://developer.android.com/guide/topics/media/media-formats)
- [UWP: Поддерживаемые кодеки](/windows/uwp/audio-video-camera/supported-codecs)
