---
title: Всплывающее меню
description: Добавление всплывающего меню, привязанного к определенному представлению.
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/31/2018
ms.openlocfilehash: a5370cfb8a5c4950b361e5f58b253c63f4f1e240
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029181"
---
# <a name="xamarinandroid-popup-menu"></a>Всплывающее меню Xamarin. Android

[Всплывающее меню](xref:Android.Widget.PopupMenu) (также называемое _контекстным меню_) — это меню, привязанное к определенному представлению. В следующем примере одно действие содержит кнопку. Когда пользователь нажмет кнопку, отображается всплывающее меню из трех элементов:

[![пример приложения с помощью кнопки и всплывающего меню с тремя элементами](popup-menu-images/01-app-example-sml.png)](popup-menu-images/01-app-example.png#lightbox)

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

Затем создайте экземпляр `PopupMenu` и прикрепите его к представлению. При создании экземпляра `PopupMenu`вы передаете его конструктору ссылку на `Context`, а также к представлению, к которому будет присоединено меню. В результате всплывающее меню привязывается к этому представлению во время его создания.

В следующем примере `PopupMenu` создается в обработчике событий Click для кнопки (с именем `showPopupMenu`). Эта кнопка также является представлением, к которому привязан `PopupMenu`, как показано в следующем примере кода:

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

Когда пользователь выбирает пункт меню, будет вызвано событие щелчка [MenuItemClick](xref:Android.Widget.PopupMenu.MenuItemClick) , и меню будет закрыто. Касание в любом месте вне меню просто откроет его. В любом случае при закрытии меню будет вызвано его [дисмиссевент](xref:Android.Widget.PopupMenu.Dismiss) . Следующий код добавляет обработчики событий для событий `MenuItemClick` и `DismissEvent`:

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
