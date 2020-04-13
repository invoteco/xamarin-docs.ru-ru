---
title: Локальные базы данных Xamarin.Forms
description: Xamarin.Forms поддерживает приложения на основе базы данных с использованием ядра СУБД SQLite, которое позволяет загружать и сохранять объекты в общем коде. В этой статье описывается, как приложения Xamarin.Forms считывают данные из локальной базы данных SQLite и записывают данные в нее с помощью SQLite.Net.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 12/05/2019
ms.openlocfilehash: 2369afc693940d83971a43877da363e2c66b31b2
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/09/2020
ms.locfileid: "80992401"
---
# <a name="xamarinforms-local-databases"></a>Локальные базы данных Xamarin.Forms

[![Скачать](~/media/shared/download.png) образец Скачать образец](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)

Движок базы данных S'Lite позволяет приложениям Xamarin.Forms загружать и сохранять объекты данных в общем коде. Для хранения элементов todo в образец приложения используется таблица базы данных S'Lite. В этой статье описывается, как использовать SQLite.Net в общем коде для хранения и получения информации в локальной базе данных.

[![Скриншоты приложения Todolist на iOS и Android](databases-images/todo-list-sml.png)](databases-images/todo-list.png#lightbox "Тодолистое приложение на iOS и Android")

Интегрируйте SQLite.NET в мобильные приложения, следуя следующим шагам:

1. [Установите пакет NuGet](#install-the-sqlite-nuget-package).
1. [Настройка констант](#configure-app-constants).
1. [Создание класса доступа к базе данных.](#create-a-database-access-class)
1. [Доступ к данным в Xamarin.Forms](#access-data-in-xamarinforms).
1. [Расширенная конфигурация](#advanced-configuration).

## <a name="install-the-sqlite-nuget-package"></a>Установка пакета S'Lite NuGet

Используйте менеджер пакетов NuGet для поиска **sqlite-net-pcl** и добавьте последнюю версию в общий код овый проект.

Существует ряд пакетов NuGet с похожими названиями. Правильный пакет имеет следующие атрибуты:

- **Автор:** Фрэнк А. Крюгер (Frank A. Krueger)
- **ID:** sqlite-net-pcl
- **Ссылка NuGet:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> Пакет NuGet **sqlite-net-pcl** следует использовать даже в проектах .NET Standard.

## <a name="configure-app-constants"></a>Настройка констант приложения

Проект выборки включает в себя **Constants.cs** файл, который предоставляет общие данные конфигурации:

```csharp
public static class Constants
{
    public const string DatabaseFilename = "TodoSQLite.db3";

    public const SQLite.SQLiteOpenFlags Flags =
        // open the database in read/write mode
        SQLite.SQLiteOpenFlags.ReadWrite |
        // create the database if it doesn't exist
        SQLite.SQLiteOpenFlags.Create |
        // enable multi-threaded database access
        SQLite.SQLiteOpenFlags.SharedCache;

    public static string DatabasePath
    {
        get
        {
            var basePath = Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData);
            return Path.Combine(basePath, DatabaseFilename);
        }
    }
}
```

The constants file specifies default `SQLiteOpenFlag` enum values that are used to initialize the database connection. Enum поддерживает следующие `SQLiteOpenFlag` значения:

- `Create`: Соединение автоматически создаст файл базы данных, если он не существует.
- `FullMutex`: Соединение открывается в режиме сериализованного потока.
- `NoMutex`: Соединение открывается в многопоточном режиме.
- `PrivateCache`: Соединение не будет участвовать в общем кэше, даже если оно включено.
- `ReadWrite`: Соединение может читать и писать данные.
- `SharedCache`: Соединение будет участвовать в общем кэше, если оно включено.
- `ProtectionComplete`: Файл зашифрован и недоступен, пока устройство заблокировано.
- `ProtectionCompleteUnlessOpen`: Файл шифруется до тех пор, пока он не открыт, но затем доступен, даже если пользователь блокирует устройство.
- `ProtectionCompleteUntilFirstUserAuthentication`: Файл шифруется до тех пор, пока пользователь не загрузит и не разблокирует устройство.
- `ProtectionNone`: Файл базы данных не зашифрован.

Возможно, потребуется указать различные флаги в зависимости от того, как будет использоваться база данных. Для получения `SQLiteOpenFlags`дополнительной информации об открытии [новой базы данных подключение](https://www.sqlite.org/c3ref/open.html) на sqlite.org.

## <a name="create-a-database-access-class"></a>Создание класса доступа к базе данных

Класс обертки базы данных абстрагирует слой доступа к данным от остальной части приложения. Этот класс централизует логику запроса и упрощает управление инициализацией базы данных, облегчая рефакторинг или расширение операций с данными по мере роста приложения. Приложение Todo определяет `TodoItemDatabase` класс для этой цели.

### <a name="lazy-initialization"></a>Отложенная инициализация

Используется `TodoItemDatabase` класс .NET `Lazy` для задержки инициализации базы данных до тех пор, пока она не будет впервые доступна. Использование ленивой инициализации предотвращает процесс загрузки базы данных, что задерживает запуск приложения. Для получения дополнительной информации, см [Ленивый&lt;T&gt; класса](xref:System.Lazy`1).

```csharp
public class TodoItemDatabase
{
    static readonly Lazy<SQLiteAsyncConnection> lazyInitializer = new Lazy<SQLiteAsyncConnection>(() =>
    {
        return new SQLiteAsyncConnection(Constants.DatabasePath, Constants.Flags);
    });

    static SQLiteAsyncConnection Database => lazyInitializer.Value;
    static bool initialized = false;

    public TodoItemDatabase()
    {
        InitializeAsync().SafeFireAndForget(false);
    }

    async Task InitializeAsync()
    {
        if (!initialized)
        {
            if (!Database.TableMappings.Any(m => m.MappedType.Name == typeof(TodoItem).Name))
            {
                await Database.CreateTablesAsync(CreateFlags.None, typeof(TodoItem)).ConfigureAwait(false);
                initialized = true;
            }
        }
    }

    //...
}
```

Соединение базы данных является статическим полем, которое гарантирует, что для жизни приложения используется единое соединение базы данных. Использование постоянного статического соединения обеспечивает лучшую производительность, чем открытие и закрытие соединений несколько раз во время одного сеанса приложения.

Метод `InitializeAsync` отвечает за проверку того, существует ли `TodoItem` таблица для хранения объектов. Этот метод автоматически создает таблицу, если она не существует.

### <a name="the-safefireandforget-extension-method"></a>Метод расширения SafeFireиидЗайд

Когда `TodoItemDatabase` класс мгновенно, он должен инициализировать соединение базы данных, которое является асинхронным процессом. Но:

- Классные конструкторы не могут быть асинхронными.
- Метод асин, который не ожидается, не будет бросать исключения.
- `Wait` Метод блокирует поток _и_ проглатывает исключения.

Для того, чтобы начать асинхронную инициализацию, избежать блокирования исполнения и иметь `SafeFireAndForget`возможность поймать исключения, пример приложения использует метод расширения называется . Метод `SafeFireAndForget` расширения предоставляет дополнительную `Task` функциональность классу:

```csharp
public static class TaskExtensions
{
    // NOTE: Async void is intentional here. This provides a way
    // to call an async method from the constructor while
    // communicating intent to fire and forget, and allow
    // handling of exceptions
    public static async void SafeFireAndForget(this Task task,
        bool returnToCallingContext,
        Action<Exception> onException = null)
    {
        try
        {
            await task.ConfigureAwait(returnToCallingContext);
        }

        // if the provided action is not null, catch and
        // pass the thrown exception
        catch (Exception ex) when (onException != null)
        {
            onException(ex);
        }
    }
}
```

Метод `SafeFireAndForget` ожидает асинхронного выполнения предоставленного `Task` объекта и позволяет прикрепить, `Action` который называется, если брошено исключение.

Для получения дополнительной информации [см.](https://docs.microsoft.com/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)

### <a name="data-manipulation-methods"></a>Методы манипулирования данными

Класс `TodoItemDatabase` включает методы для четырех типов манипуляций с данными: создавать, читать, редактить и удалять. Библиотека SQLite.NET предоставляет простую адресную карту (ORM), которая позволяет хранить и извлекать объекты без написания инструкций по S'L.

```csharp
public class TodoItemDatabase {

    // ...

    public Task<List<TodoItem>> GetItemsAsync()
    {
        return Database.Table<TodoItem>().ToListAsync();
    }

    public Task<List<TodoItem>> GetItemsNotDoneAsync()
    {
        // SQL queries are also possible
        return Database.QueryAsync<TodoItem>("SELECT * FROM [TodoItem] WHERE [Done] = 0");
    }

    public Task<TodoItem> GetItemAsync(int id)
    {
        return Database.Table<TodoItem>().Where(i => i.ID == id).FirstOrDefaultAsync();
    }

    public Task<int> SaveItemAsync(TodoItem item)
    {
        if (item.ID != 0)
        {
            return Database.UpdateAsync(item);
        }
        else
        {
            return Database.InsertAsync(item);
        }
    }

    public Task<int> DeleteItemAsync(TodoItem item)
    {
        return Database.DeleteAsync(item);
    }
}
```

## <a name="access-data-in-xamarinforms"></a>Доступ к данным в Xamarin.Forms

Класс Xamarin.Forms `App` предоставляет экземпляр `TodoItemDatabase` класса:

```csharp
static TodoItemDatabase database;
public static TodoItemDatabase Database
{
    get
    {
        if (database == null)
        {
            database = new TodoItemDatabase();
        }
        return database;
    }
}
```

Это свойство позволяет компонентам Xamarin.Forms вызывать методы поиска данных и манипуляции в экземпляре `Database` в ответ на взаимодействие с пользователем. Пример:

```csharp
var saveButton = new Button { Text = "Save" };
saveButton.Clicked += async (sender, e) =>
{
    var todoItem = (TodoItem)BindingContext;
    await App.Database.SaveItemAsync(todoItem);
    await Navigation.PopAsync();
};
```

## <a name="advanced-configuration"></a>Расширенная конфигурация

S'Lite предоставляет надежный API с большим количеством функций, чем описано в этой статье и примере приложения. Следующие разделы охватывают функции, которые важны для масштабируемости.

Для получения более подробной информации о sqlite.org [см.](https://www.sqlite.org/docs.html)

### <a name="write-ahead-logging"></a>Запись-вперед и лесозаготовки

По умолчанию, S'Lite использует традиционный журнал отката. Копия неизмененного содержимого базы данных записывается в отдельный файл отката, затем изменения записываются непосредственно в файл базы данных. COMMIT возникает при удалении журнала отката.

Write-Ahead Logging (WAL) сначала записывает изменения в отдельный файл WAL. В режиме WAL COMMIT — это специальная запись, приложенная к файлу WAL, которая позволяет совершать несколько транзакций в одном файле WAL. Файл WAL сливается обратно в файл базы данных в рамках спецоперации под названием _checkpoint._

WAL может быть быстрее для локальных баз данных, потому что читатели и писатели не блокируют друг друга, позволяя выполнять операции одновременно горит и писать. Однако режим WAL не позволяет изменять _размер страницы,_ добавляет дополнительные файлообменные ассоциации в базу данных и добавляет дополнительную операцию _контрольной точки._

Чтобы включить WAL в `EnableWriteAheadLoggingAsync` SQLite.NET, `SQLiteAsyncConnection` позвоните в метод на экземпляре:

```csharp
await Database.EnableWriteAheadLoggingAsync();
```

Для получения более подробной информации, см [sqlite.org.](https://www.sqlite.org/wal.html)

### <a name="copying-a-database"></a>Копирование базы данных

Есть несколько случаев, когда может потребоваться скопировать базу данных S'Lite:

- База данных отправлена с вашим приложением, но должна быть скопирована или перемещена для записываемого хранилища на мобильном устройстве.
- Вам нужно сделать резервную копию или копию базы данных.
- Вам нужно отобрать, переместить или переименовать файл базы данных.

В целом перемещение, переименование или копирование файла базы данных является тем же процессом, что и любой другой тип файла с несколькими дополнительными соображениями:

- Все соединения базы данных должны быть закрыты перед попыткой перемещения файла базы данных.
- Если вы используете [запись вперед и журнал,](#write-ahead-logging)S'Lite создаст общий доступ к памяти (.shm) файл и (Запись Впереди журнал) (.wal) файл. Убедитесь, что вы применяете любые изменения в этих файлах, а также.

Для получения дополнительной информации, [см.](~/xamarin-forms/data-cloud/data/files.md)

## <a name="related-links"></a>Связанные ссылки

- [Применение образца Todo](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)
- [SQLite.NET пакет NuGet](https://www.nuget.org/packages/sqlite-net-pcl/)
- [Документация S'Lite](https://www.sqlite.org/docs.html)
- [Использование S'Lite с Android](~/android/data-cloud/data-access/using-sqlite-orm.md)
- [Использование S'Lite с iOS](~/ios/data-cloud/data/using-sqlite-orm.md)
- [Асинхронный шаблон на основе задач (TAP)](https://docs.microsoft.com/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)
- [Ленивый&lt;T&gt; класс](xref:System.Lazy`1)
