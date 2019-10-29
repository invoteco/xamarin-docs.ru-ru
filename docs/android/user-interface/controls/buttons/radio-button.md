---
title: RadioButton
ms.prod: xamarin
ms.assetid: 3C32EA3F-D917-C988-72C5-A17354DA791E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 9f51adcbd1accb4f780318cc0853e612ed8e5bed
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029306"
---
# <a name="radiobutton"></a>RadioButton

В этом разделе вы создадите два взаимоисключающих переключателя (что позволяет отключить другой), используя [`RadioGroup`](xref:Android.Widget.RadioGroup)
и [`RadioButton`](xref:Android.Widget.RadioButton)
мини-приложения. При нажатии любого из этих переключателей появится всплывающее сообщение.

Откройте файл **Resources/Layout/Main. axml** и добавьте два [`RadioButton`](xref:Android.Widget.RadioButton)s, вложенные в [`RadioGroup`](xref:Android.Widget.RadioGroup) (внутри [`LinearLayout`](xref:Android.Widget.LinearLayout)):

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

Важно, чтобы [`RadioButton`](xref:Android.Widget.RadioButton)s были сгруппированы вместе с элементом [`RadioGroup`](xref:Android.Widget.RadioGroup) , чтобы одновременно не было выбрано ни одно. Эта логика автоматически обрабатывается системой Android. Когда один [`RadioButton`](xref:Android.Widget.RadioButton)
в группе выбраны все остальные флажки автоматически отменяются.

Чтобы сделать что-то при выборе каждого [`RadioButton`](xref:Android.Widget.RadioButton) , необходимо написать обработчик событий:

```csharp
private void RadioButtonClick (object sender, EventArgs e)
{
    RadioButton rb = (RadioButton)sender;
    Toast.MakeText (this, rb.Text, ToastLength.Short).Show ();
}
```

Во-первых, передаваемый отправитель преобразуется в RadioButton.
Затем [`Toast`](xref:Android.Widget.Toast)
сообщение отображает текст выбранного переключателя.

Теперь в нижней части [`OnCreate()`](xref:Android.App.Activity.OnCreate*)
Добавьте следующее:

```csharp
RadioButton radio_red = FindViewById<RadioButton>(Resource.Id.radio_red);
RadioButton radio_blue = FindViewById<RadioButton>(Resource.Id.radio_blue);

radio_red.Click += RadioButtonClick;
radio_blue.Click += RadioButtonClick;
```

Это захватывает все [`RadioButton`](xref:Android.Widget.RadioButton)s из макета и добавляет только что созданное событие хандлерто.

Запустите приложение.

> [!TIP]
> Если необходимо изменить состояние самостоятельно (например, при загрузке сохраненной [`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)), используйте [`Checked`](xref:Android.Widget.CompoundButton.Checked)
> метод задания свойства или [`Toggle()`](xref:Android.Widget.CompoundButton.Toggle)
> метод.

*Части этой страницы являются изменениями на основе работы, созданной и совместно используемой проектом Android с открытым кодом, и используются в соответствии с терминами, описанными в* [*лицензии
Creative Commons Attribution 2,5*](https://creativecommons.org/licenses/by/2.5/). 
