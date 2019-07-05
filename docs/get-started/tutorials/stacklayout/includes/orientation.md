---
ms.openlocfilehash: 5a464196c08220158432219d96bf82578789d8e5
ms.sourcegitcommit: 8ecfa339d0f3e7687977bfe4fc96448942690183
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2019
ms.locfileid: "67560019"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** измените объявление [`StackLayout`](xref:Xamarin.Forms.StackLayout), чтобы выровнять его дочерние элементы по горизонтали, а не по вертикали:

    ```xaml
    <StackLayout Margin="20,35,20,25"
                 Orientation="Horizontal">
        <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
        <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
        <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
    </StackLayout>
    ```

    В этом коде свойству [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) присваивается значение [`Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal).

1. На панели инструментов Visual Studio нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS удаленной работы или эмуляторе Android:

    [![Снимок экрана горизонтальных дочерних представлений StackLayout в iOS и Android](../images/orientation.png "StackLayout с горизонтальным расположением экземпляров меток")](../images/orientation-large.png#lightbox "StackLayout с горизонтальным расположением экземпляров меток")

    Обратите внимание, что экземпляры [`Label`](xref:Xamarin.Forms.Label) в [`StackLayout`](xref:Xamarin.Forms.StackLayout) теперь выровнены по горизонтали, а не по вертикали.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** измените объявление [`StackLayout`](xref:Xamarin.Forms.StackLayout), чтобы выровнять его дочерние элементы по горизонтали, а не по вертикали:

    ```xaml
    <StackLayout Margin="20,35,20,25"
                 Orientation="Horizontal">
        <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
        <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
        <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
    </StackLayout>
    ```

    В этом коде свойству [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) присваивается значение [`Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal).

1. На панели инструментов Visual Studio для Mac нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android:

    [![Снимок экрана горизонтальных дочерних представлений StackLayout в iOS и Android](../images/orientation.png "StackLayout с горизонтальным расположением экземпляров меток")](../images/orientation-large.png#lightbox "StackLayout с горизонтальным расположением экземпляров меток")

    Обратите внимание, что экземпляры [`Label`](xref:Xamarin.Forms.Label) в [`StackLayout`](xref:Xamarin.Forms.StackLayout) теперь выровнены по горизонтали, а не по вертикали.

-----

> [!div class="nextstepaction"]
> [У меня возникла проблема](https://github.com/MicrosoftDocs/xamarin-docs/issues/new?title=StackLayout+Tutorial+Step+2+Feedback&template=tutorial_template.md)
