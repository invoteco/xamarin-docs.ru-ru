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
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2019
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

Каждый [`Label`](xref:Xamarin.Forms.Label) экземпляр имеет одинаковые значения свойств для управления внешним видом текста, отображаемого `Label`. Результат показан на следующих снимках экрана.

[![Внешний вид метки без стилей](introduction-images/no-styles.png)](introduction-images/no-styles-large.png#lightbox)

Настройка внешнего вида каждого отдельного элемента управления может быть повторена и подвержена ошибкам. Вместо этого можно создать стиль, определяющий внешний вид, а затем применить его к необходимым элементам управления.

## <a name="create-a-style"></a>Создание стиля

[`Style`](xref:Xamarin.Forms.Style) Класс группирует коллекцию значений свойств в один объект, который затем можно применить к нескольким экземплярам визуальных элементов. Это помогает уменьшить повторяющуюся разметку и позволяет упростить изменение внешнего вида приложений.

Хотя стили разрабатывались в основном для приложений на основе XAML, их также можно создавать в C#:

- [`Style`](xref:Xamarin.Forms.Style)экземпляры, созданные в XAML, обычно определяются [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) в, который назначается [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) коллекции элемента управления, [`Resources`](xref:Xamarin.Forms.Application.Resources) страницы или коллекции приложения.
- [`Style`](xref:Xamarin.Forms.Style)экземпляры, созданные C# в, обычно определяются в классе страницы или в классе, доступ к которому можно получить глобально.

От того, где определен шаблон [`Style`](xref:Xamarin.Forms.Style), зависит то, где его можно использовать.

- [`Style`](xref:Xamarin.Forms.Style)экземпляры, определенные на уровне элемента управления, могут быть применены только к элементу управления и его дочерним элементам.
- [`Style`](xref:Xamarin.Forms.Style)экземпляры, определенные на уровне страницы, могут быть применены только к странице и ее дочерним элементам.
- [`Style`](xref:Xamarin.Forms.Style)экземпляры, определенные на уровне приложения, можно применять ко всему приложению.

Каждый [`Style`](xref:Xamarin.Forms.Style) экземпляр содержит коллекцию из одного или нескольких [`Setter`](xref:Xamarin.Forms.Setter) объектов [`Property`](xref:Xamarin.Forms.Setter.Property) , [`Value`](xref:Xamarin.Forms.Setter.Value)каждый `Setter` из которых имеет и. — Это имя привязываемого свойства элемента, к которому применяется стиль, `Value` а — это значение, которое применяется к свойству. `Property`

Каждый [`Style`](xref:Xamarin.Forms.Style) экземпляр может быть *явным*или *неявным*:

- *Явный* [`Style`](xref:Xamarin.Forms.Style) экземпляр [`Style`](xref:Xamarin.Forms.NavigableElement.Style) определяется с помощью`x:Key` указания изначения`x:Key` , а также путем присвоения свойству целевого элемента ссылки. [`TargetType`](xref:Xamarin.Forms.Style.TargetType) Дополнительные сведения о *явных* стилях см. в разделе [явные стили](~/xamarin-forms/user-interface/styles/explicit.md).
- *Неявный* [`Style`](xref:Xamarin.Forms.Style) экземпляр определяется [`TargetType`](xref:Xamarin.Forms.Style.TargetType)путем указания только. Затем `Style` экземпляр будет автоматически применен ко всем элементам этого типа. Обратите внимание, что в `TargetType` `Style` подклассах объекта не применяется автоматически. Дополнительные сведения о *неявных* стилях см. в разделе [Неявные стили](~/xamarin-forms/user-interface/styles/implicit.md).

При создании [`Style`](xref:Xamarin.Forms.Style) [`TargetType`](xref:Xamarin.Forms.Style.TargetType) свойство всегда является обязательным. В следующем примере кода показан *явный* стиль (Примечание), `x:Key`созданный в XAML:

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

Чтобы применить `Style`объект, целевой объект должен [`VisualElement`](xref:Xamarin.Forms.VisualElement) иметь значение, совпадающее [`TargetType`](xref:Xamarin.Forms.Style.TargetType) со значением `Style`свойства, как показано в следующем примере кода XAML:

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

Стили более низкого уровня в иерархии представлений имеют приоритет над определенных выше вверх. Например, установка [ `Style` ](xref:Xamarin.Forms.Style) , задает [ `Label.TextColor` ](xref:Xamarin.Forms.Label.TextColor) для `Red` приложения будут переопределены уровень стиль уровня страницы, который задает `Label.TextColor` для `Green`. Аналогичным образом стиль уровня будут переопределены уровня стиль элемента управления. Кроме того, если `Label.TextColor` свойство задано непосредственно для свойства элемента управления, это имеет приоритет над любыми стилями.

В статьях этого раздела показано, как создавать и применять *явные* и *неявные* стили, как создавать глобальные стили, наследование стилей, как реагировать на изменения стиля во время выполнения, а также как использовать встроенные стили, входящие в Xamarin. Forms.

> [!NOTE]
> **Что такое Стилеид?**
>
> До Xamarin. Forms 2,2 [`StyleId`](xref:Xamarin.Forms.Element.StyleId) свойство было использовано для идентификации отдельных элементов в приложении для идентификации в тестировании пользовательского интерфейса, а также в механизмах тем, например пиксате. Однако Xamarin. Forms 2,2 представил [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) свойство, заменяя [`StyleId`](xref:Xamarin.Forms.Element.StyleId) свойство.

## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Стиль](xref:Xamarin.Forms.Style)
- [Метод задания](xref:Xamarin.Forms.Setter)
