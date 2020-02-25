---
title: Тулбаритем Xamarin. Forms
description: Класс Тулбаритем является специальной разновидностью кнопки, используемой на панели навигации приложения.
ms.prod: xamarin
ms.assetId: CC737D54-0280-46BD-A2BC-A0FB67DDD6A1
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/29/2019
ms.openlocfilehash: afdf9029f836ac8e55b2bb338b31f669af946c12
ms.sourcegitcommit: 6d86aac422d6ce2131930d18ada161d117c8c61b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/24/2020
ms.locfileid: "77567084"
---
# <a name="xamarinforms-toolbaritem"></a>Тулбаритем Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-toolbaritem/)

Класс [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) Xamarin. Forms — это специальный тип кнопки, который можно добавить в коллекцию `ToolbarItems` объекта `Page`. Каждый объект `ToolbarItem` будет отображаться в виде кнопки на панели навигации приложения. Экземпляр `ToolbarItem` может иметь значок и отображаться как первичный или вторичный элемент меню. Класс `ToolbarItem` наследует от [`MenuItem`](xref:Xamarin.Forms.MenuItem).

На следующих снимках экрана показаны `ToolbarItem` объекты на панели навигации в iOS и Android:

!["Демонстрационный снимок экрана Тулбаритем на Android и iOS"](toolbaritem-images/toolbaritem-device-screenshot.png "Демонстрационный снимок экрана Тулбаритем на устройствах Android и iOS")

Класс `ToolbarItem` определяет следующие свойства:

* [`Order`](xref:Xamarin.Forms.ToolbarItem.Order) — это `ToolbarItemOrder` перечислимое значение, которое определяет, отображается ли экземпляр `ToolbarItem` в главном или дополнительном меню.
* [`Priority`](xref:Xamarin.Forms.ToolbarItem.Priority) — это `integer` значение, определяющее порядок вывода элементов в коллекции `ToolbarItems` `Page` объекта.

Класс `ToolbarItem` наследует следующие свойства, обычно используемые классом `MenuItem`:

* [`Command`](xref:Xamarin.Forms.MenuItem.Command) — это `ICommand`, который позволяет привязать действия пользователя, такие как касания пальца или щелчки, к командам, определенным в ViewModel.
* [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter) — это `object`, указывающий параметр, который должен быть передан `Command`.
* [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource) — это `ImageSource` значение, определяющее значок вывода объекта `ToolbarItem`.
* [`Text`](xref:Xamarin.Forms.MenuItem.Text) — это `string`, который определяет отображаемый текст объекта `ToolbarItem`.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, поэтому экземпляр `ToolbarItem` может быть целевым объектом привязок данных.

> [!NOTE]
> Альтернативой созданию панели инструментов из [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) объектов является установка [`NavigationPage.TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) присоединенным свойством класса макета, который содержит несколько представлений. Дополнительные сведения см. [в разделе Отображение представлений на панели навигации](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md#displaying-views-in-the-navigation-bar).

## <a name="create-a-toolbaritem"></a>Создание Тулбаритем

В XAML можно создать экземпляр объекта `ToolbarItem`. Свойства `Text` и `IconImageSource` можно задать, чтобы определить способ отображения кнопки на панели навигации. В следующем примере показано, как создать экземпляр `ToolbarItem` с заданными общими свойствами и добавить его в коллекцию `ToolbarItems` `ContentPage`.

```xaml
<ContentPage.ToolbarItems>
    <ToolbarItem Text="Example Item"
                 IconImageSource="example_icon.png"
                 Order="Primary"
                 Priority="0" />
</ContentPage.ToolbarItems>
```

В этом примере создается объект `ToolbarItem`, который содержит текст, значок и отображается первым в области основной панели навигации. `ToolbarItem` также можно создать в коде и добавить в коллекцию `ToolbarItems`:

```csharp
ToolbarItem item = new ToolbarItem
{
    Text = "Example Item",
    IconImageSource = ImageSource.FromFile("example_icon.png"),
    Order = ToolbarItemOrder.Primary,
    Priority = 0
};

// "this" refers to a Page object
this.ToolbarItems.Add(item);
```

Файл, представленный `string`, предоставленный в качестве свойства `IconImageSource`, должен существовать в каждом проекте платформы.

> [!NOTE]
> Ресурсы изображений обрабатываются по-разному на каждой платформе. `ImageSource` может поступать из источников, включая локальный файл или внедренный ресурс, универсальный код ресурса (URI) или поток. Дополнительные сведения о настройке свойства `IconImageSource` и изображений в Xamarin. Forms см. в разделе [изображения в Xamarin. Forms](~/xamarin-forms/user-interface/images.md).

## <a name="define-button-behavior"></a>Определение поведения кнопки

Класс `ToolbarItem` наследует событие `Clicked` из класса `MenuItem`. Обработчик событий может быть присоединен к событию `Clicked`, чтобы реагировать на касания или на экземпляры `ToolbarItem` в XAML:

```xaml
<ToolbarItem ...
             Clicked="OnItemClicked" />
```

Обработчик событий можно также присоединить в коде:

```csharp
ToolbarItem item = new ToolbarItem { ... }
item.Clicked += OnItemClicked;
```

В предыдущих примерах был указан обработчик событий `OnItemClicked`. В следующем коде показан пример реализации:

```csharp
void OnItemClicked(object sender, EventArgs e)
{
    ToolbarItem item = (ToolbarItem)sender;
    messageLabel.Text = $"You clicked the \"{item.Text}\" toolbar item.";
}
```

`ToolbarItem` объекты также могут использовать свойства `Command` и `CommandParameter`, чтобы реагировать на ввод пользователя без обработчиков событий. Дополнительные сведения о `ICommand` интерфейсе и привязке данных MVVM см. в разделе [поведение MVVM в элементе меню Xamarin. Forms](~/xamarin-forms/user-interface/menuitem.md#define-menuitem-behavior-with-mvvm).

## <a name="enable-or-disable-a-toolbaritem-at-runtime"></a>Включение или отключение Тулбаритем во время выполнения

Чтобы включить отключение `ToolbarItem` во время выполнения, привяжите его свойство `Command` к реализации `ICommand` и убедитесь, что делегат `canExecute` включает и отключает `ICommand` соответствующим образом.

Дополнительные сведения см. в разделе [Включение или отключение элемента MenuItem во время выполнения](menuitem.md#enable-or-disable-a-menuitem-at-runtime).

## <a name="primary-and-secondary-menus"></a>Главное и дополнительное меню

Перечисление `ToolbarItemOrder` имеет значения `Default`, `Primary`и `Secondary`.

Если свойство `Order` имеет значение `Primary`, то объект `ToolbarItem` будет отображаться на главной панели навигации на всех платформах. `ToolbarItem` объекты имеют приоритет над заголовком страницы, который будет обрезан, чтобы освободить место для элементов. На следующих снимках экрана показаны `ToolbarItem` объекты в основном меню iOS и Android:

![Снимок экрана "первичное меню Тулбаритем для Android и iOS"](toolbaritem-images/toolbaritem-primary-menu.png "Снимок экрана основного меню Тулбаритем на устройствах Android и iOS")

Если свойство `Order` имеет значение `Secondary`, поведение зависит от разных платформ. В UWP и Android меню `Secondary` элементы отображается в виде трех точек, которые можно коснуться или щелкнуть для отображения элементов в вертикальном списке. В iOS меню `Secondary` Items (элементы) отображается под панелью навигации в виде горизонтального списка. На следующих снимках экрана показано дополнительное меню в iOS и Android:

![Снимок экрана "Тулбаритем-дополнительное меню" для Android и iOS "](toolbaritem-images/toolbaritem-secondary-menu.png "Снимок экрана дополнительного меню Тулбаритем в Android и iOS")

> [!WARNING]
> Поведение значков в `ToolbarItem` объектах, свойство `Order` которых имеет значение `Secondary`, не согласовано на разных платформах. Не устанавливайте свойство `IconImageSource` для элементов, отображаемых во вторичном меню.

## <a name="related-links"></a>Связанные ссылки

* [Демонстрации Тулбаритем](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-toolbaritem/)
* [Изображения в Xamarin. Forms](~/xamarin-forms/user-interface/images.md)
* [Xamarin.Forms MenuItem](~/xamarin-forms/user-interface/menuitem.md)
