---
title: Xamarin. Android элемент LinearLayout
ms.prod: xamarin
ms.assetid: B49D129C-AF24-3C5A-C833-5A34AFBB2442
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/07/2018
ms.openlocfilehash: b1cff01c66ae2581a68286e62bd8c8c5fb7f9d72
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028956"
---
# <a name="xamarinandroid-linearlayout"></a>Xamarin. Android элемент LinearLayout

[`LinearLayout`](xref:Android.Widget.LinearLayout) является [`ViewGroup`](xref:Android.Views.ViewGroup)
, отображающий дочерние [`View`](xref:Android.Views.View)
элементы в линейном направлении: по вертикали или по горизонтали.

Следует соблюдать осторожность при чрезмерном использовании [`LinearLayout`](xref:Android.Widget.LinearLayout).
Если вы приступите к вложению нескольких [`LinearLayout`](xref:Android.Widget.LinearLayout), попробуйте использовать [`RelativeLayout`](xref:Android.Widget.RelativeLayout)
Используйте.

Запустите новый проект с именем **хеллолинеарлайаут**.

Откройте **ресурсы/макет/Main. axml** и вставьте следующее:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation=    "vertical"
    android:layout_width=    "match_parent"
    android:layout_height=    "match_parent"    >

  <LinearLayout
      android:orientation=    "horizontal"
      android:layout_width=    "match_parent"
      android:layout_height=    "match_parent"
      android:layout_weight=    "1"    >
      <TextView
          android:text=    "red"
          android:gravity=    "center_horizontal"
          android:background=    "#aa0000"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "green"
          android:gravity=    "center_horizontal"
          android:background=    "#00aa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "blue"
          android:gravity=    "center_horizontal"
          android:background=    "#0000aa"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "yellow"
          android:gravity=    "center_horizontal"
          android:background=    "#aaaa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
  </LinearLayout>
        
  <LinearLayout
    android:orientation=    "vertical"
    android:layout_width=    "match_parent"
    android:layout_height=    "match_parent"
    android:layout_weight=    "1"    >
    <TextView
        android:text=    "row one"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row two"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row three"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row four"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
       android:layout_weight=    "1"    />
  </LinearLayout>

</LinearLayout>
```

Внимательно изучите этот XML-код. Существует корневой [`LinearLayout`](xref:Android.Widget.LinearLayout)
, определяющий ориентацию по вертикали &ndash; все дочерние [`View`](xref:Android.Views.View)s (из которых он имеет два) будут вертикально стеками. Первый дочерний элемент является еще одним [`LinearLayout`](xref:Android.Widget.LinearLayout)
При этом используется горизонтальная ориентация, а второй дочерний — [`LinearLayout`](xref:Android.Widget.LinearLayout)
для этого используется вертикальная ориентация. Каждый из этих вложенных [`LinearLayout`](xref:Android.Widget.LinearLayout)s содержит несколько [`TextView`](xref:Android.Widget.TextView)
элементы, которые ориентированы друг на друга в соответствии со своими родительскими [`LinearLayout`](xref:Android.Widget.LinearLayout).

Теперь откройте **HelloLinearLayout.CS** и убедитесь, что он загружает макет **ресурсов/макета/основной. axml** в [`OnCreate()`](xref:Android.App.Activity.OnCreate*)
Method

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

Метод [`SetContentView(int)`](xref:Android.App.Activity.SetContentView*)) загружает файл макета для [`Activity`](xref:Android.App.Activity), определяемый идентификатором ресурса, &ndash; `Resources.Layout.Main` ссылается на файл макета **Resources/Layout/Main. axml** .

Запустите приложение. Вы должны увидеть следующее:

[Снимок экрана первого элемент LinearLayout приложения по горизонтали, второй по вертикали![](linear-layout-images/helloviews1.png)](linear-layout-images/helloviews1.png#lightbox)

Обратите внимание, что атрибуты XML определяют поведение каждого представления. Попробуйте поэкспериментировать с различными значениями для `android:layout_weight`, чтобы увидеть, как будет распространяться фактическое пространство на экране в зависимости от веса каждого элемента. Дополнительные сведения о том, как [`LinearLayout`](xref:Android.Widget.LinearLayout) , см. в документе [Общие объекты макета](https://developer.android.com/guide/topics/ui/declaring-layout.html) .
обрабатывает атрибут `android:layout_weight`.

## <a name="references"></a>Ссылки

- [`LinearLayout`](xref:Android.Widget.LinearLayout)
- [`TextView`](xref:Android.Widget.TextView)

_Части этой страницы являются изменениями на основе работы, созданной и совместно используемой проектом Android с открытым кодом, и используются в соответствии с терминами, описанными в [лицензии Creative commons attribution 2,5](https://creativecommons.org/licenses/by/2.5/)._
