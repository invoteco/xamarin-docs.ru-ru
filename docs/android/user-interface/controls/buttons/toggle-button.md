---
title: ToggleButton
ms.prod: xamarin
ms.assetid: 9ADA8FCF-63ED-897A-DD56-D7D86535A92C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: fe444d255beb9c08b4b5bcf5de36a8740e503b55
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029296"
---
# <a name="togglebutton"></a>ToggleButton

В этом разделе вы создадите кнопку, которая будет использоваться специально для переключения между двумя состояниями с помощью мини-приложения [`ToggleButton`](xref:Android.Widget.ToggleButton) . Это мини-приложение является отличной альтернативой переключателям, если имеется два простых состояния, которые являются взаимоисключающими (например, "on" и "OFF"). В Android 4,0 (API уровня 14) появилась альтернатива выключателю, называемый [`Switch`](xref:Android.Widget.Switch).

Пример выключателя **можно увидеть** в левой паре изображений, в то время как в правой части изображений представлен пример **переключателя**:

![Примеры переключателей и Тогглебуттонс в состояниях on и Off](toggle-button-images/togglebutton-switch.png)  

Элемент управления, используемый приложением, зависит от стиля. Оба мини-приложения функционально эквивалентны.

Откройте файл **Resources/Layout/Main. axml** и добавьте элемент [`ToggleButton`](xref:Android.Widget.ToggleButton) (внутри [`LinearLayout`](xref:Android.Widget.LinearLayout)):

Чтобы сделать что-то при изменении состояния, добавьте следующий код в конец [`OnCreate()`](xref:Android.App.Activity.OnCreate*)
Method

```csharp
ToggleButton togglebutton = FindViewById<ToggleButton>(Resource.Id.togglebutton);

togglebutton.Click += (o, e) => {
    // Perform action on clicks
    if (togglebutton.Checked)
        Toast.MakeText(this, "Checked", ToastLength.Short).Show ();
    else
        Toast.MakeText(this, "Not checked", ToastLength.Short).Show ();
};
```

Это захватывает элемент [`ToggleButton`](xref:Android.Widget.ToggleButton) из макета и обрабатывает событие щелчка, которое определяет действие, выполняемое при нажатии кнопки. В этом примере метод проверяет новое состояние кнопки, а затем отображает [`Toast`](xref:Android.Widget.Toast) сообщение, указывающее текущее состояние.

Обратите внимание, что [`ToggleButton`](xref:Android.Widget.ToggleButton) обрабатывает свое собственное изменение состояния между установленным и неустановленным, поэтому вы просто запрашиваете его.

Запустите приложение.

> [!TIP]
> Если необходимо изменить состояние самостоятельно (например, при загрузке сохраненной [`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)), используйте [`Checked`](xref:Android.Widget.CompoundButton.Checked)
> метод задания свойства или [`Toggle()`](xref:Android.Widget.CompoundButton.Toggle)
> метод.

## <a name="related-links"></a>Связанные ссылки

- [ToggleButton](https://developer.android.com/reference/android/widget/ToggleButton.html)
- [Коммутатор](https://developer.android.com/reference/android/widget/Switch.html)
