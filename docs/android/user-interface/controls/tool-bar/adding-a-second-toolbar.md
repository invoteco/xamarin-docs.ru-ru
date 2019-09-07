---
title: Добавление второй панели инструментов
ms.prod: xamarin
ms.assetid: FCE0AD27-8B6B-47C6-AD19-2B1C12E1BBBF
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: 9fb5c696e830710e6ad99140477eedcbfe0e8823
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70764693"
---
# <a name="adding-a-second-toolbar"></a>Добавление второй панели инструментов

## <a name="overview"></a>Обзор 

Это может сделать больше, чем заменить панель &ndash; действий, которую можно использовать в рамках действия несколько раз. ее можно настроить для размещения в любом месте экрана, и ее можно настроить так, чтобы она занимала лишь частичную ширину экрана. `Toolbar` В приведенных ниже примерах показано, как создать `Toolbar` секунды и поместить ее в нижнюю часть экрана. Это `Toolbar` реализует элементы меню **Копировать**, **Вырезать**и **Вставить** . 

## <a name="define-the-second-toolbar"></a>Определение второй панели инструментов 

Измените файл макета **Main. axml** и замените его содержимое на следующий XML-код:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <include
        android:id="@+id/toolbar"
        layout="@layout/toolbar" />
    <LinearLayout
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:id="@+id/main_content"
        android:layout_below="@id/toolbar">
      <ImageView
          android:layout_width="fill_parent"
          android:layout_height="0dp"
          android:layout_weight="1" />
      <Toolbar
          android:id="@+id/edit_toolbar"
          android:minHeight="?android:attr/actionBarSize"
          android:background="?android:attr/colorAccent"
          android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"
          android:layout_width="match_parent"
          android:layout_height="wrap_content" />
    </LinearLayout>
</RelativeLayout>
```

Этот XML-код добавляет `Toolbar` секунды в нижнюю часть экрана с пустым `ImageView` заполнением середины экрана. Высота этого `Toolbar` параметра равна высоте панели действий: 

```xml
android:minHeight="?android:attr/actionBarSize"
```

Цвет фона для этого `Toolbar` элемента задается как контрастный цвет, который будет определен далее:

```xml
android:background="?android:attr/colorAccent
```

Обратите внимание `Toolbar` , что это основано на другой теме (**семеоверлай. Material. темно. актионбар**), чем, `Toolbar` используемой в созданном при [замене панели](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md) &ndash; действий, она не привязана к окну действия дéкор или к Тема, используемая в первой `Toolbar`.

Измените **Resources/Values/Styles. XML** и добавьте следующий контрастный цвет в определение стиля: 

```xml
<item name="android:colorAccent">#C7A935</item>
```

Это дает нижней панели инструментов темно-оранжевый цвет. При создании и запуске приложения в нижней части экрана отображается пустая Вторая панель инструментов: 

[![Снимок экрана приложения с желтой второй панелью инструментов в нижней части экрана](adding-a-second-toolbar-images/01-second-toolbar-sml.png)](adding-a-second-toolbar-images/01-second-toolbar.png#lightbox)

## <a name="add-edit-menu-items"></a>Добавление пунктов меню "Правка" 

В этом разделе объясняется, как добавить пункты меню Правка в `Toolbar`нижнюю часть страницы. 

Добавление пунктов меню во вторичную `Toolbar`: 

1. Добавьте значки меню в `mipmap-` папки проекта приложения (при необходимости).

2. Определите содержимое пунктов меню, добавив дополнительный файл ресурсов меню в **Resources/Menu**. 

3. В `OnCreate` методе действия `Toolbar` найдите (с помощью вызова `FindViewById`) и `Toolbar`раскрывающихся меню.

4. Реализуйте обработчик щелчка `OnCreate` в для новых пунктов меню. 

Этот процесс подробно описан в следующих разделах: Пункты меню **Вырезать**, **Копировать**и **Вставить** добавляются в нижнюю часть `Toolbar`. 

### <a name="define-the-edit-menu-resource"></a>Определение ресурса меню "Правка"

В подкаталоге **Resources/Menu** создайте новый XML-файл с именем **edit_menus. XML** и замените его содержимое следующим кодом XML:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item
       android:id="@+id/menu_cut"
       android:icon="@mipmap/ic_menu_cut_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Cut" />
  <item
       android:id="@+id/menu_copy"
       android:icon="@mipmap/ic_menu_copy_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Copy" />
  <item
       android:id="@+id/menu_paste"
       android:icon="@mipmap/ic_menu_paste_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Paste" />
</menu>
```

Этот XML-код создает пункты меню " **Вырезать**", " **Копировать**" и " **Вставить** " (с `mipmap-` помощью значков, добавленных в папки в области [замены панели действий](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md)).

### <a name="inflate-the-menus"></a>Раскрывающиеся меню

В конце `OnCreate` метода в **MainActivity.CS**добавьте следующие строки кода: 

```csharp
var editToolbar = FindViewById<Toolbar>(Resource.Id.edit_toolbar);
editToolbar.Title = "Editing";
editToolbar.InflateMenu (Resource.Menu.edit_menus);
editToolbar.MenuItemClick += (sender, e) => {
    Toast.MakeText(this, "Bottom toolbar tapped: " + e.Item.TitleFormatted, ToastLength.Short).Show();
};
```

`edit_toolbar` Этот код находит представление, определенное в **Main. axml**, задает его заголовок для **редактирования**и увеличивает его элементы меню (определенные в **edit_menus. XML**). Он определяет обработчик нажатия меню, который отображает всплывающее уведомление для указания того, какой значок редактирования был нажат. 

Выполните сборку и запуск приложения. При запуске приложения текст и значки, добавленные выше, будут выглядеть, как показано ниже: 

[![Диаграмма нижней панели инструментов с значками вырезания, копирования и вставки](adding-a-second-toolbar-images/02-bottom-toolbar-sml.png)](adding-a-second-toolbar-images/02-bottom-toolbar.png#lightbox)

Если коснуться значка меню **Вырезать** , отобразится следующее всплывающее уведомление: 

[![Снимок экрана всплывающего окна с сообщением о том, что был нажат значок меню "вырезать"](adding-a-second-toolbar-images/03-bottom-tapped-sml.png)](adding-a-second-toolbar-images/03-bottom-tapped.png#lightbox)

При нажатии пунктов меню на любой из панелей инструментов отображаются полученные всплывающие уведомления: 

[![Снимки экрана с всплывающими уведомлениями о нажатии кнопок «сохранить», «копировать» и «вставить»](adding-a-second-toolbar-images/04-menu-action-sml.png)](adding-a-second-toolbar-images/04-menu-action.png#lightbox)

## <a name="the-up-button"></a>Кнопка "вверх" 

Большинство приложений Android используют кнопку " **назад** " для навигации в приложении. При нажатии кнопки **назад** пользователь переходит на предыдущий экран.
Тем не менее может потребоваться создать кнопку **up** , которая позволяет пользователям легко переходить на основной экран приложения. Когда пользователь нажимает кнопку **up** , пользователь переходит на более высокий уровень иерархии &ndash; приложений, т. е. приложение извлекает пользователю несколько действий в стеке назад, а не вернется к ранее просмотренному действию. 

Чтобы включить кнопку **up** во втором действии, которое использует в `Toolbar` качестве `SetDisplayHomeAsUpEnabled` панели действий, вызовите методы `OnCreate` и `SetHomeButtonEnabled` во втором методе действия:

```csharp
SetActionBar (toolbar);
...
ActionBar.SetDisplayHomeAsUpEnabled (true);
ActionBar.SetHomeButtonEnabled (true);
```

В примере кода [панели инструментов поддержки версии 7](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar) показано использование кнопки **вверх** в действии. Этот пример (который использует библиотеку совместимости, описанный далее) реализует второе действие, которое использует кнопку " **вверх** " для иерархического перехода к предыдущему действию. В этом примере `DetailActivity` кнопка "Главная" активирует кнопку " **вверх** ", выполнив `SupportActionBar` следующие вызовы метода: 

```csharp
SetSupportActionBar (toolbar);
...
SupportActionBar.SetDisplayHomeAsUpEnabled (true);
SupportActionBar.SetHomeButtonEnabled (true);
```

`MainActivity` При переходе пользователя с на `DetailActivity` `DetailActivity` отображается кнопка **вверх** (стрелка влево), как показано на снимке экрана:

[![Снимок экрана: пример стрелки влево кнопки вверх на панели инструментов](adding-a-second-toolbar-images/05-up-button-sml.png)](adding-a-second-toolbar-images/05-up-button.png#lightbox)

**Нажатие этой кнопки** приводит к тому, что приложение возвращается `MainActivity`к. В более сложном приложении с несколькими уровнями иерархии при нажатии этой кнопки пользователь вернется к следующему высшему уровню приложения, а не к предыдущему экрану. 

## <a name="related-links"></a>Связанные ссылки

- [Панель инструментов без описания операций (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-toolbar)
- [Панель инструментов AppCompat (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
