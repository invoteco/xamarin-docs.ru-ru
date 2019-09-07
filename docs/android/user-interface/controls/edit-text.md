---
title: Изменить текст
description: Использование мини-приложения EditText для приема данных, вводимых пользователем.
ms.prod: xamarin
ms.assetid: E513BCBC-438E-15E8-B83A-4B768A8E8B32
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/09/2018
ms.openlocfilehash: e8ffe337e1f5c74bc348b9600a466f1232f40b0b
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758831"
---
# <a name="xamarinandroid-edit-text"></a>Изменение текста Xamarin. Android

В этом разделе вы создадите текстовое поле для ввода данных пользователем с помощью мини-приложения [EditText](xref:Android.Widget.EditText) . После ввода текста в поле Клавиша **Ввод** выводит текст всплывающего сообщения.

Откройте **Resources/Layout/activity_main. axml** и добавьте элемент [EditText](xref:Android.Widget.EditText) в содержащий его макет. В следующем примере **activity_main. axml** имеет объект `EditText` , который был добавлен в: `LinearLayout`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <EditText
        android:id="@+id/edittext"
        android:layout_width="match_parent"
        android:imeOptions="actionGo"
        android:inputType="text"
        android:layout_height="wrap_content" />
</LinearLayout>
```

В этом примере `EditText` кода атрибуту `android:imeOptions` присваивается значение `actionGo`. Этот параметр изменяет [действие по умолчанию на](https://developer.android.com/reference/android/view/inputmethod/EditorInfo#IME_ACTION_DONE) действие [Go](https://developer.android.com/reference/android/view/inputmethod/EditorInfo#IME_ACTION_GO) , так что нажатие клавиши `KeyPress` ввод активирует обработчик ввода.
(Обычно используется `actionGo` , чтобы клавиша **Ввод** перебирала пользователя в целевой объект URL-адреса, введенного в.)

Чтобы управлять вводом текста пользователя, добавьте следующий код в конец метода [OnCreate](xref:Android.App.Activity.OnCreate*) в **MainActivity.CS**:

```csharp
EditText edittext = FindViewById<EditText>(Resource.Id.edittext);
edittext.KeyPress += (object sender, View.KeyEventArgs e) => {
    e.Handled = false;
    if (e.Event.Action == KeyEventActions.Down && e.KeyCode == Keycode.Enter) 
    {
        Toast.MakeText(this, edittext.Text, ToastLength.Short).Show();
        e.Handled = true;
    }
};
```

Кроме того, добавьте следующую `using` инструкцию в верхнюю часть **MainActivity.CS** , если она еще не существует:

```csharp
using Android.Views;
```

В этом примере кода реализуется элемент [EditText](xref:Android.Widget.EditText) из макета и добавляется обработчик [нажатия клавиши](xref:Android.Views.View.KeyPress) , определяющий действие, которое должно быть выполнено при нажатии клавиши, когда мини-приложение находится в фокусе. В этом случае метод определяется для прослушивания клавиши **Ввод** (при касании) и [появления](xref:Android.Widget.Toast) всплывающего сообщения с введенным текстом. Обратите внимание, что обрабатываемое свойство [Handled](xref:Android.Views.View.KeyEventArgs.Handled) всегда должно быть `true`, если событие было обработано. Это необходимо, чтобы предотвратить восходящую маршрутизацию события (что привело бы к возврату каретки в текстовое поле).

Запустите приложение и введите в текстовое поле некоторый текст. При нажатии клавиши **Ввод** всплывающее уведомление будет отображаться, как показано справа:

[![Примеры ввода текста в EditText](edit-text-images/edit-text-sml.png)](edit-text-images/edit-text.png#lightbox)

*Части этой страницы являются изменениями на основе созданных работ и* [*совместно используется проектом Android с открытым исходным кодом*](http://code.google.com/policies.html) *и используются в соответствии с терминами, описанными в разделе* [*Лицензия на соотнесение Creative commons attribution 2,5*](http://creativecommons.org/licenses/by/2.5/) *. Этот учебник основан на* [*руководстве по формам Android*](https://developer.android.com/resources/tutorials/views/hello-formstuff.html) *.*

## <a name="related-links"></a>Связанные ссылки

- [едиттекстсампле](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-edittextsample)
