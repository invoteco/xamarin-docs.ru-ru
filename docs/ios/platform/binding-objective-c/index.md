---
title: Привязка библиотек iOS
description: В этом документе описывается создание C# привязок к коду цели-C, что позволяет использовать собственные библиотеки и CocoaPods в приложении Xamarin. iOS.
ms.prod: xamarin
ms.assetid: EBDC50DC-B44B-4003-AB2B-1EEB868A5E01
ms.technology: xamarin-ios
ms.custom: xamu-video
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 61d1adfc997b34302f1f89f56653af906ca90135
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022220"
---
# <a name="binding-ios-libraries"></a>Привязка библиотек iOS

Используйте следующие ссылки, чтобы узнать о привязке целевых библиотек C и CocoaPods для Xamarin. iOS и Xamarin. Mac:

- [**Общие сведения**](~/cross-platform/macios/binding/overview.md) -
  описывает, как работает привязка.
- [**Цели привязки — библиотеки C**](~/cross-platform/macios/binding/objective-c-libraries.md) -
  инструкции по привязке библиотек цели-c для использования в проектах Xamarin.
- [**Справочное руководство по определению типов**](~/cross-platform/macios/binding/binding-types-reference.md) -
  описывает все атрибуты, доступные для привязки авторов, чтобы управлять процессом создания привязки.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Цель Шарпие — это средство командной строки, помогающее выполнить начальную загрузку первого прохода привязки.
Он работает путем анализа файлов заголовков собственной библиотеки, чтобы сопоставлять открытый API с [определением привязки](~/cross-platform/macios/binding/objective-c-libraries.md) (процесс, который в противном случае выполняется вручную). Цель Шарпие не создает привязку самостоятельно, но может помочь вам приступить к работе!

Цель Шарпие 3,0 предоставила возможность непосредственной привязки Cocoapods!

## <a name="walkthrough---binding-an-ios-objective-c-librarywalkthroughmd"></a>[Пошаговое руководство. привязка библиотеки цели iOS-C](walkthrough.md)

На этой странице представлено пошаговое руководство по созданию проекта привязки iOS с использованием проекта [**инфколорпиккер**](https://github.com/InfinitApps/InfColorPicker) цели-C с открытым исходным кодом в качестве примера. Библиотека **инфколорпиккер** предоставляет доступный для повторного использования контроллер представлений, позволяющий пользователю выбирать цвет на основе его представления HSB, что делает выбор цветов более удобным для пользователя.
Целевое Шарпие будет использоваться для помощи в процессе привязки.

## <a name="video"></a>Видео

> [!VIDEO https://youtube.com/embed/ZUoPLcmnf1o]

**Привязки iOS в C/C++ видео**

## <a name="related-links"></a>Связанные ссылки

- [Привязка Objective-C](~/cross-platform/macios/binding/index.md)
- [Привязка Mac](~/mac/platform/binding.md)
