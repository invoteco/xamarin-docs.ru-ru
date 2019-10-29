---
title: Android 9 Pie
description: Как приступить к разработке приложений для секторов Android 9 с помощью Xamarin. Android.
ms.prod: xamarin
ms.assetid: 6575DD32-9DC8-44E6-85EF-1F8BD07D3780
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/21/2018
ms.openlocfilehash: 0105b43116df697bc6688becb77298c236dfa601
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73019875"
---
# <a name="android-pie-features"></a>Возможности для круговой диаграммы Android

_Как приступить к разработке приложений для секторов Android 9 с помощью Xamarin. Android._

[Сектор Android 9](https://developer.android.com/about/versions/pie/) теперь доступен в Google. В этом выпуске предоставляется ряд новых функций и API, и многие из них необходимы для использования новых возможностей оборудования на последних устройствах Android.

![Hero изображение для круговой диаграммы Android](pie-images/01-android-p-logo.png)

Эта статья содержит сведения, которые помогут вам приступить к разработке приложений Xamarin. Android на круговой диаграмме Android. В нем объясняется, как установить необходимые обновления, настроить пакет SDK и подготовить эмулятор или устройство для тестирования. Кроме того, здесь приводится описание новых функций на круговой диаграмме Android, а также пример исходного кода, демонстрирующий использование некоторых ключевых функций Android.

Xamarin. Android 9,0 обеспечивает поддержку для круговой диаграммы Android. Дополнительные сведения о поддержке Xamarin. Android для круговой диаграммы Android см. в заметках о выпуске [Android P Developer Preview 3](https://docs.microsoft.com/xamarin/android/release-notes/9/9.0/#android-p-dp1) .

## <a name="requirements"></a>Требования

Следующий список необходим для использования функций круговой диаграммы Android в приложениях на основе Xamarin:

- Рекомендуется **Visual studio** &ndash; visual Studio 2019.
    Если вы используете Visual Studio 2017, в Windows Update до Visual Studio 2017 версии 15,8 или более поздней. В macOS обновите Visual Studio 2017 для Mac версии 7,6 или более поздней.

- **Xamarin. android** &ndash; Xamarin. Android 9.0.0.17 или более поздней версии должен быть установлен вместе с Visual Studio (Xamarin. Android автоматически устанавливается в рамках рабочей нагрузки **для разработки мобильных приложений на .NET** ).

- Для **Java Developer Kit** &ndash; разработке Xamarin Android 9,0 требуется [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (или вы можете попробовать предварительную версию дистрибутива корпорации Майкрософт [OpenJDK](~/android/get-started/installation/openjdk.md)). JDK8 автоматически устанавливается в составе рабочей нагрузки **.NET для разработки мобильных приложений** .

- **Пакет SDK для Android** &ndash; пакет SDK для Android API 28 или более поздней версии необходимо установить через диспетчер пакет SDK для Android.

## <a name="getting-started"></a>Начало работы

Чтобы приступить к разработке приложений для круговой системы Android с помощью Xamarin. Android, необходимо загрузить и установить последние версии средств и пакетов SDK, прежде чем можно будет создать первый круговой проект Android:

1. Рекомендуется использовать Visual Studio 2019. Если вы используете Visual Studio 2017, обновите [Visual studio 2017 до версии 15,8](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes) или более поздней. Если вы используете Visual Studio для Mac, обновите [Visual Studio 2017 для Mac версии 7,6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes) или более поздней.

2. Установка пакетов и средств для **круговой диаграммы Android (API 28)** с помощью диспетчера пакетов SDK.

3. Создайте новый проект Xamarin. Android, предназначенный для **Android 9,0**.

4. Настройка эмулятора или устройства для тестирования круговых приложений Android.

Каждое из этих действий описано в следующих разделах:

### <a name="update-visual-studio"></a>Обновление Visual Studio

Visual Studio 2019 рекомендуется использовать для создания приложений для круговой диаграммы Android с помощью Xamarin.

Если вы используете Visual Studio 2017, обновите Visual Studio 2017 до версии 15,8 или более поздней (инструкции см. в статье [обновление до последней версии Visual studio 2017](https://docs.microsoft.com/visualstudio/install/update-visual-studio)). В macOS обновление до Visual Studio 2017 для Mac 7,6 или более поздней версии (инструкции см. в разделе [Установка и установка Visual Studio для Mac](https://docs.microsoft.com/visualstudio/mac/installation)).

### <a name="install-the-android-sdk"></a>Установка пакет SDK для Android

Чтобы создать проект с помощью Xamarin. Android 9,0, необходимо сначала использовать диспетчер пакет SDK для Android, чтобы установить платформу SDK для **секторов Android (уровень API 28)** или более поздней версии.

1. Запустите диспетчер пакетов SDK. В Visual Studio щелкните **сервис > Android > пакет SDK для Android Manager**. В Visual Studio для Mac выберите **сервис > диспетчер пакетов SDK**.

2. В правом нижнем углу щелкните значок шестеренки и выберите **репозиторий > Google (не поддерживается)** :

    [![настройки репозитория в Google](pie-images/vs/set-repo-sml.png)](pie-images/vs/set-repo.png#lightbox)

3. Установите пакеты SDK для **секторов Android** , которые перечислены как **пакет SDK для Android Платформа 28** на вкладке " **платформы** " (Дополнительные сведения об использовании диспетчера пакетов SDK см. в разделе [пакет SDK для Android Setup](~/android/get-started/installation/android-sdk.md)):

    [![установка многокруговых пакетов Android](pie-images/vs/sdk-manager-sml.png)](pie-images/vs/sdk-manager.png#lightbox)

4. Если вы используете эмулятор, создайте виртуальное устройство, которое поддерживает **уровень API 28**. Дополнительные сведения о создании виртуальных устройств см. [в статье Управление виртуальными устройствами с помощью Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md).

### <a name="start-a-xamarinandroid-project"></a>Запуск проекта Xamarin. Android

Создайте новый проект Xamarin. Android. Если вы не знакомы с разработкой Android с помощью Xamarin, ознакомьтесь со статьей [Hello, Android,](~/android/get-started/hello-android/index.md) чтобы узнать о создании проектов Xamarin. Android.

При создании проекта Android необходимо настроить параметры версии для целевого устройства Android 9,0 или более поздней версии. Например, чтобы выбрать для проекта круговую диаграмму Android, необходимо настроить уровень API Android для вашего проекта на **android 9,0** (API 28). Рекомендуется также задать для целевой платформы уровень API 28 или более поздней версии. Дополнительные сведения о настройке уровней API Android см. в разделе [Общие сведения об уровнях API Android](~/android/app-fundamentals/android-api-levels.md).

### <a name="configure-a-device-or-emulator"></a>Настройка устройства или эмулятора

При использовании физического устройства, например хранилища или пикселя, можно обновить устройство на круговую диаграмму Android, следуя инструкциям в разделе [образы фабрики для хранилища и точечных устройств](https://developers.google.com/android/images).

Если вы используете эмулятор, создайте виртуальное устройство для уровня API 28 и выберите образ на основе x86. Сведения об использовании Android Device Manager для создания виртуальных устройств и управления ими см. в статье [Управление виртуальными устройствами с помощью Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md).
Сведения об использовании эмулятора Android для тестирования и отладки см. в разделе [Отладка на Android Emulator](~/android/deploy-test/debugging/debug-on-emulator.md).

## <a name="new-features"></a>Новые функции

Круговая диаграмма Android предоставляет ряд новых функций. Некоторые из этих новых функций предназначены для использования новых возможностей оборудования, предлагаемых последними устройствами Android, а другие предназначены для дальнейшего улучшения пользовательского интерфейса Android.

- **Отображать поддержку отреза** &ndash; предоставляет интерфейсы API для поиска расположения и формы _отреза_ в верхней части экрана на новых устройствах Android.

- **Улучшения уведомлений** &ndash; уведомления теперь могут отображать изображения, а новый класс `Person` используется для упрощения участников диалога.

- **Позиционирование** в предметной области &ndash; поддержки платформы для протокола приема-передачи WiFi, что позволяет приложениям использовать устройства Wi-Fi для навигации по параметрам.

- **Поддержка нескольких камер** &ndash; предоставляет возможность одновременного доступа к потокам из нескольких физических камер (например, двойных и двойных).

В следующих разделах описаны эти функции и приведены краткие примеры кода, которые помогут приступить к их использованию в приложении.

### <a name="display-cutout-support"></a>Отображать поддержку отреза

Многие новые устройства Android с пограничными экранами имеют *отрезки экранов* (или «деление») в верхней части экрана для камеры и докладчика.
На следующем снимке экрана показан пример эмулятора:

[![эмулятора Android, моделирующий отрезки](pie-images/02-example-cutout-sml.png)](pie-images/02-example-cutout.png#lightbox)

Чтобы управлять отображением содержимого в окне приложения на устройствах с вырезой экрана, круговая диаграмма Android добавила новый атрибут макета окна [лайаутиндисплайкутаутмоде](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#layoutInDisplayCutoutMode) . Этому атрибуту можно присвоить одно из следующих значений:

- [Лайаутиндисплайкутаутмоденевер](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_NEVER) &ndash; окно никогда не может перекрываться с областью отреза.

- [Лайаутиндисплайкутаутмодешортеджес](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_SHORT_EDGES) &ndash; окно разрешается расширять на область отреза, но только на короткие края экрана. 

- [Лайаутиндисплайкутаутмодедефаулт](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_DEFAULT) &ndash; окно может быть расширено до области отреза, если она содержится в системной панели.

Например, чтобы предотвратить перекрытие окна приложения с областью отреза, установите для параметра режим отреза макета значение *никогда*: 

```csharp
Window.Attributes.LayoutInDisplayCutoutMode =
    Android.Views.LayoutInDisplayCutoutMode.Never;
```

В следующих примерах приведены примеры этих режимов отреза. Первый снимок экрана слева находится в приложении в режиме, отличном от полноэкранного. На снимке экрана центра приложение переходит в полноэкранный режим с `LayoutInDisplayCutoutMode` установленным в значение `LayoutInDisplayCutoutModeShortEdges`. Обратите внимание, что белый фон приложения распространяется на область «отрезки экрана»:

[![примере отображение режимов отреза в эмуляторе](pie-images/03-cutout-modes-sml.png)](pie-images/03-cutout-modes.png#lightbox)

На завершающем снимке экрана (расположенном выше справа) `LayoutInDisplayCutoutMode` имеет значение `LayoutInDisplayCutoutModeShortNever` перед переходом в полноэкранный режим.
Обратите внимание, что белый фон приложения не разрешается расширять в область "отрезки экрана".

Если вам нужны более подробные сведения о области отреза на устройстве, можно использовать новый класс [дисплайкутаут](https://developer.android.com/reference/android/view/DisplayCutout.html) . `DisplayCutout` представляет область экрана, которую нельзя использовать для вывода содержимого. Эти сведения можно использовать для получения расположения и формы отреза, чтобы приложение не предпринимало попытки отобразить содержимое в этой нефункциональной области.

Дополнительные сведения о новых функциях отреза в Android P см. в разделе [Поддержка отображаемых отверстий](https://developer.android.com/about/versions/pie/android-9.0#cutout).

### <a name="notifications-enhancements"></a>Усовершенствования уведомлений

Круговая диаграмма Android предоставляет следующие усовершенствования для улучшения работы с обменом сообщениями:

- Каналы уведомления (появившиеся в [Android Oreo](~/android/platform/oreo.md)) теперь поддерживают блокировку групп каналов.

- Система уведомлений имеет три новые категории "не беспокоить" (приоритетные оповещения, системные звуки и источники мультимедиа). Кроме того, существует семь новых режимов «не беспокоить», которые можно использовать для подавления визуальных прерываний (таких как эмблемы, Индикаторы уведомлений, отображение строк состояния и запуск полноэкранных действий).

- Для представления отправителя сообщения был добавлен новый класс [Person](https://developer.android.com/reference/android/app/Person.html) . Использование этого класса помогает оптимизировать отрисовку каждого уведомления путем определения людей, участвующих в беседе (включая аватары и URI).

- Теперь уведомления могут отображать изображения. 

В следующем примере показано, как использовать новые API для создания уведомления, которое содержит изображение. На следующих снимках экрана публикуется текстовое уведомление, за которым следует уведомление с внедренным изображением. При разворачивании уведомлений (как показано справа) отображается текст первого уведомления и изображение, внедренное во второе уведомление, увеличивается.

[![пример уведомления с изображением](pie-images/04-example-notifications-sml.png)](pie-images/04-example-notifications.png#lightbox)

В следующем примере показано, как включить изображение в многосегментное уведомление Android и демонстрируется использование нового класса `Person`:

1. Создайте объект `Person`, представляющий отправителя. Например, имя и значок отправителя включены в `fromPerson`:

    ```csharp
    Icon senderIcon = Icon.CreateWithResource(this, Resource.Drawable.sender_icon);
    Person fromPerson = new Person.Builder()
        .SetIcon(senderIcon)
        .SetName("Mark Sender")
        .Build();
    ```

2. Создайте `Notification.MessagingStyle.Message`, содержащий образ для отправки, передав его в новый метод [Notification. мессагингстиле. Message. SetData](https://developer.android.com/reference/android/app/Notification.MessagingStyle.Message.html#setData%28java.lang.String,%20android.net.Uri) .
   Пример:

    ```csharp
    Uri imageUri = Uri.Parse("android.resource://com.xamarin.pminidemo/drawable/example_image");
    Notification.MessagingStyle.Message message = new Notification.MessagingStyle
            .Message("Here's a picture of where I'm currently standing", 0, fromPerson)
            .SetData("image/", imageUri);
    ```

3. Добавьте сообщение в объект `Notification.MessagingStyle`. Пример:

    ```csharp
    Notification.MessagingStyle style = new Notification.MessagingStyle(fromPerson)
            .AddMessage(message);
    ```

4. Вставьте этот стиль в построитель уведомлений. Пример:

    ```csharp
    builder = new Notification.Builder(this, MY_CHANNEL)
        .SetContentTitle("Tour of the Colosseum")
        .SetContentText("I'm standing right here!")
        .SetSmallIcon(Resource.Mipmap.ic_notification)
        .SetStyle(style)
        .SetChannelId(MY_CHANNEL);
    ```

5. Опубликуйте уведомление. Пример:

    ```csharp
    const int notificationId = 1000;
    notificationManager.Notify(notificationId, builder.Build());
    ```

Дополнительные сведения о создании уведомлений см. в разделе [локальные уведомления](~/android/app-fundamentals/notifications/local-notifications.md).

### <a name="indoor-positioning"></a>Позиционирование внутри

Круговая диаграмма Android обеспечивает поддержку IEEE 802.11 MC (также известного как время приема-передачи _WiFi_ или _RTT_), что позволяет приложениям обнаруживать расстояние до одной или нескольких точек доступа Wi-Fi. Используя эти сведения, приложение может использовать преимущества *позиционирования* с точностью до двух метров. На устройствах Android, обеспечивающих аппаратную поддержку для IEEE 801.11 MC, приложение может предлагать функции навигации, такие как управление на основе местоположения для смарт-устройств или последующее включение инструкций в хранилище:

[![пример навигации с помощью RTT Wi-Fi](pie-images/05-wifi-rtt-sml.png)](pie-images/05-wifi-rtt.png#lightbox)

Новый класс [вифирттманажер](https://developer.android.com/reference/android/net/wifi/rtt/WifiRttManager) и несколько вспомогательных классов предоставляют средства для измерения расстояния до устройств Wi-Fi. Дополнительные сведения о интерфейсах API позиционирования для размещения, появившихся в Android P, см. в разделе [Android .NET. Wi. RTT](https://developer.android.com/reference/android/net/wifi/rtt/package-summary).

### <a name="multi-camera-support"></a>Поддержка нескольких камер

Многие новые устройства Android имеют Сдвоенные или двойные камеры, которые полезны для таких функций, как стерео, улучшенные визуальные эффекты и улучшенные возможности масштабирования. В Android P появился новый API с [несколькими камерами](https://developer.android.com/about/versions/pie/android-9.0#camera) , позволяющий приложению использовать *логическую камеру* (или *логическую*камеру), которая поддерживается двумя или более физическими камерами.
Чтобы определить, поддерживает ли устройство логическую камеру, можно просмотреть возможности каждой камеры на устройстве, чтобы узнать, поддерживает ли он [рекуеставаилаблекапабилитиеслогикалмултикамера](https://developer.android.com/reference/android/hardware/camera2/CameraMetadata#REQUEST_AVAILABLE_CAPABILITIES_LOGICAL_MULTI_CAMERA).

Круговая диаграмма Android также включает новый класс [сессионконфигуратион](https://developer.android.com/reference/android/hardware/camera2/params/SessionConfiguration.html) , который можно использовать для сокращения задержек во время первоначального захвата и устранения необходимости запуска и запуска потока камеры.

Дополнительные сведения о поддержке нескольких камер в Android P см. в разделе [Поддержка нескольких камер и обновление камеры](https://developer.android.com/about/versions/pie/android-9.0#camera).

### <a name="other-features"></a>Другие возможности

Кроме того, на круговой диаграмме Android поддерживаются еще несколько новых функций:

- Новый класс [аниматедимажедравабле](https://developer.android.com/reference/android/graphics/drawable/AnimatedImageDrawable.html) , который можно использовать для рисования и отображения анимированных изображений.

- Новый класс [имажедекодер](https://developer.android.com/reference/android/graphics/ImageDecoder.html) , который заменяет `BitmapFactory`. `ImageDecoder` можно использовать для декодирования `AnimatedImageDrawable`.

- Поддержка изображений HDR (с высоким динамическим диапазоном) видео и ХЕИФ (формат файлов изображений с высоким уровнем эффективности).

- [Жобсчедулер](https://developer.android.com/reference/android/app/job/JobScheduler.html) был дополнен к более интеллектуальной обработке связанных с сетью заданий. Новый метод WebMethod класса [жобпараметерс](https://developer.android.com/reference/android/app/job/JobParameters) возвращает [лучшую сеть для](https://developer.android.com/reference/android/app/job/JobParameters#getNetwork%28%29) выполнения любых сетевых запросов для данного задания.

Дополнительные сведения о последних функциях для круговой диаграммы Android см. в разделе [функции и API-интерфейсы Android 9](https://developer.android.com/about/versions/pie/android-9.0).

## <a name="behavior-changes"></a>Изменения в поведении

Если для целевой версии Android задан уровень API 28, то существует несколько изменений платформы, которые могут повлиять на поведение приложения, даже если вы не реализуете новые функции, описанные выше. Ниже приведен краткий обзор этих изменений.

- Приложения должны теперь запрашивать разрешение переднего плана перед использованием служб переднего плана.

- Если приложение содержит более одного процесса, оно не может совместно использовать один каталог данных [WebView](xref:Android.Webkit.WebView) в процессах.

- Прямой доступ к каталогу данных другого приложения по пути больше не разрешен.

Дополнительные сведения об изменениях в поведении для приложений, предназначенных для Android P, см. в разделе [изменения в поведении](https://developer.android.com/about/versions/pie/android-9.0-changes-all#p-apps).

## <a name="sample-code"></a>Пример кода

[Андроидпминидемо](https://github.com/xamarin/monodroid-samples/tree/master/android-p/AndroidPMiniDemo) — это пример приложения Xamarin. Android для Android, в котором показано, как устанавливать режимы отображения отреза, как использовать новый класс `Person` и как отправить уведомление, включающее изображение.

## <a name="summary"></a>Сводка

В этой статье представлена круговая диаграмма под управлением Android и объясняется, как установить и настроить новейшие средства и пакеты для разработки Xamarin. Android на круговой диаграмме Android. В нем предоставлен обзор основных функций, доступных на круговой диаграмме Android, а также пример исходного кода для некоторых из этих функций.
В нем содержатся ссылки на разделы документации по API и разработчиков Android, которые помогут вам приступить к созданию приложений для круговой диаграммы Android. Он также выделяет наиболее важные изменения в поведении на круги Android, которые могут повлиять на существующие приложения.

## <a name="related-links"></a>Связанные ссылки

- [Круговая диаграмма Android 9](https://developer.android.com/about/versions/pie/)
