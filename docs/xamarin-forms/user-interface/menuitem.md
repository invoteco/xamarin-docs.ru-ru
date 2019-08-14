---
title: Меню Xamarin. Forms
description: Класс MenuItem используется для создания пунктов меню для меню, таких как контекстные меню элементов ListView и всплывающих меню приложения оболочки.
ms.prod: xamarin
ms.assetId: 62655C21-6053-466D-A7F4-DE2BE36538F5
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/01/2019
ms.openlocfilehash: 68560c6cc814f54bb8ba9348bc53334089c36a93
ms.sourcegitcommit: 41a029c69925e3a9d2de883751ebfd649e8747cd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/13/2019
ms.locfileid: "68984476"
---
# <a name="xamarinforms-menuitem"></a>Меню Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/en-us/samples/xamarin/xamarin-forms-samples/userinterface-menuitem/)

Класс Xamarin. Forms [`MenuItem`](xref:Xamarin.Forms.MenuItem) используется для определения пунктов меню для меню, таких как `ListView` контекстные меню элементов и всплывающие меню приложения оболочки.

На следующем снимке `MenuItem` экрана показаны объекты `ListView` в контекстном меню в iOS и Android:

[ !["MenuItems в iOS и Android"](menuitem-images/menuitem-demo-cropped.png "MenuItems в iOS и Android") ] (menuitem-images/menuitem-demo-full.png#lightbox "MenuItems в iOS и Androidfull image")

`MenuItem` Класс определяет следующие свойства:

* [`Command`](xref:Xamarin.Forms.MenuItem.Command)— Это `ICommand` , которое позволяет привязать пользовательские действия, такие как касания пальца или щелчки, к командам, определенным в ViewModel.
* [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter)аргумент, указывающий параметр, который должен быть передан в `Command`. `object`
* [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource)`ImageSource` значение, определяющее значок вывода.
* [`IsDestructive`](xref:Xamarin.Forms.MenuItem.IsDestructive)значение, указывающее, `MenuItem` удаляет ли связанный элемент пользовательского интерфейса из списка. `bool`
* [`IsEnabled`](xref:Xamarin.Forms.MenuItem.IsEnabled)`bool` значение, определяющее, реагирует ли этот объект на вводимые пользователем данные.
* [`Text`](xref:Xamarin.Forms.MenuItem.Text)`string` значение, указывающее отображаемый текст.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, `MenuItem` поэтому экземпляр может быть целевым объектом привязок данных.

## <a name="create-a-menuitem"></a>Создание элемента меню

`MenuItem`объекты можно использовать в контекстном меню `ListView` элементов объекта. Наиболее распространенным шаблоном является создание `MenuItem` объектов `ViewCell` в экземпляре, `DataTemplate` который используется в `ListView`качестве объекта для s `ItemTemplate`. Когда объект заполняется, он создает каждый элемент `MenuItem` с помощью, предоставляя варианты при активации контекстного меню для элемента. `DataTemplate` `ListView`

В следующем примере показано `MenuItem` создание экземпляра в контексте `ListView` объекта:

```xaml
<ListView>
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <ViewCell.ContextActions>
                    <MenuItem Text="Context Menu Option" />
                </ViewCell.ContextActions>
                <Label Text="{Binding .}" />
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Также `MenuItem` можно создать в коде:

```csharp
// A function returns a ViewCell instance that
// is used as the template for each list item
DataTemplate dataTemplate = new DataTemplate(() =>
{
    // A Label displays the list item text
    Label label = new Label();
    label.SetBinding(Label.TextProperty, ".");

    // A ViewCell serves as the DataTemplate
    ViewCell viewCell = new ViewCell
    {
        View = label
    };

    // Add a MenuItem instance to the ContextActions
    MenuItem menuItem = new MenuItem
    {
        Text = "Context Menu Option"
    };
    viewCell.ContextActions.Add(menuItem);

    // The function returns the custom ViewCell
    // to the DataTemplate constructor
    return viewCell;
});

// Finally, the dataTemplate is provided to
// the ListView object
ListView listView = new ListView
{
    ...
    ItemTemplate = dataTemplate
};
```

## <a name="define-menuitem-behavior-with-events"></a>Определение поведения элемента MenuItem с помощью событий

Класс `MenuItem` представляет событие `Clicked`. Обработчик событий может быть присоединен к этому событию, чтобы реагировать на касания или на `MenuItem` экземпляр в XAML:

```xaml
<MenuItem ...
          Clicked="OnItemClicked" />
```

Обработчик событий можно также присоединить в коде:

```csharp
MenuItem item = new MenuItem { ... }
item.Clicked += OnItemClicked;
```

В `OnItemClicked` предыдущих примерах указан обработчик событий. В следующем коде показан пример реализации:

```csharp
void OnItemClicked(object sender, EventArgs e)
{
    // The sender is the menuItem
    MenuItem menuItem = sender as MenuItem;

    // Access the list item through the BindingContext
    var contextItem = menuItem.BindingContext;

    // Do something with the contextItem here
}
```

## <a name="define-menuitem-behavior-with-mvvm"></a>Определение поведения MenuItem с помощью MVVM

Класс поддерживает шаблон Model-View-ViewModel (MVVM) через [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объекты и `ICommand` интерфейс. `MenuItem` В следующем коде XAML `MenuItem` показаны экземпляры, привязанные к командам, определенным в ViewModel:

```xaml
<ContentPage.BindingContext>
    <viewmodels:ListPageViewModel />
</ContentPage.BindingContext>

<StackLayout>
    <Label Text="{Binding Message}" ... />
    <ListView ItemsSource="{Binding Items}">
        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <ViewCell.ContextActions>
                        <MenuItem Text="Edit"
                                    IconImageSource="icon.png"
                                    Command="{Binding Source={x:Reference contentPage}, Path=BindingContext.EditCommand}"
                                    CommandParameter="{Binding .}"/>
                        <MenuItem Text="Delete"
                                    Command="{Binding Source={x:Reference contentPage}, Path=BindingContext.DeleteCommand}"
                                    CommandParameter="{Binding .}"/>
                    </ViewCell.ContextActions>
                    <Label Text="{Binding .}" />
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackLayout>
```

В предыдущем примере два `MenuItem` объекта определяются с их `Command` свойствами и, `CommandParameter` привязанными к командам в ViewModel. ViewModel содержит команды, упоминаемые в XAML:

```csharp
public class ListPageViewModel : INotifyPropertyChanged
{
    ...

    public ICommand EditCommand => new Command<string>((string item) =>
    {
        Message = $"Edit command was called on: {item}";
    });

    public ICommand DeleteCommand => new Command<string>((string item) =>
    {
        Message = $"Delete command was called on: {item}";
    });
}
```

Пример приложения включает `DataService` класс, используемый для получения списка элементов для заполнения `ListView` объектов. Создается экземпляр ViewModel, элементы `DataService` класса и задаются `BindingContext` как в коде программной части:

```csharp
public MenuItemXamlMvvmPage()
{
    InitializeComponent();
    BindingContext = new ListPageViewModel(DataService.GetListItems());
}
```

## <a name="menuitem-icons"></a>Значки MenuItem

> [!WARNING]
> `MenuItem`на устройствах Android отображаются только значки. На других платформах будет отображаться только текст, заданный `Text` свойством.

 Значки задаются с `IconImageSource` помощью свойства. Если указан значок, то текст, заданный `Text` свойством, не будет отображаться. На следующем снимке экрана `MenuItem` показан значок со значком на Android:

![Снимок экрана значка MenuItem в Android](menuitem-images/menuitem-android-icon.png "Снимок экрана значка MenuItem в Android")

Дополнительные сведения об использовании образов в Xamarin. Forms см. в разделе [изображения в Xamarin. Forms](~/xamarin-forms/user-interface/images.md).

## <a name="cross-platform-context-menu-behavior"></a>Поведение контекстного меню на разных платформах

К контекстным меню обращаются и отображаются по-разному на каждой платформе.

В Android контекстное меню активируется длительным нажатием элемента списка. Контекстное меню заменяет заголовок и область панели навигации, а `MenuItem` параметры отображаются горизонтальными кнопками.

!["Снимок экрана контекстного меню в Android"](menuitem-images/menuitem-android-icon.png "Снимок экрана: контекстное меню в Android")

В iOS контекстное меню активируется функцией прокрутки на элементе списка. Контекстное меню отображается в элементе списка и `MenuItems` отображается в виде горизонтальных кнопок.

!["Снимок экрана контекстного меню в iOS"](menuitem-images/menuitem-ios-contextmenu.png "Снимок экрана: контекстное меню в iOS")

В UWP контекстное меню активируется щелчком правой кнопкой мыши по элементу списка. Контекстное меню отображается рядом с курсором в виде вертикального списка.

!["Снимок экрана контекстного меню в UWP"](menuitem-images/menuitem-uwp.png "Снимок экрана: контекстное меню в UWP")

## <a name="related-links"></a>Связанные ссылки

* [Демонстрации MenuItem](https://docs.microsoft.com/en-us/samples/xamarin/xamarin-forms-samples/userinterface-menuitem/)
* [Изображения в Xamarin. Forms](~/xamarin-forms/user-interface/images.md)
