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
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027029"
---
# <a name="where-can-i-set-my-android-sdk-locations"></a>Где можно задать свои расположения пакета SDK для Android?

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

В Visual Studio перейдите к **меню сервис > параметры > параметры Xamarin > Android** , чтобы просмотреть и задать расположение пакет SDK для Android.

[Вкладка «примеры расположения![» в разделе «настройки»](android-sdk-location-images/win/01-locations-sml.png)](android-sdk-location-images/win/01-locations.png#lightbox)

Расположение по умолчанию для каждого пути выглядит следующим образом:

- Расположение пакета средств разработки Java: 

    **В.\\программные файлы\\Java\\JDK 1.8.0 _131**

- Расположение пакет SDK для Android: 

    **C:\\Program Files (x86)\\Android\\android-sdk**

- Расположение Android NDK: 

    **В.\\папка ProgramData\\Microsoft\\AndroidNDK64\\Android-NDK-r13b**

Обратите внимание, что номер версии NDK может отличаться. Например, вместо **Android-NDK-r13b**это может быть более ранняя версия, например **Android-NDK-r10e**.

Чтобы задать расположение пакет SDK для Android, введите полный путь к каталогу пакет SDK для Android в поле **пакет SDK для Android Location (расположение** ). Можно переходить к расположению пакет SDK для Android в проводнике, скопировать путь из адресной строки и вставить этот путь в поле **расположение пакет SDK для Android** .
Например, если пакет SDK для Android расположен в папке **C:\\пользователи\\имя_пользователя\\AppData\\Local\\Android\\SDK**, очистите старый путь в поле **Расположение пакет SDK для Android** , вставьте его по указанному пути. и нажмите кнопку **ОК**.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

В Visual Studio для Mac перейдите к пункту **настройки > проекты > расположения пакетов SDK > Android**. На странице **Android** перейдите на вкладку **расположения** , чтобы просмотреть и задать расположение пакета SDK:

[Вкладка «примеры расположения![» в разделе «настройки»](android-sdk-location-images/mac/01-locations-sml.png)](android-sdk-location-images/mac/01-locations.png#lightbox)

Расположение по умолчанию для каждого пути выглядит следующим образом:

- Расположение пакет SDK для Android: 

    **~/Либрари/девелопер/ксамарин/андроид-СДК-макоскс**

- Расположение Android NDK: 

    **~/Library/Developer/Xamarin/android-ndk/android-ndk-r14b**

- Расположение пакета SDK для Java (JDK): 

    **/usr**

Обратите внимание, что номер версии NDK может отличаться. Например, вместо **Android-NDK-r14b**это может быть более ранняя версия, например **Android-NDK-r10e**.

Чтобы задать расположение пакет SDK для Android, введите полный путь к каталогу пакет SDK для Android в поле **пакет SDK для Android Location (расположение** ). Можно выбрать папку пакет SDK для Android в Finder, нажать **сочетание клавиш&#8984;Ctrl + + I** , чтобы просмотреть сведения о папке, щелкнуть и перетащить путь справа от раздела **:** , скопировать, а затем вставить его в поле **Расположение пакет SDK для Android** на вкладке **расположения** . Например, если расположение пакет SDK для Android находится в папке **~/либрари/девелопер/андроид/СДК**, очистите старый путь в поле **Расположение пакет SDK для Android** , вставьте этот путь и нажмите кнопку **ОК**.

-----
