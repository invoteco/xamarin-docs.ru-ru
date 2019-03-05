# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** измените объявление [`Button`](xref:Xamarin.Forms.Button) таким образом, чтобы он установил обработчик для события [`Clicked`](xref:Xamarin.Forms.Button.Clicked):

    ```xaml
    <Button Text="Click me"
            Clicked="OnButtonClicked" />
    ```

    Этот код задает событие [`Clicked`](xref:Xamarin.Forms.Button.Clicked) для обработчика событий с именем `OnButtonClicked`, который будет создан на следующем шаге.

1. В **обозревателе решений** в проекте **ButtonTutorial** разверните узел **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его. Затем в **MainPage.xaml.cs** добавьте обработчик событий `OnButtonClicked` в класс:

    ```csharp
    void OnButtonClicked(object sender, EventArgs e)
    {
        (sender as Button).Text = "Click me again!";
    }
    ```

    При касании [`Button`](xref:Xamarin.Forms.Button) выполняется метод `OnButtonClicked`. Аргумент `sender` является объектом `Button`, ответственным за запуск события `Clicked`, и может использоваться для доступа к объекту `Button`. Этот обработчик обновляет текст, отображаемый элементом `Button`.

    > [!NOTE]
    > Помимо события `Clicked` `Button` также определяет события [`Pressed`](xref:Xamarin.Forms.Button.Pressed) и [`Released`](xref:Xamarin.Forms.Button.Released). Дополнительные сведения см. в разделе [Нажатие и отпускание кнопки](~/xamarin-forms/user-interface/button.md#pressing-and-releasing-the-button) в руководстве по [кнопкам в Xamarin.Forms](~/xamarin-forms/user-interface/button.md).

1. На панели инструментов Visual Studio нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном удаленном симуляторе iOS или эмуляторе Android. Нажмите кнопку [`Button`](xref:Xamarin.Forms.Button) и обратите внимание, что ее текст меняется:

    [![Снимок экрана: текст кнопки меняется после щелчка в iOS и Android](../images/handle-button-click.png "Обработка нажатия кнопки")](../images/handle-button-click-large.png#lightbox "Обработка нажатия кнопки")

    Дополнительные сведения об обработке нажатий кнопок см. в разделе [Обработка нажатия кнопки](~/xamarin-forms/user-interface/button.md#handling-button-clicks) в руководстве по [кнопкам в Xamarin.Forms](~/xamarin-forms/user-interface/button.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** измените объявление [`Button`](xref:Xamarin.Forms.Button) таким образом, чтобы он установил обработчик для события [`Clicked`](xref:Xamarin.Forms.Button.Clicked):

    ```xaml
    <Button Text="Click me"
            Clicked="OnButtonClicked" />
    ```

    Этот код задает событие [`Clicked`](xref:Xamarin.Forms.Button.Clicked) для обработчика событий с именем `OnButtonClicked`, который будет создан на следующем шаге.

1. На **панели решений** в проекте **ButtonTutorial** разверните узел **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его. Затем в **MainPage.xaml.cs** добавьте обработчик событий `OnButtonClicked` в класс:

    ```csharp
    void OnButtonClicked(object sender, EventArgs e)
    {
        (sender as Button).Text = "Click me again!";
    }
    ```

    При касании [`Button`](xref:Xamarin.Forms.Button) выполняется метод `OnButtonClicked`. Аргумент `sender` является объектом `Button`, ответственным за запуск события `Clicked`, и может использоваться для доступа к объекту `Button`. Этот обработчик обновляет текст, отображаемый элементом `Button`.

    > [!NOTE]
    > Помимо события `Clicked` `Button` также определяет события [`Pressed`](xref:Xamarin.Forms.Button.Pressed) и [`Released`](xref:Xamarin.Forms.Button.Released). Дополнительные сведения см. в разделе [Нажатие и отпускание кнопки](~/xamarin-forms/user-interface/button.md#pressing-and-releasing-the-button) в руководстве по [кнопкам в Xamarin.Forms](~/xamarin-forms/user-interface/button.md).

1. На панели инструментов Visual Studio для Mac нажмите клавишу **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android. Нажмите кнопку [`Button`](xref:Xamarin.Forms.Button) и обратите внимание, что ее текст меняется:

    [![Снимок экрана: текст кнопки меняется после щелчка в iOS и Android](../images/handle-button-click.png "Обработка нажатия кнопки")](../images/handle-button-click-large.png#lightbox "Обработка нажатия кнопки")

    Дополнительные сведения об обработке нажатий кнопок см. в разделе [Обработка нажатия кнопки](~/xamarin-forms/user-interface/button.md#handling-button-clicks) в руководстве по [кнопкам в Xamarin.Forms](~/xamarin-forms/user-interface/button.md).
