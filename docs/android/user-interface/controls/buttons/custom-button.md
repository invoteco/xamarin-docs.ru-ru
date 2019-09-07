---
title: Настраиваемая кнопка
ms.prod: xamarin
ms.assetid: C523D41E-5855-248D-079D-6B12B74B7617
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 4504045eb1692d95ee1e981bbec3da3a45699db3
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758930"
---
# <a name="custom-button"></a>Настраиваемая кнопка

В этом разделе вы создадите кнопку с пользовательским изображением, а не текстом, с помощью [`Button`](xref:Android.Widget.Button) мини-приложения и файла XML, определяющего три разных изображения, которые будут использоваться для различных состояний кнопок. При нажатии кнопки появляется короткое сообщение.

Щелкните правой кнопкой мыши и скачайте три приведенных ниже образа, а затем скопируйте их в каталог **ресурсов или нарисованный** файл проекта. Они будут использоваться для различных состояний кнопок.

 [ ![](custom-button-images/android-pressed.png)](custom-button-images/android-pressed.png#lightbox) [Зеленый значок Android для стандартного состояния оранжевый значок Android для значка состояния "желтый" ![](custom-button-images/android-normal.png)](custom-button-images/android-normal.png#lightbox) [ ![](custom-button-images/android-focused.png)](custom-button-images/android-focused.png#lightbox)

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

Это определяет один рисуемый ресурс, который изменит его изображение на основе текущего состояния кнопки. Первый `<item>` определяет **android_pressed. png** в качестве изображения при нажатии кнопки (она была активирована); во втором `<item>` определяется **android_focused. png** в качестве изображения, когда нажимается кнопка. выделено с помощью трекбола или направленной панели); а третья `<item>` определяет **android_normal. png** как изображение для нормального состояния (если ни одна из них не нажата или не имеет внимания). Этот XML-файл теперь представляет один рисуемый ресурс и при ссылке на [`Button`](xref:Android.Widget.Button) него на фоне отображаемое изображение изменится на основе этих трех состояний.

> [!NOTE]
> Порядок `<item>` элементов важен. При наличии ссылки на этот рисунок они проходят `<item>`по порядку, чтобы определить, какой из них подходит для текущего состояния кнопки.
> Так как "нормальный" образ является последним, он применяется только в том случае, `android:state_pressed` если `android:state_focused` условия и оба оценивают значение false.

Откройте файл **Resources/Layout/Main. axml** и добавьте [`Button`](xref:Android.Widget.Button) элемент:

```xml
<Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:background="@drawable/android_button" />
```

Атрибут задает рисуемый ресурс, используемый для фона кнопки (что при сохранении в ресурсах, а также `@drawable/android`при их **нарисовании или Android. XML**). `android:background` Это заменяет нормальное фоновое изображение, используемое для кнопок во всей системе. Чтобы изображение было изменено на основе состояния кнопки, оно должно быть применено к фону.

Чтобы кнопка была нажата, добавьте следующий код в конец[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
Method

```csharp
Button button = FindViewById<Button>(Resource.Id.button);

button.Click += (o, e) => {
    Toast.MakeText (this, "Beep Boop", ToastLength.Short).Show ();
};
```

Это захватывает объект [`Button`](xref:Android.Widget.Button) из макета, а затем [`Toast`](xref:Android.Widget.Toast) добавляет сообщение, отображаемое при [`Button`](xref:Android.Widget.Button) нажатии кнопки.

Теперь запустите приложение.

*Части этой страницы являются изменениями на основе работы, созданной и совместно используемой проектом Android с открытым кодом, и используются в соответствии с терминами,* 
описанными в[*лицензии Creative Commons Attribution 2,5*](http://creativecommons.org/licenses/by/2.5/).
