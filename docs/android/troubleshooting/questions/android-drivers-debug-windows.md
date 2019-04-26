---
title: Какие драйверы USB нужны для отладки Android в Windows?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/22/2018
ms.openlocfilehash: 85045967f5c63eb39c45f917b957d2a393a3a068
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60945566"
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Какие драйверы USB нужны для отладки Android в Windows?

## <a name="finding-usb-drivers"></a>Поиск драйверов USB

Отладка на устройстве Android, при разработке в Windows; необходимо установить драйвер USB. Диспетчер Android SDK включает в себя «Драйвер Google USB» по умолчанию, который добавляет поддержку для устройств хранилища, как описано здесь: [https://developer.android.com/sdk/win-usb.html](https://developer.android.com/sdk/win-usb.html)

Другие устройства требуют драйверов USB, в частности опубликованных производителем устройства. Некоторые ссылки для наиболее распространенных производителей включаются в этом руководстве: [https://developer.android.com/tools/extras/oem-usb.html](https://developer.android.com/tools/extras/oem-usb.html)

## <a name="alternatives"></a>Альтернативные варианты

В зависимости от manfacturer бывает сложно отследить точное требуется USB-драйвера. Некоторые альтернативные решения для тестирования приложений Android разработан в Windows, в том числе с помощью эмулятора Android или использование внешних служб тестирования. Некоторые из них указаны ниже:

- [Тестов центра приложений](https://docs.microsoft.com/appcenter/test-cloud/) - облако тестирование служб, запускаемых на сотнях реальных устройств Android.

- [Эмулятор Visual Studio для Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/)

- [Отладка приложений в Android Emulator](~/android/deploy-test/debugging/debug-on-emulator.md)

