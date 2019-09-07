---
title: Всплывающее меню
description: Добавление всплывающего меню, привязанного к определенному представлению.
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/31/2018
ms.openlocfilehash: 8a808e6ea49338de5b6bd1618fc2227e6cf5f0b1
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70764886"
---
# <a name="xamarinandroid-popup-menu"></a>Всплывающее меню Xamarin. Android

[Всплывающее меню](xref:Android.Widget.PopupMenu) (также называемое _контекстным меню_) — это меню, привязанное к определенному представлению. В следующем примере одно действие содержит кнопку. Когда пользователь нажмет кнопку, отображается всплывающее меню из трех элементов:

[![Пример приложения с кнопкой и всплывающим меню с тремя элементами](popup-menu-images/01-app-example-sml.png)](popup-menu-images/01-app-example.png#lightbox)

## <a name="creating-a-popup-menu"></a>Создание всплывающего меню

Первый шаг — создать файл ресурсов меню и поместить его в меню **ресурсы** Например, следующий код XML является кодом для меню из трех элементов, отображаемого на предыдущем снимке экрана, **Resources/Menu/popup_menu. XML**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/item1"
          android:title="item 1" />
    <item android:id="@+id/item1"
          android:title="item 2" />
    <item android:id="@+id/item1"
          android:title="item 3" />
</menu>
```

Затем создайте экземпляр `PopupMenu` и прикрепите его к представлению. При создании экземпляра `PopupMenu`вы передаете его конструктору ссылку на, `Context` а также представление, к которому будет присоединено меню. В результате всплывающее меню привязывается к этому представлению во время его создания.

В следующем примере `PopupMenu` создается в обработчике события Click для кнопки (с именем `showPopupMenu`). Эта кнопка также является представлением, к которому `PopupMenu` привязан объект, как показано в следующем примере кода:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
};
```

Наконец, всплывающее меню должно быть *сведено* к ресурсу меню, созданному ранее. В следующем примере добавляется вызов метода [Deflate](xref:Android.Views.LayoutInflater.Inflate*) в меню, а для его отображения вызывается метод [демонстрации](xref:Android.Widget.PopupMenu.Show) :

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);
    menu.Show ();
};
```

## <a name="handling-menu-events"></a>Обработка событий меню

Когда пользователь выбирает пункт меню, будет вызвано событие щелчка [MenuItemClick](xref:Android.Widget.PopupMenu.MenuItemClick) , и меню будет закрыто. Касание в любом месте вне меню просто откроет его. В любом случае при закрытии меню будет вызвано его [дисмиссевент](xref:Android.Widget.PopupMenu.Dismiss) . Следующий код добавляет обработчики событий для `MenuItemClick` событий и: `DismissEvent`

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);

    menu.MenuItemClick += (s1, arg1) => {
        Console.WriteLine ("{0} selected", arg1.Item.TitleFormatted);
    };

    menu.DismissEvent += (s2, arg2) => {
        Console.WriteLine ("menu dismissed");
    };
    menu.Show ();
};
```

## <a name="related-links"></a>Связанные ссылки

- [Попупменудемо (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/popupmenudemo)
