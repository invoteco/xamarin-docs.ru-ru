---
title: Воспроизведение звука в tvOS с помощью помощью avaudioplayer в Xamarin
description: В этой статье показано, как использовать вспомогательный класс для управления воспроизведением звука с помощью помощью avaudioplayer в приложении Xamarin. iOS.
ms.prod: xamarin
ms.assetid: E0305572-DC64-48BB-BD97-0A5096E6CA04
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 4dddde8d4408df6a9b9d73c0a3efff62f563591a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030779"
---
# <a name="playing-sound-in-tvos-with-avaudioplayer-in-xamarin"></a>Воспроизведение звука в tvOS с помощью помощью avaudioplayer в Xamarin

## <a name="about-the-avaudioplayer"></a>О помощью avaudioplayer

`AVAudioPlayer` используется для воспроизведения звуковых данных из памяти или файла. Корпорация Apple рекомендует использовать этот класс для воспроизведения звука в приложении, если вы не выполняете потоковую передачу или требует ввода-вывода с низкой задержкой.

`AVAudioPlayer` можно использовать для следующих задач:

- Воспроизводить звуки любой длительности с необязательным циклом.
- Одновременное воспроизведение нескольких звуков с дополнительной синхронизацией.
- Контроль громкости, скорость воспроизведения и положение стерео для каждого воспроизводимого звука.
- Поддержка таких функций, как быстрая переадресация или перемотка назад.
- Получение данных отслеживания уровня воспроизведения.

`AVAudioPlayer` поддерживает звук в любом звуковом формате, предоставляемом iOS, tvOS и OS X, например `.aif`, `.wav` или `.mp3`.

## <a name="playing-sounds-in-tvos"></a>Воспроизведение звуков в tvOS

Так как tvOS поддерживает те же классы панели элементов, что и iOS, дополнительные сведения о воспроизведении звука в приложении Xamarin. tvOS см. в статье воспроизведение звука в iOS [с помощью помощью avaudioplayer](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/sound/avaudioplayer) документации.

## <a name="related-links"></a>Связанные ссылки

- [Справочник по помощью avaudioplayer](https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVAudioPlayerClassReference/)
