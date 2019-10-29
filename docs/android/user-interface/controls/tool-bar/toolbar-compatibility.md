---
title: Совместимость панели инструментов
ms.prod: xamarin
ms.assetid: A0798CA1-2C7D-43B6-9E91-4435CC7B6683
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: 809dc8ec8fd1106b8ad8631c0c506067abdf0d97
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029075"
---
# <a name="toolbar-compatibility"></a>Совместимость панели инструментов

## <a name="overview"></a>Обзор

В этом разделе объясняется, как использовать `Toolbar` в версиях Android более ранних, чем Android 5,0 без описания операций. Если приложение не поддерживает версии Android, предшествующие Android 5,0, этот раздел можно пропустить. 

Так как `Toolbar` входит в библиотеку поддержки Android версии 7, ее можно использовать на устройствах с Android 2,1 (API уровня 7) и более поздних версий. Однако [Библиотека поддержки Android версии 7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet должна быть установлена, а код изменен так, чтобы использовать реализацию `Toolbar`, предоставленную в этой библиотеке. В этом разделе объясняется, как установить этот NuGet и изменить приложение **тулбарфун** , [добавив вторую панель инструментов](~/android/user-interface/controls/tool-bar/adding-a-second-toolbar.md) , чтобы она выполнялась в версиях Android раньше, чем с параметром без описания 5,0.

Чтобы изменить приложение для использования версии на панели инструментов AppCompat, выполните следующие действия. 

1. Задайте минимальные и целевые версии Android для приложения.

2. Установите пакет NuGet для AppCompat.

3. Используйте тему AppCompat вместо встроенной темы Android.

4. Измените `MainActivity` таким образом, чтобы он подклассировать `AppCompatActivity`, а не `Activity`. 

Каждый из этих шагов подробно описан в следующих разделах.

## <a name="set-the-minimum-and-target-android-version"></a>Установка минимальной и целевой версий Android

Целевая платформа приложения должна быть установлена на уровне API 21 или выше, иначе приложение не будет развернуто должным образом. Если при развертывании приложения обнаружена ошибка, например, **идентификатор ресурса не найден для атрибута "тилемодекс" в пакете "Android"** , это связано с тем, что для целевой платформы не задано значение **Android 5,0 (API уровня 21-интерфейса)** или больше. 

Задайте для целевого уровня платформы уровень API 21 или выше и задайте для параметров проекта уровня API Android минимальную версию Android, которую будет поддерживать приложение. Дополнительные сведения о настройке уровней API Android см. в разделе Общие сведения об [уровнях API Android](~/android/app-fundamentals/android-api-levels.md). В `ToolbarFun` примере для минимальной версии Android задано значение KitKat (API уровня 4,4). 

## <a name="install-the-appcompat-nuget-package"></a>Установка пакета AppCompat NuGet

Затем добавьте в проект пакет [AppCompat версии 7 для библиотеки поддержки Android](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) . В Visual Studio щелкните правой кнопкой мыши **ссылки** и выберите пункт **Управление пакетами NuGet...** . Нажмите кнопку **Обзор** и найдите **библиотеку поддержки Android версии 7 AppCompat**. Выберите **Xamarin. Android. support. версии 7. AppCompat** и нажмите кнопку **установить**. 

[![снимок экрана пакета AppCompat для версии 7, выбранного в окне "Управление пакетами NuGet"](toolbar-compatibility-images/01-appcompat-nuget-sml.png)](toolbar-compatibility-images/01-appcompat-nuget.png#lightbox)

При установке этого NuGet также устанавливаются несколько других пакетов NuGet (например, **Xamarin. Android. support. анимированный. Vector.** , которые можно нарисовать, **Xamarin. Android. support. v4**и  **Xamarin. Android. support. Vector. Draw**). Дополнительные сведения об установке пакетов NuGet см. [в разделе Пошаговое руководство. Включение NuGet в проект](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough). 

## <a name="use-an-appcompat-theme-and-toolbar"></a>Использование темы и панели инструментов AppCompat

Библиотека AppCompat поставляется с несколькими `Theme.AppCompat` темами, которые можно использовать в любой версии Android, поддерживаемой библиотекой AppCompat. Тема примера приложения `ToolbarFun` является производной от `Theme.Material.Light.DarkActionBar`, которая недоступна в версиях Android, предшествующих интерфейсу без описания операций. Поэтому `ToolbarFun` должны быть адаптированы для использования аналога AppCompat для этой темы, `Theme.AppCompat.Light.DarkActionBar`. Кроме того, поскольку `Toolbar` недоступна в версиях Android, предшествующих интерфейсу без описания операций, необходимо использовать версию AppCompat `Toolbar`. Поэтому макеты должны использовать `android.support.v7.widget.Toolbar` вместо `Toolbar`. 

### <a name="update-layouts"></a>Обновление макетов

Измените **Resources/Layout/Main. axml** и замените элемент `Toolbar` следующим кодом XML: 

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

Обратите внимание, что `?attr` значения больше не начинаются с `android:` (Помните, что нотация `?` ссылается на ресурс в текущей теме). Если `?android:attr` по-прежнему использовались здесь, Android будет ссылаться на значение атрибута из текущей платформы, а не из библиотеки AppCompat. Поскольку в этом примере используется `actionBarSize`, определяемый библиотекой AppCompat, префикс `android:` удаляется. Аналогичным образом `@android:style` изменяется на `@style`, чтобы атрибуту `android:theme` было присвоено значение темы в библиотеке AppCompat &ndash; вместо `ThemeOverlay.AppCompat.Dark.ActionBar` используется тема `ThemeOverlay.Material.Dark.ActionBar`. 

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

Имена элементов и родительские темы в этом примере больше не начинаются с `android:`, так как мы используем библиотеку AppCompat. Кроме того, Родительская тема изменяется на версию AppCompat `Light.DarkActionBar`. 

### <a name="update-menus"></a>Обновить меню

Для поддержки более ранних версий Android библиотека AppCompat использует настраиваемые атрибуты, отражающие атрибуты пространства имен `android:`. Однако некоторые атрибуты (например, атрибут `showAsAction`, используемые в теге `<menu>`) не существуют в платформе Android на старых устройствах &ndash; `showAsAction` был введен в интерфейсе API Android версии 11, но недоступен в интерфейсе API Android 7. По этой причине для префикса всех атрибутов, определенных библиотекой поддержки, необходимо использовать пользовательское пространство имен. В файлах ресурсов меню определено пространство имен с именем `local` для префикса атрибута `showAsAction`. 

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

`local` пространство имен добавляется в следующую строку:

```xml
xmlns:local="http://schemas.android.com/apk/res-auto">
```

`showAsAction` атрибуте предшествует это `local:` пространство имен, а не `android:` 

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

Как этот переключатель пространства имен обеспечивает поддержку атрибута `showAsAction` в версиях Android до уровня API 11? `showAsAction` настраиваемого атрибута и всех его возможных значений включаются в приложение при установке AppCompat NuGet. 

## <a name="subclass-appcompatactivity"></a>Подкласс Аппкомпатактивити

Последним шагом в преобразовании является изменение `MainActivity`, чтобы он был подклассом `AppCompactActivity`. Измените **MainActivity.CS** и добавьте следующие `using` операторы: 

```csharp
using Android.Support.V7.App;
using Toolbar = Android.Support.V7.Widget.Toolbar;
```

Это объявление `Toolbar` как версия `Toolbar`AppCompat. Затем измените определение класса `MainActivity`: 

```csharp
public class MainActivity : AppCompatActivity
```

Чтобы задать для панели действий версию AppCompat `Toolbar`, замените вызов `SetActionBar` на `SetSupportActionBar`. В этом примере заголовок также изменяется и указывает на то, что используется версия AppCompat `Toolbar`.

```csharp
SetSupportActionBar (toolbar);
SupportActionBar.Title = "My AppCompat Toolbar";
```

Наконец, измените минимальный уровень Android на значение предварительной версии, которое должно поддерживаться (например, API 19). 

Выполните сборку приложения и запустите его на устройстве предварительной версии или эмуляторе Android. На следующем снимке экрана показана версия совместимости **тулбарфун** для хранилища 4 с KITKAT (API 19): 

[![полный снимок экрана приложения, работающего на устройстве KitKat, отображаются обе панели инструментов](toolbar-compatibility-images/02-running-on-kitkat-sml.png)](toolbar-compatibility-images/02-running-on-kitkat.png#lightbox)

При использовании библиотеки AppCompat темы не нужно переключать на основе версии Android &ndash; библиотека AppCompat позволяет обеспечить согласованное взаимодействие с пользователем во всех поддерживаемых версиях Android. 

## <a name="related-links"></a>Связанные ссылки

- [Панель инструментов без описания операций (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-toolbar)
- [Панель инструментов AppCompat (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
