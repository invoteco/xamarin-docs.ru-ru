---
ms.openlocfilehash: 87eb021e6cc571a9a5522697cde2aa11ee991308
ms.sourcegitcommit: 6ad272c2c7b0c3c30e375ad17ce6296ac1ce72b2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/23/2019
ms.locfileid: "66193822"
---

В Xamarin.Forms есть модальное всплывающее окно, известное как таблица действий, которое помогает пользователям выполнять различные задачи. В этом упражнении вы воспользуетесь методом [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) из класса [`Page`](xref:Xamarin.Forms.Page), чтобы отобразить таблицу действий, которая проводит пользователей через задачу.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** добавьте новое объявление [`Button`](xref:Xamarin.Forms.Button), которое будет отображать таблицу действий:

    ```xaml
    <Button Text="Display action sheet"
            Clicked="OnDisplayActionSheetButtonClicked" />
    ```

     Свойство [`Button.Text`](xref:Xamarin.Forms.Button.Text) указывает текст, отображаемый в `Button`. Кроме того, код задает для события [`Clicked`](xref:Xamarin.Forms.Button.Clicked) обработчик событий с именем `OnDisplayActionSheetButtonClicked`, который будет создан на следующем шаге.

1. В **обозревателе решений** в проекте **PopupsTutorial** разверните узел **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его. Затем в **MainPage.xaml.cs** добавьте обработчик событий `OnDisplayActionSheetButtonClicked` в класс:

    ```csharp
    async void OnDisplayActionSheetButtonClicked(object sender, EventArgs e)
    {
        string action = await DisplayActionSheet("Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
        Console.WriteLine("Action: " + action);
    }
    ```

    При касании [`Button`](xref:Xamarin.Forms.Button) выполняется метод `OnDisplayActionSheetButtonClicked`. Этот метод вызывает метод [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*), чтобы предоставить пользователю набор вариантов дальнейших действий в рамках задачи. После того как пользователь выберет один из вариантов, выбор возвращается в виде `string`.

    > [!IMPORTANT]
    > Метод [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) является асинхронным и всегда должен ожидаться с ключевым словом `await`.

1. На панели инструментов Visual Studio нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном удаленном симуляторе iOS или эмуляторе Android. Затем коснитесь пункта [`Button`](xref:Xamarin.Forms.Button), добавленного в [`ContentPage`](xref:Xamarin.Forms.ContentPage):

    [![Снимок экрана с таблицей действий в iOS и Android](../images/actionsheet.png "Таблица действий, которая помогает пользователям пройти задачу")](../images/actionsheet-large.png#lightbox "Таблица действий, которая помогает пользователям пройти задачу")

    Обратите внимание, что после выбора альтернативы в диалоговом окне таблицы действий выбор выводится в окне **вывода** Visual Studio.

    Дополнительные сведения об отображении таблицы действий см. в разделе [с инструкциями по задачам](~/xamarin-forms/user-interface/pop-ups.md#guide-users-through-tasks) в руководстве [по отображению всплывающих окон](~/xamarin-forms/user-interface/pop-ups.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** добавьте новое объявление [`Button`](xref:Xamarin.Forms.Button), которое будет отображать таблицу действий:

    ```xaml
    <Button Text="Display action sheet"
            Clicked="OnDisplayActionSheetButtonClicked" />
    ```

    Свойство [`Button.Text`](xref:Xamarin.Forms.Button.Text) указывает текст, отображаемый в `Button`. Кроме того, код задает для события [`Clicked`](xref:Xamarin.Forms.Button.Clicked) обработчик событий с именем `OnDisplayActionSheetButtonClicked`, который будет создан на следующем шаге.

1. На **Панели решения** в проекте **PopupsTutorial** разверните узел **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его. Затем в **MainPage.xaml.cs** добавьте обработчик событий `OnDisplayActionSheetButtonClicked` в класс:

    ```csharp
    async void OnDisplayActionSheetButtonClicked(object sender, EventArgs e)
    {
        string action = await DisplayActionSheet("Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
        Console.WriteLine("Action: " + action);
    }
    ```

    При касании [`Button`](xref:Xamarin.Forms.Button) выполняется метод `OnDisplayActionSheetButtonClicked`. Этот метод вызывает метод [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*), чтобы предоставить пользователю набор вариантов дальнейших действий в рамках задачи. После того как пользователь выберет один из вариантов, выбор возвращается в виде `string`.

    > [!IMPORTANT]
    > Метод [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) является асинхронным и всегда должен ожидаться с ключевым словом `await`.

1. На панели инструментов Visual Studio для Mac нажмите клавишу **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android. Затем коснитесь пункта [`Button`](xref:Xamarin.Forms.Button), добавленного в [`ContentPage`](xref:Xamarin.Forms.ContentPage):

    [![Снимок экрана с таблицей действий в iOS и Android](../images/actionsheet.png "Таблица действий, которая помогает пользователям пройти задачу")](../images/actionsheet-large.png#lightbox "Таблица действий, которая помогает пользователям пройти задачу")

    Обратите внимание, что после выбора альтернативы в диалоговом окне таблицы действий выбор выводится в окне **вывода** Visual Studio для Mac.

    Дополнительные сведения об отображении таблицы действий см. в разделе [с инструкциями по задачам](~/xamarin-forms/user-interface/pop-ups.md#guide-users-through-tasks) в руководстве [по отображению всплывающих окон](~/xamarin-forms/user-interface/pop-ups.md).
