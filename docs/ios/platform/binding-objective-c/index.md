---
title: Привязка библиотек iOS
description: В этом документе описывается создание C# привязки для кода Objective-C, что позволяет использовать собственные библиотеки и CocoaPods в приложении Xamarin.iOS.
ms.prod: xamarin
ms.assetid: EBDC50DC-B44B-4003-AB2B-1EEB868A5E01
ms.technology: xamarin-ios
ms.custom: xamu-video
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 8c4dcbe0baf74479e94f8663280e7654b4d58a9d
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/01/2019
ms.locfileid: "64978157"
---
# <a name="binding-ios-libraries"></a>Привязка библиотек iOS

Используйте следующие ссылки, чтобы узнать о привязке библиотеки Objective-C и CocoaPods для Xamarin.iOS и Xamarin.Mac.

- [**Общие сведения о** ](~/cross-platform/macios/binding/overview.md) -
  описан принцип действия привязки.
- [**Привязка библиотек Objective-C** ](~/cross-platform/macios/binding/objective-c-libraries.md) -
  инструкции о том, как выполнить привязку библиотеки Objective-C для использования в проектах Xamarin.
- [**Введите справочное руководство по определению** ](~/cross-platform/macios/binding/binding-types-reference.md) -
  описывает все атрибуты, доступные для авторов привязки для управления процессом создания привязки.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Цели Sharpie — это средство командной строки для начальной загрузки при первом проходе привязки.
Он работает путем анализа файлов заголовков из собственной библиотеки для сопоставления открытого API-интерфейса в [определения привязки](~/cross-platform/macios/binding/objective-c-libraries.md) (этот процесс, в противном случае выполняется вручную). Цели Sharpie не обеспечивает привязку, сам по себе, но он может помочь приступить к работе!

Цели Sharpie 3.0 появилась возможность напрямую привязать Cocoapods!

## <a name="walkthrough---binding-an-ios-objective-c-librarywalkthroughmd"></a>[Пошаговое руководство. Привязка библиотек Objective-C iOS](walkthrough.md)

Эта страница содержит пошаговое руководство по созданию проекта привязки iOS с помощью с открытым исходным кодом [ **InfColorPicker** ](https://github.com/InfinitApps/InfColorPicker) проекта Objective-C в качестве примера. **InfColorPicker** библиотека предоставляет контроллер многократно используемых представления, которые позволяют пользователю выбирать цвета в его представление HSB, выбора цвета более понятные.
Цели Sharpie будет использоваться для помощи в процессе привязки.

## <a name="video"></a>Видео

> [!VIDEO https://youtube.com/embed/ZUoPLcmnf1o]

**iOS привязками в C /C++ видео**

## <a name="related-links"></a>Связанные ссылки

- [Привязка Objective-C](~/cross-platform/macios/binding/index.md)
- [Привязка Mac](~/mac/platform/binding.md)
