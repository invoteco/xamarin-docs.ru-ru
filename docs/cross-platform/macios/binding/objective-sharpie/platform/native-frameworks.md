---
title: Привязка собственных платформ
description: В этом документе описывается использование цели Sharpie современному параметры платформы для создания привязки в библиотеку, распределенному как платформу.
ms.prod: xamarin
ms.assetid: 91AE058A-3A1F-41A9-9DE4-4B96880A1869
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 0da40918c8ae36c4ab3d4c41128429b49706d653
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/01/2019
ms.locfileid: "64977645"
---
# <a name="binding-native-frameworks"></a>Привязка собственных платформ

Иногда собственной библиотеки распространяется в виде [framework](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html). Цели Sharpie предоставляет удобное средство для привязки правильно определенные платформы через `-framework` параметр.

Например, привязка [Adobe Creative SDK Framework](https://creativesdk.adobe.com/downloads.html) для операций ввода-вывода выполняется просто:

<pre>$ <b>sharpie bind \
    -framework AdobeCreativeSDKFoundation.framework \
    -sdk iphoneos8.1</b></pre>

В некоторых случаях будет указать платформу **Info.plist** которого указывает, к какой пакет SDK платформы должны быть скомпилированы. Если этой информации и без явного указания `-sdk` передается параметр, цель Sharpie определит его из платформы **Info.plist** (либо `DTSDKName` ключ или сочетание `DTPlatformName` и `DTPlatformVersion`ключи).

`-framework` Параметр не поддерживает явные заголовка файлов для передачи. Файл заголовка обобщающий выбирается по соглашению, на основе имени платформы. Если не удается найти заголовок обобщающий, Sharpie цели не будет пытаться привязать платформы и привязки необходимо выполнить вручную, указав правильный обобщающий файлы заголовка для синтаксического анализа, и любые другие аргументы framework для clang (например `-F`параметр пути поиска framework).

За кулисами указав `-framework` является просто ярлыком. Следующие аргументы привязки идентичны `-framework` сокращением выше.
Особенное значение имеет `-F .` путь поиска платформы для clang (Обратите внимание, места и период, которые являются неотъемлемой частью команды).

<pre>$ <b>sharpie bind \
    -sdk iphoneos8.1 \
    AdobeCreativeSDKFoundation.framework/Headers/AdobeCreativeSDKFoundation.h \
    -scope AdobeCreativeSDKFoundation.framework/Headers \
    -c -F .</b></pre>
