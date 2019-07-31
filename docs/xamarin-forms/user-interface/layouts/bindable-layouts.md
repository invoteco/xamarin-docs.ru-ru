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
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68647911"
---
# <a name="bindable-layouts-in-xamarinforms"></a>Связываемые макеты в Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)

Макеты с возможностью привязки позволяют любому классу макета, производному от [`Layout<T>`](xref:Xamarin.Forms.Layout`1) класса, создавать его содержимое путем привязки к коллекции элементов с возможностью задать внешний вид каждого элемента [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)с помощью. Связываемые макеты предоставляются `BindableLayout` классом, который предоставляет следующие вложенные свойства:

- `ItemsSource`— Указывает коллекцию `IEnumerable` элементов, отображаемых макетом.
- `ItemTemplate`— Задает объект [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , применяемый к каждому элементу в коллекции элементов, отображаемых макетом.
- `ItemTemplateSelector`— Указывает [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) , который будет использоваться для [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) выбора элемента во время выполнения.

Эти свойства могут быть присоединены к [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)классам [`Grid`](xref:Xamarin.Forms.Grid), [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) [`FlexLayout`](xref:Xamarin.Forms.FlexLayout),, [`StackLayout`](xref:Xamarin.Forms.StackLayout) и, которые все являются производными от [`Layout<T>`](xref:Xamarin.Forms.Layout`1) класса.

> [!NOTE]
> Свойство имеет приоритет, если `ItemTemplate` заданы свойства `ItemTemplateSelector`и. `ItemTemplate`

`Layout<T>` [Класс`Children`](xref:Xamarin.Forms.Layout`1.Children) предоставляет коллекцию, в которую добавляются дочерние элементы макета. Если свойство установлено в коллекцию элементов и присоединяется [`Layout<T>`](xref:Xamarin.Forms.Layout`1)к классу, производному от, каждый элемент в `Layout<T>.Children` коллекции добавляется в коллекцию для просмотра макетом. `BinableLayout.ItemsSource` Класс `Layout<T>`, производный от, обновит свои дочерние представления при изменении базовой коллекции. Дополнительные сведения о цикле макета Xamarin. Forms см. [в разделе Создание пользовательского макета](~/xamarin-forms/user-interface/layouts/custom.md).

Макеты с возможностью привязки следует использовать только в том случае, если коллекция отображаемых элементов невелика, а прокрутка и выбор не требуются. Хотя возможность прокрутки может быть предоставлена путем заключения в оболочку [`ScrollView`](xref:Xamarin.Forms.ScrollView)связываемого макета, это не рекомендуется, так как в структурах, поддерживающих связывание, отсутствует виртуализация пользовательского интерфейса. Если требуется прокрутка, следует использовать прокручиваемое представление, которое включает в себя виртуализацию [`CollectionView`](xref:Xamarin.Forms.CollectionView)пользовательского интерфейса, например [`ListView`](xref:Xamarin.Forms.ListView) или. Несоблюдение этой рекомендации может привести к проблемам с производительностью.

> [!IMPORTANT]
>Хотя технически можно присоединить связываемый макет к любому классу [`Layout<T>`](xref:Xamarin.Forms.Layout`1) макета, производному от класса, это не всегда целесообразно, особенно [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)для классов, [`Grid`](xref:Xamarin.Forms.Grid)и [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) . Например, рассмотрим ситуацию, когда нужно отобразить коллекцию данных в [`Grid`](xref:Xamarin.Forms.Grid) с использованием связываемого макета, где каждый элемент в коллекции является объектом, содержащим несколько свойств. Каждая строка в `Grid` должна отображать объект из коллекции, каждый столбец `Grid` в котором отображает одно из свойств объекта. Поскольку для связываемого макета может содержаться только один объект, необходимо, чтобы этот объект был классом макета, содержащим несколько представлений, каждый из которых отображает одно из свойств объекта в определенном `Grid` столбце. [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Хотя этот сценарий может быть реализован с помощью связываемых макетов, он приводит к использованию `Grid` родителя, содержащего `Grid` дочерний элемент для каждого элемента в привязанной коллекции, что является очень неэффективным `Grid` и проблематичным использованием макета.

## <a name="populating-a-bindable-layout-with-data"></a>Заполнение связываемого макета данными

Связываемый макет заполняется данными путем установки его `ItemsSource` свойства в любую коллекцию, реализующую `IEnumerable`, [`Layout<T>`](xref:Xamarin.Forms.Layout`1)и присоединение ее к производному классу:

```xaml
<Grid BindableLayout.ItemsSource="{Binding Items}" />
```

Эквивалентный код на C# выглядит так:

```csharp
IEnumerable<string> items = ...;
var grid = new Grid();
BindableLayout.SetItemsSource(grid, items);
```

`BindableLayout` [`Label`](xref:Xamarin.Forms.Label) `IEnumerable` `BindableLayout.ItemTemplate` Если вложенное свойство задано для макета, но присоединенное свойство не задано, каждый элемент в коллекции будет отображаться объектом, созданным классом. `BindableLayout.ItemsSource`

## <a name="defining-item-appearance"></a>Определение внешнего вида элемента

Внешний вид каждого элемента в макете с возможностью привязки можно определить, установив `BindableLayout.ItemTemplate` для присоединенного свойства значение. [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)

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

В этом примере каждый элемент в `TopFollowers` коллекции будет отображаться `CircleImage` в представлении, определенном в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

![Связываемый макет с DataTemplate](bindable-layouts-images/top-followers.png "Связываемый макет с шаблоном данных")

Дополнительные сведения о шаблонах данных см. в разделе [Шаблоны данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="choosing-item-appearance-at-runtime"></a>Выбор внешнего вида элемента во время выполнения

Внешний вид каждого элемента в макете с возможностью привязки можно выбрать во время выполнения на основе значения элемента, установив `BindableLayout.ItemTemplateSelector` [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)для присоединенного свойства значение:

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

Объект [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) , используемый в примере приложения, показан в следующем примере:

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

Класс определяет `DefaultTemplate` свойства и`XamarinFormsTemplate` , [длякоторыхустановленыразныешаблоныданных.`DataTemplate`](xref:Xamarin.Forms.DataTemplate) `TechItemTemplateSelector` `OnSelectTemplate` Метод`XamarinFormsTemplate`возвращает, который отображает элемент темно-красного цвета с сердце рядом с ним, когда элемент равен "Xamarin. Forms". Если элемент не равен "Xamarin. Forms", `OnSelectTemplate` метод `DefaultTemplate`возвращает объект, который отображает элемент, используя цвет [`Label`](xref:Xamarin.Forms.Label)по умолчанию:

![Связываемый макет с DataTemplateSelector](bindable-layouts-images/favorite-tech.png "Связываемый макет с помощью селектора шаблона данных")

Дополнительные сведения о селекторах шаблонов данных см. [в разделе Создание DataTemplateSelector Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Связанные ссылки

- [Демонстрация компоновки с возможностью привязки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [Создание пользовательского макета](~/xamarin-forms/user-interface/layouts/custom.md)
- [Шаблоны данных Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Создание DataTemplateSelector Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
