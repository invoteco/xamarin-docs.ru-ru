---
title: Использование ADO.NET с Android
ms.prod: xamarin
ms.assetid: F6ABCEF1-951E-40D8-9EA9-DD79123C2650
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/08/2018
ms.openlocfilehash: 6592bd6d5cf7b78918fa2d020be723d662625e06
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73023758"
---
# <a name="using-adonet-with-android"></a>Использование ADO.NET с Android

Xamarin имеет встроенную поддержку для базы данных SQLite, которая доступна в Android и может быть предоставлена с помощью привычного синтаксиса, похожего на ADO.NET. Для использования этих API необходимо написать инструкции SQL, которые обрабатываются SQLite, такие как `CREATE TABLE`, `INSERT` и операторы `SELECT`.

## <a name="assembly-references"></a>Ссылки на сборки

Чтобы использовать SQLite для доступа через ADO.NET, необходимо добавить `System.Data` и `Mono.Data.Sqlite` ссылки на проект Android, как показано ниже:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows) 

![Ссылки на Android в Visual Studio](using-adonet-images/image7.png "Ссылки на Android в Visual Studio") 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos) 

![Ссылки на Android в Visual Studio для Mac](using-adonet-images/image5.png "Ссылки на Android в Visual Studio для Mac") 

-----

Щелкните правой кнопкой мыши **ссылку > изменить ссылки...** затем щелкните, чтобы выбрать необходимые сборки.

## <a name="about-monodatasqlite"></a>Сведения о Mono. Data. SQLite

Мы будем использовать класс `Mono.Data.Sqlite.SqliteConnection`, чтобы создать пустой файл базы данных, а затем создать экземпляр `SqliteCommand` объектов, которые можно использовать для выполнения инструкций SQL в базе данных.

**Создание пустой базы данных** &ndash; вызов метода `CreateFile` с допустимым путем к файлу (т. е. для записи). Перед вызовом этого метода следует убедиться, что файл уже существует, в противном случае новая (пустая) база данных будет создана в верхней части старой, а данные в старом файле будут потеряны.
`Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);` переменная `dbPath` должна быть определена в соответствии с правилами, описанными ранее в этом документе.

**Создание подключения к базе данных** &ndash; после создания файла базы данных SQLite можно создать объект соединения для доступа к данным. Соединение создается со строкой подключения, которая принимает форму `Data Source=file_path`, как показано ниже:

```csharp
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open();
// do stuff
connection.Close();
```

Как упоминалось ранее, соединение не следует использовать повторно в разных потоках. Если вы сомневаетесь, создайте подключение по мере необходимости и закройте его по завершении. но учитывать делать это чаще, чем требуется.

**Создание и выполнение команды базы данных** &ndash; после подключения мы можем выполнять на нем произвольные команды SQL. В приведенном ниже коде показана выполняемая инструкция `CREATE TABLE`.

```csharp
using (var command = connection.CreateCommand ()) {
    command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
    var rowcount = command.ExecuteNonQuery ();
}
```

При выполнении SQL непосредственно в базе данных необходимо принять обычные меры предосторожности, не позволяющие делать недопустимые запросы, например пытаться создать уже существующую таблицу. Следите за структурой базы данных, чтобы не вызывалась `SqliteException`, например **Таблица ошибок SQLite [Items], уже существует**.

## <a name="basic-data-access"></a>Базовый доступ к данным

Пример кода *DataAccess_Basic* для этого документа выглядит следующим образом при работе в Android:

![Пример ADO.NET для Android](using-adonet-images/image8.png "Пример ADO.NET для Android")

В приведенном ниже коде показано, как выполнять простые операции SQLite и выводить результаты в виде текста в главном окне приложения.

Необходимо включить следующие пространства имен:

```csharp
using System;
using System.IO;
using Mono.Data.Sqlite;
```

В следующем образце кода показано взаимодействие базы данных целиком.

1. Создание файла базы данных
2. Вставка данных
3. Запрос данных

Эти операции обычно появляются в коде в нескольких местах. Например, можно создать файл базы данных и таблицы при первом запуске приложения и выполнять операции чтения и записи данных на отдельных экранах в приложении. В приведенном ниже примере для этого примера были сгруппированы один метод:

```csharp
public static SqliteConnection connection;
public static string DoSomeDataAccess ()
{
    // determine the path for the database file
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "adodemo.db3");

    bool exists = File.Exists (dbPath);

    if (!exists) {
        Console.WriteLine("Creating database");
        // Need to create the database before seeding it with some data
        Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);
        connection = new SqliteConnection ("Data Source=" + dbPath);

        var commands = new[] {
            "CREATE TABLE [Items] (_id ntext, Symbol ntext);",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'AAPL')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('2', 'GOOG')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('3', 'MSFT')"
        };
        // Open the database connection and create table with data
        connection.Open ();
        foreach (var command in commands) {
            using (var c = connection.CreateCommand ()) {
                c.CommandText = command;
                var rowcount = c.ExecuteNonQuery ();
                Console.WriteLine("\tExecuted " + command);
            }
        }
    } else {
        Console.WriteLine("Database already exists");
        // Open connection to existing database file
        connection = new SqliteConnection ("Data Source=" + dbPath);
        connection.Open ();
    }

    // query the database to prove data was inserted!
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT [_id], [Symbol] from [Items]";
        var r = contents.ExecuteReader ();
        Console.WriteLine("Reading data");
        while (r.Read ())
            Console.WriteLine("\tKey={0}; Value={1}",
                              r ["_id"].ToString (),
                              r ["Symbol"].ToString ());
    }
    connection.Close ();
}

```

## <a name="more-complex-queries"></a>Более сложные запросы

Поскольку SQLite позволяет выполнять произвольные команды SQL для данных, можно выполнять любые `CREATE`, `INSERT`, `UPDATE`, `DELETE`или `SELECT` операторы. Сведения о командах SQL, поддерживаемых SQLite, можно узнать на веб-сайте SQLite. Инструкции SQL выполняются с помощью одного из трех методов объекта `SqliteCommand`:

- &ndash; **ExecuteNonQuery** обычно используется для создания таблицы или вставки данных. Возвращаемым значением для некоторых операций является число затронутых строк, в противном случае — значение-1.

- **ExecuteReader** &ndash; используется, когда коллекция строк должна возвращаться как `SqlDataReader`.

- **ExecuteScalar** &ndash; извлекает одно значение (например, статистическое выражение).

### <a name="executenonquery"></a>EXECUTENONQUERY

инструкции `INSERT`, `UPDATE`и `DELETE` возвращают количество затронутых строк. Все остальные инструкции SQL возвращают-1.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

Следующий метод показывает предложение `WHERE` в инструкции `SELECT`.
Поскольку код является созданием полной инструкции SQL, он должен позаботиться о необходимости экранирования зарезервированных символов, таких как кавычки (') вокруг строк.

```csharp
public static string MoreComplexQuery ()
{
    var output = "";
    output += "\nComplex query example: ";
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal), "ormdemo.db3");

    connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open ();
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT * FROM [Items] WHERE Symbol = 'MSFT'";
        var r = contents.ExecuteReader ();
        output += "\nReading data";
        while (r.Read ())
            output += String.Format ("\n\tKey={0}; Value={1}",
                    r ["_id"].ToString (),
                    r ["Symbol"].ToString ());
    }
    connection.Close ();

    return output;
}
```

Метод `ExecuteReader` возвращает объект `SqliteDataReader`. В дополнение к методу `Read`, показанному в примере, другие полезные свойства включают:

- **Ровсаффектед** &ndash; количество строк, затронутых запросом.

- **Хасровс** &ndash;, были ли возвращены какие бы то ни было строки.

### <a name="executescalar"></a>EXECUTESCALAR

Используйте его для `SELECT` инструкций, возвращающих одно значение (например, статистическое выражение).

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

Тип возвращаемого значения метода `ExecuteScalar` `object` &ndash; необходимо привести результат в зависимости от запроса к базе данных. Результатом может быть целое число из `COUNT` запроса или строки из одного столбца `SELECT` запроса. Обратите внимание, что это отличается от других методов `Execute`, возвращающих объект Reader или количество затронутых строк.

## <a name="related-links"></a>Связанные ссылки

- [Базовый доступ к данным (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Расширенный доступ к данным (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Рецепты по данным Android](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Доступ к данным Xamarin. Forms](~/xamarin-forms/data-cloud/data/databases.md)
