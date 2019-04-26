---
title: Воспроизведение звука с помощью AVAudioPlayer в Xamarin.Mac
description: В этом документе описывается воспроизведение звука с помощью AVAudioPlayer в приложении Xamarin.Mac. Здесь рассматриваются помощью AVAudioPlayer на высоком уровне и ссылки на другую документацию, которая более полно освещает его.
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 10/19/2016
ms.openlocfilehash: 9aeb7bbfc2fddef1f690b5299ec060c475ea1ce7
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61234872"
---
# <a name="playing-sound-with-avaudioplayer-in-xamarinmac"></a>Воспроизведение звука с помощью AVAudioPlayer в Xamarin.Mac

## <a name="about-the-avaudioplayer"></a>О помощью AVAudioPlayer

`AVAudioPlayer` Класс используется для воспроизведения звуковых данных из памяти или файла. Компания Apple рекомендует использовать этот класс для воспроизведение звука в приложении, если вы не выполняете сети потоковой передачи или потребовать аудио низкую задержку ввода-вывода.

Можно использовать `AVAudioPlayer` класс для следующих целей:

- Воспроизведение звуков любой продолжительности необязательно выполнения цикла.
- Несколько звук в то же время, с необязательным синхронизации.
- Управлять тома, темп воспроизведения и стерео размещение каждого звуков, воспроизведение.
- Поддерживает функции, такие как Перемотка вперед или назад.
- Получите уровень воспроизведения данных отслеживания использования.

`AVAudioPlayer` поддерживает звуков в любой формат аудио, предоставляемого системами iOS, tvOS и macOS, например .aif, WAV или MP3.

## <a name="playing-sounds-in-macos"></a>Воспроизведение звуков в macOS

Так как macOS поддерживает те же классы аудио элементов, как iOS, см. наш iOS [Игральных звука с помощью AVAudioPlayer](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/sound/avaudioplayer) документации, полный список воспроизведения звука в приложении Xamarin.Mac.

## <a name="related-links"></a>Связанные ссылки

- [Справочник по с помощью AVAudioPlayer](https://developer.apple.com/documentation/avfoundation/avaudioplayer)
