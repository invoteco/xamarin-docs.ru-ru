---
title: Платформы цели-C
description: В этом документе описываются различные платформы, которые могут быть ориентированы на внедрение .NET при работе с кодом цели-C. В нем обсуждаются macOS, iOS, tvOS и watchOS.
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
author: conceptdev
ms.author: crdun
ms.date: 11/14/2017
ms.openlocfilehash: f97b595f129cb1ad1ea56e3ae43b0f0a477fef5a
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70282729"
---
# <a name="objective-c-platforms"></a>Платформы цели-C

Внедрение .NET может ориентироваться на различные платформы при формировании кода цели на C:

* macOS
* iOS
* tvOS
* watchOS [еще не реализовано]

Платформа выбирается путем передачи `--platform=<platform>` аргумента командной строки в внедрение .NET.

При создании платформ для iOS, tvOS и watchOS внедрение .NET всегда создает платформу, которая внедряет Xamarin. iOS, так как Xamarin. iOS содержит большой код поддержки среды выполнения, необходимый на этих платформах.

Однако при создании для платформы macOS можно выбрать, должна ли созданная платформа внедрять Xamarin. Mac. Вы можете не внедрять Xamarin. Mac, если привязанная сборка не ссылается на Xamarin. Mac. dll (прямо или косвенно), и этот вариант выбран путем передачи `--platform=macOS` в средство внедрения .NET.

Если связанная сборка содержит ссылку на Xamarin. Mac. dll, необходимо внедрить Xamarin. Mac, а кроме того, ембеддинатор должен быть уверен, какую целевую платформу использовать.

Существует `modern` три возможные целевые платформы Xamarin. Mac: (ранее назывались `mobile`) `full` и `system` (разница между ними описана в документации по [целевой платформе][1] Xamarin. Mac), и каждая из них выбирается путем передачи `--platform=macOS-modern` `--platform=macOS-full` или `--platform=macOS-system` для средства внедрения .NET.

[1]: ~/mac/platform/target-framework.md
