---
ms.openlocfilehash: 3c88b71cea834f5e6ef20d43332904c052c6e3a6
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/09/2020
ms.locfileid: "61037631"
---
Ранее мы заполнили [`ListView`](xref:Xamarin.Forms.ListView) данными с помощью привязки данных. Однако несмотря на привязку данных к коллекции, где каждый объект в коллекции определял несколько элементов данных, только один элемент данных отображался для каждого объекта (свойство `Name` объекта `Monkey`).

В этом упражнении вы измените проект **ListViewTutorial**, чтобы [`ListView`](xref:Xamarin.Forms.ListView) отображал несколько элементов данных в каждой строке.

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** измените объявление [`ListView`](xref:Xamarin.Forms.Image), чтобы настроить внешний вид каждой строки:

    ```xaml
    <ListView ItemsSource="{Binding Monkeys}"
              HasUnevenRows="true"
              ItemSelected="OnListViewItemSelected"
              ItemTapped="OnListViewItemTapped">
        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <Grid Padding="10">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="*" />
                        </Grid.RowDefinitions>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="Auto" />
                            <ColumnDefinition Width="*" />
                        </Grid.ColumnDefinitions>
                        <Image Grid.RowSpan="2"
                               Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="60"
                               WidthRequest="60" />
                        <Label Grid.Column="1"
                               Text="{Binding Name}"
                               FontAttributes="Bold" />
                        <Label Grid.Row="1"
                               Grid.Column="1"
                               Text="{Binding Location}"
                               VerticalOptions="End" />
                    </Grid>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
    ```

    Этот код задает для свойства [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), которое определяет внешний вид каждой строки в [`ListView`](xref:Xamarin.Forms.ListView). Дочерний объект `DataTemplate` должен иметь тип [`Cell`](xref:Xamarin.Forms.Cell) или производный от него тип. Этот код использует [`ViewCell`](xref:Xamarin.Forms.ViewCell), который является производным от `Cell`, чтобы создать пользовательский макет для каждой строки `ListView`. Макет внутри `ViewCell` управляется здесь с помощью [`Grid`](xref:Xamarin.Forms.Grid), который содержит объект [`Image`](xref:Xamarin.Forms.Image) и два объекта [`Label`](xref:Xamarin.Forms.Label). Объект `Image` привязывает свое свойство [`Source`](xref:Xamarin.Forms.Image.Source) к свойству `ImageUrl` каждого объекта `Monkey`, а объекты `Label` привязывают свои свойства [`Text`](xref:Xamarin.Forms.Label.Text) к свойствам `Name` и `Location` каждого объекта `Monkey`.

    По умолчанию все строки в [`ListView`](xref:Xamarin.Forms.ListView) имеют одинаковую высоту. Однако этот код задает свойству [`ListView.HasUnevenRows`](xref:Xamarin.Forms.ListView.HasUnevenRows) значение `true`, чтобы высота строк изменялась по содержимому. Это позволяет вместить свойства `Name` и `Location` каждого объекта `Monkey` со строками разной длины.

    Дополнительные сведения о внешнем виде ячеек [`ListView`](xref:Xamarin.Forms.ListView) см. в статье [Настройка внешнего вида ячеек ListView](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md). Дополнительные сведения о шаблонах данных см. в разделе [Шаблоны данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

1. На панели инструментов Visual Studio нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS удаленной работы или эмуляторе Android:

    [![Снимок экрана: ListView, элементы которого воспроизводятся по шаблону с помощью шаблона данных](../images/customize-cell-appearance.png "ListView, отображающий шаблонные данные")](../images/customize-cell-appearance-large.png#lightbox "ListView, отображающий шаблонные данные")

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** измените объявление [`ListView`](xref:Xamarin.Forms.Image), чтобы настроить внешний вид каждой строки:

    ```xaml
    <ListView ItemsSource="{Binding Monkeys}"
              HasUnevenRows="true"
              ItemSelected="OnListViewItemSelected"
              ItemTapped="OnListViewItemTapped">
        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <Grid Padding="10">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="*" />
                        </Grid.RowDefinitions>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="Auto" />
                            <ColumnDefinition Width="*" />
                        </Grid.ColumnDefinitions>
                        <Image Grid.RowSpan="2"
                               Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="60"
                               WidthRequest="60" />
                        <Label Grid.Column="1"
                               Text="{Binding Name}"
                               FontAttributes="Bold" />
                        <Label Grid.Row="1"
                               Grid.Column="1"
                               Text="{Binding Location}"
                               VerticalOptions="End" />
                    </Grid>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
    ```

    Этот код задает для свойства [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), которое определяет внешний вид каждой строки в [`ListView`](xref:Xamarin.Forms.ListView). Дочерний объект `DataTemplate` должен иметь тип [`Cell`](xref:Xamarin.Forms.Cell) или производный от него тип. Этот код использует [`ViewCell`](xref:Xamarin.Forms.ViewCell), который является производным от `Cell`, чтобы создать пользовательский макет для каждой строки `ListView`. Макет внутри `ViewCell` управляется здесь с помощью [`Grid`](xref:Xamarin.Forms.Grid), который содержит объект [`Image`](xref:Xamarin.Forms.Image) и два объекта [`Label`](xref:Xamarin.Forms.Label). Объект `Image` привязывает свое свойство [`Source`](xref:Xamarin.Forms.Image.Source) к свойству `ImageUrl` каждого объекта `Monkey`, а объекты `Label` привязывают свои свойства [`Text`](xref:Xamarin.Forms.Label.Text) к свойствам `Name` и `Location` каждого объекта `Monkey`.

    По умолчанию все строки в [`ListView`](xref:Xamarin.Forms.ListView) имеют одинаковую высоту. Однако этот код задает свойству [`ListView.HasUnevenRows`](xref:Xamarin.Forms.ListView.HasUnevenRows) значение `true`, чтобы высота строк изменялась по содержимому. Это позволяет вместить свойства `Name` и `Location` каждого объекта `Monkey` со строками разной длины.

    Дополнительные сведения о внешнем виде ячеек [`ListView`](xref:Xamarin.Forms.ListView) см. в статье [Настройка внешнего вида ячеек ListView](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md). Дополнительные сведения о шаблонах данных см. в разделе [Шаблоны данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

1. На панели инструментов Visual Studio для Mac нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android.

    [![Снимок экрана: ListView, элементы которого воспроизводятся по шаблону с помощью шаблона данных](../images/customize-cell-appearance.png "ListView, отображающий шаблонные данные")](../images/customize-cell-appearance-large.png#lightbox "ListView, отображающий шаблонные данные")
