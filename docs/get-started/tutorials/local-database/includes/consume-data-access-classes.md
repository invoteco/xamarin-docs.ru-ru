В этом упражнении будет создан пользовательский интерфейс, который используется для работы с ранее созданными классами доступа к данным.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. В **обозревателе решений** в проекте **LocalDatabaseTutorial** дважды щелкните **MainPage.xaml**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="LocalDatabaseTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry x:Name="nameEntry"
                   Placeholder="Enter name" />
            <Entry x:Name="ageEntry"
                   Placeholder="Enter age" />
            <Button Text="Add to Database"
                    Clicked="OnButtonClicked" />
            <ListView x:Name="listView">
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <TextCell Text="{Binding Name}"
                                  Detail="{Binding Age}" />
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </StackLayout>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из двух экземпляров [`Entry`](xref:Xamarin.Forms.Entry) ([`Button`](xref:Xamarin.Forms.Button) и [`ListView`](xref:Xamarin.Forms.ListView)) в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Каждый экземпляр `Entry` обладает собственным набором свойств [`Placeholder`](xref:Xamarin.Forms.Entry.Placeholder), которые указывают замещающий текст, который отображается перед вводом данных пользователем. `Button` устанавливает событие [`Clicked`](xref:Xamarin.Forms.Button.Clicked) для обработчика событий `OnButtonClicked`, который будет создан на следующем шаге. `ListView` устанавливает свойство [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) для [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), которое использует [`TextCell`](xref:Xamarin.Forms.TextCell) для определения внешнего вида каждой строки в [`ListView`](xref:Xamarin.Forms.ListView). Данные `TextCell` привязывают свойства [`Text`](xref:Xamarin.Forms.TextCell.Text) и [`Detail`](xref:Xamarin.Forms.TextCell.Detail) к свойствам `Name` и `Age` каждого из объектов `Person` соответственно.

    Кроме этого экземпляры [`Entry`](xref:Xamarin.Forms.Entry) и [`ListView`](xref:Xamarin.Forms.ListView) обладают именами, указанными в атрибуте `x:Name`. Это позволяет файлу с выделенным кодом получать доступ к этим объектам с помощью назначенных имен.

1. В **обозревателе решений** в проекте **LocalDatabaseTutorial** разверните узел **MainPage.xaml** и дважды щелкните **MainPage.xaml.cs**, чтобы открыть его. Затем в **MainPage.xaml.cs** добавьте переопределение `OnAppearing` и обработчик событий `OnButtonClicked` в класс.

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();
        listView.ItemsSource = await App.Database.GetPeopleAsync();
    }

    async void OnButtonClicked(object sender, EventArgs e)
    {
        if (!string.IsNullOrWhiteSpace(nameEntry.Text) && !string.IsNullOrWhiteSpace(ageEntry.Text))
        {
            await App.Database.SavePersonAsync(new Person
            {
                Name = nameEntry.Text,
                Age = int.Parse(ageEntry.Text)
            });

            nameEntry.Text = ageEntry.Text = string.Empty;
            listView.ItemsSource = await App.Database.GetPeopleAsync();
        }
    }
    ```

    Метод `OnAppearing` заполняет [`ListView`](xref:Xamarin.Forms.ListView) любыми данными, хранящимися в базе данных. Метод `OnButtonClicked`, который выполняется при нажатии [`Button`](xref:Xamarin.Forms.Button), сохраняет введенные данные в базу данных перед очисткой обоих экземпляров [`Entry`](xref:Xamarin.Forms.Entry) и обновлением данных в `ListView`.

    > [!NOTE]
    > Переопределение метода `OnAppearing` выполняется после готовности [`ContentPage`](xref:Xamarin.Forms.ContentPage), но только прежде чем он станет видимым. Таким образом лучше всего задать содержимое представлений Xamarin.Forms.

1. На панели инструментов Visual Studio нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном удаленном симуляторе iOS или эмуляторе Android.

    Введите несколько элементов данных нажав [`Button`](xref:Xamarin.Forms.Button) для каждого из них. Это действие позволит сохранить данные в базу данных и повторно заполнить [`ListView`](xref:Xamarin.Forms.ListView) всеми данными базы данных.

    [![Снимок экрана сохраняемости данных SQLite.NET локальной базы данных на iOS и Android](../images/consume-data-access-classes.png "Сохраняемость данных локальной базы данных")](../images/consume-data-access-classes-large.png#lightbox "Сохраняемость данных локальной базы данных")

    Дополнительные сведения о локальных базах данных в Xamarin.Forms см. в статье [Локальные базы данных Xamarin.Forms (руководство)](~/xamarin-forms/app-fundamentals/databases.md)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. На **Панели решения** в проекте **LocalDatabaseTutorial** дважды щелкните **MainPage.xaml**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="LocalDatabaseTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry x:Name="nameEntry"
                   Placeholder="Enter name" />
            <Entry x:Name="ageEntry"
                   Placeholder="Enter age" />
            <Button Text="Add to Database"
                    Clicked="OnButtonClicked" />
            <ListView x:Name="listView">
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <TextCell Text="{Binding Name}"
                                  Detail="{Binding Age}" />
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </StackLayout>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из двух экземпляров [`Entry`](xref:Xamarin.Forms.Entry) ([`Button`](xref:Xamarin.Forms.Button) и [`ListView`](xref:Xamarin.Forms.ListView)) в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Каждый экземпляр `Entry` обладает собственным набором свойств [`Placeholder`](xref:Xamarin.Forms.Entry.Placeholder), которые указывают замещающий текст, который отображается перед вводом данных пользователем. `Button` устанавливает событие [`Clicked`](xref:Xamarin.Forms.Button.Clicked) для обработчика событий `OnButtonClicked`, который будет создан на следующем шаге. `ListView` устанавливает свойство [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) для [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), которое использует [`TextCell`](xref:Xamarin.Forms.TextCell) для определения внешнего вида каждой строки в [`ListView`](xref:Xamarin.Forms.ListView). Данные `TextCell` привязывают свойства [`Text`](xref:Xamarin.Forms.TextCell.Text) и [`Detail`](xref:Xamarin.Forms.TextCell.Detail) к свойствам `Name` и `Age` каждого из объектов `Person` соответственно.

    Кроме этого экземпляры [`Entry`](xref:Xamarin.Forms.Entry) и [`ListView`](xref:Xamarin.Forms.ListView) обладают именами, указанными в атрибуте `x:Name`. Это позволяет файлу с выделенным кодом получать доступ к этим объектам с помощью назначенных имен.

1. На **Панели решения** в проекте **LocalDatabaseTutorial** разверните узел **MainPage.xaml** и дважды щелкните **MainPage.xaml.cs**, чтобы открыть его. Затем в **MainPage.xaml.cs** добавьте переопределение `OnAppearing` и обработчик событий `OnButtonClicked` в класс.

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();
        listView.ItemsSource = await App.Database.GetPeopleAsync();
    }

    async void OnButtonClicked(object sender, EventArgs e)
    {
        if (!string.IsNullOrWhiteSpace(nameEntry.Text) && !string.IsNullOrWhiteSpace(ageEntry.Text))
        {
            await App.Database.SavePersonAsync(new Person
            {
                Name = nameEntry.Text,
                Age = int.Parse(ageEntry.Text)
            });

            nameEntry.Text = ageEntry.Text = string.Empty;
            listView.ItemsSource = await App.Database.GetPeopleAsync();
        }
    }
    ```

    Метод `OnAppearing` заполняет [`ListView`](xref:Xamarin.Forms.ListView) любыми данными, хранящимися в базе данных. Метод `OnButtonClicked`, который выполняется при нажатии [`Button`](xref:Xamarin.Forms.Button), сохраняет введенные данные в базу данных перед очисткой обоих экземпляров [`Entry`](xref:Xamarin.Forms.Entry) и обновлением данных в `ListView`.

    > [!NOTE]
    > Переопределение метода `OnAppearing` выполняется после готовности [`ContentPage`](xref:Xamarin.Forms.ContentPage), но только прежде чем он станет видимым. Таким образом лучше всего задать содержимое представлений Xamarin.Forms.

1. Чтобы запустить приложения в выбранном симуляторе iOS или эмуляторе Android, нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения) на панели инструментов Visual Studio для Mac.

    Введите несколько элементов данных нажав [`Button`](xref:Xamarin.Forms.Button) для каждого из них. Это действие позволит сохранить данные в базу данных и повторно заполнить [`ListView`](xref:Xamarin.Forms.ListView) всеми данными базы данных.

    [![Снимок экрана сохраняемости данных SQLite.NET локальной базы данных на iOS и Android](../images/consume-data-access-classes.png "Сохраняемость данных локальной базы данных")](../images/consume-data-access-classes-large.png#lightbox "Сохраняемость данных локальной базы данных")

    Дополнительные сведения о локальных базах данных в Xamarin.Forms см. в статье [Локальные базы данных Xamarin.Forms (руководство)](~/xamarin-forms/app-fundamentals/databases.md)
