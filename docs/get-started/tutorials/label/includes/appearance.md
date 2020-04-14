---
ms.openlocfilehash: 4fc6c50b5aa2ce502b4157ca2b15f0d33a68ecd1
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/09/2020
ms.locfileid: "60896734"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** измените объявление [`Label`](xref:Xamarin.Forms.Label), чтобы изменить его внешний вид:

    ```xaml
    <Label Text="Welcome to Xamarin.Forms!"
           TextColor="Blue"
           FontAttributes="Italic"
           FontSize="22"
           TextDecorations="Underline"
           HorizontalOptions="Center" />
    ```

    Этот код задает свойства, которые изменяют внешний вид [`Label`](xref:Xamarin.Forms.Label). Свойство [`TextColor`](xref:Xamarin.Forms.Label.TextColor) задает цвет текста `Button`. Свойство [`FontAttributes`](xref:Xamarin.Forms.Label.FontAttributes) используется для применения курсива, а свойство [`FontSize`](xref:Xamarin.Forms.Label.FontSize) задает размер шрифта. Кроме того, подчеркивание текста применяется к `Label`, при применении свойства [`TextDecorations`](xref:Xamarin.Forms.Label.TextDecorations), а чтобы центрировать его по горизонтали, необходимо придать свойству [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) значение [`Center`](xref:Xamarin.Forms.LayoutOptions.Center).

1. На панели инструментов Visual Studio нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном удаленном симуляторе iOS или эмуляторе Android. Обратите внимание, что внешний вид [`Label`](xref:Xamarin.Forms.Label) изменился:

    [![Снимок экрана: метка с измененным внешним видом в iOS и Android](../images/change-label-appearance.png "Метка с измененным видом")](../images/change-label-appearance-large.png#lightbox "Метка с измененным видом")

    Дополнительные сведения о внешнем виде параметра [`Label`](xref:Xamarin.Forms.Label) см. в руководстве [Xamarin.Forms Label](~/xamarin-forms/user-interface/text/label.md).

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** измените объявление [`Label`](xref:Xamarin.Forms.Label), чтобы изменить его внешний вид:

    ```xaml
    <Label Text="Welcome to Xamarin.Forms!"
           TextColor="Blue"
           FontAttributes="Italic"
           FontSize="22"
           TextDecorations="Underline"
           HorizontalOptions="Center" />
    ```

    Этот код задает свойства, которые изменяют внешний вид [`Label`](xref:Xamarin.Forms.Label). Свойство [`TextColor`](xref:Xamarin.Forms.Label.TextColor) задает цвет текста `Button`. Свойство [`FontAttributes`](xref:Xamarin.Forms.Label.FontAttributes) используется для применения курсива, а свойство [`FontSize`](xref:Xamarin.Forms.Label.FontSize) задает размер шрифта. Кроме того, подчеркивание текста применяется к `Label`, при применении свойства [`TextDecorations`](xref:Xamarin.Forms.Label.TextDecorations), а чтобы центрировать его по горизонтали, необходимо придать свойству [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) значение [`Center`](xref:Xamarin.Forms.LayoutOptions.Center).

1. На панели инструментов Visual Studio для Mac нажмите клавишу **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android. Обратите внимание, что внешний вид [`Label`](xref:Xamarin.Forms.Label) изменился:

    [![Снимок экрана: метка с измененным внешним видом в iOS и Android](../images/change-label-appearance.png "Метка с измененным видом")](../images/change-label-appearance-large.png#lightbox "Метка с измененным видом")

    Дополнительные сведения о внешнем виде параметра [`Label`](xref:Xamarin.Forms.Label) см. в руководстве [Xamarin.Forms Label](~/xamarin-forms/user-interface/text/label.md).
