---
title: Устранение неполадок Xamarin Workbooks в Android
description: Этот документ содержит советы по устранению неполадок при работе с Xamarin Workbooks на Android. В нем обсуждается поддержка эмулятора, книги, которые не загружаются, и другие разделы.
ms.prod: xamarin
ms.assetid: F1BD293B-4EB7-4C18-A699-718AB2844DFB
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: be19005ab1125c060ab0111e9f37568d5f4abe45
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029592"
---
# <a name="troubleshooting-xamarin-workbooks-on-android"></a>Устранение неполадок Xamarin Workbooks в Android

## <a name="emulator-support"></a>Поддержка эмулятора

Для запуска книги Android эмулятор Android должен быть доступен для использования. Физические устройства Android не поддерживаются.

Мы рекомендуем использовать эмулятор Google вместе с HAXM, если он поддерживается компьютером.
Если в системе необходимо включить Hyper-V, вместо этого перейдите в Android Emulator Visual Studio.

Необходим эмулятор, работающий под управлением Android 5,0 или более поздней версии. Эмуляторы ARM не поддерживаются. Используйте только `x86` или `x86_64` устройства.

Ознакомьтесь с [документацией по настройке эмуляторов Android][android-emu] , если вы не знакомы с этим процессом.

> [!NOTE]
> Книги 1,1 и более ранних версий попытаются (и завершатся ошибкой) использовать Эмуляторы ARM, если они доступны. Чтобы обойти это, запустите эмулятор x86 по своему усмотрению перед открытием или созданием книги Android. Книги всегда предпочитают подключаться к работающему эмулятору, если он является совместимым.

## <a name="workbooks-wont-load"></a>Книги не загружаются

### <a name="workbook-window-spins-forever-never-loads-windows"></a>Окно книги непрерывно вращается, никогда не загружается (Windows)

Сначала убедитесь, что у эмулятора есть полный сетевой доступ, проверив любой веб-сайт в веб-браузере эмулятора.

### <a name="visual-studio-android-emulator-cannot-connect-to-the-internet"></a>Visual Studio Android Emulator не удается подключиться к Интернету

Если у эмулятора нет доступа к сети, для исправления сетевого коммутатора Hyper-V может потребоваться выполнить следующие действия. При частом переключении между сетями Wi-Fi может потребоваться повторить это периодически.

1. **Убедитесь, что все критические сетевые операции завершены, так как это может временно отключить Windows от Интернета.**
1. Закройте Эмуляторы.
1. Откройте `Hyper-V Manager`.
1. В разделе `Actions`откройте `Virtual Switch Manager...`.
1. Удалите все виртуальные коммутаторы.
1. Нажмите кнопку `OK`.
1. Запуск VS Android Emulator. Возможно, вам будет предложено повторно создать коммутатор виртуальной сети.
1. Проверьте, что браузер VS Android Emulator имеет доступ к Интернету.

[android-emu]: ~/android/deploy-test/debugging/debug-on-emulator.md

## <a name="related-links"></a>Связанные ссылки

- [Сообщения об ошибках](~/tools/workbooks/install.md#reporting-bugs)
