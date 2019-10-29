---
title: Привязка библиотек Mac для Xamarin. Mac
description: В этом документе содержатся ссылки на руководства, в которых описывается работа с привязками цели-C в приложении Xamarin. Mac, включая целевые Шарпие и примеры кода.
ms.prod: xamarin
ms.assetid: 521707CD-79D3-488A-84CB-A37EBF93AC94
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 01/13/2017
ms.openlocfilehash: c478437a9c84475e8c31484523db16336f8808e6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029933"
---
# <a name="binding-mac-libraries-for-xamarinmac"></a>Привязка библиотек Mac для Xamarin. Mac

Перейдите по следующим ссылкам, чтобы узнать о привязке библиотек цели-C на Xamarin. Mac:

- [**Общие сведения**](~/cross-platform/macios/binding/overview.md) -
  описывает, как работает привязка.
- [**Цели привязки — библиотеки C**](~/cross-platform/macios/binding/objective-c-libraries.md) -
  инструкции по привязке библиотек цели-c для использования в проектах Xamarin.
- [**Справочное руководство по определению типов**](~/cross-platform/macios/binding/binding-types-reference.md) -
  описывает все атрибуты, доступные для привязки авторов, чтобы управлять процессом создания привязки.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Цель Шарпие — это средство командной строки, помогающее выполнить начальную загрузку первого прохода привязки.
Он работает путем анализа файлов заголовков собственной библиотеки, чтобы сопоставлять открытый API с [определением привязки](~/cross-platform/macios/binding/binding-types-reference.md) (процесс, который в противном случае выполняется вручную). Цель Шарпие не создает привязку самостоятельно, но может помочь вам приступить к работе!

## <a name="examples"></a>Примеры

Сведения о создании привязки Mac с помощью привязок проектов см. в [примере Ксмбиндинжексампле Mac](https://github.com/xamarin/mac-samples/tree/master/XMBindingExample) .

## <a name="related-links"></a>Связанные ссылки

- [Привязка Objective-C](~/cross-platform/macios/binding/index.md)
- [Привязка библиотек iOS](~/ios/platform/binding-objective-c/index.md)
