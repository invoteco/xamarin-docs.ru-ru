---
title: Функции KitKat
description: Android 4,4 (KitKat) поставляется с корнукопиаом функций для пользователей и разработчиков. В этом разделе описываются некоторые из этих функций, а также приведены примеры кода и сведения о реализации, которые помогут максимально эффективно использовать KitKat.
ms.prod: xamarin
ms.assetid: D3FDEA1C-F076-406F-BCC3-2A55D2C6ADEE
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 11ec23abda6f1faae19ba9108150b9aaf55ef290
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510584"
---
# <a name="kitkat-features"></a>Функции KitKat

_Android 4,4 (KitKat) поставляется с корнукопиаом функций для пользователей и разработчиков. В этом разделе описываются некоторые из этих функций, а также приведены примеры кода и сведения о реализации, которые помогут максимально эффективно использовать KitKat._

## <a name="overview"></a>Обзор

Android 4,4 (API уровня 19), также известный как "KitKat", был выпущен в конце 2013. KitKat предлагает ряд новых функций и усовершенствований, в том числе:

-  [Взаимодействие с пользователем](#user_experience) &ndash; Простая анимация с инфраструктурой переходов, полупрозрачными состояниями и панелями навигации, а также полноэкранный режим, позволяющий пользователю улучшить работу.

-  [Содержимое пользователя](#user_content) &ndash; Управление файлами пользователей упрощено с помощью платформы доступа к хранилищу; печать изображений, веб-сайтов и другого содержимого упрощается благодаря улучшенным интерфейсам API печати.

-  [Оборудование](#hardware) Превратите любое приложение в NFC-карту с помощью эмуляции карты на основе поставщика NFC; запустите датчики `SensorManager` с низким энергопотреблением. &ndash;

-  [Средства для разработчиков](#developer_tools) &ndash; Приложения ролика в действии с клиентом Android Debug Bridge, доступным как часть пакет SDK для Android.


Это руководство содержит рекомендации по переносу существующего приложения Xamarin. Android в KitKat, а также общий обзор KitKat для разработчиков Xamarin. Android.

## <a name="requirements"></a>Требования

Для разработки приложений Xamarin. Android с помощью KitKat требуется *Xamarin. Android 4.11.0* или более поздней версии и Android 4,4 (API уровня 19), установленные с помощью диспетчера пакет SDK для Android, как показано на следующем снимке экрана:

[![Выбор Android 4,4 в диспетчере пакет SDK для Android](kitkat-images/api19.png)](kitkat-images/api19.png#lightbox)

<a name="Migrating_Your_App_to_KitKat" />

## <a name="migrating-your-app-to-kitkat"></a>Перенос приложения в KitKat

В этом разделе содержатся некоторые элементы первого ответа, помогающие перенести существующие приложения на Android 4,4.

### <a name="check-system-version"></a>Проверить версию системы

Если приложение должно быть совместимо с более старыми версиями Android, необходимо заключить код, зависящий от KitKat, в проверку версии системы, как показано в примере кода ниже:

```csharp
if (Build.VERSION.SdkInt >= BuildVersionCodes.Kitkat) {
    //KitKat only code here
}
```

### <a name="alarm-batching"></a>Пакетная обработка предупреждений

Android использует службы сигнализации для пробуждения приложения в фоновом режиме в указанное время. KitKat выполняет более подробное пакетирование оповещений для экономии энергии. Это означает, что вместо пробуждения каждого приложения в точное время KitKat предпочитает сгруппировать несколько приложений, зарегистрированных для пробуждения в течение одного интервала времени, и одновременно вывести их из спящего режима.
Чтобы перевести приложение Android в спящий режим в течение заданного интервала времени `SetWindow` , вызовите метод [`AlarmManager`](xref:Android.App.AlarmManager), передав минимальное и максимальное время в миллисекундах, которое может пройти до того, как приложение будет пробуждении, и выполнить операцию при пробуждении.
В следующем коде приведен пример приложения, которое должно быть пробуждении в течение половины часа и часа с момента установки окна:

```csharp
AlarmManager alarmManager = (AlarmManager)GetSystemService(AlarmService);
alarmManager.SetWindow (AlarmType.Rtc, AlarmManager.IntervalHalfHour, AlarmManager.IntervalHour, pendingIntent);
```

Чтобы продолжить пробуждение приложения в точное время, используйте `SetExact`, передав точное время, когда приложение должно быть пробуждении, и выполняемую операцию:

```csharp
alarmManager.SetExact (AlarmType.Rtc, AlarmManager.IntervalDay, pendingIntent);
```

KitKat больше не позволяет задать точную повторяющуюся сигнализацию. Приложения, использующие[`SetRepeating`](xref:Android.App.AlarmManager.SetRepeating*)
и требовать, чтобы точные оповещения работали, необходимо активировать каждый будильник вручную.

### <a name="external-storage"></a>Внешнее хранилище

Теперь внешнее хранилище делится на два типа: хранилище, уникальное для приложения, и данные, совместно используемые несколькими приложениями. Для чтения и записи в конкретное расположение приложения во внешнем хранилище не требуются специальные разрешения. Для взаимодействия с данными в общем хранилище теперь требуется `READ_EXTERNAL_STORAGE` разрешение или. `WRITE_EXTERNAL_STORAGE` Эти два типа можно классифицировать следующим образом:

-  Если вы получаете путь к файлу или каталогу путем вызова метода в `Context` , например,[`GetExternalFilesDir`](xref:Android.Content.Context.GetExternalFilesDir*)
   ни[`GetExternalCacheDirs`](xref:Android.Content.Context.GetExternalCacheDirs)
   - приложению не требуются дополнительные разрешения.

-  Если вы получаете путь к файлу или каталогу путем доступа к свойству или вызова метода в `Environment` , например[`GetExternalStorageDirectory`](xref:Android.OS.Environment.ExternalStorageDirectory)
   ни[`GetExternalStoragePublicDirectory`](xref:Android.OS.Environment.GetExternalStoragePublicDirectory*)
   приложение должно иметь `READ_EXTERNAL_STORAGE` разрешение или `WRITE_EXTERNAL_STORAGE` .

> [!NOTE]
> `WRITE_EXTERNAL_STORAGE``READ_EXTERNAL_STORAGE` подразумевает разрешение, поэтому вам нужно только установить одно разрешение.

### <a name="sms-consolidation"></a>Консолидация SMS

KitKat упрощает обмен сообщениями для пользователя путем агрегирования всего содержимого SMS в одном приложении по умолчанию, выбранном пользователем. Разработчик несет ответственность за то, чтобы приложение было выбрано в качестве приложения для обмена сообщениями по умолчанию и правильно навести себя в коде и в жизни, если приложение не выбрано. Дополнительные сведения о переводе приложения SMS в KitKat см. в руководстве по обновлению [SMS для KitKat](http://android-developers.blogspot.com/2013/10/getting-your-sms-apps-ready-for-kitkat.html) в Google.

### <a name="webview-apps"></a>Приложения WebView

[WebView](xref:Android.Webkit.WebView) получил преобразование в KitKat. Крупнейшим изменением является защита загрузки содержимого в `WebView`. Хотя большинство приложений, предназначенных для более старых версий API, должны работать ожидаемым образом, `WebView` настоятельно рекомендуется тестировать приложения, использующие этот класс. Дополнительные сведения о затронутых API WebView см. в документации по Android миграция в [WebView в android 4,4](https://developer.android.com/guide/webapps/migrating.html) .

<a name="user_experience" />

## <a name="user-experience"></a>Взаимодействие с пользователем

KitKat поставляется с несколькими новыми API-интерфейсами для повышения удобства работы пользователей, включая новую платформу перехода для обработки анимации свойств и полупрозрачный параметр пользовательского интерфейса для них. Эти изменения описаны ниже.

### <a name="transition-framework"></a>Платформа перехода

Платформа переходов упрощает реализацию анимации. KitKat позволяет выполнять простую анимацию свойств с помощью всего одной строки кода или настраивать переходы с использованием *сцен*.

#### <a name="simple-property-animation"></a>Анимация простого свойства

Новая библиотека переходов Android упрощает анимацию свойств кода программной части. Платформа позволяет выполнять простую анимацию с минимальным кодом. Например, в следующем примере кода используется[`TransitionManager.BeginDelayedTransition`](xref:Android.Transitions.TransitionManager.BeginDelayedTransition*)
анимация отображения и скрытия `TextView`:

```csharp
using Android.Transitions;

public class MainActivity : Activity
{
    LinearLayout linear;
    Button button;
    TextView text;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        SetContentView (Resource.Layout.Main);

        linear = FindViewById<LinearLayout> (Resource.Id.linearLayout);
        button = FindViewById<Button> (Resource.Id.button);
        text = FindViewById<TextView> (Resource.Id.textView);

        button.Click += (o, e) => {

            TransitionManager.BeginDelayedTransition (linear);

            if(text.Visibility != ViewStates.Visible)
            {
                text.Visibility = ViewStates.Visible;
            }
            else
            {
                text.Visibility = ViewStates.Invisible;
            }
        };
    }
}
```

В приведенном выше примере используется платформа переходов для создания автоматического перехода между изменяющимися значениями свойств по умолчанию. Поскольку анимация обрабатывается одной строкой кода, вы можете легко сделать ее совместимой с более старыми версиями Android, заключив `BeginDelayedTransition` вызов в проверку версии системы. Дополнительные сведения см. в разделе [миграция приложения в KitKat](#Migrating_Your_App_to_KitKat) .

На следующем снимке экрана показано приложение перед анимацией:

[![Снимок экрана приложения перед началом анимации](kitkat-images/trans-before.png)](kitkat-images/trans-before.png#lightbox)

На следующем снимке экрана показано приложение после анимации:

[![Снимок экрана приложения после завершения анимации](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

Вы можете получить больший контроль над переходом с помощью сцен, которые рассматриваются в следующем разделе.

#### <a name="android-scenes"></a>Сцены Android

[Сцены](xref:Android.Transitions.Scene) появились в составе платформы переходов, чтобы предоставить разработчику больший контроль над анимациями. Сцены создают динамическую область в пользовательском интерфейсе: вы указываете контейнер и несколько версий или "сцены" для XML-содержимого в контейнере, а Android выполняет оставшуюся часть работы для анимации переходов между сценой. Сцены Android позволяют создавать сложные анимации с минимальными усилиями на стороне разработки.

Статический элемент пользовательского интерфейса, размещает динамическое содержимое, называется *контейнером* или *основой сцены*. В приведенном ниже примере используется Android Designer для создания `RelativeLayout` вызываемого `container`:

[![Создание контейнера RelativeLayout с помощью Android Designer](kitkat-images/container.png)](kitkat-images/container.png#lightbox)

В примере макета также определена кнопка, `sceneButton` `container`вызываемая под. Эта кнопка вызывает переход.

Динамическое содержимое в контейнере требует двух новых макетов Android. Эти макеты указывают только код *внутри* контейнера.
В приведенном ниже примере кода определяется макет с именем *Scene1* , который содержит два текстовых поля, которые считывают "Kit" и "Kat" соответственно, и второй макет с именем *Scene2* , который содержит одинаковые текстовые поля. XML-код выглядит следующим образом:

 **Scene1. axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android">
    <TextView
        android:id="@+id/textA"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Kit"
        android:textSize="35sp" />
    <TextView
        android:id="@+id/textB"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/textA"
        android:text="Kat"
        android:textSize="35sp" />
</merge>
```

 **Scene2. axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android">
    <TextView
        android:id="@+id/textB"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Kat"
        android:textSize="35sp" />
    <TextView
        android:id="@+id/textA"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/textB"
        android:text="Kit"
        android:textSize="35sp" />
</merge>
```

В приведенном выше `merge` примере используется, чтобы сделать код представления короче и упростить иерархию представлений. Дополнительные сведения о `merge` макетах можно узнать [здесь](http://android-developers.blogspot.com/2009/03/android-layout-tricks-3-optimize-by.html).

Сцена создается путем вызова [`Scene.GetSceneForLayout`](xref:Android.Transitions.Scene.GetSceneForLayout*), передачи объекта контейнера, идентификатора ресурса файла макета сцены и текущего `Context`объекта, как показано в следующем примере кода:

```csharp
RelativeLayout container = FindViewById<RelativeLayout> (Resource.Id.container);

Scene scene1 = Scene.GetSceneForLayout(container, Resource.Layout.Scene1, this);
Scene scene2 = Scene.GetSceneForLayout(container, Resource.Layout.Scene2, this);

scene1.Enter();
```

При нажатии кнопки переворачивается между двумя кадрами, которые Android анимируется со значениями перехода по умолчанию:

```csharp
sceneButton.Click += (o, e) => {
    Scene temp = scene2;
    scene2 = scene1;
    scene1 = temp;

    TransitionManager.Go (scene1);
};
```

На следующем снимке экрана показана сцена перед анимацией:

[![Снимок экрана приложения перед началом анимации](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

На следующем снимке экрана показана сцена после анимации:

[![Снимок экрана приложения после завершения анимации](kitkat-images/scene.png)](kitkat-images/scene.png#lightbox)


> [!NOTE]
> В библиотеке переходов Android есть [известная ошибка](https://code.google.com/p/android/issues/detail?id=62450) , из-за которой сцены `GetSceneForLayout` , созданные с помощью, прерываются, когда пользователь переходит по действию во второй раз. Решение Java описывается [здесь](http://www.doubleencore.com/2013/11/new-transitions-framework/).


##### <a name="custom-transitions-in-scenes"></a>Пользовательские переходы в сценах

Пользовательский переход можно определить в файле ресурсов XML в `transition` `Resources`каталоге, как показано на снимке экрана ниже:

[![Расположение файла Transition. XML в каталоге ресурсов или перехода](kitkat-images/resources.png)](kitkat-images/resources.png#lightbox)

В следующем образце кода определяется переход, который анимируется в течение 5 секунд и использует [интерполяцию отклонение](https://developer.android.com/reference/android/views/animation/OvershootInterpolator.html):

```xml
<changeBounds
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:duration="5000"
  android:interpolator="@android:anim/overshoot_interpolator" />
```

Переход создается в действии с помощью [транситионинфлатер](xref:Android.Transitions.TransitionInflater), как показано в следующем коде:

```csharp
Transition transition = TransitionInflater.From(this).InflateTransition(Resource.Transition.transition);
```

Затем новый переход добавляется к `Go` вызову, который начинает анимацию:

```csharp
TransitionManager.Go (scene1, transition);
```

### <a name="translucent-ui"></a>Полупрозрачный пользовательский интерфейс

KitKat обеспечивает более полный контроль над приложениями с помощью необязательного состояния и панелей навигации. Вы можете изменить полупрозрачность элементов пользовательского интерфейса системы в том же XML-файле, который используется для определения темы Android. KitKat вводит следующие свойства:

-  `windowTranslucentStatus`— Если задано значение true, верхняя строка состояния становится полупрозрачной.

-  `windowTranslucentNavigation`— Если задано значение true, Нижняя панель навигации становится полупрозрачной.

-  `fitsSystemWindows`— Установка значения по умолчанию для верхней или нижней полоски трансклуент смещает содержимое в прозрачных элементах пользовательского интерфейса. Присвоение этому свойству `true` простого способа предотвращения перекрытия содержимого с помощью полупрозрачных элементов пользовательского интерфейса системы.


Следующий код определяет тему со полупрозрачным состоянием и панелями навигации:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <style name="KitKatTheme" parent="android:Theme.Holo.Light">
        <item name="android:windowBackground">@color/xamgray</item>
        <item name="android:windowTranslucentStatus">true</item>
        <item name="android:windowTranslucentNavigation">true</item>
        <item name="android:fitsSystemWindows">true</item>
        <item name="android:actionBarStyle">@style/ActionBar.Solid.KitKat</item>
    </style>

    <style name="ActionBar.Solid.KitKat" parent="@android:style/Widget.Holo.Light.ActionBar.Solid">
        <item name="android:background">@color/xampurple</item>
    </style>
</resources>
```

На снимке экрана ниже показана тема с полупрозрачным состоянием и панелями навигации:

[![Пример снимка экрана приложения со полупрозрачным состоянием и панелями навигации](kitkat-images/theme.png)](kitkat-images/theme.png#lightbox)

<a name="user_content" />

## <a name="user-content"></a>Содержимое пользователя

### <a name="storage-access-framework"></a>Платформа доступа к хранилищу

Платформа доступа к хранилищу (SAF) — это новый способ взаимодействия пользователей с сохраненным содержимым, таким как изображения, видео и документы. Вместо предоставления пользователям диалогового окна для выбора приложения для обработки содержимого KitKat открывает новый пользовательский интерфейс, который позволяет пользователям обращаться к своим данным в одном статистическом расположении. После выбора содержимого пользователь вернется в приложение, которое запросило содержимое, и работа приложения продолжится в нормальном режиме.

Для этого изменения требуются два действия на стороне разработчика: сначала приложения, требующие содержимого от поставщиков, должны быть обновлены до нового способа запроса содержимого. Во-вторых, приложения, которые записывают данные в, `ContentProvider` должны быть изменены для использования новой платформы. Оба сценария зависят от нового[`DocumentsProvider`](xref:Android.Provider.DocumentsProvider)
API.

#### <a name="documentsprovider"></a>документспровидер

В KitKat взаимодействия с `ContentProviders` `DocumentsProvider` классами абстрактны с классом. Это означает, что SAF не следит за физическими данными, пока они доступны через `DocumentsProvider` API. Локальные поставщики, облачные службы и внешние запоминающие устройства используют один и тот же интерфейс и обрабатываются таким же образом, предоставляя пользователю и разработчику одно место для взаимодействия с содержимым пользователя.

В этом разделе описывается загрузка и сохранение содержимого с помощью платформы доступа к хранилищу.

#### <a name="request-content-from-a-provider"></a>Запросить содержимое от поставщика

Мы можем сообщить KitKat, что мы хотим выбрать содержимое с помощью пользовательского интерфейса SAF с `ActionOpenDocument` намерением, что означает, что мы хотим подключиться ко всем поставщикам содержимого, доступным для устройства. Можно добавить фильтрацию по этому намерению, указав `CategoryOpenable`, что означает, что будет возвращено только содержимое, которое можно открыть (т. е. доступное для использования содержимое). KitKat также позволяет фильтровать содержимое с `MimeType`помощью. Например, код, приведенный ниже, фильтрует для результатов изображения, указывая `MimeType`изображение:

```csharp
Intent intent = new Intent (Intent.ActionOpenDocument);
intent.AddCategory (Intent.CategoryOpenable);
intent.SetType ("image/*");
StartActivityForResult (intent, save_request_code);
```

Вызов `StartActivityForResult` запускает пользовательский интерфейс SAF, который пользователь может затем выбрать для выбора изображения:

[![Пример снимка экрана приложения с использованием платформы доступа к хранилищу для обзора изображения](kitkat-images/saf-ui.png)](kitkat-images/saf-ui.png#lightbox)

После выбора пользователем изображения `OnActivityResult` `Android.Net.Uri` возвращает из выбранного файла. В приведенном ниже примере кода отображается выбранный пользователем образ:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);

    if (resultCode == Result.Ok && data != null && requestCode == save_request_code) {
        imageView = FindViewById<ImageView> (Resource.Id.imageView);
        imageView.SetImageURI (data.Data);
    }
}
```

#### <a name="write-content-to-a-provider"></a>Запись содержимого в поставщик

Помимо загрузки содержимого из пользовательского интерфейса SAF, KitKat также позволяет сохранять содержимое в любой `ContentProvider` , который `DocumentProvider` реализует API. При сохранении содержимого используется `Intent` объект `ActionCreateDocument`со следующим параметром:

```csharp
Intent intentCreate = new Intent (Intent.ActionCreateDocument);
intentCreate.AddCategory (Intent.CategoryOpenable);
intentCreate.SetType ("text/plain");
intentCreate.PutExtra (Intent.ExtraTitle, "NewDoc");
StartActivityForResult (intentCreate, write_request_code);
```

Приведенный выше пример кода загружает пользовательский интерфейс SAF, позволяя пользователю изменить имя файла и выбрать каталог для размещения нового файла:

[![Снимок экрана: пользователь изменяет имя файла на Невдок в каталоге загрузок](kitkat-images/saf-save.png)](kitkat-images/saf-save.png#lightbox)

Когда пользователь нажимает кнопку "Сохранить `OnActivityResult` ", передается `Android.Net.Uri` только что созданный файл, к которому можно получить `data.Data`доступ с помощью. Универсальный код ресурса (URI) можно использовать для потоковой передачи данных в новый файл:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);

    if (resultCode == Result.Ok && data != null && requestCode == write_request_code) {
        using (Stream stream = ContentResolver.OpenOutputStream(data.Data)) {
            Encoding u8 = Encoding.UTF8;
            string content = "Hello, world!";
            stream.Write (u8.GetBytes(content), 0, content.Length);
        }
    }
}
```

Обратите внимание, что[`ContentResolver.OpenOutputStream(Android.Net.Uri)`](xref:Android.Content.ContentResolver.OpenOutputStream*)
`System.IO.Stream`возвращает, поэтому весь процесс потоковой передачи может быть написан на .NET.

Дополнительные сведения о загрузке, создании и редактировании содержимого с помощью платформы доступа к хранилищу см. в [документации Android для платформы доступа к хранилищу](https://developer.android.com/guide/topics/providers/document-provider.html).

### <a name="printing"></a>Печать

Печать содержимого упрощается в KitKat с введением [служб печати](xref:Android.PrintServices) и `PrintManager`. KitKat — это также первая версия API, которая позволяет полностью использовать [API облачной службы печати Google](https://developers.google.com/cloud-print/) с помощью [приложения Google Cloud Print](https://play.google.com/store/apps/details?id=com.google.android.apps.cloudprint).
Большинство устройств, поставляемых с KitKat, автоматически загружают приложение для печати Google Cloud и [подключаемый модуль HP Print Service](https://play.google.com/store/apps/details?id=com.hp.android.printservice)при первом подключении к Wi-Fi. Пользователь может проверить параметры печати своего устройства, перейдя к **параметрам > системе > печати**:

[![Пример снимка экрана экрана параметров печати](kitkat-images/printing.png)](kitkat-images/printing.png#lightbox)


> [!NOTE]
> Хотя интерфейсы API печати настроены для работы с Google Cloud Print по умолчанию, Android по-прежнему позволяет разработчикам подготавливать содержимое для печати с помощью новых интерфейсов API и передавать их другим приложениям для выполнения печати.



#### <a name="printing-html-content"></a>Печать HTML-содержимого

KitKat автоматически создает [`PrintDocumentAdapter`](xref:Android.Print.PrintDocumentAdapter) для веб-представления с помощью `WebView.CreatePrintDocumentAdapter`. Печать веб-содержимого — это координированные усилия [`WebViewClient`](xref:Android.Webkit.WebViewClient) , которые ожидают загрузки содержимого HTML и позволяют действию сообщить о том, что параметр печати доступен в меню Параметры, и действие, которое ожидает от пользователя выбора параметра печати и вызовов. `Print`в .`PrintManager` В этом разделе описывается базовая настройка, необходимая для печати содержимого на экране на экранах HTML.

Обратите внимание, что для загрузки и печати веб-содержимого требуется разрешение в Интернете:

[![Настройка разрешения Internet в параметрах приложения](kitkat-images/internet.png)](kitkat-images/internet.png#lightbox)

##### <a name="print-menu-item"></a>Пункт меню "Печать"

Параметр Печать, как правило, отображается в [меню параметров](https://developer.android.com/guide/topics/ui/menus.html#options-menu)действия.
Меню Параметры позволяет пользователям выполнять действия с действием. Он находится в правом верхнем углу экрана и выглядит следующим образом:

[![Пример снимка экрана пункта меню "Печать", отображаемый в правом верхнем углу экрана](kitkat-images/menu.png)](kitkat-images/menu.png#lightbox)


Дополнительные пункты меню можно определить в каталоге *меню*в разделе *ресурсы*. Приведенный ниже код определяет образец пункта меню [Print](xref:Android.Print.PrintManager):

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/menu_print"
        android:title="Print"
        android:showAsAction="never" />
</menu>
```

Взаимодействие с меню параметров в действии выполняется с помощью `OnCreateOptionsMenu` методов и. `OnOptionsItemSelected`
`OnCreateOptionsMenu`— Это место для добавления новых пунктов меню, таких как печать, из каталога «ресурсы *меню* ».
`OnOptionsItemSelected`прослушивает пользователя, выбрав пункт Печать в меню, и начинает печать:

```csharp
bool dataLoaded;

public override bool OnCreateOptionsMenu (IMenu menu)
{
    base.OnCreateOptionsMenu (menu);
    if (dataLoaded) {
        MenuInflater.Inflate (Resource.Menu.print, menu);
    }
    return true;
}

public override bool OnOptionsItemSelected (IMenuItem item)
{
    if (item.ItemId == Resource.Id.menu_print) {
        PrintPage ();
        return true;
    }
    return base.OnOptionsItemSelected (item);
}
```

Приведенный выше код также определяет переменную `dataLoaded` , которая называется для наблюдения за состоянием содержимого HTML. `WebViewClient` Для этой переменной задается значение true при загрузке всего содержимого, поэтому действие знает, как добавить пункт меню Печать в меню Параметры.

##### <a name="webviewclient"></a>WebViewClient

Задача в `WebViewClient` заключается в том, чтобы убедиться, что `WebView` данные в полностью загружены, прежде чем в меню появится параметр Печать, `OnPageFinished` который выполняется с помощью метода. `OnPageFinished`прослушивает веб-содержимое для завершения загрузки и сообщает действию повторно создать меню параметров с помощью `InvalidateOptionsMenu`:

```csharp
class MyWebViewClient : WebViewClient
{
    PrintHtmlActivity caller;

    public MyWebViewClient (PrintHtmlActivity caller)
    {
        this.caller = caller;
    }

    public override void OnPageFinished (WebView view, string url)
    {
        caller.dataLoaded = true;
        caller.InvalidateOptionsMenu ();
    }
}
```

`OnPageFinished`также задает `dataLoaded` `true`значение, поэтому `OnCreateOptionsMenu` может воссоздать меню с помощью параметра Печать на месте.

##### <a name="printmanager"></a>принтманажер

В следующем примере кода выводится содержимое `WebView`:

```csharp
void PrintPage ()
{
    PrintManager printManager = (PrintManager)GetSystemService (Context.PrintService);
    PrintDocumentAdapter printDocumentAdapter = myWebView.CreatePrintDocumentAdapter ();
    printManager.Print ("MyWebPage", printDocumentAdapter, null);
}
```

`Print`принимает в качестве аргументов: имя задания печати (в этом примере — «Мивебпаже»),[`PrintDocumentAdapter`](xref:Android.Print.PrintDocumentAdapter)
, который создает документ печати из содержимого и[`PrintAttributes`](xref:Android.Print.PrintAttributes)
(`null` в приведенном выше примере). Можно указать `PrintAttributes` для размещения содержимого на печатной странице, хотя атрибуты по умолчанию должны работать в большинстве случаев.

При `Print` вызове загружается пользовательский интерфейс печати, в котором перечислены параметры задания печати. Пользовательский интерфейс предоставляет пользователям возможность печати или сохранения содержимого HTML в формате PDF, как показано на снимках экрана ниже:

[![Снимок экрана Принстмлактивити, отображающий меню "Печать"](kitkat-images/print1.png)](kitkat-images/print1.png#lightbox)

[![Снимок экрана Принстмлактивити меню "Сохранить как PDF"](kitkat-images/print2.png)](kitkat-images/print2.png#lightbox)

<a name="hardware" />

## <a name="hardware"></a>Оборудование

KitKat добавляет несколько API для размещения новых функций устройства. Наиболее важным из них является Эмуляция карт на основе узла и Новая `SensorManager`.

### <a name="host-based-card-emulation-in-nfc"></a>Эмуляция карт на основе узла в NFC

Эмуляция карт на основе узла (ХЦЕ) позволяет приложениям работать как с NFC-картами или устройствами чтения NFC, не полагаясь на собственный защищенный элемент перевозчика. Перед настройкой ХЦЕ убедитесь, что ХЦЕ доступен на устройстве с `PackageManager.HasSystemFeature`помощью:

```csharp
bool hceSupport = PackageManager.HasSystemFeature(PackageManager.FeatureNfcHostCardEmulation);
```

Хце требует, чтобы функция хце и `Nfc` разрешение были зарегистрированы в `AndroidManifest.xml`приложении:

```xml
<uses-feature android:name="android.hardware.nfc.hce" />
```

[![Настройка разрешения NFC в параметрах приложения](kitkat-images/nfc.png)](kitkat-images/nfc.png#lightbox)

Для работы ХЦЕ должен быть способен работать в фоновом режиме, и он должен запускаться, когда пользователь выполняет NFC-транзакцию, даже если приложение, использующее ХЦЕ, не запущено. Это можно сделать, написав код ХЦЕ как `Service`. Служба хце реализует `HostApduService` интерфейс, который реализует следующие методы:

-  *Процесскоммандапду* — блок данных протокола приложения (категориях APDU), который передается между NFC-сканером и службой хце. Этот метод потребляет АДПУ из средства чтения и возвращает единицу данных в ответе.

-  *OnDeactivate* — отключается, `HostAdpuService` когда служба хце больше не взаимодействует с NFC-сканером.


Служба ХЦЕ также должна быть зарегистрирована в манифесте приложения и дополнена соответствующими разрешениями, фильтром намерения и метаданными. Ниже приведен пример `HostApduService` кода, зарегистрированного в манифесте Android с `Service` помощью атрибута (Дополнительные сведения об атрибутах см. в руководстве по [работе с манифестом Xamarin Working с Android](~/android/platform/android-manifest.md) ).

```csharp
[Service(Exported=true, Permission="android.permissions.BIND_NFC_SERVICE"),
    IntentFilter(new[] {"android.nfc.cardemulation.HOST_APDU_SERVICE"}),
    MetaData("android.nfc.cardemulation.host.apdu_service",
    Resource="@xml/hceservice")]

class HceService : HostApduService
{
    public override byte[] ProcessCommandApdu(byte[] apdu, Bundle extras)
    {
        ...
    }

    public override void OnDeactivated (DeactivationReason reason)
    {
        ...
    }
}
```

Приведенная выше служба предоставляет NFC-сканеру способ взаимодействия с приложением, но средство чтения NFC, тем не менее, не имеет возможности узнать, эмулирует ли эта служба NFC-карту, которую необходимо проверить. Чтобы помочь NFC-сканеру идентифицировать службу, можно назначить службе уникальный *идентификатор приложения (помощь)* . Мы указываем вспомогательное средство, а также другие метаданные о службе хце в файле ресурсов XML, зарегистрированном с `MetaData` помощью атрибута (см. пример кода выше). В этом файле ресурсов указывается один или несколько фильтров поддержки — строк уникальных идентификаторов в шестнадцатеричном формате, которые соответствуют вспомогательным средствам одного или нескольких устройств NFC-сканера:

```xml
<host-apdu-service xmlns:android="http://schemas.android.com/apk/res/android"
    android:description="@string/hce_service_description"
    android:requireDeviceUnlock="false"
    android:apduServiceBanner="@drawable/service_banner">
    <aid-group android:description="@string/aid_group_description"
                android:category="payment">
        <aid-filter android:name="1111111111111111"/>
        <aid-filter android:name="0123456789012345"/>
    </aid-group>
</host-apdu-service>
```

В дополнение к ВСПОМОГАТЕЛЬным фильтрам, файл ресурсов XML также содержит описание службы ХЦЕ, в котором указывается группа ВСПОМОГАТЕЛЬных приложений (платежное приложение и другое), а в случае платежного приложения — баннер 260x96 DP, отображаемый пользователю.

Настройка, описанная выше, предоставляет основные стандартные блоки для приложения, моделирующего NFC. Сам NFC требует выполнения еще нескольких действий и дальнейшего тестирования для настройки. Дополнительные сведения об эмуляции карт на основе узла см. на портале [документации Android](https://developer.android.com/guide/topics/connectivity/nfc/hce.html).
Дополнительные сведения об использовании NFC с Xamarin см. в примерах [XAMARIN NFC](https://github.com/xamarin/monodroid-samples/tree/master/NfcSample).

### <a name="sensors"></a>Датчиков

KitKat предоставляет доступ к датчикам устройства через [`SensorManager`](xref:Android.Hardware.SensorManager).
`SensorManager` Позволяет операционной системе запланировать доставку данных датчиков в приложение в пакетах, сохраняя время работы от аккумулятора.

KitKat также поставляется с двумя новыми типами датчиков для отслеживания действий пользователя. Они основаны на акселерометр и включают:

-  *Степдетектор* — приложение получает уведомления и пробуждении, когда пользователь выполняет шаг, а детектор предоставляет значение времени для выполнения шага.

-  *Степкаунтер* — отслеживает количество шагов, выполненных пользователем с момента регистрации датчика *до следующей перезагрузки устройства*.

На следующем снимке экрана показан счетчик Step в действии:

[![Снимок экрана приложения Сенсорсактивити, отображающего счетчик шагов](kitkat-images/stepcounter.png)](kitkat-images/stepcounter.png#lightbox)

Можно создать `SensorManager` , вызвав `GetSystemService(SensorService)` и `SensorManager`приведя результат в качестве. Чтобы использовать счетчик шагов, вызовите `GetDefaultSensor` `SensorManager`в. Вы можете зарегистрировать датчик и прослушать изменения в количестве шагов с помощью[`ISensorEventListener`](xref:Android.Hardware.ISensorEventListener)
, как показано в примере кода ниже:

```csharp
public class MainActivity : Activity, ISensorEventListener
{
    float count = 0;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        SetContentView (Resource.Layout.Main);

        SensorManager senMgr = (SensorManager) GetSystemService (SensorService);
        Sensor counter = senMgr.GetDefaultSensor (SensorType.StepCounter);
        if (counter != null) {
            senMgr.RegisterListener(this, counter, SensorDelay.Normal);
        }
    }

    public void OnAccuracyChanged (Sensor sensor, SensorStatus accuracy)
    {
        Log.Info ("SensorManager", "Sensor accuracy changed");
    }

    public void OnSensorChanged (SensorEvent e)
    {
        count = e.Values [0];
    }
}
```

`OnSensorChanged`вызывается, если количество шагов обновляется, пока приложение находится на переднем плане. Если приложение входит в фоновый режим или устройство находится в спящем `OnSensorChanged` режиме, не будет вызываться, однако действия будут учитываться до `UnregisterListener` вызова метода.

Помните, что *значение счетчика шагов является совокупным для всех приложений, регистрирующих датчик*. Это означает, что даже если удалить и переустановить приложение и инициализировать `count` переменную в 0 при запуске приложения, значение, сообщаемое датчиком, останется общим числом шагов, выполненных при регистрации датчика, будь то или другого приложения. Вы можете запретить приложению добавлять в счетчик шагов, вызвав метод `UnregisterListener` `SensorManager`в, как показано в следующем коде:

```csharp
protected override void OnPause()
{
    base.OnPause ();
    senMgr.UnregisterListener(this);
}
```

При перезагрузке устройства число шагов сбрасывается в 0. Приложению потребуется дополнительный код, чтобы убедиться, что он сообщает о точности для приложения, независимо от других приложений, использующих датчик или состояние устройства.


> [!NOTE]
> Хотя API для обнаружения и инвентаризации шагов поставляется с KitKat, не все телефоны имеют датчик. Можно проверить, доступен `PackageManager.HasSystemFeature(PackageManager.FeatureSensorStepCounter);`ли датчик, или убедиться, что возвращенное `GetDefaultSensor` значение нет `null`.


<a name="developer_tools" />

## <a name="developer-tools"></a>Средства разработчика

### <a name="screen-recording"></a>Запись экрана

KitKat включает новые возможности записи экрана, чтобы разработчики могли записывать приложения в действии. Запись экрана доступна через клиент [Android Debug Bridge (ADB)](https://developer.android.com/tools/help/adb.html) , который можно скачать как часть пакет SDK для Android.

Чтобы записать экран, подключите устройство. Затем найдите пакет SDK для Android установку, перейдите в каталог **Platform-Tools** и запустите клиент **ADB** :

```shell
adb shell screenrecord /sdcard/screencast.mp4
```

Приведенная выше команда запишет 3-минутное видео по умолчанию при разрешении 4Mbps. Чтобы изменить длину, добавьте флаг *--time-limit* .
Чтобы изменить разрешение, добавьте флаг *--bit-rate* . Следующая команда запишет видео в течение минуты в 8Mbps:

```shell
adb shell screenrecord --bit-rate 8000000 --time-limit 60 /sdcard/screencast.mp4
```

Видео можно найти на устройстве. оно появится в коллекции после завершения записи.


## <a name="other-kitkat-additions"></a>Другие дополнения KitKat

В дополнение к описанным выше изменениям KitKat позволяет:

-  *В полноэкранном* режиме — KitKat представляет новый [режим погружения](https://developer.android.com/reference/android/view/View.html#setSystemUiVisibility(int)) для обзора содержимого, воспроизведения игр и выполнения других приложений, которые могут использовать преимущества полноэкранного режима работы.

-  *Настройка уведомлений* . получение дополнительных сведений о системных уведомлениях с помощью[`NotificationListenerService`](xref:Android.Service.Notification.NotificationListenerService)
   . Это позволяет представлять информацию по-другому в приложении.

-  *Зеркальные рисуемые ресурсы* . для создаваемых ресурсов[`autoMirrored`](https://developer.android.com/reference/android/R.attr.html#autoMirrored)
   атрибут, сообщающий системе о создании зеркальной версии для изображений, требующих зеркального отображения для макетов слева направо.

-  *Приостановить анимацию* — приостанавливать и возобновлять анимации, созданные с помощью[`Animator`](xref:Android.Animation.Animator)
   .

-  *Чтение динамически изменяемых частей текста* в пользовательском интерфейсе, динамически обновляемых с новым текстом в качестве "динамических регионов" с новым[`accessibilityLiveRegion`](https://developer.android.com/reference/android/R.attr.html#accessibilityLiveRegion)
   атрибут, поэтому новый текст будет автоматически прочитан в режиме специальных возможностей.

-  *Улучшение работы с аудио* — Следите за громкостью с помощью[`LoudnessEnhancer`](xref:Android.Media.Audiofx.LoudnessEnhancer)
   , найдите пиковый уровень и RMS для аудиопотока с помощью[`Visualizer`](xref:Android.Media.Audiofx.Visualizer.MeasurementModePeakRms)
   и получить сведения из [метки времени звука](xref:Android.Media.AudioTimestamp) , чтобы помочь при синхронизации аудио-видео.

-  *ContentResolver синхронизации с настраиваемым интервалом* — KitKat добавляет некоторую вариативность к времени выполнения запроса на синхронизацию. Синхронизируйте `ContentResolver` объект в настраиваемое время или интервал `ContentResolver.RequestSync` , вызвав метод `SyncRequest`и передав в него.

-  *Различие контроллеров* . в KitKat контроллерам назначаются уникальные целочисленные идентификаторы, доступ к которым можно получить через `ControllerNumber` свойство устройства. Это упрощает разделение игроков в игре.

-  *Удаленное управление* . с несколькими изменениями на стороне оборудования и программного обеспечения KitKat позволяет превратить устройство с инфракрасным приемопередатчиком в пульт дистанционного управления с помощью `ConsumerIrService`и взаимодействовать с периферийными устройствами с помощью нового[`RemoteController`](xref:Android.Media.RemoteController)
   Интерфейсы.

Дополнительные сведения об изменениях, описанных выше, см. в обзоре [API-интерфейсов Google Android 4,4](https://developer.android.com/about/versions/android-4.4.html) .


## <a name="summary"></a>Сводка

В этой статье представлены некоторые новые API, доступные в Android 4,4 (API уровня 19), и рекомендации по переходу приложения на KitKat. Внесены изменения в интерфейсы API, влияющие на взаимодействие с пользователем, включая *платформу перехода* и новые параметры *.* Далее мы предоставили платформу и `DocumentsProvider` класс для *доступа к хранилищу* , а также новые *API-интерфейсы для печати*. Исследование *карт на основе NFC* и работа с датчиками с *низким*энергопотреблением, в том числе два новых датчика для отслеживания действий пользователя. Наконец, он демонстрирует запись демонстраций приложений в реальном времени с помощью *записи экрана*и предоставляет подробный список изменений и дополнений API KitKat.


## <a name="related-links"></a>Связанные ссылки

- [Пример KitKat](https://developer.xamarin.com/samples/monodroid/KitKat/)
- [API-интерфейсы Android 4,4](https://developer.android.com/about/versions/android-4.4.html)
- [KitKat Android](https://developer.android.com/about/versions/kitkat.html)
