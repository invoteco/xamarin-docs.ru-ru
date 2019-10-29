---
title: CheckBox
ms.prod: xamarin
ms.assetid: A884AF10-D5EA-72CA-2301-B80CEC7FFBE7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 06908ad8993eb6d47476006b23865fa1c7fe694f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029348"
---
# <a name="checkbox"></a>CheckBox

В этом разделе вы создадите флажок для выбора элементов с помощью мини-приложения [`CheckBox`](xref:Android.Widget.CheckBox) . При нажатии этого флажка всплывающее сообщение будет указывать текущее состояние флажка.

Откройте файл **Resources/Layout/Main. axml** и добавьте элемент [`CheckBox`](xref:Android.Widget.CheckBox) (внутри [`LinearLayout`](xref:Android.Widget.LinearLayout)):

```xml
<CheckBox android:id="@+id/checkbox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="check it out" />
```

Чтобы сделать что-то при изменении состояния, добавьте следующий код в конец метода [`OnCreate()`](xref:Android.App.Activity.OnCreate*) :

```csharp
CheckBox checkbox = FindViewById<CheckBox>(Resource.Id.checkbox);

checkbox.Click += (o, e) => {
    if (checkbox.Checked)
        Toast.MakeText (this, "Selected", ToastLength.Short).Show ();
    else
        Toast.MakeText (this, "Not selected", ToastLength.Short).Show ();
};
```

Это захватывает элемент [`CheckBox`](xref:Android.Widget.CheckBox) из макета, а затем обрабатывает событие щелчка, которое определяет действие, выполняемое при нажатии на флажок. При нажатии вызывается свойство [`Checked`](xref:Android.Widget.CompoundButton.Checked) для проверки нового состояния флажка. Если этот флажок установлен, [`Toast`](xref:Android.Widget.Toast) отображает сообщение "Selected", в противном случае отображается значение "не выбрано". [`CheckBox`](xref:Android.Widget.CheckBox) обрабатывает свои собственные изменения состояния, поэтому необходимо только запросить текущее состояние.

Запустите его.

> [!TIP]
> Если необходимо изменить состояние самостоятельно (например, при загрузке сохраненного [`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference), используйте метод задания свойства [`Checked`](xref:Android.Widget.CompoundButton.Checked) или [`Toggle()`](xref:Android.Widget.CompoundButton.Toggle) .

*Части этой страницы являются изменениями на основе работы, созданной и совместно используемой проектом Android с открытым кодом, и используются в соответствии с терминами, описанными в* [*лицензии Creative Commons Attribution 2,5*](https://creativecommons.org/licenses/by/2.5/).
