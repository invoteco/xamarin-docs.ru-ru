---
ms.openlocfilehash: 83e28796a2c387927dddd708da3ee6623f800a35
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61193254"
---
В этом упражнении вы добавите классы доступа к данным в проект **LocalDatabaseTutorial**, который будет использоваться для сохранения данных о пользователях в базе данных.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. В **обозревателе решений** в проекте **LocalDatabaseTutorial** добавьте новый класс с именем `Person`. Далее, удалите в **Person.cs** весь код шаблона и замените его приведенным ниже.

    ```csharp
    using SQLite;

    namespace LocalDatabaseTutorial
    {
        public class Person
        {
            [PrimaryKey, AutoIncrement]
            public int ID { get; set; }
            public string Name { get; set; }
            public int Age { get; set; }
        }
    }
    ```

    Этот код определяет класс `Person`, в котором будут храниться данные о каждом пользователе приложения. Свойство `ID` помечено атрибутами `PrimaryKey` и `AutoIncrement`, чтобы каждый экземпляр `Person` в базе данных имел уникальный идентификатор, предоставленный SQLite.NET.

1. В **обозревателе решений** в проекте **LocalDatabaseTutorial** добавьте новый класс с именем `Database`. Далее, удалите в **Database.cs** весь код шаблона и замените его приведенным ниже.

    ```csharp
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using SQLite;

    namespace LocalDatabaseTutorial
    {
        public class Database
        {
            readonly SQLiteAsyncConnection _database;

            public Database(string dbPath)
            {
                _database = new SQLiteAsyncConnection(dbPath);
                _database.CreateTableAsync<Person>().Wait();
            }

            public Task<List<Person>> GetPeopleAsync()
            {
                return _database.Table<Person>().ToListAsync();
            }

            public Task<int> SavePersonAsync(Person person)
            {
                return _database.InsertAsync(person);
            }
        }
    }
    ```

    Этот класс содержит код, чтобы создать базу данных, читать и записывать данные в ней. В коде используются асинхронные API-интерфейсы SQLite.NET, которые перемещают операции базы данных в фоновые потоки. Кроме того конструктор `Database` принимает путь файла базы данных в качестве аргумента. Этот путь будет предоставлен классом `App` в следующем упражнении.

1. В **обозревателе решений** в проекте **LocalDatabaseTutorial** разверните **App.xaml** и дважды щелкните файл **App.xaml.cs**, чтобы открыть его. Удалите в **App.xaml.cs** весь код шаблона и замените его приведенным ниже кодом.

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace LocalDatabaseTutorial
    {
        public partial class App : Application
        {
            static Database database;

            public static Database Database
            {
                get
                {
                    if (database == null)
                    {
                        database = new Database(Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "people.db3"));
                    }
                    return database;
                }
            }

            public App()
            {
                InitializeComponent();

                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                // Handle when your app starts
            }

            protected override void OnSleep()
            {
                // Handle when your app sleeps
            }

            protected override void OnResume()
            {
                // Handle when your app resumes
            }
        }
    }
    ```

    Этот код определяет свойство `Database`, которое создает новый экземпляр `Database` в качестве отдельной базы данных. Локальный путь к файлу и имя файла, с помощью которых представляется место хранения базы данных, передаются в качестве аргумента конструктору класса `Database`.

    > [!ВАЖНО. Преимущество использования отдельной базы данных в том, что создается отдельное подключение к базе данных, которое остается открытым, пока работает приложение. Это позволяет избежать затрат, связанных с открытием и закрытием файла базы данных каждый раз, когда выполняется операция с ней.

1. Создайте решение, чтобы убедиться в отсутствии ошибок.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. На **Панели решений** в проекте **LocalDatabaseTutorial** добавьте новый класс с именем `Person`. Далее, удалите в **Person.cs** весь код шаблона и замените его приведенным ниже.

    ```csharp
    using SQLite;

    namespace LocalDatabaseTutorial
    {
        public class Person
        {
            [PrimaryKey, AutoIncrement]
            public int ID { get; set; }
            public string Name { get; set; }
            public int Age { get; set; }
        }
    }
    ```

    Этот код определяет класс `Person`, в котором будут храниться данные о каждом пользователе приложения. Свойство `ID` помечено атрибутами `PrimaryKey` и `AutoIncrement`, чтобы каждый экземпляр `Person` в базе данных имел уникальный идентификатор, предоставленный SQLite.NET.

1. На **Панели решений** в проекте **LocalDatabaseTutorial** добавьте новый класс с именем `Database`. Далее, удалите в **Database.cs** весь код шаблона и замените его приведенным ниже.

    ```csharp
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using SQLite;

    namespace LocalDatabaseTutorial
    {
        public class Database
        {
            readonly SQLiteAsyncConnection _database;

            public Database(string dbPath)
            {
                _database = new SQLiteAsyncConnection(dbPath);
                _database.CreateTableAsync<Person>().Wait();
            }

            public Task<List<Person>> GetPeopleAsync()
            {
                return _database.Table<Person>().ToListAsync();
            }

            public Task<int> SavePersonAsync(Person person)
            {
                return _database.InsertAsync(person);
            }
        }
    }
    ```

    Этот класс содержит код, чтобы создать базу данных, читать и записывать данные в ней. В коде используются асинхронные API-интерфейсы SQLite.NET, которые перемещают операции базы данных в фоновые потоки. Кроме того конструктор `Database` принимает путь файла базы данных в качестве аргумента. Этот путь будет предоставлен классом `App` в следующем упражнении.

1. На **Панели решений** в проекте **LocalDatabaseTutorial** разверните **App.xaml** и дважды щелкните файл **App.xaml.cs**, чтобы открыть его. Удалите в **App.xaml.cs** весь код шаблона и замените его приведенным ниже кодом.

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace LocalDatabaseTutorial
    {
        public partial class App : Application
        {
            static Database database;

            public static Database Database
            {
                get
                {
                    if (database == null)
                    {
                        database = new Database(Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "people.db3"));
                    }
                    return database;
                }
            }

            public App()
            {
                InitializeComponent();

                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                // Handle when your app starts
            }

            protected override void OnSleep()
            {
                // Handle when your app sleeps
            }

            protected override void OnResume()
            {
                // Handle when your app resumes
            }
        }
    }
    ```

    Этот код определяет свойство `Database`, которое создает новый экземпляр `Database` в качестве отдельной базы данных. Локальный путь к файлу и имя файла, с помощью которых представляется место хранения базы данных, передаются в качестве аргумента конструктору класса `Database`.

    > [!ВАЖНО. Преимущество использования отдельной базы данных в том, что создается отдельное подключение к базе данных, которое остается открытым, пока работает приложение. Это позволяет избежать затрат, связанных с открытием и закрытием файла базы данных каждый раз, когда выполняется операция с ней.
    
1. Создайте решение, чтобы убедиться в отсутствии ошибок.
