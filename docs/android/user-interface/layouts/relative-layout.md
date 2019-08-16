---
title: Использование RelativeLayout в Xamarin. Android
description: Использование RelativeLayout в приложении Xamarin. Android
ms.prod: xamarin
ms.assetid: AFD9C849-02C3-E728-BC78-77A563612BC5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/29/2018
ms.openlocfilehash: af74ae3c7c87f501bff519bcfa361264205ca3f1
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522377"
---
# <a name="xamarinandroid-relativelayout"></a>Xamarin. Android RelativeLayout

[`RelativeLayout`](xref:Android.Widget.RelativeLayout)[`ViewGroup`](xref:Android.Views.ViewGroup) , отображающий дочерний элемент[`View`](xref:Android.Views.View)
элементы в относительном положении. Положение [`View`](xref:Android.Views.View) объекта может быть задано относительно родственных элементов (например, в левом или нижнем углу заданного элемента) или в позициях, относящихся к элементу[`RelativeLayout`](xref:Android.Widget.RelativeLayout)
область (например, выравнивание по нижнему краю, слева от центра).

— Это очень мощная служебная программа для разработки пользовательского интерфейса, так как она [`ViewGroup`](xref:Android.Views.ViewGroup)может исключить вложенные элементы. [`RelativeLayout`](xref:Android.Widget.RelativeLayout) Если вы можете найти себя с помощью нескольких вложенных[`LinearLayout`](xref:Android.Widget.LinearLayout)
группы, возможно, вы сможете заменить их одним [`RelativeLayout`](xref:Android.Widget.RelativeLayout)из них.

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

Обратите внимание на `android:layout_*` каждый атрибут, `layout_below`например, `layout_alignParentRight`, и `layout_toLeftOf`.
При использовании [`RelativeLayout`](xref:Android.Widget.RelativeLayout)можно использовать эти атрибуты, чтобы описать, как их следует [`View`](xref:Android.Views.View)располагать. Каждый из этих атрибутов определяет другой тип относительного положения. Некоторые атрибуты используют идентификатор ресурса одноуровневого [`View`](xref:Android.Views.View) элемента для определения его собственной относительной координаты. Например, Последнее [`Button`](xref:Android.Widget.Button) определяется в соответствии с левым и равным-Top-of, [`View`](xref:Android.Views.View) определяемым идентификатором `ok` (который является предыдущим [`Button`](xref:Android.Widget.Button)).

Все доступные атрибуты макета определяются в [`RelativeLayout.LayoutParams`](xref:Android.Widget.RelativeLayout.LayoutParams).

Убедитесь, что вы загрузили этот макет в[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
Method

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[`Activity`](xref:Android.App.Activity) &mdash; `Resource.Layout.Main` Метод загружает файл макета для, заданный идентификатором ресурса, относится к файлу макета RESOURCES/Layout/Main. axml. [`SetContentView(int)`](xref:Android.App.Activity.SetContentView*)

Запустите приложение. Вы должны увидеть следующий макет:

[![Снимок экрана относительного макета с TextView, EditText и двумя кнопками](relative-layout-images/helloviews2.png)](relative-layout-images/helloviews2.png#lightbox)

## <a name="resources"></a>Ресурсы

- [`RelativeLayout`](xref:Android.Widget.RelativeLayout)
- [`RelativeLayout.LayoutParams`](xref:Android.Widget.RelativeLayout.LayoutParams)
- [`TextView`](xref:Android.Widget.TextView)
- [`EditText`](xref:Android.Widget.EditText)
- [`Button`](xref:Android.Widget.Button)

_Части этой страницы являются изменениями на основе работы, созданной и совместно используемой проектом Android с открытым кодом, и используются в соответствии с терминами, описанными в [лицензии Creative commons attribution 2,5](http://creativecommons.org/licenses/by/2.5/)._
