---
title: Пример реального мира с использованием CocoaPods
description: В этом документе показано, как использовать целевое Шарпие для автоматического C# создания определений привязки из кокоапод.
ms.prod: xamarin
ms.assetid: 233B781D-5841-4250-9F63-0585231D2112
author: conceptdev
ms.author: crdun
ms.date: 03/28/2018
ms.openlocfilehash: 0f730b1c0a0deacdb84c198cfe4af47308a268cc
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290031"
---
# <a name="real-world-example-using-cocoapods"></a>Пример реального мира с использованием CocoaPods

> [!NOTE]
> В этом примере используется [Афнетворкинг кокоапод](https://cocoapods.org/pods/AFNetworking).

Новая версия в версии 3,0, Целевая Шарпие поддерживает привязку CocoaPods и даже включает команду`sharpie pod`(), чтобы сделать загрузку, настройку и сборку CocoaPods очень простой. Прежде чем использовать эту функцию, следует [ознакомиться с CocoaPods](https://cocoapods.org) в целом.

## <a name="creating-a-binding-for-a-cocoapod"></a>Создание привязки для Кокоапод

`sharpie pod` Команда имеет один глобальный параметр и две подкоманды:

```bash
$ sharpie pod -help
usage: sharpie pod [OPTIONS] COMMAND [COMMAND_OPTIONS]

Pod Options:
  -d, -dir DIR     Use DIR as the CocoaPods binding directory,
                   defaulting to the current directory

Available Commands:
  init         Initialize a new Xamarin C# CocoaPods binding project
  bind         Bind an existing Xamarin C# CocoaPods project
```

`init` Подкоманда также имеет некоторую полезную справку:

```bash
$ sharpie pod init -help
usage: sharpie pod init [INIT_OPTIONS] TARGET_SDK POD_SPEC_NAMES

Init Options:
  -f, -force       Initialize a new Podfile and run actions against
                   it even if one already exists
```

В `init`можно указать несколько имен кокоапод и подспецификаций.

```bash
$ sharpie pod init ios AFNetworking
** Setting up CocoaPods master repo ...
   (this may take a while the first time)
** Searching for requested CocoaPods ...
** Working directory:
**   - Writing Podfile ...
**   - Installing CocoaPods ...
**     (running `pod install --no-integrate --no-repo-update`)
Analyzing dependencies
Downloading dependencies
Installing AFNetworking (2.6.0)
Generating Pods project
Sending stats
** 🍻 Success! You can now use other `sharpie podn`  commands.
```

После настройки Кокоапод теперь можно создать привязку:

```bash
$ sharpie pod bind
```

Это приведет к построению проекта Кокоапод Xcode, который затем вычисляется и анализируется целевым Шарпие. Будет сформировано множество выходных данных консоли, но в конце должно появиться определение привязки:

```bash
(... lots of build output ...)

Parsing 19 header files...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Done.
```

## <a name="next-steps"></a>Следующие шаги

После создания файлов **ApiDefinitions.CS** и **StructsAndEnums.CS** ознакомьтесь со следующей документацией, чтобы создать сборку для использования в приложениях:

- [Общие сведения о цели привязки в C](~/cross-platform/macios/binding/overview.md)
- [Цель привязки-библиотеки C](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Пошаговое руководство: Привязка библиотеки цели iOS-C](~/ios/platform/binding-objective-c/walkthrough.md)
