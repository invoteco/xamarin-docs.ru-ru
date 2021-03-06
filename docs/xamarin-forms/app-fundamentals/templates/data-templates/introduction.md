---
title: Общие сведения о шаблонах данных Xamarin.Forms
description: Шаблоны данных Xamarin.Forms дают возможность настраивать представление данных в поддерживаемых элементах управления. В этой статье описываются шаблоны данных и объясняется их необходимость.
ms.prod: xamarin
ms.assetid: 4ED4ACF4-BE4A-44ED-8EAF-C03947B8663B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 10bba38de1dc8908ad853d5e4ca2bb845b4ac8c6
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "70771279"
---
# <a name="introduction-to-xamarinforms-data-templates"></a>Общие сведения о шаблонах данных Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplates)

_Шаблоны данных Xamarin.Forms дают возможность настраивать представление данных в поддерживаемых элементах управления. В этой статье описываются шаблоны данных и объясняется их необходимость._

Предположим, имеется элемент [`ListView`](xref:Xamarin.Forms.ListView), в котором отображается коллекция объектов `Person`. В следующем примере кода показано определение класса `Person`.

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Location { get; set; }
}
```

В классе `Person` определены свойства `Name`, `Age` и `Location`, которые можно задать при создании объекта `Person`. Элемент [`ListView`](xref:Xamarin.Forms.ListView) используется для отображения коллекции объектов `Person`, как показано в следующем примере кода XAML.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataTemplates"
             ...>
    <StackLayout Margin="20">
        ...
        <ListView Margin="0,20,0,0">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:Person}">
                    <local:Person Name="Steve" Age="21" Location="USA" />
                    <local:Person Name="John" Age="37" Location="USA" />
                    <local:Person Name="Tom" Age="42" Location="UK" />
                    <local:Person Name="Lucas" Age="29" Location="Germany" />
                    <local:Person Name="Tariq" Age="39" Location="UK" />
                    <local:Person Name="Jane" Age="30" Location="USA" />
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </StackLayout>
</ContentPage>
```

Элементы добавляются в [`ListView`](xref:Xamarin.Forms.ListView) в XAML путем инициализации свойства [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) на основе массива экземпляров `Person`.

> [!NOTE]
> Обратите внимание на то, что для элемента `x:Array` требуется атрибут `Type`, указывающий тип элементов в массиве.

Эквивалентная страница на C# показана в следующем примере кода, в котором свойство [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) инициализируется с помощью списка `List` экземпляров `Person`:

```csharp
public WithoutDataTemplatePageCS()
{
    ...
    var people = new List<Person>
    {
        new Person { Name = "Steve", Age = 21, Location = "USA" },
        new Person { Name = "John", Age = 37, Location = "USA" },
        new Person { Name = "Tom", Age = 42, Location = "UK" },
        new Person { Name = "Lucas", Age = 29, Location = "Germany" },
        new Person { Name = "Tariq", Age = 39, Location = "UK" },
        new Person { Name = "Jane", Age = 30, Location = "USA" }
    };

    Content = new StackLayout
    {
        Margin = new Thickness(20),
        Children = {
            ...
            new ListView { ItemsSource = people, Margin = new Thickness(0, 20, 0, 0) }
        }
    };
}
```

При отображении объектов в коллекции [`ListView`](xref:Xamarin.Forms.ListView) вызывает `ToString`. Так как переопределения `Person.ToString` нет, `ToString` возвращает имя типа каждого объекта, как показано на следующих снимках экрана.

![](introduction-images/no-data-template.png "ListView without a Data Template")

Объект `Person` может переопределять метод `ToString` для отображения осмысленных данных, как показано в следующем примере кода.

```csharp
public class Person
{
  ...
  public override string ToString ()
  {
    return Name;
  }
}
```

В результате в [`ListView`](xref:Xamarin.Forms.ListView) отображается значение свойства `Person.Name` для каждого объекта в коллекции, как показано на следующих снимках экрана.

![](introduction-images/override-tostring.png "ListView with a Data Template")

Переопределение `Person.ToString` могло бы возвращать форматированную строку, состоящую из значений свойств `Name`, `Age` и `Location`. Однако такой подход не позволяет полностью контролировать внешний вид каждого элемента данных. Чтобы добиться большей гибкости, можно создать шаблон [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), определяющий внешний вид данных.

## <a name="creating-a-datatemplate"></a>Создание DataTemplate

Шаблон [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) используется для определения внешнего вида данных и обычно привязывается к данным для отображения. Стандартный сценарий его использования — отображение данных из коллекции объектов в [`ListView`](xref:Xamarin.Forms.ListView). Например, если элемент `ListView` привязан к коллекции объектов `Person`, свойству `ListView.ItemTemplate` присваивается шаблон `DataTemplate`, определяющий внешний вид каждого объекта `Person` в `ListView`. Шаблон `DataTemplate` будет содержать элементы, привязанные к значениям свойств каждого объекта `Person`. Дополнительные сведения о привязке данных см. в статье [Основы привязки данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

Шаблон [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) можно использовать в качестве значения следующих свойств:

- [`ListView.HeaderTemplate`](xref:Xamarin.Forms.ListView.HeaderTemplate)
- [`ListView.FooterTemplate`](xref:Xamarin.Forms.ListView.FooterTemplate)
- [`ListView.GroupHeaderTemplate`](xref:Xamarin.Forms.ListView.GroupHeaderTemplate)
- [`ItemsView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1), наследуемое элементом [`ListView`](xref:Xamarin.Forms.ListView);
- [`MultiPage.ItemTemplate`](xref:Xamarin.Forms.MultiPage`1), наследуемое элементами [`CarouselPage`](xref:Xamarin.Forms.CarouselPage), [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) и [`TabbedPage`](xref:Xamarin.Forms.TabbedPage).

> [!NOTE]
> Обратите внимание на то, что, хотя представление [`TableView`](xref:Xamarin.Forms.TableView) использует объекты [`Cell`](xref:Xamarin.Forms.Cell), оно не использует шаблон [`DataTemplate`](xref:Xamarin.Forms.DataTemplate). Связано это с тем, что привязки данных всегда задаются непосредственно для объектов `Cell`.

Шаблон [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), являющийся прямым потомком перечисленных выше свойств, называется *встроенным*. Кроме того, шаблон `DataTemplate` можно определить как ресурс на уровне элемента управления, страницы или приложения. От того, где определен шаблон [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), зависит то, где его можно использовать.

- Шаблон [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), определенный на уровне элемента управления, можно применять только к элементу управления.
- Шаблон [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), определенный на уровне страницы, можно применять к допустимым элементам управления на странице.
- Шаблон [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), определенный на уровне приложения, можно применять к допустимым элементам управления в приложении.

Шаблоны данных, которые находятся ниже в иерархии представлений, имеют приоритет над определенными выше в иерархии, если они имеют общие атрибуты `x:Key`. Например, шаблон данных на уровне приложения переопределяется шаблоном данных на уровне страницы, а шаблон данных на уровне страницы переопределяется шаблоном данных на уровне элемента управления или встроенным шаблоном данных.

## <a name="related-links"></a>Связанные ссылки

- [Вид ячейки](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Шаблоны данных (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplates)
- [DataTemplate](xref:Xamarin.Forms.DataTemplate)
