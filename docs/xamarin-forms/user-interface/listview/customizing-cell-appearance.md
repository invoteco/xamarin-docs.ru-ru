---
title: Настройка внешнего вида ячейки ListView
description: В этой статье рассматриваются параметры для представления данных в приложениях Xamarin.Forms, а использование преимуществ удобства элемента управления ListView.
ms.prod: xamarin
ms.assetid: FD45CB91-1A8F-46FB-B432-6BC20492E456
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/12/2019
ms.openlocfilehash: ab54b54c9f2f7d6d7748137ea079439b7c3ddfca
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/07/2020
ms.locfileid: "78916979"
---
# <a name="customizing-listview-cell-appearance"></a>Настройка внешнего вида ячейки ListView

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)

Класс [`ListView`](xref:Xamarin.Forms.ListView) Xamarin. Forms используется для представления прокручиваемых списков, которые можно настроить с помощью элементов `ViewCell`. Элемент `ViewCell` может отображать текст и изображения, указывать состояние true/false и принимать входные данные пользователя.

## <a name="built-in-cells"></a>Встроенные в ячейках
Xamarin. Forms поставляется со встроенными ячейками, которые работают во многих приложениях:

- [`TextCell`](#textcell) элементы управления используются для отображения текста с необязательной второй линией для текста сведений.
- [`ImageCell`](#imagecell) элементы управления похожи на `TextCell`s, но содержат изображение слева от текста.
- элементы управления `SwitchCell` используются для представления и записи состояния включения и выключения или true/false.
- элементы управления `EntryCell` используются для представления текстовых данных, которые пользователь может изменять.

Элементы управления [`SwitchCell`](~/xamarin-forms/user-interface/tableview.md#switchcell) и [`EntryCell`](~/xamarin-forms/user-interface/tableview.md#entrycell) чаще всего используются в контексте [`TableView`](~/xamarin-forms/user-interface/tableview.md).

### <a name="textcell"></a>TextCell

[`TextCell`](xref:Xamarin.Forms.TextCell) — это ячейка для отображения текста, при необходимости со второй строкой в качестве подробного текста. На следующем снимке экрана показаны `TextCell` элементы в iOS и Android:

![](customizing-cell-appearance-images/text-cell-default.png "Default TextCell Example")

Текстцеллс отображаются как собственные элементы управления во время выполнения, поэтому производительность очень хороша по сравнению с настраиваемым `ViewCell`. Текстцеллс можно настраивать, позволяя задавать следующие свойства:

- `Text` &ndash; текст, отображаемый в первой строке крупным шрифтом.
- `Detail` &ndash; текст, отображаемый под первой строкой, с меньшим шрифтом.
- `TextColor` &ndash; цвет текста.
- `DetailColor` &ndash; цвет текста сведений

На следующем снимке экрана показаны `TextCell` элементы с настроенными свойствами цвета:

![](customizing-cell-appearance-images/text-cell-custom.png "Custom TextCell Example")

### <a name="imagecell"></a>ImageCell

[`ImageCell`](xref:Xamarin.Forms.ImageCell), например `TextCell`, можно использовать для отображения текста и дополнительного текста подробностей, а также обеспечивает высокую производительность, используя собственные элементы управления платформы. `ImageCell` отличается от `TextCell` тем, что отображает изображение слева от текста.

На следующем снимке экрана показаны `ImageCell` элементы в iOS и Android: !["Default Имажецелл example" (пример по умолчанию](customizing-cell-appearance-images/image-cell-default.png "Пример Имажецелл по умолчанию") ).

`ImageCell` удобно использовать, если необходимо отобразить список данных с визуальным аспектом, например список контактов или фильмов. `ImageCell`ы можно настраивать, позволяя задавать следующие значения:

- `Text` &ndash; текст, отображаемый в первой строке, крупным шрифтом
- `Detail` &ndash; текст, отображаемый под первой строкой, с меньшим шрифтом
- `TextColor` &ndash; цвет текста
- `DetailColor` &ndash; цвет текста сведений
- `ImageSource` &ndash; изображение, отображаемое рядом с текстом

На следующем снимке экрана показаны `ImageCell` элементы с настроенными свойствами цвета: !["настроенный пример имажецелл"](customizing-cell-appearance-images/image-cell-custom.png "Пример настраиваемого Имажецелл") .

## <a name="custom-cells"></a>Пользовательские ячейки
Пользовательские ячейки позволяют создавать макеты ячеек, которые не поддерживаются встроенными ячейками. Например можно представить ячейки с двух меток, которые имеют равный вес. `TextCell` быть недостаточно, так как `TextCell` имеет одну метку, которая меньше. Большая часть настройки ячейку добавьте дополнительные данные только для чтения (например, дополнительные метки, изображения или другие сведения для отображения).

Все пользовательские ячейки должны быть производными от [`ViewCell`](xref:Xamarin.Forms.ViewCell)— того же базового класса, который используется всеми встроенными типами ячеек.

Xamarin. Forms предлагает [поведение кэширования](~/xamarin-forms/user-interface/listview/performance.md#caching-strategy) для элемента управления `ListView`, что может повысить производительность прокрутки для некоторых типов пользовательских ячеек.

На следующем снимке экрана показан пример пользовательской ячейки:

!["Пример настраиваемой ячейки"](customizing-cell-appearance-images/custom-cell.png "Пример пользовательской ячейки")

### <a name="xaml"></a>XAML
Пользовательская ячейка, показанная на предыдущем снимке экрана, может быть создана с помощью следующего кода XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="demoListView.ImageCellPage">
    <ContentPage.Content>
        <ListView  x:Name="listView">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout BackgroundColor="#eee"
                        Orientation="Vertical">
                            <StackLayout Orientation="Horizontal">
                                <Image Source="{Binding image}" />
                                <Label Text="{Binding title}"
                                TextColor="#f35e20" />
                                <Label Text="{Binding subtitle}"
                                HorizontalOptions="EndAndExpand"
                                TextColor="#503026" />
                            </StackLayout>
                        </StackLayout>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

КОД XAML работает следующим образом:

- Пользовательская ячейка вложена в `DataTemplate`, которая находится внутри `ListView.ItemTemplate`. Это тот же процесс, что и при использовании любой встроенной ячейки.
- `ViewCell` — это тип пользовательской ячейки. Дочерний элемент элемента `DataTemplate` должен иметь класс `ViewCell` или быть производным от него.
- Внутри `ViewCell`макет может управляться любым макетом Xamarin. Forms. В этом примере макет управляется `StackLayout`, что позволяет настроить цвет фона.

> [!NOTE]
> Любое свойство `StackLayout`, которое можно привязать, может быть привязано внутри пользовательской ячейки. Однако эта возможность не показана в примере XAML.

### <a name="code"></a>Код

Пользовательскую ячейку также можно создать в коде. Во-первых, необходимо создать пользовательский класс, производный от `ViewCell`:

```csharp
public class CustomCell : ViewCell
    {
        public CustomCell()
        {
            //instantiate each of our views
            var image = new Image ();
            StackLayout cellWrapper = new StackLayout ();
            StackLayout horizontalLayout = new StackLayout ();
            Label left = new Label ();
            Label right = new Label ();

            //set bindings
            left.SetBinding (Label.TextProperty, "title");
            right.SetBinding (Label.TextProperty, "subtitle");
            image.SetBinding (Image.SourceProperty, "image");

            //Set properties for desired design
            cellWrapper.BackgroundColor = Color.FromHex ("#eee");
            horizontalLayout.Orientation = StackOrientation.Horizontal;
            right.HorizontalOptions = LayoutOptions.EndAndExpand;
            left.TextColor = Color.FromHex ("#f35e20");
            right.TextColor = Color.FromHex ("503026");

            //add views to the view hierarchy
            horizontalLayout.Children.Add (image);
            horizontalLayout.Children.Add (left);
            horizontalLayout.Children.Add (right);
            cellWrapper.Children.Add (horizontalLayout);
            View = cellWrapper;
        }
    }
```

В конструкторе страницы свойству `ItemTemplate` ListView присвоено значение `DataTemplate` с указанным типом `CustomCell`:

```csharp
public partial class ImageCellPage : ContentPage
    {
        public ImageCellPage ()
        {
            InitializeComponent ();
            listView.ItemTemplate = new DataTemplate (typeof(CustomCell));
        }
    }
```

### <a name="binding-context-changes"></a>Изменения контекста привязки

При привязке к экземплярам [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) пользовательских типов ячеек элементы управления пользовательского интерфейса, отображающие `BindableProperty` значения, должны использовать переопределение [`OnBindingContextChanged`](xref:Xamarin.Forms.Cell.OnBindingContextChanged) , чтобы задать отображение данных в каждой ячейке, а не в конструкторе ячейки, как показано в следующем примере кода:

```csharp
public class CustomCell : ViewCell
{
    Label nameLabel, ageLabel, locationLabel;

    public static readonly BindableProperty NameProperty =
        BindableProperty.Create ("Name", typeof(string), typeof(CustomCell), "Name");
    public static readonly BindableProperty AgeProperty =
        BindableProperty.Create ("Age", typeof(int), typeof(CustomCell), 0);
    public static readonly BindableProperty LocationProperty =
        BindableProperty.Create ("Location", typeof(string), typeof(CustomCell), "Location");

    public string Name
    {
        get { return(string)GetValue (NameProperty); }
        set { SetValue (NameProperty, value); }
    }

    public int Age
    {
        get { return(int)GetValue (AgeProperty); }
        set { SetValue (AgeProperty, value); }
    }

    public string Location
    {
        get { return(string)GetValue (LocationProperty); }
        set { SetValue (LocationProperty, value); }
    }
    ...

    protected override void OnBindingContextChanged ()
    {
        base.OnBindingContextChanged ();

        if (BindingContext != null)
        {
            nameLabel.Text = Name;
            ageLabel.Text = Age.ToString ();
            locationLabel.Text = Location;
        }
    }
}
```

Переопределение [`OnBindingContextChanged`](xref:Xamarin.Forms.Cell.OnBindingContextChanged) будет вызываться при срабатывании события [`BindingContextChanged`](xref:Xamarin.Forms.BindableObject.BindingContextChanged) в ответ на значение свойства [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) . Поэтому при изменении `BindingContext` элементы управления пользовательского интерфейса, отображающие значения [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) , должны задавать свои данные. Обратите внимание, что `BindingContext` следует проверять на `null` значение, так как оно может быть задано в Xamarin. Forms для сборки мусора, что, в свою очередь, приведет к вызову переопределения `OnBindingContextChanged`.

Кроме того, элементы управления пользовательского интерфейса могут привязываться к экземплярам [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) для вывода их значений, что устраняет необходимость переопределения метода `OnBindingContextChanged`.

> [!NOTE]
> При переопределении `OnBindingContextChanged`убедитесь, что вызывается метод `OnBindingContextChanged` базового класса, чтобы зарегистрированные делегаты получали событие `BindingContextChanged`.

В XAML привязка пользовательского типа ячейки к данным может осуществляться как показано в следующем примере кода:

```xaml
<ListView x:Name="listView">
    <ListView.ItemTemplate>
        <DataTemplate>
            <local:CustomCell Name="{Binding Name}" Age="{Binding Age}" Location="{Binding Location}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Это привязывает `Name`, `Age`и `Location` привязываемые свойства в экземпляре `CustomCell` к свойствам `Name`, `Age`и `Location` каждого объекта в базовой коллекции.

В следующем примере кода показан эквивалентный привязки на языке C#:

```csharp
var customCell = new DataTemplate (typeof(CustomCell));
customCell.SetBinding (CustomCell.NameProperty, "Name");
customCell.SetBinding (CustomCell.AgeProperty, "Age");
customCell.SetBinding (CustomCell.LocationProperty, "Location");

var listView = new ListView
{
    ItemsSource = people,
    ItemTemplate = customCell
};
```

В iOS и Android, если [`ListView`](xref:Xamarin.Forms.ListView) перезапускают элементы, а пользовательская ячейка использует пользовательский модуль подготовки отчетов, пользовательский модуль подготовки отчетов должен правильно реализовать уведомление об изменении свойства. При повторном использовании ячеек значения их свойств изменятся при обновлении контекста привязки до значения доступной ячейки с `PropertyChanged` событиями. Дополнительные сведения см. [в разделе Настройка ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md). Дополнительные сведения о перезапуске ячеек см. в разделе [стратегия кэширования](~/xamarin-forms/user-interface/listview/performance.md#caching-strategy).

## <a name="related-links"></a>Связанные ссылки

- [Встроенные ячейки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-builtincells)
- [Пользовательские ячейки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)
- [Контекст привязки изменен (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-bindingcontextchanged)
