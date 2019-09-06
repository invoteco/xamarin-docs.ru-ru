---
title: Воспроизведение звука с помощью помощью avaudioplayer в Xamarin. Mac
description: В этом документе описывается, как воспроизвести звук с помощью помощью avaudioplayer в приложении Xamarin. Mac. В нем обсуждается помощью avaudioplayer на высоком уровне и ссылки на другую документацию, которая более подробно рассматривает ее.
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 10/19/2016
ms.openlocfilehash: b4a5ead3e3c02fbdd2ae5486a6ac637defeb5abd
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70283305"
---
# <a name="playing-sound-with-avaudioplayer-in-xamarinmac"></a>Воспроизведение звука с помощью помощью avaudioplayer в Xamarin. Mac

## <a name="about-the-avaudioplayer"></a>О помощью avaudioplayer

`AVAudioPlayer` Класс используется для воспроизведения звуковых данных из памяти или файла. Корпорация Apple рекомендует использовать этот класс для воспроизведения звука в приложении, если вы не выполняете потоковую передачу или требует ввода-вывода с низкой задержкой.

`AVAudioPlayer` Класс можно использовать для следующих задач:

- Воспроизводить звуки любой длительности с необязательным циклом.
- Одновременное воспроизведение нескольких звуков с дополнительной синхронизацией.
- Контроль громкости, скорость воспроизведения и положение стерео для каждого воспроизводимого звука.
- Поддержка таких функций, как быстрая переадресация или перемотка назад.
- Получение данных отслеживания уровня воспроизведения.

`AVAudioPlayer`поддерживает звук в любом звуковом формате, предоставляемом iOS, tvOS и macOS, например. AIF,. WAV или. mp3.

## <a name="playing-sounds-in-macos"></a>Воспроизведение звуков в macOS

Так как macOS поддерживает те же классы панели элементов, что и iOS, дополнительные сведения о воспроизведении звука в приложении Xamarin. Mac см. в статье [Воспроизведение звука в iOS с помощью помощью avaudioplayer](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/sound/avaudioplayer) документации.

## <a name="related-links"></a>Связанные ссылки

- [Справочник по помощью avaudioplayer](https://developer.apple.com/documentation/avfoundation/avaudioplayer)
