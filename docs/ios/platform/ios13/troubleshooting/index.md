---
title: Xamarin и устранение неполадок операций ввода-вывода 13
description: Этот раздел содержит советы по устранению неполадок для компонентов Xamarin, связанные с iOS 13.
ms.prod: xamarin
ms.assetid: 00DE8C33-1407-45C0-A0C7-32AF1E490034
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/12/2019
ms.openlocfilehash: 99fd7e41e1521c6a9254d286bf989281658ccf24
ms.sourcegitcommit: 85c45dc28ab3625321c271804768d8e4fce62faf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67039791"
---
# <a name="troubleshooting-tips-for-ios-13-and-xamarinios"></a>Советы для iOS 13 и Xamarin.iOS

![Предварительная версия](~/media/shared/preview.png)

## <a name="updating-to-xcode-11-stops-the-simulator-from-launching"></a>Обновление до Xcode 11 останавливает запуск симулятора

После обновления до **Xcode 11 бета-версия 1** каждый раз при запуске симулятор следующее исключение, и не запускается в симуляторе. Это осуществляется с помощью всех симуляторах.

### <a name="exception"></a>Исключение

`Foundation.ObjCException: NSInvalidArgumentException: -[SimDevice registerNotificationHandler:]: unrecognized selector sent to instance 0x7ffbf5d1e110`

### <a name="workaround"></a>Обходной путь

Пока появится [исправить](https://github.com/xamarin/xamarin-macios/issues/6216), повторная установка старого симулятор платформу, позволяющую разработчикам работать по-прежнему применимы следующие действия:

> [!NOTE]
> Далее предполагается, что у вас есть два приложения Xcode:
> - **Xcode11 beta1.app** – бета-версии, что не работает с симуляторы и Visual Studio для Mac.
> - **Xcode102.App** — стабильная версия Xcode 10. Вам также может называться **Xcode.app**.
>
> Измените приведенных ниже примерах командной строки, подходящие для вашей конфигурации.

1. Убедитесь, что у вас есть 11 Xcode, выбранный в рамках xcode-select:

   `sudo xcode-select -s /Applications/Xcode11-beta1.app/Contents/Developer/`

2. Запустить, при необходимости, настройки и средств в первый раз.

    `/Applications/Xcode11-beta1.app/Contents/Developer/usr/bin/xcodebuild -runFirstLaunch`

3. Удалите следующие платформы:

    `sudo rm -Rf  /Library/Developer/PrivateFrameworks/CoreSimulator.framework/Versions/*`

4. Вернуться к старой версии Xcode

   `sudo xcode-select -s /Applications/Xcode102.app/Contents/Developer/`

5. Повторно запустите средство первого запуска для _СТАРЫЙ_ выбранную версию Xcode

   `/Applications/Xcode102.app/Contents/Developer/usr/bin/xcodebuild -runFirstLaunch`

После выполнения этих действий можно будет возобновить работу с симулятором iOS.