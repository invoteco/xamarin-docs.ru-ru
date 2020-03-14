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
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306251"
---
# <a name="working-with-the-watchos-parent-application-in-xamarin"></a>Работа с watchOS родительское приложение в Xamarin

Для обмена данными между приложении для Apple watch и приложение iOS, которое объединяется с разными способами:

- Смотреть приложения могут [запускать код](#run-code) в родительском приложении на iPhone.

- Расширения Watch могут [совместно использовать расположение хранилища](#shared-storage) с родительским приложением iPhone.

- Используйте пересылку, чтобы передать данные из уведомления в приложение наблюдения, отправив пользователю конкретный контроллер интерфейса в приложении.

Родительское приложение, также иногда называется приложения-контейнера.

## <a name="run-code"></a>Выполните код

В этих двух примерах показано, как использовать `WCSession` для выполнения кода и отправки сообщений между приложением Watch и парным iPhone.

- [Просмотр подключения](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchconnectivity/)
- [симплеватчконнективити](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-simplewatchconnectivity/) 

## <a name="shared-storage"></a>Общее хранилище

Если вы настраиваете [группу приложений](~/ios/watchos/app-fundamentals/app-groups.md) , расширения iOS 8 (включая расширения контрольных значений) могут обмениваться данными с родительским приложением.

### <a name="nsuserdefaults"></a>Параметры NSUserDefaults

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

Расширение приложения "и" Контрольные значения операций ввода-вывода также могут совместно использовать файлы, используя общий путь к файлу.

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

## <a name="related-links"></a>Связанные ссылки

- [Справочник по Вкинтерфацеконтроллер Apple](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [Совместное использование данных Apple с содержащим приложением](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
