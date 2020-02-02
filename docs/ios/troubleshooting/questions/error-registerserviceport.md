---
title: Ошибка конструктора iOS RegisterServicePort
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: b76de0f0dc8f1fd2124eac9cfe03b872c71095b1
ms.sourcegitcommit: 52fb214c0e0243587d4e9ad9306b75e92a8cc8b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2020
ms.locfileid: "76940955"
---
# <a name="ios-designer-error-with-registerserviceport"></a>Ошибка конструктора iOS RegisterServicePort

## <a name="sample-error"></a>Пример ошибки
> System. AggregateException: произошла одна или несколько ошибок---> System. SystemException: Регистерсервицепорт (com. Xamarin. Мсостинг. 2a0b1, com. Apple. Поверманажемент. Control): ядро вернуло:-308 (-308): (IPC/MIG) сервер умер

## <a name="explanation"></a>Пояснение
Ошибки, возникающие с `RegisterServicePort` и похожими сообщениями об ошибках, как показано выше, обычно являются проблемой с программами-шпионами и вредоносными программами. Дополнительные сведения см. в [комментариях к этому отчету об ошибках](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4) , а также в ссылке на [форуме Apple Forum](https://discussions.apple.com/thread/5596008) о том, как удалить возможное заражение. 

Чтобы помочь в диагностике проблемы, откройте **консоль** приложения macOS и удалите каждый файл в разделе " **отчеты о диагностике пользователей** " [https://screencast.com/t/y9i3NKcuMy](https://screencast.com/t/y9i3NKcuMy). Затем запустите Visual Studio для Mac и попытайтесь использовать конструктор. Если в этом разделе появляются новые файлы журнала после сбоя инициализации конструктора, сохраните их для анализа.  

Обратите внимание, что важно проверить наличие этого файла: 
> /usr/lib/libimckit.dylib

Независимо от указанных выше результатов, если этот файл существует, то на вашем компьютере присутствует упомянутая выше ошибка шпионского по или вредоносного по.  

Ниже приведена ссылка на действия по удалению программы-шпиона или вредоносной программы: [https://www.thesafemac.com/arg-genieo/](https://www.thesafemac.com/arg-genieo/)  
