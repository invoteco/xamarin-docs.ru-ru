---
title: RadioButton
ms.prod: xamarin
ms.assetid: 3C32EA3F-D917-C988-72C5-A17354DA791E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: c1dabfcd481dccf50075c02c54019ee27499769f
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758828"
---
# <a name="radiobutton"></a>RadioButton

В этом разделе вы создадите два взаимоисключающих переключателя (что позволяет отключить друг друга), используя[`RadioGroup`](xref:Android.Widget.RadioGroup)
перетаскивани[`RadioButton`](xref:Android.Widget.RadioButton)
мини-приложения. При нажатии любого из этих переключателей появится всплывающее сообщение.

Откройте файл **Resources/Layout/Main. axml** и добавьте [`RadioButton`](xref:Android.Widget.RadioButton)два файла [`RadioGroup`](xref:Android.Widget.RadioGroup) , вложенные в (внутри [`LinearLayout`](xref:Android.Widget.LinearLayout)):

```xml
<RadioGroup
  android:layout_width="fill_parent"
  android:layout_height="wrap_content"
  android:orientation="vertical">
  <RadioButton android:id="@+id/radio_red"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Red" />
  <RadioButton android:id="@+id/radio_blue"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Blue" />
</RadioGroup>
```

Важно, [`RadioButton`](xref:Android.Widget.RadioButton)чтобы объекты группируются вместе [`RadioGroup`](xref:Android.Widget.RadioGroup) по элементу, чтобы одновременно можно было выбрать не более одного. Эта логика автоматически обрабатывается системой Android. Когда один[`RadioButton`](xref:Android.Widget.RadioButton)
в группе выбраны все остальные флажки автоматически отменяются.

Чтобы сделать что-то [`RadioButton`](xref:Android.Widget.RadioButton) при выборе каждого из них, необходимо написать обработчик событий:

```csharp
private void RadioButtonClick (object sender, EventArgs e)
{
    RadioButton rb = (RadioButton)sender;
    Toast.MakeText (this, rb.Text, ToastLength.Short).Show ();
}
```

Во-первых, передаваемый отправитель преобразуется в RadioButton.
Затем элемент[`Toast`](xref:Android.Widget.Toast)
сообщение отображает текст выбранного переключателя.

Теперь в нижней части[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
Добавьте следующее:

```csharp
RadioButton radio_red = FindViewById<RadioButton>(Resource.Id.radio_red);
RadioButton radio_blue = FindViewById<RadioButton>(Resource.Id.radio_blue);

radio_red.Click += RadioButtonClick;
radio_blue.Click += RadioButtonClick;
```

Это захватывает каждый из объектов [`RadioButton`](xref:Android.Widget.RadioButton)из макета и добавляет только что созданное событие хандлерто.

Запустите приложение.

> [!TIP]
> Если необходимо изменить состояние самостоятельно (например, при загрузке сохраненного [`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)), используйте[`Checked`](xref:Android.Widget.CompoundButton.Checked)
> метод задания свойства или[`Toggle()`](xref:Android.Widget.CompoundButton.Toggle)
> метод.

*Части этой страницы являются изменениями на основе работы, созданной и совместно используемой проектом Android с открытым кодом, и используются в соответствии с терминами,* 
описанными в[*лицензии Creative Commons Attribution 2,5*](http://creativecommons.org/licenses/by/2.5/). 
