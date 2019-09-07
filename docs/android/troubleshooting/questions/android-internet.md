---
title: Почему моя сборка выпуска Android не подключается к Интернету?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A6FE770B-A19A-4BF8-95E9-2CF880D4AFC5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 162378c00f3e20574d04dc373fcc492a9407b88d
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761033"
---
# <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>Почему моя сборка выпуска Android не подключается к Интернету?

## <a name="cause"></a>Причина

Наиболее распространенной причиной этой проблемы является то, что **Интернет** -разрешение автоматически включается в отладочную сборку, но его необходимо задать вручную для сборки выпуска. Это связано с тем, что разрешение «Интернет» позволяет отладчику присоединяться к процессу, как описано в [статье](~/android/deploy-test/building-apps/build-process.md)«DebugSymbols».

## <a name="fix"></a>Fix

Чтобы устранить эту проблему, можно запросить разрешение Интернета в манифесте Android. Это можно сделать с помощью редактора манифеста или кодовприложений манифеста:

- Исправить в редакторе: В проекте Android перейдите в раздел **Свойства — > AndroidManifest. XML — > необходимые разрешения** и проверьте **Интернет** .

- Исправление в Кодовприложений: Откройте AndroidManifest в редакторе исходного кода и добавьте тег `<Manifest>` разрешения в Теги:

    ```xml
    <Manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    </Manifest>
    ```
