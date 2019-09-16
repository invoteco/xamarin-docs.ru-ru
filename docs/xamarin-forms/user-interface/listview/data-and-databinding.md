---
title: Источники данных ListView
description: В этой статье объясняется, как для заполнения ListView Xamarin.Forms с данными и как использовать привязку данных с помощью ListView.
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/30/2018
ms.openlocfilehash: aa968562470a1e3405bf68be7eb0294273970386
ms.sourcegitcommit: a5ef4497db04dfa016865bc7454b3de6ff088554
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/15/2019
ms.locfileid: "70998029"
---
# <a name="listview-data-sources"></a>Источники данных ListView

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)

Xamarin. Forms [`ListView`](xref:Xamarin.Forms.ListView) используется для отображения списков данных. В этой статье объясняется, `ListView` как заполнить с помощью данных и как привязать данные к выбранному элементу.

## <a name="itemssource"></a>ItemsSource

Объект [ `ListView` ](xref:Xamarin.Forms.ListView) заполняется данными с помощью [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) свойство, которое может принимать любое коллекция, реализующая `IEnumerable`. Самый простой способ заполнения `ListView` предполагает использование массива строк:

```xaml
<ListView>
      <ListView.ItemsSource>
          <x:Array Type="{x:Type x:String}">
            <x:String>mono</x:String>
            <x:String>monodroid</x:String>
            <x:String>monotouch</x:String>
            <x:String>monorail</x:String>
            <x:String>monodevelop</x:String>
            <x:String>monotone</x:String>
            <x:String>monopoly</x:String>
            <x:String>monomodal</x:String>
            <x:String>mononucleosis</x:String>
          </x:Array>
      </ListView.ItemsSource>
</ListView>
```

Ниже приведен аналогичный код C#:

```csharp
var listView = new ListView();
listView.ItemsSource = new string[]
{
  "mono",
  "monodroid",
  "monotouch",
  "monorail",
  "monodevelop",
  "monotone",
  "monopoly",
  "monomodal",
  "mononucleosis"
};
```

![](data-and-databinding-images/itemssource-simple.png "ListView отображения списка строк")

Этот подход будет заполняться `ListView` списком строк. По умолчанию `ListView` вызовет `ToString` и отображения результата в `TextCell` для каждой строки. Чтобы настроить способ отображения данных, см. в разделе [внешний вид ячейки](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

Так как `ItemsSource` отправлен в массив, не будет обновляться при изменении базового списка или массива. Если требуется ListView на автоматическое обновление при элементы добавления, удаления и изменения в базовом списке, необходимо использовать `ObservableCollection`. [`ObservableCollection`](xref:System.Collections.ObjectModel.ObservableCollection`1) определяется в `System.Collections.ObjectModel` и так же, как `List`, за исключением того, что их можно уведомить `ListView` о любых изменениях:

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
listView.ItemsSource = employees;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employees.Add(new Employee(){ DisplayName="Mr. Mono"});
```

## <a name="data-binding"></a>Привязка данных

Привязка данных — это «привязывание», связывающая свойства объекта пользовательского интерфейса со свойствами некоторого объекта CLR, например класса в ViewModel. Привязка данных полезно в тех случаях, поскольку он упрощает разработку пользовательских интерфейсов, заменив массу скучно стандартный код.

Привязка данных работает путем синхронизации объектов при изменении их связанных значений. Вместо того, чтобы создавать обработчики событий для каждого изменения значения элемента управления, необходимо установить привязку и включить привязку в ViewModel.

Дополнительные сведения о привязке данных см. в разделе [основы привязки данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) которого является четвертой частью [серия статей основы XAML Xamarin.Forms](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="binding-cells"></a>Привязка ячеек

Свойства ячеек (и дочерних ячеек) могут быть привязаны к свойствам объектов в `ItemsSource`. Например, `ListView` можно использовать для представления списка сотрудников.

Класса "Сотрудник":

```csharp
public class Employee
{
    public string DisplayName {get; set;}
}
```

Создается, задается `ListView` `ItemsSource`как, а список заполняется данными: `ObservableCollection<Employee>`

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
public ObservableCollection<Employee> Employees { get { return employees; }}

public EmployeeListPage()
{
    EmployeeView.ItemsSource = employees;

    // ObservableCollection allows items to be added after ItemsSource
    // is set and the UI will react to changes
    employees.Add(new Employee{ DisplayName="Rob Finnerty"});
    employees.Add(new Employee{ DisplayName="Bill Wrestler"});
    employees.Add(new Employee{ DisplayName="Dr. Geri-Beth Hooper"});
    employees.Add(new Employee{ DisplayName="Dr. Keith Joyce-Purdy"});
    employees.Add(new Employee{ DisplayName="Sheri Spruce"});
    employees.Add(new Employee{ DisplayName="Burt Indybrick"});
}
```

> [!WARNING]
> `ObservableCollection`не является потокобезопасным. `ObservableCollection` Изменение приводит к тому, что обновления пользовательского интерфейса происходят в том же потоке, в котором были выполнены изменения. Если поток не является основным потоком пользовательского интерфейса, это вызовет исключение.

В следующем фрагменте показано `ListView` привязан к список сотрудников:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:constants="clr-namespace:XamarinFormsSample;assembly=XamarinFormsXamlSample"
             x:Class="XamarinFormsXamlSample.Views.EmployeeListPage"
             Title="Employee List">
  <ListView x:Name="EmployeeView"
            ItemsSource="{Binding Employees}">
    <ListView.ItemTemplate>
      <DataTemplate>
        <TextCell Text="{Binding DisplayName}" />
      </DataTemplate>
    </ListView.ItemTemplate>
  </ListView>
</ContentPage>
```

В этом примере XAML определяется `ContentPage` объект, `ListView`содержащий. Источник данных `ListView` задается с помощью `ItemsSource` атрибута. Макет каждой строки в `ItemsSource` задается `ListView.ItemTemplate` в элементе. Это приводит к следующим снимкам экрана:

![](data-and-databinding-images/bound-data.png "С помощью привязки данных ListView")

### <a name="binding-selecteditem"></a>SelectedItem привязки

Часто необходимо привязать к выбранному элементу списка `ListView`, вместо того чтобы использовать обработчик событий для реагирования на изменения. Чтобы сделать это в XAML, создайте привязку `SelectedItem` свойство:

```xaml
<ListView x:Name="listView"
          SelectedItem="{Binding Source={x:Reference SomeLabel},
          Path=Text}">
 …
</ListView>
```

`listView` `SomeLabel` Если является списком `SelectedItem`строк, свойствобудетпривязанок.`Text` `ItemsSource`

## <a name="related-links"></a>Связанные ссылки

- [Двусторонней привязки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)
