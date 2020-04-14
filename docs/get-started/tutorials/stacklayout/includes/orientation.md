---
ms.openlocfilehash: a7c2aa15521b89e4930746dc5421ce67fa26b2b9
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/09/2020
ms.locfileid: "67560019"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

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

1. На панели инструментов Visual Studio нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном удаленном симуляторе iOS или эмуляторе Android.

    [![Снимок экрана: дочерние представления с горизонтальной ориентацией в StackLayout в iOS и Android](../images/orientation.png "StackLayout, содержащий горизонтально ориентированные экземпляры меток")](../images/orientation-large.png#lightbox "StackLayout, содержащий горизонтально ориентированные экземпляры меток")

    Обратите внимание, что экземпляры [`Label`](xref:Xamarin.Forms.Label) в [`StackLayout`](xref:Xamarin.Forms.StackLayout) теперь выровнены по горизонтали, а не по вертикали.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

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

1. На панели инструментов Visual Studio для Mac нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android.

    [![Снимок экрана: дочерние представления с горизонтальной ориентацией в StackLayout в iOS и Android](../images/orientation.png "StackLayout, содержащий горизонтально ориентированные экземпляры меток")](../images/orientation-large.png#lightbox "StackLayout, содержащий горизонтально ориентированные экземпляры меток")

    Обратите внимание, что экземпляры [`Label`](xref:Xamarin.Forms.Label) в [`StackLayout`](xref:Xamarin.Forms.StackLayout) теперь выровнены по горизонтали, а не по вертикали.
