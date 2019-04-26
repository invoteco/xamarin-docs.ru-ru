---
title: В какой версии Xamarin.Android добавлена поддержка Lollipop?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63B6E10C-098D-4C82-9253-07CA62EA85A5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 7e31f9ad46a04b648a6a1f24c075426f7d98a663
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61228268"
---
# <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>В какой версии Xamarin.Android добавлена поддержка Lollipop?

**Примечание.** В этом руководстве, первоначально был написан для предварительной версии Android L.

-   [Xamarin.Android 4.17](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.17/) добавлена поддержка Android L предварительной версии.
-   [Xamarin.Android 4.20](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.20/) добавлена поддержка Android Lollipop.

Xamarin поддерживает только активно текущий стабильный выпуск средства Xamarin. Информация ниже, предоставляется «как-является» для более старых версий средств. Последние сведения о выпусках Xamarin, проверьте [здесь](http://releases.xamarin.com/).

## <a name="missing-androidjar-for-api-level-21-in-android-l-preview"></a>«Отсутствуют android.jar для уровень API 21» в предварительной версии Android L

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Может появиться следующее сообщение об ошибке (или схожий):

```cmd
Error 1 Could not find android.jar for API Level 21.
```

Это сообщение означает, что платформа SDK для Android для API 21 уровень не установлен. Либо установите диспетчер пакетов SDK Android (**средства > открыть диспетчер Android SDK Manager...** ), или измените проект Xamarin.Android для установленной версии API.

Существует несколько обходных путей для этой проблемы:

1. Изменить проект, чтобы он предназначен для API 19 или более ранней.

2. Переименуйте папку android-21 от android 21 для android-L. (В лучшем случае это должно использоваться только как временное решение, и он может не работать очень хорошо.)

   **% LOCALAPPDATA %\\Android\\android-sdk\\платформ\\android-21**

3. Временно понизить обратно к предварительной версии Android API 21 уровень "L" [1]:

    1.  Удалить **% LOCALAPPDATA %\\Android\\android-sdk\\платформ\\android-21** 
    2.  Извлечения [1] на **C:\\пользователей\\&lt;username&gt;\\AppData\\локального\\Android\\android-sdk\\платформ**  для создания **android-L** папки.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

Может появиться следующее сообщение об ошибке (или схожий):

```bash
/Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: 
Error: Could not find android.jar for API Level 21.**
```

Это означает, что платформа SDK для Android для API 21 уровень не установлен. Либо установите диспетчер пакетов SDK для Android (Сервис > Диспетчер пакетов SDK...), или измените проект Xamarin.Android для установленной версии API.

Существует несколько обходных путей для этой проблемы:

1. Изменить проект, чтобы он предназначен для API 19 или более ранней.

2. Переименуйте папку android-21 от android 21 для android-L. (В лучшем случае это должно использоваться только как временное решение, и он может не работать очень хорошо.)

   **~/Library/Developer/Xamarin/Android-SDK-MacOSX/Android-21**

3. Временно понизить обратно к предварительной версии Android API 21 уровень "L" [1]:

    1.  Удалить **/Users/username/Library/Developer/Xamarin/android-sdk-macosx/android-21**
    2.  Извлечения [1] на **/Users/username/Library/Developer/Xamarin/android-sdk-macosx** для создания **android-L** папки.

-----


[1] - [https://dl-ssl.google.com/android/repository/android-L_r04.zip](https://dl-ssl.google.com/android/repository/android-L_r04.zip)
