---
title: Явные стили в Xamarin.Forms
description: Явный стиль — это приложения, выборочно применять к элементам управления, задав их свойства стиля. В этой статье объясняется, как использовать явные стили в приложении Xamarin.Forms.
ms.prod: xamarin
ms.assetid: C0DF9F8F-B431-4374-A574-325BC3C41A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: b7888ea29dd02733059346bca21fec0a4c510c2e
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/03/2019
ms.locfileid: "70227994"
---
# <a name="explicit-styles-in-xamarinforms"></a>Явные стили в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_Явный стиль — это приложения, выборочно применять к элементам управления, задав их свойства стиля._

## <a name="create-an-explicit-style-in-xaml"></a>Создание явного стиля в XAML

Чтобы объявить [ `Style` ](xref:Xamarin.Forms.Style) на уровне страницы, [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) должны добавляться к странице и затем одному или нескольким `Style` объявления могут быть включены в `ResourceDictionary`. Объект `Style` становится *явные* , предоставляя его объявление `x:Key` атрибут, который присваивает ему описательное ключ в `ResourceDictionary`. *Явные* этого необходимо применить стили для определенных визуальных элементов, задав их [ `Style` ](xref:Xamarin.Forms.NavigableElement.Style) свойства.

В следующем коде показано в примере *явные* стили, объявленных в XAML, на странице `ResourceDictionary` и применены на страницу [ `Label` ](xref:Xamarin.Forms.Label) экземпляров:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="labelRedStyle" TargetType="Label">
                <Setter Property="HorizontalOptions"
                        Value="Center" />
                <Setter Property="VerticalOptions"
                        Value="CenterAndExpand" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
            <Style x:Key="labelGreenStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Green" />
            </Style>
            <Style x:Key="labelBlueStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Blue" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   Style="{StaticResource labelRedStyle}" />
            <Label Text="are demonstrating"
                   Style="{StaticResource labelGreenStyle}" />
            <Label Text="explicit styles,"
                   Style="{StaticResource labelBlueStyle}" />
            <Label Text="and an explicit style override"
                   Style="{StaticResource labelBlueStyle}"
                   TextColor="Teal" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) Определяет три *явные* стили, которые применяются к странице [ `Label` ](xref:Xamarin.Forms.Label) экземпляров. Каждый `Style` используется для отображения текста в другой цвет, а для параметра шрифт размером и горизонтальных и вертикальных макетов. Каждый `Style` применяется к другим `Label` , задав его [ `Style` ](xref:Xamarin.Forms.NavigableElement.Style) свойства с помощью `StaticResource` расширение разметки. Результат показан на следующих снимках экрана.

[![Пример явных стилей](explicit-images/explicit-styles.png)](explicit-images/explicit-styles-large.png#lightbox)

Кроме того, конечный [ `Label` ](xref:Xamarin.Forms.Label) имеет [ `Style` ](xref:Xamarin.Forms.Style) применения к нему, но также переопределяет [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) свойства различных `Color`значение.

### <a name="create-an-explicit-style-at-the-control-level"></a>Создание явного стиля на уровне элемента управления

В дополнение к созданию *явные* стили на уровне страницы, они также могут создаваться на уровне управления, как показано в следующем примере кода:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" IconImageSource="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="labelRedStyle" TargetType="Label">
                      ...
                    </Style>
                    ...
                </ResourceDictionary>
            </StackLayout.Resources>
            <Label Text="These labels" Style="{StaticResource labelRedStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

В этом примере *явные* [ `Style` ](xref:Xamarin.Forms.Style) экземпляры назначаются [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) коллекцию [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) элемента управления. Стили можно затем применяться в элемент управления и его дочерних элементов.

Сведения о создании стилей в приложения [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), см. в разделе [глобальные стили](~/xamarin-forms/user-interface/styles/application.md).

## <a name="create-an-explicit-style-in-c35"></a>Создание явного стиля в C&#35;

[`Style`](xref:Xamarin.Forms.Style) экземпляры можно добавить на страницу [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) коллекции в C# путем создания нового [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)и затем добавив `Style` экземпляры `ResourceDictionary`, как показано на Следующий пример кода:

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Red    }
            }
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Green }
            }
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Blue }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("labelRedStyle", labelRedStyle);
        Resources.Add ("labelGreenStyle", labelGreenStyle);
        Resources.Add ("labelBlueStyle", labelBlueStyle);
        ...

        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels",
                            Style = (Style)Resources ["labelRedStyle"] },
                new Label { Text = "are demonstrating",
                            Style = (Style)Resources ["labelGreenStyle"] },
                new Label { Text = "explicit styles,",
                            Style = (Style)Resources ["labelBlueStyle"] },
                new Label {    Text = "and an explicit style override",
                            Style = (Style)Resources ["labelBlueStyle"], TextColor = Color.Teal }
            }
        };
    }
}
```

Конструктор определяет три *явные* стили, которые применяются к странице [ `Label` ](xref:Xamarin.Forms.Label) экземпляров. Каждый *явные* [ `Style` ](xref:Xamarin.Forms.Style) добавляется [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) с помощью [ `Add` ](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) метод, указав `key` строки для ссылки на `Style` экземпляра. Каждый `Style` применяется к другим `Label` , задав их [ `Style` ](xref:Xamarin.Forms.NavigableElement.Style) свойства.

Однако нет никакой выгоды, с помощью [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) здесь. Вместо этого [ `Style` ](xref:Xamarin.Forms.Style) экземпляры можно назначить непосредственно к [ `Style` ](xref:Xamarin.Forms.NavigableElement.Style) свойства требуется визуальных элементов и `ResourceDictionary` может быть удален, как показано ниже пример кода:

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            ...
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            ...
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            ...
        };
        ...
        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels", Style = labelRedStyle },
                new Label { Text = "are demonstrating", Style = labelGreenStyle },
                new Label { Text = "explicit styles,", Style = labelBlueStyle },
                new Label { Text = "and an explicit style override", Style = labelBlueStyle,
                            TextColor = Color.Teal }
            }
        };
    }
}
```

Конструктор определяет три *явные* стили, которые применяются к странице [ `Label` ](xref:Xamarin.Forms.Label) экземпляров. Каждый `Style` используется для отображения текста в другой цвет, а для параметра шрифт размером и горизонтальных и вертикальных макетов. Каждый `Style` применяется к другим `Label` , задав его [ `Style` ](xref:Xamarin.Forms.NavigableElement.Style) свойства. Кроме того, конечный `Label` имеет `Style` применения к нему, но также переопределяет `TextColor` свойство с другим `Color` значение.

## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Основные стили (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
- [Работа с использованием стилей (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Стиль](xref:Xamarin.Forms.Style)
- [Метод задания](xref:Xamarin.Forms.Setter)
