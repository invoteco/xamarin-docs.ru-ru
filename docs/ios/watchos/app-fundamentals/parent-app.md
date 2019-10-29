---
title: Работа с родительским приложением watchOS в Xamarin
description: В этом документе описывается работа с родительским приложением watchOS в Xamarin. В нем обсуждаются расширения приложений WatchKit, приложения iOS, общее хранилище и многое другое.
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 08637fe13b28565e7c6ab1c89b291c6db3b81025
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73001475"
---
# <a name="working-with-the-watchos-parent-application-in-xamarin"></a>Работа с родительским приложением watchOS в Xamarin

> [!IMPORTANT]
> Доступ к родительскому приложению с использованием приведенных ниже примеров работает только в приложениях watchOS 1 Watch.

Существует несколько способов обмена данными между приложением Watch и приложением iOS, с которым оно объединяется.

- Расширения Watch могут [вызывать метод](#code) для родительского приложения, которое выполняется в фоновом режиме на iPhone.

- Расширения Watch могут [совместно использовать расположение хранилища](#storage) с родительским приложением iPhone.

- Использование функции Pass для передачи данных с одного взгляда или уведомления в приложение Watch, отправляя пользователя на конкретный контроллер интерфейса в приложении.

Кроме того, родительское приложение иногда называют приложением-контейнером.

<a name="code" />

## <a name="run-code"></a>Выполнение кода

Взаимодействие между расширением Watch и родительским приложением iPhone показано в [примере гпсватч](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-gpswatch).
Расширение "Контрольное значение" может запрашивать от своего имени родительское приложение iOS, использующее метод `OpenParentApplication`.

Это особенно полезно для длительно выполняемых задач (включая сетевые запросы) — только родительское приложение iOS может использовать преимущества фоновой обработки для выполнения этих задач и сохранения извлеченных данных в расположении, доступном для расширения Watch.

### <a name="watch-kit-app-extension"></a>Просмотр расширения приложения для комплекта

Вызовите `WKInterfaceController.OpenParentApplication` в расширении приложения Watch. Он возвращает `bool`, который указывает, успешно ли отправлен запрос метода. Проверьте параметр `error` в `Action` отклика, чтобы определить, произошли ли какие либо ошибки во время выполнения метода в приложении iPhone.

```csharp
WKInterfaceController.OpenParentApplication (new NSDictionary (), (replyInfo, error) => {
    if(error != null) {
        Console.WriteLine (error);
        return;
    }
    Console.WriteLine ("parent app responded");
    // do something with replyInfo[] dictionary
});
```

### <a name="ios-app"></a>Приложение iOS

Все вызовы из расширения приложения для наблюдения направляются через метод `HandleWatchKitExtensionRequest` приложения iPhone.
При выполнении различных запросов в приложении Watch этот метод должен запросить словарь `userInfo`, чтобы определить, как обрабатывать запрос.

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
    // ... other AppDelegate methods
    public override void HandleWatchKitExtensionRequest
        (UIApplication application, NSDictionary userInfo, Action<NSDictionary> reply)
    {
        var status = 2;
        // do something in the background, and respond
        reply (new NSDictionary (
            "count", NSNumber.FromInt32 ((int)status),
            "value2", new NSString("some-info")
            ));
    }
}
```

<a name="storage" />

## <a name="shared-storage"></a>Общее хранилище

Если вы настраиваете [группу приложений](~/ios/watchos/app-fundamentals/app-groups.md) , расширения iOS 8 (включая расширения контрольных значений) могут обмениваться данными с родительским приложением.

<a name="nsuserdefaults" />

### <a name="nsuserdefaults"></a>нсусердефаултс

Следующий код можно записать как в расширении приложения Watch, так и в родительском приложении iPhone, чтобы они могли ссылаться на общий набор `NSUserDefaults`:

```csharp
NSUserDefaults shared = new NSUserDefaults(
        "group.com.your-company.watchstuff",
        NSUserDefaultsType.SuiteName);

// set values
shared.SetInt (2, "count");
shared.Synchronize ();

// get values
shared.Synchronize ();
var count = shared.IntForKey ("count");
```

<a name="files" />

### <a name="files"></a>Файлы

Приложение iOS и расширение Watch могут также использовать общие пути к файлам.

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =
            FileManager.GetContainerUrl ("group.com.your-company.watchstuff");
var appGroupContainerPath = appGroupContainer.Path;
Console.WriteLine ("agcpath: " + appGroupContainerPath);
// use the path to create and update files
```

Примечание. Если путь `null`, проверьте [конфигурацию группы приложений](~/ios/watchos/app-fundamentals/app-groups.md) , чтобы убедиться, что профили подготовки настроены правильно и были загружены или установлены на компьютере разработчика.

Дополнительные сведения см. в документации по [возможностям группы приложений](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) .

## <a name="wormholesharp"></a>вормхолешарп

Популярный механизм с открытым исходным кодом для watchOS 1 (на основе [ммвормхоле](https://github.com/mutualmobile/MMWormhole)) для передачи данных или команд между родительским приложением и приложением Watch.

Вы можете настроить норку с помощью группы приложений, подобной этой, в приложении iOS и просмотреть расширение:

```csharp
// AppDelegate (iOS) or InterfaceController (watch extension)
Wormhole wormHole;
// ...
wormHole = new Wormhole ("group.com.your-company.watchstuff", "messageDir");
```

Скачайте C# версию [вормхолешарп](https://github.com/Clancey/WormHoleSharp).

## <a name="related-links"></a>Связанные ссылки

- [Гпсватч (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Вормхолешарп (пример)](https://github.com/Clancey/WormHoleSharp)
- [Справочник по Вкинтерфацеконтроллер Apple](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [Совместное использование данных Apple с содержащим приложением](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
