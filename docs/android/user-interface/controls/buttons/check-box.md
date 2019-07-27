---
title: CheckBox
ms.prod: xamarin
ms.assetid: A884AF10-D5EA-72CA-2301-B80CEC7FFBE7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: f6f594d86cab8b1173ee9f67402862e1ec2890b2
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510370"
---
# <a name="checkbox"></a>CheckBox

В этом разделе вы создадите флажок для выбора элементов, используя[`CheckBox`](xref:Android.Widget.CheckBox)
». При нажатии этого флажка всплывающее сообщение будет указывать текущее состояние флажка.

Откройте файл **Resources/Layout/Main. axml** и добавьте [`CheckBox`](xref:Android.Widget.CheckBox) элемент (внутри него [`LinearLayout`](xref:Android.Widget.LinearLayout)):

```xml
<CheckBox android:id="@+id/checkbox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="check it out" />
```

Чтобы сделать что-то при изменении состояния, добавьте следующий код в конец элемента[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
Method

```csharp
CheckBox checkbox = FindViewById<CheckBox>(Resource.Id.checkbox);

checkbox.Click += (o, e) => {
    if (checkbox.Checked)
        Toast.MakeText (this, "Selected", ToastLength.Short).Show ();
    else
        Toast.MakeText (this, "Not selected", ToastLength.Short).Show ();
};
```

Это захватывает[`CheckBox`](xref:Android.Widget.CheckBox)
, а затем обрабатывает событие щелчка, которое определяет действие, выполняемое при нажатии на флажок. При нажатии кнопки[`Checked`](xref:Android.Widget.CompoundButton.Checked)
свойство вызывается для проверки нового состояния флажка. Если он установлен, то[`Toast`](xref:Android.Widget.Toast)
Отображает сообщение "выбрано", в противном случае отображается значение "не выбрано". Тот[`CheckBox`](xref:Android.Widget.CheckBox)
обрабатывает свои собственные изменения состояния, поэтому необходимо только запросить текущее состояние.

Запустите его.

> [!TIP]
> Если необходимо изменить состояние самостоятельно (например, при загрузке сохраненного [`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference), используйте[`Checked`](xref:Android.Widget.CompoundButton.Checked)
> метод задания свойства или[`Toggle()`](xref:Android.Widget.CompoundButton.Toggle)
> метод.

*Части этой страницы являются изменениями на основе работы, созданной и совместно используемой проектом Android с открытым кодом, и используются в соответствии с терминами,* 
описанными в[*лицензии Creative Commons Attribution 2,5*](http://creativecommons.org/licenses/by/2.5/).
