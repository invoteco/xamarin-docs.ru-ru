# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Запустите Visual Studio и создайте пустое приложение Xamarin.Forms **LabelTutorial**. Убедитесь, что в качестве механизма общего кода в приложении используется .NET Standard.

    > [!IMPORTANT]
    > Фрагменты кода на C# и XAML из этого руководства предполагают, что решение называется **LabelTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms: глубокое погружение в обработку](~/get-started/first-app/index.md).

1. В **обозревателе решений** дважды щелкните файл **MainPage.xaml** в проекте **LabelTutorial**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="LabelTutorial.MainPage">
       <StackLayout Margin="20,35,20,20">
            <Label Text="Welcome to Xamarin.Forms!"
                   HorizontalOptions="Center" />
       </StackLayout>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`Label`](xref:Xamarin.Forms.Label) в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Свойство [`Label.Text`](xref:Xamarin.Forms.Button.Text) определяет текст, который отображается, а свойство [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) указывает, что текст будет выравниваться по горизонтали по центру.

1. На панели инструментов Visual Studio нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS удаленной работы или эмуляторе Android:

    [![Снимок экрана метки, выровненной по центру, в iOS и Android](../images/create-label.png "Метка по центру")](../images/create-label-large.png#lightbox "Метка по центру")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Запустите Visual Studio для Mac и создайте пустое приложение Xamarin.Forms **LabelTutorial**. Убедитесь, что в качестве механизма общего кода в приложении используется .NET Standard.

    > [!IMPORTANT]
    > Фрагменты кода на C# и XAML из этого руководства предполагают, что решение называется **LabelTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms: глубокое погружение в обработку](~/get-started/first-app/index.md).

1. На **Панели решения** дважды щелкните файл **MainPage.xaml** в проекте **LabelTutorial**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="LabelTutorial.MainPage">
       <StackLayout Margin="20,35,20,20">
            <Label Text="Welcome to Xamarin.Forms!"
                   HorizontalOptions="Center" />
       </StackLayout>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`Label`](xref:Xamarin.Forms.Label) в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Свойство [`Label.Text`](xref:Xamarin.Forms.Button.Text) определяет текст, который отображается, а свойство [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) указывает, что текст будет выравниваться по горизонтали по центру.

1. На панели инструментов Visual Studio для Mac нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android:

    [![Снимок экрана метки, выровненной по центру, в iOS и Android](../images/create-label.png "Метка по центру")](../images/create-label-large.png#lightbox "Метка по центру")
