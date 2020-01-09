---
title: Работа с watchOS родительское приложение в Xamarin
description: В этом документе описывается, как работать с watchOS родительского приложения в Xamarin. В нем обсуждаются расширения приложений watchOS, приложения iOS, общее хранилище и многое другое.
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 3e11b163d16be9711bf09102e3ab8604d98299d7
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75487767"
---
# <a name="working-with-the-watchos-parent-application-in-xamarin"></a>Работа с watchOS родительское приложение в Xamarin

Для обмена данными между приложении для Apple watch и приложение iOS, которое объединяется с разными способами:

- Смотреть приложения могут [запускать код](#run-code) в родительском приложении на iPhone.

- Контрольные значения расширений может [совместно используют хранилище, доступном](#shared-storage) с приложением iPhone родительского.

- Используйте пересылку, чтобы передать данные из уведомления в приложение наблюдения, отправив пользователю конкретный контроллер интерфейса в приложении.

Родительское приложение, также иногда называется приложения-контейнера.

## <a name="run-code"></a>Выполните код

В этих двух примерах показано, как использовать `WCSession` для выполнения кода и отправки сообщений между приложением Watch и парным iPhone.

- [Просмотр подключения](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchconnectivity/)
- [симплеватчконнективити](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-simplewatchconnectivity/) 

## <a name="shared-storage"></a>Общее хранилище

Если вы настраиваете [групп приложений](~/ios/watchos/app-fundamentals/app-groups.md) то расширения iOS 8 (включая расширения watch) могут обмениваться данными в родительское приложение.

### <a name="nsuserdefaults"></a>Параметры NSUserDefaults

Следующий код могут быть написаны на расширение приложения watch и iPhone в родительское приложение таким образом, они могут ссылаться на общий набор `NSUserDefaults`:

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

Расширение приложения "и" Контрольные значения операций ввода-вывода также могут совместно использовать файлы, используя общий путь к файлу.

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =
            FileManager.GetContainerUrl ("group.com.your-company.watchstuff");
var appGroupContainerPath = appGroupContainer.Path;
Console.WriteLine ("agcpath: " + appGroupContainerPath);
// use the path to create and update files
```

Примечание: Если путь является `null` проверьте [конфигурации группы приложений](~/ios/watchos/app-fundamentals/app-groups.md) для обеспечения профили подготовки были настроены правильно и были загружены или не установлены на компьютере разработчика.

Дополнительные сведения см. в разделе [возможностей групп приложений](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) документации.

## <a name="related-links"></a>Связанные ссылки

- [Справочник по WKInterfaceController Apple](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [Apple обмена данными с содержащего приложение](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
