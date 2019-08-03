---
title: CollectionView Емптивиев Xamarin. Forms
description: В CollectionView можно указать пустое представление, которое предоставляет пользователю отзыв о том, что данные недоступны для отображения. Пустое представление может быть строкой, представлением или несколькими представлениями.
ms.prod: xamarin
ms.assetid: 6CEBCFE6-5577-4F68-9709-431062609153
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: c6a2a53f267a7f6764ec441944193e8c5ecd9189
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739194"
---
# <a name="xamarinforms-collectionview-emptyview"></a>CollectionView Емптивиев Xamarin. Forms

![](~/media/shared/preview.png "Этот API в настоящее время предоставляется в режиме предварительной версии")

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)определяет следующие свойства, которые можно использовать для предоставления отзывов пользователей, когда нет данных для показа:

- [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)Тип `object`, строка, привязка или представление, которые будут отображаться, [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) если свойство имеет `null`значение `ItemsSource` , или если коллекция, указанная свойством, имеет `null` значение или пустое значение. Значение по умолчанию — `null`.
- [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)Тип [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)— шаблон, используемый для форматирования указанного `EmptyView`объекта. Значение по умолчанию — `null`.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что свойства могут быть целевыми объектами привязок данных.

Основные сценарии использования для установки [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) свойства отображают Отзывы пользователей, когда операция [`CollectionView`](xref:Xamarin.Forms.CollectionView) фильтрации не возвращает никаких данных, и отображает Отзывы пользователей при получении данных из веб-службы.

> [!NOTE]
> Для [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) свойства можно задать представление, которое включает в себя интерактивное содержимое, если это необходимо.

Дополнительные сведения о шаблонах данных см. в разделе [Шаблоны данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="display-a-string-when-data-is-unavailable"></a>Отображать строку, если данные недоступны

`null` [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) `null` `ItemsSource` Свойству может быть присвоена строка, которая будет отображаться, если свойство имеет значение, или если коллекция, указанная свойством, имеет значение или является пустым. [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) В следующем коде XAML показан пример этого сценария:

```xaml
<CollectionView ItemsSource="{Binding EmptyMonkeys}"
                EmptyView="No items to display" />
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    EmptyView = "No items to display"
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "EmptyMonkeys");
```

В результате, поскольку коллекция, привязанная к данным, `null`имеет значение, то строка, [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) заданная в качестве значения свойства, отображается следующим образом:

[ ![Снимок экрана: вертикальный список CollectionView с пустым представлением текста, в iOS и Android](emptyview-images/null-itemssource.png "CollectionView Vertical List с пустым представлением текста") ] (emptyview-images/null-itemssource-large.png#lightbox "Вертикальный список CollectionView с пустым представлением текста")

## <a name="display-views-when-data-is-unavailable"></a>Отображать представления, если данные недоступны

`null` `ItemsSource` `null` [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) Для свойства можно задать представление, которое будет отображаться, если свойство имеет значение, или если коллекция, указанная свойством, имеет значение или является пустым. [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) Это может быть одно представление или представление, содержащее несколько дочерних представлений. В следующем примере XAML показано `EmptyView` свойство, для которого задано представление, содержащее несколько дочерних представлений:

```xaml
<StackLayout Margin="20">
    <SearchBar x:Name="searchBar"
               SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
               Placeholder="Filter" />
    <CollectionView ItemsSource="{Binding Monkeys}">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                ...
            </DataTemplate>
        </CollectionView.ItemTemplate>
        <CollectionView.EmptyView>
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
        </CollectionView.EmptyView>
    </CollectionView>
</StackLayout>
```

Эквивалентный код на C# выглядит так:

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
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
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

[`CollectionView`](xref:Xamarin.Forms.CollectionView) [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) Когда выполняет ,`FilterCommand`коллекция, отображаемая, фильтруется для условия поиска, хранящегося в свойстве. [`SearchBar`](xref:Xamarin.Forms.SearchBar) Если операция фильтрации не возвращает данные, то в [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) качестве значения свойства будет указан набор.

[ ![Снимок экрана: вертикальный список CollectionView с настраиваемым пустым представлением в вертикальном списке iOS и Android](emptyview-images/filter-multiple-views.png "CollectionView с настраиваемым пустым представлением") ] (emptyview-images/filter-multiple-views-large.png#lightbox "Вертикальный список CollectionView с настраиваемым пустым представлением")

## <a name="display-a-templated-custom-type-when-data-is-unavailable"></a>Отображать шаблонный пользовательский тип, если данные недоступны

`null` `ItemsSource` `null` [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) Для свойства можно задать пользовательский тип, шаблон которого отображается, если свойство имеет значение, или если коллекция, указанная свойством, имеет значение или является пустым. [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) Свойству может быть присвоено [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) значение, `EmptyView`определяющее внешний вид. [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) В следующем коде XAML показан пример этого сценария:

```xaml
<StackLayout Margin="20">
    <SearchBar x:Name="searchBar"
               SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
               Placeholder="Filter" />
    <CollectionView ItemsSource="{Binding Monkeys}">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                ...
            </DataTemplate>
        </CollectionView.ItemTemplate>
        <CollectionView.EmptyView>
            <views:FilterData Filter="{Binding Source={x:Reference searchBar}, Path=Text}" />
        </CollectionView.EmptyView>
        <CollectionView.EmptyViewTemplate>
            <DataTemplate>
                <Label Text="{Binding Filter, StringFormat='Your filter term of {0} did not match any records.'}"
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </DataTemplate>
        </CollectionView.EmptyViewTemplate>
    </CollectionView>
</StackLayout>
```

Эквивалентный код на C# выглядит так:

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = new FilterData { Filter = searchBar.Text },
    EmptyViewTemplate = new DataTemplate(() =>
    {
        return new Label { ... };
    })
};
```

Тип определяет свойство и соответствующий [`BindableProperty`:](xref:Xamarin.Forms.BindableProperty) `Filter` `FilterData`

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

Свойству задается `FilterData` объект, а `Filter` данные свойства привязываются к [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) свойству. [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) [`CollectionView`](xref:Xamarin.Forms.CollectionView) `Filter` Когда выполняет ,`FilterCommand`коллекция, отображаемая, фильтруется для условия поиска, хранящегося в свойстве. [`SearchBar`](xref:Xamarin.Forms.SearchBar) Если операция фильтрации не возвращает никаких данных, то [`Label`](xref:Xamarin.Forms.Label) отображается значение, заданное [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)в [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) свойстве, которое задается в качестве значения свойства.

[ ![Снимок экрана CollectionViewого вертикального списка с пустым шаблоном представления в](emptyview-images/emptyviewtemplate.png "списке по вертикали iOS и Android CollectionView с пустым шаблоном представления") ] (emptyview-images/emptyviewtemplate-large.png#lightbox "Вертикальный список CollectionView с пустым шаблоном представления")

> [!NOTE]
> При отображении пользовательского типа с шаблоном, когда данные недоступны, [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) свойству может быть присвоено представление, содержащее несколько дочерних представлений.

## <a name="choose-an-emptyview-at-runtime"></a>Выбор Емптивиев во время выполнения

Представления, которые будут отображаться [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) при недоступности данных, могут быть определены как [`ContentView`](xref:Xamarin.Forms.ContentView) объекты в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary). Во время выполнения для `ContentView` свойстваможнозадатьконкретноезначение,основанноенаопределеннойбизнес-логике.`EmptyView` В следующем примере XAML показан пример этого сценария:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CollectionViewDemos.Views.EmptyViewSwapPage"
             Title="EmptyView (swap)">
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
        <SearchBar x:Name="searchBar"
                   SearchCommand="{Binding FilterCommand}"
                   SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
                   Placeholder="Filter" />
        <StackLayout Orientation="Horizontal">
            <Label Text="Toggle EmptyViews" />
            <Switch Toggled="OnEmptyViewSwitchToggled" />
        </StackLayout>
        <CollectionView x:Name="collectionView"
                        ItemsSource="{Binding Monkeys}">
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    ...
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </StackLayout>
</ContentPage>
```

Этот XAML определяет два [`ContentView`](xref:Xamarin.Forms.ContentView) объекта на уровне [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)страницы с [`Switch`](xref:Xamarin.Forms.Switch) объектом, [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) управляющим объектом, который `ContentView` будет задан как значение свойства. При переключении `OnEmptyViewSwitchToggled` обработчик`ToggleEmptyView`событийвыполняетметод: [`Switch`](xref:Xamarin.Forms.Switch)

```csharp
void ToggleEmptyView(bool isToggled)
{
    collectionView.EmptyView = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
}
```

`ToggleEmptyView` Метод [`ContentView`](xref:Xamarin.Forms.ContentView) задает для свойства`collectionView`объектаодиниз двух [объектов`Switch.IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) , хранящихся в, в зависимости от [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) значения свойства. [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) Когда выполняет ,`FilterCommand`коллекция, отображаемая, фильтруется для условия поиска, хранящегося в свойстве. [`SearchBar`](xref:Xamarin.Forms.SearchBar) Если операция фильтрации не возвращает никаких данных, то `ContentView` объект, заданный `EmptyView` как свойство, отображается следующим образом:

[ ![Снимок экрана CollectionViewого вертикального списка с замененными пустыми представлениями в вертикальном списке iOS и Android](emptyview-images/swap.png "CollectionView с переключенными пустыми представлениями") ] (emptyview-images/swap-large.png#lightbox "Вертикальный список CollectionView с замененными пустыми представлениями")

Дополнительные сведения о словарях ресурсов см. в статье [словари ресурсов Xamarin. Forms](~/xamarin-forms/xaml/resource-dictionaries.md).

## <a name="choose-an-emptyviewtemplate-at-runtime"></a>Выбор Емптивиевтемплате во время выполнения

Внешний вид [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) можно выбрать во время выполнения на основе его значения, [`CollectionView.EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) задав для [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) свойства объект.

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CollectionViewDemos.Controls">
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
                   SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
                   Placeholder="Filter" />
        <CollectionView ItemsSource="{Binding Monkeys}"
                        EmptyView="{Binding Source={x:Reference searchBar}, Path=Text}"
                        EmptyViewTemplate="{StaticResource SearchSelector}" />
    </StackLayout>
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = searchBar.Text,
    EmptyViewTemplate = new SearchTermDataTemplateSelector { ... }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Свойству [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) задается свойство, а [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) свойству `SearchTermDataTemplateSelector` присваивается объект. [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) Когда выполняет ,`FilterCommand`коллекция, отображаемая, фильтруется для условия поиска, хранящегося в свойстве. [`SearchBar`](xref:Xamarin.Forms.SearchBar) Если операция фильтрации не возвращает никаких данных, то [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) выбранный `SearchTermDataTemplateSelector` [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) объект задается как свойство и отображается.

В следующем примере показан `SearchTermDataTemplateSelector` класс:

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

Класс определяет `DefaultTemplate` свойства и`OtherTemplate` , [длякоторыхустановленыразныешаблоныданных.`DataTemplate`](xref:Xamarin.Forms.DataTemplate) `SearchTermTemplateSelector` Переопределение возвращает `DefaultTemplate`, при котором пользователю выводится сообщение, если поисковый запрос не равен Xamarin. `OnSelectTemplate` Если поисковый запрос равен Xamarin, то `OnSelectTemplate` переопределение возвращает `OtherTemplate`, которое отображает базовое сообщение для пользователя:

[ ![Снимок экрана: Выбор шаблона пустого представления среды выполнения CollectionView в iOS и]в(emptyview-images/datatemplateselector.png "среде выполнения Android пустое выделение шаблона представления в CollectionView") ] (emptyview-images/datatemplateselector-large.png#lightbox "Выбор шаблона пустого представления среды выполнения в CollectionView")

Дополнительные сведения о селекторах шаблонов данных см. [в разделе Создание DataTemplateSelector Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Шаблоны данных Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Словари ресурсов Xamarin. Forms](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Создание DataTemplateSelector Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
