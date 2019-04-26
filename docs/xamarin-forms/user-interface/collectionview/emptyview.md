---
title: Отобразить EmptyView, когда данных недоступен.
description: В CollectionView можно указать пустое представление, предоставляющий отзыв для пользователя, когда данные недоступны для отображения. Пустое представление может быть строка, представление или несколько представлений.
ms.prod: xamarin
ms.assetid: 6CEBCFE6-5577-4F68-9709-431062609153
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/19/2019
ms.openlocfilehash: a430387bba83887045e5687c99d9295d4be373e4
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61019486"
---
# <a name="display-an-emptyview-when-data-is-unavailable"></a>Отобразить EmptyView, когда данных недоступен.

![Предварительный просмотр](~/media/shared/preview.png)

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

> [!IMPORTANT]
> `CollectionView` В настоящее время доступна в предварительной версии, а не имеет части плановой функциональных возможностей. Кроме того API может измениться, как реализация завершилась.

`CollectionView` определяет следующие свойства, которые могут использоваться для предоставления отзывов пользователей, при отсутствии данных для отображения:

- `EmptyView`, типа `object`, строку, привязки или представление, которое будет, которые отображаются после `ItemsSource` свойство `null`, или когда коллекции, заданный параметром `ItemsSource` свойство `null` или пустым. Значение по умолчанию — `null`.
- `EmptyViewTemplate`, типа [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), шаблон, используемый для форматирования указанного `EmptyView`. Значение по умолчанию — `null`.

Эти свойства поддерживаются [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) объектов, что означает, что свойства могут быть целями привязки данных.

Сценарии использования основной параметр `EmptyView` свойства при отображении отзывы пользователей, когда операция фильтрации на `CollectionView` дает нет данных и отображение отзывы пользователей, пока данные извлекаются из веб-службы.

> [!NOTE]
> `EmptyView` Может быть установлено для представления, включающего интерактивное содержимое, если это необходимо.

Дополнительные сведения о шаблонах данных см. в разделе [Шаблоны данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="display-a-string-when-data-is-unavailable"></a>Отображение строки в том случае, когда данные недоступны

`EmptyView` Свойство может устанавливаться в строку, которая будет отображается, когда `ItemsSource` свойство `null`, или когда коллекции, заданный параметром `ItemsSource` свойство `null` или пустым. Следующий XAML показан пример такого сценария.

```xaml
<CollectionView ItemsSource="{Binding EmptyMonkeys}"
                EmptyView="No items to display" />
```

Ниже приведен аналогичный код C#:

```csharp
CollectionView collectionView = new CollectionView
{
    EmptyView = "No items to display"
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "EmptyMonkeys");
```

Результатом является то, что, так как коллекции с привязкой данных `null`, задать строку в качестве `EmptyView` отображается значение свойства:

[![Снимок экрана CollectionView вертикальный список с пустым представления текста, в iOS и Android](emptyview-images/null-itemssource.png "CollectionView вертикального списка с пустым представления текста")](emptyview-images/null-itemssource-large.png#lightbox "CollectionView вертикального списка с пустым текстом представление")

## <a name="display-views-when-data-is-unavailable"></a>Отображение представлений в том случае, когда данные недоступны

`EmptyView` Может быть установлено на представление, которое будет иметь отображается, когда `ItemsSource` свойство `null`, или когда коллекции, заданный параметром `ItemsSource` свойство `null` или пустым. Это может быть одно представление или представление, содержащее несколько дочерних представлений. В следующем примере показан XAML `EmptyView` свойство присвоено представление, содержащее несколько дочерних представлений:

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

Ниже приведен аналогичный код C#:

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

При [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) выполняет `FilterCommand`, отображаемого элементом коллекции `CollectionView` фильтруется по условию поиска хранимых в [ `SearchBar.Text` ](xref:Xamarin.Forms.SearchBar.Text) свойство. Если данные не дает операции фильтрации [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) задать в качестве `EmptyView` отображается значение свойства:

[![Снимок экрана CollectionView вертикальный список с помощью пользовательских пустое представление, в iOS и Android](emptyview-images/filter-multiple-views.png "CollectionView вертикального списка с помощью пользовательского представления пустой")](emptyview-images/filter-multiple-views-large.png#lightbox "CollectionView вертикального списка с пользовательским пустое представление")

## <a name="display-a-templated-custom-type-when-data-is-unavailable"></a>Отобразить шаблонного пользовательского типа, когда данные недоступны

`EmptyView` Может быть установлено для пользовательского типа, шаблон которого — отображается, когда `ItemsSource` свойство `null`, или когда коллекции, заданный параметром `ItemsSource` свойство является `null` или пустой. `EmptyViewTemplate` Свойству может быть присвоено [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) , определяющий внешний вид `EmptyView`. Следующий XAML показан пример такого сценария.

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

Ниже приведен аналогичный код C#:

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

`FilterData` Определяет тип `Filter` свойства и соответствующего [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty):

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

`EmptyView` Свойству `FilterData` объекта и `Filter` свойство данных привязывает к [ `SearchBar.Text` ](xref:Xamarin.Forms.SearchBar.Text) свойство. При [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) выполняет `FilterCommand`, отображаемого элементом коллекции `CollectionView` фильтруется по условию поиска хранимых в `Filter` свойство. Если данные не дает операции фильтрации [ `Label` ](xref:Xamarin.Forms.Label) определенные в [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), для которой включено как `EmptyViewTemplate` отображается значение свойства:

[![Снимок экрана CollectionView вертикальный список с помощью шаблона пустое представление, в iOS и Android](emptyview-images/emptyviewtemplate.png "CollectionView вертикального списка с помощью шаблона пустое представление")](emptyview-images/emptyviewtemplate-large.png#lightbox "CollectionView вертикального списка с пустое представление шаблона")

> [!NOTE]
> При отображении шаблонного пользовательского типа, когда данные недоступны, `EmptyViewTemplate` свойству может быть присвоено представление, содержащее несколько дочерних представлений.

## <a name="choose-an-emptyview-at-runtime"></a>Выберите EmptyView во время выполнения

Представления, которые будут отображаться как `EmptyView` при недоступности данных, могут быть определены как [ `ContentView` ](xref:Xamarin.Forms.ContentView) объекты в [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). `EmptyView` Затем можно задать свойства для конкретного `ContentView`в зависимости от бизнес-логику, во время выполнения. В следующем примере XAML показан пример такого сценария:

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

Этот XAML определяет два [ `ContentView` ](xref:Xamarin.Forms.ContentView) объектов на уровне страницы [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), с помощью [ `Switch` ](xref:Xamarin.Forms.Switch) объекта, управление которой `ContentView` как будет установлен объект `EmptyView` значение свойства. Когда [ `Switch` ](xref:Xamarin.Forms.Switch) переключена, `OnEmptyViewSwitchToggled` выполнения обработчика событий `ToggleEmptyView` метод:

```csharp
void ToggleEmptyView(bool isToggled)
{
    collectionView.EmptyView = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
}
```

`ToggleEmptyView` Метода задает `EmptyView` свойство `collectionView` одному из двух [ `ContentView` ](xref:Xamarin.Forms.ContentView) объектов, хранящихся в [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), основываясь на значении [ `Switch.IsToggled` ](xref:Xamarin.Forms.Switch.IsToggled) свойство. При [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) выполняет `FilterCommand`, отображаемого элементом коллекции `CollectionView` фильтруется по условию поиска хранимых в [ `SearchBar.Text` ](xref:Xamarin.Forms.SearchBar.Text) свойство. Если данные не дает операции фильтрации `ContentView` объекта в качестве `EmptyView` свойство отображается:

[![Снимок экрана CollectionView вертикальный список с местами пустой представлениями, iOS и Android](emptyview-images/swap.png "CollectionView вертикального списка с местами пустой представлениями")](emptyview-images/swap-large.png#lightbox "CollectionView вертикального списка с поменять местами пустой представления")

Дополнительные сведения о словарях ресурсов, см. в разделе [словари ресурсов Xamarin.Forms](~/xamarin-forms/xaml/resource-dictionaries.md).

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Шаблоны Xamarin.Forms данных](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Словари ресурсов Xamarin.Forms](~/xamarin-forms/xaml/resource-dictionaries.md)
