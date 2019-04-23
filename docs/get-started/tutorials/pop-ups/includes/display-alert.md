---
ms.openlocfilehash: cff466025306ee5d23b5245e625eba23c5448c15
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61384733"
---
В Xamarin.Forms есть модальный всплывающий элемент (оповещение), позволяющий выводить предупреждения или задавать простые вопросы пользователю. В этом упражнении вы воспользуетесь методом [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) из класса [`Page`](xref:Xamarin.Forms.Page), чтобы выводить оповещения для пользователя, а также задать простой вопрос.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Запустите Visual Studio и создайте пустое приложение Xamarin.Forms **PopupsTutorial**. Убедитесь, что в качестве механизма общего кода в приложении используется .NET Standard.

    > [!IMPORTANT]
    > Фрагменты кода на C# и XAML из этого руководства предполагают, что решение называется **PopupsTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms: глубокое погружение в обработку](~/get-started/first-app/index.md).

1. В **обозревателе решений** дважды щелкните файл **MainPage.xaml** в проекте **PopupsTutorial**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="PopupsTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Button Text="Display alert"
                    Clicked="OnDisplayAlertButtonClicked" />
            <Button Text="Display alert question"
                    Clicked="OnDisplayAlertQuestionButtonClicked" />
        </StackLayout>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из двух объектов [`Button`](xref:Xamarin.Forms.Button) в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Свойства [`Button.Text`](xref:Xamarin.Forms.Button.Text) указывают текст, отображаемый в каждом `Button`, а события [`Clicked`](xref:Xamarin.Forms.Button.Clicked) связаны с обработчиками событий, которые будут созданы в следующем шаге.

1. В **обозревателе решений** в проекте **PopupsTutorial** разверните узел **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его. Затем в **MainPage.xaml.cs** добавьте обработчики событий `OnDisplayAlertButtonClicked` и `OnDisplayAlertQuestionButtonClicked` в класс.

    ```csharp
    async void OnDisplayAlertButtonClicked(object sender, EventArgs e)
    {
        await DisplayAlert("Alert", "This is an alert.", "OK");
    }

    async void OnDisplayAlertQuestionButtonClicked(object sender, EventArgs e)
    {
        bool response = await DisplayAlert("Save?", "Would you like to save your data?", "Yes", "No");
        Console.WriteLine("Save data: " + response);
    }
    ```

    При касании [`Button`](xref:Xamarin.Forms.Button) выполняется метод обработчика соответствующего события. Метод `OnDisplayAlertButtonClicked` вызывает метод [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) для отображения модального оповещения с одной кнопкой "Отмена". После закрытия оповещения пользователь может продолжить работу с приложением.

    Метод `OnDisplayAlertQuestionButtonClicked` вызывает перегрузку метода [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) для отображения модального оповещения с кнопками "Принять" и "Отмена". После того как пользователь нажмет одну из кнопок, выбор возвращается в виде `boolean`.

    > [!IMPORTANT]
    > Метод [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) является асинхронным и всегда должен ожидаться с ключевым словом `await`.

1. На панели инструментов Visual Studio нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном удаленном симуляторе iOS или эмуляторе Android. Выберите первый [`Button`](xref:Xamarin.Forms.Button):

    [![Снимок экрана с оповещением в iOS и Android](../images/alert.png "Оповещение")](../images/alert-large.png#lightbox "Оповещение")

    После отклонения оповещения коснитесь второго [`Button`](xref:Xamarin.Forms.Button):

    [![Снимок экрана оповещения, где задается вопрос, на iOS и Android](../images/alert-question.png "Оповещение, где задается вопрос")](../images/alert-question-large.png#lightbox "Оповещение, где задается вопрос")

    Обратите внимание, что после выбора ответа на вопрос он поступает в окно **вывода** Visual Studio.

    Дополнительные сведения об отображении оповещений см. в разделе [Вывод оповещения](~/xamarin-forms/app-fundamentals/navigation/pop-ups.md#displaying-an-alert) в руководстве [Отображение всплывающих окон](~/xamarin-forms/app-fundamentals/navigation/pop-ups.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Запустите Visual Studio для Mac и создайте пустое приложение Xamarin.Forms **PopupsTutorial**. Убедитесь, что в качестве механизма общего кода в приложении используется .NET Standard.

    > [!IMPORTANT]
    > Фрагменты кода на C# и XAML из этого руководства предполагают, что решение называется **PopupsTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms: глубокое погружение в обработку](~/get-started/first-app/index.md).

1. На **Панели решения** дважды щелкните файл **MainPage.xaml** в проекте **PopupsTutorial**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="PopupsTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Button Text="Display alert"
                    Clicked="OnDisplayAlertButtonClicked" />
            <Button Text="Display alert question"
                    Clicked="OnDisplayAlertQuestionButtonClicked" />
        </StackLayout>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из двух объектов [`Button`](xref:Xamarin.Forms.Button) в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Свойства [`Button.Text`](xref:Xamarin.Forms.Button.Text) указывают текст, отображаемый в каждом `Button`, а события [`Clicked`](xref:Xamarin.Forms.Button.Clicked) связаны с обработчиками событий, которые будут созданы в следующем шаге.

1. На **Панели решения** в проекте **PopupsTutorial** разверните узел **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его. Затем в **MainPage.xaml.cs** добавьте обработчики событий `OnDisplayAlertButtonClicked` и `OnDisplayAlertQuestionButtonClicked` в класс.

    ```csharp
    async void OnDisplayAlertButtonClicked(object sender, EventArgs e)
    {
        await DisplayAlert("Alert", "This is an alert.", "OK");
    }

    async void OnDisplayAlertQuestionButtonClicked(object sender, EventArgs e)
    {
        bool response = await DisplayAlert("Save?", "Would you like to save your data?", "Yes", "No");
        Console.WriteLine("Save data: " + response);
    }
    ```

    При касании [`Button`](xref:Xamarin.Forms.Button) выполняется метод обработчика соответствующего события. Метод `OnDisplayAlertButtonClicked` вызывает метод [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) для отображения модального оповещения с одной кнопкой "Отмена". После закрытия оповещения пользователь может продолжить работу с приложением.

    Метод `OnDisplayAlertQuestionButtonClicked` вызывает перегрузку метода [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) для отображения модального оповещения с кнопками "Принять" и "Отмена". После того как пользователь нажмет одну из кнопок, выбор возвращается в виде `boolean`.

    > [!IMPORTANT]
    > Метод [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) является асинхронным и всегда должен ожидаться с ключевым словом `await`.

1. На панели инструментов Visual Studio для Mac нажмите клавишу **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android. Выберите первый [`Button`](xref:Xamarin.Forms.Button):

    [![Снимок экрана с оповещением в iOS и Android](../images/alert.png "Оповещение")](../images/alert-large.png#lightbox "Оповещение")

    После отклонения оповещения коснитесь второго [`Button`](xref:Xamarin.Forms.Button):

    [![Снимок экрана оповещения, где задается вопрос, на iOS и Android](../images/alert-question.png "Оповещение, где задается вопрос")](../images/alert-question-large.png#lightbox "Оповещение, где задается вопрос")

    Обратите внимание, что после выбора ответа на вопрос он поступает в окно **вывода** Visual Studio для Mac.

    Дополнительные сведения об отображении оповещений см. в разделе [Вывод оповещения](~/xamarin-forms/app-fundamentals/navigation/pop-ups.md#displaying-an-alert) в руководстве [Отображение всплывающих окон](~/xamarin-forms/app-fundamentals/navigation/pop-ups.md).
