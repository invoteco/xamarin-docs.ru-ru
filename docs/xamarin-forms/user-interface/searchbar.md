---
title: Сеарчбар Xamarin. Forms
description: Сеарчбар Xamarin. Forms — это пользовательский элемент управления вводом, который используется для запуска поиска. Элемент управления Сеарчбар поддерживает текст заполнителя, ввод запроса, выполнение и отмену. В этой статье объясняется, как использовать Сеарчбар в XAML и коде.
ms.prod: xamarin
ms.assetId: F5EFEA72-CB23-4DD6-9545-D9BB755AF3CB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/12/2019
ms.openlocfilehash: 41bb9e082f042e7ca2933d72b4b71a4ff6c4fef4
ms.sourcegitcommit: 9178e2e689f027212ea3e623b556b312985d79fe
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/21/2019
ms.locfileid: "69658026"
---
# <a name="xamarinforms-searchbar"></a>Сеарчбар Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/)

Xamarin. Forms [`SearchBar`](xref:Xamarin.Forms.SearchBar) — это элемент управления вводом пользователя, используемый для запуска поиска. `SearchBar` Элемент управления поддерживает текст заполнителя, ввод запроса, выполнение поиска и отмену. На следующем снимке экрана `SearchBar` показан запрос с результатами, `ListView`отображаемыми в:

[ ![Снимок экрана Сеарчбар в iOS и Android](searchbar-images/device-searchbars-cropped.png "Сеарчбар в iOS и Android") ] (searchbar-images/device-searchbars.png#lightbox "Сеарчбар в iOS и Android")

`SearchBar` Класс определяет следующие свойства:

* [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor)значение типа `Color` , определяющее цвет кнопки отмены.
* [`FontAttributes`](xref:Xamarin.Forms.SearchBar.FontAttributes)значение перечисления, которое определяет, является ли `SearchBar` шрифт полужирным, курсивом или ни тем ни другого. `FontAttributes`
* [`FontFamily`](xref:Xamarin.Forms.SearchBar.FontFamily)значение типа `string` , определяющее семейство шрифтов, используемое `SearchBar`.
* [`FontSize`](xref:Xamarin.Forms.SearchBar.FontSize)может быть `NamedSize` значением перечисления `double` или значением, представляющим определенные размеры шрифтов на разных платформах.
* [`HorizontalTextAlignment`](xref:Xamarin.Forms.SearchBar.HorizontalTextAlignment)`TextAlignment` значение перечисления, определяющее выравнивание текста запроса по горизонтали.
* [`Placeholder`](xref:Xamarin.Forms.SearchBar.Placeholder)`string` значение, определяющее текст заполнителя, например "Поиск...".
* [`PlaceholderColor`](xref:Xamarin.Forms.SearchBar.PlaceholderColor)значение типа `Color` , определяющее цвет текста заполнителя.
* [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand)— Это `ICommand` , которое позволяет привязать пользовательские действия, такие как касания пальца или щелчки, к командам, определенным в ViewModel.
* [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter)аргумент, указывающий параметр, который должен быть передан в `SearchCommand`. `object`
* [`Text`](xref:Xamarin.Forms.SearchBar.Text)Объект `string` , содержащий текст запроса `SearchBar`в.
* [`TextColor`](xref:Xamarin.Forms.SearchBar.TextColor)значение типа `Color` , определяющее цвет текста запроса.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, `SearchBar` что означает возможность настройки и назначения привязок данных. Указание свойств `SearchBar` шрифта в согласуется с настройками текста в других [элементах управления "текст" Xamarin. Forms](~/xamarin-forms/user-interface/text/index.md). Дополнительные сведения см. [в статье шрифты в Xamarin. Forms](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="create-a-searchbar"></a>Создание Сеарчбар

Экземпляр `SearchBar` можно создать в XAML. Его необязательное `Placeholder` свойство можно задать для определения текста подсказки в поле ввода запроса. Значение по умолчанию для `Placeholder` параметра — пустая строка, поэтому не отображается заполнитель, если он не задан. В следующем примере показано, как создать экземпляр `SearchBar` в XAML с помощью необязательного `Placeholder` набора свойств:

```xaml
<SearchBar Placeholder="Search items..." />
```

Также `SearchBar` можно создать в коде:

```csharp
SearchBar searchBar = new SearchBar{ Placeholder = "Search items..." };
```

### <a name="searchbar-appearance-properties"></a>Свойства внешнего вида Сеарчбар

`SearchBar` Элемент управления определяет множество свойств, которые настраивают внешний вид элемента управления. В следующем примере показано, как создать экземпляр `SearchBar` в XAML с несколькими указанными свойствами:

```xaml
<SearchBar Placeholder="Search items..."
           CancelButtonColor="Orange"
           PlaceholderColor="Orange"
           TextColor="Orange"
           HorizontalTextAlignment="Center"
           FontSize="Medium"
           FontAttributes="Italic" />
```

Эти свойства также можно указать при создании `SearchBar` объекта в коде:

```csharp
SearchBar searchBar = new SearchBar
{
    Placeholder = "Search items...",
    PlaceholderColor = Color.Orange,
    TextColor = Color.Orange,
    HorizontalTextAlignment = TextAlignment.Center,
    FontSize = Device.GetNamedSize(NamedSize.Medium, typeof(SearchBar)),
    FontAttributes = FontAttributes.Italic
};
```

На следующем снимке экрана показан `SearchBar` итоговый элемент управления:

[ ![Снимок экрана настраиваемого Сеарчбар в iOS и Android],(searchbar-images/device-searchbars-styled-cropped.png "настроенном Сеарчбар в iOS и Android") ] (searchbar-images/device-searchbars-styled.png#lightbox "Настраиваемые сеарчбар в iOS и Android")

## <a name="perform-a-search-with-event-handlers"></a>Выполнение поиска с помощью обработчиков событий

Поиск можно выполнить с помощью `SearchBar` элемента управления, присоединив обработчик событий к одному из следующих событий:

* [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)вызывается, когда пользователь либо нажимает кнопку поиска, либо нажимает клавишу ВВОД.
* [`TextChanged`](xref:Xamarin.Forms.SearchBar.TextChanged)вызывается всякий раз, когда изменяется текст в поле запроса.

В следующем примере показан обработчик событий, присоединенный к `TextChanged` событию в XAML и `ListView` использующий для отображения результатов поиска:

```xaml
<SearchBar TextChanged="OnTextChanged" />
<ListView x:Name="searchResults" >
```

Обработчик событий можно также присоединить к `SearchBar` созданному в коде:

```csharp
SearchBar searchBar = new SearchBar {/*...*/};
searchBar.TextChanged += OnTextChanged;
```

Обработчик событий в файле кода программной части одинаков, создается ли объект с `SearchBar` помощью XAML или кода: `TextChanged`

```csharp
void OnTextChanged(object sender, EventArgs e)
{
    SearchBar searchBar = (SearchBar)sender;
    searchResults.ItemsSource = DataService.GetSearchResults(searchBar.Text);
}
```

В предыдущем примере подразумевается существование `DataService` класса `GetSearchResults` с методом, способным возвращать элементы, соответствующие запросу. `SearchBar` `ItemsSource` Значение свойства элемента управления передается в `ListView` метод,арезультатиспользуетсядляобновлениясвойстваэлементауправления.`GetSearchResults` `Text` Общий результат заключается в том, что результаты поиска отображаются в `ListView` элементе управления.

Пример приложения предоставляет `DataService` реализацию класса, которую можно использовать для тестирования функций поиска.

## <a name="perform-a-search-using-a-viewmodel"></a>Выполнение поиска с помощью ViewModel

Поиск может выполняться без обработчиков событий путем привязки `SearchCommand` свойств и `SearchCommandParameter` к `ICommand` реализациям. Пример проекта демонстрирует эти реализации с помощью шаблона Model-View-ViewModel (MVVM). Дополнительные сведения о привязках данных с помощью MVVM см. [в разделе привязки данных с помощью MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

В примере приложения ViewModel содержит следующий код:

```csharp
public class SearchViewModel : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;

    protected virtual void NotifyPropertyChanged([CallerMemberName] string propertyName = "")
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }

    public ICommand PerformSearch => new Command<string>((string query) =>
    {
        SearchResults = DataService.GetSearchResults(query);
    });

    private List<string> searchResults = DataService.Fruits;
    public List<string> SearchResults
    {
        get
        {
            return searchResults;
        }
        set
        {
            searchResults = value;
            NotifyPropertyChanged();
        }
    }
}
```

> [!NOTE]
> ViewModel предполагает наличие `DataService` класса, способного выполнять поиск. `DataService` Класс, включая пример данных, доступен в примере приложения.

В следующем коде XAML показано, как привязать `SearchBar` к примеру ViewModel, `ListView` с элементом управления, отображающим результаты поиска:

```xaml
<ContentPage ...>
    <ContentPage.BindingContext>
        <viewmodels:SearchViewModel />
    </ContentPage.BindingContext>
    <StackLayout ...>
        <SearchBar x:Name="searchBar"
                   ...
                   SearchCommand="{Binding PerformSearch}"
                   SearchCommandParameter="{Binding Text, Source={x:Reference searchBar}}"/>
        <ListView x:Name="searchResults"
                  ...
                  ItemsSource="{Binding SearchResults} />
    </StackLayout>
</ContentPage>
```

В этом примере задается `BindingContext` как экземпляр `SearchViewModel` класса. Он `SearchCommand` привязывает `SearchCommandParameter` свойство `PerformSearch` `SearchBar` к в ViewModel и привязывает `Text` свойство к свойству. `ICommand` Свойство привязано `SearchResults` к свойству ViewModel. `ListView.ItemsSource`

Дополнительные сведения о `ICommand` интерфейсе и привязках см. в разделе [Привязка данных Xamarin. Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md) и [интерфейс ICommand](~/xamarin-forms/app-fundamentals/data-binding/commanding.md).

## <a name="related-links"></a>Связанные ссылки

* [Демонстрации Сеарчбар](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/)
* [Текстовые элементы управления Xamarin. Forms](~/xamarin-forms/user-interface/text/index.md)
* [Шрифты в Xamarin. Forms](~/xamarin-forms/user-interface/text/fonts.md)
* [Привязка данных Xamarin. Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md)
