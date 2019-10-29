---
title: Использование RelativeLayout в Xamarin. Android
description: Использование RelativeLayout в приложении Xamarin. Android
ms.prod: xamarin
ms.assetid: AFD9C849-02C3-E728-BC78-77A563612BC5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/29/2018
ms.openlocfilehash: 6cac771f46242cc0475be0a7ec0d475950f4b4e1
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028793"
---
# <a name="xamarinandroid-relativelayout"></a>Xamarin. Android RelativeLayout

[`RelativeLayout`](xref:Android.Widget.RelativeLayout) — это [`ViewGroup`](xref:Android.Views.ViewGroup) , отображающий дочерние [`View`](xref:Android.Views.View)
элементы в относительном положении. Положение [`View`](xref:Android.Views.View) может быть задано относительно родственных элементов (например, в левом или нижнем углу заданного элемента) или в позициях относительно [`RelativeLayout`](xref:Android.Widget.RelativeLayout)
область (например, выравнивание по нижнему краю, слева от центра).

[`RelativeLayout`](xref:Android.Widget.RelativeLayout) — это очень мощная служебная программа для разработки пользовательского интерфейса, так как она может исключить вложенные [`ViewGroup`](xref:Android.Views.ViewGroup)s. Если вы используете несколько вложенных [`LinearLayout`](xref:Android.Widget.LinearLayout)
группы, вы можете заменить их одним [`RelativeLayout`ом ](xref:Android.Widget.RelativeLayout).

Запустите новый проект с именем **хеллорелативелайаут**.

Откройте файл **Resources/Layout/Main. axml** и вставьте в него следующее:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:id="@+id/label"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Type here:"/>
    <EditText
        android:id="@+id/entry"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@android:drawable/editbox_background"
        android:layout_below="@id/label"/>
    <Button
        android:id="@+id/ok"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/entry"
        android:layout_alignParentRight="true"
        android:layout_marginLeft="10dip"
        android:text="OK" />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toLeftOf="@id/ok"
        android:layout_alignTop="@id/ok"
        android:text="Cancel" />
</RelativeLayout>
```

Обратите внимание на все `android:layout_*` атрибуты, такие как `layout_below`, `layout_alignParentRight`и `layout_toLeftOf`.
При использовании [`RelativeLayout`](xref:Android.Widget.RelativeLayout)эти атрибуты можно использовать для описания того, как нужно разместить каждую [`View`](xref:Android.Views.View). Каждый из этих атрибутов определяет другой тип относительного положения. Некоторые атрибуты используют идентификатор ресурса одноуровневого [`View`](xref:Android.Views.View) для определения его собственной относительной координаты. Например, последняя [`Button`](xref:Android.Widget.Button) определяется как левое и выравниваемая-with-Top-of [`View`](xref:Android.Views.View) , определяемого идентификатором `ok` (который является предыдущим [`Button`](xref:Android.Widget.Button)).

Все доступные атрибуты макета определяются в [`RelativeLayout.LayoutParams`](xref:Android.Widget.RelativeLayout.LayoutParams).

Убедитесь, что вы загрузили этот макет в [`OnCreate()`](xref:Android.App.Activity.OnCreate*)
Method

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

Метод [`SetContentView(int)`](xref:Android.App.Activity.SetContentView*) загружает файл макета для [`Activity`](xref:Android.App.Activity), определяемый идентификатором ресурса, &mdash; `Resource.Layout.Main` ссылается на файл макета **Resources/Layout/Main. axml** .

Запустите приложение. Вы должны увидеть следующий макет:

[![снимок экрана относительного макета с помощью TextView, EditText и двух кнопок](relative-layout-images/helloviews2.png)](relative-layout-images/helloviews2.png#lightbox)

## <a name="resources"></a>Ресурсы

- [`RelativeLayout`](xref:Android.Widget.RelativeLayout)
- [`RelativeLayout.LayoutParams`](xref:Android.Widget.RelativeLayout.LayoutParams)
- [`TextView`](xref:Android.Widget.TextView)
- [`EditText`](xref:Android.Widget.EditText)
- [`Button`](xref:Android.Widget.Button)

_Части этой страницы являются изменениями на основе работы, созданной и совместно используемой проектом Android с открытым кодом, и используются в соответствии с терминами, описанными в [лицензии Creative commons attribution 2,5](https://creativecommons.org/licenses/by/2.5/)._
