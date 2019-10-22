---
title: Связываемые макеты в Xamarin. Forms
description: Макеты с возможностью привязки позволяют классам макета создавать свое содержимое путем привязки к коллекции элементов с параметром для установки внешнего вида каждого элемента с помощью DataTemplate.
ms.prod: xamarin
ms.assetid: 824C3319-20A0-42D0-8632-CDECD98349C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/18/2018
ms.openlocfilehash: a824c892d21df9264b772bed09a4aef893f3b949
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "68647911"
---
# <a name="bindable-layouts-in-xamarinforms"></a>Связываемые макеты в Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)

Макеты с возможностью привязки позволяют любому классу макета, производному от класса [`Layout<T>`](xref:Xamarin.Forms.Layout`1) , создавать его содержимое путем привязки к коллекции элементов с возможностью задать внешний вид каждого элемента с помощью [`DataTemplate`](xref:Xamarin.Forms.DataTemplate). Связываемые макеты предоставляются классом `BindableLayout`, который предоставляет следующие вложенные свойства:

- `ItemsSource` — указывает коллекцию элементов `IEnumerable`, отображаемых макетом.
- `ItemTemplate` — указывает [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , применяемый к каждому элементу в коллекции элементов, отображаемых макетом.
- `ItemTemplateSelector` — указывает [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) , который будет использоваться для выбора [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) для элемента во время выполнения.

Эти свойства могут быть присоединены к классам [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout), [`FlexLayout`](xref:Xamarin.Forms.FlexLayout), [`Grid`](xref:Xamarin.Forms.Grid), [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)и [`StackLayout`](xref:Xamarin.Forms.StackLayout) , которые являются производными от класса [1](xref:Xamarin.Forms.Layout`1) .

> [!NOTE]
> Свойство `ItemTemplate` имеет приоритет, если заданы свойства `ItemTemplate` и `ItemTemplateSelector`.

Класс `Layout<T>` предоставляет коллекцию [`Children`](xref:Xamarin.Forms.Layout`1.Children) , в которую добавляются дочерние элементы макета. Если свойство `BinableLayout.ItemsSource` задано как коллекция элементов и присоединено к производному от [`Layout<T>`](xref:Xamarin.Forms.Layout`1)классу, каждый элемент в коллекции добавляется в коллекцию `Layout<T>.Children` для просмотра макетом. После этого класс, производный от `Layout<T>`, обновит свои дочерние представления при изменении базовой коллекции. Дополнительные сведения о цикле макета Xamarin. Forms см. [в разделе Создание пользовательского макета](~/xamarin-forms/user-interface/layouts/custom.md).

Макеты с возможностью привязки следует использовать только в том случае, если коллекция отображаемых элементов невелика, а прокрутка и выбор не требуются. Хотя возможность прокрутки может быть предоставлена путем обертывания связываемого макета в [`ScrollView`](xref:Xamarin.Forms.ScrollView), это не рекомендуется, так как в структурах, поддерживающих связывание, отсутствует ВИРТУАЛИЗАЦИЯ пользовательского интерфейса. Если требуется прокрутка, следует использовать прокручиваемое представление, которое включает в себя виртуализацию пользовательского интерфейса, например [`ListView`](xref:Xamarin.Forms.ListView) или [`CollectionView`](xref:Xamarin.Forms.CollectionView). Несоблюдение этой рекомендации может привести к проблемам с производительностью.

> [!IMPORTANT]
>Хотя технически можно присоединить связываемый макет к любому классу макета, производному от класса [`Layout<T>`](xref:Xamarin.Forms.Layout`1) , это не всегда целесообразно, особенно для классов [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout), [`Grid`](xref:Xamarin.Forms.Grid)и [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) . Например, рассмотрим ситуацию, когда нужно отобразить коллекцию данных в [`Grid`](xref:Xamarin.Forms.Grid) с помощью макета с возможностью привязки, где каждый элемент в коллекции является объектом, содержащим несколько свойств. Каждая строка в `Grid` должна отображать объект из коллекции, каждый столбец в `Grid` отображает одно из свойств объекта. Поскольку [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) для связываемого макета может содержать только один объект, необходимо, чтобы этот объект был классом макета, содержащим несколько представлений, каждый из которых отображает одно из свойств объекта в определенном `Grid` столбце. Несмотря на то, что этот сценарий может быть реализован с помощью связываемых макетов, он приводит к получению родительской `Grid`, содержащей дочерний `Grid` для каждого элемента в привязанной коллекции, что является очень неэффективным и проблематичным использованием макета `Grid`.

## <a name="populating-a-bindable-layout-with-data"></a>Заполнение связываемого макета данными

Связываемый макет заполняется данными путем присвоения свойству `ItemsSource` любой коллекции, реализующей `IEnumerable`, и присоединения ее к производному от [`Layout<T>`](xref:Xamarin.Forms.Layout`1)классу:

```xaml
<Grid BindableLayout.ItemsSource="{Binding Items}" />
```

Эквивалентный код на C# выглядит так:

```csharp
IEnumerable<string> items = ...;
var grid = new Grid();
BindableLayout.SetItemsSource(grid, items);
```

Если присоединенное свойство `BindableLayout.ItemsSource` задано для макета, но присоединенное `BindableLayout.ItemTemplate` свойство не задано, каждый элемент в коллекции `IEnumerable` будет отображаться [`Label`](xref:Xamarin.Forms.Label) , созданного классом `BindableLayout`.

## <a name="defining-item-appearance"></a>Определение внешнего вида элемента

Внешний вид каждого элемента в макете с возможностью привязки можно определить, задав для присоединенного свойства `BindableLayout.ItemTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding User.TopFollowers}"
             Orientation="Horizontal"
             ...>
    <BindableLayout.ItemTemplate>
        <DataTemplate>
            <controls:CircleImage Source="{Binding}"
                                  Aspect="AspectFill"
                                  WidthRequest="44"
                                  HeightRequest="44"
                                  ... />
        </DataTemplate>
    </BindableLayout.ItemTemplate>
</StackLayout>
```

Эквивалентный код на C# выглядит так:

```csharp
DataTemplate circleImageTemplate = ...;
var stackLayout = new StackLayout();
BindableLayout.SetItemsSource(stackLayout, viewModel.User.TopFollowers);
BindableLayout.SetItemTemplate(stackLayout, circleImageTemplate);
```

В этом примере каждый элемент в коллекции `TopFollowers` будет отображен `CircleImage` представлением, определенным в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

![Связываемый макет с DataTemplate](bindable-layouts-images/top-followers.png "Связываемый макет с шаблоном данных")

Дополнительные сведения о шаблонах данных см. в разделе [Шаблоны данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="choosing-item-appearance-at-runtime"></a>Выбор внешнего вида элемента во время выполнения

Внешний вид каждого элемента в макете с возможностью привязки можно выбрать во время выполнения на основе значения элемента, установив для присоединенного свойства `BindableLayout.ItemTemplateSelector` [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector):

```xaml
<FlexLayout BindableLayout.ItemsSource="{Binding User.FavoriteTech}"
            BindableLayout.ItemTemplateSelector="{StaticResource TechItemTemplateSelector}"
            ... />
```

Эквивалентный код на C# выглядит так:

```csharp
DataTemplateSelector dataTemplateSelector = new TechItemTemplateSelector { ... };
var flexLayout = new FlexLayout();
BindableLayout.SetItemsSource(flexLayout, viewModel.User.FavoriteTech);
BindableLayout.SetItemTemplateSelector(flexLayout, dataTemplateSelector);
```

[@No__t_1](xref:Xamarin.Forms.DataTemplateSelector) , используемые в примере приложения, показаны в следующем примере:

```csharp
public class TechItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate XamarinFormsTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return (string)item == "Xamarin.Forms" ? XamarinFormsTemplate : DefaultTemplate;
    }
}
```

Класс `TechItemTemplateSelector` определяет `DefaultTemplate` и `XamarinFormsTemplate` свойства [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , для которых заданы разные шаблоны данных. Метод `OnSelectTemplate` возвращает `XamarinFormsTemplate`, который отображает элемент темно-красного цвета с сердце рядом с ним, когда элемент равен «Xamarin. Forms». Если элемент не равен "Xamarin. Forms", метод `OnSelectTemplate` возвращает `DefaultTemplate`, который отображает элемент, используя цвет по умолчанию [`Label`](xref:Xamarin.Forms.Label):

![Связываемый макет с DataTemplateSelector](bindable-layouts-images/favorite-tech.png "Связываемый макет с помощью селектора шаблона данных")

Дополнительные сведения о селекторах шаблонов данных см. [в разделе Создание DataTemplateSelector Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Связанные ссылки

- [Демонстрация компоновки с возможностью привязки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [Создание пользовательского макета](~/xamarin-forms/user-interface/layouts/custom.md)
- [Шаблоны данных Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Создание DataTemplateSelector Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
