---
title: 'Xamarin.Essentials: Вспомогательные методы для файловой системы'
description: Класс FileSystem в Xamarin.Essentials содержит ряд вспомогательных функций для поиска кэша приложения, каталогов данных и открытых файлов в пакете приложения.
ms.assetid: B3EC2DE0-EFC0-410C-AF71-7410AE84CF84
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 5b155e4976a67bda36e66d2ca3565c9237fde3c6
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "68738867"
---
# <a name="xamarinessentials-file-system-helpers"></a>Xamarin.Essentials: Вспомогательные методы для файловой системы

Класс **FileSystem** содержит ряд вспомогательных функций для поиска кэша приложения, каталогов данных и открытых файлов в пакете приложения.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-file-system-helpers"></a>Использование вспомогательных функций FileSystem

Добавьте в свой класс ссылку на Xamarin.Essentials:

```csharp
using Xamarin.Essentials;
```

Чтобы получить каталог приложения для хранения **кэшированных данных**. Данные кэша можно использовать для любых данных, которые должны сохраняться дольше, чем временные, но не требуются для правильной работы, так как ОС определяет, когда это хранилище очищается.

```csharp
var cacheDir = FileSystem.CacheDirectory;
```

Чтобы получить каталог верхнего уровня приложения для любых файлов, которые не являются файлами пользовательских данных. Эти файлы архивируются на платформе синхронизации операционной системы. Особенности реализации для платформ см. ниже.

```csharp
var mainDir = FileSystem.AppDataDirectory;
```

Чтобы открыть файл, который входит в пакет приложения, используйте следующий код:

```csharp
 using (var stream = await FileSystem.OpenAppPackageFileAsync(templateFileName))
 {
    using (var reader = new StreamReader(stream))
    {
        var fileContents = await reader.ReadToEndAsync();
    }
 }
```

## <a name="platform-implementation-specifics"></a>Особенности реализации для платформ

# <a name="android"></a>[Android](#tab/android)

- **CacheDirectory** — возвращает [CacheDir](https://developer.android.com/reference/android/content/Context.html#getCacheDir) текущего контекста.
- **AppDataDirectory** — возвращает [FilesDir](https://developer.android.com/reference/android/content/Context.html#getFilesDir) текущего контекста и выполняет резервное копирование с использованием [автоматической архивации](https://developer.android.com/guide/topics/data/autobackup.html), начиная с API версии 23 и более поздней.

Добавьте любой файл в папку **Assets** проекта Android и пометьте действие сборки как **AndroidAsset**, чтобы использовать его с `OpenAppPackageFileAsync`.

# <a name="ios"></a>[iOS](#tab/ios)

- **CacheDirectory** — возвращает каталог [Library/Caches](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html).
- **AppDataDirectory** — возвращает каталог [Library](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html), чья резервная копия создана в iTunes и iCloud.

Добавьте любой файл в папку **Resources** проекта iOS и пометьте действие сборки как **BundledResource**, чтобы использовать его с `OpenAppPackageFileAsync`.

# <a name="uwp"></a>[UWP](#tab/uwp)

- **CacheDirectory** — возвращает каталог [LocalCacheFolder](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localcachefolder#Windows_Storage_ApplicationData_LocalCacheFolder).
- **AppDataDirectory** — возвращает каталог [LocalFolder](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder#Windows_Storage_ApplicationData_LocalFolder), чья резервная копия создана в облаке.

Добавьте любой файл в корневой каталог в проекте UWP и пометьте действие сборки как **Content** для использования с `OpenAppPackageFileAsync`.

--------------

## <a name="api"></a>API

- [Исходный код вспомогательных функций класса FileSystem](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/FileSystem)
- [Документация по API файловой системы](xref:Xamarin.Essentials.FileSystem)
