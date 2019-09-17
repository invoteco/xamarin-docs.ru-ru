---
ms.openlocfilehash: a537bdaf2015dd7d179545cb7587934818e960d7
ms.sourcegitcommit: a5ef4497db04dfa016865bc7454b3de6ff088554
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/15/2019
ms.locfileid: "71004151"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** измените объявление [`ListView`](xref:Xamarin.Forms.ListView) таким образом, чтобы оно установило обработчики для событий [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) и [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped).

    ```xaml
    <ListView ItemsSource="{Binding Monkeys}"
              ItemSelected="OnListViewItemSelected"
              ItemTapped="OnListViewItemTapped" />
    ```

    Этот код устанавливает событие [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) для обработчика событий `OnListViewItemSelected`, а [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) — для обработчика `OnListViewItemTapped`. На следующем шаге создаются оба обработчика событий.

1. В **обозревателе решений** в проекте **ListViewTutorial** разверните **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его. Затем в файле **MainPage.xaml.cs** добавьте обработчики событий `OnListViewItemSelected` и `OnListViewItemTapped` в класс.

    ```csharp
    void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
    {
        Monkey selectedItem = e.SelectedItem as Monkey;
    }

    void OnListViewItemTapped(object sender, ItemTappedEventArgs e)
    {
        Monkey tappedItem = e.Item as Monkey;
    }
    ```

    Если нажат элемент в [`ListView`](xref:Xamarin.Forms.ListView), срабатывают события [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) и [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped), что соответственно, выполняет методы `OnListViewItemSelected` и `OnListItemTapped`. Аргумент `sender` для обоих методов является объектом `ListView`, ответственным за запуск события, и может использоваться для доступа к объекту `ListView`. Аргумент [`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs) в методе `OnListViewItemSelected` предоставляет выбранный элемент, а аргумент [`ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs) в методе `OnListViewItemTapped` предоставляет нажатый элемент.

    > [!IMPORTANT]
    > Событие [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) происходит только в том случае, когда новый элемент выбран в [`ListView`](xref:Xamarin.Forms.ListView). Таким образом, при касании того же элемента дважды, будут срабатывать два события [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped), но только событие `ItemSelected` будет запущено.

1. На панели инструментов Visual Studio нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном удаленном симуляторе iOS или эмуляторе Android.

    [![Снимок экрана ListView, который реагирует на выбор элементов и нажатие кнопок в iOS и Android](../images/item-selection.png "Выбор элемента ListView")](../images/item-selection-large.png#lightbox "Выбор элемента ListView")

    Установите точки останова в двух обработчиках событий и нажмите на элементы в [`ListView`](xref:Xamarin.Forms.ListView). Обратите внимание, что событие [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) срабатывает только в том случае, когда элемент выбран в [`ListView`](xref:Xamarin.Forms.ListView), тогда как событие[`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) срабатывает при каждом касании элемента.

    Дополнительные сведения о выборе элементов и нажатии кнопок, см. в разделе [Выбор & касание](~/xamarin-forms/user-interface/listview/interactivity.md#selection-and-taps) в руководстве [Интерактивность ListView](~/xamarin-forms/user-interface/listview/interactivity.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** измените объявление [`ListView`](xref:Xamarin.Forms.ListView) таким образом, чтобы оно установило обработчики для событий [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) и [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped).

    ```xaml
    <ListView ItemsSource="{Binding Monkeys}"
              ItemSelected="OnListViewItemSelected"
              ItemTapped="OnListViewItemTapped" />
    ```

    Этот код устанавливает событие [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) для обработчика событий `OnListViewItemSelected`, а [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) — для обработчика `OnListViewItemTapped`. На следующем шаге создаются оба обработчика событий.

1. На **Панели решения** в проекте **ListViewTutorial**, разверните **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его. Затем в файле **MainPage.xaml.cs** добавьте обработчики событий `OnListViewItemSelected` и `OnListViewItemTapped` в класс.

    ```csharp
    void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
    {
        Monkey selectedItem = e.SelectedItem as Monkey;
    }

    void OnListViewItemTapped(object sender, ItemTappedEventArgs e)
    {
        Monkey tappedItem = e.Item as Monkey;
    }
    ```

    Если нажат элемент в [`ListView`](xref:Xamarin.Forms.ListView), срабатывают события [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) и [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped), что соответственно, выполняет методы `OnListViewItemSelected` и `OnListItemTapped`. Аргумент `sender` для обоих методов является объектом `ListView`, ответственным за запуск события, и может использоваться для доступа к объекту `ListView`. Аргумент [`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs) в методе `OnListViewItemSelected` предоставляет выбранный элемент, а аргумент [`ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs) в методе `OnListViewItemTapped` предоставляет нажатый элемент.

    > [!IMPORTANT]
    > Событие [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) происходит только в том случае, когда новый элемент выбран в [`ListView`](xref:Xamarin.Forms.ListView). Таким образом, при касании того же элемента дважды, будут срабатывать два события [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped), но только событие `ItemSelected` будет запущено.

1. На панели инструментов Visual Studio для Mac нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android.

    [![Снимок экрана ListView, который реагирует на выбор элементов и нажатие кнопок в iOS и Android](../images/item-selection.png "Выбор элемента ListView")](../images/item-selection-large.png#lightbox "Выбор элемента ListView")

    Установите точки останова в двух обработчиках событий и нажмите на элементы в [`ListView`](xref:Xamarin.Forms.ListView). Обратите внимание, что событие [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) срабатывает только в том случае, когда элемент выбран в [`ListView`](xref:Xamarin.Forms.ListView), тогда как событие[`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) срабатывает при каждом касании элемента.

    Дополнительные сведения о выборе элементов и о касаниях см. в разделе [Выделение и касания](~/xamarin-forms/user-interface/listview/interactivity.md#selection-and
