---
title: Меню Xamarin. Forms
description: Класс MenuItem используется для создания пунктов меню для меню, таких как контекстные меню элементов ListView и всплывающих меню приложения оболочки.
ms.prod: xamarin
ms.assetId: 62655C21-6053-466D-A7F4-DE2BE36538F5
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/01/2019
ms.openlocfilehash: 5bc36f03eac4ced7c19a0053dfea93dbe2ca4497
ms.sourcegitcommit: 850dd7a3ed10eb3f66692e765d3e31438cff0288
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2019
ms.locfileid: "72531010"
---
# <a name="xamarinforms-menuitem"></a>Меню Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-menuitemdemos/)

Класс [`MenuItem`](xref:Xamarin.Forms.MenuItem) Xamarin. Forms определяет пункты меню для меню, такие как контекстные меню `ListView` элементов и всплывающие меню приложения оболочки.

На следующих снимках экрана показаны `MenuItem` объекты в контекстном меню `ListView` в iOS и Android:

[!["MenuItems в iOS и Android"](menuitem-images/menuitem-demo-cropped.png "MenuItems в iOS и Android")](menuitem-images/menuitem-demo-full.png#lightbox "MenuItems в iOS и Android full image")

Класс `MenuItem` определяет следующие свойства:

* [`Command`](xref:Xamarin.Forms.MenuItem.Command) — это `ICommand`, который позволяет привязать действия пользователя, такие как касания пальца или щелчки, к командам, определенным в ViewModel.
* [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter) — это `object`, указывающий параметр, который должен быть передан `Command`.
* [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource) — это `ImageSource` значение, определяющее значок вывода.
* [`IsDestructive`](xref:Xamarin.Forms.MenuItem.IsDestructive) — это `bool` значение, указывающее, удаляет ли `MenuItem` связанный с ним элемент пользовательского интерфейса из списка.
* [`IsEnabled`](xref:Xamarin.Forms.MenuItem.IsEnabled) — это `bool` значение, определяющее, реагирует ли этот объект на вводимые пользователем данные.
* [`Text`](xref:Xamarin.Forms.MenuItem.Text) — это `string` значение, указывающее отображаемый текст.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, поэтому экземпляр `MenuItem` может быть целевым объектом привязок данных.

## <a name="create-a-menuitem"></a>Создание элемента меню

`MenuItem` объекты можно использовать в контекстном меню элементов объекта `ListView`. Наиболее распространенный шаблон — создание `MenuItem` объектов в экземпляре `ViewCell`, который используется в качестве объекта `DataTemplate` для `ListView`s `ItemTemplate`. Когда объект `ListView` заполняется, он создает каждый элемент с помощью `DataTemplate`, предоставляя возможность выбора `MenuItem` при активации контекстного меню для элемента.

В следующем примере показано создание экземпляра `MenuItem` в контексте объекта `ListView`.

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

@No__t_0 также можно создать в коде:

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

Класс `MenuItem` представляет событие `Clicked`. Обработчик событий может быть присоединен к этому событию, чтобы реагировать на касания или на экземпляр `MenuItem` в XAML:

```xaml
<MenuItem ...
          Clicked="OnItemClicked" />
```

Обработчик событий можно также присоединить в коде:

```csharp
MenuItem item = new MenuItem { ... }
item.Clicked += OnItemClicked;
```

В предыдущих примерах был указан обработчик событий `OnItemClicked`. В следующем коде показан пример реализации:

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

Класс `MenuItem` поддерживает шаблон Model-View-ViewModel (MVVM) через [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объекты и интерфейс `ICommand`. В следующем коде XAML показаны `MenuItem` экземпляры, привязанные к командам, определенным в ViewModel:

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

В предыдущем примере определены два `MenuItem` объектов с их `Command` и `CommandParameter` свойства, привязанные к командам в ViewModel. ViewModel содержит команды, упоминаемые в XAML:

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

Пример приложения включает класс `DataService`, используемый для получения списка элементов для заполнения объектов `ListView`. Создается экземпляр ViewModel, элементы класса `DataService` и устанавливаются в качестве `BindingContext` в коде программной части:

```csharp
public MenuItemXamlMvvmPage()
{
    InitializeComponent();
    BindingContext = new ListPageViewModel(DataService.GetListItems());
}
```

## <a name="menuitem-icons"></a>Значки MenuItem

> [!WARNING]
> `MenuItem` объекты отображаются только значки на устройстве Android. На других платформах будет отображаться только текст, заданный свойством `Text`.

 Значки задаются с помощью свойства `IconImageSource`. Если указан значок, то текст, заданный свойством `Text`, не будет отображаться. На следующем снимке экрана показан `MenuItem` со значком на Android:

![Снимок экрана значка MenuItem в Android](menuitem-images/menuitem-android-icon.png "Снимок экрана значка MenuItem в Android")

Дополнительные сведения об использовании образов в Xamarin. Forms см. в разделе [изображения в Xamarin. Forms](~/xamarin-forms/user-interface/images.md).

## <a name="cross-platform-context-menu-behavior"></a>Поведение контекстного меню на разных платформах

К контекстным меню обращаются и отображаются по-разному на каждой платформе.

В Android контекстное меню активируется длительным нажатием элемента списка. Контекстное меню заменяет заголовок и область панели навигации, а параметры `MenuItem` отображаются как горизонтальные кнопки.

!["Снимок экрана контекстного меню в Android"](menuitem-images/menuitem-android-icon.png "Снимок экрана: контекстное меню в Android")

В iOS контекстное меню активируется функцией прокрутки на элементе списка. Контекстное меню отображается в элементе списка, а `MenuItems` отображаются как горизонтальные кнопки.

!["Снимок экрана контекстного меню в iOS"](menuitem-images/menuitem-ios-contextmenu.png "Снимок экрана: контекстное меню в iOS")

В UWP контекстное меню активируется щелчком правой кнопкой мыши по элементу списка. Контекстное меню отображается рядом с курсором в виде вертикального списка.

!["Снимок экрана контекстного меню в UWP"](menuitem-images/menuitem-uwp.png "Снимок экрана: контекстное меню в UWP")

## <a name="related-links"></a>Связанные ссылки

* [Демонстрации MenuItem](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-menuitemdemos/)
* [Изображения в Xamarin. Forms](~/xamarin-forms/user-interface/images.md)
