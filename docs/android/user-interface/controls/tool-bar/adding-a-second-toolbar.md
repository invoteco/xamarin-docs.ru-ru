---
title: Добавление второй панели инструментов
ms.prod: xamarin
ms.assetid: FCE0AD27-8B6B-47C6-AD19-2B1C12E1BBBF
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: 43030d4a221ceadf68c30df2e37449e7f996fa6d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029108"
---
# <a name="adding-a-second-toolbar"></a>Добавление второй панели инструментов

## <a name="overview"></a>Обзор 

`Toolbar` может выполнять больше, чем заменить панель действий &ndash; можно использовать несколько раз в рамках действия, она может быть настроена для размещения в любом месте экрана, и ее можно настроить на охват только частичной ширины экрана. В приведенных ниже примерах показано, как создать второй `Toolbar` и поместить его в нижнюю часть экрана. Этот `Toolbar` реализует элементы меню " **Копировать**", " **Вырезать**" и " **Вставить** ". 

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

Этот XML-код добавляет второй `Toolbar` в нижнюю часть экрана с пустой `ImageView` заполняя середину экрана. Высота этой `Toolbar` задается высотой панели действий: 

```xml
android:minHeight="?android:attr/actionBarSize"
```

Цвет фона этого `Toolbar` установлен на контрастный цвет, который будет определен далее:

```xml
android:background="?android:attr/colorAccent
```

Обратите внимание, что этот `Toolbar` основан на другой теме (**семеоверлай. Material. темно. актионбар**), которая используется `Toolbar`, созданной при [замене панели действий](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md) &ndash; она не привязана к окну действия дéкор или к теме, используемой в Первый `Toolbar`.

Измените **Resources/Values/Styles. XML** и добавьте следующий контрастный цвет в определение стиля: 

```xml
<item name="android:colorAccent">#C7A935</item>
```

Это дает нижней панели инструментов темно-оранжевый цвет. При создании и запуске приложения в нижней части экрана отображается пустая Вторая панель инструментов: 

[![снимок экрана приложения с желтой второй панелью инструментов в нижней части экрана](adding-a-second-toolbar-images/01-second-toolbar-sml.png)](adding-a-second-toolbar-images/01-second-toolbar.png#lightbox)

## <a name="add-edit-menu-items"></a>Добавление пунктов меню "Правка" 

В этом разделе объясняется, как добавить элементы меню "Правка" в нижнюю `Toolbar`. 

Добавление пунктов меню во вторичную `Toolbar`: 

1. Добавьте значки меню в папки `mipmap-` проекта приложения (при необходимости).

2. Определите содержимое пунктов меню, добавив дополнительный файл ресурсов меню в **Resources/Menu**. 

3. В методе `OnCreate` действия найдите `Toolbar` (путем вызова `FindViewById`) и раскрывающихся меню `Toolbar`.

4. Реализуйте обработчик щелчка в `OnCreate` для новых пунктов меню. 

В следующих разделах подробно описывается этот процесс: **вырезание**, **копирование**и **Вставка** пунктов меню добавляются в нижнюю `Toolbar`. 

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

Этот XML-код создает пункты меню " **Вырезать**", " **Копировать**" и " **Вставить** " (с помощью значков, добавленных в `mipmap-` папки при [замене панели действий](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md)).

### <a name="inflate-the-menus"></a>Раскрывающиеся меню

В конце метода `OnCreate` в **MainActivity.CS**добавьте следующие строки кода: 

```csharp
var editToolbar = FindViewById<Toolbar>(Resource.Id.edit_toolbar);
editToolbar.Title = "Editing";
editToolbar.InflateMenu (Resource.Menu.edit_menus);
editToolbar.MenuItemClick += (sender, e) => {
    Toast.MakeText(this, "Bottom toolbar tapped: " + e.Item.TitleFormatted, ToastLength.Short).Show();
};
```

Этот код находит представление `edit_toolbar`, определенное в **Main. axml**, задает его заголовок для **редактирования**и увеличивает его элементы меню (определенные в **edit_menus. XML**). Он определяет обработчик нажатия меню, который отображает всплывающее уведомление для указания того, какой значок редактирования был нажат. 

Выполните сборку и запуск приложения. При запуске приложения текст и значки, добавленные выше, будут выглядеть, как показано ниже: 

[![ная диаграмма нижней панели инструментов с значками вырезания, копирования и вставки](adding-a-second-toolbar-images/02-bottom-toolbar-sml.png)](adding-a-second-toolbar-images/02-bottom-toolbar.png#lightbox)

Если коснуться значка меню **Вырезать** , отобразится следующее всплывающее уведомление: 

[![снимок экрана с уведомлением о том, что был нажат значок меню "вырезать"](adding-a-second-toolbar-images/03-bottom-tapped-sml.png)](adding-a-second-toolbar-images/03-bottom-tapped.png#lightbox)

При нажатии пунктов меню на любой из панелей инструментов отображаются полученные всплывающие уведомления: 

[![снимки экрана с всплывающими уведомлениями о нажатии кнопки «Сохранить», «копировать» и «вставить»](adding-a-second-toolbar-images/04-menu-action-sml.png)](adding-a-second-toolbar-images/04-menu-action.png#lightbox)

## <a name="the-up-button"></a>Кнопка "вверх" 

Большинство приложений Android используют кнопку " **назад** " для навигации в приложении. При нажатии кнопки **назад** пользователь переходит на предыдущий экран.
Тем не менее может потребоваться создать кнопку **up** , которая позволяет пользователям легко переходить на основной экран приложения. Когда пользователь нажимает кнопку **Up (вверх** ), пользователь переходит на более высокий уровень иерархии приложений &ndash; то есть, приложение извлекает пользователю несколько действий в стеке назад, а не вернется к ранее просмотренному действию. 

Чтобы включить кнопку **up** во втором действии, которое использует `Toolbar` в качестве панели действий, вызовите методы `SetDisplayHomeAsUpEnabled` и `SetHomeButtonEnabled` во втором методе `OnCreate`.

```csharp
SetActionBar (toolbar);
...
ActionBar.SetDisplayHomeAsUpEnabled (true);
ActionBar.SetHomeButtonEnabled (true);
```

В примере кода [панели инструментов поддержки версии 7](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar) показано использование кнопки **вверх** в действии. Этот пример (который использует библиотеку совместимости, описанный далее) реализует второе действие, которое использует кнопку " **вверх** " для иерархического перехода к предыдущему действию. В этом примере кнопка `DetailActivity` Главная активирует кнопку **up** , делая следующие вызовы методов `SupportActionBar`: 

```csharp
SetSupportActionBar (toolbar);
...
SupportActionBar.SetDisplayHomeAsUpEnabled (true);
SupportActionBar.SetHomeButtonEnabled (true);
```

Когда пользователь переходит с `MainActivity` на `DetailActivity`, в `DetailActivity` отображается кнопка **вверх** (стрелка влево), как показано на снимке экрана:

[![снимок экрана: пример стрелки влево кнопки вверх на панели инструментов](adding-a-second-toolbar-images/05-up-button-sml.png)](adding-a-second-toolbar-images/05-up-button.png#lightbox)

**Нажатие этой кнопки** приводит к тому, что приложение возвращается в `MainActivity`. В более сложном приложении с несколькими уровнями иерархии при нажатии этой кнопки пользователь вернется к следующему высшему уровню приложения, а не к предыдущему экрану. 

## <a name="related-links"></a>Связанные ссылки

- [Панель инструментов без описания операций (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-toolbar)
- [Панель инструментов AppCompat (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
