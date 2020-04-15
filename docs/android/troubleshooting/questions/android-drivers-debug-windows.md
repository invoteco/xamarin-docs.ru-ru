---
title: Какие драйверы USB нужны для отладки Android в Windows?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/22/2018
ms.openlocfilehash: 21fd8eff64d374e52e64194524a8c096cdf4d90e
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027043"
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Какие драйверы USB нужны для отладки Android в Windows?

## <a name="finding-usb-drivers"></a>Как найти USB-драйверы

Для отладки на устройстве Android при разработке в Windows необходимо установить совместимый драйвер USB. Диспетчер SDK Android по умолчанию включает USB-драйвер Google, который обеспечивает дополнительную поддержку для устройств Nexus, как описано здесь: [https://developer.android.com/sdk/win-usb.html](https://developer.android.com/sdk/win-usb.html).

Для других устройств требуются драйверы USB, специально опубликованные производителем устройства. В этом руководстве содержатся некоторые ссылки на наиболее распространенных производителей: [https://developer.android.com/tools/extras/oem-usb.html](https://developer.android.com/tools/extras/oem-usb.html).

## <a name="alternatives"></a>Альтернативные варианты

Иногда может быть трудно найти необходимый драйвер USB. Это зависит от производителя. Есть несколько альтернатив для тестирования приложений Android, разработанных в Windows, включая использование эмулятора Android или внешних служб тестирования. Некоторые из них указаны ниже:

- [Тестирование в Центре приложений](https://docs.microsoft.com/appcenter/test-cloud/) — облачные службы тестирования работают на сотнях реальных устройств Android.

- [Эмулятор Visual Studio для Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/)

- [Отладка приложений в Android Emulator](~/android/deploy-test/debugging/debug-on-emulator.md)
