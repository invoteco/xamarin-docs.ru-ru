---
title: Устранение неполадок Xamarin Workbooks в Android
description: Этот документ содержит советы по устранению неполадок для работы с Xamarin Workbooks в Android. В нем описывается поддержка эмулятора, книг, которые не загрузит и другим темам.
ms.prod: xamarin
ms.assetid: F1BD293B-4EB7-4C18-A699-718AB2844DFB
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: 08fa7f57f3fe44721bc00f0d59ed5df93300cf1e
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/13/2019
ms.locfileid: "67864030"
---
# <a name="troubleshooting-xamarin-workbooks-on-android"></a>Устранение неполадок Xamarin Workbooks в Android

## <a name="emulator-support"></a>Поддержка эмулятора

Чтобы запустить книгу Android, эмулятор Android должен быть доступен для использования. Физические устройства Android не поддерживаются.

Мы рекомендуем эмулятор от Google вместе с HAXM, если его поддерживает ваш компьютер.
Если необходимо иметь Hyper-V включена на компьютере, перейдите с помощью эмулятора Android Visual Studio.

Необходимо иметь эмулятор, выполняется Android 5.0 или более поздней версии. Эмуляторы ARM не поддерживаются. Используйте `x86` или `x86_64` только для устройств.

См. в статье [нашей документации по настройке эмуляторов Android][android-emu] Если вы не знакомы с процессом.

> [!NOTE]
> Книги, 1.1 и более ранних версий будет попробуйте (и ошибкой!) для использования эмуляторов ARM, если они доступны. Чтобы обойти этот, эмулятор запуска x86 по своему усмотрению до открытия или создания книги Android. Книги всегда предпочтут подключиться к запущенному эмулятору, пока он совместим.

## <a name="workbooks-wont-load"></a>Не будет загружать книги

### <a name="workbook-window-spins-forever-never-loads-windows"></a>Окна книги навсегда, вращается никогда не загружает (Windows)

Во-первых убедитесь, что симулятор имеет доступ к сети полностью рабочее тестирование любого веб-сайта в веб-браузере эмулятора.

### <a name="visual-studio-android-emulator-cannot-connect-to-the-internet"></a>Эмулятор Android Visual Studio не удается подключиться к Интернету

Если эмулятор не имеет доступа к сети, может потребоваться для исправления своего сетевого коммутатора Hyper-V выполните следующие действия. Повторите это действие периодически может потребоваться в том случае, если переключаться между режимами часто сетей Wi-Fi:

1. **Убедитесь, что любые важные сетевые операции завершены, как это могут быть временно разорваны Windows из Интернета.**
1. Закройте эмуляторы.
1. Откройте `Hyper-V Manager`.
1. В разделе `Actions`откройте `Virtual Switch Manager...`.
1. Удалите все виртуальные коммутаторы.
1. Нажмите кнопку `OK`.
1. Запуск эмулятора Android Visual STUDIO. Возможно, вам будет предложено повторно создать виртуальный сетевой коммутатор.
1. Тест, браузер Android эмулятора Visual STUDIO доступа в Интернет.

[android-emu]: https://developer.xamarin.com/guides/android/deployment,_testing,_and_metrics/debug-on-emulator/


## <a name="related-links"></a>Связанные ссылки

- [Сообщения об ошибках](~/tools/workbooks/install.md#reporting-bugs)
