---
title: CheckBox
ms.prod: xamarin
ms.assetid: A884AF10-D5EA-72CA-2301-B80CEC7FFBE7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 9f2fd10e5cfe28206d323b2769517c2584919232
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67829722"
---
# <a name="checkbox"></a>CheckBox

В этом разделе вы создадите флажок для выбора элементов, с помощью [`CheckBox`](https://developer.xamarin.com/api/type/Android.Widget.CheckBox)
мини-приложение. При нажатии флажок, появится всплывающее сообщение о текущее состояние флажка.

Откройте **Resources/layout/Main.axml** файл и добавьте [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/) элемент (внутри [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout)):

```xml
<CheckBox android:id="@+id/checkbox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="check it out" />
```

Чтобы сделать что-то, при изменении состояния, добавьте следующий код в конец [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle)
метод:

```csharp
CheckBox checkbox = FindViewById<CheckBox>(Resource.Id.checkbox);

checkbox.Click += (o, e) => {
    if (checkbox.Checked)
        Toast.MakeText (this, "Selected", ToastLength.Short).Show ();
    else
        Toast.MakeText (this, "Not selected", ToastLength.Short).Show ();
};
```

Здесь фиксируется [`CheckBox`](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/)
элемент из макета, затем обрабатывает событие щелчка, который определяет действие, которое необходимо выполнить при нажатии флажок. При нажатии [`Checked`](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/)
свойство вызывается новое состояние флажка. Если проверки, а затем [`Toast`](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
Отображает сообщение «Выбранные», в противном случае отображается «Не выбран». В [`CheckBox`](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/)
обрабатывает свои собственные изменения состояния, поэтому необходимо запрашивать текущее состояние.

Запустите его.

> [!TIP]
> Если необходимо изменить состояние самостоятельно (например, при загрузке сохраненного [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference), используйте [`Checked`](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked)
> метод задания свойства или [`Toggle()`](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle)
> метод.

*Некоторые части этой страницы, изменения с учетом работы создана и совместно используется Android откройте исходный проект и используются в соответствии с условиями, описанными в*
[*лицензии Creative Commons 2.5 Attribution* ](http://creativecommons.org/licenses/by/2.5/).
