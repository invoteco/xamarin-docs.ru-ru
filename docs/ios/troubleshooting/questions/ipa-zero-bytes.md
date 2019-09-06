---
title: Размер IPA-файла составляет 0 байт
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 376BBA27-8694-4E63-9976-BF60349D42D8
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: 10fc13124a1c97cc7534d8cf14b7717b2ddc4fa1
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291044"
---
# <a name="ipa-file-is-0-bytes"></a>Размер IPA-файла составляет 0 байт

> [!IMPORTANT]
> Эта проблема решена в последних версиях Xamarin. Однако если проблема возникает в последней версии программного обеспечения, запишите [новую ошибку](~/cross-platform/troubleshooting/questions/howto-file-bug.md) с полными сведениями о версии и полным выходным файлом журнала сборки.



В предыдущих версиях Xamarin существовали некоторые известные проблемы, которые могут привести к тому, что файл IPA в Windows будет иметь значение 0 байт. 

### <a name="fixed-in-xamarin-for-visual-studio-311584"></a>Исправлено в Xamarin для Visual Studio 3.11.584 
- [Ошибка 24416. при создании конфигурации "ad-hoc" из командной строки не копирует файл IPA в Windows](https://bugzilla.xamarin.com/show_bug.cgi?id=24416)
- [Ошибка 24417-изменение свойства "Project Properties-> iOS IPA->" имя пакета "предотвращает копирование IPA в Windows](https://bugzilla.xamarin.com/show_bug.cgi?id=24417)
- [Ошибка 29822-[КСВС. iOS 3,11] Установка номера "сборка", отличающегося от номера версии, приводит к тому, что IPA не копируется в Windows](https://bugzilla.xamarin.com/show_bug.cgi?id=29822)

### <a name="fixed-in-xamarin-for-visual-studio-410496"></a>Исправлено в Xamarin для Visual Studio 4.1.0.496
- [Ошибка 27989. не удается отобразить файл IPA на сервере сборки, если имя сборки не соответствует имени проекта](https://bugzilla.xamarin.com/show_bug.cgi?id=27989)
