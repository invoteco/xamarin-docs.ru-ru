---
title: CardView
description: Мини-приложение Кардвиев — это компонент пользовательского интерфейса, который представляет содержимое текста и изображения в представлениях, напоминающих карты. В этом руководство объясняется, как использовать и настраивать Кардвиев в приложениях Xamarin. Android с сохранением обратной совместимости с более ранними версиями Android.
ms.prod: xamarin
ms.assetid: CF12FE85-D03A-4E64-95D2-D7115061A500
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 37afc9ef7773bbfefe442216055c0501af2ab966
ms.sourcegitcommit: 52fb214c0e0243587d4e9ad9306b75e92a8cc8b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2020
ms.locfileid: "76940880"
---
# <a name="xamarinandroid-cardview"></a>Xamarin. Android Кардвиев

_Мини-приложение Кардвиев — это компонент пользовательского интерфейса, который представляет содержимое текста и изображения в представлениях, напоминающих карты. В этом руководство объясняется, как использовать и настраивать Кардвиев в приложениях Xamarin. Android с сохранением обратной совместимости с более ранними версиями Android._

## <a name="overview"></a>Обзор

Мини-приложение `Cardview`, представленное в Android 5,0 (без описания операций), представляет собой компонент пользовательского интерфейса, который представляет содержимое текста и изображения в представлениях, напоминающих карты. `CardView` реализуется как мини-приложение `FrameLayout` со скругленными углами и тенью. Как правило, `CardView` используется для представления одного элемента строки в группе представлений `ListView` или `GridView`. Например, на следующем снимке экрана показан пример приложения для резервирования командировок, которое реализует `CardView`ные карточки назначения для прокрутки `ListView`:

![Пример приложения с использованием Кардвиев для каждого назначения в командировке](card-view-images/01-cardview-example.png)

В этом руководство объясняется, как добавить пакет `CardView` в проект Xamarin. Android, как добавить `CardView` в макет и как настроить внешний вид `CardView` в приложении. Кроме того, в этом руководством содержится подробный список атрибутов `CardView`, которые можно изменить, включая атрибуты, помогающие использовать `CardView` в версиях Android более ранних, чем Android 5,0.

<a name="requirements" />

## <a name="requirements"></a>Требования

Для использования новых функций Android 5,0 и более поздних версий (включая `CardView`) в приложениях на основе Xamarin необходимо следующее:

- **Xamarin. android** &ndash; Xamarin. Android 4,20 или более поздней версии должен быть установлен и настроен с помощью Visual Studio или Visual Studio для Mac.

- **Пакет SDK для Android** &ndash; Android 5,0 (API 21) или более поздней версии необходимо установить с помощью диспетчера пакет SDK для Android.

- **Java JDK 1,8** &ndash; JDK 1,7 можно использовать, если вы специально нацелены на API уровня 23 и более ранних версий. JDK 1,8 доступен в [Oracle](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Приложение должно также включать пакет `Xamarin.Android.Support.v7.CardView`. Чтобы добавить пакет `Xamarin.Android.Support.v7.CardView` в Visual Studio для Mac, выполните следующие действия.

1. Откройте проект, щелкните правой кнопкой мыши **пакеты** и выберите команду **Добавить пакеты**.

2. В диалоговом окне **Добавление пакетов** выполните поиск по запросу **кардвиев**.

3. Выберите **Библиотека поддержки Xamarin версии 7 кардвиев**, а затем щелкните **Добавить пакет**.

Чтобы добавить пакет `Xamarin.Android.Support.v7.CardView` в Visual Studio, выполните следующие действия.

1. Откройте проект, щелкните правой кнопкой мыши узел **ссылки** (в области **Обозреватель решений** ) и выберите пункт **Управление пакетами NuGet...** .

2. Когда откроется диалоговое окно **Управление пакетами NuGet** , введите **кардвиев** в поле поиска.

3. Когда появится **Библиотека поддержки Xamarin версии 7 кардвиев** , нажмите кнопку **установить**.

Сведения о настройке проекта приложения Android 5,0 см. в разделе [Настройка проекта android 5,0](~/android/platform/lollipop.md).
Дополнительные сведения об установке пакетов NuGet см. [в разделе Пошаговое руководство. Включение NuGet в проект](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

## <a name="introducing-cardview"></a>Знакомство с Кардвиев

`CardView` по умолчанию напоминает белую карту с минимально закругленными углами и небольшой тенью. В следующем примере макета **Main. axml** отображается одно мини-приложение `CardView`, содержащее `TextView`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:gravity="center_horizontal"
    android:padding="5dp">
    <android.support.v7.widget.CardView
        android:layout_width="fill_parent"
        android:layout_height="245dp"
        android:layout_gravity="center_horizontal">
        <TextView
            android:text="Basic CardView"
            android:layout_marginTop="0dp"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:gravity="center"
            android:layout_centerVertical="true"
            android:layout_alignParentRight="true"
            android:layout_alignParentEnd="true" />
    </android.support.v7.widget.CardView>
</LinearLayout>
```

Если вы используете этот XML-код для замены существующего содержимого **Main. axml**, не забудьте закомментировать любой код в **MainActivity.CS** , который ссылается на ресурсы в предыдущем XML.

В этом примере макета создается `CardView` по умолчанию с одной строкой текста, как показано на следующем снимке экрана:

[![снимок экрана Кардвиев с белым фоном и строкой текста](card-view-images/02-basic-cardview-sml.png)](card-view-images/02-basic-cardview.png#lightbox)

В этом примере для стиля приложения задана тема «светлого материала» (`Theme.Material.Light`), чтобы упростить просмотр `CardView` теней и границ. Дополнительные сведения о приложениях Android 5,0 см. в разделе « [Тема материалов](~/android/user-interface/material-theme.md)». В следующем разделе мы расскажем, как настроить `CardView` для приложения.

## <a name="customizing-cardview"></a>Настройка Кардвиев

Можно изменить основные атрибуты `CardView`, чтобы настроить внешний вид `CardView` в приложении. Например, повышение уровня `CardView` может быть увеличено для приведения более крупной тени (что делает карту плавающей выше, чем у фона). Кроме того, радиус угла можно увеличить, чтобы сделать углы карты более скругленными.

В следующем примере макета настроенный `CardView` используется для создания имитации печатной фотографии ("моментальный снимок"). `ImageView` добавляется в `CardView` для отображения изображения, а `TextView` располагается под `ImageView` для отображения заголовка изображения.
В этом примере макет `CardView` имеет следующие настройки:

- `cardElevation` увеличивается до 4dp для приведения более крупной тени.
- `cardCornerRadius` увеличивается до 5dp, чтобы сделать углы более скругленными.

Поскольку `CardView` предоставляется библиотекой поддержки Android версии 7, ее атрибуты недоступны из пространства имен `android:`. Поэтому необходимо определить собственное пространство имен XML и использовать это пространство имен в качестве префикса атрибута `CardView`. В приведенном ниже примере макета мы будем использовать эту строку для определения пространства имен с именем `cardview`:

```xml
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
```

Это пространство имен можно вызвать `card_view` или даже `myapp` если выбрать (доступно только в области этого файла). Независимо от выбранного для вызова этого пространства имен необходимо использовать его для префикса атрибута `CardView`, который требуется изменить. В этом примере макета `cardview` пространство имен — это префикс для `cardElevation` и `cardCornerRadius`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:gravity="center_horizontal"
    android:padding="5dp">
    <android.support.v7.widget.CardView
        android:layout_width="fill_parent"
        android:layout_height="245dp"
        android:layout_gravity="center_horizontal"
        cardview:cardElevation="4dp"
        cardview:cardCornerRadius="5dp">
        <LinearLayout
            android:layout_width="fill_parent"
            android:layout_height="240dp"
            android:orientation="vertical"
            android:padding="8dp">
            <ImageView
                android:layout_width="fill_parent"
                android:layout_height="190dp"
                android:id="@+id/imageView"
                android:scaleType="centerCrop" />
            <TextView
                android:layout_width="fill_parent"
                android:layout_height="wrap_content"
                android:textAppearance="?android:attr/textAppearanceMedium"
                android:textColor="#333333"
                android:text="Photo Title"
                android:id="@+id/textView"
                android:layout_gravity="center_horizontal"
                android:layout_marginLeft="5dp" />
        </LinearLayout>
    </android.support.v7.widget.CardView>
</LinearLayout>
```

Если этот пример макета используется для отображения изображения в приложении для просмотра фотографий, `CardView` имеет внешний вид снимка фотографии, как показано на следующем снимке экрана:

[![Кардвиев с изображением и заголовком под изображением](card-view-images/03-photo-cardview-sml.png)](card-view-images/03-photo-cardview.png#lightbox)

Этот снимок экрана взят из примера приложения [рециклервиевер](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer) , который использует мини-приложение `RecyclerView` для представления прокручиваемого списка изображений `CardView` для просмотра фотографий. Дополнительные сведения о `RecyclerView`см. в разделе [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) Guide.

Обратите внимание, что в области содержимого `CardView` может отображаться более одного дочернего представления. Например, в приведенном выше примере приложения для просмотра фотографий область содержимого состоит из `ListView`, содержащей `ImageView` и `TextView`. Хотя `CardView` экземпляры часто располагаются вертикально, их также можно упорядочить по горизонтали (см. раздел [Создание пользовательского стиля представления](~/android/user-interface/material-theme.md#customview) для примера снимка экрана).

### <a name="cardview-layout-options"></a>Параметры макета Кардвиев

макеты `CardView` могут быть настроены путем установки одного или нескольких атрибутов, влияющих на его заполнение, повышение прав, радиус угла и цвет фона.

[![ная схема атрибутов Кардвиев](card-view-images/04-attributes-sml.png)](card-view-images/04-attributes.png#lightbox)

Каждый атрибут также может быть изменен динамически путем вызова аналога `CardView` метода (Дополнительные сведения о методах `CardView` см. в [справочнике по классам кардвиев](https://developer.android.com/reference/android/support/v7/widget/CardView.html)).
Обратите внимание, что эти атрибуты (за исключением цвета фона) принимают значение измерения, которое представляет собой десятичное число, за которым следует единица измерения. Например, `11.5dp` задает 11,5 пикселов, не зависящих от плотности.

#### <a name="padding"></a>Заполнение

`CardView` предлагает пять атрибутов заполнения для размещения содержимого в карточке. Их можно задать в XML макета или вызвать аналогичные методы в коде:

[![ная схема атрибутов заполнения Кардвиев](card-view-images/05-padding-sml.png)](card-view-images/05-padding.png#lightbox)

Атрибуты заполнения объясняются следующим образом.

- `contentPadding` &ndash; внутреннее заполнение между дочерними представлениями `CardView` и всеми краями карты.

- `contentPaddingBottom` &ndash; внутреннее заполнение между дочерними представлениями `CardView` и нижней границей карточки.

- `contentPaddingLeft` &ndash; внутреннее заполнение между дочерними представлениями `CardView` и левой границей карточки.

- `contentPaddingRight` &ndash; внутреннее заполнение между дочерними представлениями `CardView` и правой границей карты.

- `contentPaddingTop` &ndash; внутреннее заполнение между дочерними представлениями `CardView` и верхней границы карты.

Атрибуты заполнения содержимого зависят от границы области содержимого, а не от какого-либо заданного мини-приложения, расположенного в области содержимого.
Например, если `contentPadding` было достаточно увеличено в приложении для просмотра фотографий, `CardView` обрезают изображение и текст, отображаемый на карточке.

#### <a name="elevation"></a>Полномочий

`CardView` предлагает два атрибута повышения прав для управления его повышением прав и, в результате, размер его тени:

[![схема атрибутов повышения прав Кардвиев](card-view-images/06-elevation-sml.png)](card-view-images/06-elevation.png#lightbox)

Атрибуты повышения прав описаны ниже.

- `cardElevation` &ndash; повышение прав `CardView` (представляет свою ось Z).

- `cardMaxElevation` &ndash; максимальное значение повышения прав `CardView`.

Большие значения `cardElevation` увеличивают размер тени, чтобы сделать `CardView` намерены перемещаться выше по фону. Атрибут `cardElevation` также определяет порядок отображения перекрывающихся представлений; то есть `CardView` будет отображаться в другом перекрывающейся представлении с более высоким уровнем повышения прав, а также над всеми перекрывающимися представлениями с более низким значением повышения прав.
Параметр `cardMaxElevation` полезен, когда ваше приложение динамически изменяет повышение прав &ndash; оно предотвращает расширение тени за пределами, заданными с помощью этого параметра.

#### <a name="corner-radius-and-background-color"></a>Радиус и цвет фона угла

`CardView` предлагает атрибуты, которые можно использовать для управления радиусом его угла и его цветом фона. Эти два свойства позволяют изменить общий стиль `CardView`:

[![ная диаграмма Кардвиев угла радиаус и атрибуты цвета фона](card-view-images/07-radius-bgcolor-sml.png)](card-view-images/07-radius-bgcolor.png#lightbox)

Эти атрибуты объясняются следующим образом.

- `cardCornerRadius` &ndash; радиусе углов для всех углов `CardView`.

- `cardBackgroundColor` &ndash; цвет фона `CardView`.

На этой схеме `cardCornerRadius` задано более скругленная 10dp, а `cardBackgroundColor` имеет значение `"#FFFFCC"` (светло-желтый).

## <a name="compatibility"></a>Совместимость

`CardView` можно использовать в версиях Android раньше, чем Android 5,0 без описания операций. Поскольку `CardView` входит в библиотеку поддержки Android версии 7, вы можете использовать `CardView` с Android 2,1 (API уровня 7) и более поздней версии.
Однако необходимо установить пакет `Xamarin.Android.Support.v7.CardView`, как описано в разделе [требования](#requirements)выше.

`CardView` приводит к незначительному различному поведению на устройствах перед интерфейсом без описания операций (уровень API 21):

- `CardView` использует программную теневую реализацию, которая добавляет дополнительное дополнение.

- `CardView` не обрезает дочерние представления, пересекающиеся со скругленными углами `CardView`.

Чтобы упростить управление этими различиями в совместимости, `CardView` предоставляет несколько дополнительных атрибутов, которые можно настроить в макете:

- `cardPreventCornerOverlap` &ndash; присвоить этому атрибуту значение `true`, чтобы добавить заполнение при выполнении приложения в более ранних версиях Android (уровень API 20 и более ранние версии). Этот параметр предотвращает пересечение `CardView`ного содержимого с скругленными углами `CardView`.

- `cardUseCompatPadding` &ndash; присвоить этому атрибуту значение `true`, чтобы добавить заполнение при запуске приложения в версиях Android с уровнем API 21 или выше. Если вы хотите использовать `CardView` на устройствах с предварительной установкой, и они выглядят одинаково в параметре без описания операций (или более поздней версии), присвойте этому атрибуту значение `true`. Если этот атрибут включен, `CardView` добавляет дополнительное дополнение для прорисовки теней при запуске на устройствах с предварительной компоновкой. Это помогает преодолеть различия в заполнении, которые появились, когда вступают в действие предварительные реализации программных теневых теней.

Дополнительные сведения о поддержке совместимости с более ранними версиями Android см. в разделе [поддержание совместимости](https://developer.android.com/training/material/compatibility.html).

## <a name="summary"></a>Сводка

В этом руководством появился новый мини-приложение `CardView`, включенное в Android 5,0 (без описания операций). Он демонстрирует внешний вид `CardView` по умолчанию и объясняет, как настраивать `CardView` путем изменения его повышения прав, скругления углов, заполнения содержимого и цвета фона. В нем указаны атрибуты макета `CardView` (со схемами-образцами), а также объясняется, как использовать `CardView` на устройствах Android, более ранних, чем Android 5,0 без описания операций. Дополнительные сведения о `CardView`см. в [справочнике по классам кардвиев](https://developer.android.com/reference/android/support/v7/widget/CardView.html).

## <a name="related-links"></a>Связанные ссылки

- [RecyclerView (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [Введение в интерфейс без описания операций](~/android/platform/lollipop.md)
- [Справочник по классам Кардвиев](https://developer.android.com/reference/android/support/v7/widget/CardView.html)
