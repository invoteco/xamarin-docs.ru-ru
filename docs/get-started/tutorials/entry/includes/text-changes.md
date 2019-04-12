# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** измените объявление [`Entry`](xref:Xamarin.Forms.Entry) таким образом, чтобы он установил обработчик для событий [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) и [`Completed`](xref:Xamarin.Forms.Entry.Completed).

    ```xaml
    <Entry Placeholder="Enter text"
           TextChanged="OnEntryTextChanged"
           Completed="OnEntryCompleted" />
    ```

    Этот код устанавливает событие [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) для обработчика событий `OnEntryTextChanged`, а [`Completed`](xref:Xamarin.Forms.Entry.Completed) — для обработчика `OnEntryCompleted`. Оба обработчика событий будут созданы на следующем шаге.

1. В **обозревателе решений** в проекте **LabelTutorial** разверните узел **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его. Затем в **MainPage.xaml.cs** добавьте обработчики событий `OnEntryTextChanged` и `OnEntryCompleted` в класс.

    ```csharp
    void OnEntryTextChanged(object sender, TextChangedEventArgs e)
    {
        string oldText = e.OldTextValue;
        string newText = e.NewTextValue;
    }

    void OnEntryCompleted(object sender, EventArgs e)
    {
        string text = ((Entry)sender).Text;
    }
    ```

    Если текст в [`Entry`](xref:Xamarin.Forms.Entry) изменяется, выполняется метод `OnEntryTextChanged`. Аргумент `sender` является объектом `Entry`, ответственным за запуск события `TextChanged`, и может использоваться для доступа к объекту `Entry`. Аргумент [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) содержит старые и новые текстовые значения (до и после изменения текста).

    После того как текст в [`Entry`](xref:Xamarin.Forms.Entry) завершается нажатием клавиши ВВОД, выполняется метод `OnEntryCompleted`. Аргумент `sender` является объектом `Entry`, ответственным за запуск события `TextChanged`, и может использоваться для доступа к объекту `Entry`.

    > [!IMPORTANT]
    > Любой текст, введенный в [`Entry`](xref:Xamarin.Forms.Entry), хранится в свойстве [`Text`](xref:Xamarin.Forms.Entry.Text).

1. На панели инструментов Visual Studio нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS удаленной работы или эмуляторе Android:

    [![Снимок экрана записи, содержащей текст, в iOS и Android](../images/text-changes.png "Запись с текстом")](../images/text-changes-large.png#lightbox "Запись с текстом")

    Установите точки останова в двух обработчиках событий, введите текст в [`Entry`](xref:Xamarin.Forms.Entry) и просмотрите запуск событий [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) и [`Completed`](xref:Xamarin.Forms.Entry.Completed).

    Дополнительные сведения о событиях [`Entry`](xref:Xamarin.Forms.Entry) см. в разделе [События и интерактивность](~/xamarin-forms/user-interface/text/entry.md#events-and-interactivity) в руководстве [Запись Xamarin.Forms](~/xamarin-forms/user-interface/text/entry.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** измените объявление [`Entry`](xref:Xamarin.Forms.Entry) таким образом, чтобы он установил обработчик для событий [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) и [`Completed`](xref:Xamarin.Forms.Entry.Completed).

    ```xaml
    <Entry Placeholder="Enter text"
           TextChanged="OnEntryTextChanged"
           Completed="OnEntryCompleted" />
    ```

    Этот код устанавливает событие [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) для обработчика событий `OnEntryTextChanged`, а [`Completed`](xref:Xamarin.Forms.Entry.Completed) — для обработчика `OnEntryCompleted`. Оба обработчика событий будут созданы на следующем шаге.

1. На **Панели решения** в проекте **LabelTutorial** разверните узел **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его. Затем в **MainPage.xaml.cs** добавьте обработчики событий `OnEntryTextChanged` и `OnEntryCompleted` в класс.

    ```csharp
    void OnEntryTextChanged(object sender, TextChangedEventArgs e)
    {
        string oldText = e.OldTextValue;
        string newText = e.NewTextValue;
    }

    void OnEntryCompleted(object sender, EventArgs e)
    {
        string text = ((Entry)sender).Text;
    }
    ```

    Если текст в [`Entry`](xref:Xamarin.Forms.Entry) изменяется, выполняется метод `OnEntryTextChanged`. Аргумент `sender` является объектом `Entry`, ответственным за запуск события `TextChanged`, и может использоваться для доступа к объекту `Entry`. Аргумент [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) содержит старые и новые текстовые значения (до и после изменения текста).

    После того как текст в [`Entry`](xref:Xamarin.Forms.Entry) завершается нажатием клавиши ВВОД, выполняется метод `OnEntryCompleted`. Аргумент `sender` является объектом `Entry`, ответственным за запуск события `TextChanged`, и может использоваться для доступа к объекту `Entry`.

    > [!IMPORTANT]
    > Любой текст, введенный в [`Entry`](xref:Xamarin.Forms.Entry), хранится в свойстве [`Text`](xref:Xamarin.Forms.Entry.Text).

1. На панели инструментов Visual Studio для Mac нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android:

    [![Снимок экрана записи, содержащей текст, в iOS и Android](../images/text-changes.png "Запись с текстом")](../images/text-changes-large.png#lightbox "Запись с текстом")

    Установите точки останова в двух обработчиках событий, введите текст в [`Entry`](xref:Xamarin.Forms.Entry) и просмотрите запуск событий [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) и [`Completed`](xref:Xamarin.Forms.Entry.Completed).

    Дополнительные сведения о событиях [`Entry`](xref:Xamarin.Forms.Entry) см. в разделе [События и интерактивность](~/xamarin-forms/user-interface/text/entry.md#events-and-interactivity) в руководстве [Запись Xamarin.Forms](~/xamarin-forms/user-interface/text/entry.md).