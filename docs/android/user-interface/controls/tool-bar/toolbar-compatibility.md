---
title: Совместимость панели инструментов
ms.prod: xamarin
ms.assetid: A0798CA1-2C7D-43B6-9E91-4435CC7B6683
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: f8dcda63c97c15367157b6a6cb857c0bfef79a27
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70764588"
---
# <a name="toolbar-compatibility"></a>Совместимость панели инструментов

## <a name="overview"></a>Обзор

В этом разделе объясняется, `Toolbar` как использовать в версиях Android более ранние, чем Android 5,0 без описания операций. Если приложение не поддерживает версии Android, предшествующие Android 5,0, этот раздел можно пропустить. 

Так `Toolbar` как является частью библиотеки поддержки Android версии 7, ее можно использовать на устройствах под управлением Android 2,1 (API уровня 7) и более поздних версий. Однако [Библиотека поддержки Android версии 7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet должна быть установлена, а код изменен так, чтобы использовать реализацию, `Toolbar` предоставленную в этой библиотеке. В этом разделе объясняется, как установить этот NuGet и изменить приложение **тулбарфун** , [добавив вторую панель инструментов](~/android/user-interface/controls/tool-bar/adding-a-second-toolbar.md) , чтобы она выполнялась в версиях Android раньше, чем с параметром без описания 5,0.

Чтобы изменить приложение для использования версии на панели инструментов AppCompat, выполните следующие действия. 

1. Задайте минимальные и целевые версии Android для приложения.

2. Установите пакет NuGet для AppCompat.

3. Используйте тему AppCompat вместо встроенной темы Android.

4. Измените `MainActivity` таким образом, чтобы он `AppCompatActivity` подклассы `Activity`, а не. 

Каждый из этих шагов подробно описан в следующих разделах.

## <a name="set-the-minimum-and-target-android-version"></a>Установка минимальной и целевой версий Android

Целевая платформа приложения должна быть установлена на уровне API 21 или выше, иначе приложение не будет развернуто должным образом. Если при развертывании приложения обнаружена ошибка, например, **идентификатор ресурса не найден для атрибута "тилемодекс" в пакете "Android"** , это связано с тем, что для целевой платформы не задано значение **Android 5,0 (API уровня 21-интерфейса)** или больше. 

Задайте для целевого уровня платформы уровень API 21 или выше и задайте для параметров проекта уровня API Android минимальную версию Android, которую будет поддерживать приложение. Дополнительные сведения о настройке уровней API Android см. в разделе Общие сведения об [уровнях API Android](~/android/app-fundamentals/android-api-levels.md). В этом `ToolbarFun` примере для минимальной версии Android задано значение KitKat (API уровня 4,4). 

## <a name="install-the-appcompat-nuget-package"></a>Установка пакета AppCompat NuGet

Затем добавьте в проект пакет [AppCompat версии 7 для библиотеки поддержки Android](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) . В Visual Studio щелкните правой кнопкой мыши **ссылки** и выберите пункт **Управление пакетами NuGet...** . Нажмите кнопку **Обзор** и найдите **библиотеку поддержки Android версии 7 AppCompat**. Выберите **Xamarin. Android. support. версии 7. AppCompat** и нажмите кнопку **установить**. 

[![Снимок экрана пакета версии 7 AppCompat, выбранного в окне "Управление пакетами NuGet"](toolbar-compatibility-images/01-appcompat-nuget-sml.png)](toolbar-compatibility-images/01-appcompat-nuget.png#lightbox)

При установке этого NuGet также устанавливаются несколько других пакетов NuGet (например, **Xamarin. Android. support. анимированный. Vector.** , которые можно нарисовать, **Xamarin. Android. support. v4**и  **Xamarin. Android. support. Vector. Draw**). Дополнительные сведения об установке пакетов NuGet см. в [разделе Пошаговое руководство. Включение NuGet в проект](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough). 

## <a name="use-an-appcompat-theme-and-toolbar"></a>Использование темы и панели инструментов AppCompat

Библиотека AppCompat поставляется с несколькими `Theme.AppCompat` темами, которые можно использовать в любой версии Android, поддерживаемой библиотекой AppCompat. Тема примера приложения является производной от `Theme.Material.Light.DarkActionBar`, которая недоступна в версиях Android, предшествующих интерфейсу без описания операций. `ToolbarFun` Поэтому необходимо адаптироваться к использованию аналога AppCompat для этой `Theme.AppCompat.Light.DarkActionBar`темы. `ToolbarFun` Кроме того, `Toolbar` поскольку недоступно в версиях Android, предшествующих интерфейсу без описания операций, необходимо использовать `Toolbar`версию AppCompat. Поэтому макеты должны использовать `android.support.v7.widget.Toolbar` `Toolbar`вместо. 

### <a name="update-layouts"></a>Обновление макетов

Измените **Resources/Layout/Main. axml** и `Toolbar` замените элемент следующим XML-кодом: 

```xml
<android.support.v7.widget.Toolbar
    android:id="@+id/edit_toolbar"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorAccent"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

Измените **Resources/Layout/Toolbar. XML** и замените его содержимое следующим XML-кодом: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v7.widget.Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"/>
```

Обратите внимание `?attr` , что значения больше не начинаются с `android:` префикса (Помните `?` , что запись ссылается на ресурс в текущей теме). Если `?android:attr` все еще использовались здесь, Android будет ссылаться на значение атрибута из текущей платформы, а не из библиотеки AppCompat. Поскольку в этом примере используется `actionBarSize` объект, определенный библиотекой AppCompat `android:` , префикс удаляется. `android:theme` `ThemeOverlay.Material.Dark.ActionBar` &ndash; `ThemeOverlay.AppCompat.Dark.ActionBar` Аналогичным образом изменяется на `@style` , чтобы атрибут был задан в теме в библиотеке AppCompat, а не в. `@android:style` 

### <a name="update-the-style"></a>Обновление стиля

Измените **Resources/Values/Styles. XML** и замените его содержимое следующим XML-кодом: 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<resources>
  <style name="MyTheme" parent="MyTheme.Base"> </style>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light.DarkActionBar">
    <item name="windowNoTitle">true</item>
    <item name="windowActionBar">false</item>
    <item name="colorPrimary">#5A8622</item>
    <item name="colorAccent">#A88F2D</item>
  </style>
</resources>
```

Имена элементов и родительские темы в этом примере больше не являются префиксами `android:` , так как мы используем библиотеку AppCompat. Кроме того, Родительская тема изменяется на версию `Light.DarkActionBar`AppCompat. 

### <a name="update-menus"></a>Обновить меню

Для поддержки более ранних версий Android библиотека AppCompat использует настраиваемые атрибуты, отражающие атрибуты `android:` пространства имен. Однако некоторые `showAsAction` атрибуты (например, атрибут, используемый `<menu>` в теге) не существуют в платформе Android на старых устройствах &ndash; `showAsAction` были введены в интерфейсе API Android 11, но недоступны в API Android 7. По этой причине для префикса всех атрибутов, определенных библиотекой поддержки, необходимо использовать пользовательское пространство имен. В файлах ресурсов меню для `local` `showAsAction` префикса атрибута определено пространство имен с именем. 

Измените **Resources/Menu/top_menus. XML** и замените его содержимое следующим XML-кодом:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:local="http://schemas.android.com/apk/res-auto">
  <item
       android:id="@+id/menu_edit"
       android:icon="@mipmap/ic_action_content_create"
       local:showAsAction="ifRoom"
       android:title="Edit" />
  <item
       android:id="@+id/menu_save"
       android:icon="@mipmap/ic_action_content_save"
       local:showAsAction="ifRoom"
       android:title="Save" />
  <item
       android:id="@+id/menu_preferences"
       local:showAsAction="never"
       android:title="Preferences" />
</menu>
```

`local` Пространство имен добавляется с этой строкой:

```xml
xmlns:local="http://schemas.android.com/apk/res-auto">
```

Этот атрибут предшествует этому `local:` пространству имен, а не `showAsAction``android:` 

```csharp
local:showAsAction="ifRoom"
```

Аналогичным образом измените **Resources/Menu/edit_menus. XML** и замените его содержимое следующим XML-кодом:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:local="http://schemas.android.com/apk/res-auto">
  <item
       android:id="@+id/menu_cut"
       android:icon="@mipmap/ic_menu_cut_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Cut" />
  <item
       android:id="@+id/menu_copy"
       android:icon="@mipmap/ic_menu_copy_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Copy" />
  <item
       android:id="@+id/menu_paste"
       android:icon="@mipmap/ic_menu_paste_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Paste" />
</menu>
```

Как этот переключатель пространства имен обеспечивает поддержку `showAsAction` атрибута в версиях Android до уровня API 11? Пользовательский атрибут `showAsAction` и все возможные значения включаются в приложение при установке AppCompat NuGet. 

## <a name="subclass-appcompatactivity"></a>Подкласс Аппкомпатактивити

Последним шагом в преобразовании является изменение `MainActivity` , чтобы он был `AppCompactActivity`подклассом. Измените **MainActivity.CS** и добавьте следующие `using` инструкции: 

```csharp
using Android.Support.V7.App;
using Toolbar = Android.Support.V7.Widget.Toolbar;
```

Это объявляется `Toolbar` как `Toolbar`версия AppCompat. Затем измените определение `MainActivity`класса: 

```csharp
public class MainActivity : AppCompatActivity
```

Чтобы задать панель действий для версии `Toolbar`AppCompat, замените вызов `SetSupportActionBar`на `SetActionBar` . В этом примере заголовок также изменяется, указывая, что используется версия `Toolbar` AppCompat:

```csharp
SetSupportActionBar (toolbar);
SupportActionBar.Title = "My AppCompat Toolbar";
```

Наконец, измените минимальный уровень Android на значение предварительной версии, которое должно поддерживаться (например, API 19). 

Выполните сборку приложения и запустите его на устройстве предварительной версии или эмуляторе Android. На следующем снимке экрана показана версия совместимости **тулбарфун** для хранилища 4 с KITKAT (API 19): 

[![Полный снимок экрана приложения, работающего на устройстве KitKat, отображаются обе панели инструментов](toolbar-compatibility-images/02-running-on-kitkat-sml.png)](toolbar-compatibility-images/02-running-on-kitkat.png#lightbox)

При использовании библиотеки AppCompat темы не нужно переключать на основе версии &ndash; Android. Библиотека AppCompat позволяет обеспечить согласованное взаимодействие с пользователем во всех поддерживаемых версиях Android. 

## <a name="related-links"></a>Связанные ссылки

- [Панель инструментов без описания операций (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-toolbar)
- [Панель инструментов AppCompat (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
