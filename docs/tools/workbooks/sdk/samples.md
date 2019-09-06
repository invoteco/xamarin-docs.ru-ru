---
title: Примеры интеграции
description: В этом документе приведены ссылки на примеры, демонстрирующие интеграцию Xamarin Workbooks. Связанные образцы работают с визуализацией представления и SkiaSharp.
ms.prod: xamarin
ms.assetid: 327DAD2E-1F76-4EB5-BCD0-9E7384D99E48
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: fbe471aa7f08d85a870d68505cf2c983b7e442e9
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292816"
---
# <a name="sample-integrations"></a>Примеры интеграции

Рабочий пример интеграции см. в примере [приемника кухни][KitchenSink] . Просто выполните `KitchenSink.sln` сборку в Visual Studio для Mac или Visual Studio, а `KitchenSink.workbook`затем откройте.

[![Снимок экрана интеграции с приемником кухни](samples-images/kitchensinkintegrationscreenshot.png)](samples-images/kitchensinkintegrationscreenshot.png#lightbox)

В образце приемника кухни показаны оба набора концепций:

* На фрагментах представления показано, как `RepresentationManager` использовать для улучшения отрисовки с помощью встроенных представлений.
* Объект и связанный с ним модуль подготовки JavaScript демонстрируют использование `ISerializableObject` без использования поставщика представления. `Person`

См. также [SkiaSharp][skiasharp] для реального примера интеграции, в котором используются существующие [представления](~/tools/workbooks/sdk/representations.md) , предоставляемые Xamarin Workbooks для отрисовки своих типов.

[KitchenSink]: https://github.com/xamarin/Workbooks/tree/master/SDK/Samples/KitchenSink
[skiasharp]: https://github.com/mono/SkiaSharp/tree/master/source/SkiaSharp.Workbooks
