# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** измените объявление [`Grid`](xref:Xamarin.Forms.Grid) для определения столбцов и строк и разместите содержимое в столбцах и строках:

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
        </Grid.RowDefinitions>
        <Label Grid.ColumnSpan="2"
               Text="This text uses the ColumnSpan property to span both columns." />
        <Label Grid.Row="1"
               Grid.RowSpan="2"
               Text="This text uses the RowSpan property to span two rows." />
    </Grid>
    ```

    Этот код определяет столбцы и строки для [`Grid`](xref:Xamarin.Forms.Grid) и располагает экземпляры [`Label`](xref:Xamarin.Forms.Label) в конкретных столбцах и строках. Первый `Label` задает вложенное свойство [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) так, чтобы его текст охватывал несколько столбцов. Свойство `ColumnSpan` имеет значение 2; оно задает количество столбцов, которые `Label` будет охватывать. Второй `Label` задает вложенное свойство [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) так, чтобы его текст охватывал несколько строк. Свойство `RowSpan` имеет значение 2; оно задает количество строк, которые `Label` будет охватывать.

1. На панели инструментов Visual Studio нажмите клавишу **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном удаленном симуляторе iOS или эмуляторе Android.

    [![Снимок экрана сетки с содержимым, разделенным по нескольким столбцам и строкам, в iOS и Android](../images/span-columns-rows.png "Сетка с содержимым, разделенным по нескольким столбцам и строкам")](../images/span-columns-rows-large.png#lightbox "Сетка с содержимым, разделенным по нескольким столбцам и строкам")

    Дополнительные сведения об охвате нескольких столбцов и строк см. в разделе [Диапазоны](~/xamarin-forms/user-interface/layouts/grid.md#spans) в руководстве [Сетка Xamarin.Forms](~/xamarin-forms/user-interface/layouts/grid.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** измените объявление [`Grid`](xref:Xamarin.Forms.Grid) для определения столбцов и строк и разместите содержимое в столбцах и строках:

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
        </Grid.RowDefinitions>
        <Label Grid.ColumnSpan="2"
               Text="This text uses the ColumnSpan property to span both columns." />
        <Label Grid.Row="1"
               Grid.RowSpan="2"
               Text="This text uses the RowSpan property to span two rows." />
    </Grid>
    ```

    Этот код определяет столбцы и строки для [`Grid`](xref:Xamarin.Forms.Grid) и располагает экземпляры [`Label`](xref:Xamarin.Forms.Label) в конкретных столбцах и строках. Первый `Label` задает вложенное свойство [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) так, чтобы его текст охватывал несколько столбцов. Свойство `ColumnSpan` имеет значение 2; оно задает количество столбцов, которые `Label` будет охватывать. Второй `Label` задает вложенное свойство [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) так, чтобы его текст охватывал несколько строк. Свойство `RowSpan` имеет значение 2; оно задает количество строк, которые `Label` будет охватывать.

1. На панели инструментов Visual Studio для Mac нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android:

    [![Снимок экрана сетки с содержимым, разделенным по нескольким столбцам и строкам, в iOS и Android](../images/span-columns-rows.png "Сетка с содержимым, разделенным по нескольким столбцам и строкам")](../images/span-columns-rows-large.png#lightbox "Сетка с содержимым, разделенным по нескольким столбцам и строкам")

    Дополнительные сведения об охвате нескольких столбцов и строк см. в разделе [Диапазоны](~/xamarin-forms/user-interface/layouts/grid.md#spans) в руководстве [Сетка Xamarin.Forms](~/xamarin-forms/user-interface/layouts/grid.md).
