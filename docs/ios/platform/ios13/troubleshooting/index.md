---
title: Устранение неполадок Xamarin и iOS 13
description: В этом разделе содержатся советы по устранению неполадок в работе Xamarin, связанных с iOS 13.
ms.prod: xamarin
ms.assetid: 00DE8C33-1407-45C0-A0C7-32AF1E490034
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/12/2019
ms.openlocfilehash: 9a12163b0300a8d523632ad38e1b66a8c44b7aaf
ms.sourcegitcommit: 09bc69d7119a04684c9e804c5cb113b8b1bb7dfc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2019
ms.locfileid: "71206316"
---
# <a name="troubleshooting-tips-for-ios-13-and-xamarinios"></a>Советы по устранению неполадок в iOS 13 и Xamarin. iOS

## <a name="updating-to-xcode-11-stops-the-simulator-from-launching"></a>Обновление до Xcode 11 останавливает запуск симулятора

После обновления до **Xcode 11 Beta 1** при каждом запуске симулятора создается следующее исключение, и симулятор не запускается. Это происходит со всеми симуляторами.

### <a name="exception"></a>Исключение

`Foundation.ObjCException: NSInvalidArgumentException: -[SimDevice registerNotificationHandler:]: unrecognized selector sent to instance 0x7ffbf5d1e110`

### <a name="workaround"></a>Возможное решение

Пока не будет установлено [исправление](https://github.com/xamarin/xamarin-macios/issues/6216), можно выполнить следующие действия, чтобы переустановить старую платформу симуляторов, чтобы разработчики могли продолжать работу:

> [!NOTE]
> В этих шагах предполагается, что у вас есть два Xcode приложения:
>
> - **Xcode11-Beta1. app** — бета-версия, которая не работает с симуляторами и Visual Studio для Mac.
> - **Xcode102. app** — стабильная версия Xcode 10. Кроме того, можно назвать **Xcode. app**.
>
> Измените приведенные ниже примеры командной строки в соответствии с конфигурацией.

1. Убедитесь, что выбрано Xcode 11 с помощью Xcode-SELECT:

   `sudo xcode-select -s /Applications/Xcode11-beta1.app/Contents/Developer/`

2. Запустите (при необходимости) средства установки в первый раз.

    `/Applications/Xcode11-beta1.app/Contents/Developer/usr/bin/xcodebuild -runFirstLaunch`

3. Удалите следующую платформу:

    `sudo rm -Rf  /Library/Developer/PrivateFrameworks/CoreSimulator.framework/Versions/*`

4. Вернитесь к старой версии Xcode

   `sudo xcode-select -s /Applications/Xcode102.app/Contents/Developer/`

5. Повторное выполнение первого средства запуска для только что выбранной _старой_ версии Xcode

   `/Applications/Xcode102.app/Contents/Developer/usr/bin/xcodebuild -runFirstLaunch`

После выполнения этих действий вы сможете снова работать с симулятором iOS.
