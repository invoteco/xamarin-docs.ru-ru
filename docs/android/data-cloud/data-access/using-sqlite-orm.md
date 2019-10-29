---
title: Использование SQLite.NET с Android
description: Библиотека NuGet PCL SQLite.NET предоставляет простой механизм доступа к данным для приложений Xamarin. Android.
ms.prod: xamarin
ms.assetid: 3447B7EE-A320-489E-AF02-E5721097760A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/18/2018
ms.openlocfilehash: 96f084dc49a5558767b162eee59eff722f247904
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73023674"
---
# <a name="using-sqlitenet-with-android"></a>Использование SQLite.NET с Android

Библиотека SQLite.NET, которую Xamarin рекомендует, — это очень простая модель ORM, которая позволяет легко хранить и извлекать объекты в локальной базе данных SQLite на устройстве Android. ORM означает объектно-реляционное сопоставление &ndash; API, который позволяет сохранять и извлекать "объекты" из базы данных без написания инструкций SQL.

Чтобы включить библиотеку SQLite.NET в приложение Xamarin, добавьте в проект следующий пакет NuGet:

- **Имя пакета:** SQLite-NET-PCL
- **Автор:** Федор A. Круежер
- **Идентификатор:** sqlite-net-pcl
- **URL-адрес:** [NuGet.org/Packages/SQLite-NET-PCL](https://www.nuget.org/packages/sqlite-net-pcl/)

[![Пакет NuGet SQLite.NET](using-sqlite-orm-images/image1a-sml.png "Пакет NuGet SQLite.NET")](using-sqlite-orm-images/image1a.png#lightbox)

> [!TIP]
> Доступно несколько различных пакетов SQLite. Убедитесь, что выбран правильный вариант (возможно, он не является самым максимальным результатом поиска).

Получив доступ к библиотеке SQLite.NET, выполните следующие три шага, чтобы использовать ее для доступа к базе данных.

1. **Добавьте инструкцию using** &ndash; добавьте следующую инструкцию в файлы C# , где требуется доступ к данным:

    ```csharp
    using SQLite;
    ```

2. **Создайте пустую базу данных** , &ndash; ссылку на базу данных можно создать, передав путь к файлу в конструкторе класса склитеконнектион. Не нужно проверять, существует ли файл, &ndash; он будет автоматически создан при необходимости, в противном случае будет открыт существующий файл базы данных. Переменная `dbPath` должна быть определена в соответствии с правилами, описанными ранее в этом документе:

    ```csharp
    var db = new SQLiteConnection (dbPath);
    ```

3. **Сохранить данные** &ndash; после создания объекта склитеконнектион команды базы данных выполняются путем вызова своих методов, таких как CreateTable и INSERT следующим образом:

    ```csharp
    db.CreateTable<Stock> ();
    db.Insert (newStock); // after creating the newStock object
    ```

4. **Получение данных** &ndash; для получения объекта (или списка объектов) используйте следующий синтаксис:

    ```csharp
    var stock = db.Get<Stock>(5); // primary key id of 5
    var stockList = db.Table<Stock>();
    ```

## <a name="basic-data-access-sample"></a>Пример базового доступа к данным

Пример кода *DataAccess_Basic* для этого документа выглядит следующим образом при работе в Android. В коде показано, как выполнять простые операции SQLite.NET и выводить результаты в виде текста в главном окне приложения.

**Android**

![Пример SQLite.NET для Android](using-sqlite-orm-images/image3.png "Пример SQLite.NET для Android")

В следующем примере кода показано взаимодействие базы данных с использованием библиотеки SQLite.NET для инкапсуляции базового доступа к базе данных.
Он показывает:

1. Создание файла базы данных

2. Вставка данных путем создания объектов и последующего их сохранения

3. Запрос данных

Необходимо включить следующие пространства имен:

```csharp
using SQLite; // from the github SQLite.cs class
```

Последний требует добавления SQLite в проект. Обратите внимание, что таблица базы данных SQLite определяется путем добавления атрибутов к классу (класс `Stock`), а не команды CREATE TABLE.

```csharp
[Table("Items")]
public class Stock {
    [PrimaryKey, AutoIncrement, Column("_id")]
    public int Id { get; set; }
    [MaxLength(8)]
    public string Symbol { get; set; }
}
public static void DoSomeDataAccess () {
       Console.WriteLine ("Creating database, if it doesn't already exist");
   string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "ormdemo.db3");
   var db = new SQLiteConnection (dbPath);
   db.CreateTable<Stock> ();
   if (db.Table<Stock> ().Count() == 0) {
        // only insert the data if it doesn't already exist
        var newStock = new Stock ();
        newStock.Symbol = "AAPL";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "GOOG";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "MSFT";
        db.Insert (newStock);
    }
    Console.WriteLine("Reading data");
    var table = db.Table<Stock> ();
    foreach (var s in table) {
        Console.WriteLine (s.Id + " " + s.Symbol);
    }
}
```

Использование атрибута `[Table]` без указания параметра имени таблицы приведет к тому, что имя базовой таблицы базы данных будет совпадать с именем класса (в данном случае — «фондовой биржи»). Фактическое имя таблицы важно, если вы пишете SQL-запросы непосредственно к базе данных, а не используете методы доступа к данным ORM. Аналогично, атрибут `[Column("_id")]` является необязательным, и при отсутствии столбца в таблицу будет добавлен столбец с тем же именем, что и у свойства в классе.

## <a name="sqlite-attributes"></a>Атрибуты SQLite

Общие атрибуты, которые можно применять к классам для управления их хранением в базовой базе данных, включают:

- **[PrimaryKey]** &ndash; этот атрибут можно применить к целочисленному свойству, чтобы принудительно использовать первичный ключ базовой таблицы. Составные первичные ключи не поддерживаются.

- **[AutoIncrement]** &ndash; этот атрибут приведет к автоматическому приращению значения целочисленного свойства для каждого нового объекта, вставленного в базу данных

- **[Column (Name)]** &ndash; при указании необязательного параметра `name` будет переопределять значение по умолчанию для имени столбца базовой базы данных (то же, что и свойство).

- **[Table (Name)]** &ndash; помечает класс как способный храниться в базовой таблице SQLite. Указание необязательного параметра name переопределит значение по умолчанию для имени базовой таблицы базы данных (которое совпадает с именем класса).

- **[MaxLength (значение)]** &ndash; ограничения длины свойства текста при попыток вставки базы данных. Использование кода должно проверить это перед вставкой объекта, так как этот атрибут является "checked" только при попытке выполнить операцию вставки или обновления базы данных.

- **[Ignore]** &ndash; приводит к тому, что SQLite.NET игнорирует это свойство.
    Это особенно полезно для свойств, имеющих тип, который не может храниться в базе данных, или свойства, которые не могут быть автоматически разрешены SQLite.

- **[Unique]** &ndash; гарантирует уникальность значений в столбце базовой базы данных.

Большинство этих атрибутов являются необязательными, SQLite будет использовать значения по умолчанию для имен таблиц и столбцов. Всегда следует указывать целочисленный первичный ключ, чтобы можно было эффективно выполнять запросы на выборку и удаление данных.

## <a name="more-complex-queries"></a>Более сложные запросы

Следующие методы `SQLiteConnection` можно использовать для выполнения других операций с данными:

- **Insert** &ndash; добавляет новый объект в базу данных.

- **Получите&lt;t&gt;** &ndash; пытается получить объект с помощью первичного ключа.

- **Таблица&lt;t&gt;** &ndash; возвращает все объекты в таблице.

- **Delete** &ndash; удаляет объект, используя его первичный ключ.

- **Запрос&lt;t&gt;** &ndash; выполнения SQL-запроса, возвращающего несколько строк (в виде объектов).

- **Выполнить** &ndash; используйте этот метод (и не `Query`), если не предполагается, что строки возвращаются из SQL (например, инструкции INSERT, Update и DELETE).

### <a name="getting-an-object-by-the-primary-key"></a>Получение объекта по первичному ключу

SQLite.Net предоставляет метод Get для получения одного объекта на основе его первичного ключа.

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>Выбор объекта с помощью LINQ

Методы, возвращающие коллекции, поддерживают `IEnumerable<T>` так что можно использовать LINQ для запроса или сортировки содержимого таблицы. В следующем коде показан пример использования LINQ для фильтрации всех записей, начинающихся с буквы "A":

```csharp
var apple = from s in db.Table<Stock>()
    where s.Symbol.StartsWith ("A")
    select s;
Console.WriteLine ("-> " + apple.FirstOrDefault ().Symbol);
```

### <a name="selecting-an-object-using-sql"></a>Выбор объекта с помощью SQL

Несмотря на то, что SQLite.Net может предоставлять доступ к данным на основе объектов, иногда может потребоваться более сложный запрос, чем разрешено LINQ (или может потребоваться более высокая производительность). Вы можете использовать команды SQL с методом query, как показано ниже:

```csharp
var stocksStartingWithA = db.Query<Stock>("SELECT * FROM Items WHERE Symbol = ?", "A");
foreach (var s in stocksStartingWithA) {
    Console.WriteLine ("a " + s.Symbol);
}
```

> [!NOTE]
> При написании инструкций SQL напрямую вы создаете зависимость от имен таблиц и столбцов в базе данных, которые были созданы из классов и их атрибутов. При изменении этих имен в коде следует помнить, что необходимо обновить все инструкции SQL, написанные вручную.

### <a name="deleting-an-object"></a>Удаление объекта

Первичный ключ используется для удаления строки, как показано ниже:

```csharp
var rowcount = db.Delete<Stock>(someStock.Id); // Id is the primary key
```

Можно проверить `rowcount`, чтобы подтвердить, сколько строк было затронуто (в этом случае удалено).

## <a name="using-sqlitenet-with-multiple-threads"></a>Использование SQLite.NET с несколькими потоками

SQLite поддерживает три различных режима работы с потоками: *один поток*, *несколько потоков*и *сериализованный*. Если требуется получить доступ к базе данных из нескольких потоков без каких бы то ни было ограничений, можно настроить SQLite для использования **сериализованного** режима потоков. Важно установить этот режим на ранних этапах работы приложения (например, в начале метода `OnCreate`).

Чтобы изменить режим потоковой обработки, вызовите `SqliteConnection.SetConfig`. Например, эта строка кода настраивает SQLite для **сериализованного** режима:

```csharp
using using Mono.Data.Sqlite;
...
SqliteConnection.SetConfig(SQLiteConfig.Serialized);
```

Версия SQLite для Android имеет ограничение, которое требует еще нескольких шагов. Если вызов `SqliteConnection.SetConfig` создает исключение SQLite, такое как `library used incorrectly`, необходимо использовать следующее решение:

1. Ссылка на собственную библиотеку **libsqlite.so** , чтобы интерфейсы API `sqlite3_shutdown` и `sqlite3_initialize` были доступны приложению:

    ```csharp
    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_shutdown();

    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_initialize();
    ```

2. В самом начале `OnCreate`ного метода добавьте этот код, чтобы завершить работу SQLite, настроить его для работы в **сериализованном** режиме и повторно инициализировать SQLite:

    ```csharp
    using using Mono.Data.Sqlite;
    ...
    sqlite3_shutdown();
    SqliteConnection.SetConfig(SQLiteConfig.Serialized);
    sqlite3_initialize();
    ```

Это решение также работает для библиотеки `Mono.Data.Sqlite`. Дополнительные сведения о SQLite и многопоточности см. в разделе [SQLite и несколько потоков](https://www.sqlite.org/threadsafe.html).

## <a name="related-links"></a>Связанные ссылки

- [Базовый доступ к данным (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Расширенный доступ к данным (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Доступ к данным Xamarin. Forms](~/xamarin-forms/data-cloud/data/databases.md)
