---
title: Примеры интеграции
description: В этом документе приведены ссылки на примеры, демонстрирующие интеграцию Xamarin Workbooks. Связанные образцы работают с визуализацией представления и SkiaSharp.
ms.prod: xamarin
ms.assetid: 327DAD2E-1F76-4EB5-BCD0-9E7384D99E48
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: fa66ee2b2b469900381e2d31dc5dec49eb42c4f6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73018120"
---
# <a name="sample-integrations"></a>Примеры интеграции

Рабочий пример интеграции см. в примере [приемника кухни][KitchenSink] . Просто создайте `KitchenSink.sln` в Visual Studio для Mac или Visual Studio, а затем откройте `KitchenSink.workbook`.

[Снимок экрана интеграции приемника![кухни](samples-images/kitchensinkintegrationscreenshot.png)](samples-images/kitchensinkintegrationscreenshot.png#lightbox)

В образце приемника кухни показаны оба набора концепций:

* На фрагментах представления показано, как использовать `RepresentationManager` для улучшения отрисовки с помощью встроенных представлений.
* Объект `Person` и связанный с ним модуль подготовки отчетов JavaScript демонстрирует использование `ISerializableObject` без использования поставщика представления.

См. также [SkiaSharp][skiasharp] для реального примера интеграции, в котором используются существующие [представления](~/tools/workbooks/sdk/representations.md) , предоставляемые Xamarin Workbooks для отрисовки своих типов.

[KitchenSink]: https://github.com/xamarin/Workbooks/tree/master/SDK/Samples/KitchenSink
[skiasharp]: https://github.com/mono/SkiaSharp/tree/master/source/SkiaSharp.Workbooks
