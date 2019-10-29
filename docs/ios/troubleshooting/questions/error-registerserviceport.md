---
title: Ошибка конструктора iOS RegisterServicePort
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: 07b7e47ac448fdb9a4297fe03e8f4ea16295fddd
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031138"
---
# <a name="ios-designer-error-with-registerserviceport"></a>Ошибка конструктора iOS RegisterServicePort

## <a name="sample-error"></a>Пример ошибки
> System. AggregateException: произошла одна или несколько ошибок---> System. SystemException: Регистерсервицепорт (com. Xamarin. Мсостинг. 2a0b1, com. Apple. Поверманажемент. Control): ядро вернуло:-308 (-308): (IPC/MIG) сервер умер

## <a name="explanation"></a>Объяснение
Ошибки, возникающие с `RegisterServicePort` и похожими сообщениями об ошибках, как показано выше, обычно являются проблемой с программами-шпионами и вредоносными программами. Дополнительные сведения см. в [комментариях к этому отчету об ошибках](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4) , а также в ссылке на [форуме Apple Forum](https://discussions.apple.com/thread/5596008) о том, как удалить возможное заражение. 

Чтобы помочь в диагностике проблемы, откройте **консоль** приложения macOS и удалите каждый файл в разделе " **отчеты о диагностике пользователей** " [https://screencast.com/t/y9i3NKcuMy](https://screencast.com/t/y9i3NKcuMy). Затем запустите Visual Studio для Mac и попытайтесь использовать конструктор. Если в этом разделе появляются новые файлы журнала после сбоя инициализации конструктора, сохраните их для анализа.  

Обратите внимание, что важно проверить наличие этого файла: 
> /уср/либ/либимккит.дилиб

Независимо от указанных выше результатов, если этот файл существует, то на вашем компьютере присутствует упомянутая выше ошибка шпионского по или вредоносного по.  

Ниже приведена ссылка на действия по удалению программы-шпиона или вредоносной программы: [https://www.thesafemac.com/arg-genieo/](https://www.thesafemac.com/arg-genieo/)  
