---
title: Экран-заставка Xamarin. Forms
description: В этой статье объясняется, как создать экран-заставку для приложения Xamarin. Forms.
ms.prod: xamarin
ms.assetId: 338C8F60-90F2-4B3D-BB47-7F846AE8DC6C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 10/1/2019
ms.openlocfilehash: 2624c3f35a9cfac122a0b36ea8c1684d30f57824
ms.sourcegitcommit: 5110d1279809a2af58d3d66cd14c78113bb51436
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/08/2019
ms.locfileid: "72035776"
---
# <a name="xamarinforms-splash-screen"></a>Экран-заставка Xamarin. Forms

Приложения часто имеют задержку запуска, пока приложение завершает процесс инициализации. Разработчики могут предложить фирменный опыт, который, как правило, называется экраном-заставкой, когда приложение запускается. В этой статье объясняется, как создать экраны-заставки для приложений Xamarin. Forms.

Xamarin. Forms инициализируется на каждой платформе после завершения собственной последовательности запуска. Xamarin. Forms инициализированы:

- В методе `OnCreate` класса `MainActivity` в Android.
- В методе `FinishedLaunching` класса `AppDelegate` в iOS.
- В методе `OnLaunched` класса `App` в UWP.

Экран-заставка должен отображаться как можно скорее при запуске приложения, но Xamarin. Forms не инициализируются до конца последовательности запуска, что означает, что экран-заставка должен быть реализован за пределами Xamarin. Forms на каждой платформе. В следующих разделах объясняется, как создать экран-заставку на каждой платформе.

## <a name="xamarinforms-android-splash-screen"></a>Экран-заставка Xamarin. Forms Android

Создание экрана-заставки в Android требует создания заставки `Activity` как `MainLauncher` с особой темой. Как только заставка запустится `Activity`, она запускает основной `Activity` с нормальной темой приложения.

Дополнительные сведения о экранах-заставках на Xamarin. Android см. на [экране-заставке Xamarin. Android](~/android/user-interface/splash-screen.md).

## <a name="xamarinforms-ios-splash-screen"></a>Экран-заставка для iOS в Xamarin. Forms

Экран-заставка на iOS называется экраном запуска. Для создания экрана запуска в iOS необходимо создать раскадровку, определяющую пользовательский интерфейс экрана запуска, а затем задать раскадровку в качестве экрана запуска в файле **info. plist**.

Дополнительные сведения о экранах запуска Xamarin. iOS см. в разделе [экран запуска Xamarin. iOS](~/ios/app-fundamentals/images-icons/launch-screens.md).

## <a name="xamarinforms-uwp-splash-screen"></a>Заставка UWP для Xamarin. Forms

В UWP **пакет. appxmanifest** содержит вкладку " **визуальные ресурсы** " с подменю " **экран-заставка** ". В этом меню можно указать изображение экрана-заставки:

[экран-заставка ![параметра в UWP](splashscreen-images/uwp-splashscreen-cropped.png)](splashscreen-images/uwp-splashscreen.png#lightbox)

## <a name="related-links"></a>Связанные ссылки

- [Экран-заставка Xamarin. Android](~/android/user-interface/splash-screen.md)
- [Экран запуска Xamarin. iOS](~/ios/app-fundamentals/images-icons/launch-screens.md)
