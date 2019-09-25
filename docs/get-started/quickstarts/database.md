---
title: Хранение данных в локальной базе данных SQLite.NET
description: В этой статье объясняется, как хранить данные в локальной базе данных SQLite.NET.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 5BF901BD-FDE8-4B74-B4AB-418E81745A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2019
ms.openlocfilehash: 2cd4726566e73aece5d0deef90ad1feedefaa2d8
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2019
ms.locfileid: "71249680"
---
# <a name="store-data-in-a-local-sqlitenet-database"></a>Хранение данных в локальной базе данных SQLite.NET

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/)

В этом кратком руководстве вы узнаете, как выполнять следующие задачи:

- Используйте диспетчер пакетов NuGet, чтобы добавить пакет NuGet в проект.
- Храните данные локально в базе данных SQLite.NET.

Краткое руководство посвящено хранению данных в локальной базе данных SQLite.NET. Ниже показано итоговое приложение:

[![](database-images/screenshots1-sml.png "")Страница "заметки](database-images/screenshots1.png#lightbox "Страница «заметки»")" для записи заметки
[(database-images/screenshots2-sml.png "") ![]](database-images/screenshots2.png#lightbox "Страница записи примечания")

## <a name="prerequisites"></a>Предварительные требования

Перед выполнением этого краткого руководства необходимо успешно выполнить [предыдущее краткое руководство](multi-page.md) . Также можно скачать [предыдущий пример краткого руководства](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/) и использовать его в качестве отправной точки для этого краткого руководства.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Обновление приложения с помощью Visual Studio

1. Запустите Visual Studio и откройте решение Notes.

2. В **Обозреватель решений**выберите проект **Notes** , щелкните правой кнопкой мыши и выберите пункт **Управление пакетами NuGet...** :

    ![](database-images/vs/add-nuget-packages.png "Добавление пакетов NuGet")    

3. В разделе **Диспетчер пакетов NuGet** выберите вкладку **Обзор**, выполните поиск пакета NuGet **sqlite-net-pcl**, выберите его и нажмите кнопку **Установить**, чтобы добавить его в проект.

    ![](database-images/vs/add-package.png "Добавить пакет")

    > [!NOTE]
    > Существует ряд пакетов NuGet с похожими названиями. Правильный пакет имеет следующие атрибуты:
    > - **Автор(ы)** : Фрэнк А. Крюгер (Frank A. Krueger)
    > - **Идентификатор:** sqlite-net-pcl
    > - **Ссылка в NuGet:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > Несмотря на название, этот пакет NuGet можно использовать в проектах .NET Standard.

    Этот пакет будет использоваться для включения операций базы данных в приложение.

4. В **Обозреватель решений**в проекте **Notes** откройте **Note.CS** в папке **Models** и замените существующий код следующим кодом:

    ```csharp
    using System;
    using SQLite;

    namespace Notes.Models
    {
        public class Note
        {
            [PrimaryKey, AutoIncrement]
            public int ID { get; set; }
            public string Text { get; set; }
            public DateTime Date { get; set; }
        }
    }
    ```

    Этот класс определяет `Note` модель, которая будет хранить данные о каждой заметке в приложении. Свойство помечается атрибутами `AutoIncrement` `Note` и, чтобы гарантировать, что каждый экземпляр в базе данных SQLite.NET будет иметь уникальный идентификатор, предоставленный SQLite.NET. `PrimaryKey` `ID`

    Сохраните изменения в **Note.CS** , нажав клавиши **CTRL + S**, и закройте файл.

    > [!WARNING]
    > Попытка создать приложение на этом этапе приведет к ошибкам, которые будут исправлены в последующих шагах.

5. В **Обозреватель решений**добавьте новую папку с именем **Data** в проект **notess** .

6. В **Обозреватель решений**в проекте **Notes** добавьте новый класс с именем **нотедатабасе** в папку **Data** .

7. В **NoteDatabase.CS**замените существующий код следующим кодом:

    ```csharp
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using SQLite;
    using Notes.Models;

    namespace Notes.Data
    {
        public class NoteDatabase
        {
            readonly SQLiteAsyncConnection _database;

            public NoteDatabase(string dbPath)
            {
                _database = new SQLiteAsyncConnection(dbPath);
                _database.CreateTableAsync<Note>().Wait();
            }

            public Task<List<Note>> GetNotesAsync()
            {
                return _database.Table<Note>().ToListAsync();
            }

            public Task<Note> GetNoteAsync(int id)
            {
                return _database.Table<Note>()
                                .Where(i => i.ID == id)
                                .FirstOrDefaultAsync();
            }

            public Task<int> SaveNoteAsync(Note note)
            {
                if (note.ID != 0)
                {
                    return _database.UpdateAsync(note);
                }
                else
                {
                    return _database.InsertAsync(note);
                }
            }

            public Task<int> DeleteNoteAsync(Note note)
            {
                return _database.DeleteAsync(note);
            }
        }
    }
    ```

    Этот класс содержит код для создания базы данных, считывания данных из нее, записи данных в нее и удаления данных из нее. В коде используются асинхронные API-интерфейсы SQLite.NET, которые перемещают операции базы данных в фоновые потоки. Кроме того конструктор `NoteDatabase` принимает путь файла базы данных в качестве аргумента. Этот путь будет предоставлен `App` классом на следующем шаге.

    Сохраните изменения в **NoteDatabase.CS** , нажав клавиши **CTRL + S**, и закройте файл.

    > [!WARNING]
    > Попытка создать приложение на этом этапе приведет к ошибкам, которые будут исправлены в последующих шагах.

8. В **Обозреватель решений**в проекте **Notes** дважды щелкните **app.XAML.CS** , чтобы открыть его. Затем замените существующий код следующим кодом:

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;
    using Notes.Data;

    namespace Notes
    {
        public partial class App : Application
        {
            static NoteDatabase database;

            public static NoteDatabase Database
            {
                get
                {
                    if (database == null)
                    {
                        database = new NoteDatabase(Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "Notes.db3"));
                    }
                    return database;
                }
            }

            public App()
            {
                InitializeComponent();
                MainPage = new NavigationPage(new NotesPage());
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

    Этот код определяет `Database` свойство, которое создает новый `NoteDatabase` экземпляр в виде одноэлементного массива, передавая имя файла базы данных в `NoteDatabase` качестве аргумента в конструктор. Преимущество использования отдельной базы данных в том, что создается отдельное подключение к базе данных, которое остается открытым, пока работает приложение. Это позволяет избежать затрат, связанных с открытием и закрытием файла базы данных каждый раз, когда выполняется операция с ней.

    Сохраните изменения в файле **App.xaml.cs**, нажав клавиши **CTRL+S**, и закройте файл.

    > [!WARNING]
    > Попытка создать приложение на этом этапе приведет к ошибкам, которые будут исправлены в последующих шагах.

9. В **Обозреватель решений**в проекте **Notes** дважды щелкните **NotesPage.XAML.CS** , чтобы открыть его. Затем замените `OnAppearing` метод следующим кодом:

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        listView.ItemsSource = await App.Database.GetNotesAsync();
    }
    ```    

    Этот код заполняет [`ListView`](xref:Xamarin.Forms.ListView) с помощью любых заметок, хранящихся в базе данных.

    Сохраните изменения в **NotesPage.XAML.CS** , нажав клавиши **CTRL + S**, и закройте файл.

    > [!WARNING]
    > Попытка создать приложение на этом этапе приведет к ошибкам, которые будут исправлены в последующих шагах.

10. В **Обозреватель решений**дважды щелкните **NoteEntryPage.XAML.CS** , чтобы открыть его. Затем замените `OnSaveButtonClicked` методы и `OnDeleteButtonClicked` следующим кодом:

      ```csharp
      async void OnSaveButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          note.Date = DateTime.UtcNow;
          await App.Database.SaveNoteAsync(note);
          await Navigation.PopAsync();
      }

      async void OnDeleteButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          await App.Database.DeleteNoteAsync(note);
          await Navigation.PopAsync();
      }
      ```    

      Объект `NoteEntryPage` [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) хранит экземпляр, представляющий отдельную заметку, на странице. `Note` При выполнении`Note` обработчика событийэкземплярсохраняетсявбазеданных,иприложениепереходитна`OnSaveButtonClicked` предыдущую страницу. При выполнении`Note` обработчика событийэкземплярудаляетсяизбазыданных,иприложениепереходитна`OnDeleteButtonClicked` предыдущую страницу.

      Сохраните изменения в **NoteEntryPage.XAML.CS** , нажав клавиши **CTRL + S**, и закройте файл.

11. Создайте и запустите проект на каждой платформе. Дополнительные сведения см. [в разделе Создание краткого руководства](single-page.md#building-the-quickstart).

    В **нотеспаже** нажмите **+** кнопку, чтобы перейти к **нотинтрипаже** и ввести Примечание. После сохранения примечания приложение вернется обратно к **нотеспаже**.

    Введите число заметок, отличающихся длиной, для отслеживания поведения приложения.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Обновление приложения с помощью Visual Studio для Mac

1. Запустите Visual Studio для Mac и откройте проект Notes.

2. В **панель решения**выберите проект **Notes** , щелкните правой кнопкой мыши и выберите **Добавить > Добавить пакеты NuGet...** :

    ![](database-images/vsmac/add-nuget-packages.png "Добавление пакетов NuGet")    

3. В окне **Добавление пакетов** выполните поиск пакета NuGet **sqlite-net-pcl**, выберите его и нажмите кнопку **Добавить пакет**, чтобы добавить его в проект.

    ![](database-images/vsmac/add-package.png "Добавить пакет")

    > [!NOTE]
    > Существует ряд пакетов NuGet с похожими названиями. Правильный пакет имеет следующие атрибуты:
    > - **Автор**: Фрэнк А. Крюгер (Frank A. Krueger)
    > - **Идентификатор:** sqlite-net-pcl
    > - **Ссылка в NuGet:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > Несмотря на название, этот пакет NuGet можно использовать в проектах .NET Standard.

    Этот пакет будет использоваться для включения операций базы данных в приложение.

4. В **панель решения**в проекте **Notes** откройте **Note.CS** в папке **Models** и замените существующий код следующим кодом:

    ```csharp
    using System;
    using SQLite;

    namespace Notes.Models
    {
        public class Note
        {
            [PrimaryKey, AutoIncrement]
            public int ID { get; set; }
            public string Text { get; set; }
            public DateTime Date { get; set; }
        }
    }
    ```

    Этот класс определяет `Note` модель, которая будет хранить данные о каждой заметке в приложении. Свойство помечается атрибутами `AutoIncrement` `Note` и, чтобы гарантировать, что каждый экземпляр в базе данных SQLite.NET будет иметь уникальный идентификатор, предоставленный SQLite.NET. `PrimaryKey` `ID`

    Сохраните изменения в **Note.CS** , выбрав **Файл > Сохранить** (или нажав клавиши  **&#8984; + S**) и закройте файл.

    > [!WARNING]
    > Попытка создать приложение на этом этапе приведет к ошибкам, которые будут исправлены в последующих шагах.

5. В **панель решения**добавьте новую папку с именем **Data** в проект **notess** .

6. В **панель решения**в проекте **Notes** добавьте новый класс с именем **нотедатабасе** в папку **Data** .

7. В **NoteDatabase.CS**замените существующий код следующим кодом:

    ```csharp
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using SQLite;
    using Notes.Models;

    namespace Notes.Data
    {
        public class NoteDatabase
        {
            readonly SQLiteAsyncConnection _database;

            public NoteDatabase(string dbPath)
            {
                _database = new SQLiteAsyncConnection(dbPath);
                _database.CreateTableAsync<Note>().Wait();
            }

            public Task<List<Note>> GetNotesAsync()
            {
                return _database.Table<Note>().ToListAsync();
            }

            public Task<Note> GetNoteAsync(int id)
            {
                return _database.Table<Note>()
                                .Where(i => i.ID == id)
                                .FirstOrDefaultAsync();
            }

            public Task<int> SaveNoteAsync(Note note)
            {
                if (note.ID != 0)
                {
                    return _database.UpdateAsync(note);
                }
                else
                {
                    return _database.InsertAsync(note);
                }
            }

            public Task<int> DeleteNoteAsync(Note note)
            {
                return _database.DeleteAsync(note);
            }
        }
    }
    ```

    Этот класс содержит код для создания базы данных, считывания данных из нее, записи данных в нее и удаления данных из нее. В коде используются асинхронные API-интерфейсы SQLite.NET, которые перемещают операции базы данных в фоновые потоки. Кроме того конструктор `NoteDatabase` принимает путь файла базы данных в качестве аргумента. Этот путь будет предоставлен `App` классом на следующем шаге.

    Сохраните изменения в **NoteDatabase.CS** , выбрав **Файл > Сохранить** (или нажав клавиши  **&#8984; + S**) и закройте файл.

    > [!WARNING]
    > Попытка создать приложение на этом этапе приведет к ошибкам, которые будут исправлены в последующих шагах.

8. В **панель решения**в проекте **Notes** дважды щелкните **app.XAML.CS** , чтобы открыть его. Затем замените существующий код следующим кодом:

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;
    using Notes.Data;

    namespace Notes
    {
        public partial class App : Application
        {
            static NoteDatabase database;

            public static NoteDatabase Database
            {
                get
                {
                    if (database == null)
                    {
                        database = new NoteDatabase(Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "Notes.db3"));
                    }
                    return database;
                }
            }

            public App()
            {
                InitializeComponent();
                MainPage = new NavigationPage(new NotesPage());
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

    Этот код определяет `Database` свойство, которое создает новый `NoteDatabase` экземпляр в виде одноэлементного массива, передавая имя файла базы данных в `NoteDatabase` качестве аргумента в конструктор. Преимущество использования отдельной базы данных в том, что создается отдельное подключение к базе данных, которое остается открытым, пока работает приложение. Это позволяет избежать затрат, связанных с открытием и закрытием файла базы данных каждый раз, когда выполняется операция с ней.

    Сохраните изменения в файле **App.xaml.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**, и закройте файл.

    > [!WARNING]
    > Попытка создать приложение на этом этапе приведет к ошибкам, которые будут исправлены в последующих шагах.

9. В **панель решения**в проекте **Notes** дважды щелкните **NotesPage.XAML.CS** , чтобы открыть его. Затем замените `OnAppearing` метод следующим кодом:

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        listView.ItemsSource = await App.Database.GetNotesAsync();
    }
    ```    

    Этот код заполняет [`ListView`](xref:Xamarin.Forms.ListView) с помощью любых заметок, хранящихся в базе данных.

    Сохраните изменения в **NotesPage.XAML.CS** , выбрав **Файл > Сохранить** (или нажав клавиши  **&#8984; + S**) и закройте файл.

    > [!WARNING]
    > Попытка создать приложение на этом этапе приведет к ошибкам, которые будут исправлены в последующих шагах.

10. В **панель решения**дважды щелкните **NoteEntryPage.XAML.CS** , чтобы открыть его. Затем замените `OnSaveButtonClicked` методы и `OnDeleteButtonClicked` следующим кодом:

      ```csharp
      async void OnSaveButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          note.Date = DateTime.UtcNow;
          await App.Database.SaveNoteAsync(note);
          await Navigation.PopAsync();
      }

      async void OnDeleteButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          await App.Database.DeleteNoteAsync(note);
          await Navigation.PopAsync();
      }
      ```    

      Объект `NoteEntryPage` [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) хранит экземпляр, представляющий отдельную заметку, на странице. `Note` При выполнении`Note` обработчика событийэкземплярсохраняетсявбазеданных,иприложениепереходитна`OnSaveButtonClicked` предыдущую страницу. При выполнении`Note` обработчика событийэкземплярудаляетсяизбазыданных,иприложениепереходитна`OnDeleteButtonClicked` предыдущую страницу.

      Сохраните изменения в **NoteEntryPage.XAML.CS** , выбрав **Файл > Сохранить** (или нажав клавиши  **&#8984; + S**) и закройте файл.

11. Создайте и запустите проект на каждой платформе. Дополнительные сведения см. [в разделе Создание краткого руководства](single-page.md#building-the-quickstart).

    В **нотеспаже** нажмите **+** кнопку, чтобы перейти к **нотинтрипаже** и ввести Примечание. После сохранения примечания приложение вернется обратно к **нотеспаже**.

    Введите число заметок, отличающихся длиной, для отслеживания поведения приложения.

::: zone-end

## <a name="next-steps"></a>Следующие шаги

В этом кратком руководстве вы узнали, как выполнять следующие задачи:

- Используйте диспетчер пакетов NuGet, чтобы добавить пакет NuGet в проект.
- Храните данные локально в базе данных SQLite.NET.

Чтобы присвоить стилю приложения стили XAML, перейдите к следующему краткому руководству.

> [!div class="nextstepaction"]
> [Вперед](styling.md)

## <a name="related-links"></a>Связанные ссылки

- [Заметки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/)
- [Углубленное углубление в руководстве по Xamarin. Forms](deepdive.md)
