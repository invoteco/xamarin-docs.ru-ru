---
title: Навигация в многостраничном приложении Xamarin.Forms
description: В этой статье объясняется, как превратить одностраничное приложение, способное хранить одно Примечание, в многостраничное приложение, способное хранить несколько заметок.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 9DC3B3D6-6CBC-4705-BE80-3D86A9E65F92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2019
ms.openlocfilehash: 9ce02b4c6412eab1f4b1003b262573c59940286c
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2019
ms.locfileid: "68653798"
---
# <a name="perform-navigation-in-a-multi-page-xamarinforms-application"></a>Выполнение навигации в приложении Xamarin. Forms с несколькими страницами

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/)

В этом кратком руководстве вы узнаете, как выполнять следующие задачи:

- Добавьте дополнительные страницы в решение Xamarin. Forms.
- Выполнить навигацию между страницами.
- Используйте привязку данных для синхронизации данных между элементами пользовательского интерфейса и их источником данных.

В кратком руководстве описывается, как превратить одноплатформенное приложение Xamarin. Forms, которое может хранить одну заметку, в многостраничное приложение, способное хранить несколько заметок. Ниже показано итоговое приложение:

[![](multi-page-images/screenshots1-sml.png "")Страница "заметки](multi-page-images/screenshots1.png#lightbox "Страница «заметки»")" для записи заметки
[(multi-page-images/screenshots2-sml.png "") ![]](multi-page-images/screenshots2.png#lightbox "Страница записи примечания")

### <a name="prerequisites"></a>Предварительные требования

Перед выполнением этого краткого руководства необходимо успешно выполнить [предыдущее краткое руководство](single-page.md) . Также можно скачать [предыдущий пример краткого руководства](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-singlepage/) и использовать его в качестве отправной точки для этого краткого руководства.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Обновление приложения с помощью Visual Studio

1. Запустите Visual Studio. В окне "Пуск" щелкните решение " **Примечания** " в списке "последние проекты/решения" или нажмите кнопку **Открыть проект или**решение, а затем в диалоговом окне **Открыть проект/решение** выберите файл решения для проекта Notes:

    ![](multi-page-images/vs/open-solution.png "Открытие проекта")

2. В **Обозреватель решений**щелкните правой кнопкой мыши проект **Notes** и выберите **Добавить > Новая папка**:

    ![](multi-page-images/vs/add-new-item.png "Добавление нового элемента")

3. В **Обозреватель решений**назовите новые **модели**папок:

    ![](multi-page-images/vs/name-folder.png "Папка Models")

4. В **Обозреватель решений**выберите папку **модели** , щелкните ее правой кнопкой мыши и выберите **Добавить > новый элемент...** :

    ![](multi-page-images/vs/add-new-models-file.png "Добавление нового файла")

5. В диалоговом окне **Добавление нового элемента** выберите **визуальные C# элементы > класс**, присвойте новому файлу **Примечание**и нажмите кнопку **Добавить** :

    ![](multi-page-images/vs/add-note-class.png "Добавление класса Note")

    В результате в папку **Models** проекта **Notes** будет добавлен класс **Note** .

6. В **Note.CS**удалите весь код шаблона и замените его следующим кодом:

    ```csharp
    using System;

    namespace Notes.Models
    {
        public class Note
        {
            public string Filename { get; set; }
            public string Text { get; set; }
            public DateTime Date { get; set; }
        }
    }
    ```

    Этот класс определяет `Note` модель, которая будет хранить данные о каждой заметке в приложении.    

    Сохраните изменения в **Note.CS** , нажав клавиши **CTRL + S**, и закройте файл.

7. В **Обозреватель решений**щелкните правой кнопкой мыши проект **Notes** и выберите **Добавить > новый элемент..** . В диалоговом окне **Добавление нового элемента** выберите  **C# визуальные элементы > странице Xamarin. Forms > Content**, назовите новый файл **нотинтрипаже**и нажмите кнопку **Добавить** :

    ![](multi-page-images/vs/add-note-entry-page.png "Добавление Xamarin. Forms ContentPage")

    При этом в корневую папку проекта будет добавлена новая страница с именем **нотинтрипаже** . Эта страница будет второй страницей в приложении.

8. В **нотинтрипаже. XAML**удалите весь код шаблона и замените его следующим кодом:

      ```xaml
      <?xml version="1.0" encoding="UTF-8"?>
      <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                   x:Class="Notes.NoteEntryPage"
                   Title="Note Entry">
          <StackLayout Margin="20">
              <Editor Placeholder="Enter your note"
                      Text="{Binding Text}"
                      HeightRequest="100" />
              <Grid>
                  <Grid.ColumnDefinitions>
                      <ColumnDefinition Width="*" />
                      <ColumnDefinition Width="*" />
                  </Grid.ColumnDefinitions>
                  <Button Text="Save"
                          Clicked="OnSaveButtonClicked" />
                  <Button Grid.Column="1"
                          Text="Delete"
                          Clicked="OnDeleteButtonClicked"/>
              </Grid>
          </StackLayout>
      </ContentPage>
      ```

      Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`Editor`](xref:Xamarin.Forms.Editor) для ввода текста, и два [`Button`](xref:Xamarin.Forms.Button) экземпляра, которые направляют приложение для сохранения или удаления файла. Два `Button` экземпляра располагаются горизонтально [`Grid`](xref:Xamarin.Forms.Grid)в, с `Editor` помощью и `Grid` располагаются вертикально в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Кроме того `Editor` , компонент использует привязку данных для привязки `Text` к свойству `Note` модели. Дополнительные сведения о привязке данных см. в разделе [Привязка данных](deepdive.md#data-binding) в [кратком руководстве по Xamarin. Forms](deepdive.md).

      Сохраните изменения в **нотинтрипаже. XAML** , нажав клавиши **CTRL + S**, и закройте файл.

9. В **NoteEntryPage.XAML.CS**удалите весь код шаблона и замените его следующим кодом:

      ```csharp
      using System;
      using System.IO;
      using Xamarin.Forms;
      using Notes.Models;

      namespace Notes
      {
          public partial class NoteEntryPage : ContentPage
          {
              public NoteEntryPage()
              {
                  InitializeComponent();
              }

              async void OnSaveButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (string.IsNullOrWhiteSpace(note.Filename))
                  {
                      // Save
                      var filename = Path.Combine(App.FolderPath, $"{Path.GetRandomFileName()}.notes.txt");
                      File.WriteAllText(filename, note.Text);
                  }
                  else
                  {
                      // Update
                      File.WriteAllText(note.Filename, note.Text);
                  }

                  await Navigation.PopAsync();
              }

              async void OnDeleteButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (File.Exists(note.Filename))
                  {
                      File.Delete(note.Filename);
                  }

                  await Navigation.PopAsync();
              }
          }
      }
      ```

      Этот код сохраняет `Note` экземпляр, представляющий одну заметку, [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) на странице. При нажатии кнопки `OnSaveButtonClicked` " **сохранить** [`Button`](xref:Xamarin.Forms.Button) " выполняется обработчик события, который либо сохраняет содержимое в `Editor` новый файл со случайно сформированным именем файла, либо в существующий файл, если заметка обновляется. В обоих случаях файл хранится в папке локальных данных приложений для приложения. Затем метод перейдет назад на предыдущую страницу. При нажатии кнопки `OnDeleteButtonClicked` " **Удалить** `Button` " выполняется обработчик события, который удаляет файл, если он существует, и переходит назад к предыдущей странице. Дополнительные сведения о навигации см. в разделе [Навигация](deepdive.md#navigation) в [кратком руководстве по Xamarin. Forms](deepdive.md).

      Сохраните изменения в **NoteEntryPage.XAML.CS** , нажав клавиши **CTRL + S**, и закройте файл.

      > [!WARNING]
      > Попытка создать приложение на этом этапе приведет к ошибкам, которые будут исправлены в последующих шагах.

10. В **Обозреватель решений**щелкните правой кнопкой мыши проект **Notes** и выберите **Добавить > новый элемент..** . В диалоговом окне **Добавление нового элемента** выберите  **C# визуальные элементы > странице Xamarin. Forms > Content**, назовите новый файл **нотеспаже**и нажмите кнопку **Добавить** .

      При этом в корневую папку проекта будет добавлена страница с именем **нотеспаже** . Эта страница будет корневой страницей приложения.

11. В **нотеспаже. XAML**удалите весь код шаблона и замените его следующим кодом:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NotesPage"
                 Title="Notes">
        <ContentPage.ToolbarItems>
            <ToolbarItem Text="+"
                         Clicked="OnNoteAddedClicked" />
        </ContentPage.ToolbarItems>
        <ListView x:Name="listView"
                  Margin="20"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Text}"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, состоящий из [`ListView`](xref:Xamarin.Forms.ListView) [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)и. Использует привязку данных для вывода всех заметок, извлекаемых приложением, и выбора примечания будет переходить к, `NoteEntryPage` где можно изменить заметку. `ListView` Кроме того, можно создать новую заметку, нажав кнопку `ToolbarItem`. Дополнительные сведения о привязке данных см. в разделе [Привязка данных](deepdive.md#data-binding) в [кратком руководстве по Xamarin. Forms](deepdive.md).

    Сохраните изменения в **нотеспаже. XAML** , нажав клавиши **CTRL + S**, и закройте файл.

12. В **NotesPage.XAML.CS**удалите весь код шаблона и замените его следующим кодом:

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Xamarin.Forms;
    using Notes.Models;

    namespace Notes
    {
        public partial class NotesPage : ContentPage
        {
            public NotesPage()
            {
                InitializeComponent();
            }

            protected override void OnAppearing()
            {
                base.OnAppearing();

                var notes = new List<Note>();

                var files = Directory.EnumerateFiles(App.FolderPath, "*.notes.txt");
                foreach (var filename in files)
                {
                    notes.Add(new Note
                    {
                        Filename = filename,
                        Text = File.ReadAllText(filename),
                        Date = File.GetCreationTime(filename)
                    });
                }

                listView.ItemsSource = notes
                    .OrderBy(d => d.Date)
                    .ToList();
            }

            async void OnNoteAddedClicked(object sender, EventArgs e)
            {
                await Navigation.PushAsync(new NoteEntryPage
                {
                    BindingContext = new Note()
                });
            }

            async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
            {
                if (e.SelectedItem != null)
                {
                    await Navigation.PushAsync(new NoteEntryPage
                    {
                        BindingContext = e.SelectedItem as Note
                    });
                }
            }
        }
    }
    ```    

    Этот код определяет функциональные возможности для `NotesPage`. При появлении `OnAppearing` страницы выполняется метод, который заполняется [`ListView`](xref:Xamarin.Forms.ListView) заметками, полученными из папки локальных данных приложения. [Принажатиикнопкивыполняетсяобработчиксобытия.`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) `OnNoteAddedClicked` Этот метод переходит `NoteEntryPage`к, [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) присвоив параметру `NoteEntryPage` значение нового `Note` экземпляра. `ListView` Если`OnListViewItemSelected` выбран элемент в, выполняется обработчик события. Этот метод выполняет переход `NoteEntryPage`к, [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) устанавливая `NoteEntryPage` объект в выбранном `Note` экземпляре. Дополнительные сведения о навигации см. в разделе [Навигация](deepdive.md#navigation) в [кратком руководстве по Xamarin. Forms](deepdive.md).

    Сохраните изменения в **NotesPage.XAML.CS** , нажав клавиши **CTRL + S**, и закройте файл.

    > [!WARNING]
    > Попытка создать приложение на этом этапе приведет к ошибкам, которые будут исправлены в последующих шагах.

13. В **Обозреватель решений**дважды щелкните **app.XAML.CS** , чтобы открыть его. Затем замените существующий код следующим кодом:

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class App : Application
        {
            public static string FolderPath { get; private set; }

            public App()
            {
                InitializeComponent();
                FolderPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData));
                MainPage = new NavigationPage(new NotesPage());
            }
            // ...
        }
    }
    ```

    Этот код добавляет объявление пространства имен для `System.IO` пространства имен и добавляет объявление для статического `FolderPath` свойства типа `string`. `FolderPath` Свойство используется для хранения пути на устройстве, где будут храниться данные о заметках. Кроме того, код инициализирует `FolderPath` свойство `App` в конструкторе и инициализирует [`MainPage`](xref:Xamarin.Forms.Application.MainPage) свойство [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) , `NotesPage`чтобы оно было размещено в экземпляре. Дополнительные сведения о навигации см. в разделе [Навигация](deepdive.md#navigation) в [кратком руководстве по Xamarin. Forms](deepdive.md).

    Сохраните изменения в файле **App.xaml.cs**, нажав клавиши **CTRL+S**, и закройте файл.

14. В **Обозреватель решений**в проекте **Notes** щелкните правой кнопкой мыши **MainPage. XAML**и выберите **Удалить**. В появившемся диалоговом окне нажмите кнопку **ОК** , чтобы удалить файл с жесткого диска.

    При этом удаляется страница, которая больше не используется.

15. Создайте и запустите проект на каждой платформе. Дополнительные сведения см. [в разделе Создание краткого руководства](single-page.md#building-the-quickstart).

    В **нотеспаже** нажмите **+** кнопку, чтобы перейти к **нотинтрипаже** и ввести Примечание. После сохранения примечания приложение вернется обратно к **нотеспаже**.

    Введите число заметок, отличающихся длиной, для отслеживания поведения приложения.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Обновление приложения с помощью Visual Studio для Mac

1. Запуск Visual Studio для Mac В окне Пуск щелкните **Открыть**и в диалоговом окне выберите файл решения для проекта Notes.

    ![](multi-page-images/vsmac/open-solution.png "Открытие решения")

2. В **панель решения**выберите проект **notess** , щелкните правой кнопкой мыши и выберите **Добавить > Новая папка**:

    ![](multi-page-images/vsmac/add-new-folder.png "Добавить новую папку")

3. В **панель решения**назовите новые **модели**папок:

    ![](multi-page-images/vsmac/name-folder.png "Папка Models")

4. В **панель решения**выберите папку **Models** , щелкните ее правой кнопкой мыши и выберите **Добавить > новый файл...** :

    ![](multi-page-images/vsmac/add-new-models-file.png "Добавление нового файла")

5. В диалоговом окне **новый файл** выберите **Общие > пустой класс**, присвойте новому файлу имя **Note**и нажмите кнопку **создать** :

    ![](multi-page-images/vsmac/add-note-class.png "Добавление класса Note")

    В результате в папку **Models** проекта **Notes** будет добавлен класс **Note** .

6. В **Note.CS**удалите весь код шаблона и замените его следующим кодом:

    ```csharp
    using System;

    namespace Notes.Models
    {
        public class Note
        {
            public string Filename { get; set; }
            public string Text { get; set; }
            public DateTime Date { get; set; }
        }
    }
    ```

    Этот класс определяет `Note` модель, которая будет хранить данные о каждой заметке в приложении.

    Сохраните изменения в **Note.CS** , выбрав **Файл > Сохранить** (или нажав клавиши  **&#8984; + S**) и закройте файл.

7. В **панель решения**выберите проект **notess** , щелкните правой кнопкой мыши и выберите **Добавить > новый файл...** . В диалоговом окне **новый файл** выберите **Forms > Forms ContentPage XAML**, назовите новый файл **нотинтрипаже**и нажмите кнопку **создать** :

    ![](multi-page-images/vsmac/add-note-entry-page.png "Добавление Xamarin. Forms ContentPage")

    При этом в корневую папку проекта будет добавлена новая страница с именем **нотинтрипаже** . Эта страница будет второй страницей в приложении.

8. В **нотинтрипаже. XAML**удалите весь код шаблона и замените его следующим кодом:

      ```xaml
      <?xml version="1.0" encoding="UTF-8"?>
      <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                   x:Class="Notes.NoteEntryPage"
                   Title="Note Entry">
          <StackLayout Margin="20">
              <Editor Placeholder="Enter your note"
                      Text="{Binding Text}"
                      HeightRequest="100" />
              <Grid>
                  <Grid.ColumnDefinitions>
                      <ColumnDefinition Width="*" />
                      <ColumnDefinition Width="*" />
                  </Grid.ColumnDefinitions>
                  <Button Text="Save"
                          Clicked="OnSaveButtonClicked" />
                  <Button Grid.Column="1"
                          Text="Delete"
                          Clicked="OnDeleteButtonClicked"/>
              </Grid>
          </StackLayout>
      </ContentPage>
      ```

      Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`Editor`](xref:Xamarin.Forms.Editor) для ввода текста, и два [`Button`](xref:Xamarin.Forms.Button) экземпляра, которые направляют приложение для сохранения или удаления файла. Два `Button` экземпляра располагаются горизонтально [`Grid`](xref:Xamarin.Forms.Grid)в, с `Editor` помощью и `Grid` располагаются вертикально в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Кроме того `Editor` , компонент использует привязку данных для привязки `Text` к свойству `Note` модели. Дополнительные сведения о привязке данных см. в разделе [Привязка данных](deepdive.md#data-binding) в [кратком руководстве по Xamarin. Forms](deepdive.md).

      Сохраните изменения в **нотинтрипаже. XAML** , выбрав **Файл > Сохранить** (или нажав клавиши  **&#8984; + S**) и закройте файл.

9. В **NoteEntryPage.XAML.CS**удалите весь код шаблона и замените его следующим кодом:

      ```csharp
      using System;
      using System.IO;
      using Xamarin.Forms;
      using Notes.Models;

      namespace Notes
      {
          public partial class NoteEntryPage : ContentPage
          {
              public NoteEntryPage()
              {
                  InitializeComponent();
              }

              async void OnSaveButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (string.IsNullOrWhiteSpace(note.Filename))
                  {
                      // Save
                      var filename = Path.Combine(App.FolderPath, $"{Path.GetRandomFileName()}.notes.txt");
                      File.WriteAllText(filename, note.Text);
                  }
                  else
                  {
                      // Update
                      File.WriteAllText(note.Filename, note.Text);
                  }

                  await Navigation.PopAsync();
              }

              async void OnDeleteButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (File.Exists(note.Filename))
                  {
                      File.Delete(note.Filename);
                  }

                  await Navigation.PopAsync();
              }
          }
      }
      ```

      Этот код сохраняет `Note` экземпляр, представляющий одну заметку, [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) на странице. При нажатии кнопки `OnSaveButtonClicked` " **сохранить** [`Button`](xref:Xamarin.Forms.Button) " выполняется обработчик события, который либо сохраняет содержимое в `Editor` новый файл со случайно сформированным именем файла, либо в существующий файл, если заметка обновляется. В обоих случаях файл хранится в папке локальных данных приложений для приложения. Затем метод перейдет назад на предыдущую страницу. При нажатии кнопки `OnDeleteButtonClicked` " **Удалить** `Button` " выполняется обработчик события, который удаляет файл, если он существует, и переходит назад к предыдущей странице. Дополнительные сведения о навигации см. в разделе [Навигация](deepdive.md#navigation) в [кратком руководстве по Xamarin. Forms](deepdive.md).

      Сохраните изменения в **NoteEntryPage.XAML.CS** , выбрав **Файл > Сохранить** (или нажав клавиши  **&#8984; + S**) и закройте файл.

      > [!WARNING]
      > Попытка создать приложение на этом этапе приведет к ошибкам, которые будут исправлены в последующих шагах.

10. В **панель решения**выберите проект **notess** , щелкните правой кнопкой мыши и выберите **Добавить > новый файл...** . В диалоговом окне **новый файл** выберите **Forms > Forms ContentPage XAML**, назовите новый файл **нотеспаже**и нажмите кнопку **создать** .

      При этом в корневую папку проекта будет добавлена страница с именем **нотеспаже** . Эта страница будет корневой страницей приложения.

11. В **нотеспаже. XAML**удалите весь код шаблона и замените его следующим кодом:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NotesPage"
                 Title="Notes">
        <ContentPage.ToolbarItems>
            <ToolbarItem Text="+"
                         Clicked="OnNoteAddedClicked" />
        </ContentPage.ToolbarItems>
        <ListView x:Name="listView"
                  Margin="20"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Text}"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, состоящий из [`ListView`](xref:Xamarin.Forms.ListView) [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)и. Использует привязку данных для вывода всех заметок, извлекаемых приложением, и выбора примечания будет переходить к, `NoteEntryPage` где можно изменить заметку. `ListView` Кроме того, можно создать новую заметку, нажав кнопку `ToolbarItem`. Дополнительные сведения о привязке данных см. в разделе [Привязка данных](deepdive.md#data-binding) в [кратком руководстве по Xamarin. Forms](deepdive.md).

    Сохраните изменения в **нотеспаже. XAML** , выбрав **Файл > Сохранить** (или нажав клавиши  **&#8984; + S**) и закройте файл.

12. В **NotesPage.XAML.CS**удалите весь код шаблона и замените его следующим кодом:

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Xamarin.Forms;
    using Notes.Models;

    namespace Notes
    {
        public partial class NotesPage : ContentPage
        {
            public NotesPage()
            {
                InitializeComponent();
            }

            protected override void OnAppearing()
            {
                base.OnAppearing();

                var notes = new List<Note>();

                var files = Directory.EnumerateFiles(App.FolderPath, "*.notes.txt");
                foreach (var filename in files)
                {
                    notes.Add(new Note
                    {
                        Filename = filename,
                        Text = File.ReadAllText(filename),
                        Date = File.GetCreationTime(filename)
                    });
                }

                listView.ItemsSource = notes
                    .OrderBy(d => d.Date)
                    .ToList();
            }

            async void OnNoteAddedClicked(object sender, EventArgs e)
            {
                await Navigation.PushAsync(new NoteEntryPage
                {
                    BindingContext = new Note()
                });
            }

            async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
            {
                if (e.SelectedItem != null)
                {
                    await Navigation.PushAsync(new NoteEntryPage
                    {
                        BindingContext = e.SelectedItem as Note
                    });
                }
            }
        }
    }
    ```    

    Этот код определяет функциональные возможности для `NotesPage`. При появлении `OnAppearing` страницы выполняется метод, который заполняется [`ListView`](xref:Xamarin.Forms.ListView) заметками, полученными из папки локальных данных приложения. [Принажатиикнопкивыполняетсяобработчиксобытия.`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) `OnNoteAddedClicked` Этот метод переходит `NoteEntryPage`к, [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) присвоив параметру `NoteEntryPage` значение нового `Note` экземпляра. `ListView` Если`OnListViewItemSelected` выбран элемент в, выполняется обработчик события. Этот метод выполняет переход `NoteEntryPage`к, [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) устанавливая `NoteEntryPage` объект в выбранном `Note` экземпляре. Дополнительные сведения о навигации см. в разделе [Навигация](deepdive.md#navigation) в [кратком руководстве по Xamarin. Forms](deepdive.md).

    Сохраните изменения в **NotesPage.XAML.CS** , выбрав **Файл > Сохранить** (или нажав клавиши  **&#8984; + S**) и закройте файл.

    > [!WARNING]
    > Попытка создать приложение на этом этапе приведет к ошибкам, которые будут исправлены в последующих шагах.

13. В **панель решения**дважды щелкните **app.XAML.CS** , чтобы открыть его. Затем замените существующий код следующим кодом:

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class App : Application
        {
            public static string FolderPath { get; private set; }

            public App()
            {
                InitializeComponent();
                FolderPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData));
                MainPage = new NavigationPage(new NotesPage());
            }
            // ...
        }
    }
    ```

    Этот код добавляет объявление пространства имен для `System.IO` пространства имен и добавляет объявление для статического `FolderPath` свойства типа `string`. `FolderPath` Свойство используется для хранения пути на устройстве, где будут храниться данные о заметках. Кроме того, код инициализирует `FolderPath` свойство `App` в конструкторе и инициализирует [`MainPage`](xref:Xamarin.Forms.Application.MainPage) свойство [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) , `NotesPage`чтобы оно было размещено в экземпляре. Дополнительные сведения о навигации см. в разделе [Навигация](deepdive.md#navigation) в [кратком руководстве по Xamarin. Forms](deepdive.md).

    Сохраните изменения в файле **App.xaml.cs**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**, и закройте файл.

14. В **панель решения**в проекте **Notes** щелкните правой кнопкой мыши **MainPage. XAML**и выберите команду **Удалить**. В появившемся диалоговом окне нажмите кнопку **Удалить** , чтобы удалить файл с жесткого диска.

    При этом удаляется страница, которая больше не используется.

15. Создайте и запустите проект на каждой платформе. Дополнительные сведения см. [в разделе Создание краткого руководства](single-page.md#building-the-quickstart).

    В **нотеспаже** нажмите **+** кнопку, чтобы перейти к **нотинтрипаже** и ввести Примечание. После сохранения примечания приложение вернется обратно к **нотеспаже**.

    Введите число заметок, отличающихся длиной, для отслеживания поведения приложения.

::: zone-end

## <a name="next-steps"></a>Следующие шаги

В этом кратком руководстве вы узнали, как выполнять следующие задачи:

- Добавьте дополнительные страницы в решение Xamarin. Forms.
- Выполнить навигацию между страницами.
- Используйте привязку данных для синхронизации данных между элементами пользовательского интерфейса и их источником данных.

Чтобы изменить приложение таким образом, чтобы оно сохраняло свои данные в локальной базе данных SQLite.NET, перейдите к следующему краткому руководству.

> [!div class="nextstepaction"]
> [Вперед](database.md)

## <a name="related-links"></a>Связанные ссылки

- [Заметки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/)
- [Углубленное углубление в руководстве по Xamarin. Forms](deepdive.md)
