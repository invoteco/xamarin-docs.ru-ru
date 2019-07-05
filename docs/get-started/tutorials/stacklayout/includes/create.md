---
ms.openlocfilehash: 4ea10a3cc4eca0708d1de1aee68bed74c960cbc2
ms.sourcegitcommit: 8ecfa339d0f3e7687977bfe4fc96448942690183
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2019
ms.locfileid: "67560018"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Для работы с этим руководством у вас должен быть последний выпуск Visual Studio 2019 с установленной рабочей нагрузкой **Разработка мобильных приложений на .NET**. Кроме того, вам потребуется компьютер Mac для сборки учебного приложения на iOS. Сведения об установке платформы Xamarin см. в статье [Установка Xamarin](~/get-started/installation/index.md). Сведения о подключении Visual Studio 2019 к узлу сборки Mac см. в статье [Связывание с Mac при разработке для Xamarin.iOS](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Запустите Visual Studio и создайте пустое приложение Xamarin.Forms **StackLayoutTutorial**. Убедитесь, что в качестве механизма общего кода в приложении используется .NET Standard.

    > [!IMPORTANT]
    > Для фрагментов кода на C# и XAML в этом руководстве необходимо решение с именем **StackLayoutTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms глубокое погружение в обработку](~/get-started/first-app/index.md).

1. В **обозревателе решений** в проекте **StackLayoutTutorial** дважды щелкните файл **MainPage.xaml**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="StackLayoutTutorial.MainPage">
        <StackLayout Margin="20,35,20,25">
            <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
            <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
            <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
        </StackLayout>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из трех экземпляров [`Label`](xref:Xamarin.Forms.Label) в [`StackLayout`](xref:Xamarin.Forms.StackLayout). [`StackLayout`](xref:Xamarin.Forms.StackLayout) размещает свои дочерние представления (экземпляры `Label`) в одну строку, которая по умолчанию ориентирована вертикально. Кроме того свойство [`Margin`](xref:Xamarin.Forms.View.Margin) указывает позицию отрисовки `StackLayout` в [`ContentPage`](xref:Xamarin.Forms.ContentPage).

    > [!NOTE]
    > В дополнение к свойству [`Margin`](xref:Xamarin.Forms.View.Margin), свойства [`Padding`](xref:Xamarin.Forms.Layout.Padding) и [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) также могут быть установлены на [`StackLayout`](xref:Xamarin.Forms.StackLayout). Значение свойства [`Padding`](xref:Xamarin.Forms.Layout.Padding) задает расстояние между представлениями в `StackLayout`, а значение свойства [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) задает объем пространства между каждым дочерним элементом в `StackLayout`. Дополнительные сведения см. в статье [Поля и заполнение](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

1. На панели инструментов Visual Studio нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном удаленном симуляторе iOS или эмуляторе Android.

    [![Снимок экрана дочерних представлений в StackLayout на iOS и Android](../images/create-stacklayout.png "StackLayout, содержащая экземпляры Label")](../images/create-stacklayout-large.png#lightbox "StackLayout, содержащая экземпляры Label")

    Дополнительные сведения о [`StackLayout`](xref:Xamarin.Forms.StackLayout) см. в статье [Xamarin.Forms StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Для работы с этим руководством вам нужно установить Visual Studio для Mac (последний выпуск) с поддержкой платформ Android и iOS. Кроме того, вам потребуется Xcode (последний выпуск). Дополнительные сведения об установке платформы Xamarin см. в статье [Установка Xamarin](~/get-started/installation/index.md).

1. Запустите Visual Studio для Mac и создайте пустое приложение Xamarin.Forms **StackLayoutTutorial**. Убедитесь, что в качестве механизма общего кода в приложении используется .NET Standard.

    > [!IMPORTANT]
    > Для фрагментов кода на C# и XAML в этом руководстве необходимо решение с именем **StackLayoutTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms глубокое погружение в обработку](~/get-started/first-app/index.md).

1. На **Панели решений** в проекте **StackLayoutTutorial** дважды щелкните файл **MainPage.xaml**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="StackLayoutTutorial.MainPage">
        <StackLayout Margin="20,35,20,25">
            <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
            <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
            <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
        </StackLayout>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из трех экземпляров [`Label`](xref:Xamarin.Forms.Label) в [`StackLayout`](xref:Xamarin.Forms.StackLayout). [`StackLayout`](xref:Xamarin.Forms.StackLayout) размещает свои дочерние представления (экземпляры `Label`) в одну строку, которая по умолчанию ориентирована вертикально. Кроме того свойство [`Margin`](xref:Xamarin.Forms.View.Margin) указывает позицию отрисовки `StackLayout` в [`ContentPage`](xref:Xamarin.Forms.ContentPage).

    > [!NOTE]
    > В дополнение к свойству [`Margin`](xref:Xamarin.Forms.View.Margin), свойства [`Padding`](xref:Xamarin.Forms.Layout.Padding) и [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) также могут быть установлены на [`StackLayout`](xref:Xamarin.Forms.StackLayout). Значение свойства [`Padding`](xref:Xamarin.Forms.Layout.Padding) задает расстояние между представлениями в `StackLayout`, а значение свойства [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) задает объем пространства между каждым дочерним элементом в `StackLayout`. Дополнительные сведения см. в статье [Поля и заполнение](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

1. На панели инструментов Visual Studio для Mac нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android.

    [![Снимок экрана дочерних представлений в StackLayout на iOS и Android](../images/create-stacklayout.png "StackLayout, содержащая экземпляры Label")](../images/create-stacklayout-large.png#lightbox "StackLayout, содержащая экземпляры Label")

    Дополнительные сведения о [`StackLayout`](xref:Xamarin.Forms.StackLayout) см. в статье [Xamarin.Forms StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md).

-----

> [!div class="nextstepaction"]
> [У меня возникла проблема](https://github.com/MicrosoftDocs/xamarin-docs/issues/new?title=StackLayout+Tutorial+Step+1+Feedback&template=tutorial_template.md)
