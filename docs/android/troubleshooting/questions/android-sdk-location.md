---
title: Где можно задать свои расположения пакета SDK для Android?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6A9DE6E9-3E27-4DD2-87D2-34E95E5D401C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 11/16/2017
ms.openlocfilehash: 8685be4bb1cc45ff04dc8d9f7d8e64e7b1483b60
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027029"
---
# <a name="where-can-i-set-my-android-sdk-locations"></a>Где можно задать свои расположения пакета SDK для Android?

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

В Visual Studio перейдите к **Средства > Параметры > Xamarin > Параметры Android**, чтобы просмотреть и задать расположение пакета SDK для Android.

[![Вкладка "Расположения" в разделе "Предпочтения"](android-sdk-location-images/win/01-locations-sml.png)](android-sdk-location-images/win/01-locations.png#lightbox)

Расположение по умолчанию для каждого пути выглядит следующим образом:

- Расположение пакета средств разработки Java: 

    **C:\\Program Files\\Java\\jdk1.8.0_131**

- Расположение пакета SDK для Android 

    **C:\\Program Files (x86)\\Android\\android-sdk**

- Расположение пакета Android NDK: 

    **C:\\ProgramData\\Microsoft\\AndroidNDK64\\android-ndk-r13b**

Обратите внимание, что номер версии NDK может отличаться. Например, вместо **android-ndk-r13b** может быть более ранняя версия, например **android-ndk-r10e**.

Чтобы задать расположение пакета SDK для Android, введите полный путь к каталогу пакета SDK для Android в поле **Расположение пакета SDK для Android**. Вы можете перейти к расположению пакета SDK для Android в проводнике, скопировать путь из адресной строки и вставить этот путь в поле **Расположение пакета SDK для Android**.
Например, если расположение пакета SDK для Android — это **C:\\Users\\username\\AppData\\Local\\Android\\Sdk**, очистите старый путь в поле **Расположение пакета SDK для Android**, вставьте этот путь и нажмите **ОК**.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

В Visual Studio для Mac перейдите к **Параметры > Проекты > Расположения пакетов SDK > Android**. На странице **Android** перейдите на вкладку **Расположения**, чтобы просмотреть и задать расположение пакета SDK:

[![Вкладка "Расположения" в разделе "Предпочтения"](android-sdk-location-images/mac/01-locations-sml.png)](android-sdk-location-images/mac/01-locations.png#lightbox)

Расположение по умолчанию для каждого пути выглядит следующим образом:

- Расположение пакета SDK для Android 

    **~/Library/Developer/Xamarin/android-sdk-macosx**

- Расположение пакета Android NDK: 

    **~/Library/Developer/Xamarin/android-ndk/android-ndk-r14b**

- Расположение пакета SDK для Java (JDK): 

    **/usr**

Обратите внимание, что номер версии NDK может отличаться. Например, вместо **android-ndk-r14b** может быть более ранняя версия, например **android-ndk-r10e**.

Чтобы задать расположение пакета SDK для Android, введите полный путь к каталогу пакета SDK для Android в поле **Расположение пакета SDK для Android**. Вы можете выбрать папку пакета SDK для Android в Finder, нажать **CTRL+&#8984;+I**, чтобы просмотреть сведения о папке, щелкнуть и перетащить путь справа от **Где:** , копировать, а затем вставить его в поле **Расположение пакета SDK для Android** на вкладке **Расположения**. Например, если расположение пакета SDK для Android находится в **~/Library/Developer/Android/Sdk**, очистите старый путь в поле **Расположение пакета SDK для Android**, вставьте этот путь и нажмите **ОК**.

-----
