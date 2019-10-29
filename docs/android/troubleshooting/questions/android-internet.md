---
title: Почему моя сборка выпуска Android не подключается к Интернету?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A6FE770B-A19A-4BF8-95E9-2CF880D4AFC5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 5996cfa3c0a18fc186ea862a2b3d7910594e1281
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027009"
---
# <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>Почему моя сборка выпуска Android не подключается к Интернету?

## <a name="cause"></a>Причина

Наиболее распространенной причиной этой проблемы является то, что **Интернет** -разрешение автоматически включается в отладочную сборку, но его необходимо задать вручную для сборки выпуска. Это связано с тем, что разрешение «Интернет» позволяет отладчику присоединяться к процессу, как описано в [статье](~/android/deploy-test/building-apps/build-process.md)«DebugSymbols».

## <a name="fix"></a>Исправление

Чтобы устранить эту проблему, можно запросить разрешение Интернета в манифесте Android. Это можно сделать с помощью редактора манифеста или кодовприложений манифеста:

- Исправить в редакторе. в проекте Android перейдите в раздел **Properties-> AndroidManifest. XML-> необходимые разрешения** и проверьте **Интернет** .

- Исправление в Кодовприложений: Откройте AndroidManifest в редакторе исходного кода и добавьте тег разрешения в теги `<Manifest>`:

    ```xml
    <Manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    </Manifest>
    ```
