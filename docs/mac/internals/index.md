---
title: Внутри Xamarin. Mac
description: В этом документе содержатся ссылки на различные руководства, описывающие внутреннюю работу Xamarin. Mac. Связанные документы обсуждаются перед компиляцией времени, архитектурой Xamarin. Mac и регистратором Xamarin. Mac.
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 11/10/2017
ms.openlocfilehash: c13f8db60d296b8fa684e1d6861ba5caf38a7680
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029939"
---
# <a name="under-the-hood-in-xamarinmac"></a>Внутри Xamarin. Mac

## <a name="ahead-of-time-compilation-aotaotmd"></a>[Компиляция с опережением времени (AOT)](aot.md)

Компиляция до времени (AOT) — это мощный метод оптимизации для повышения производительности при запуске. Однако это также влияет на время сборки, размер приложения и выполнение программы с помощью более глубокого способа, поэтому важно понимать, как это работает.

## <a name="mac-architecturearchitecturemd"></a>[Архитектура Mac](architecture.md)

Отношение Xamarin. Mac к цели-C, включая такие концепции, как компиляция, селекторы, регистраторы, запуск приложений и генератор.

## <a name="xamarinmac-registrarregistrarmd"></a>[Регистратор Xamarin. Mac](registrar.md)

Xamarin. Mac связывает зазор между средой выполнения управляемого мира и Cocoa, позволяя управляемым классам вызывать неуправляемые классы цели-C и вызывать их обратно при возникновении событий. Работа, необходимая для дается этой «волшебки», обрабатывается регистратором, но понимание того, что происходит во внутреннем случае, иногда может оказаться полезной.
