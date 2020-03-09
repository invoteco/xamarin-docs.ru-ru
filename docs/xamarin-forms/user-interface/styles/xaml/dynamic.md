---
title: Динамические стили в Xamarin.Forms
description: В этой статье объясняется, как приложения Xamarin.Forms можно реагировать на изменения стиля динамически во время выполнения с помощью динамических ресурсов.
ms.prod: xamarin
ms.assetid: 13D4FA4B-DF10-42BF-B001-2C49367FC216
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/28/2019
ms.custom: video
ms.openlocfilehash: 9a26532d13b843b812da94739be071c7accac212
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/07/2020
ms.locfileid: "78918648"
---
# <a name="dynamic-styles-in-xamarinforms"></a>Динамические стили в Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)

_Стили не реагируют на изменения свойств и остаются неизменными в течение всего приложения. Например, после назначения стиля визуальному элементу, если один из экземпляров Setter изменяется, удаляется или добавляется новый экземпляр метода задания, изменения не будут применены к визуальному элементу. Однако приложения могут динамически реагировать на изменения стиля во время выполнения с помощью динамических ресурсов._

Расширение разметки `DynamicResource` аналогично расширению разметки `StaticResource` в том, что оба используют ключ словаря для выборки значения из [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary). Однако, хотя `StaticResource` выполняет поиск по одному словарю, `DynamicResource` сохраняет ссылку на ключ словаря. Таким образом при замене элемента словаря, связанное с ключом, изменение будет применено к визуальный элемент. Благодаря этому среда выполнения стиля вносить изменения в приложении.

В следующем примере кода показаны *динамические* стили на странице XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesPage" Title="Dynamic" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
            <Style x:Key="blueSearchBarStyle"
                   TargetType="SearchBar"
                   BasedOn="{StaticResource baseStyle}">
              ...
            </Style>
            <Style x:Key="greenSearchBarStyle"
                   TargetType="SearchBar">
              ...
            </Style>
            ...
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <SearchBar Placeholder="These SearchBar controls"
                       Style="{DynamicResource searchBarStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Экземпляры [`SearchBar`](xref:Xamarin.Forms.SearchBar) используют расширение разметки `DynamicResource` для ссылки на [`Style`](xref:Xamarin.Forms.Style) с именем `searchBarStyle`, которое не определено в XAML. Однако, поскольку свойства [`Style`](xref:Xamarin.Forms.NavigableElement.Style) экземпляров `SearchBar` задаются с помощью `DynamicResource`, отсутствующий ключ словаря не приводит к созданию исключения.

Вместо этого в файле кода программной части конструктор создает запись [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) с ключевым `searchBarStyle`, как показано в следующем примере кода:

```csharp
public partial class DynamicStylesPage : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesPage ()
    {
        InitializeComponent ();
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];
    }

    void OnButtonClicked (object sender, EventArgs e)
    {
        if (originalStyle) {
            Resources ["searchBarStyle"] = Resources ["greenSearchBarStyle"];
            originalStyle = false;
        } else {
            Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];
            originalStyle = true;
        }
    }
}
```

При выполнении обработчика `OnButtonClicked` событий `searchBarStyle` переключается между `blueSearchBarStyle` и `greenSearchBarStyle`. Результат показан на следующих снимках экрана:

[![Пример динамического](dynamic-images/dynamic-style-blue.png)](dynamic-images/dynamic-style-blue-large.png#lightbox)
[![стиля"синий"впримересзеленымстилем](dynamic-images/dynamic-style-green.png)](dynamic-images/dynamic-style-green-large.png#lightbox)

В следующем примере кода показаны эквивалентные страницы на языке C#:

```csharp
public class DynamicStylesPageCS : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesPageCS ()
    {
        ...
        var baseStyle = new Style (typeof(View)) {
            ...
        };
        var blueSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var greenSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        ...
        var searchBar1 = new SearchBar { Placeholder = "These SearchBar controls" };
        searchBar1.SetDynamicResource (VisualElement.StyleProperty, "searchBarStyle");
        ...
        Resources = new ResourceDictionary ();
        Resources.Add ("blueSearchBarStyle", blueSearchBarStyle);
        Resources.Add ("greenSearchBarStyle", greenSearchBarStyle);
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];

        Content = new StackLayout {
            Children = { searchBar1, searchBar2, searchBar3, searchBar4,    button    }
        };
    }
    ...
}
```

В C# [`SearchBar`](xref:Xamarin.Forms.SearchBar) экземпляры используют метод [`SetDynamicResource`](xref:Xamarin.Forms.Element.SetDynamicResource*) для ссылки на `searchBarStyle`. Код обработчика событий `OnButtonClicked` идентичен примеру XAML, и при выполнении `searchBarStyle` переключается между `blueSearchBarStyle` и `greenSearchBarStyle`.

## <a name="dynamic-style-inheritance"></a>Динамическое наследование стиля

Невозможно получить стиль из динамического стиля с помощью свойства [`Style.BasedOn`](xref:Xamarin.Forms.Style.BasedOn) . Вместо этого класс [`Style`](xref:Xamarin.Forms.Style) включает свойство [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) , для которого можно задать ключ словаря, значение которого может динамически изменяться.

В следующем примере кода показано *динамическое* наследование стиля на странице XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesInheritancePage" Title="Dynamic Inheritance" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
            <Style x:Key="blueSearchBarStyle" TargetType="SearchBar" BasedOn="{StaticResource baseStyle}">
              ...
            </Style>
            <Style x:Key="greenSearchBarStyle" TargetType="SearchBar">
              ...
            </Style>
            <Style x:Key="tealSearchBarStyle" TargetType="SearchBar" BaseResourceKey="searchBarStyle">
              ...
            </Style>
            ...
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <SearchBar Text="These SearchBar controls" Style="{StaticResource tealSearchBarStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Экземпляры [`SearchBar`](xref:Xamarin.Forms.SearchBar) используют расширение разметки `StaticResource` для ссылки на [`Style`](xref:Xamarin.Forms.Style) с именем `tealSearchBarStyle`. Этот `Style` задает некоторые дополнительные свойства и использует свойство [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) для ссылки на `searchBarStyle`. Расширение разметки `DynamicResource` не требуется, так как `tealSearchBarStyle` не изменится, за исключением того, `Style` он является производным от. Таким образом, `tealSearchBarStyle` поддерживает ссылку на `searchBarStyle` и изменяется при изменении базового стиля.

В файле кода программной части конструктор создает запись [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) с ключом `searchBarStyle`, как в предыдущем примере, в котором демонстрируются динамические стили. При выполнении обработчика `OnButtonClicked` событий `searchBarStyle` переключается между `blueSearchBarStyle` и `greenSearchBarStyle`. Результат показан на следующих снимках экрана:

[![](dynamic-images/dynamic-style-inheritance-blue.png)](dynamic-images/dynamic-style-inheritance-blue-large.png#lightbox)
[![Примеробщего наследования динамического стиля с синим примером](dynamic-images/dynamic-style-inheritance-green.png)](dynamic-images/dynamic-style-inheritance-green-large.png#lightbox)

В следующем примере кода показаны эквивалентные страницы на языке C#:

```csharp
public class DynamicStylesInheritancePageCS : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesInheritancePageCS ()
    {
        ...
        var baseStyle = new Style (typeof(View)) {
            ...
        };
        var blueSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var greenSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var tealSearchBarStyle = new Style (typeof(SearchBar)) {
            BaseResourceKey = "searchBarStyle",
            ...
        };
        ...
        Resources = new ResourceDictionary ();
        Resources.Add ("blueSearchBarStyle", blueSearchBarStyle);
        Resources.Add ("greenSearchBarStyle", greenSearchBarStyle);
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];

        Content = new StackLayout {
            Children = {
                new SearchBar { Text = "These SearchBar controls", Style = tealSearchBarStyle },
                ...
            }
        };
    }
    ...
}
```

`tealSearchBarStyle` присваивается непосредственно свойству [`Style`](xref:Xamarin.Forms.NavigableElement.Style) экземпляров [`SearchBar`](xref:Xamarin.Forms.SearchBar) . Этот `Style` задает некоторые дополнительные свойства и использует свойство [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) для ссылки на `searchBarStyle`. Метод [`SetDynamicResource`](xref:Xamarin.Forms.Element.SetDynamicResource*) не требуется, так как `tealSearchBarStyle` не изменится, за исключением `Style`, производного от. Таким образом, `tealSearchBarStyle` поддерживает ссылку на `searchBarStyle` и изменяется при изменении базового стиля.

## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Динамические стили (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)
- [Работа со стилями (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Style](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Dynamic-Resources/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
