---
title: Ошибка конструктора iOS RegisterServicePort
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
ms.openlocfilehash: fc4c143d6b5f7c211d24e6e3ed2ed3bb8d264410
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61421974"
---
# <a name="ios-designer-error-with-registerserviceport"></a>Ошибка конструктора iOS RegisterServicePort

## <a name="sample-error"></a>Пример ошибки
> System.AggregateException: Произошла одна или несколько ошибок---> System.SystemException: RegisterServicePort(com.xamarin.MTHosting.2a0b1, com.apple.PowerManagement.control): Возвращаемый ядра: -308 (-308): умер сервера (ipc/mig)

## <a name="explanation"></a>Объяснение
Ошибки, связанные с `RegisterServicePort` и аналогичные сообщения об ошибках как выше обычно представляют собой проблему шпионского ПО и вредоносных программ на компьютере. Попробуйте [комментарий для этого отчета об ошибках](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4) Дополнительные сведения и ссылки [обсуждения в форумах Apple](https://discussions.apple.com/thread/5596008) об удалении заражения. 

Чтобы помочь в диагностике проблемы, откройте приложение macOS **консоли** и удалите каждый файл внутри **пользователя диагностические отчеты** разделе [ http://screencast.com/t/y9i3NKcuMy ](http://screencast.com/t/y9i3NKcuMy). Затем запустите Visual Studio для Mac и попробуйте использовать конструктор. Если создание файлов журнала отображается в этом разделе после конструктора не удалось инициализировать, сохраните их для необходимости анализировать.  

Обратите внимание на то, что наиболее важно проверить, является ли этот файл: 
> /usr/lib/libimckit.dylib

Независимо от того, приведенные выше результаты если этот файл существует, упомянутых выше проблему шпионского ПО и вредоносных программ на компьютере.  

Используйте следующую ссылку включает шаги для удаления шпионских программ или вредоносной программе. [http://www.thesafemac.com/arg-genieo/](http://www.thesafemac.com/arg-genieo/)  

