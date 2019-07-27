---
title: Xamarin. Android Таблелайаут
ms.prod: xamarin
ms.assetid: 0C7B9C95-5E5F-A069-BA37-984E49F7DCAD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 9a5186fe49edc81f5d6ec8ca297146a27a0274fa
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68509683"
---
# <a name="xamarinandroid-tablelayout"></a>Xamarin. Android Таблелайаут

[`TableLayout`](xref:Android.Widget.TableLayout)является[`ViewGroup`](xref:Android.Views.ViewGroup)
, отображающий дочерний элемент[`View`](xref:Android.Views.View)
элементы в строках и столбцах.

Запустите новый проект с именем **хеллотаблелайаут**.

Откройте файл **Resources/Layout/Main. axml** и вставьте в него следующее:

```xml
<?xml version="1.0" encoding="utf-8"?>
<TableLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:stretchColumns="1">

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Open..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-O"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Save..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-S"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Save As..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-Shift-S"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <View
        android:layout_height="2dip"
        android:background="#FF909090"/>

    <TableRow>
        <TextView
            android:text="X"
            android:padding="3dip"/>
        <TextView
            android:text="Import..."
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:text="X"
            android:padding="3dip"/>
        <TextView
            android:text="Export..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-E"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <View
        android:layout_height="2dip"
        android:background="#FF909090"/>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Quit"
            android:padding="3dip"/>
    </TableRow>
</TableLayout>
```

Обратите внимание, что это напоминает структуру HTML-таблицы. Тот[`TableLayout`](xref:Android.Widget.TableLayout)
элемент подобен HTML- `<table>` элементу;[`TableRow`](xref:Android.Widget.TableRow)
параметр подобен `<tr>` элементу, но для ячеек можно использовать любой [`View`](xref:Android.Views.View) тип элемента. В этом примере[`TextView`](xref:Android.Widget.TextView)
используется для каждой ячейки. В некоторых строках также имеется базовый [`View`](xref:Android.Views.View)элемент, который используется для рисования горизонтальной линии.

Убедитесь, что действие **хеллотаблелайаут** загружает этот макет в[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
Method

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[`Activity`](xref:Android.App.Activity) &mdash; `Resource.Layout.Main`  Метод) загружает файл макета для, заданный идентификатором ресурса, относится к файлу макета RESOURCES/Layout/Main. axml. [`SetContentView(int)`](xref:Android.App.Activity.SetContentView*)

Запустите приложение. Вы должны увидеть следующее:

[![Пример снимка экрана приложения Таблелайаут, отображающего несколько строк таблицы](table-layout-images/helloviews3.png)](table-layout-images/helloviews3.png#lightbox)



## <a name="references"></a>Ссылки

-   [`TableLayout`](xref:Android.Widget.TableLayout) 

-   [`TableRow`](xref:Android.Widget.TableRow) 

-   [`TextView`](xref:Android.Widget.TextView) 

*Части этой страницы являются изменениями на основе работы, созданной и совместно используемой проектом Android с открытым кодом, и используются в соответствии с терминами,* 
описанными в[*лицензии Creative Commons Attribution 2,5*](http://creativecommons.org/licenses/by/2.5/).
