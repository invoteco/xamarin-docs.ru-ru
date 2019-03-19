---
title: 'Xamarin.Essentials: Общий доступ'
description: Класс Share в Xamarin.Essentials позволяет приложению обмениваться данными, такими как текст и веб-ссылки, с другими приложениями на устройстве.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 02/12/2019
ms.custom: video
ms.openlocfilehash: ad56a626133e03c1ca75b1db26b0904d5df7fea3
ms.sourcegitcommit: 64d6da88bb6ba222ab2decd2fdc8e95d377438a6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/18/2019
ms.locfileid: "58175334"
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

## <a name="api"></a>API

- [Исходный код Share](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Share)
- [Документация по API для Share](xref:Xamarin.Essentials.Share)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Share-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
