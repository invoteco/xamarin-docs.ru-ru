---
title: Отладка приложений Xamarin.Android на устройствах и эмуляторах
description: Тестирование и отладка приложения Xamarin.Android
ms.prod: xamarin
ms.assetid: A355A471-8195-4391-93FE-0000BCB17923
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/22/2018
ms.openlocfilehash: 3b3fa14ec81bd4f06322197b7140654f9086ce73
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "75556487"
---
# <a name="debug-xamarinandroid-apps"></a>Отладка приложений Xamarin.Android

В этом разделе описываются принципы отладки приложения Xamarin.Android на устройствах или эмуляторах.

## <a name="debugging-overview"></a>Общие сведения об отладке

При разработке приложений Android приложения требуется запускать либо на физическом оборудовании, либо с помощью эмулятора. Использование оборудования является лучшим, но не всегда самым целесообразным вариантом. Во многих случаях может быть проще и рентабельнее смоделировать или сэмулировать оборудование Android с помощью одного из описанных ниже эмуляторов.

### <a name="debugging-on-the-android-emulator"></a>[Отладка приложений в Android Emulator](~/android/deploy-test/debugging/debug-on-emulator.md)

В этой статье описано, как открыть Android Emulator из Visual Studio и запустить приложение на виртуальном устройстве.

### <a name="debugging-on-a-device"></a>[Отладка на устройстве](~/android/deploy-test/debugging/debug-on-device.md)

В этой статье показано, как настроить физическое устройство Android для развертывания на нем приложения Xamarin.Android непосредственно в Visual Studio или Visual Studio для Mac.

### <a name="android-debug-log"></a>[Журнал отладки Android](~/android/deploy-test/debugging/android-debug-log.md)

Очень часто разработчики для отладки своих приложений используют `Console.WriteLine`. Однако на мобильной платформе, такой как Android, консоль отсутствует. На устройствах Android доступен журнал, который, скорее всего, потребуется вам при создании приложения. Иногда его называют **logcat** из-за команды, вводимой для его получения. Из этой статьи вы узнаете, как использовать **logcat**.
