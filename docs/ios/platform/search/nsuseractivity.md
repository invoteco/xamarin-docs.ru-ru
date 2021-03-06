---
title: Поиск с помощью Нсусерактивити в Xamarin. iOS
description: В этом документе описывается, как индексировать Нсусерактивити, делая его поиском в Spotlight и Safari. В нем обсуждается реагирование на выбор Нсусерактивити в результатах поиска.
ms.prod: xamarin
ms.assetid: 0B28B284-C7C9-4C0D-A782-D471FBBC4CAE
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: c6ceb6e10abc4dbd26bffecbb6fefa5835f3d630
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031543"
---
# <a name="search-with-nsuseractivity-in-xamarinios"></a>Поиск с помощью Нсусерактивити в Xamarin. iOS

`NSUserActivity` была введена в iOS 8 и используется для передачи данных.
Она позволяет создавать действия в отдельных частях приложения, которые затем можно передать в другой экземпляр приложения, работающего на другом устройстве iOS. Принимающее устройство может затем продолжить действие, запущенное на предыдущем устройстве. Дополнительные сведения об использовании передачи см. [в статье Введение в](~/ios/platform/handoff.md) документацию по передаче данных.

Новые возможности iOS 9 `NSUserActivity` могут индексироваться (общедоступные и частные) и искаться в центре поиска Spotlight и Safari. Пометив `NSUserActivity` как поддерживающую Поиск и добавив индексируемые метаданные, действие может быть указано в результатах поиска на устройстве iOS.

[![](nsuseractivity-images/apphistory01.png "The App History overview")](nsuseractivity-images/apphistory01.png#lightbox)

Если пользователь выбирает результат поиска, принадлежащий действию из приложения, приложение будет запущено и действие, описываемое `NSUserActivity`, будет перезапущено и представлено пользователю.

Для поддержки поиска приложений используются следующие свойства `NSUserActivity`.

- `EligibleForHandoff` — если `true`, это действие можно использовать в операции передачи.
- `EligibleForSearch` — если `true`, это действие будет добавлено в индекс на устройстве и появится в результатах поиска.
- `EligibleForPublicIndexing` — если `true`, это действие будет добавлено в облачный индекс Apple и представлено пользователям (с помощью поиска), которые еще не установили приложение на устройстве iOS. Дополнительные сведения см. в разделе [индексирование общедоступного поиска](#public-search-indexing) .
- `Title` — содержит название действия и отображается в результатах поиска. Пользователи также могут искать текст самого заголовка.
- `Keywords` — это массив строк, используемый для описания действия, которое будет индексироваться и предоставляться для поиска конечным пользователем.
- `ContentAttributeSet` — это `CSSearchableItemAttributeSet`, который используется для подробного описания действия и предоставления расширенного содержимого в результатах поиска.
- `ExpirationDate` — если требуется, чтобы действие отображалось только до определенной даты, можно указать здесь дату.
- `WebpageURL` — если действие можно просмотреть в Интернете или ваше приложение поддерживает глубокие ссылки Safari, можно установить ссылку для посещения этой статьи.

## <a name="nsuseractivity-quickstart"></a>Краткое руководство по Нсусерактивити

Выполните следующие инструкции, чтобы реализовать в приложении `NSUserActivity`, поддерживающие поиск.

- [Создание идентификаторов типов действий](#creatingtypeid)
- [Создание действия](#createactivity)
- [Реагирование на действие](#respondactivity)
- [Индексирование общего поиска](#indexing)

Существует ряд [дополнительных преимуществ](#benefits) использования `NSUserActivity` для обеспечения возможности поиска содержимого.

<a name="creatingtypeid" />

## <a name="creating-activity-type-identifiers"></a>Создание идентификаторов типов действий

Прежде чем можно будет создать действие поиска, необходимо создать _идентификатор типа действия_ для его идентификации. Идентификатор типа действия — это короткая строка, добавляемая в `NSUserActivityTypes` массив файла **info. plist** приложения, используемый для уникальной идентификации данного типа действия пользователя. В массиве будет одна запись для каждого действия, которое поддерживает приложение и предоставляет поиск приложения. 

Apple предлагает использовать нотацию в стиле DNS для идентификатора типа действия, чтобы избежать конфликтов. Например: `com.company-name.appname.activity` для конкретных действий на основе приложения или `com.company-name.activity` для действий, которые могут выполняться в нескольких приложениях.

Идентификатор типа действия используется при создании экземпляра `NSUserActivity` для идентификации типа действия. Когда действие будет продолжено в результате нажатия пользователем результата поиска, тип действия (вместе с ИДЕНТИФИКАТОРом группы приложения) определяет, какое приложение следует запустить для продолжения действия.

Чтобы создать необходимые идентификаторы типов действий для поддержки такого поведения, измените файл **info. plist** и переключитесь в представление **исходного кода** . Добавьте `NSUserActivityTypes` ключ и создайте идентификаторы в следующем формате:

[![](nsuseractivity-images/type01.png "The NSUserActivityTypes key and required identifiers in the plist editor")](nsuseractivity-images/type01.png#lightbox)

В приведенном выше примере мы создали один новый идентификатор типа действия для действия поиска (`com.xamarin.platform`). При создании собственных приложений замените содержимое массива `NSUserActivityTypes` идентификаторами типов действий, характерными для действий, которые поддерживает приложение.

<a name="createactivity" />

## <a name="creating-an-activity"></a>Создание действия

Ниже приведен пример создания действия для поиска, размещенного в индексе устройства.

```csharp
// Create App Search Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Add App Search ability
activity.EligibleForSearch = true;
activity.BecomeCurrent();
```

Мы можем добавить дополнительные сведения, задав свойство `ContentAttributeSet` для `NSUserActivity` следующим образом:

[![](nsuseractivity-images/apphistory02.png "Addition Search Details overview")](nsuseractivity-images/apphistory02.png#lightbox)

С помощью `ContentAttributeSet` можно создавать сложные результаты поиска, которые приводят к взаимодействию конечного пользователя с ними.

<a name="respondactivity" />

## <a name="responding-to-an-activity"></a>Реагирование на действие

Чтобы ответить на пользователя, коснувшись результата поиска (`NSUserActivity`) для нашего приложения, измените файл **AppDelegate.CS** и переопределите метод `ContinueUserActivity`. Пример:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    }

    return true;
}
```

Обратите внимание, что это то же переопределение метода, которое используется для реагирования на запросы на перегрузку. Теперь, если пользователь щелкнет ссылку из нашего приложения в результатах поиска Spotlight, наше приложение будет перенесено на передний план (или запущено, если оно еще не запущено), а содержимое, Навигация или компонент, представленные этой ссылкой, будут отображаться следующим образом:

[![](nsuseractivity-images/apphistory03.png "Restore Previous State from Search")](nsuseractivity-images/apphistory03.png#lightbox)

<a name="indexing" />

## <a name="public-search-indexing"></a>Индексирование общего поиска

Как было показано выше, iOS 9 позволяет легко предоставить поисковый доступ к содержимому приложения и функциям, которые пользователь уже обнаружил и использовал на заданном устройстве iOS. Благодаря общедоступному индексированию iOS 9 позволяет пользователям, которые еще не обнаружили содержимое или компоненты (или кто еще не установил приложение), получить эти результаты в поиске.

Общедоступное индексирование работает следующим образом:

1. При создании действия для приложения его можно пометить как общедоступное.
2. Общедоступные действия отправляются в Apple и индексируются в облаке.
3. По мере того, как больше пользователей взаимодействует с приложением на устройстве и использует определенную функцию или содержимое, представленное этим действием, оно растет на звании.
4. Популярные общедоступные результаты будут доступны другим пользователям, даже если на них не установлено приложение.
5. Эти открытые результаты будут отображаться в поиске Spotlight и Safari (если действие содержит URL-адрес).

Мы можем взять закрытое действие поиска, созданное ранее, и развернуть его как общедоступный:

```csharp
// Create App Search Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Add App Search ability
activity.EligibleForSearch = true;
activity.EligibleForPublicIndexing = true;
activity.BecomeCurrent();
```

Так как действие было установлено для общедоступного индексирования путем установки `EligibleForPublicIndexing = true`, оно не означает, что оно будет автоматически добавлено в общедоступный облачный индекс Apple. Первыми должны быть выполнены следующие условия.

1. Он должен отображаться в результатах поиска и выбираться многими пользователями. Результаты остаются закрытыми до тех пор, пока не будет достигнут порог активности действия.
2. Подготовка приложений предотвращает индексирование и предоставление общедоступных пользовательских данных.

<a name="benefits" />

## <a name="additional-benefits"></a>Дополнительные преимущества

Принимая Поиск приложения с помощью `NSUserActivity` в приложении, вы также получаете следующие возможности:

- Передается, так как поиск приложения предоставляет доступ к содержимому, переходу и (или) функциям, используя тот же механизм, что и при приеме (`NSUserActivity`), вы можете легко разрешить пользователям приложения запускать действие на одном устройстве и продолжать его на другом.
- **Предложения Siri** . Вместе с стандартными предложениями, которые обычно делают Siri предложения, активные приложения могут быть предложены автоматически.
- **Siri интеллектуальные напоминания** — пользователи смогут попросить Siri напомнить о действиях из вашего приложения.

## <a name="related-links"></a>Связанные ссылки

- [Примеры для iOS 9](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [iOS 9 для разработчиков](https://developer.apple.com/ios/pre-release/)
- [iOS 9,0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Инструкции по программированию поиска приложений](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
- [Пример & записи блога](https://blog.xamarin.com/improve-discoverability-with-search-in-ios-9/)
