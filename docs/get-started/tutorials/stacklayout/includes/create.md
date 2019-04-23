---
ms.openlocfilehash: a7b23e47269ecca4d36a344ce3589443a4a85e31
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61382555"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

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
