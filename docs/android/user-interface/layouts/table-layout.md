---
title: Xamarin. Android Таблелайаут
ms.prod: xamarin
ms.assetid: 0C7B9C95-5E5F-A069-BA37-984E49F7DCAD
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 4175b1fa62b2bc0e4209d13934c2bdbdd1e2a085
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028748"
---
# <a name="xamarinandroid-tablelayout"></a>Xamarin. Android Таблелайаут

[`TableLayout`](xref:Android.Widget.TableLayout) является [`ViewGroup`](xref:Android.Views.ViewGroup)
, отображающий дочерние [`View`](xref:Android.Views.View)
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

Обратите внимание, что это напоминает структуру HTML-таблицы. [`TableLayout`](xref:Android.Widget.TableLayout)
элемент подобен HTML-элементу `<table>`; [`TableRow`](xref:Android.Widget.TableRow)
объект подобен элементу `<tr>`; но для ячеек можно использовать любой тип элемента [`View`](xref:Android.Views.View) . В этом примере это [`TextView`](xref:Android.Widget.TextView)
используется для каждой ячейки. В некоторых строках также имеется базовый [`View`](xref:Android.Views.View), который используется для отрисовки горизонтальной линии.

Убедитесь, что действие **хеллотаблелайаут** загружает этот макет в [`OnCreate()`](xref:Android.App.Activity.OnCreate*)
Method

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

Метод [`SetContentView(int)`](xref:Android.App.Activity.SetContentView*)) загружает файл макета для [`Activity`](xref:Android.App.Activity), определяемый идентификатором ресурса, &mdash; `Resource.Layout.Main` ссылается на файл макета **Resources/Layout/Main. axml** .

Запустите приложение. Вы должны увидеть следующее:

[снимок экрана![пример приложения Таблелайаут, отображающего несколько строк таблицы](table-layout-images/helloviews3.png)](table-layout-images/helloviews3.png#lightbox)

## <a name="references"></a>Ссылки

- [`TableLayout`](xref:Android.Widget.TableLayout)
- [`TableRow`](xref:Android.Widget.TableRow)
- [`TextView`](xref:Android.Widget.TextView)

_Части этой страницы являются изменениями на основе работы, созданной и совместно используемой проектом Android с открытым кодом, и используются в соответствии с терминами, описанными в [лицензии Creative commons attribution 2,5](https://creativecommons.org/licenses/by/2.5/)._
