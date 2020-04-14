---
ms.openlocfilehash: b11a5972c2aabace8a6991a82f5719f34450297d
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/09/2020
ms.locfileid: "67841531"
---
В этом упражнении вы добавите классы доступа к данным в проект **LocalDatabaseTutorial**, который будет использоваться для сохранения данных о пользователях в базе данных.

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

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

    > [!IMPORTANT]
    > Преимущество использования отдельной базы данных в том, что создается отдельное подключение к базе данных, которое остается открытым, пока работает приложение. Это позволяет избежать затрат, связанных с открытием и закрытием файла базы данных каждый раз, когда выполняется операция с ней.

1. Соберите решение, чтобы убедиться в отсутствии ошибок.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

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

    > [!IMPORTANT]
    > Преимущество использования отдельной базы данных в том, что создается отдельное подключение к базе данных, которое остается открытым, пока работает приложение. Это позволяет избежать затрат, связанных с открытием и закрытием файла базы данных каждый раз, когда выполняется операция с ней.

1. Соберите решение, чтобы убедиться в отсутствии ошибок.
