---
title: Привязка собственных платформ
description: В этом документе описывается использование параметра целевой платформы Шарпие для создания привязки к библиотеке, распространяемой в качестве платформы.
ms.prod: xamarin
ms.assetid: 91AE058A-3A1F-41A9-9DE4-4B96880A1869
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: cb6c39b2110161b3f839b8adc03701007f09cc4d
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2019
ms.locfileid: "69521880"
---
# <a name="binding-native-frameworks"></a>Привязка собственных платформ

Иногда собственная библиотека распространяется как [платформа](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html). Цель Шарпие предоставляет удобную функцию для привязки правильно определенных платформ с помощью `-framework` параметра.

Например, привязка [платформы Adobe Creative SDK](https://creativesdk.adobe.com/downloads.html) для iOS проста:

```
$ sharpie bind \
    -framework AdobeCreativeSDKFoundation.framework \
    -sdk iphoneos8.1
```

В некоторых случаях платформа будет указывать **info. plist** , указывающий, в каком пакете SDK должна быть скомпилирована платформа. Если эта информация существует и ни один `-sdk` параметр не передан, Целевая Шарпие будет вычислять его из **info. plist** ( `DTSDKName` ключа или сочетания `DTPlatformName` ключей и `DTPlatformVersion` ).

`-framework` Параметр не допускает передачи явных файлов заголовков. Файл заголовка "тег" выбирается по соглашению на основе имени платформы. Если заголовок не удается найти, Целевая Шарпие не будет пытаться привязать платформу, и необходимо вручную выполнить привязку, предоставив правильные файлы заголовков (-ов) для синтаксического анализа, а также аргументы платформы для Clang (например, `-F`параметр пути поиска платформы).

Внутри, указание `-framework` — это просто ярлык. Следующие аргументы привязки идентичны `-framework` приведенным выше.
Особая важность — `-F .` это путь поиска платформы, предоставленный для Clang (Обратите внимание на пространство и период, которые требуются как часть команды).

```
$ sharpie bind \
    -sdk iphoneos8.1 \
    AdobeCreativeSDKFoundation.framework/Headers/AdobeCreativeSDKFoundation.h \
    -scope AdobeCreativeSDKFoundation.framework/Headers \
    -c -F .
```
