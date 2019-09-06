---
title: MDocArchiveToMsxDocConverter.exe не найден, rver.BaseCommand.OnRequest
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5AC6AC4-0E7C-4746-A7CF-872F0E75AFF4
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: d8c0c958a6aaad2603f6571a3531b7338d8cdab0
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291009"
---
# <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequest"></a>MDocArchiveToMsxDocConverter.exe не найден, rver.BaseCommand.OnRequest

> [!IMPORTANT]
> Эта проблема решена в последних версиях Xamarin. Однако если проблема возникает в последней версии программного обеспечения, запишите [новую ошибку](~/cross-platform/troubleshooting/questions/howto-file-bug.md) с полными сведениями о версии и полным выходным файлом журнала сборки.


## <a name="error-message"></a>Сообщение об ошибке

Эта ошибка может появиться в *журнале сервера Mac* в Visual Studio:

```
Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found
 rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context, System.Object commandRequestState) [0x00000] in <filename unknown>:0
  at Mtb.Server.Listener.OnRequest (System.Object state) [0x00000] in <filename unknown>:0
```

В этом сообщении есть две отдельные проблемы:

1. `Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found`

    Эта ошибка является безвредной, но также приводит к неверности. В следующем выпуске он [будет удален](https://bugzilla.xamarin.com/show_bug.cgi?id=21667) .

2. `rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context …`

    Эта ошибка является реальной проблемой. К сожалению, из-за [ограничения](https://bugzilla.xamarin.com/show_bug.cgi?id=22080) Эта трассировка стека исключений является *неполной*. Если вы заметили неполную трассировку стека подобным образом в журнале сервера Mac, можно проверить `~/Library/Logs/Xamarin/MonoTouchVS/mtbserver.log` файл на узле сборки Mac, чтобы найти полную трассировку стека.
