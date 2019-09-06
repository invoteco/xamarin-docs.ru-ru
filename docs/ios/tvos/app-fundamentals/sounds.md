---
title: Воспроизведение звука в tvOS с помощью помощью avaudioplayer в Xamarin
description: В этой статье показано, как использовать вспомогательный класс для управления воспроизведением звука с помощью помощью avaudioplayer в приложении Xamarin. iOS.
ms.prod: xamarin
ms.assetid: E0305572-DC64-48BB-BD97-0A5096E6CA04
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: b34c769eaa3aef5bf47a9bfa891859289b195f03
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70283786"
---
# <a name="playing-sound-in-tvos-with-avaudioplayer-in-xamarin"></a>Воспроизведение звука в tvOS с помощью помощью avaudioplayer в Xamarin

## <a name="about-the-avaudioplayer"></a>О помощью avaudioplayer

`AVAudioPlayer` Используется для воспроизведения звуковых данных из памяти или файла. Корпорация Apple рекомендует использовать этот класс для воспроизведения звука в приложении, если вы не выполняете потоковую передачу или требует ввода-вывода с низкой задержкой.

`AVAudioPlayer` Для этого можно использовать команду.

- Воспроизводить звуки любой длительности с необязательным циклом.
- Одновременное воспроизведение нескольких звуков с дополнительной синхронизацией.
- Контроль громкости, скорость воспроизведения и положение стерео для каждого воспроизводимого звука.
- Поддержка таких функций, как быстрая переадресация или перемотка назад.
- Получение данных отслеживания уровня воспроизведения.

`AVAudioPlayer`поддерживает звук в любом звуковом формате, предоставляемом iOS, tvOS и OS X `.aif`, `.wav` например `.mp3`, или.

## <a name="playing-sounds-in-tvos"></a>Воспроизведение звуков в tvOS

Так как tvOS поддерживает те же классы панели элементов, что и iOS, дополнительные сведения о воспроизведении звука в приложении Xamarin. tvOS см. в статье воспроизведение звука в iOS [с помощью помощью avaudioplayer](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/sound/avaudioplayer) документации.



## <a name="related-links"></a>Связанные ссылки

- [Справочник по помощью avaudioplayer](https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVAudioPlayerClassReference/)
