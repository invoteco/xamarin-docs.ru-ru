---
title: Платформы цели-C
description: В этом документе описываются различные платформы, которые могут быть ориентированы на внедрение .NET при работе с кодом цели-C. В нем обсуждаются macOS, iOS, tvOS и watchOS.
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
author: davidortinau
ms.author: daortin
ms.date: 11/14/2017
ms.openlocfilehash: dbc2c40e06f14ec636b4aa4cc11fc4f2d230ee6d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73006425"
---
# <a name="objective-c-platforms"></a>Платформы цели-C

Внедрение .NET может ориентироваться на различные платформы при формировании кода цели на C:

* macOS
* iOS
* tvOS
* watchOS [еще не реализовано]

Платформа выбирается путем передачи аргумента командной строки `--platform=<platform>` в внедрение .NET.

При создании платформ для iOS, tvOS и watchOS внедрение .NET всегда создает платформу, которая внедряет Xamarin. iOS, так как Xamarin. iOS содержит большой код поддержки среды выполнения, необходимый на этих платформах.

Однако при создании для платформы macOS можно выбрать, должна ли созданная платформа внедрять Xamarin. Mac. Вы можете не внедрять Xamarin. Mac, если привязанная сборка не ссылается на Xamarin. Mac. dll (прямо или косвенно) и выбрана путем передачи `--platform=macOS` средству внедрения .NET.

Если связанная сборка содержит ссылку на Xamarin. Mac. dll, необходимо внедрить Xamarin. Mac, а кроме того, ембеддинатор должен быть уверен, какую целевую платформу использовать.

Существует три возможные целевые платформы Xamarin. Mac: `modern` (ранее именуемые `mobile`), `full` и `system` (разница между ними описана в документации по [целевой платформе][1] Xamarin. Mac), и каждый из них выбирается путем передачи `--platform=macOS-modern`, `--platform=macOS-full` или `--platform=macOS-system` средства внедрения .NET.

[1]: ~/mac/platform/target-framework.md
