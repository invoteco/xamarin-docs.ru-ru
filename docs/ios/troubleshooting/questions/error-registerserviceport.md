---
title: Ошибка конструктора iOS RegisterServicePort
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: 68c87355a2a6a081e0fff741ffe8a4466abb540a
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292604"
---
# <a name="ios-designer-error-with-registerserviceport"></a>Ошибка конструктора iOS RegisterServicePort

## <a name="sample-error"></a>Пример ошибки
> System. AggregateException: ---> System. SystemException произошла одна или несколько ошибок: Регистерсервицепорт (com. Xamarin. Мсостинг. 2a0b1, com. Apple. Поверманажемент. Control): Ядро вернуло:-308 (-308): (IPC/MIG) умер сервера

## <a name="explanation"></a>Объяснение
`RegisterServicePort` Ошибки и подобные сообщения об ошибках, подобные описанным выше, обычно являются проблемой с программами-шпионами и вредоносными программами на компьютере. Дополнительные сведения см. в [комментариях к этому отчету об ошибках](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4) , а также в ссылке на [форуме Apple Forum](https://discussions.apple.com/thread/5596008) о том, как удалить возможное заражение. 

Чтобы помочь в диагностике проблемы, откройте **консоль** приложения macOS и удалите каждый файл в разделе [http://screencast.com/t/y9i3NKcuMy](http://screencast.com/t/y9i3NKcuMy)" **отчеты о диагностике пользователей** ". Затем запустите Visual Studio для Mac и попытайтесь использовать конструктор. Если в этом разделе появляются новые файлы журнала после сбоя инициализации конструктора, сохраните их для анализа.  

Обратите внимание, что важно проверить наличие этого файла: 
> /usr/lib/libimckit.dylib

Независимо от указанных выше результатов, если этот файл существует, то на вашем компьютере присутствует упомянутая выше ошибка шпионского по или вредоносного по.  

Ниже приведена ссылка на действия по удалению программы-шпиона или вредоносной программы.[http://www.thesafemac.com/arg-genieo/](http://www.thesafemac.com/arg-genieo/)  

