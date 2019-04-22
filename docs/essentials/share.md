---
title: 'Xamarin.Essentials: Общий доступ'
description: Класс Share в Xamarin.Essentials позволяет приложению обмениваться данными, такими как текст и веб-ссылки, с другими приложениями на устройстве.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 04/02/2019
ms.custom: video
ms.openlocfilehash: 1a9a7b008773255d9d7743a4fcb21f02feb3e116
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2019
ms.locfileid: "58869381"
---
# <a name="xamarinessentials-share"></a>Xamarin.Essentials: Общий доступ

Класс **Share** позволяет приложению обмениваться данными, такими как текст и веб-ссылки, с другими приложениями на устройстве.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-share"></a>Использование класса Share

Добавьте в свой класс ссылку на Xamarin.Essentials:

```csharp
using Xamarin.Essentials;
```

Чтобы использовать функции класса Share, вызовите метод `RequestAsync` с полезными данными запроса, включающего сведения для обмена с другими приложениями. Текст и URI могут сочетаться, и каждая платформа будет выполнять фильтрацию по содержимому.

```csharp

public class ShareTest
{
    public async Task ShareText(string text)
    {
        await Share.RequestAsync(new ShareTextRequest
            {
                Text = text,
                Title = "Share Text"
            });
    }

    public async Task ShareUri(string uri)
    {
        await Share.RequestAsync(new ShareTextRequest
            {
                Uri = uri,
                Title = "Share Web Link"
            });
    }
}
```

Пользовательский интерфейс для общего использования с внешним приложением, отображаемый при запросе:

![Общий доступ](images/share.png)

## <a name="platform-differences"></a>Различия платформ

# <a name="androidtabandroid"></a>[Android](#tab/android)

* Свойство `Subject` используется для требуемой темы сообщения.

# <a name="iostabios"></a>[iOS](#tab/ios)

* `Subject` не используется.
* `Title` не используется.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* В `Title` будет использоваться значение по умолчанию (имя приложения), если значение не задано.
* `Subject` не используется.

-----

## <a name="files"></a>Файлы

![Предварительная версия](~/media/shared/preview.png)

Общий доступ к файлам доступен как экспериментальная предварительная версия в Xamarin.Essentials версии 1.1.0. Эта функция позволяет приложению предоставлять общий доступ к файлам для других приложений на устройстве. Чтобы включить эту функцию, задайте следующее свойство в коде запуска приложения.

```csharp
ExperimentalFeatures.Enable(ExperimentalFeatures.ShareFileRequest);
```

После включения этой функции можно предоставить общий доступ к любому файлу. Xamarin.Essentials автоматически обнаруживает тип файла (MIME) и запрашивает его добавление в общий доступ. Каждая платформа может поддерживать только определенные расширения файлов.

Ниже приведен пример записи текста на диск и предоставления общего доступа к нему другим приложениям.

```csharp
var fn =  "Attachment.txt";
var file = Path.Combine(FileSystem.CacheDirectory, fn);
File.WriteAllText(file, "Hello World");

await Share.RequestAsync(new ShareFileRequest
{
    Title = Title,
    File = new ShareFile(file)
});
```

## <a name="api"></a>API

- [Исходный код Share](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Share)
- [Документация по API для Share](xref:Xamarin.Essentials.Share)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Share-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
