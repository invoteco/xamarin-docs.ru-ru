---
title: В какой версии Xamarin.Android добавлена поддержка Lollipop?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63B6E10C-098D-4C82-9253-07CA62EA85A5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 9e36189c771ed0c91a6030fd0ab615ab9af4dd52
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73026715"
---
# <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>В какой версии Xamarin.Android добавлена поддержка Lollipop?

> [!NOTE]
> Это руководством изначально было написано для предварительной версии Android L.

- В [Xamarin. Android 4,17](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_4/xamarin.android_4.17/index.md) добавлена поддержка Android L Preview.
- В [Xamarin. Android 4,20](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_4/xamarin.android_4.20/index.md) добавлена поддержка интерфейса поддержки Android.

Xamarin активно поддерживает текущий стабильный выпуск инструментов Xamarin. Приведенные ниже сведения предоставляются "как есть" для более старых версий инструментов. Последние сведения о выпусках Xamarin см. в [заметках о выпуске](https://docs.microsoft.com/xamarin/whats-new/#product-release-notes).

## <a name="missing-androidjar-for-api-level-21-in-android-l-preview"></a>Отсутствие Android. jar для API уровня 21 в предварительной версии Android L

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Может отобразиться следующее сообщение об ошибке (или похожее):

```cmd
Error 1 Could not find android.jar for API Level 21.
```

Это сообщение означает, что платформа пакет SDK для Android для уровня API 21 не установлена. Либо установите его в пакет SDK для Android Manager (**средства > открыть диспетчер пакет SDK для Android...** ), либо измените проект Xamarin. Android, чтобы он нацелились на установленную версию API.

Существует несколько обходных путей для этой проблемы:

1. Измените проект так, чтобы он нацелились на API 19 или ниже.

2. Переименуйте папку Android-21 с Android-21 на Android-L. (Лучше всего использовать его только как временное исправление, и оно может работать не слишком хорошо.)

   **% LOCALAPPDATA%\\Android\\Android-SDK\\платформы\\Android-21**

3. Временное понижение до уровня API Android 21 "L" Preview [1]:

    1. Удалите **% LocalAppData%\\android\\Android-sdk\\платформы\\Android-21** 
    2. Извлечение [1] в **C:\\пользователей\\&lt;имя_пользователя&gt;\\AppData\\Local\\android\\Android-sdk\\платформы** для создания папки **Android-L** .

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

Может отобразиться следующее сообщение об ошибке (или похожее):

```bash
/Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: 
Error: Could not find android.jar for API Level 21.**
```

Это означает, что платформа пакет SDK для Android для уровня API 21 не установлена. Установите его в пакет SDK для Android Manager (Tools > SDK Manager...) или измените проект Xamarin. Android, указав версию API, которая установлена.

Существует несколько обходных путей для этой проблемы:

1. Измените проект так, чтобы он нацелились на API 19 или ниже.

2. Переименуйте папку Android-21 с Android-21 на Android-L. (Лучше всего использовать его только как временное исправление, и оно может работать не слишком хорошо.)

   **~/Library/Developer/Xamarin/android-sdk-macosx/android-21**

3. Временное понижение до уровня API Android 21 "L" Preview [1]:

    1. Удалить **/Users/username/Library/Developer/Xamarin/Android-SDK-MacOSX/Android-21**
    2. Извлечение [1] в **/усерс/усернаме/либрари/девелопер/ксамарин/андроид-СДК-макоскс** для создания папки **Android-L** .

-----

[1] — [https://dl-ssl.google.com/android/repository/android-L_r04.zip](https://dl-ssl.google.com/android/repository/android-L_r04.zip)
