---
title: Привязка собственных платформ
description: В этом документе описывается использование параметра целевой платформы Шарпие для создания привязки к библиотеке, распространяемой в качестве платформы.
ms.prod: xamarin
ms.assetid: 91AE058A-3A1F-41A9-9DE4-4B96880A1869
author: davidortinau
ms.author: daortin
ms.date: 01/15/2016
ms.openlocfilehash: 0733e2b406f5032a2e2717df66c96dcb28301f09
ms.sourcegitcommit: 24883be72e485e5311dd0eb91f9a22f78eeec11a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/17/2020
ms.locfileid: "77374132"
---
# <a name="binding-native-frameworks"></a>Привязка собственных платформ

Иногда собственная библиотека распространяется как [платформа](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html). Цель Шарпие предоставляет удобную функцию для привязки правильно определенных платформ с помощью параметра `-framework`.

Например, привязка [платформы Adobe Creative SDK](https://creativesdk.adobe.com/downloads.html) для iOS проста:

```
$ sharpie bind \
    -framework ./AdobeCreativeSDKFoundation.framework \
    -sdk iphoneos8.1
```

В некоторых случаях платформа будет указывать **info. plist** , указывающий, в каком пакете SDK должна быть скомпилирована платформа. Если эта информация существует и ни один параметр `-sdk` не передан, Целевая Шарпие будет вычислять его из **info. plist** (ключ `DTSDKName` или сочетание `DTPlatformName` и `DTPlatformVersion`ных ключей).

Параметр `-framework` не допускает передачи явных файлов заголовков. Файл заголовка "тег" выбирается по соглашению на основе имени платформы. Если не удается найти заголовок управляющего объекта, Целевая Шарпие не будет пытаться привязать платформу, и необходимо вручную выполнить привязку, предоставив правильные файлы заголовков (-ов) для синтаксического анализа, а также аргументы платформы для Clang (например, параметр пути поиска `-F` Framework).

В этом случае указание `-framework` является просто ярлыком. Следующие аргументы привязки идентичны `-framework` краткости.
Особая важность — это `-F .` путь поиска платформы, предоставленный Clang (Обратите внимание на пробел и период, которые требуются как часть команды).

```
$ sharpie bind \
    -sdk iphoneos8.1 \
    ./AdobeCreativeSDKFoundation.framework/Headers/AdobeCreativeSDKFoundation.h \
    -scope AdobeCreativeSDKFoundation.framework/Headers \
    -c -F .
```
