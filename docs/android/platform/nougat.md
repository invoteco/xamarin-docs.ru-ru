---
title: Функции Nougat
description: Как приступить к работе с Xamarin. Android для разработки приложений для Android Nougat.
ms.prod: xamarin
ms.assetid: 5C74ABE2-C862-4ED0-8EA5-C7FEE5251D4B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/02/2018
ms.openlocfilehash: 6274c75abf229268070d495ced662724f5c16627
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027091"
---
# <a name="nougat-features"></a>Функции Nougat

_Как приступить к работе с Xamarin. Android для разработки приложений для Android Nougat._

В этой статье представлен обзор функций Android Nougat, описывается подготовка Xamarin. Android для Android Nougat Development и приводятся ссылки на примеры приложений, иллюстрирующие использование функций Android Nougat в Приложения Xamarin. Android.

## <a name="overview"></a>Обзор

[Android Nougat](https://developer.android.com/about/versions/nougat/android-7.0.html) — это подписчик Google 6,0 Marshmallow. Xamarin. Android обеспечивает поддержку **привязок Android 7. x** в Xamarin Android 7,0 и более поздних версиях. Android Nougat добавляет множество новых API для функций Nougat, описанных ниже. Эти API-интерфейсы доступны для приложений Xamarin. Android при использовании Xamarin. Android 7,0.

[![Hero образы планшетов Android и телефонов с Android Nougat](nougat-images/android-n-hero-sml.png)](nougat-images/android-n-hero.png#lightbox)

Дополнительные сведения о API-интерфейсах Android 7. x см. в разделе [android 7,1 для разработчиков](https://developer.android.com/preview/api-overview.html).
Список известных проблем Xamarin. Android 7,0 см. в [заметках о выпуске](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_7/xamarin.android_7.0/index.md).

Android Nougat предоставляет множество новых функций, представляющих интерес для разработчиков Xamarin. Android. Эти функции включают перечисленные ниже.

- **Поддержка нескольких окон** &ndash; это улучшение позволяет пользователям одновременно открывать два приложения на экране.

- **Улучшения уведомлений** &ndash;. переработанная система уведомлений в Android Nougat включает функцию *прямого ответа* , позволяющую пользователям быстро реагировать на текстовые сообщения непосредственно из пользовательского интерфейса уведомления. Кроме того, если приложение создает уведомления для полученных сообщений, Новая функция *Объединенных уведомлений* может объединять уведомления в одну группу при получении нескольких сообщений.

- **Заставка данных** &ndash; эта функция — это новая системная служба, которая помогает сократить использование сотовых данных приложениями. Он дает пользователям возможность управлять использованием сотовых данных приложениями.

Кроме того, Android Nougat предлагает разработчикам приложений множество других усовершенствований, таких как новая функция настройки безопасности сети, дозе в дороге, аттестацию ключей, новые API быстрых настроек, поддержку нескольких национальных настроек, API ICU4J, WebView усовершенствований, доступ к функциям языка Java 8, доступу к каталогам с заданной областью, API пользовательского указателя, поддержке платформы VR, виртуальным файлам и оптимизации фоновой обработки.

В этой статье объясняется, как приступить к созданию приложений с помощью Android Nougat, чтобы испытать новые возможности и спланировать миграцию или работу функций, чтобы ориентироваться на новую платформу Android Nougat.

## <a name="requirements"></a>Требования

Для использования новых функций Android Nougat в приложениях на основе Xamarin необходимо следующее:

- **Visual Studio или Visual Studio для Mac** &ndash; если вы используете Visual Studio, требуется версия 4.2.0.628 или более поздняя инструменты Visual Studio для Xamarin. Если вы используете Visual Studio для Mac, требуется версия 6.1.0 или более поздняя Visual Studio для Mac.

- **Xamarin. android** &ndash; Xamarin. Android 7,0 или более поздней версии должен быть установлен и настроен с помощью Visual Studio или Visual Studio для Mac.

- **Пакет SDK для Android** -пакет SDK для Android 7,0 (API 24) или более поздней версии необходимо установить с помощью диспетчера пакет SDK для Android.

- Для **Java Developer Kit** &ndash; разработке Xamarin Android 7,0 требуется [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) или более поздней версии при разработке для API уровня 24 или выше (JDK 8 также поддерживает уровни API, предшествующие 24). 64-разрядная версия JDK 8 необходима, если используются пользовательские элементы управления или средства просмотра форм.

> [!IMPORTANT]
> Xamarin.Android не поддерживает пакет JDK 9.

Обратите внимание, что приложения должны быть перестроены с помощью Xamarin C6SR4 или более поздней версии для надежной работы с Android Nougat. Так как Android Nougat может ссылаться только на [собственные библиотеки NDK](https://developer.android.com/about/versions/nougat/android-7.0-changes.html), существующие приложения, использующие такие библиотеки, как **Mono. Data. SQLite. dll** , могут аварийно завершаться при запуске в Nougat Android, если они не были правильно перестроены.

## <a name="getting-started"></a>Начало работы

Чтобы приступить к работе с Android Nougat с Xamarin. Android, перед созданием проекта Nougat для Android необходимо загрузить и установить последние версии средств и пакетов SDK:

1. Установите последние обновления Xamarin. Android из Xamarin.

2. Установите пакеты и инструменты **Android 7,0 (API 24)** или более поздней версии.

3. Создайте новый проект Xamarin. Android, предназначенный для Android Nougat.

4. Настройте эмулятор или устройство для Android Nougat.

Каждое из этих действий описано в следующих разделах:

### <a name="install-xamarin-updates"></a>Установка обновлений Xamarin

Чтобы добавить поддержку Xamarin для Android Nougat, измените канал обновления в Visual Studio или Visual Studio для Mac на стабильный канал и примените последние обновления. Если вам также нужны функции, доступные только в канале Alpha или Beta, можно переключиться на канал Alpha или Beta (каналы Alpha и Beta также обеспечивают поддержку Android 7. x). Сведения о том, как изменить канал обновлений (releases), см. [в разделе Изменение канала обновлений](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel).

### <a name="install-the-android-sdk"></a>Установка пакет SDK для Android

Чтобы создать проект с Xamarin Android 7,0, необходимо сначала использовать диспетчер пакет SDK для Android, чтобы установить **пакет SDK для Android N (API 24)** или более поздней версии. Также необходимо установить последнюю версию **Android SDK Tools**:

1. Запустите диспетчер пакет SDK для Android (в Visual Studio для Mac используйте **средства > открыть пакет SDK для Android Manager&hellip;** ; в Visual Studio используйте **инструменты > Android > пакет SDK для Android Manager**).

2. Установите **Android 7,0 (API 24)** или более поздней версии.

    [![выбора пакетов Android 7,0 в диспетчере пакет SDK для Android](nougat-images/preview-packages.png)](nougat-images/preview-packages.png#lightbox)

3. Установите новейшие средства пакет SDK для Android:

    [![выборе последних средств пакет SDK для Android в диспетчере пакет SDK для Android](nougat-images/preview-tools.png)](nougat-images/preview-tools.png#lightbox)

    Необходимо установить Android SDK Tools редакции 25.2.2 или более поздней версии, пакет SDK для Android инструменты платформы 24.0.3 или более поздней версии и пакет SDK для Android средства сборки 24.0.2 или более поздней версии.

4. Убедитесь, что **расположение пакета Java Development Kit** настроено для JDK 1,8:

    [![настройке пути JDK 8 в разделе Сервис параметры](nougat-images/use-jdk-1.8.png)](nougat-images/use-jdk-1.8.png#lightbox)

    Чтобы просмотреть этот параметр в Visual Studio, щелкните **сервис > параметры > параметры Xamarin > Android**. В Visual Studio для Mac щелкните **предпочтения > проекты > расположения пакетов SDK > Android**.

### <a name="start-a-xamarinandroid-project"></a>Запуск проекта Xamarin. Android

Создайте новый проект Xamarin. Android. Если вы не знакомы с разработкой Android с помощью Xamarin, ознакомьтесь со статьей [Hello, Android,](~/android/get-started/hello-android/index.md) чтобы узнать о создании проектов Xamarin. Android.

При создании проекта Android необходимо настроить параметры версии для целевого устройства Android 7,0 или более поздней версии. Например, чтобы выбрать целевой проект для Android 7,0, необходимо настроить уровень API Android для проекта на **android 7,0 (API 24-Nougat)** . Рекомендуется задать для целевой платформы уровень API 24 или более поздней версии. Дополнительные сведения о настройке уровней уровня API Android см. в разделе [Общие сведения об уровнях API Android](~/android/app-fundamentals/android-api-levels.md).

> [!NOTE]
> В настоящее время для развертывания приложения на устройствах и эмуляторах Android Nougat необходимо задать для **минимальной версии Android версию** **Android 7,0 (API 24-Nougat)** .

### <a name="configure-an-emulator-or-device"></a>Настройка эмулятора или устройства

Если вы используете эмулятор, запустите Android AVD Manager и создайте новое устройство, используя следующие параметры:

- Устройство: 5X хранилища, хранилища 6, хранилища 6P, проигрывателя хранилища, хранилища 9 или пиксель C.
- Цель: Android 7,0 — API уровня 24
- ABI: x86 или x86\_64

Например, это виртуальное устройство настроено для эмуляции хранилища 6:

[![настройке AVD с помощью устройства хранилища 7,0, целевого объекта Android и процессора Intel Atom x86/ABI](nougat-images/android-n-avd.png)](nougat-images/android-n-avd.png#lightbox)

При использовании физического устройства, такого как 5X хранилища, 6 или 9, можно либо обновить устройство с помощью автоматического обновления через воздушный (OTA), либо загрузить образ системы и напрямую включить в устройство. Дополнительные сведения о ручном обновлении устройства до Android Nougat см. в разделе [образы OTA для устройств хранилища](https://developers.google.com/android/nexus/ota).

Обратите внимание, что устройства с хранилищами 5 не поддерживаются в Android Nougat.

## <a name="new-features"></a>Новые функции

В Android Nougat введено множество новых функций и возможностей, таких как поддержка нескольких окон, усовершенствование уведомлений и экономия данных. В следующих разделах описаны эти функции и приведены ссылки, которые помогут приступить к их использованию в приложении.

### <a name="multi-window-mode"></a>Режим с несколькими окнами

Многооконный режим позволяет пользователям одновременно открывать два приложения с полной поддержкой многозадачности. Эти приложения могут работать параллельно (альбомная) или одна выше (Книжная) в режиме разделения экрана.
Пользователи могут перетаскивать разделитель между приложениями, чтобы изменить их размер, а также вырезать и вставлять содержимое между приложениями. Когда два приложения представлены в режиме с несколькими окнами, выбранное действие продолжает выполняться, пока невыбранное действие приостанавливается, но остается видимым. Режим с несколькими окнами не изменяет жизненный цикл действия Android.

[![примеров приложений, выполняемых в многооконном режиме в книжной и альбомной ориентации](nougat-images/multi-window-mode.png)](nougat-images/multi-window-mode.png#lightbox)

Вы можете настроить, как действия приложения Xamarin. Android поддерживают многооконный режим. Например, можно настроить атрибуты, устанавливающие минимальный размер и высоту и ширину по умолчанию приложения в многооконном режиме. Вы можете использовать новое свойство `Activity.IsInMultiWindowMode`, чтобы определить, находится ли ваше действие в многооконном режиме. Пример:

```csharp
if (!IsInMultiWindowMode) {
    multiDisabledMessage.Visibility = ViewStates.Visible;
} else {
    multiDisabledMessage.Visibility = ViewStates.Gone;
}
```

Пример приложения [мултивиндовплайграунд](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-multiwindowplayground) включает в C# себя код, демонстрирующий использование преимуществ нескольких пользовательских интерфейсов окон в приложении.

Дополнительные сведения о многооконном режиме см. в разделе [Поддержка нескольких окон](https://developer.android.com/guide/topics/ui/multi-window.html).

### <a name="enhanced-notifications"></a>Улучшенные уведомления

В Android Nougat введена переработанная система уведомлений. В нем реализована новая функция прямого ответа, позволяющая пользователям быстро отвечать на уведомления входящих текстовых сообщений непосредственно в пользовательском интерфейсе уведомления. Начиная с Android 7,0, сообщения уведомления могут быть объединены в единую группу при получении нескольких сообщений. Кроме того, разработчики могут настраивать представления уведомлений, использовать украшения системы в уведомлениях и использовать новые шаблоны уведомлений при создании уведомлений.

#### <a name="direct-reply"></a>Прямой ответ

Когда пользователь получает уведомление для входящего сообщения, Android Nougat делает возможным ответ на сообщение в уведомлении (вместо того, чтобы открывать приложение для обмена сообщениями для отправки ответа).
Эта функция встроенного ответа позволяет пользователям быстро реагировать на SMS или текстовые сообщения непосредственно в интерфейсе уведомления:

[![снимок экрана уведомления с полем встроенного прямого ответа](nougat-images/notifications-inline-reply-sml.png)](nougat-images/notifications-inline-reply.png#lightbox)

Для поддержки этой функции в приложении необходимо добавить *встроенные действия ответа* в приложение через объект [ремотеинпут](xref:Android.App.RemoteInput) , чтобы пользователи могли ответить через текст непосредственно из пользовательского интерфейса уведомления.
Например, следующий код создает `RemoteInput` для получения текстового ввода, создает цель ожидания для действия ответа и создает действие Remote input Enabled.

```csharp
// Build a RemoteInput for receiving text input:
var remoteInput = new Android.Support.V4.App.RemoteInput.Builder (EXTRA_REMOTE_REPLY)
    .SetLabel (GetString (Resource.String.reply))
    .Build ();

// Build a Pending Intent for the reply action to trigger:
PendingIntent replyIntent = PendingIntent.GetBroadcast (ApplicationContext,
                                conversation.ConversationId,
                                GetMessageReplyIntent (conversation.ConversationId),
                                PendingIntentFlags.UpdateCurrent);

// Build an Android 7.0 compatible Remote Input enabled action:
NotificationCompat.Action actionReplyByRemoteInput = new NotificationCompat.Action.Builder (
    Resource.Drawable.notification_icon,
    GetString (Resource.String.reply),
    replyIntent).AddRemoteInput (remoteInput).Build ();
```

Это действие добавляется к уведомлению:

```csharp
// Create the notification:
NotificationCompat.Builder builder = new NotificationCompat.Builder (ApplicationContext)
   .SetSmallIcon (Resource.Drawable.notification_icon)
   ...
   .AddAction (actionReplyByRemoteInput);
```

Пример приложения [службы обмена сообщениями](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-messagingservice) включает в себя C# код, демонстрирующий способ расширения уведомлений с помощью объекта`RemoteInput`. Дополнительные сведения о добавлении встроенных действий по ответу в приложение для Android 7,0 или более поздней версии см. в разделе Android, [отвечающий на уведомления](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#direct) .

#### <a name="bundled-notifications"></a>Объединенные уведомления

Android Nougat может группировать сообщения уведомлений (например, по разделу сообщения) и отображать группу, а не каждое отдельное сообщение.
Эта функция *пакетных уведомлений* позволяет пользователям отклонять или архивировать группу уведомлений в одном действии. Пользователь может продвигаться вниз, чтобы развернуть пакет уведомлений для подробного просмотра каждого уведомления:

[Пример по![снимка экрана с объединенными уведомлениями](nougat-images/bundled-notifications-sml.png)](nougat-images/bundled-notifications.png#lightbox)

Для поддержки пакетов уведомлений приложение может использовать метод [Builder. сетграуп](xref:Android.App.Notification.Builder.SetGroup*) для объединения аналогичных уведомлений. Дополнительные сведения о пакетных группах уведомлений в Android N см. в разделе [уведомления о объединениях](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#bundle) Android.

#### <a name="custom-views"></a>Пользовательские представления

Android Nougat позволяет создавать пользовательские представления уведомлений с помощью заголовков системных уведомлений, действий и расширяемых макетов. Дополнительные сведения о пользовательских представлениях уведомлений в Android Nougat см. в разделе [улучшения уведомлений](https://developer.android.com/about/versions/nougat/android-7.0.html#notification_enhancements) Android.

### <a name="data-saver"></a>Заставка данных

Начиная с Android Nougat, пользователи могут включить новый параметр *экономии данных* , который блокирует фоновое использование данных. Этот параметр также сигнализирует приложению использовать меньше данных на переднем плане, когда это возможно. [Коннективитиманажер](xref:Android.Net.ConnectivityManager) был расширен в Android Nougat, чтобы приложение могла проверить, включил ли пользователь заставку данных, чтобы приложение могла ограничить использование данных при включенной заставке данных.

Дополнительные сведения о новой функции экономии данных в Android Nougat см. в статье [Оптимизация использования данных в сети](https://developer.android.com/training/basics/network-ops/data-saver.html) для Android.

### <a name="app-shortcuts"></a>Ярлыки приложений

В Android 7,1 появилась функция *ярлыков приложений* , позволяющая пользователям быстро запускать обычные или Рекомендуемые задачи с приложением.
Чтобы активировать меню ярлыков, пользователь долго нажимает значок приложения во второй или более &ndash; меню отображается с быстрой вибрации.
Освобождение нажатия приводит к тому, что меню остается:

[![пример экрана контекстного меню приложения для обмена сообщениями](nougat-images/app-shortcuts-sml.png)](nougat-images/app-shortcuts.png#lightbox)

Эта функция доступна только на уровне API 25 или выше.
Дополнительные сведения о новых функциях ярлыков приложений в Android 7,1 см. в разделе [ярлыки приложений](https://developer.android.com/guide/topics/ui/shortcuts.html) для Android.

### <a name="sample-code"></a>Пример кода

Существует несколько примеров Xamarin. Android, демонстрирующих использование преимуществ Android Nougat:

- [Мултивиндовплайграунд](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-multiwindowplayground) демонстрирует использование многооконного API, доступного в Android Nougat. Можно переключить пример приложения в многооконный режим, чтобы увидеть, как он влияет на жизненный цикл и поведение приложения.

- [Служба обмена сообщениями](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-messagingservice) — это простая служба, которая отправляет уведомления с помощью `NotificationCompatManager`. Он также расширяет уведомление с помощью объекта `RemoteInput`, позволяя устройствам Android Nougat отвечать через текст непосредственно из уведомления, не открывая приложение.

- [Активные уведомления](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-activenotifications) показывают, как использовать API `NotificationManager`, чтобы узнать, сколько уведомлений в настоящее время отображается в приложении.

- [Доступ к каталогу](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-scopeddirectoryaccess) с заданной областью Демонстрирует использование API доступа к каталогам с заданной областью для простого доступа к конкретным каталогам. Это служит альтернативой определению `READ_EXTERNAL_STORAGE` или `WRITE_EXTERNAL_STORAGE` разрешений в манифесте.

- [Прямая загрузка](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-directboot) Показывает, как хранить данные в хранилище, зашифрованном с помощью устройства, которое всегда доступно, пока устройство загружается как до, так и после ввода учетных данных пользователя (ПИН-код/шаблон/пароль).

## <a name="summary"></a>Сводка

В этой статье представлено использование Android Nougat и объясняется, как установить и настроить новейшие средства и пакеты для разработки Xamarin. Android на Android Nougat. В нем также предоставлен обзор основных функций, доступных в Android Nougat, со ссылками на пример исходного кода, которые помогут вам приступить к созданию приложений для Android Nougat.

## <a name="related-links"></a>Связанные ссылки

- [Android 7,1 для разработчиков](https://developer.android.com/about/versions/nougat/android-7.1.html)
- [Заметки о выпуске Xamarin Android 7,0](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_7/xamarin.android_7.0/index.md)
