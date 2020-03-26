---
title: Источники данных ListView
description: В этой статье объясняется, как для заполнения ListView Xamarin.Forms с данными и как использовать привязку данных с помощью ListView.
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2020
ms.openlocfilehash: e51f0bd011750b030c0a11b9b89a2c2473f2a9ed
ms.sourcegitcommit: d83c6af42ed26947aa7c0ecfce00b9ef60f33319
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/25/2020
ms.locfileid: "80247591"
---
# <a name="listview-data-sources"></a>Источники данных ListView

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)

[`ListView`](xref:Xamarin.Forms.ListView) Xamarin. Forms используется для отображения списков данных. В этой статье объясняется, как заполнить `ListView` данными и как привязать данные к выбранному элементу.

## <a name="itemssource"></a>ItemsSource

[`ListView`](xref:Xamarin.Forms.ListView) заполняется данными с помощью свойства [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) , которое может принимать любую коллекцию, реализующую `IEnumerable`. Самый простой способ заполнения `ListView` заключается в использовании массива строк:

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

Эквивалентный код на C# выглядит так:

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

![](data-and-databinding-images/itemssource-simple.png "ListView Displaying List of Strings")

Этот подход заполнит `ListView` списком строк. По умолчанию `ListView` будет вызывать `ToString` и отображать результат в `TextCell` для каждой строки. Сведения о настройке отображения данных см. в разделе [внешний вид ячеек](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

Поскольку `ItemsSource` был отправлен в массив, содержимое не будет обновляться по мере изменения базового списка или массива. Если необходимо, чтобы ListView автоматически подменялся при добавлении, удалении и изменении элементов в базовом списке, необходимо использовать `ObservableCollection`. [`ObservableCollection`](xref:System.Collections.ObjectModel.ObservableCollection`1) определяется в `System.Collections.ObjectModel` и выглядит так же, как `List`, за исключением того, что он может уведомлять `ListView` о любых изменениях:

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
listView.ItemsSource = employees;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employees.Add(new Employee(){ DisplayName="Mr. Mono"});
```

## <a name="data-binding"></a>Привязка данных

Привязка данных — это «привязывание», связывающая свойства объекта пользовательского интерфейса со свойствами некоторого объекта CLR, например класса в ViewModel. Привязка данных полезно в тех случаях, поскольку он упрощает разработку пользовательских интерфейсов, заменив массу скучно стандартный код.

Привязка данных работает путем синхронизации объектов при изменении их связанных значений. Вместо того, чтобы создавать обработчики событий для каждого изменения значения элемента управления, необходимо установить привязку и включить привязку в ViewModel.

Дополнительные сведения о привязке данных см. в статье основы [привязки данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) , которая является частью 4 из [серии статей основы работы с XAML Xamarin. Forms](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="binding-cells"></a>Привязка ячеек

Свойства ячеек (и потомков ячеек) можно привязать к свойствам объектов в `ItemsSource`. Например, можно использовать `ListView` для предоставления списка сотрудников.

Класса "Сотрудник":

```csharp
public class Employee
{
    public string DisplayName {get; set;}
}
```

Создается `ObservableCollection<Employee>`, устанавливается как `ListView` `ItemsSource`, а список заполняется данными:

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
> Хотя `ListView` будет обновляться в ответ на изменения в его базовом `ObservableCollection`, `ListView` не будет обновлена, если другой экземпляр `ObservableCollection` назначен исходной ссылке `ObservableCollection` (например, `employees = otherObservableCollection;`).

В следующем фрагменте кода демонстрируется `ListView`, привязанный к списку сотрудников:

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

В этом примере XAML определяется `ContentPage`, содержащий `ListView`. Источник данных `ListView` задается с помощью атрибута `ItemsSource`. Макет каждой строки в `ItemsSource` определяется в элементе `ListView.ItemTemplate`. Это приводит к следующим снимкам экрана:

![](data-and-databinding-images/bound-data.png "ListView using Data Binding")

> [!WARNING]
> `ObservableCollection` не является потокобезопасным. Изменение `ObservableCollection` приводит к тому, что обновления пользовательского интерфейса происходят в том же потоке, который выполнил изменения. Если поток не является основным потоком пользовательского интерфейса, это вызовет исключение.

### <a name="binding-selecteditem"></a>SelectedItem привязки

Часто требуется выполнить привязку к выбранному элементу `ListView`, вместо того чтобы использовать обработчик событий для реагирования на изменения. Чтобы сделать это в XAML, привяжите свойство `SelectedItem`:

```xaml
<ListView x:Name="listView"
          SelectedItem="{Binding Source={x:Reference SomeLabel},
          Path=Text}">
 …
</ListView>
```

Если `ItemsSource` `listView`является списком строк, `SomeLabel` будет иметь свойство `Text`, привязанное к `SelectedItem`.

## <a name="related-links"></a>Связанные ссылки

- [Двусторонняя привязка (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)
