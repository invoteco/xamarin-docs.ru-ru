---
title: Локальные базы данных Xamarin.Forms
description: Xamarin.Forms поддерживает приложения на основе базы данных с использованием ядра СУБД SQLite, которое позволяет загружать и сохранять объекты в общем коде. В этой статье описывается, как приложения Xamarin.Forms считывают данные из локальной базы данных SQLite и записывают данные в нее с помощью SQLite.Net.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 12/05/2019
ms.openlocfilehash: 190aeb83456fa7c7ba8a9415b02ab56f3f8779da
ms.sourcegitcommit: 4691b48f14b166afcec69d1350b769ff5bf8c9f6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2020
ms.locfileid: "75728282"
---
# <a name="xamarinforms-local-databases"></a>Локальные базы данных Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)

Обработчик базы данных SQLite позволяет приложениям Xamarin. Forms загружать и сохранять объекты данных в общем коде. Пример приложения использует таблицу базы данных SQLite для хранения элементов Todo. В этой статье описывается, как использовать SQLite.Net в общем коде для хранения и извлечения информации в локальной базе данных.

[![снимков экрана приложения ToDoList в iOS и Android](databases-images/todo-list-sml.png)](databases-images/todo-list.png#lightbox "Приложение ToDoList в iOS и Android")

Интегрируйте SQLite.NET в мобильные приложения, выполнив следующие действия.

1. [Установите пакет NuGet](#install-the-sqlite-nuget-package).
1. [Настройка констант](#configure-app-constants).
1. [Создание класса доступа к базе данных](#create-a-database-access-class).
1. [Доступ к данным в Xamarin. Forms](#access-data-in-xamarinforms).
1. [Расширенная конфигурация](#advanced-configuration).

## <a name="install-the-sqlite-nuget-package"></a>Установка пакета NuGet для SQLite

Используйте диспетчер пакетов NuGet для поиска **SQLite-NET-PCL** и добавления последней версии в проект общего кода.

Существует ряд пакетов NuGet с похожими названиями. Правильный пакет имеет следующие атрибуты:

- **Автор:** Фрэнк А. Крюгер (Frank A. Krueger)
- **ID:** SQLite-NET-PCL
- **Ссылка NuGet:** [SQLite-NET-PCL](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> Пакет NuGet **sqlite-net-pcl** следует использовать даже в проектах .NET Standard.

## <a name="configure-app-constants"></a>Настройка констант приложения

Пример проекта включает файл **Constants.CS** , который предоставляет общие данные конфигурации:

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

В файле констант указано значение по умолчанию `SQLiteOpenFlag` перечислимые значения, используемые для инициализации подключения к базе данных. Перечисление `SQLiteOpenFlag` поддерживает следующие значения:

- `Create`: подключение автоматически создаст файл базы данных, если он не существует.
- `FullMutex`: соединение открыто в режиме сериализованного потока.
- `NoMutex`: соединение открывается в режиме многопоточной обработки.
- `PrivateCache`: соединение не будет участвовать в общем кэше, даже если оно включено.
- `ReadWrite`: подключение может считывать и записывать данные.
- `SharedCache`: подключение будет участвовать в общем кэше, если он включен.
- `ProtectionComplete`: файл зашифрован и недоступен, пока устройство заблокировано.
- `ProtectionCompleteUnlessOpen`: файл шифруется до его открытия, но затем становится доступным, даже если пользователь блокирует устройство.
- `ProtectionCompleteUntilFirstUserAuthentication`: файл шифруется до тех пор, пока пользователь не загрузится и не разблокирует устройство.
- `ProtectionNone`: файл базы данных не зашифрован.

Может потребоваться указать различные флаги в зависимости от того, как будет использоваться база данных. Дополнительные сведения о `SQLiteOpenFlags`см. в разделе [Открытие нового подключения к базе данных](https://www.sqlite.org/c3ref/open.html) в SQLite.org.

## <a name="create-a-database-access-class"></a>Создание класса доступа к базе данных

Класс-оболочка базы данных абстрагирует уровень доступа к данным от остальной части приложения. Этот класс централизует логику запросов и упрощает управление инициализацией базы данных, что упрощает рефакторинг или расширение операций с данными по мере роста приложения. Приложение Todo определяет для этой цели класс `TodoItemDatabase`.

### <a name="lazy-initialization"></a>Отложенная инициализация

`TodoItemDatabase` использует класс .NET `Lazy` для задержки инициализации базы данных до первого обращения к ней. Использование отложенной инициализации предотвращает задержку запуска приложения в процессе загрузки базы данных. Дополнительные сведения см. в разделе [Lazy&lt;t&gt; Class](https://docs.microsoft.com/dotnet/api/system.lazy-1).

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

Соединение с базой данных — это статическое поле, которое обеспечивает использование одного подключения к базе данных в течение всего жизненного цикла приложения. Постоянное статическое подключение обеспечивает лучшую производительность, чем несколько раз открывать и закрывать соединения в течение одного сеанса приложения.

Метод `InitializeAsync` отвечает за проверку наличия таблицы для хранения объектов `TodoItem`. Этот метод автоматически создает таблицу, если она не существует.

### <a name="the-safefireandforget-extension-method"></a>Метод расширения Сафефиреандфоржет

При создании экземпляра класса `TodoItemDatabase` необходимо инициализировать подключение к базе данных, которое является асинхронным процессом. Однако:

- Конструкторы классов не могут быть асинхронными.
- Асинхронный метод, который не ожидается, не будет вызывать исключения.
- Использование метода `Wait` блокирует потоки _и_ исключения поглощает.

Чтобы запустить асинхронную инициализацию, не блокируя выполнение и иметь возможность перехватывать исключения, в примере приложения используется метод расширения, именуемый `SafeFireAndForget`. Метод расширения `SafeFireAndForget` предоставляет дополнительные функциональные возможности для класса `Task`.

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

Метод `SafeFireAndForget` ожидает асинхронное выполнение предоставленного объекта `Task` и позволяет присоединить `Action`, который вызывается при возникновении исключения.

Дополнительные сведения см. в разделе [асинхронная модель на основе задач (TAP)](https://docs.microsoft.com/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap).

### <a name="data-manipulation-methods"></a>Методы обработки данных

Класс `TodoItemDatabase` содержит методы для обработки данных четырех типов: создание, чтение, изменение и удаление. Библиотека SQLite.NET предоставляет простую объектную реляционную карту (ORM), позволяющую хранить и извлекать объекты без написания инструкций SQL.

```csharp
public static class TodoItemDatabase {

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

## <a name="access-data-in-xamarinforms"></a>Доступ к данным в Xamarin. Forms

Класс `App` Xamarin. Forms предоставляет экземпляр класса `TodoItemDatabase`:

```csharp
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

Это свойство позволяет компонентам Xamarin. Forms вызывать методы получения и обработки данных в экземпляре `Database` в ответ на взаимодействие с пользователем. Например:

```csharp
var saveButton = new Button { Text = "Save" };
saveButton.Clicked += async (sender, e) =>
{
    var todoItem = (TodoItem)BindingContext;
    await App.Database.SaveItemAsync(todoItem);
    await Navigation.PopAsync();
};
```

## <a name="advanced-configuration"></a>Расширенная настройка

SQLite предоставляет надежный API с дополнительными функциями, чем описано в этой статье и примере приложения. В следующих разделах описываются функции, важные для масштабируемости.

Дополнительные сведения см. в [документации по SQLite](https://www.sqlite.org/docs.html) в SQLite.org.

### <a name="write-ahead-logging"></a>Ведение журнала с упреждающей записью

По умолчанию SQLite использует традиционный журнал отката. Копия неизмененного содержимого базы данных записывается в отдельный файл отката, после чего изменения записываются непосредственно в файл базы данных. ФИКСАЦИя происходит при удалении журнала отката.

Ведение журнала с упреждающей записью (WAL) сначала записывает изменения в отдельный файл WAL. В режиме WAL ФИКСАЦИя — это специальная запись, которая добавляется в файл WAL, что позволяет нескольким транзакциям выполняться в одном файле WAL. Файл WAL объединяется в файл базы данных в специальной операции, называемой _контрольной точкой_.

WAL может быть быстрее для локальных баз данных, так как модули чтения и записи не блокируют друг друга, что позволяет выполнять операции чтения и записи одновременно. Однако режим WAL не допускает изменения _размера страницы_, добавляет к базе данных дополнительные ассоциации файлов и добавляет операцию дополнительной _контрольной точки_ .

Чтобы включить WAL в SQLite.NET, вызовите метод `EnableWriteAheadLoggingAsync` в экземпляре `SQLiteAsyncConnection`:

```csharp
await Database.EnableWriteAheadLoggingAsync();
```

Дополнительные сведения см. в разделе [SQLite Write-упреждающее ведение журнала](https://www.sqlite.org/wal.html) на SQLite.org.

### <a name="copying-a-database"></a>Копирование базы данных

Существует несколько случаев, когда может потребоваться скопировать базу данных SQLite:

- База данных поставляется с приложением, но ее необходимо скопировать или переместить в хранилище, доступное для записи на мобильном устройстве.
- Необходимо создать резервную копию или копию базы данных.
- Необходимо выполнить версию, переместить или переименовать файл базы данных.

В общем случае перемещение, переименование или копирование файла базы данных выполняется так же, как и для любого другого типа файлов, с помощью нескольких дополнительных соображений.

- Прежде чем пытаться переместить файл базы данных, необходимо закрыть все подключения к базе данных.
- Если вы используете [ведение журнала с упреждающей записью](#write-ahead-logging), SQLite создаст файл с доступом к общей памяти (. SHM) и файл (с расширением WAL). Убедитесь, что вы также применяете все изменения к этим файлам.

Дополнительные сведения см. [в разделе Обработка файлов в Xamarin. Forms](~/xamarin-forms/data-cloud/data/files.md).

## <a name="related-links"></a>Связанные ссылки

- [Пример приложения Todo](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)
- [Пакет NuGet SQLite.NET](https://www.nuget.org/packages/sqlite-net-pcl/)
- [Документация по SQLite](https://www.sqlite.org/docs.html)
- [Использование SQLite с Android](~/android/data-cloud/data-access/using-sqlite-orm.md)
- [Использование SQLite с iOS](~/ios/data-cloud/data/using-sqlite-orm.md)
- [Асинхронная модель на основе задач (TAP)](https://docs.microsoft.com/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)
- [Класс Lazy<T>](https://docs.microsoft.com//api/system.lazy-1)