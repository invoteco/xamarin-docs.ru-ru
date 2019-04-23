---
ms.openlocfilehash: a7a5d51b8931aa9dffd2c16b4ead51371390f033
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61372990"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Запустите Visual Studio и создайте пустое приложение Xamarin.Forms **ButtonTutorial**. Убедитесь, что в качестве механизма общего кода в приложении используется .NET Standard.

    > [!IMPORTANT]
    > Фрагменты кода на C# и XAML из этого руководства предполагают, что решение называется **ButtonTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms: глубокое погружение в обработку](~/get-started/first-app/index.md).

1. В **обозревателе решений** дважды щелкните файл **MainPage.xaml** в проекте **ButtonTutorial**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>    
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ButtonTutorial.MainPage">
       <StackLayout Margin="20,35,20,20">
           <Button Text="Click me" />
       </StackLayout>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`Button`](xref:Xamarin.Forms.Button) в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Свойство [`Button.Text`](xref:Xamarin.Forms.Button.Text) указывает текст, отображаемый в `Button`.

1. На панели инструментов Visual Studio нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS удаленной работы или эмуляторе Android:

    [![Снимок экрана: кнопка на iOS и Android](../images/create-button.png "Кнопка, содержащая текст")](../images/create-button-large.png#lightbox "Кнопка, содержащая текст")

    Обратите внимание, что по умолчанию [`Button`](xref:Xamarin.Forms.Button) обычно занимает все выделенное для него пространство — в этом случае полную ширину своего родительского элемента ([`StackLayout`](xref:Xamarin.Forms.StackLayout)).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Запустите Visual Studio для Mac и создайте пустое приложение Xamarin.Forms **ButtonTutorial**. Убедитесь, что в качестве механизма общего кода в приложении используется .NET Standard.

    > [!IMPORTANT]
    > Фрагменты кода на C# и XAML из этого руководства предполагают, что решение называется **ButtonTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms: глубокое погружение в обработку](~/get-started/first-app/index.md).

1. На **панели решений** дважды щелкните файл **MainPage.xaml** в проекте **ButtonTutorial**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ButtonTutorial.MainPage">
       <StackLayout Margin="20,35,20,20">
           <Button Text="Click me" />
       </StackLayout>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`Button`](xref:Xamarin.Forms.Button) в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Свойство [`Button.Text`](xref:Xamarin.Forms.Button.Text) указывает текст, отображаемый в `Button`.

1. На панели инструментов Visual Studio для Mac нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android:

    [![Снимок экрана: кнопка на iOS и Android](../images/create-button.png "Кнопка, содержащая текст")](../images/create-button-large.png#lightbox "Кнопка, содержащая текст")

    Обратите внимание, что по умолчанию [`Button`](xref:Xamarin.Forms.Button) обычно занимает все выделенное для него пространство — в этом случае полную ширину своего родительского элемента ([`StackLayout`](xref:Xamarin.Forms.StackLayout)).
