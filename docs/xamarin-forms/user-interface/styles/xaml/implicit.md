---
title: Неявные стили в Xamarin.Forms
description: Неявный стиль, которое используется для всех элементов управления из же TargetType, без необходимости каждый элемент управления для ссылки на стиль.
ms.prod: xamarin
ms.assetid: 02A75F3B-4389-49D4-A2F4-AFD473A4A161
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/30/2019
ms.openlocfilehash: d58ba81596cccf470b7246514d71f35968599880
ms.sourcegitcommit: ccbf914615c0ce6b3f308d930f7a77418aeb4dbc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/11/2020
ms.locfileid: "77131059"
---
# <a name="implicit-styles-in-xamarinforms"></a>Неявные стили в Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_Неявный стиль — это тот, который используется всеми элементами управления одного и того же TargetType, не требуя, чтобы каждый элемент управления ссылался на стиль._

## <a name="create-an-implicit-style-in-xaml"></a>Создание неявного стиля в XAML

Чтобы объявить [`Style`](xref:Xamarin.Forms.Style) на уровне страницы, на страницу необходимо добавить [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) , а затем одно или несколько объявлений `Style` могут быть включены в `ResourceDictionary`. `Style` делается *неявным* , не указывая атрибут `x:Key`. Затем стиль будет применяться к визуальным элементам, соответствующим `TargetType` в точности, но не к элементам, производным от значения `TargetType`.

В следующем примере кода показан *неявный* стиль, объявленный в XAML в `ResourceDictionary`страницы и применяемый к экземплярам [`Entry`](xref:Xamarin.Forms.Entry) страницы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Entry">
                <Setter Property="HorizontalOptions" Value="Fill" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                <Setter Property="BackgroundColor" Value="Yellow" />
                <Setter Property="FontAttributes" Value="Italic" />
                <Setter Property="TextColor" Value="Blue" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Entry Text="These entries" />
            <Entry Text="are demonstrating" />
            <Entry Text="implicit styles," />
            <Entry Text="and an implicit style override" BackgroundColor="Lime" TextColor="Red" />
            <local:CustomEntry Text="Subclassed Entry is not receiving the style" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) определяет один *неявный* стиль, применяемый к экземплярам [`Entry`](xref:Xamarin.Forms.Entry) страницы. `Style` используется для отображения синего текста на желтом фоне, а также для настройки других параметров внешнего вида. `Style` добавляется в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) страницы без указания атрибута `x:Key`. Таким образом, `Style` применяется ко всем экземплярам `Entry` неявным образом, так как они соответствуют свойству [`TargetType`](xref:Xamarin.Forms.Style.TargetType) `Style`. Однако `Style` не применяется к экземпляру `CustomEntry`, который является подклассом `Entry`. Результат показан на следующих снимках экрана:

[Пример неявных стилей ![](implicit-images/implicit-styles.png)](implicit-images/implicit-styles-large.png#lightbox)

Кроме того, четвертый [`Entry`](xref:Xamarin.Forms.Entry) переопределяет свойства [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) и [`TextColor`](xref:Xamarin.Forms.InputView.TextColor) неявного стиля на различные значения `Color`.

### <a name="create-an-implicit-style-at-the-control-level"></a>Создание неявного стиля на уровне элемента управления

Помимо создания *неявных* стилей на уровне страницы их также можно создать на уровне элемента управления, как показано в следующем примере кода:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" IconImageSource="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style TargetType="Entry">
                        <Setter Property="HorizontalOptions" Value="Fill" />
                        ...
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            <Entry Text="These entries" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

В этом примере *неявное* [`Style`](xref:Xamarin.Forms.Style) присваивается коллекции [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) элемента управления [`StackLayout`](xref:Xamarin.Forms.StackLayout) . После этого *неявный* стиль можно применить к элементу управления и его дочерним элементам.

Сведения о создании стилей в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)приложения см. в разделе [глобальные стили](~/xamarin-forms/user-interface/styles/application.md).

## <a name="create-an-implicit-style-in-c35"></a>Создание неявного стиля в C&#35;

[`Style`](xref:Xamarin.Forms.Style) экземпляры можно добавить в коллекцию [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) страницы в C# , создав новый [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), а затем добавив `Style` экземпляры в `ResourceDictionary`, как показано в следующем примере кода:

```csharp
public class ImplicitStylesPageCS : ContentPage
{
    public ImplicitStylesPageCS ()
    {
        var entryStyle = new Style (typeof(Entry)) {
            Setters = {
                ...
                new Setter { Property = Entry.TextColorProperty, Value = Color.Blue }
            }
        };

        ...
        Resources = new ResourceDictionary ();
        Resources.Add (entryStyle);

        Content = new StackLayout {
            Children = {
                new Entry { Text = "These entries" },
                new Entry { Text = "are demonstrating" },
                new Entry { Text = "implicit styles," },
                new Entry { Text = "and an implicit style override", BackgroundColor = Color.Lime, TextColor = Color.Red },
                new CustomEntry  { Text = "Subclassed Entry is not receiving the style" }
            }
        };
    }
}
```

Конструктор определяет один *неявный* стиль, применяемый к экземплярам [`Entry`](xref:Xamarin.Forms.Entry) страницы. `Style` используется для отображения синего текста на желтом фоне, а также для настройки других параметров внешнего вида. `Style` добавляется в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) страницы без указания `key` строки. Таким образом, `Style` применяется ко всем экземплярам `Entry` неявным образом, так как они соответствуют свойству [`TargetType`](xref:Xamarin.Forms.Style.TargetType) `Style`. Однако `Style` не применяется к экземпляру `CustomEntry`, который является подклассом `Entry`.

## <a name="apply-a-style-to-derived-types"></a>Применить стиль к производным типам

Свойство [`Style.ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) позволяет применять стиль к элементам управления, производным от базового типа, на который ссылается свойство [`TargetType`](xref:Xamarin.Forms.Style.TargetType) . Поэтому присвоение этому свойству значения `true` позволяет использовать один стиль для нескольких типов при условии, что типы являются производными от базового типа, указанного в свойстве `TargetType`.

В следующем примере показан неявный стиль, который устанавливает красный цвет фона для [`Button`ных](xref:Xamarin.Forms.Button) экземпляров:

```xaml
<Style TargetType="Button"
       ApplyToDerivedTypes="True">
    <Setter Property="BackgroundColor"
            Value="Red" />
</Style>
```

Если поместить этот стиль в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) на уровне страницы, он будет применен ко всем экземплярам [`Button`](xref:Xamarin.Forms.Button) на странице, а также ко всем элементам управления, производным от `Button`. Однако если свойство [`ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) оставалось неопределенным, стиль будет применен только к экземплярам `Button`.

Эквивалентный код на C# выглядит так:

```csharp
var buttonStyle = new Style(typeof(Button))
{
    ApplyToDerivedTypes = true,
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.Red
        }
    }
};

Resources = new ResourceDictionary { buttonStyle };
```

## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Базовые стили (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
- [Работа со стилями (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Style](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
