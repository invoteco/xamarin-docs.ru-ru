---
title: Xamarin. Требования к формам
description: Требования к платформе и системе разработки для Xamarin. Формах.
ms.prod: xamarin
ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/16/2019
no-loc:
- Xamarin
- Xamarin.Forms
- Xamarin.Android
- Xamarin.Essentials
- Xamarin.iOS
- Xamarin.Mac
ms.openlocfilehash: d12daa358917399fc5fd1febf02d4f96a647f360
ms.sourcegitcommit: 6f09bc2b760e76a61a854f55d6a87c4f421ac6c8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/02/2020
ms.locfileid: "75607858"
---
# <a name="opno-locxamarinforms-requirements"></a>Xamarin. Требования к формам

_Требования к платформе и системе разработки для Xamarin. Формах._

Обзор рекомендаций по установке и настройке, действующих для различных платформ, см. в статье [Установка](installation/index.md).

## <a name="target-platforms"></a>Целевые платформы

Xamarin. Приложения форм могут быть написаны для следующих операционных систем:

- iOS 9 или более поздней версии
- Android 4.4 (API 19) или более поздние версии ([подробнее](#android));
- универсальная платформа Windows для Windows 10 ([подробнее](#windows10));

Однако в качестве минимального API рекомендуется использовать Android 5,0 (API 21). Это гарантирует полную совместимость со всеми библиотеками поддержки Android, при этом нацеливание на большинство устройств Android.

Предполагается, что разработчики знакомы с [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md).

### <a name="additional-platform-support"></a>Поддержка дополнительных платформ

Состояние этих платформ можно найти на [Xamarin. Формы GitHub](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support):

- Samsung Tizen;
- macOS
- GTK#
- WPF

### <a name="android"></a>Android

Необходимо установить последнюю версию Android SDK Tools и платформы API Android. Для обновления до последних версий можно воспользоваться [диспетчером пакетов SDK Android](~/android/get-started/installation/android-sdk.md).

Кроме того, в целевой версии или версии компиляции для проектов Android **должен** быть выбран параметр *Использовать самую новую установленную платформу*. Однако минимальной версией может быть API 19, что позволяет поддерживать устройства, использующие Android 4.4 и более поздние версии. Эти значения задаются в разделе **Параметры проекта**.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

**Параметры проекта > Приложение > Свойства приложения**

![Параметры сборки Android в Visual Studio](requirements-images/options-android-vs-sml.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

**Сборка > Общие**

![Выберите последнюю целевую платформу](requirements-images/options-general-sml.png)

**Сборка > Приложение Android**

![Выберите минимальные и целевые версии Android для приложения](requirements-images/options-android-sml.png)

-----

## <a name="development-system-requirements"></a>Требования к системе для разработки

Xamarin. Приложения форм можно разрабатывать на macOS и Windows. Однако для создания Windows-версий приложения требуются Windows и Visual Studio.

## <a name="mac-system-requirements"></a>Требования к системе Mac

Для разработки Xamarinможно использовать Visual Studio для Mac. Приложения форм на macOS High Сьерра (10,13) или более поздней версии. Для разработки приложений iOS рекомендуется использовать последнюю версию Xcode, iOS и macOS. Требования к конкретной версии см. в [заметках о выпускеXamarin. iOS](/xamarin/ios/release-notes/).

> [!NOTE]
> Приложения Windows нельзя разрабатывать в macOS.

## <a name="windows-system-requirements"></a>Требования к системе Windows

Xamarin. Приложения форм для iOS и Android могут быть построены в любой установке Windows, поддерживающей Xamarinную разработку. Для полной поддержки функций текущей платформы используйте последнюю версию Visual Studio. 

Для разработки iOS требуется сетевой компьютер Mac, использующий последнюю версию Xcode и минимальную версию macOS, указанную Apple.

<a name="windows10" />

### <a name="universal-windows-platform-uwp"></a>Универсальная платформа Windows (UWP)

Разработка Xamarin. Для приложений форм для UWP требуется:

- Windows 10 (рекомендуется самая последняя версия, не менее создателей обновлений)

- Visual Studio 2019 рекомендуется

- [Пакет средств разработки Windows 10](https://dev.windows.com/downloads/windows-10-sdk)

[Приложение универсальная платформа Windows (UWP) можно добавить](~/xamarin-forms/platform/windows/installation/index.md) в существующую Xamarin. Решение "формы" в любое время.

## <a name="deprecated-platforms"></a>Устаревшие платформы

Эти платформы не поддерживаются при использовании Xamarin. Формы 3,0 или более поздней версии:

- *Windows 8.1 или Windows Phone 8.1 WinRT;*
- *Windows Phone 8 Silverlight.*
