---
title: Настройка SQLite в Xamarin. iOS
description: В этом документе описывается, как определить расположение файла базы данных SQLite в приложении Xamarin. iOS. Эти понятия важны независимо от выбранного механизма доступа к данным.
ms.prod: xamarin
ms.assetid: E5582F4B-AD74-420F-9E6D-B07CFB420B3A
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/11/2016
ms.openlocfilehash: 9140b98511aae5e24ee8aaf069668a793221c1a7
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73009053"
---
# <a name="configuring-sqlite-in-xamarinios"></a>Настройка SQLite в Xamarin. iOS

Чтобы использовать SQLite в приложении Xamarin. iOS, необходимо определить правильное расположение файла базы данных.

## <a name="database-file-path"></a>Путь к файлу базы данных

Независимо от того, какой метод доступа к данным используется, необходимо создать файл базы данных, прежде чем данные можно будет сохранить с помощью SQLite. В зависимости от платформы, для которой предназначено расположение файла, будет отличаться. Для iOS можно использовать класс среды для создания допустимого пути, как показано в следующем фрагменте кода:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

При выборе места хранения файла базы данных необходимо учитывать и другие моменты. В iOS может потребоваться автоматическое резервное копирование базы данных (или нет).

Если вы хотите использовать другое расположение на каждой платформе в кросс-платформенном приложении, можно использовать директиву компилятора, как показано ниже, чтобы создать другой путь для каждой платформы:

```csharp
var sqliteFilename = "MyDatabase.db3";
#if __ANDROID__
// Just use whatever directory SpecialFolder.Personal returns
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
// we need to put in /Library/ on iOS5.1+ to meet Apple's iCloud terms
// (they don't want non-user-generated data in Documents)
string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine (documentsPath, "..", "Library"); // Library folder instead
#endif
var path = Path.Combine (libraryPath, sqliteFilename);
```

Дополнительные сведения о том, какие расположения файлов следует использовать в iOS, см. в статье [Работа с файловой системой](~/ios/app-fundamentals/file-system.md) . Дополнительные сведения об использовании директив компилятора для написания кода, характерного для каждой платформы, см. в документе [Создание межплатформенных приложений](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) .

## <a name="threading"></a>Потоки

Не следует использовать одно и то же подключение к базе данных SQLite в нескольких потоках. Будьте внимательны при открытии, используйте и закройте все соединения, созданные в том же потоке.

Чтобы гарантировать, что код не пытается получить доступ к базе данных SQLite из нескольких потоков одновременно, блокировка будет выполнена при каждом обращении к базе данных следующим образом:

```csharp
object locker = new object(); // class level private field
// rest of class code
lock (locker){
    // Do your query or insert here
}
```

Все доступ к базе данных (операции чтения, записи, обновления и т. д.) должны быть заключены в одну и ту же блокировку. Необходимо соблюдать осторожность, чтобы избежать ситуации взаимоблокировки, гарантируя, что работа внутри предложения блокировки сохраняется просто и не вызывает другие методы, которые также могут снять блокировку.

## <a name="related-links"></a>Связанные ссылки

- [Базовый доступ к данным (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Расширенный доступ к данным (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Рецепты на данные iOS](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Доступ к данным Xamarin. Forms](~/xamarin-forms/data-cloud/data/databases.md)
