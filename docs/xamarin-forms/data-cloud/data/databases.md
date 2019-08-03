---
title: Локальные базы данных Xamarin.Forms
description: Xamarin.Forms поддерживает приложения на основе базы данных с использованием ядра СУБД SQLite, которое позволяет загружать и сохранять объекты в общем коде. В этой статье описывается, как приложения Xamarin.Forms считывают данные из локальной базы данных SQLite и записывают данные в нее с помощью SQLite.Net.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: 9ea105b27aacef9ca9d63af0c57de880d039ff53
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739175"
---
# <a name="xamarinforms-local-databases"></a>Локальные базы данных Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)

_Xamarin.Forms поддерживает приложения на основе базы данных с использованием ядра СУБД SQLite, которое позволяет загружать и сохранять объекты в общем коде. В этой статье описывается, как приложения Xamarin.Forms считывают данные из локальной базы данных SQLite и записывают данные в нее с помощью SQLite.Net._

## <a name="overview"></a>Обзор

Приложения Xamarin.Forms могут использовать пакет [NuGet SQLite.NET PCL](https://www.nuget.org/packages/sqlite-net-pcl/) для включения операций с базой данных в общий код путем обращения к классам `SQLite`, которые входят в состав NuGet. Операции с базой данных можно определить в проекте библиотеки .NET Standard решения Xamarin.Forms.

В качестве [примера](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo) предлагается простое приложение для ведения списка задач. На следующих снимках экрана показано, как оно выглядит на каждой платформе.

[![Снимки экрана с примером работы с базой данных на платформе Xamarin.Forms](databases-images/todo-list-sml.png "Снимки экрана с первой страницей списка задач")](databases-images/todo-list.png#lightbox "Снимки экрана с первой страницей списка задач") [![Снимки экрана с примером работы с базой данных на платформе Xamarin.Forms](databases-images/todo-list-sml.png "Снимки экрана с первой страницей списка задач")](databases-images/todo-list.png#lightbox "Снимки экрана с первой страницей списка задач")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>Использование SQLite

Чтобы добавить поддержку SQLite в библиотеку .NET Standard для Xamarin.Forms, выполните поиск в NuGet по запросу **sqlite-net-pcl** и установите последнюю версию пакета:

![Добавление пакета NuGet SQLite.NET PCL](databases-images/vs2017-sqlite-pcl-nuget.png "Добавление пакета NuGet SQLite.NET PCL")

Есть несколько пакетов NuGet с похожими именами. Нужный пакет имеет следующие атрибуты:

- **Автор:** Фрэнк А. Крюгер (Frank A. Krueger)
- **Идентификатор:** sqlite-net-pcl
- **Ссылка в NuGet:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> Пакет NuGet **sqlite-net-pcl** следует использовать даже в проектах .NET Standard.

После добавления ссылки добавьте в класс `App` свойство, возвращающее путь к локальному файлу для хранения базы данных:

```csharp
static TodoItemDatabase database;

public static TodoItemDatabase Database
{
  get
  {
    if (database == null)
    {
      database = new TodoItemDatabase(
        Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "TodoSQLite.db3"));
    }
    return database;
  }
}
```

Конструктор `TodoItemDatabase`, принимающий путь к файлу базы данных в качестве аргумента, выглядит так:

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

Преимущество использования отдельной базы данных в том, что создается отдельное подключение к базе данных, которое остается открытым, пока работает приложение. Это позволяет избежать затрат, связанных с открытием и закрытием файла базы данных каждый раз, когда выполняется операция с ней.

Остальная часть класса `TodoItemDatabase` представляет собой кроссплатформенные запросы SQLite. Ниже представлен пример кода запроса (дополнительные сведения о синтаксисе см. в статье [Использование SQLite.NET с Xamarin.iOS](~/ios/data-cloud/data/using-sqlite-orm.md).

```csharp
public Task<List<TodoItem>> GetItemsAsync()
{
  return database.Table<TodoItem>().ToListAsync();
}

public Task<List<TodoItem>> GetItemsNotDoneAsync()
{
  return database.QueryAsync<TodoItem>("SELECT * FROM [TodoItem] WHERE [Done] = 0");
}

public Task<TodoItem> GetItemAsync(int id)
{
  return database.Table<TodoItem>().Where(i => i.ID == id).FirstOrDefaultAsync();
}

public Task<int> SaveItemAsync(TodoItem item)
{
  if (item.ID != 0)
  {
    return database.UpdateAsync(item);
  }
  else {
    return database.InsertAsync(item);
  }
}

public Task<int> DeleteItemAsync(TodoItem item)
{
  return database.DeleteAsync(item);
}
```

> [!NOTE]
> Преимущество использования асинхронного интерфейса API SQLite.Net в том, что операции с базой данных переносятся в фоновые потоки. Кроме того, нет необходимости писать дополнительный код для обеспечения параллелизма, так как интерфейс API отвечает за это.

## <a name="summary"></a>Сводка

Xamarin.Forms поддерживает приложения на основе базы данных с использованием ядра СУБД SQLite, которое позволяет загружать и сохранять объекты в общем коде.

В этой статье рассматривался **доступ** к базе данных SQLite с помощью Xamarin.Forms. Дополнительные сведения о работе с SQLite.Net см. в документации по [использованию SQLite.NET в Android](~/android/data-cloud/data-access/using-sqlite-orm.md) или [iOS](~/ios/data-cloud/data/using-sqlite-orm.md).

## <a name="related-links"></a>Связанные ссылки

- [Пример списка задач](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)
- [Примеры Xamarin.Forms](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
