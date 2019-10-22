---
title: Карауселвиев Емптивиев Xamarin. Forms
description: В Карауселвиев можно указать пустое представление, которое предоставляет пользователю отзыв о том, что данные недоступны для отображения. Пустое представление может быть строкой, представлением или несколькими представлениями.
ms.prod: xamarin
ms.assetid: C6DEE1A9-63FC-4889-BC77-F401D5D7DF32
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/03/2019
ms.openlocfilehash: 55944b422495c9c3a7c93c6a2eab90a2db790780
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697845"
---
# <a name="xamarinforms-carouselview-emptyview"></a>Карауселвиев Емптивиев Xamarin. Forms

![](~/media/shared/preview.png "This API is currently pre-release")

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView) определяет следующие свойства, которые можно использовать для предоставления отзывов пользователей, когда нет данных для показа:

- [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView), тип `object`, строка, привязка или представление, которые будут отображаться, когда свойство [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) `null`, или если коллекция, указанная свойством `ItemsSource`, является `null` или пуста. Значение по умолчанию — `null`.
- [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate), тип [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), шаблон, используемый для форматирования указанного `EmptyView`. Значение по умолчанию — `null`.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами. Это означает, что свойства могут быть целевыми объектами привязок данных.

Основные сценарии использования для установки свойства [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) отображают Отзывы пользователей, когда операция фильтрации в [`CarouselView`](xref:Xamarin.Forms.CarouselView) не дает никаких данных и отображает Отзывы пользователей при получении данных из веб-службы.

> [!NOTE]
> Для свойства [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) можно задать представление, которое включает в себя интерактивное содержимое, если это необходимо.

Дополнительные сведения о шаблонах данных см. в разделе [Шаблоны данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="display-a-string-when-data-is-unavailable"></a>Отображать строку, если данные недоступны

Свойству [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) может быть присвоена строка, которая будет отображаться, если свойство [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) `null` или если коллекция, указанная свойством `ItemsSource`, имеет значение `null` или пусто. В следующем коде XAML показан пример этого сценария:

```xaml
<CarouselView ItemsSource="{Binding EmptyMonkeys}"
              EmptyView="No items to display." />
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView
{
    EmptyView = "No items to display."
};
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "EmptyMonkeys");
```

В результате, поскольку коллекция с привязкой к данным `null`, в качестве значения свойства [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) отображается строковый набор.

## <a name="display-views-when-data-is-unavailable"></a>Отображать представления, если данные недоступны

Свойству [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) может быть присвоено представление, которое будет отображаться, если свойство [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) `null` или если коллекция, указанная свойством `ItemsSource`, имеет значение `null` или пусто. Это может быть одно представление или представление, содержащее несколько дочерних представлений. В следующем примере XAML показано свойство `EmptyView`, для которого задано представление, содержащее несколько дочерних представлений:

```xaml
<StackLayout Margin="20">
    <SearchBar SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={RelativeSource Self}, Path=Text}"
               Placeholder="Filter" />
    <CarouselView ItemsSource="{Binding Monkeys}">
        <CarouselView.EmptyView>
            <StackLayout>
                <Label Text="No results matched your filter."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
                <Label Text="Try a broader filter?"
                       FontAttributes="Italic"
                       FontSize="12"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </CarouselView.EmptyView>
        <CarouselView.ItemTemplate>
            ...
        </CarouselView.ItemTemplate>
    </CarouselView>
</StackLayout>
```

Эквивалентный код на C# выглядит так:

```csharp
SearchBar searchBar = new SearchBar { ... };
CarouselView carouselView = new CarouselView
{
    EmptyView = new StackLayout
    {
        Children =
        {
            new Label { Text = "No results matched your filter.", ... },
            new Label { Text = "Try a broader filter?", ... }
        }
    }
};
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Когда [`SearchBar`](xref:Xamarin.Forms.SearchBar) выполняет `FilterCommand`, коллекция, отображаемая [`CarouselView`](xref:Xamarin.Forms.CarouselView) , фильтруется для условия поиска, хранящегося в свойстве [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) . Если операция фильтрации не дает никаких данных, [`StackLayout`](xref:Xamarin.Forms.StackLayout) задается как значение свойства [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) .

## <a name="display-a-templated-custom-type-when-data-is-unavailable"></a>Отображать шаблонный пользовательский тип, если данные недоступны

Свойству [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) может быть присвоено настраиваемый тип, шаблон которого отображается, если свойство [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) имеет значение `null` или если коллекция, указанная свойством `ItemsSource`, имеет значение `null` или пусто. Свойству [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) может быть присвоено значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , определяющее внешний вид `EmptyView`. В следующем коде XAML показан пример этого сценария:

```xaml
<StackLayout Margin="20">
    <SearchBar x:Name="searchBar"
               SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={RelativeSource Self}, Path=Text}"
               Placeholder="Filter" />
    <CarouselView ItemsSource="{Binding Monkeys}">
        <CarouselView.EmptyView>
            <controls:FilterData Filter="{Binding Source={x:Reference searchBar}, Path=Text}" />
        </CarouselView.EmptyView>
        <CarouselView.EmptyViewTemplate>
            <DataTemplate>
                <Label Text="{Binding Filter, StringFormat='Your filter term of {0} did not match any records.'}"
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </DataTemplate>
        </CarouselView.EmptyViewTemplate>
        <CarouselView.ItemTemplate>
            ...
        </CarouselView.ItemTemplate>
    </CarouselView>
</StackLayout>
```

Эквивалентный код на C# выглядит так:

```csharp
SearchBar searchBar = new SearchBar { ... };
CarouselView carouselView = new CarouselView
{
    EmptyView = new FilterData { Filter = searchBar.Text },
    EmptyViewTemplate = new DataTemplate(() =>
    {
        return new Label { ... };
    })
};
```

Тип `FilterData` определяет свойство `Filter` и соответствующий [`BindableProperty`](xref:Xamarin.Forms.BindableProperty):

```csharp
public class FilterData : BindableObject
{
    public static readonly BindableProperty FilterProperty = BindableProperty.Create(nameof(Filter), typeof(string), typeof(FilterData), null);

    public string Filter
    {
        get { return (string)GetValue(FilterProperty); }
        set { SetValue(FilterProperty, value); }
    }
}
```

Свойству [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) присвоено `FilterData` объект, а данные свойства `Filter` привязываются к свойству [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) . Когда [`SearchBar`](xref:Xamarin.Forms.SearchBar) выполняет `FilterCommand`, коллекция, отображаемая [`CarouselView`](xref:Xamarin.Forms.CarouselView) , фильтруется для условия поиска, хранящегося в свойстве `Filter`. Если операция фильтрации не дает никаких данных, то отображается [`Label`](xref:Xamarin.Forms.Label) , определенная в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), которая задается как значение свойства [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) .

> [!NOTE]
> При отображении пользовательского типа с шаблоном, когда данные недоступны, свойству [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) может быть присвоено представление, содержащее несколько дочерних представлений.

## <a name="choose-an-emptyview-at-runtime"></a>Выбор Емптивиев во время выполнения

Представления, которые будут отображаться в виде [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) при недоступности данных, могут быть определены как [`ContentView`](xref:Xamarin.Forms.ContentView) объекты в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary). В качестве значения свойства `EmptyView` можно задать определенный `ContentView` в зависимости от конкретной бизнес-логики во время выполнения. В следующем примере XAML показан пример этого сценария:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:viewmodels="clr-namespace:CarouselViewDemos.ViewModels"
             x:Class="CarouselViewDemos.Views.EmptyViewSwapPage"
             Title="EmptyView (swap)">
    <ContentPage.BindingContext>
        <viewmodels:MonkeysViewModel />
    </ContentPage.BindingContext>
    <ContentPage.Resources>
        <ContentView x:Key="BasicEmptyView">
            <StackLayout>
                <Label Text="No items to display."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </ContentView>
        <ContentView x:Key="AdvancedEmptyView">
            <StackLayout>
                <Label Text="No results matched your filter."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
                <Label Text="Try a broader filter?"
                       FontAttributes="Italic"
                       FontSize="12"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </ContentView>
    </ContentPage.Resources>
    <StackLayout Margin="20">
        <SearchBar SearchCommand="{Binding FilterCommand}"
                   SearchCommandParameter="{Binding Source={RelativeSource Self}, Path=Text}"
                   Placeholder="Filter" />
        <StackLayout Orientation="Horizontal">
            <Label Text="Toggle EmptyViews" />
            <Switch Toggled="OnEmptyViewSwitchToggled" />
        </StackLayout>
        <CarouselView x:Name="carouselView"
                      ItemsSource="{Binding Monkeys}">
            <CarouselView.ItemTemplate>
                ...
            </CarouselView.ItemTemplate>
        </CarouselView>
    </StackLayout>
</ContentPage>
```

Этот XAML определяет два [`ContentView`](xref:Xamarin.Forms.ContentView) объектов в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)на уровне страницы с объектом [`Switch`](xref:Xamarin.Forms.Switch) , контролирующим, какой `ContentView` объект будет установлен в качестве значения свойства [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) . При переключении [`Switch`](xref:Xamarin.Forms.Switch) обработчик событий `OnEmptyViewSwitchToggled` выполняет метод `ToggleEmptyView`:

```csharp
void ToggleEmptyView(bool isToggled)
{
    carouselView.EmptyView = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
}
```

Метод `ToggleEmptyView` задает для свойства [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) объекта `carouselView` один из двух объектов [`ContentView`](xref:Xamarin.Forms.ContentView) , хранящихся в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), в зависимости от значения свойства [`Switch.IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) . Когда [`SearchBar`](xref:Xamarin.Forms.SearchBar) выполняет `FilterCommand`, коллекция, отображаемая [`CarouselView`](xref:Xamarin.Forms.CarouselView) , фильтруется для условия поиска, хранящегося в свойстве [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) . Если операция фильтрации не дает никаких данных, то будет отображен `ContentView` объект, заданный как свойство `EmptyView`.

Дополнительные сведения о словарях ресурсов см. в статье [словари ресурсов Xamarin. Forms](~/xamarin-forms/xaml/resource-dictionaries.md).

## <a name="choose-an-emptyviewtemplate-at-runtime"></a>Выбор Емптивиевтемплате во время выполнения

Внешний вид [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) можно выбрать во время выполнения на основе его значения, задав для свойства [`CarouselView.EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) объект [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) :

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CarouselViewDemos.Controls">
    <ContentPage.Resources>
        <DataTemplate x:Key="AdvancedTemplate">
            ...
        </DataTemplate>

        <DataTemplate x:Key="BasicTemplate">
            ...
        </DataTemplate>

        <controls:SearchTermDataTemplateSelector x:Key="SearchSelector"
                                                 DefaultTemplate="{StaticResource AdvancedTemplate}"
                                                 OtherTemplate="{StaticResource BasicTemplate}" />
    </ContentPage.Resources>

    <StackLayout Margin="20">
        <SearchBar x:Name="searchBar"
                   SearchCommand="{Binding FilterCommand}"
                   SearchCommandParameter="{Binding Source={RelativeSource Self}, Path=Text}"
                   Placeholder="Filter" />
        <CarouselView ItemsSource="{Binding Monkeys}"
                      EmptyView="{Binding Source={x:Reference searchBar}, Path=Text}"
                      EmptyViewTemplate="{StaticResource SearchSelector}">
            <CarouselView.ItemTemplate>
                ...
            </CarouselView.ItemTemplate>
        </CarouselView>
    </StackLayout>
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
SearchBar searchBar = new SearchBar { ... };
CarouselView carouselView = new CarouselView()
{
    EmptyView = searchBar.Text,
    EmptyViewTemplate = new SearchTermDataTemplateSelector { ... }
};
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Свойству [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) присвоено свойство [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) , а свойству [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) — объект `SearchTermDataTemplateSelector`.

Когда [`SearchBar`](xref:Xamarin.Forms.SearchBar) выполняет `FilterCommand`, коллекция, отображаемая [`CarouselView`](xref:Xamarin.Forms.CarouselView) , фильтруется для условия поиска, хранящегося в свойстве [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) . Если операция фильтрации не дает никаких данных, [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , выбранная объектом `SearchTermDataTemplateSelector`, задается как свойство [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) и отображается.

В следующем примере показан класс `SearchTermDataTemplateSelector`:

```csharp
public class SearchTermDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate OtherTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        string query = (string)item;
        return query.ToLower().Equals("xamarin") ? OtherTemplate : DefaultTemplate;
    }
}
```

Класс `SearchTermTemplateSelector` определяет `DefaultTemplate` и `OtherTemplate` свойства [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , для которых заданы разные шаблоны данных. Переопределение `OnSelectTemplate` возвращает `DefaultTemplate`, которое отображает сообщение пользователю, если поисковый запрос не равен Xamarin. Если поисковый запрос равен Xamarin, то переопределение `OnSelectTemplate` возвращает `OtherTemplate`, которое отображает базовое сообщение для пользователя.

Дополнительные сведения о селекторах шаблонов данных см. [в разделе Создание DataTemplateSelector Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Связанные ссылки

- [Карауселвиев (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [Шаблоны данных Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Словари ресурсов Xamarin. Forms](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Создание DataTemplateSelector Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
