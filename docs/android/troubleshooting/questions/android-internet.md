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
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027009"
---
# <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>Почему моя сборка выпуска Android не подключается к Интернету?

## <a name="cause"></a>Причина:

Чаще всего эта проблема возникает из-за автоматического включения разрешения **INTERNET** в отладочную сборку, когда для сборки выпуска его необходимо задать вручную. Это связано с тем, что разрешение на Интернет-подключение позволяет отладчику присоединяться к процессу, как описано в [этой статье](~/android/deploy-test/building-apps/build-process.md) (сведения о DebugSymbols).

## <a name="fix"></a>Исправление

Чтобы устранить эту проблему, можно запросить разрешение на Интернет-подключение в манифесте Android. Это можно сделать с помощью редактора или исходного кода манифеста:

- Исправление в редакторе: в проекте Android выберите **Properties (Свойства) ->   AndroidManifest.xml -> Required Permissions (Требуемые разрешения)** и установите флажок **Internet** (Интернет).

- Исправление в исходном коде: откройте AndroidManifest в редакторе исходного кода и добавьте тег разрешения между тегами `<Manifest>`:

    ```xml
    <Manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    </Manifest>
    ```
