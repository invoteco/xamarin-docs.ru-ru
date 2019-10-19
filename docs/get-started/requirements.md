---
title: Требования к Xamarin.Forms
description: Требования к платформе и системные требования к разработке Xamarin.Forms.
ms.prod: xamarin
ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/16/2019
ms.openlocfilehash: 46a72534fba7a45323a82ad121e5844410472812
ms.sourcegitcommit: dad4dfcd194b63ec9e903363351b6d9e543d4888
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/18/2019
ms.locfileid: "72584350"
---
# <a name="xamarinforms-requirements"></a>Требования к Xamarin.Forms

_Требования к платформе и системные требования к разработке Xamarin.Forms._

Обзор рекомендаций по установке и настройке, действующих для различных платформ, см. в статье [Установка](installation/index.md).

## <a name="target-platforms"></a>Целевые платформы

Приложения Xamarin.Forms могут быть написаны для следующих операционных систем:

- iOS 9 или более поздней версии
- Android 4.4 (API 19) или более поздние версии ([подробнее](#android));
- универсальная платформа Windows для Windows 10 ([подробнее](#windows10));

Однако в качестве минимального API рекомендуется использовать Android 5,0 (API 21). Это гарантирует полную совместимость со всеми библиотеками поддержки Android, при этом нацеливание на большинство устройств Android.

Предполагается, что разработчики знакомы с [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md).

### <a name="additional-platform-support"></a>Поддержка дополнительных платформ

Сведения о состоянии этих платформ можно найти на странице [Xamarin.Forms в GitHub](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support):

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

Приложения Xamarin.Forms можно разрабатывать в ОС в macOS и Windows. Однако для создания Windows-версий приложения требуются Windows и Visual Studio.

## <a name="mac-system-requirements"></a>Требования к системе Mac

Visual Studio для Mac можно использовать для разработки приложений Xamarin. Forms в macOS High Сьерра (10,13) или более поздней версии. Для разработки приложений iOS рекомендуется использовать последнюю версию Xcode, iOS и macOS. Требования к конкретной версии см. в последних [заметках о выпуске Xamarin. iOS](/xamarin/ios/release-notes/).

> [!NOTE]
> Приложения Windows нельзя разрабатывать в macOS.

## <a name="windows-system-requirements"></a>Требования к системе Windows

Приложения Xamarin.Forms для iOS и Android можно создавать в любой установке Windows, которая поддерживает возможности разработки Xamarin. Для полной поддержки функций текущей платформы используйте последнюю версию Visual Studio. 

Для разработки iOS требуется сетевой компьютер Mac, использующий последнюю версию Xcode и минимальную версию macOS, указанную Apple.

<a name="windows10" />

### <a name="universal-windows-platform-uwp"></a>Универсальная платформа Windows (UWP)

Для разработки приложений Xamarin.Forms для UWP требуются следующие компоненты:

- Windows 10 (рекомендуется самая последняя версия, не менее создателей обновлений)

- Visual Studio 2019 рекомендуется

- [Пакет средств разработки Windows 10](https://dev.windows.com/downloads/windows-10-sdk)

[Приложение универсальной платформы Windows (UWP) можно добавить](~/xamarin-forms/platform/windows/installation/index.md) в существующее решение Xamarin.Forms в любое время.

## <a name="deprecated-platforms"></a>Устаревшие платформы

Эти платформы не поддерживаются при использовании Xamarin. Forms 3,0 или более поздней версии:

- *Windows 8.1 или Windows Phone 8.1 WinRT;*
- *Windows Phone 8 Silverlight.*
