---
title: Взгляд изнутри на Xamarin.Mac
description: Этот документ содержит ссылки на различные руководства, описывающие внутренней работы Xamarin.Mac. Связанные документы обсудить опережает время компиляции, архитектура Xamarin.Mac и Xamarin.Mac регистратор.
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 11/10/2017
ms.openlocfilehash: 872f26febf3abbe4d659773d2bf2d27348c64513
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61033247"
---
# <a name="under-the-hood-in-xamarinmac"></a>Взгляд изнутри на Xamarin.Mac

## <a name="ahead-of-time-compilation-aotaotmd"></a>[Преимущества компиляцию (AOT)](aot.md)

Ahead time (AOT) компиляции является методика мощные оптимизации быстрый запуск. Тем не менее оно также влияет на время сборки, размер приложения и выполнение программы в эффективных средств, поэтому стоит понять, как это работает.

## <a name="mac-architecturearchitecturemd"></a>[Архитектура Mac](architecture.md)

Xamarin.Mac связь с Objective-C, включая концепции, такие как компиляции, селекторы, регистраторов, запуск приложения и генератор.

## <a name="xamarinmac-registrarregistrarmd"></a>[Регистратор Xamarin.Mac](registrar.md)

Xamarin.Mac ликвидирует разрыв между управляемым миром и среды выполнения Cocoa, позволяя управляемых классов для вызова неуправляемых классов Objective-C и обратного вызова при возникновении событий. Работ, необходимый для выполнения «волшебных» обрабатывается регистратор, но понимание того, что происходит «за кулисами» иногда может быть полезно.
