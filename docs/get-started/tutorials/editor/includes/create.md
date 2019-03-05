# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Запустите Visual Studio и создайте пустое приложение Xamarin.Forms **EditorTutorial**. Убедитесь, что в качестве механизма общего кода в приложении используется .NET Standard.

    > [!IMPORTANT]
    > Для фрагментов кода на C# и XAML в этом руководстве необходимо решение с именем **EditorTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms глубокое погружение в обработку](~/get-started/first-app/index.md).

1. В **обозревателе решений** дважды щелкните файл **MainPage.xaml** в проекте **EditorTutorial**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="EditorTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Editor Placeholder="Enter multi-line text here"
                    HeightRequest="200" />
        </StackLayout>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`Editor`](xref:Xamarin.Forms.Editor) в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Свойство [`Editor.Placeholder`](xref:Xamarin.Forms.Editor.Placeholder) определяет текст заполнителя который отображается, при первом появлении `Editor`. Кроме того, свойство [`HeightRequest`](xref:Xamarin.Forms.VisualElement) указывает высоту `Editor` в аппаратно-независимых единицах.

1. На панели инструментов Visual Studio нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном удаленном симуляторе iOS или эмуляторе Android.

    [![Снимок экрана редактора в iOS и Android](../images/create-editor.png "Редактор содержащий замещающий текст")](../images/create-editor-large.png#lightbox "Редактор содержащий замещающий текст")

    > [!NOTE]
    > В то время, как в Android существует функция, которая указывает высоту [`Editor`](xref:Xamarin.Forms.Editor), в iOS такая функция отсутствует.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Запустите Visual Studio для Mac и создайте пустое приложение Xamarin.Forms **EditorTutorial**. Убедитесь, что в качестве механизма общего кода в приложении используется .NET Standard.

    > [!IMPORTANT]
    > Для фрагментов кода на C# и XAML в этом руководстве необходимо решение с именем **EditorTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms глубокое погружение в обработку](~/get-started/first-app/index.md).

1. На **Панели решения** дважды щелкните файл **MainPage.xaml** в проекте **EditorTutorial**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="EditorTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Editor Placeholder="Enter multi-line text here"
                    HeightRequest="200" />
        </StackLayout>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`Editor`](xref:Xamarin.Forms.Editor) в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Свойство [`Editor.Placeholder`](xref:Xamarin.Forms.Editor.Placeholder) определяет текст заполнителя который отображается, при первом появлении `Editor`. Кроме того, свойство [`HeightRequest`](xref:Xamarin.Forms.VisualElement) указывает высоту `Editor` в аппаратно-независимых единицах.

1. На панели инструментов Visual Studio для Mac нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android.

    [![Снимок экрана редактора в iOS и Android](../images/create-editor.png "Редактор содержащий замещающий текст")](../images/create-editor-large.png#lightbox "Редактор содержащий замещающий текст")

    > [!NOTE]
    > В то время, как в Android существует функция, которая указывает высоту [`Editor`](xref:Xamarin.Forms.Editor), в iOS такая функция отсутствует.
