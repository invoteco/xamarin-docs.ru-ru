---
title: Реальный пример, с помощью проекта Xcode
description: В этом документе описывается, как для использования в качестве прямого ввода для Sharpie цели, что упрощает процесс создания проекта Xcode C# привязки для кода Objective-C.
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: ccfc2f1760d8971e2d824cf65344fa2a5e158c12
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/01/2019
ms.locfileid: "64978370"
---
# <a name="real-world-example-using-an-xcode-project"></a>Реальный пример, с помощью проекта Xcode

**В этом примере используется [POP библиотеки из Facebook](https://github.com/facebook/pop).**

Новое в версии 3.0, цель Sharpie поддерживает проекты Xcode как входные данные. Эти проекты указать правильный заголовок файлы и флаги компилятора, необходимые для компиляции собственной библиотеки и таким образом, необходимо привязать его слишком. Цели Sharpie выберет первый _целевой_ и задана конфигурация по умолчанию проекта, если не выдал вам иные инструкции.

Прежде чем Sharpie цели пытается выполнить синтаксический анализ файлов проекта и заголовок, его необходимо построить. Проекты часто имеют этапов построения, которые правильно структурировать файлы заголовков для внешнего использования и интеграции, поэтому она лучше всегда собирался весь проект, прежде чем привязать его.

<pre>$ <b>git clone https://github.com/facebook/pop.git</b>
Cloning into 'pop'...
   <em>(more git clone output)</em>

$ <b>cd pop</b>
$ <b>sharpie bind pop.xcodeproj -sdk iphoneos9.0</b></pre>

