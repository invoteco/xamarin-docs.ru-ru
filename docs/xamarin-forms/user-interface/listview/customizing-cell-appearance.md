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
ms.sourcegitcommit: a5ef4497db04dfa016865bc7454b3de6ff088554
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/15/2019
ms.locfileid: "70998127"
---
# <a name="customizing-listview-cell-appearance"></a>Настройка внешнего вида ячейки ListView

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)

Класс Xamarin. Forms [`ListView`](xref:Xamarin.Forms.ListView) используется для представления прокручиваемых списков, которые можно настроить с `ViewCell` помощью элементов. `ViewCell` Элемент может отображать текст и изображения, указывать состояние true/false и принимать входные данные пользователя.

## <a name="built-in-cells"></a>Встроенные в ячейках
Xamarin. Forms поставляется со встроенными ячейками, которые работают во многих приложениях:

- [`TextCell`](#textcell)элементы управления используются для отображения текста с необязательной второй строкой для подробного текста.
- [`ImageCell`](#imagecell)элементы управления похожи на `TextCell`s, но включают изображение слева от текста.
- `SwitchCell`элементы управления используются для представления и записи состояния "вкл./выкл" или "истина/ложь".
- `EntryCell`элементы управления используются для представления текстовых данных, которые пользователь может изменять.

Элементы управления [`EntryCell`](~/xamarin-forms/user-interface/tableview.md#entrycell) [`TableView`](~/xamarin-forms/user-interface/tableview.md)и чаще используются в контексте. [`SwitchCell`](~/xamarin-forms/user-interface/tableview.md#switchcell)

### <a name="textcell"></a>TextCell

[`TextCell`](xref:Xamarin.Forms.TextCell) представляет собой ячейку для отображения текста, при необходимости с вторая строка обозначается как текст сведений. На следующем снимке `TextCell` экрана показаны элементы в iOS и Android:

![](customizing-cell-appearance-images/text-cell-default.png "Пример TextCell по умолчанию")

TextCells подготавливаются к просмотру как собственные элементы управления во время выполнения, поэтому производительность является очень высокой по сравнению с пользовательской `ViewCell`. Текстцеллс можно настраивать, позволяя задавать следующие свойства:

- `Text` &ndash; текст, отображаемый в первой строке крупным шрифтом.
- `Detail` &ndash; текст, который отображается под первой строке, мелким шрифтом.
- `TextColor` &ndash; цвет текста.
- `DetailColor` &ndash; цвет текста детализации

На следующем снимке `TextCell` экрана показаны элементы с настроенными свойствами цвета:

![](customizing-cell-appearance-images/text-cell-custom.png "Пример настраиваемого Текстцелл")

### <a name="imagecell"></a>ImageCell

[`ImageCell`](xref:Xamarin.Forms.ImageCell), такие как `TextCell`, можно использовать для отображения текста и текст дополнительный сведений, и он предлагает превосходную производительность процессов с помощью собственных элементов управления для каждой платформы. `ImageCell` отличается от `TextCell` тем, что показывает изображения слева от текста.

На следующем снимке `ImageCell` экрана показаны элементы в iOS и Android: !["Пример Имажецелл по умолчанию"](customizing-cell-appearance-images/image-cell-default.png "Пример Имажецелл по умолчанию")

`ImageCell` полезно, когда необходимо отобразить список данных с помощью вида, такие как список контактов или фильмах. `ImageCell`можно настраивать, что позволяет задать следующие значения:

- `Text` &ndash; текст, который отображается в первой строке, крупным шрифтом
- `Detail` &ndash; текст, который отображается под первой строке, меньший размер шрифта
- `TextColor` &ndash; цвет текста
- `DetailColor` &ndash; цвет текста детализации
- `ImageSource` &ndash; изображение, отображаемое рядом с текстом

На следующем снимке `ImageCell` экрана показаны элементы с настроенными свойствами цвета: !["Пользовательский пример имажецелл"](customizing-cell-appearance-images/image-cell-custom.png "Пример настраиваемого имажецелл")

## <a name="custom-cells"></a>Пользовательские ячейки
Пользовательские ячейки позволяют создавать макеты ячеек, которые не поддерживаются встроенными ячейками. Например можно представить ячейки с двух меток, которые имеют равный вес. Объект `TextCell` будет недостаточно поскольку `TextCell` содержит одну метку, меньший по размеру. Большая часть настройки ячейку добавьте дополнительные данные только для чтения (например, дополнительные метки, изображения или другие сведения для отображения).

Все пользовательские ячейки должен быть производным от [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), тот же базовый класс, что все ячейки, встроенные типы использования.

Xamarin. Forms `ListView` предлагает [поведение кэширования](~/xamarin-forms/user-interface/listview/performance.md#caching-strategy) для элемента управления, которое может улучшить производительность прокрутки для некоторых типов пользовательских ячеек.

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

- Пользовательской ячейки вложен в `DataTemplate`, который находится внутри `ListView.ItemTemplate`. Это тот же процесс, что и при использовании любой встроенной ячейки.
- `ViewCell` — Тип пользовательской ячейки. Дочерний `DataTemplate` элемент элемента должен иметь `ViewCell` класс или быть производным от класса.
- `ViewCell`Внутри макет может управляться любым макетом Xamarin. Forms. В этом примере макет управляется `StackLayout`, что позволяет настроить цвет фона.

> [!NOTE]
> Любое свойство `StackLayout` , которое является связываемым, может быть привязано внутри пользовательской ячейки. Однако эта возможность не показана в примере XAML.

### <a name="code"></a>Код

Пользовательскую ячейку также можно создать в коде. Во-первых, должен быть создан пользовательский класс `ViewCell` , производный от класса.

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

В конструкторе страницы `ItemTemplate` свойству `DataTemplate` ListView присваивается значение `CustomCell` типа с указанным типом:

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

При привязке к ячейки пользовательского типа [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) экземпляров, элементы управления пользовательского интерфейса, отображающие `BindableProperty` значения следует использовать [ `OnBindingContextChanged` ](xref:Xamarin.Forms.Cell.OnBindingContextChanged) переопределение, чтобы задать данные для отображения в Каждая ячейка, а не конструктор ячейки, как показано в следующем примере кода:

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

[ `OnBindingContextChanged` ](xref:Xamarin.Forms.Cell.OnBindingContextChanged) Переопределение будет выполнен при вызове [ `BindingContextChanged` ](xref:Xamarin.Forms.BindableObject.BindingContextChanged) вызывает событие, в ответ на значение [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) изменение свойства. Таким образом, когда `BindingContext` изменяется, элементы управления пользовательского интерфейса, отображающие [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) значения следует задать свои данные. Обратите внимание, что `BindingContext` должны проверяться `null` значение, как это может быть переведена с Xamarin.Forms для сборки мусора, что в свою очередь приведет к `OnBindingContextChanged` переопределить вызова.

Кроме того, можно привязать элементы управления пользовательского интерфейса к [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) экземпляров, чтобы отобразить их значения, которые устраняет необходимость в Переопределите `OnBindingContextChanged` метод.

> [!NOTE]
> При переопределении метода `OnBindingContextChanged`, убедитесь, что базовый класс `OnBindingContextChanged` метод вызывается, чтобы зарегистрированные делегаты получили `BindingContextChanged` событий.

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

Этот код привязывает `Name`, `Age`, и `Location` свойства связывания в `CustomCell` экземпляра, `Name`, `Age`, и `Location` свойства каждого объекта в базовой коллекции.

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

В iOS и Android Если [ `ListView` ](xref:Xamarin.Forms.ListView) перезапускается элементов и пользовательской ячейки использует пользовательское средство отрисовки, пользовательское средство отрисовки должен правильно реализовывать уведомления об изменении свойства. При повторно ячеек используются значения свойств изменится при обновлении контекста привязки, доступные ячейки, с помощью `PropertyChanged` вызванных событий. Дополнительные сведения см. в разделе [Настройка ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md). Дополнительные сведения о перезапуске ячейки, см. в разделе [стратегии кэширования](~/xamarin-forms/user-interface/listview/performance.md#caching-strategy).

## <a name="related-links"></a>Связанные ссылки

- [Встроенные в ячейках (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-builtincells)
- [Пользовательские ячейки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)
- [Привязка контекст изменен (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-bindingcontextchanged)
