---
ms.openlocfilehash: 841dac9486097e27923ccfe582803b4ec50371cf
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2020
ms.locfileid: "60896722"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** измените объявление [`Label`](xref:Xamarin.Forms.Label), чтобы представить текст, который использует несколько форматов, в одном `Label`.

    ```xaml
    <Label TextColor="Gray"
           FontSize="Medium">
        <Label.FormattedText>
            <FormattedString>
                <Span Text="This sentence contains " />
                <Span Text="words that are emphasized, "
                      FontAttributes="Italic" />
                <Span Text="and underlined."
                      TextDecorations="Underline" />
            </FormattedString>
        </Label.FormattedText>
    </Label>
    ```

    Этот код отображает текст в одном [`Label`](xref:Xamarin.Forms.Label), который использует несколько форматов. Текст в первом [`Span`](xref:Xamarin.Forms.Span) отображается с форматированием, заданным в `Label`, а текст во втором и третьем экземплярах `Span` отображается с форматированием, заданным в `Label`, и дополнительным форматированием, заданным в каждом `Span`.

    > [!NOTE]
    > Свойство [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) имеет тип [`FormattedString`](xref:Xamarin.Forms.FormattedString), который состоит из одного или нескольких экземпляров [`Span`](xref:Xamarin.Forms.Span).

1. На панели инструментов Visual Studio нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном удаленном симуляторе iOS или эмуляторе Android. Обратите внимание, что внешний вид [`Label`](xref:Xamarin.Forms.Label) изменился:

    [![Снимок экрана: метка, отображающая форматированный текст, в iOS и Android](../images/label-formatted-text.png "Метка с форматированным текстом")](../images/label-formatted-text-large.png#lightbox "Метка с форматированным текстом")

    Дополнительные сведения о внешнем виде параметра [`Span`](xref:Xamarin.Forms.Span) см. в разделе [Форматированный текст](~/xamarin-forms/user-interface/text/label.md#formatted-text) в руководстве по [меткам в Xamarin.Forms](~/xamarin-forms/user-interface/text/label.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** измените объявление [`Label`](xref:Xamarin.Forms.Label), чтобы представить текст, который использует несколько форматов, в одном `Label`.

    ```xaml
    <Label TextColor="Gray"
           FontSize="Medium">
        <Label.FormattedText>
            <FormattedString>
                <Span Text="This sentence contains " />
                <Span Text="words that are emphasized, "
                      FontAttributes="Italic" />
                <Span Text="and underlined."
                      TextDecorations="Underline" />
            </FormattedString>
        </Label.FormattedText>
    </Label>
    ```

    Этот код отображает текст в одном [`Label`](xref:Xamarin.Forms.Label), который использует несколько форматов. Текст в первом [`Span`](xref:Xamarin.Forms.Span) отображается с форматированием, заданным в `Label`, а текст во втором и третьем экземплярах `Span` отображается с форматированием, заданным в `Label`, и дополнительным форматированием, заданным в каждом `Span`.

    > [!NOTE]
    > Свойство [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) имеет тип [`FormattedString`](xref:Xamarin.Forms.FormattedString), который состоит из одного или нескольких экземпляров [`Span`](xref:Xamarin.Forms.Span).

1. На панели инструментов Visual Studio для Mac нажмите клавишу **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android. Обратите внимание, что внешний вид [`Label`](xref:Xamarin.Forms.Label) изменился:

    [![Снимок экрана: метка, отображающая форматированный текст, в iOS и Android](../images/label-formatted-text.png "Метка с форматированным текстом")](../images/label-formatted-text-large.png#lightbox "Метка с форматированным текстом")

    Дополнительные сведения о внешнем виде параметра [`Span`](xref:Xamarin.Forms.Span) см. в разделе [Форматированный текст](~/xamarin-forms/user-interface/text/label.md#formatted-text) в руководстве по [меткам в Xamarin.Forms](~/xamarin-forms/user-interface/text/label.md).
