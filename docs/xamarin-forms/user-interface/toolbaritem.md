---
title: Тулбаритем Xamarin. Forms
description: Класс Тулбаритем является специальной разновидностью кнопки, используемой на панели навигации приложения.
ms.prod: xamarin
ms.assetId: CC737D54-0280-46BD-A2BC-A0FB67DDD6A1
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/29/2019
ms.openlocfilehash: e6ce55f25b130c64b72a5a7e2ec2ec9a00291d14
ms.sourcegitcommit: 1341f2950b775a4daa7d0548a51fdef759afd6e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/22/2019
ms.locfileid: "69976348"
---
# <a name="xamarinforms-toolbaritem"></a>Тулбаритем Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-toolbaritem/)

Класс Xamarin. Forms [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) — это специальный тип кнопки, который можно добавить `Page` в `ToolbarItems` коллекцию объекта. Каждый `ToolbarItem` объект будет отображаться в виде кнопки на панели навигации приложения. `ToolbarItem` Экземпляр может иметь значок и отображаться как первичный или вторичный элемент меню. Класс наследует от [`MenuItem`.](xref:Xamarin.Forms.MenuItem) `ToolbarItem`

На следующих снимках `ToolbarItem` экрана показаны объекты на панели навигации в iOS и Android:

!["Демонстрационный снимок экрана тулбаритем на Android и iOS"](toolbaritem-images/toolbaritem-device-screenshot.png "Демонстрационный снимок экрана тулбаритем на устройствах Android и iOS")

`ToolbarItem` Класс определяет следующие свойства:

* [`Order`](xref:Xamarin.Forms.ToolbarItem.Order)значение перечисления, которое определяет, отображается `ToolbarItem` ли экземпляр в основном или дополнительном меню. `ToolbarItemOrder`
* [`Priority`](xref:Xamarin.Forms.ToolbarItem.Priority)значение, определяющее порядок вывода элементов `Page` в `ToolbarItems` коллекции объекта. `integer`

Класс наследует следующие свойства, `MenuItem` обычно используемые из класса: `ToolbarItem`

* [`Command`](xref:Xamarin.Forms.MenuItem.Command)— Это `ICommand` , которое позволяет привязать пользовательские действия, такие как касания пальца или щелчки, к командам, определенным в ViewModel.
* [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter)аргумент, указывающий параметр, который должен быть передан в `Command`. `object`
* [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource)значение, определяющее значок `ToolbarItem` вывода объекта. `ImageSource`
* [`Text`](xref:Xamarin.Forms.MenuItem.Text)значение типа `string` , определяющее отображаемый текст `ToolbarItem` объекта.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, `ToolbarItem` поэтому экземпляр может быть целевым объектом привязок данных.

> [!NOTE]
> Альтернативой созданию панели инструментов из [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) объектов является [`NavigationPage.TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) присвоение присоединенному свойству класса макета, содержащего несколько представлений. Дополнительные сведения см. [в разделе Отображение представлений на панели навигации](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md#displaying-views-in-the-navigation-bar).

## <a name="create-a-toolbaritem"></a>Создание Тулбаритем

Экземпляр `ToolbarItem` объекта можно создать на языке XAML. Свойства `Text` и`IconImageSource` можно задать, чтобы определить способ отображения кнопки на панели навигации. В следующем примере показано, как создать экземпляр `ToolbarItem` с некоторыми наборами общих свойств:

```xaml
<ToolbarItem Text="Example Item"
             IconImageSource="example_icon.png"
             Order="Primary"
             Priority="0" />
```

В этом примере создается `ToolbarItem` объект, который содержит текст, значок и отображается первым в области основной панели навигации. Можно также создать в коде и добавить `ToolbarItems` в коллекцию: `ToolbarItem`

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

Файл, представленный, `string`предоставленный в `IconImageSource` качестве свойства, должен существовать в каждом проекте платформы.

> [!NOTE]
> Ресурсы изображений обрабатываются по-разному на каждой платформе. `ImageSource` Может поступать из источников, включая локальный файл или внедренный ресурс, универсальный код ресурса (URI) или поток. Дополнительные сведения о настройке `IconImageSource` свойств и изображений в Xamarin. Forms см. в разделе [изображения в Xamarin. Forms](~/xamarin-forms/user-interface/images.md).

## <a name="define-button-behavior"></a>Определение поведения кнопки

Класс наследует `Clicked` событие от `MenuItem` класса. `ToolbarItem` Обработчик событий может быть присоединен к `Clicked` событию, чтобы реагировать на касания или на `ToolbarItem` экземпляры в XAML:

```xaml
<ToolbarItem ...
             Clicked="OnItemClicked" />
```

Обработчик событий можно также присоединить в коде:

```csharp
ToolbarItem item = new ToolbarItem { ... }
item.Clicked += OnItemClicked;
```

В `OnItemClicked` предыдущих примерах указан обработчик событий. В следующем коде показан пример реализации:

```csharp
void OnItemClicked(object sender, EventArgs e)
{
    ToolbarItem item = (ToolbarItem)sender;
    messageLabel.Text = $"You clicked the \"{item.Text}\" toolbar item.";
}
```

`ToolbarItem`объекты также могут использовать `Command` свойства и `CommandParameter` для реагирования на ввод пользователя без обработчиков событий. Дополнительные сведения об `ICommand` интерфейсе и привязке данных MVVM см. в разделе [поведение MVVM в Xamarin. Forms MenuItem](~/xamarin-forms/user-interface/menuitem.md#define-menuitem-behavior-with-mvvm).

## <a name="primary-and-secondary-menus"></a>Главное и дополнительное меню

Перечисление `Default`имеет `Primary`значения, `Secondary`и. `ToolbarItemOrder`

Если свойство имеет `Primary`значение, `ToolbarItem` объект будет отображаться на главной панели навигации на всех платформах. `Order` `ToolbarItem`объекты имеют приоритет над заголовком страницы, который будет обрезан, чтобы освободить место для элементов. На следующих снимках `ToolbarItem` экрана показаны объекты в основном меню iOS и Android:

![Снимок экрана "первичное меню тулбаритем для Android и iOS"](toolbaritem-images/toolbaritem-primary-menu.png "Снимок экрана основного меню тулбаритем на устройствах Android и iOS")

Если свойство имеет `Secondary`значение, поведение зависит от разных платформ. `Order` В UWP и Android меню `Secondary` Items (элементы) отображается в виде трех точек, которые можно коснуться или щелкнуть для отображения элементов в вертикальном списке. В iOS меню `Secondary` Items (элементы) отображается под панелью навигации в виде горизонтального списка. На следующих снимках экрана показано дополнительное меню в iOS и Android:

![Снимок экрана "тулбаритем-дополнительное меню" для Android и iOS "](toolbaritem-images/toolbaritem-secondary-menu.png "Снимок экрана дополнительного меню тулбаритем в Android и iOS")

> [!WARNING]
> Поведение значка `ToolbarItem` в объектах, `Order` для `Secondary` которых задано свойство, не согласовано на разных платформах. Не устанавливайте `IconImageSource` свойство для элементов, отображаемых во вторичном меню.

## <a name="related-links"></a>Связанные ссылки

* [Демонстрации Тулбаритем](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-toolbaritem/)
* [Изображения в Xamarin. Forms](~/xamarin-forms/user-interface/images.md)
* [Меню Xamarin. Forms](~/xamarin-forms/user-interface/menuitem.md)