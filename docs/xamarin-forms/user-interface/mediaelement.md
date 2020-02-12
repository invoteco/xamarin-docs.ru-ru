---
title: Элемент для Xamarin. Forms MediaElement
description: В этой статье объясняется, как использовать MediaElement для воспроизведения видео и аудио в приложении Xamarin. Forms.
ms.prod: xamarin
ms.assetid: e65f1e56-a80d-46c7-9ff4-7ae6650a3165
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/7/2020
ms.openlocfilehash: 67e4e9eec38f9dd23ebc3efe503d4b2a34ea8b39
ms.sourcegitcommit: 07941cf9704ff88cf4087de5ebdea623ff54edb1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/11/2020
ms.locfileid: "77145721"
---
# <a name="xamarinforms-mediaelement"></a>Элемент для Xamarin. Forms MediaElement

![](~/media/shared/preview.png "This API is currently pre-release")

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-samples/tree/pre-release/WorkingWithMediaElement)

`MediaElement` отображает проигрыватель мультимедиа для воспроизведения видео и аудио. Можно использовать элементы управления воспроизведением операционной системы, называемые элементами управления транспортом, или скрыть их и предоставить собственные элементы управления в соответствии с вашими требованиями к проектированию.

Прежде чем использовать новый элемент управления предварительной версии, необходимо использовать его, установив соответствующий флаг в `App.xaml.xs`:

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

Поддерживаемые платформы:

- Android
- iOS
- UWP
- WPF
- macOS
- Tizen

## <a name="set-media-source"></a>Задать источник носителя

Свойство `MediaElement` `Source` может принимать универсальный код ресурса (URI) или путь к локальному файлу. Воспроизведение начнется сразу после открытия носителя.

```xaml
<MediaElement Source="http://sec.ch9.ms/ch9/5d93/a1eab4bf-3288-4faf-81c4-294402a85d93/XamarinShow_mid.mp4" />
```

![](mediaelement-images/mediaelement-android.png "MediaElement on Android")

Чтобы задать источник для локального ресурса из проекта платформы, используйте схему универсального кода ресурса (URI) ms-appx.

```xaml
<MediaElement Source="ms-appx://XamarinShow_mid.mp4" />
```

При использовании привязки данных может потребоваться использовать преобразователь значений для применения этой схемы URI:

```csharp
public class VideoSourceConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value == null)
            return null;

        if (String.IsNullOrWhiteSpace(value.ToString()))
            return null;

        if (Device.RuntimePlatform == Device.UWP)
            return new Uri($"ms-appx:///Assets/{value}");
        else
            return new Uri($"ms-appx:///{value}");
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

```xaml
<MediaElement
    Source="{Binding VideoSource, Converter={StaticResource VideoSourceConverter}}" />
```

## <a name="control-playback"></a>Воспроизведение элемента управления

По умолчанию `MediaElement` будет использовать собственные элементы управления воспроизведением на платформах для воспроизведения и приостановки, громкости и отключения звука, поиска и времени.

Чтобы переопределить и реализовать собственные элементы управления, сначала отключите элементы управления платформой, установив `ShowsPlaybackControls="False"`. Теперь можно использовать собственные элементы управления для отражения состояния воспроизведения и управления воспроизведением с помощью следующих свойств, методов и событий.

- методы `Play()`, `Pause()`, `Stop()` для управления воспроизведением
- `AutoPlay` задать начальное поведение
- `CanSeek` для включения или отключения поиска
- свойства `Position` и `Duration` времени
- `Volume` свойство для изменения громкости и отключения звука
- `IsLooping` свойство для повторения после завершения воспроизведения
- `StateRequested`, `PositionRequested`, `VolumeRequested` для обработки обновлений пользовательского интерфейса

### <a name="handle-additional-media-events"></a>Обработку дополнительных событий мультимедиа

Вы можете отвечать на распространенные события мультимедиа, такие как события `MediaOpened`, `MediaEnded`, `MediaFailed`и `CurrentStateChanged`. Рекомендуется всегда выполнять обработку события Медиафаилед.
Куррентстатечанжед предоставляют сведения о состоянии воспроизведения. Используя значение перечисления `MediaElementState`:

- **Закрыто**: не содержит мультимедиа и отображает прозрачный фрейм
- **Открытие**: проверяется и выполняется попытка загрузки указанного источника
- **Буферизация**: загружается носитель для воспроизведения
  - Во время этого состояния его `Position` не выполняется.
  - Если видео уже воспроизводилось, отображается последний отображаемый кадр
- **Воспроизведение**: воспроизводит текущий источник мультимедиа
- **Приостановлено**: не выполняется смена `Position`
  - При воспроизведении видео текущий кадр будет отображаться
- **Остановлено**: содержит носитель, но не воспроизводится или приостановлен
  - Его `Position` 0 и не выполняется
  - Если загруженный носитель является видео, `MediaElement` отображает первый кадр.

## <a name="control-display"></a>Отображение элемента управления

Как и [`Image`](xref:Xamarin.Forms.Image), элемент управления `MediaElement` имеет `VideoHeight`, `VideoWidth`и `Aspect`. Аспект принимает три разных варианта:

- **Аспектфилл**: видео будет заполнять всю ширину и высоту элемента управления, часто суперсовременные за пределы границ элемента управления, сохраняя исходный аспект
- **Аспектфит**: видео будет помещаться в ширину и высоту элемента управления при сохранении исходного аспекта
- **Fill**: видео будет заполнять ширину и высоту элемента управления

## <a name="additional-properties"></a>Дополнительные свойства

- `VideoWidth`: ширина элемента управления
- `VideoHeight`: высота элемента управления
- `KeepScreenOn`: Если экран устройства должен остаться во время воспроизведения

## <a name="related-links"></a>Связанные ссылки

- [MediaElement (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/pre-release/WorkingWithMediaElement)
