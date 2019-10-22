---
title: Общие сведения о стилях Xamarin. Forms
description: Стили позволяют настраивать внешний вид визуальных элементов. Стили определяются для конкретного типа и содержат значения для свойств, доступных для этого типа.
ms.prod: xamarin
ms.assetid: 3FF899C0-6CFB-4C1D-837D-9E9E10181967
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 35f8dad3590c07ceb3c93aa735b8c02d75098498
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "70228170"
---
# <a name="introduction-to-xamarinforms-styles"></a>Общие сведения о стилях Xamarin. Forms

_Стили позволяют настраивать внешний вид визуальных элементов. Стили определяются для конкретного типа и содержат значения для свойств, доступных для этого типа._

Приложения Xamarin. Forms часто содержат несколько элементов управления, имеющих одинаковый внешний вид. Например, приложение может иметь несколько [`Label`](xref:Xamarin.Forms.Label) экземпляров с одинаковыми параметрами шрифта и параметрами макета, как показано в следующем примере кода XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="Styles.NoStylesPage"
    Title="No Styles"
    IconImageSource="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="are not"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="using styles"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

В следующем примере кода создается эквивалентная страница на языке C#:

```csharp
public class NoStylesPageCS : ContentPage
{
    public NoStylesPageCS ()
    {
        Title = "No Styles";
        IconImageSource = "csharp.png";
        Padding = new Thickness (0, 20, 0, 0);

        Content = new StackLayout {
            Children = {
                new Label {
                    Text = "These labels",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "are not",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "using styles",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                }
            }
        };
    }
}
```

Каждый экземпляр [`Label`](xref:Xamarin.Forms.Label) имеет одинаковые значения свойств для управления внешним видом текста, отображаемого `Label`. Результат показан на следующих снимках экрана.

[Внешний вид ![Label без стилей](introduction-images/no-styles.png)](introduction-images/no-styles-large.png#lightbox)

Настройка внешнего вида каждого отдельного элемента управления может быть повторена и подвержена ошибкам. Вместо этого можно создать стиль, определяющий внешний вид, а затем применить его к необходимым элементам управления.

## <a name="create-a-style"></a>Создание стиля

Класс [`Style`](xref:Xamarin.Forms.Style) группирует коллекцию значений свойств в один объект, который затем можно применить к нескольким экземплярам визуальных элементов. Это помогает уменьшить повторяющуюся разметку и позволяет упростить изменение внешнего вида приложений.

Хотя стили разрабатывались в основном для приложений на основе XAML, их также можно создавать в C#:

- [`Style`](xref:Xamarin.Forms.Style) экземпляры, созданные в XAML, обычно определяются в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) , назначенном коллекции [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) элемента управления, страницы или коллекции [`Resources`](xref:Xamarin.Forms.Application.Resources) приложения.
- [`Style`](xref:Xamarin.Forms.Style) экземпляры, созданные C# в, обычно определяются в классе страницы или в классе, доступ к которому можно получить глобально.

От того, где определен шаблон [`Style`](xref:Xamarin.Forms.Style), зависит то, где его можно использовать.

- [`Style`](xref:Xamarin.Forms.Style) экземпляры, определенные на уровне элемента управления, могут быть применены только к элементу управления и его дочерним элементам.
- [`Style`](xref:Xamarin.Forms.Style) экземпляры, определенные на уровне страницы, могут быть применены только к странице и ее дочерним элементам.
- [`Style`](xref:Xamarin.Forms.Style) экземпляры, определенные на уровне приложения, можно применять во всем приложении.

Каждый экземпляр [`Style`](xref:Xamarin.Forms.Style) содержит коллекцию из одного или нескольких объектов [`Setter`](xref:Xamarin.Forms.Setter) , каждый из которых `Setter` имеет [`Property`](xref:Xamarin.Forms.Setter.Property) и [`Value`](xref:Xamarin.Forms.Setter.Value). @No__t_0 — это имя привязываемого свойства элемента, к которому применяется стиль, а `Value` — это значение, применяемое к свойству.

Каждый экземпляр [`Style`](xref:Xamarin.Forms.Style) может быть *явным*или *неявным*:

- *Явный* [`Style`](xref:Xamarin.Forms.Style) экземпляр определяется путем указания [`TargetType`](xref:Xamarin.Forms.Style.TargetType) и значения `x:Key`, а также путем установки свойства [`Style`](xref:Xamarin.Forms.NavigableElement.Style) целевого элемента в ссылку `x:Key`. Дополнительные сведения о *явных* стилях см. в разделе [явные стили](~/xamarin-forms/user-interface/styles/explicit.md).
- *Неявный* экземпляр [`Style`](xref:Xamarin.Forms.Style) определяется путем указания только [`TargetType`](xref:Xamarin.Forms.Style.TargetType). Экземпляр `Style` будет автоматически применен ко всем элементам этого типа. Обратите внимание, что в подклассах `TargetType` не применяются автоматически `Style`. Дополнительные сведения о *неявных* стилях см. в разделе [Неявные стили](~/xamarin-forms/user-interface/styles/implicit.md).

При создании [`Style`](xref:Xamarin.Forms.Style)всегда требуется свойство [`TargetType`](xref:Xamarin.Forms.Style.TargetType) . В следующем примере кода показан *явный* стиль (Обратите внимание на `x:Key`), СОЗДАННЫЙ в XAML:

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

Чтобы применить `Style`, целевой объект должен быть [`VisualElement`](xref:Xamarin.Forms.VisualElement) , который соответствует значению свойства [`TargetType`](xref:Xamarin.Forms.Style.TargetType) `Style`, как показано в следующем примере кода XAML:

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

Стили ниже в иерархии представлений имеют приоритет над теми, которые определены выше. Например, задание [`Style`](xref:Xamarin.Forms.Style) , которое задает [`Label.TextColor`](xref:Xamarin.Forms.Label.TextColor) для `Red` на уровне приложения, будет переопределено стилем уровня страницы, устанавливающим `Label.TextColor` в `Green`. Аналогичным образом, стиль уровня страницы будет переопределен стилем уровня элемента управления. Кроме того, если `Label.TextColor` задается непосредственно в свойстве элемента управления, это имеет приоритет над любыми стилями.

В статьях этого раздела показано, как создавать и применять *явные* и *неявные* стили, как создавать глобальные стили, наследование стилей, как реагировать на изменения стиля во время выполнения, а также как использовать встроенные стили, входящие в Xamarin. Forms.

> [!NOTE]
> **Что такое Стилеид?**
>
> До Xamarin. Forms 2,2 свойство [`StyleId`](xref:Xamarin.Forms.Element.StyleId) использовалось для идентификации отдельных элементов в приложении для идентификации в тестировании пользовательского интерфейса, а также в механизмах тем, например пиксате. Однако Xamarin. Forms 2,2 представил свойство [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) , заменяя свойство [`StyleId`](xref:Xamarin.Forms.Element.StyleId) .

## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Стиль](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
