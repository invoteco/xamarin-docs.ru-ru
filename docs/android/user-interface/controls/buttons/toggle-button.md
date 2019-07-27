---
title: ToggleButton
ms.prod: xamarin
ms.assetid: 9ADA8FCF-63ED-897A-DD56-D7D86535A92C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 91003f9a23c667b38028a9852b28dba656ba13db
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510332"
---
# <a name="togglebutton"></a>ToggleButton

В этом разделе вы создадите кнопку, которая используется специально для переключения между двумя состояниями с помощью [`ToggleButton`](xref:Android.Widget.ToggleButton) мини-приложения. Это мини-приложение является отличной альтернативой переключателям, если имеется два простых состояния, которые являются взаимоисключающими (например, "on" и "OFF"). Android 4,0 (API уровня 14) представляет собой альтернативу выключателю, известному как [`Switch`](xref:Android.Widget.Switch).

Пример выключателя **можно увидеть** в левой паре изображений, в то время как в правой части изображений представлен пример **переключателя**:

![Примеры переключателей и Тогглебуттонс в состояниях on и Off](toggle-button-images/togglebutton-switch.png)  

Элемент управления, используемый приложением, зависит от стиля. Оба мини-приложения функционально эквивалентны.

Откройте файл **Resources/Layout/Main. axml** и добавьте [`ToggleButton`](xref:Android.Widget.ToggleButton) элемент (внутри него [`LinearLayout`](xref:Android.Widget.LinearLayout)):

Чтобы сделать что-то при изменении состояния, добавьте следующий код в конец элемента[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
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

Это захватывает [`ToggleButton`](xref:Android.Widget.ToggleButton) элемент из макета и обрабатывает событие щелчка, которое определяет действие, выполняемое при нажатии кнопки. В этом примере метод проверяет новое состояние кнопки, а затем отображает [`Toast`](xref:Android.Widget.Toast) сообщение, указывающее текущее состояние.

Обратите внимание [`ToggleButton`](xref:Android.Widget.ToggleButton) , что обработчик обрабатывает собственное изменение состояния между установленным и непроверенным состоянием, поэтому вы просто запрашиваете его.

Запустите приложение.


> [!TIP]
> Если необходимо изменить состояние самостоятельно (например, при загрузке сохраненного [`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)), используйте[`Checked`](xref:Android.Widget.CompoundButton.Checked)
> метод задания свойства или[`Toggle()`](xref:Android.Widget.CompoundButton.Toggle)
> метод.


## <a name="related-links"></a>Связанные ссылки

- [ToggleButton](https://developer.android.com/reference/android/widget/ToggleButton.html)
- [Коммутатор](https://developer.android.com/reference/android/widget/Switch.html)
