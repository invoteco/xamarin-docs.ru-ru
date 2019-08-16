---
title: Почему моя сборка выпуска Android не подключается к Интернету?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A6FE770B-A19A-4BF8-95E9-2CF880D4AFC5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: dc84ecc0ee3a71cc4e1d4233f4d6d5f22f597b07
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2019
ms.locfileid: "69523479"
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
