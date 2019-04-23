---
ms.openlocfilehash: d6dbc82e56959399c2befb6a12f0a2cf3793ee5b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61382567"
---
Размер и положение дочерних представлений в [`StackLayout`](xref:Xamarin.Forms.StackLayout) зависят от значений свойств [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) и [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) дочерних представлений и значений свойств [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) и [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions).

Свойства [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) и [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) могут задаваться для полей из структуры [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions), которая инкапсулирует две настройки макета:

- **Выравнивание** — настройки выравнивания дочернего представления, которые определяют его положение и размер в рамках родительского макета.
- **Расширение** — указывает, должно ли дочернее представление использовать дополнительное пространство, если оно доступно (используется только [`StackLayout`](xref:Xamarin.Forms.StackLayout)).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** измените объявление [`StackLayout`](xref:Xamarin.Forms.StackLayout), чтобы задать параметры выравнивания и расширения для каждого [`Label`](xref:Xamarin.Forms.Label):

    ```xaml
    <StackLayout Margin="20,35,20,25">
        <Label Text="Start"
               HorizontalOptions="Start"
               BackgroundColor="Gray" />
        <Label Text="Center"
               HorizontalOptions="Center"
               BackgroundColor="Gray" />
        <Label Text="End"
               HorizontalOptions="End"
               BackgroundColor="Gray" />
        <Label Text="Fill"
               HorizontalOptions="Fill"
               BackgroundColor="Gray" />
        <Label Text="StartAndExpand"
               VerticalOptions="StartAndExpand"
               BackgroundColor="Gray" />
        <Label Text="CenterAndExpand"
               VerticalOptions="CenterAndExpand"
               BackgroundColor="Gray" />
        <Label Text="EndAndExpand"
               VerticalOptions="EndAndExpand"
               BackgroundColor="Gray" />
        <Label Text="FillAndExpand"
               VerticalOptions="FillAndExpand"
               BackgroundColor="Gray" />
    </StackLayout>
    ```

    Этот код задает параметры выравнивания для первых четырех экземпляров [`Label`](xref:Xamarin.Forms.Label) и параметры расширения для последних четырех экземпляров `Label`. Поля [`Start`](xref:Xamarin.Forms.LayoutOptions.Start), [`Center`](xref:Xamarin.Forms.LayoutOptions.Center), [`End`](xref:Xamarin.Forms.LayoutOptions.End) и [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) используются для определения выравнивания экземпляров [`Label`](xref:Xamarin.Forms.Label) в родительском элементе [`StackLayout`](xref:Xamarin.Forms.StackLayout). Поля [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand), [`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand), [`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand) и [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) используются для определения параметров выравнивания и указывают, будет ли представление занимать больше места, если оно доступно в родительском элементе `StackLayout`.

    > [!NOTE]
    > Значение по умолчанию свойств представления [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) и [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) — [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill).

1. На панели инструментов Visual Studio нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS удаленной работы или эмуляторе Android:

    [![Снимок экрана: дочерние представления в StackLayout с заданными параметрами выравнивания и расширения в iOS и Android](../images/alignment-expansion.png "StackLayout с экземплярами метки с заданным выравниванием и расширением")](../images/alignment-expansion-large.png#lightbox "StackLayout с экземплярами метки с заданным выравниванием и расширением")

    [`StackLayout`](xref:Xamarin.Forms.StackLayout) учитывает только параметры выравнивания дочерних представлениях, которые находятся в направлении, противоположном ориентации `StackLayout`. Поэтому дочерние представления [`Label`](xref:Xamarin.Forms.Label) с `StackLayout` в вертикальной ориентации задают свои свойства [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) одному из полей выравнивания:

    - [`Start`](xref:Xamarin.Forms.LayoutOptions.Start) — располагает [`Label`](xref:Xamarin.Forms.Label) в левой части [`StackLayout`](xref:Xamarin.Forms.StackLayout).
    - [`Center`](xref:Xamarin.Forms.LayoutOptions.Center) — располагает [`Label`](xref:Xamarin.Forms.Label) в центре [`StackLayout`](xref:Xamarin.Forms.StackLayout).
    - [`End`](xref:Xamarin.Forms.LayoutOptions.End) — располагает [`Label`](xref:Xamarin.Forms.Label) в правой части [`StackLayout`](xref:Xamarin.Forms.StackLayout).
    - [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) — гарантирует, что [`Label`](xref:Xamarin.Forms.Label) заполняет ширину [`StackLayout`](xref:Xamarin.Forms.StackLayout).

    [`StackLayout`](xref:Xamarin.Forms.StackLayout) может развернуть дочерние представления только в направлении своей ориентации. Поэтому `StackLayout` в вертикальной ориентации может развернуть дочерние представления [`Label`](xref:Xamarin.Forms.Label), которые задают свои свойства [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) одному из полей выравнивания. Это означает, что для вертикального выравнивания каждый `Label` занимает одинаковый объем пространства в `StackLayout`. Но только последний `Label` со значением свойства [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions), равным [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand), имеет другой размер.

    > [!IMPORTANT]
    > Когда все пространство в [`StackLayout`](xref:Xamarin.Forms.StackLayout) занято, параметр расширения ни на что не влияет.

    Дополнительные сведения о выравнивании и расширении см. в разделе [Параметры макета в Xamarin.Forms](~/xamarin-forms/user-interface/layouts/layout-options.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** измените объявление [`StackLayout`](xref:Xamarin.Forms.StackLayout), чтобы задать параметры выравнивания и расширения для каждого [`Label`](xref:Xamarin.Forms.Label):

    ```xaml
    <StackLayout Margin="20,35,20,25">
        <Label Text="Start"
               HorizontalOptions="Start"
               BackgroundColor="Gray" />
        <Label Text="Center"
               HorizontalOptions="Center"
               BackgroundColor="Gray" />
        <Label Text="End"
               HorizontalOptions="End"
               BackgroundColor="Gray" />
        <Label Text="Fill"
               HorizontalOptions="Fill"
               BackgroundColor="Gray" />
        <Label Text="StartAndExpand"
               VerticalOptions="StartAndExpand"
               BackgroundColor="Gray" />
        <Label Text="CenterAndExpand"
               VerticalOptions="CenterAndExpand"
               BackgroundColor="Gray" />
        <Label Text="EndAndExpand"
               VerticalOptions="EndAndExpand"
               BackgroundColor="Gray" />
        <Label Text="FillAndExpand"
               VerticalOptions="FillAndExpand"
               BackgroundColor="Gray" />
    </StackLayout>
    ```

    Этот код задает параметры выравнивания для первых четырех экземпляров [`Label`](xref:Xamarin.Forms.Label) и параметры расширения для последних четырех экземпляров `Label`. Поля [`Start`](xref:Xamarin.Forms.LayoutOptions.Start), [`Center`](xref:Xamarin.Forms.LayoutOptions.Center), [`End`](xref:Xamarin.Forms.LayoutOptions.End) и [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) используются для определения выравнивания экземпляров [`Label`](xref:Xamarin.Forms.Label) в родительском элементе [`StackLayout`](xref:Xamarin.Forms.StackLayout). Поля [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand), [`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand), [`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand) и [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) используются для определения параметров выравнивания и указывают, будет ли представление занимать больше места, если оно доступно в родительском элементе `StackLayout`.

    > [!NOTE]
    > Значение по умолчанию свойств представления [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) и [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) — [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill).

1. На панели инструментов Visual Studio для Mac нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android:

    [![Снимок экрана: дочерние представления в StackLayout с заданными параметрами выравнивания и расширения в iOS и Android](../images/alignment-expansion.png "StackLayout с экземплярами метки с заданным выравниванием и расширением")](../images/alignment-expansion-large.png#lightbox "StackLayout с экземплярами метки с заданным выравниванием и расширением")

    [`StackLayout`](xref:Xamarin.Forms.StackLayout) учитывает только параметры выравнивания дочерних представлениях, которые находятся в направлении, противоположном ориентации `StackLayout`. Поэтому дочерние представления [`Label`](xref:Xamarin.Forms.Label) с `StackLayout` в вертикальной ориентации задают свои свойства [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) одному из полей выравнивания:

    - [`Start`](xref:Xamarin.Forms.LayoutOptions.Start) — располагает [`Label`](xref:Xamarin.Forms.Label) в левой части [`StackLayout`](xref:Xamarin.Forms.StackLayout).
    - [`Center`](xref:Xamarin.Forms.LayoutOptions.Center) — располагает [`Label`](xref:Xamarin.Forms.Label) в центре [`StackLayout`](xref:Xamarin.Forms.StackLayout).
    - [`End`](xref:Xamarin.Forms.LayoutOptions.End) — располагает [`Label`](xref:Xamarin.Forms.Label) в правой части [`StackLayout`](xref:Xamarin.Forms.StackLayout).
    - [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) — гарантирует, что [`Label`](xref:Xamarin.Forms.Label) заполняет ширину [`StackLayout`](xref:Xamarin.Forms.StackLayout).

    [`StackLayout`](xref:Xamarin.Forms.StackLayout) может развернуть дочерние представления только в направлении своей ориентации. Поэтому `StackLayout` в вертикальной ориентации может развернуть дочерние представления [`Label`](xref:Xamarin.Forms.Label), которые задают свои свойства [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) одному из полей выравнивания. Это означает, что для вертикального выравнивания каждый `Label` занимает одинаковый объем пространства в `StackLayout`. Но только последний `Label` со значением свойства [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions), равным [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand), имеет другой размер.

    > [!IMPORTANT]
    > Когда все пространство в [`StackLayout`](xref:Xamarin.Forms.StackLayout) занято, параметр расширения ни на что не влияет.

    Дополнительные сведения о выравнивании и расширении см. в разделе [Параметры макета в Xamarin.Forms](~/xamarin-forms/user-interface/layouts/layout-options.md).
