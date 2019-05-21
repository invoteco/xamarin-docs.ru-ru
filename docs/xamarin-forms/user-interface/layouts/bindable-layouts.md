---
title: Привязываемые макетов в Xamarin.Forms
description: Привязываемые макеты, макет классы для создания их содержимого путем привязки к коллекции элементов, с параметром, чтобы задать внешний вид каждого элемента с помощью DataTemplate.
ms.prod: xamarin
ms.assetid: 824C3319-20A0-42D0-8632-CDECD98349C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/18/2018
ms.openlocfilehash: 28846e6e9590d2adf56114fce8bc6056c0112ac1
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/21/2019
ms.locfileid: "65970965"
---
# <a name="bindable-layouts-in-xamarinforms"></a>Привязываемые макетов в Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindableLayouts/)

Привязываемые макеты, любой макет класса, производного от [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) класса, необходимо создать его содержимое, путем привязки к коллекции элементов, с параметром, чтобы задать внешний вид каждого элемента с [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Предоставляются возможности связывания макеты `BindableLayout` класс, предоставляющий следующие вложенные свойства:

- `ItemsSource` — Указывает коллекцию `IEnumerable` отображаемые макета.
- `ItemTemplate` — Указывает [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) для применения к каждому элементу в коллекции элементов, отображаемых на макет.
- `ItemTemplateSelector` — Указывает [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) , будет использоваться для выбора [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) для элемента во время выполнения.

Эти свойства можно подключить к [ `AbsoluteLayout` ](xref:Xamarin.Forms.AbsoluteLayout), [ `FlexLayout` ](xref:Xamarin.Forms.FlexLayout), [ `Grid` ](xref:Xamarin.Forms.Grid), [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout) , и [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) классы, которые являются производными от [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) класса.

> [!NOTE]
> `ItemTemplate` Свойство имеет более высокий приоритет при как `ItemTemplate` и `ItemTemplateSelector` свойств.

`Layout<T>` Предоставляет [ `Children` ](xref:Xamarin.Forms.Layout`1.Children) коллекции, к которому добавляются дочерние элементы макета. При `BinableLayout.ItemsSource` свойство коллекции элементов и служба подключен к [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1)-производном классе, каждый элемент в коллекции добавляется `Layout<T>.Children` коллекции для отображения макета. `Layout<T>`-Производном классе затем обновит его дочерние представления, при изменении коллекции. Дополнительные сведения о цикле макета Xamarin.Forms см. в разделе [Создание пользовательского макета](~/xamarin-forms/user-interface/layouts/custom.md).

Привязываемые макеты следует использовать только в том случае, когда коллекция элементов для отображения небольших и прокрутку и выбор не требуется. Во время прокрутки может быть предоставлено упаковки привязываемых макета в [ `ScrollView` ](xref:Xamarin.Forms.ScrollView), это не рекомендуется, поскольку привязываемых макетов не хватает виртуализацию пользовательского интерфейса. Прокрутка требуется, если прокручиваемые представление, которое включает виртуализацию пользовательского интерфейса, такие как [ `ListView` ](xref:Xamarin.Forms.ListView) или [ `CollectionView` ](xref:Xamarin.Forms.CollectionView), следует использовать. Чтобы наблюдать за Эта рекомендация может привести к проблемам с производительностью.

> [!IMPORTANT]
>Хотя технически возможно, для присоединения макете привязываемые к любому классу макет, который является производным от [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) класс, не всегда удобно сделать, особенно для [ `AbsoluteLayout` ](xref:Xamarin.Forms.AbsoluteLayout) , [ `Grid` ](xref:Xamarin.Forms.Grid), и [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout) классы. Например, рассмотрим ситуацию, в которой хотите отображать коллекцию данных в [ `Grid` ](xref:Xamarin.Forms.Grid) с помощью связывания макета, где каждый элемент в коллекции является объект содержащий несколько свойств. Каждая строка в `Grid` должен отображать объект из коллекции, с соответствующими столбцами `Grid` отображения одного из свойств объекта. Так как [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) для связывания макета может содержать только один объект, она необходима для этого объекта является классом макет, содержащий несколько представлений, что каждый отображения одного из свойств объекта в определенном `Grid` столбца. Хотя этот сценарий можно осознать с макетами привязки, это приводит к родительской `Grid` содержащий дочерний элемент `Grid` для каждого элемента в привязанной коллекции, который является очень неэффективным и проблемных использования `Grid` макета.

## <a name="populating-a-bindable-layout-with-data"></a>Заполнение макете связывания с данными

Привязываемые макета заполняется данными, задав его `ItemsSource` свойство к любой коллекции, реализующей `IEnumerable`и подключите его к [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1)-производный класс:

```xaml
<Grid BindableLayout.ItemsSource="{Binding Items}" />
```

Эквивалентный код на C# выглядит так:

```csharp
IEnumerable<string> items = ...;
var grid = new Grid();
BindableLayout.SetItemsSource(grid, items);
```

При `BindableLayout.ItemsSource` вложенное свойство задано в макете, но `BindableLayout.ItemTemplate` присоединенное свойство не задано, каждый элемент `IEnumerable` коллекции будет отображаться по [ `Label` ](xref:Xamarin.Forms.Label) , созданный путем `BindableLayout` класса.

## <a name="defining-item-appearance"></a>Определение внешнего вида элемента

Внешний вид каждого элемента в структуре привязки может быть определена путем задания `BindableLayout.ItemTemplate` вложенное свойство, чтобы [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate):

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

В этом примере каждый элемент в `TopFollowers` коллекции будет отображаться по `CircleImage` представление, определенное в [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate):

![Макет, связываемые с DataTemplate](bindable-layouts-images/top-followers.png "привязываемых макета с помощью шаблона данных")

Дополнительные сведения о шаблонах данных см. в разделе [Шаблоны данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="choosing-item-appearance-at-runtime"></a>Выбор внешнего вида элемента во время выполнения

Внешний вид каждого элемента в структуре привязки можно выбрать во время выполнения, в зависимости от значения элемента, задав `BindableLayout.ItemTemplateSelector` вложенное свойство, чтобы [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector):

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

[ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) Используется в образце приложения показан в следующем примере:

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

`TechItemTemplateSelector` Класс определяет `DefaultTemplate` и `XamarinFormsTemplate` [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) свойства, заданные для различные шаблоны данных. `OnSelectTemplate` Возвращает метод `XamarinFormsTemplate`, который отображает элемент в Темно-красный с сердце рядом с ним, если элемент является равным «Xamarin.Forms». Если элемент не соответствует «Xamarin.Forms», `OnSelectTemplate` метод возвращает `DefaultTemplate`, которая отображает элемент с использованием цвет по умолчанию для [ `Label` ](xref:Xamarin.Forms.Label):

![Макет, связываемые с DataTemplateSelector](bindable-layouts-images/favorite-tech.png "привязываемых макета с помощью селектора шаблона данных")

Дополнительные сведения о селекторах шаблон данных, см. в разделе [Создание Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Связанные ссылки

- [Демонстрация привязки макета (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindableLayouts/)
- [Создание пользовательского макета](~/xamarin-forms/user-interface/layouts/custom.md)
- [Шаблоны Xamarin.Forms данных](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Создание Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
