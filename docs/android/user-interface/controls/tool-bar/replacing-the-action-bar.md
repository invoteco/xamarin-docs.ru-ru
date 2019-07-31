---
title: Замена панели действий
ms.prod: xamarin
ms.assetid: 5341D28E-B203-478D-8464-6FAFDC3A4110
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/27/2018
ms.openlocfilehash: 19ac5a023b1f97b2e08bbe1821a2b9259280fc98
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68645151"
---
# <a name="replacing-the-action-bar"></a>Замена панели действий

## <a name="overview"></a>Обзор

Одно из наиболее распространенных применений `Toolbar` — замена панели действий по умолчанию настраиваемой `Toolbar` (при создании нового проекта Android используется панель действий по умолчанию). Так как `Toolbar` предоставляет возможность добавлять фирменные логотипы, заголовки, элементы меню, кнопки навигации и даже пользовательские представления в раздел панели приложений пользовательского интерфейса действия, он предлагает значительное обновление по умолчанию на панели действий.

Чтобы заменить панель действий по умолчанию приложения на `Toolbar`: 

1.  Создайте новую пользовательскую тему и измените свойства приложения так, чтобы она использовала эту новую тему. 

2.  Отключите атрибут в пользовательской теме и `windowNoTitle` включите атрибут. `windowActionBar`

3.  Определите макет для `Toolbar`.

4.  Включите макет в файл макета **Main. axml** действия. `Toolbar` 

5.  Добавьте код в `OnCreate` метод действия для `Toolbar` нахождение и вызова `SetActionBar` для установки в `ToolBar` качестве панели действий.

В следующих разделах подробно описывается этот процесс. Создается простое приложение, и его панель действий заменяется настроенной `Toolbar`. 



## <a name="start-an-app-project"></a>Запуск проекта приложения

Создайте проект Android с именем **тулбарфун** (Дополнительные сведения о создании нового проекта Android см. в разделе [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) ). После создания проекта задайте для целевых и минимальных уровней API Android значение **Android 5,0 (уровень API 21-без описания)** или более поздней версии. Дополнительные сведения о настройке уровней версии Android см. в разделе Общие сведения об [уровнях API Android](~/android/app-fundamentals/android-api-levels.md). При сборке и запуске приложения отображается панель действий по умолчанию, как показано на следующем снимке экрана:

[![Снимок экрана: панель действий по умолчанию](replacing-the-action-bar-images/01-before-sml.png)](replacing-the-action-bar-images/01-before.png#lightbox)



## <a name="create-a-custom-theme"></a>Создание пользовательской темы

Откройте каталог **Resources/Values** и создайте новый файл с именем **styles. XML**. Замените его содержимое следующим XML-кодом: 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<resources>
  <style name="MyTheme" parent="@android:style/Theme.Material.Light.DarkActionBar">
    <item name="android:windowNoTitle">true</item>
    <item name="android:windowActionBar">false</item>
    <item name="android:colorPrimary">#5A8622</item>
  </style>
</resources>
```

Этот XML-код определяет новую пользовательскую тему под названием **мисеме** , основанную на теме **Theme. материальн. light. даркактионбар** в параметре без описания операций. `windowNoTitle` Атрибут имеет `true` значение, чтобы скрыть строку заголовка: 

```xml
<item name="android:windowNoTitle">true</item>
```

Чтобы отобразить пользовательскую панель инструментов, необходимо `ActionBar` отключить значение по умолчанию: 

```xml
<item name="android:windowActionBar">false</item>
```

Параметр "Оливковый- `colorPrimary` зеленый" используется для фонового цвета панели инструментов. 
 
```xml
<item name="android:colorPrimary">#5A8622</item>
```

## <a name="apply-the-custom-theme"></a>Применение пользовательской темы

Измените **Свойства/AndroidManifest. XML** и добавьте следующий `android:theme` атрибут в `<application>` `MyTheme` элемент, чтобы приложение использовало пользовательскую тему: 

```xml
<application android:label="@string/app_name" android:theme="@style/MyTheme"></application>
```

Дополнительные сведения о применении пользовательской темы к приложению см. в разделе [Использование пользовательских тем](~/android/user-interface/material-theme.md#customtheme). 



## <a name="define-a-toolbar-layout"></a>Определение макета панели инструментов

В каталоге **ресурсов или макета** создайте файл с именем **Toolbar. XML**. Замените его содержимое следующим XML-кодом: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:minHeight="?android:attr/actionBarSize"
    android:background="?android:attr/colorPrimary"
    android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"/>
```

Этот XML-код определяет `Toolbar` пользовательский объект, который заменяет панель действий по умолчанию. Минимальная высота `Toolbar` параметра равна размеру заменяемой панели действий: 

```csharp
android:minHeight="?android:attr/actionBarSize"
```

Цвет фона элемента `Toolbar` задается в виде темно-зеленого цвета, определенного ранее в **styles. XML**:

```csharp
android:background="?android:attr/colorPrimary"
```

Начиная с с параметром `android:theme` без описания, атрибут может использоваться для стиля отдельного представления. Темы, появившиеся в интерфейсе без описания операций, позволяют `Theme.Material` накладывать темы по умолчанию, переписывая соответствующие атрибуты, чтобы сделать их светлыми или темными. `ThemeOverlay.Material` В этом примере `Toolbar` компонент использует темную тему, чтобы ее содержимое было светлым в цвете: 

```csharp
android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"
```

Этот параметр используется, чтобы элементы меню отличались от более темного цвета фона.



## <a name="include-the-toolbar-layout"></a>Включить макет панели инструментов

Измените файл макета Resources **/Layout/Main. axml** и замените его содержимое следующим XML-кодом:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <include
        android:id="@+id/toolbar"
        layout="@layout/toolbar" />
</RelativeLayout>
```

Этот макет включает объект `Toolbar` , определенный в **Toolbar. XML** , и `RelativeLayout` использует, чтобы указать `Toolbar` , что компонент будет помещен в верхнюю часть пользовательского интерфейса (над кнопкой). 



## <a name="find-and-activate-the-toolbar"></a>Поиск и активация панели инструментов

Измените **MainActivity.CS** и добавьте следующую инструкцию using:

```csharp
using Android.Views;
```

Кроме того, добавьте в конец `OnCreate` метода следующие строки кода:

```csharp
var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
SetActionBar(toolbar);
ActionBar.Title = "My Toolbar";
```

Этот код находит `Toolbar` вызовы `SetActionBar` и, `Toolbar` чтобы команда предложит характеристики панели действий по умолчанию. Заголовок панели инструментов изменится на **панель инструментов**. Как показано в этом примере кода, на `ToolBar` него можно ссылаться непосредственно как на панель действий. Скомпилируйте и запустите это приложение &ndash; . настроенное `Toolbar` значение отображается вместо панели действий по умолчанию: 

[![Снимок экрана настраиваемой панели инструментов с зеленой цветовой схемой](replacing-the-action-bar-images/02-after-sml.png)](replacing-the-action-bar-images/02-after.png#lightbox)

Обратите внимание `Toolbar` , что стиль определяется независимо `Theme.Material.Light.DarkActionBar` от темы, которая применяется к оставшейся части приложения. 

Если при выполнении приложения возникает исключение, см. раздел [Устранение неполадок](#troubleshooting) ниже.

 
## <a name="add-menu-items"></a>Добавление пунктов меню 

В этом разделе меню добавляются `Toolbar`в. Верхняя правая область `ToolBar` зарезервирована для пунктов &ndash; меню каждый пункт меню (также называемый *элементом действия*) может выполнять действия в рамках текущего действия или выполнять действия от имени всего приложения. 

Чтобы добавить меню `Toolbar`в: 

1.  Добавьте значки меню (при необходимости) в `mipmap-` папки проекта приложения. Google предоставляет набор значков свободного меню на странице [значков материалов](https://design.google.com/icons/) . 

2.  Определите содержимое пунктов меню, добавив новый файл ресурсов меню в разделе **ресурсы/меню**. 

3.  Реализуйте &ndash; метод действия, с помощью которого этот метод расширяет пункты меню. `OnCreateOptionsMenu` 

4.  Реализация метода действия &ndash; . Этот метод выполняет действие при касании пункта меню. `OnOptionsItemSelected` 

В следующих разделах подробно описывается этот процесс путем добавления элементов меню **Правка** и **сохранить** в настроенные `Toolbar`. 



### <a name="install-menu-icons"></a>Значки меню "установить"

Продолжая работу `ToolbarFun` с примером приложения, добавьте значки меню в проект приложения. Загрузите [значки панели инструментов](https://github.com/xamarin/monodroid-samples/blob/master/Supportv7/AppCompat/Toolbar/Resources/toolbar-icons-plus.zip?raw=true), распакуйте и скопируйте содержимое извлеченных *mipmap-* папок в проект *mipmap-* Folders в **тулбарфун/** Resources и включите каждый добавленный файл значков в проект.


### <a name="define-a-menu-resource"></a>Определение ресурса меню

Создайте новый подкаталог **меню** в разделе **ресурсы**. В подкаталоге **меню** создайте новый файл ресурсов меню с именем **top_menus. XML** и замените его содержимое следующим XML-кодом: 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item
       android:id="@+id/menu_edit"
       android:icon="@mipmap/ic_action_content_create"
       android:showAsAction="ifRoom"
       android:title="Edit" />
  <item
       android:id="@+id/menu_save"
       android:icon="@mipmap/ic_action_content_save"
       android:showAsAction="ifRoom"
       android:title="Save" />
  <item
       android:id="@+id/menu_preferences"
       android:showAsAction="never"
       android:title="Preferences" />
</menu>
```

Этот XML-код создает три пункта меню:

-   Пункт меню " **Правка** ", в `ic_action_content_create.png` котором используется значок (карандаш). 

-   Пункт меню " **сохранить** " `ic_action_content_save.png` , в котором используется значок (дискета). 

-   Элемент меню **настроек** , у которого нет значка.

&ndash; `ifRoom` `Toolbar` Атрибуты пунктов меню Правка и сохранить имеют значение этот параметр приводит к тому, что эти пункты меню будут отображаться в, если для их отображения достаточно места. `showAsAction` Элемент меню « **настройки»** `showAsAction` задает `never` &ndash; , что меню « **настройки»** отображается в меню *переполнения* (три вертикальные точки). 


### <a name="implement-oncreateoptionsmenu"></a>Реализация Онкреатеоптионсмену

Добавьте следующий метод в **MainActivity.CS**:

```csharp
public override bool OnCreateOptionsMenu(IMenu menu)
{
    MenuInflater.Inflate(Resource.Menu.top_menus, menu);
    return base.OnCreateOptionsMenu(menu);
}
```

Android вызывает `OnCreateOptionsMenu` метод, чтобы приложение могла указать ресурс меню для действия. В этом методе ресурс **top_menus. XML** преобразуется в переданный `menu`. Этот код приводит `Toolbar`к отображению новых элементов меню **Правка**, **сохранить**и **настройки** в. 



### <a name="implement-onoptionsitemselected"></a>Реализация Оноптионситемселектед

Добавьте следующий метод в **MainActivity.CS**:

```csharp
public override bool OnOptionsItemSelected(IMenuItem item)
{
    Toast.MakeText(this, "Action selected: " + item.TitleFormatted,
        ToastLength.Short).Show();
    return base.OnOptionsItemSelected(item);
}
```

Когда пользователь касается пункта меню, Android вызывает `OnOptionsItemSelected` метод и передает выбранный элемент меню. В этом примере реализация просто отображает всплывающее уведомление, указывающее, какой пункт меню был касанием. 

Выполните сборку и `ToolbarFun` запуск, чтобы просмотреть новые пункты меню на панели инструментов. `Toolbar` Теперь отображается три значка меню, как показано на следующем снимке экрана: 

[![Схема, иллюстрирующая расположение пунктов меню "Правка", "Сохранить" и "переполнение"](replacing-the-action-bar-images/04-menu-items-sml.png)](replacing-the-action-bar-images/04-menu-items.png#lightbox)

Когда пользователь отменяет пункт меню " **Правка** ", отображается всплывающее уведомление, указывающее `OnOptionsItemSelected` , что был вызван метод: 

[![Снимок экрана всплывающего уведомления при нажатии кнопки "изменить элемент"](replacing-the-action-bar-images/05-toast-displayed-sml.png)](replacing-the-action-bar-images/05-toast-displayed.png#lightbox)

При касании пользователем меню переполнения отображается пункт меню **настройки** . &ndash; Обычно менее распространенные действия должны быть помещены в меню переполнения. в этом примере используется меню переполнения для **настройки** , так как оно не используется так часто, как **Редактирование** и **Сохранение**. 

[![Снимок экрана: пункт меню "настройки", отображаемый в меню переполнения](replacing-the-action-bar-images/06-preferences-sml.png)](replacing-the-action-bar-images/06-preferences.png#lightbox)

Дополнительные сведения о меню Android см. в разделе [меню](https://developer.android.com/guide/topics/ui/menus.html) разработчиков Android. 
 

## <a name="troubleshooting"></a>Устранение неполадок

Следующие советы могут помочь в отладке проблем, которые могут возникнуть при замене панели действий на панель инструментов.

### <a name="activity-already-has-an-action-bar"></a>У действия уже есть панель действий

Если приложение неправильно настроено для использования пользовательской темы, как описано в разделах [применение пользовательской темы](#apply-the-custom-theme), при запуске приложения может возникнуть следующее исключение:

![Ошибка, которая может возникать, если пользовательская тема не используется](replacing-the-action-bar-images/03-theme-not-defined.png)

Кроме того, может быть создано сообщение об ошибке следующего типа: _Java. lang. Иллегалстатиксцептион: Это действие уже содержит панель действий, предоставляемую окном дéкор._ 

Чтобы исправить эту ошибку, убедитесь, что `android:theme` атрибут для пользовательской темы добавлен в `<application>` (в свойствах **/AndroidManifest. XML**), как описано выше в разделе [применение пользовательской темы](#apply-the-custom-theme). Кроме того `Toolbar` , эта ошибка может быть вызвана неправильной настройкой макета или пользовательской темы.


## <a name="related-links"></a>Связанные ссылки

- [Панель инструментов без описания операций (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-toolbar)
- [Панель инструментов AppCompat (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
