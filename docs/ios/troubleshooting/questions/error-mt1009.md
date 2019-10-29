---
title: 'Ошибка MT1009: не удалось скопировать сборку'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F9FEDFF5-C84C-42B4-8F25-E34846E7315A
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 1ccf85c03ab1f2cff6428a0120a8ceb4de250761
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031150"
---
# <a name="error-mt1009-could-not-copy-the-assembly"></a>Ошибка MT1009: не удалось скопировать сборку

> [!IMPORTANT]
> Эта проблема решена в последних версиях Xamarin. iOS. Однако если проблема возникает в последней версии программного обеспечения, запишите [новую ошибку](~/cross-platform/troubleshooting/questions/howto-file-bug.md) с полными сведениями о версии и полным выходным файлом журнала сборки.

Как описано в [заметках о выпуске](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/xamarin.ios_7/xamarin.ios_7.2/index.md), это известная проблема в Xamarin. iOS 7.2.6. Эта проблема вызвана тем, что при установке Xamarin. iOS с другой учетной записью пользователя, которая является основной учетной записью разработчика, требуются более высокие права доступа к файлам.

Чтобы решить эту проблему, откройте приложение Terminal. app на компьютере Mac и выполните следующую команду:

`sudo chmod 0644 /Developer/MonoTouch/usr/lib/mono/2.1/monotouch.dll.mdb`
