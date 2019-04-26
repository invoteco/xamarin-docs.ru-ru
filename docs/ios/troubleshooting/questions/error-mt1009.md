---
title: 'Ошибка MT1009: не удалось скопировать сборку'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F9FEDFF5-C84C-42B4-8F25-E34846E7315A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 4a67537cc53aeecf1b86d11dbf041cea79587dd2
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61422013"
---
# <a name="error-mt1009-could-not-copy-the-assembly"></a>Ошибка MT1009: не удалось скопировать сборку

> [!IMPORTANT]
> Эта проблема устранена в последних версиях Xamarin.iOS. Тем не менее, если эта проблема возникает на последнюю версию программного обеспечения, отправьте [новую ошибку](~/cross-platform/troubleshooting/questions/howto-file-bug.md) благодаря полное управление версиями сведения и полный создавать выходные данные журнала.

Как описано в нашем [заметки о выпуске](https://developer.xamarin.com/releases/ios/xamarin.ios_7/xamarin.ios_7.2/), это известная проблема в Xamarin.iOS 7.2.6. Проблема возникает из-за необходимости более высокие привилегии при установке Xamarin.iOS с помощью другой учетной записи разрешения для файлов затем основной учетной записи разработчика.

Чтобы устранить эту проблему откройте Terminal.app на рабочей станции Mac и введите следующую команду:

`sudo chmod 0644 /Developer/MonoTouch/usr/lib/mono/2.1/monotouch.dll.mdb`

