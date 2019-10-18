---
title: GridLayout
ms.prod: xamarin
ms.assetid: B69A4BF5-9CFB-443A-9F7B-062D1E498F61
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: bd05596ce8c6f8acb81b3ca68c6393a0be47768a
ms.sourcegitcommit: cb13fadbaa6d19dea94b9005bda20c2efd1b8039
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2019
ms.locfileid: "72541913"
---
# <a name="xamarinandroid-gridlayout"></a>Xamarin. Android GridLayout

@No__t_0 — это новый подкласс `ViewGroup`, который поддерживает размещение представлений в двухмерной сетке аналогично таблице HTML, как показано ниже:

 [![Cropped GridLayout, отображающий четыре ячейки](grid-layout-images/21-gridlayoutcropped.png)](grid-layout-images/21-gridlayoutcropped.png#lightbox)

 `GridLayout` работает с иерархией с плоским представлением, где дочерние представления задают свои расположения в сетке, указывая строки и столбцы, в которых они должны находиться. Таким образом, *GridLayout* может позиционировать представления в сетке, не требуя, чтобы в промежуточных представлениях была представлена табличная структура, например, в строках таблицы, используемых в таблелайаут. Благодаря поддержке плоской иерархии свойство *GridLayout* может более быстро разметки своих дочерних представлений. Давайте взглянем на пример, иллюстрирующий то, что эта концепция фактически означает в коде.

## <a name="creating-a-grid-layout"></a>Создание макета сетки

Следующий XML-код добавляет несколько элементов управления `TextView` в значение *GridLayout*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 2"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip" />
</GridLayout>
```

Макет изменит размеры строк и столбцов таким образом, чтобы ячейки могли вместить их содержимое, как показано на следующей схеме:

 [![Diagram макета, где две ячейки слева меньше, чем справа](grid-layout-images/gridlayout-cells.png)](grid-layout-images/gridlayout-cells.png#lightbox)

При запуске в приложении происходит следующий пользовательский интерфейс:

 [![Screenshot приложения Гридлайаутдемо, отображающего четыре ячейки](grid-layout-images/01-gridlayout.png)](grid-layout-images/01-gridlayout.png#lightbox)

## <a name="specifying-orientation"></a>Указание ориентации

Обратите внимание, что в приведенном выше коде XML в каждом `TextView` не указана строка или столбец. Если они не указаны, `GridLayout` назначает каждое дочернее представление по порядку на основе ориентации. Например, изменим ориентацию свойства GridLayout с горизонтального на вертикальную по вертикали следующим образом:

```xml
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2"
        android:orientation="vertical">
</GridLayout>
```

Теперь `GridLayout` будет располагать ячейки сверху вниз в каждом столбце, а не слева направо, как показано ниже:

 [![Diagram, иллюстрирующие расположение ячеек в вертикальной ориентации](grid-layout-images/gridlayoutorientation.png)](grid-layout-images/gridlayoutorientation.png#lightbox)

Это приводит к следующему пользовательскому интерфейсу во время выполнения:

 [![Screenshot Гридлайаутдемо с ячейками, расположенными в вертикальной ориентации](grid-layout-images/02-gridlayout.png)](grid-layout-images/02-gridlayout.png#lightbox)

### <a name="specifying-explicit-position"></a>Указание явной позицией

Если нужно явно управлять положением дочерних представлений в `GridLayout`, можно задать свои `layout_row` и `layout_column` атрибуты. Например, следующий код XML приведет к отображению макета на первом снимке экрана (показанном выше), независимо от ориентации.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="1" />
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="1"
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"
            android:layout_row="1"
            android:layout_column="1"  />
</GridLayout>
```

### <a name="specifying-spacing"></a>Указание интервала

Существует несколько вариантов, которые обеспечивают интервал между дочерними представлениями `GridLayout`. Можно использовать атрибут `layout_margin`, чтобы задать поля для каждого дочернего представления напрямую, как показано ниже.

```xml
<TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0"
            android:layout_margin="10dp" />
```

Кроме того, в Android 4 теперь доступно новое представление с разделением на общее назначение, именуемое `Space`. Чтобы использовать его, просто добавьте его в качестве дочернего представления.
Например, приведенный ниже XML добавляет к `GridLayout` дополнительную строку, присвоив ее `rowcount` значение 3, и добавляет представление `Space`, которое обеспечивает интервал между `TextViews`.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="3"
        android:columnCount="2"
        android:orientation="vertical">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"        
            android:layout_column="1" />
     <Space
            android:layout_row="1"
            android:layout_column="0"
            android:layout_width="50dp"         
            android:layout_height="50dp" />    
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="1" />
</GridLayout>
```

Этот XML-код создает пространство в `GridLayout`, как показано ниже:

 [![Screenshot Гридлайаутдемо, иллюстрирующих большие ячейки с отступами](grid-layout-images/03-gridlayout.png)](grid-layout-images/03-gridlayout.png#lightbox)

Преимуществом использования нового `Space` представления является то, что оно позволяет использовать интервалы и не требует от нас задавать атрибуты для каждого дочернего представления.

### <a name="spanning-columns-and-rows"></a>Объединение столбцов и строк

@No__t_0 также поддерживает ячейки, охватывающие несколько столбцов и строк. Например, предположим, что мы добавили в `GridLayout` другую строку, содержащую кнопку, как показано ниже:

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="4"
        android:columnCount="2"
        android:orientation="vertical">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"        
            android:layout_column="1" />
     <Space
            android:layout_row="1"
            android:layout_column="0"
            android:layout_width="50dp"        
            android:layout_height="50dp" />   
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"        
            android:layout_row="2"        
            android:layout_column="1" />
     <Button
            android:id="@+id/myButton"
            android:text="@string/hello"        
            android:layout_row="3"
            android:layout_column="0" />
</GridLayout>
```

Это приведет к тому, что первый столбец `GridLayout` растягивается в соответствии с размером кнопки, как показано здесь:

[![Screenshot Гридлайаутдемо с кнопкой, охватывающей только первый столбец](grid-layout-images/04-gridlayout.png)](grid-layout-images/04-gridlayout.png#lightbox)

Чтобы избежать растягивания первого столбца, можно установить кнопку, чтобы она занимала два столбца, задав его ColumnSpan следующим образом:

```xml
<Button
    android:id="@+id/myButton"
    android:text="@string/hello"       
    android:layout_row="3"
    android:layout_column="0"
    android:layout_columnSpan="2" />
```

Это приведет к разметке `TextViews`, похожей на макет, который мы ранее делали, с кнопкой, добавленной в нижнюю часть `GridLayout`, как показано ниже:

 [![Screenshot Гридлайаутдемо с кнопками, охватывающими оба столбца](grid-layout-images/05-gridlayout.png)](grid-layout-images/05-gridlayout.png#lightbox)

## <a name="related-links"></a>Связанные ссылки

- [Гридлайаутдемо (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/gridlayoutdemo)
- [Знакомство со Южным Сандвичевым](http://www.android.com/about/ice-cream-sandwich/)
- [Платформа Android 4,0](https://developer.android.com/sdk/android-4.0.html)
