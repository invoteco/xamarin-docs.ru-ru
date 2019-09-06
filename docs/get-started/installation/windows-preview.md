---
title: Установка предварительной версии Xamarin в ОС Windows
description: В этом документе описывается, как установить предварительную версию Xamarin в Visual Studio 2019 с помощью канала предварительной версии.
ms.prod: xamarin
ms.assetid: 9F730444-06E8-4B3F-8A19-CA95CD484FFA
author: conceptdev
ms.author: crdun
ms.date: 03/20/2018
ms.openlocfilehash: 805edf0aba0e45a631bb9e3f7536da3f18dc789d
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70277648"
---
# <a name="installing-xamarin-preview-on-windows"></a>Установка предварительной версии Xamarin в ОС Windows

Visual Studio 2019 и Visual Studio 2017 не поддерживают альфа, бета-версии и стабильные каналы таким же образом, как и в предыдущих версиях. Вместо этого есть два варианта:

- **выпуск** — эквивалент _стабильного_ канала в Visual Studio для Mac;
- **предварительная версия** — эквивалент _альфа-_ и _бета-_ каналов в Visual Studio для Mac.

> [!TIP]
> Чтобы испытать функции предварительной версии, необходимо [скачать предварительную версию установщика Visual Studio](https://visualstudio.microsoft.com/vs/preview/), которая предлагает возможность установки **предварительных** версий Visual Studio параллельно с стабильной (выпуском) версией. Дополнительные сведения о новых возможностях Visual Studio 2019 см. в [заметках о выпуске](https://docs.microsoft.com/visualstudio/releases/2019/release-notes).

Предварительная версия Visual Studio может включать соответствующие предварительные версии функциональных возможностей Xamarin, в том числе следующие:

- Xamarin.Forms
- Xamarin.iOS
- Xamarin.Android
- Xamarin Profiler
- Xamarin Inspector
- Симулятор iOS удаленной работы для Xamarin

На снимке экрана **установщика предварительной версии** ниже показаны параметры и предварительной версии, и выпуска (обратите внимание на номера версий, указанные серым цветом: версия 15.0 относится к выпуску, а версия 15.1 — к предварительной версии):

![отображение параметров предварительной версии в установщике](windows-images/vs2017-installer.jpg)

В процессе установки, если выполняется параллельная установка, может применяться **псевдоним установки** (чтобы версии можно было различить в меню "Пуск"), как показано ниже:

[![Изменение псевдонима перед установкой](windows-images/vs2017-nickname-sml.png "edit nickname before installing")](windows-images/vs2017-nickname.png#lightbox)

### <a name="uninstalling-visual-studio-2019-preview"></a>Удаление предварительной версии Visual Studio 2019

**Visual Studio Installer** также следует использовать для отмены установки предварительных версий Visual Studio 2019. Дополнительные сведения см. в руководстве по [удалению Xamarin](uninstalling-xamarin.md#uninstallvs2017).
