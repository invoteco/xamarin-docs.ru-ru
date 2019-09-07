---
title: Размер IPA-файла составляет 0 байт
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 376BBA27-8694-4E63-9976-BF60349D42D8
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: ba86567213fe76847459df3df9b4450881e0651e
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769234"
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
