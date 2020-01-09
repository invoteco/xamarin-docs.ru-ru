---
title: Сеарчбар Xamarin. Forms
description: Сеарчбар Xamarin. Forms — это пользовательский элемент управления вводом, который используется для запуска поиска. Элемент управления Сеарчбар поддерживает текст заполнителя, ввод запроса, выполнение и отмену. В этой статье объясняется, как использовать Сеарчбар в XAML и коде.
ms.prod: xamarin
ms.assetId: F5EFEA72-CB23-4DD6-9545-D9BB755AF3CB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 11/04/2019
ms.openlocfilehash: 8888f1615f250a908930cec9058a54bd6e7fedc2
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75490081"
---
# <a name="xamarinforms-searchbar"></a>Сеарчбар Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/)

[`SearchBar`](xref:Xamarin.Forms.SearchBar) Xamarin. Forms — это пользовательский элемент управления вводом, используемый для запуска поиска. Элемент управления `SearchBar` поддерживает текст заполнителя, ввод запроса, выполнение поиска и отмену. На следующем снимке экрана показан `SearchBar` запрос с результатами, отображаемыми в `ListView`:

[![Снимок экрана Сеарчбар в iOS и Android](searchbar-images/device-searchbars-cropped.png "Сеарчбар в iOS и Android")](searchbar-images/device-searchbars.png#lightbox "Сеарчбар в iOS и Android")

Класс `SearchBar` определяет следующие свойства:

* [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor) — это `Color`, определяющая цвет кнопки отмены.
* `CharacterSpacing` с типом `double` представляет собой интервал между знаками текста `SearchBar`.
* [`FontAttributes`](xref:Xamarin.Forms.SearchBar.FontAttributes) — это `FontAttributes` перечислимое значение, определяющее, является ли шрифт `SearchBar` полужирным, курсивом или ни одной.
* [`FontFamily`](xref:Xamarin.Forms.SearchBar.FontFamily) — это `string`, который определяет семейство шрифтов, используемое `SearchBar`.
* [`FontSize`](xref:Xamarin.Forms.SearchBar.FontSize) может быть `NamedSize` значением перечисления или `double` значением, представляющим определенные размеры шрифтов на разных платформах.
* [`HorizontalTextAlignment`](xref:Xamarin.Forms.SearchBar.HorizontalTextAlignment) — это `TextAlignment` перечислимое значение, определяющее выравнивание текста запроса по горизонтали.
* `VerticalTextAlignment` — это `TextAlignment` перечислимое значение, определяющее вертикальное выравнивание текста запроса.
* [`Placeholder`](xref:Xamarin.Forms.SearchBar.Placeholder) — это `string`, который определяет текст заполнителя, например "Поиск...".
* [`PlaceholderColor`](xref:Xamarin.Forms.SearchBar.PlaceholderColor) — это `Color`, определяющая цвет текста заполнителя.
* [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand) — это `ICommand`, который позволяет привязать действия пользователя, такие как касания пальца или щелчки, к командам, определенным в ViewModel.
* [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) — это `object`, указывающий параметр, который должен быть передан `SearchCommand`.
* [`Text`](xref:Xamarin.Forms.SearchBar.Text) — это `string`, содержащий текст запроса в `SearchBar`.
* [`TextColor`](xref:Xamarin.Forms.SearchBar.TextColor) — это `Color`, определяющая цвет текста запроса.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что `SearchBar` может быть настроена и быть целью привязок данных. Указание свойств шрифта в `SearchBar` согласуется с настройкой текста в других [элементах управления "текст" Xamarin. Forms](~/xamarin-forms/user-interface/text/index.md). Дополнительные сведения см. [в статье шрифты в Xamarin. Forms](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="create-a-searchbar"></a>Создание Сеарчбар

В XAML можно создать экземпляр `SearchBar`. Необязательное свойство `Placeholder` может быть установлено для определения текста подсказки в поле ввода запроса. Значением по умолчанию для `Placeholder` является пустая строка, поэтому не отображается заполнитель, если он не задан. В следующем примере показано, как создать экземпляр `SearchBar` в XAML с помощью необязательного набора свойств `Placeholder`:

```xaml
<SearchBar Placeholder="Search items..." />
```

`SearchBar` также можно создать в коде:

```csharp
SearchBar searchBar = new SearchBar{ Placeholder = "Search items..." };
```

### <a name="searchbar-appearance-properties"></a>Свойства внешнего вида Сеарчбар

Элемент управления `SearchBar` определяет множество свойств, которые настраивают внешний вид элемента управления. В следующем примере показано, как создать экземпляр `SearchBar` в XAML с несколькими указанными свойствами:

```xaml
<SearchBar Placeholder="Search items..."
           CancelButtonColor="Orange"
           PlaceholderColor="Orange"
           TextColor="Orange"
           HorizontalTextAlignment="Center"
           FontSize="Medium"
           FontAttributes="Italic" />
```

Эти свойства также можно указать при создании объекта `SearchBar` в коде:

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

На следующем снимке экрана показан результирующий элемент управления `SearchBar`:

[![Снимок экрана настраиваемого Сеарчбар в iOS и Android](searchbar-images/device-searchbars-styled-cropped.png "Настраиваемые Сеарчбар в iOS и Android")](searchbar-images/device-searchbars-styled.png#lightbox "Настраиваемые Сеарчбар в iOS и Android")

> [!NOTE]
> В iOS класс `SearchBarRenderer` содержит переопределяемый метод `UpdateCancelButton`. Этот метод управляет тем, когда появляется кнопка Отмена, и может быть переопределена в пользовательском модуле подготовки отчетов. Дополнительные сведения о пользовательских модулях подготовки отчетов см. в разделе [пользовательские модули подготовки Xamarin. Forms](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

## <a name="perform-a-search-with-event-handlers"></a>Выполнение поиска с помощью обработчиков событий

Поиск можно выполнить с помощью элемента управления `SearchBar`, присоединив обработчик событий к одному из следующих событий:

* [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed) вызывается, когда пользователь либо нажимает кнопку поиска, либо нажимает клавишу ВВОД.
* [`TextChanged`](xref:Xamarin.Forms.SearchBar.TextChanged) вызывается всякий раз, когда изменяется текст в поле запроса.

В следующем примере показан обработчик событий, присоединенный к событию `TextChanged` в XAML и использующий `ListView` для отображения результатов поиска:

```xaml
<SearchBar TextChanged="OnTextChanged" />
<ListView x:Name="searchResults" >
```

Обработчик событий можно также присоединить к `SearchBar`, созданному в коде:

```csharp
SearchBar searchBar = new SearchBar {/*...*/};
searchBar.TextChanged += OnTextChanged;
```

Обработчик событий `TextChanged` в файле кода программной части одинаков, создается ли `SearchBar` с помощью XAML или кода:

```csharp
void OnTextChanged(object sender, EventArgs e)
{
    SearchBar searchBar = (SearchBar)sender;
    searchResults.ItemsSource = DataService.GetSearchResults(searchBar.Text);
}
```

Предыдущий пример подразумевает существование класса `DataService` с методом `GetSearchResults`, способным возвращать элементы, соответствующие запросу. Значение свойства `Text` элемента управления `SearchBar` передается методу `GetSearchResults`, а результат используется для обновления свойства `ItemsSource` элемента управления `ListView`. Общий результат заключается в том, что результаты поиска отображаются в элементе управления `ListView`.

Пример приложения предоставляет `DataService` реализацию класса, которую можно использовать для тестирования функций поиска.

## <a name="perform-a-search-using-a-viewmodel"></a>Выполнение поиска с помощью ViewModel

Поиск может выполняться без обработчиков событий путем привязки свойств `SearchCommand` и `SearchCommandParameter` к реализациям `ICommand`. Пример проекта демонстрирует эти реализации с помощью шаблона Model-View-ViewModel (MVVM). Дополнительные сведения о привязках данных с помощью MVVM см. [в разделе привязки данных с помощью MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

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
> В ViewModel предполагается существование `DataService` класса, способного выполнять поиск. Класс `DataService`, включая примеры данных, доступен в примере приложения.

В следующем коде XAML показано, как привязать `SearchBar` к примеру ViewModel, с `ListView`ным элементом управления, отображающим результаты поиска:

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

Этот пример задает `BindingContext` как экземпляр класса `SearchViewModel`. Он привязывает свойство `SearchCommand` к `PerformSearch` `ICommand` в ViewModel и привязывает свойство `SearchBar` `Text` к свойству `SearchCommandParameter`. Свойство `ListView.ItemsSource` привязано к свойству `SearchResults` класса ViewModel.

Дополнительные сведения об интерфейсе `ICommand` и привязках см. в разделе [Привязка данных в Xamarin. Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md) и [интерфейс ICommand](~/xamarin-forms/app-fundamentals/data-binding/commanding.md).

## <a name="related-links"></a>Связанные ссылки

* [Демонстрации Сеарчбар](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/)
* [Текстовые элементы управления Xamarin. Forms](~/xamarin-forms/user-interface/text/index.md)
* [Шрифты в Xamarin. Forms](~/xamarin-forms/user-interface/text/fonts.md)
* [Привязка данных Xamarin. Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md)
