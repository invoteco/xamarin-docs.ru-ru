---
title: CheckBox
ms.prod: xamarin
ms.assetid: A884AF10-D5EA-72CA-2301-B80CEC7FFBE7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: b947217706fc8ef7ce7945bf4c88349f4367ffcd
ms.sourcegitcommit: cf56d2bae34dc0f8e94c2d3d28d5f460d59807bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985676"
---
# <a name="checkbox"></a>CheckBox

В этом разделе вы создадите флажок для выбора элементов с помощью [`CheckBox`](xref:Android.Widget.CheckBox) мини-приложения. При нажатии этого флажка всплывающее сообщение будет указывать текущее состояние флажка.

Откройте файл **Resources/Layout/Main. axml** и добавьте [`CheckBox`](xref:Android.Widget.CheckBox) элемент (внутри него [`LinearLayout`](xref:Android.Widget.LinearLayout)):

```xml
<CheckBox android:id="@+id/checkbox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="check it out" />
```

Чтобы сделать что-то при изменении состояния, добавьте в конец [`OnCreate()`](xref:Android.App.Activity.OnCreate*) метода следующий код:

```csharp
CheckBox checkbox = FindViewById<CheckBox>(Resource.Id.checkbox);

checkbox.Click += (o, e) => {
    if (checkbox.Checked)
        Toast.MakeText (this, "Selected", ToastLength.Short).Show ();
    else
        Toast.MakeText (this, "Not selected", ToastLength.Short).Show ();
};
```

Это захватывает [`CheckBox`](xref:Android.Widget.CheckBox) элемент из макета, а затем обрабатывает событие щелчка, которое определяет действие, выполняемое при нажатии на флажок. При нажатии вызывается [`Checked`](xref:Android.Widget.CompoundButton.Checked) свойство, чтобы проверить новое состояние флажка. Если флажок установлен, [`Toast`](xref:Android.Widget.Toast) то отображается сообщение "выбрано", в противном случае отображается значение "не выбрано". [`CheckBox`](xref:Android.Widget.CheckBox) Обработчик обрабатывает свои собственные изменения состояния, поэтому необходимо только запросить текущее состояние.

Запустите его.

> [!TIP]
> Если необходимо изменить состояние самостоятельно (например, при загрузке сохраненного [`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)файла, [`Checked`](xref:Android.Widget.CompoundButton.Checked) используйте метод задания или [`Toggle()`](xref:Android.Widget.CompoundButton.Toggle) метода свойства).

*Части этой страницы являются изменениями на основе работы, созданной и совместно используемой проектом Android с открытым кодом, и используются в соответствии с терминами, описанными в разделе* [*Лицензия на атрибуты Creative commons attribution 2,5*](http://creativecommons.org/licenses/by/2.5/).
