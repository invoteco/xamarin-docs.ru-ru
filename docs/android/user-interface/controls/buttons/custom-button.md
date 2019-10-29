---
title: Настраиваемая кнопка
ms.prod: xamarin
ms.assetid: C523D41E-5855-248D-079D-6B12B74B7617
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: d85c67cf18c61af04cf12bfab58a5b516d380f62
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029356"
---
# <a name="custom-button"></a>Настраиваемая кнопка

В этом разделе вы создадите кнопку с пользовательским изображением, а не текстом, используя мини-приложение [`Button`](xref:Android.Widget.Button) и файл XML, определяющий три разных изображения для различных состояний кнопок. При нажатии кнопки появляется короткое сообщение.

Щелкните правой кнопкой мыши и скачайте три приведенных ниже образа, а затем скопируйте их в каталог **ресурсов или нарисованный** файл проекта. Они будут использоваться для различных состояний кнопок.

 [![зеленый значок Android для нормального состояния](custom-button-images/android-normal.png)](custom-button-images/android-normal.png#lightbox) [![оранжевый значок Android для состояния "с состоянием](custom-button-images/android-focused.png)](custom-button-images/android-focused.png#lightbox) " [![желтый значок Android для состояния "нажато](custom-button-images/android-pressed.png)](custom-button-images/android-pressed.png#lightbox) "

Создайте новый файл в каталоге **Resources/Draw** с именем **android_button. XML**. Вставьте следующий XML-код:

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/android_pressed"
          android:state_pressed="true" />
    <item android:drawable="@drawable/android_focused"
          android:state_focused="true" />
    <item android:drawable="@drawable/android_normal" />
</selector>
```

Это определяет один рисуемый ресурс, который изменит его изображение на основе текущего состояния кнопки. Первый `<item>` определяет **android_pressed. png** в качестве изображения при нажатии кнопки (она была активирована); Вторая `<item>` определяет **android_focused. png** в качестве изображения при нажатии кнопки (когда кнопка выделена с помощью трекбола или направленной панели); а третья `<item>` определяет **android_normal. png** как изображение для нормального состояния (если ни одна из них не нажата или не имеет внимания). Этот XML-файл теперь представляет один отображаемый ресурс, и если на его фоне имеется ссылка [`Button`](xref:Android.Widget.Button) , отображаемое изображение будет изменяться на основе этих трех состояний.

> [!NOTE]
> Порядок элементов `<item>` важен. Если имеется ссылка на этот рисунок, `<item>`ы проходят по порядку, чтобы определить, какой из них подходит для текущего состояния кнопки.
> Так как "нормальный" образ является последним, он применяется только в том случае, если условия `android:state_pressed` и `android:state_focused` имеют оба вычисленных значения false.

Откройте файл **Resources/Layout/Main. axml** и добавьте элемент [`Button`](xref:Android.Widget.Button) :

```xml
<Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:background="@drawable/android_button" />
```

Атрибут `android:background` указывает рисуемый ресурс для фона кнопки (который при сохранении в **Resources/Draw. XML**) упоминается как `@drawable/android`). Это заменяет нормальное фоновое изображение, используемое для кнопок во всей системе. Чтобы изображение было изменено на основе состояния кнопки, оно должно быть применено к фону.

Чтобы кнопка была нажата, добавьте следующий код в конец [`OnCreate()`](xref:Android.App.Activity.OnCreate*)
Method

```csharp
Button button = FindViewById<Button>(Resource.Id.button);

button.Click += (o, e) => {
    Toast.MakeText (this, "Beep Boop", ToastLength.Short).Show ();
};
```

Это захватывает [`Button`](xref:Android.Widget.Button) из макета, а затем добавляет [`Toast`](xref:Android.Widget.Toast) сообщение, отображаемое при нажатии на [`Button`](xref:Android.Widget.Button) .

Теперь запустите приложение.

*Части этой страницы являются изменениями на основе работы, созданной и совместно используемой проектом Android с открытым кодом, и используются в соответствии с терминами, описанными в* [*лицензии
Creative Commons Attribution 2,5*](https://creativecommons.org/licenses/by/2.5/).
