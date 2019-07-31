---
title: Поиск с основными Spotlight в Xamarin. iOS
description: В этом документе описывается, как использовать основное Spotlight в приложении Xamarin. iOS для предоставления ссылок на содержимое в приложении. В нем обсуждается создание, восстановление, обновление и удаление элементов с возможностью поиска.
ms.prod: xamarin
ms.assetid: 1374914C-0F63-41BF-BD97-EBCEE86E57B1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: acf065a275b28863c5133f764a7f7b1f87127887
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68654211"
---
# <a name="search-with-core-spotlight-in-xamarinios"></a>Поиск с основными Spotlight в Xamarin. iOS

Основное внимание — это новая платформа для iOS 9, которая предоставляет API, аналогичный базе данных, для добавления, изменения и удаления ссылок на содержимое в приложении. Элементы, добавленные с помощью ключевого прожектора Core, будут доступны в поиске Spotlight на устройстве iOS.

Пример типов содержимого, которые можно индексировать с помощью ключевого внимания Core, см. в сообщениях Apple, mail, Calendar и Notes. В настоящее время они используют основное Spotlight для предоставления результатов поиска.

## <a name="creating-an-item"></a>Создание элемента

Ниже приведен пример создания элемента и его индексирования с помощью ключевого внимания Core:

```csharp
using CoreSpotlight;
...

// Create attributes to describe an item
var attributes = new CSSearchableItemAttributeSet();
attributes.Title = "App Center Test";
attributes.ContentDescription = "Automatically test your app on 1,000 devices in the cloud.";

// Create item
var item = new CSSearchableItem ("1", "products", attributes);

// Index item
CSSearchableIndex.DefaultSearchableIndex.Index (new CSSearchableItem[]{ item }, (error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

Эти сведения будут выглядеть следующим образом в результатах поиска:

[![](corespotlight-images/corespotlight01.png "Обзор основных результатов поиска Spotlight")](corespotlight-images/corespotlight01.png#lightbox)

## <a name="restoring-an-item"></a>Восстановление элемента

Когда пользователь отменяет элемент, добавленный в результаты поиска через основное внимание для вашего приложения, `AppDelegate` вызывается метод `ContinueUserActivity` (этот метод также используется для `NSUserActivity`). Например:

```csharp
public override bool ContinueUserActivity (UIApplication application,
   NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    default:
        if (userActivity.ActivityType == CSSearchableItem.ActionType.ToString ()) {
            // Display content for searchable item...
        }
        break;
    }

    return true;
}
```

Обратите внимание, что на этот раз мы проверяем действие, `ActivityType` имеющее значение `CSSearchableItem.ActionType`.

## <a name="updating-an-item"></a>Обновление элемента

Возможны случаи, когда элемент индекса, созданный с помощью ключевого Spotlight, необходимо изменить, например, если требуется изменить заголовок или эскиз изображения. Чтобы внести это изменение, мы используем тот же метод, который использовался для первоначального создания индекса.
Мы создаем новый `CSSearchableItem` с тем же идентификатором, который использовался для создания элемента, и присоединяем новый `CSSearchableItemAttributeSet` , содержащий измененные атрибуты:

[![](corespotlight-images/corespotlight02.png "Обновление обзора элемента")](corespotlight-images/corespotlight02.png#lightbox)

Когда этот элемент записывается в индекс с возможностью поиска, существующий элемент обновляется новыми данными.

## <a name="deleting-an-item"></a>Удаление элемента

Основное Spotlight предоставляет несколько способов удаления элемента индекса, если он больше не требуется.

Сначала можно удалить элемент по его идентификатору, например:

```csharp
// Delete Items by ID
CSSearchableIndex.DefaultSearchableIndex.Delete(new string[]{"1","16"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

Затем можно удалить группу элементов индекса по доменному имени. Например:

```csharp
// Delete by Domain Name
CSSearchableIndex.DefaultSearchableIndex.DeleteWithDomain(new string[]{"domain-name"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

Наконец, можно удалить все элементы индекса с помощью следующего кода:

```csharp
// Delete all index items
CSSearchableIndex.DefaultSearchableIndex.DeleteAll((error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```
## <a name="additional-core-spotlight-features"></a>Дополнительные основные функции Spotlight

Основное Spotlight имеет следующие функции, которые помогают обеспечить точность и актуальность индекса:

- **Поддержка пакетного обновления** . Если приложению требуется создать или изменить большую группу индексов одновременно, весь пакет можно отправить `Index` в метод `CSSearchableIndex` класса в одном вызове.
- **Реагирование на изменения индекса** — с `CSSearchableIndexDelegate` помощью приложения можно реагировать на изменения и уведомления в индексе с возможностью поиска.
- **Применение защиты данных** . используя классы защиты данных, можно реализовать безопасность элементов, добавляемых в индекс с возможностью поиска, с помощью ключевого прожектора Core.



## <a name="related-links"></a>Связанные ссылки

- [Примеры для iOS 9](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [iOS 9 для разработчиков](https://developer.apple.com/ios/pre-release/)
- [iOS 9,0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Инструкции по программированию поиска приложений](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
