---
title: Возможности Android 10
description: Как приступить к разработке приложений для Android 10 с помощью Xamarin. Android.
ms.assetid: B3342772-FB88-4B7F-BC15-8BC78EED749E
author: JonDouglas
ms.author: jodou
ms.date: 09/17/2019
ms.openlocfilehash: 3c729517c687e3d81fc3cc17938c96762c7b9252
ms.sourcegitcommit: f324193bcfb1c674e421955cf6ee2a4249d61ac3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/19/2019
ms.locfileid: "71119460"
---
# <a name="android-10-with-xamarin"></a>Android 10 с Xamarin

_Как приступить к разработке приложений для Android 10 с помощью Xamarin. Android._

Android 10 теперь доступен в Google. В этом выпуске предоставляется ряд новых функций и API, и многие из них необходимы для использования новых возможностей оборудования на последних устройствах Android.

![Логотип Android 10](~/android/platform/android-10-images/android10_black.png)

Эта статья содержит сведения, которые помогут вам приступить к разработке приложений Xamarin. Android для Android 10. В нем объясняется, как установить необходимые обновления, настроить пакет SDK и подготовить эмулятор или устройство для тестирования. В нем также представлен обзор новых возможностей Android 10 и приведен пример исходного кода, демонстрирующий использование некоторых ключевых функций Android 10.

Xamarin. Android 10,0 обеспечивает поддержку Android 10. Дополнительные сведения о поддержке Xamarin. Android для Android 10 см. в [заметках о выпуске Xamarin. android 10,0](https://docs.microsoft.com/xamarin/android/release-notes/10/10.0).

## <a name="requirements"></a>Требования

Следующий список необходим для использования функций Android 10 в приложениях на основе Xamarin:

- **Visual Studio** — рекомендуется visual Studio 2019. В Windows Update до Visual Studio 2019 версии 16,3 или более поздней. В macOS обновите Visual Studio 2019 для Mac версии 8,3 или более поздней.
- **Xamarin. Android** — Xamarin. Android 10,0 или более поздней версии должен быть установлен вместе с Visual Studio (Xamarin. Android автоматически устанавливается в составе рабочей нагрузки **.NET для разработки мобильных приложений на платформе** Windows и устанавливается как часть **Visual Studio для установщика Mac**)
- **Пакет Java Developer Kit** — для разработки Xamarin. Android 10,0 требуется JDK 8. Дистрибутив OpenJDK Майкрософт автоматически устанавливается в составе Visual Studio.
- **Пакет SDK для Android** -пакет SDK для Android API 29 должен быть установлен с помощью диспетчера пакет SDK для Android.

## <a name="get-started"></a>Начало работы

Чтобы приступить к разработке приложений Android 10 с помощью Xamarin. Android, необходимо загрузить и установить последние версии средств и пакетов SDK, прежде чем можно будет создать первый проект Android 10:

1. **Рекомендуется использовать Visual Studio 2019**. Обновите Visual Studio 2019 версии 16,3 или более поздней. Если вы используете Visual Studio для Mac 2019, обновите Visual Studio 2019 для Mac версии 8,3 или более поздней.
2. Установите пакеты и средства **Android 10 (API 29)** с помощью диспетчера пакетов SDK.
    - Платформа пакета SDK для Android 10 (API 29)
    - Образ системы Android 10 (API 29)
    - Пакет SDK для Android Build-Tools 29.0.0 +
    - Пакет SDK для Android Platform-Tools 29.0.0 +
    - Android Emulator 29.0.0 +
3. Создайте новый проект Xamarin. Android, предназначенный для Android 10,0.
4. Настройте эмулятор или устройство для тестирования приложений Android 10.

Каждый из этих шагов описан ниже.

### <a name="update-visual-studio"></a>Обновление Visual Studio

Visual Studio 2019 рекомендуется использовать для создания приложений Android 10 с помощью Xamarin.

Если вы используете Visual Studio 2019, обновите Visual Studio 2019 до версии 16,3 или более поздней (инструкции см. в статье [обновление до последней версии Visual studio 2019](https://docs.microsoft.com/visualstudio/install/update-visual-studio)). В macOS обновление до Visual Studio 2019 для Mac 8,3 или более поздней версии (инструкции см. в статье [обновление Visual studio 2019 для Mac до последней версии](https://docs.microsoft.com/en-us/visualstudio/mac/update)).

### <a name="install-the-android-sdk"></a>Установка пакет SDK для Android

Чтобы создать проект с помощью Xamarin. Android 10,0, необходимо сначала использовать диспетчер пакет SDK для Android, чтобы установить платформу SDK для **Android 10 (API уровня 29)** .

1. Запустите диспетчер пакетов SDK. В Visual Studio щелкните **сервис > Android > пакет SDK для Android Manager.** В Visual Studio для Mac выберите **сервис > диспетчер пакетов SDK.**
2. В правом нижнем углу щелкните значок шестеренки и выберите **репозиторий > Google (не поддерживается):**

![Выбор репозитория пакет SDK для Android Manager](~/android/platform/android-10-images/sdkrepository.png)

3. Установите пакеты **платформы SDK для Android 10** , которые указаны как **пакет SDK для Android Platform 29** на вкладке **платформы** (Дополнительные сведения об использовании диспетчера пакетов SDK см. в разделе [пакет SDK для Android Setup](https://docs.microsoft.com/en-us/xamarin/android/get-started/installation/android-sdk)):

![Вкладка "платформа" пакет SDK для Android Manager](~/android/platform/android-10-images/sdkplatforms.png)

### <a name="create-a-xamarinandroid-project"></a>Создание проекта Xamarin. Android

Создайте новый проект Xamarin. Android. Если вы не знакомы с разработкой Android с помощью Xamarin, ознакомьтесь со статьей [Hello, Android,](https://docs.microsoft.com/en-us/xamarin/android/get-started/hello-android/index) чтобы узнать о создании проектов Xamarin. Android.

При создании проекта Android необходимо настроить параметры версии для целевого устройства Android 10,0 или более поздней версии. Например, чтобы нацелить проект на версию Android 10, необходимо настроить уровень API Android для своего проекта на **android 10,0 (API 29)** . Сюда входит **Целевая версия .NET Framework** и **целевая версия пакет SDK для Android** в API 29 или более поздней версии. Дополнительные сведения о настройке уровней API Android см. в разделе Общие сведения об [уровнях API Android.](https://docs.microsoft.com/en-us/xamarin/android/app-fundamentals/android-api-levels)

![Целевая платформа Xamarin. Android](~/android/platform/android-10-images/targetframework.png)

### <a name="configure-a-device-or-emulator"></a>Настройка устройства или эмулятора

Если вы используете физическое устройство, например пиксель, вы можете скачать обновление Android 10, перейдя к `System`  >  `System update`  >  `Check for update` в параметрах вашего телефона. Если вы предпочитаете вспышку устройства, ознакомьтесь с инструкциями по миганию [образа фабрики](https://developers.google.com/android/ota) или оборудования [OTA](https://developers.google.com/android/ota) на устройстве.

Если вы используете эмулятор, создайте виртуальное устройство для API уровня 29 и выберите образ на основе x86. Сведения об использовании Android Device Manager для создания виртуальных устройств и управления ими см. в статье [Управление виртуальными устройствами с помощью Android Device Manager.](https://docs.microsoft.com/en-us/xamarin/android/get-started/installation/android-emulator/device-manager) Сведения об использовании Android Emulator для тестирования и отладки см. в разделе [Отладка в Android Emulator.](https://docs.microsoft.com/en-us/xamarin/android/deploy-test/debugging/debug-on-emulator)

## <a name="new-features"></a>Новые функции

В Android 10 появились различные новые функции. Некоторые из этих новых функций предназначены для использования новых возможностей оборудования, предлагаемых последними устройствами Android, а другие предназначены для дальнейшего улучшения пользовательского интерфейса Android.

## <a name="enhance-your-app-with-android-10-features-and-apis"></a>Расширьте возможности приложения с помощью функций и API-интерфейсов Android 10

Затем, когда будете готовы, Познакомьтесь с Android 10 и Узнайте о [новых функциях и интерфейсах API](https://developer.android.com/preview/api-overview.html) , которые можно использовать. Ниже приведены некоторые из основных возможностей, с которыми можно начать работу.

Эти функции рекомендуются для каждого приложения:

- **Темная тема.**  Обеспечьте единообразную работу пользователей, которые включают в себя темную тему на уровне всей системы, добавив [темную тему](https://developer.android.com/preview/features/darktheme) или включив [Force-темный](https://developer.android.com/preview/features/darktheme#force_dark).

![Темная тема](~/android/platform/android-10-images/darktheme.png)

- **Поддержка [жестурал навигации](https://developer.android.com/preview/features/gesturalnav)**  в приложении с помощью границ и убедитесь, что пользовательские жесты дополняются жестами навигации системы.

![Навигация по жестам](~/android/platform/android-10-images/gesturenavigation.png)

- **Оптимизировать для фолдаблес:**  Создавайте простые и пограничные возможности на современных инновационных устройствах, [оптимизируя фолдаблес](https://developer.android.com/preview/features/foldables).

![Свертываемые](~/android/platform/android-10-images/foldable.png)

Эти функции рекомендуются, если это уместно для вашего приложения:

- **Более интерактивные уведомления.**  если ваши уведомления включают сообщения, включите [предлагаемые ответы и действия в уведомлениях](https://developer.android.com/preview/features#smart-suggestions) , чтобы привлечь пользователей и позволить им мгновенно принимать меры.
- **Улучшенные биометрические показатели.**  если вы используете биометрическую проверку подлинности, переходите на [биометрикпромпт](https://developer.android.com/reference/androidx/biometric/BiometricPrompt), чтобы обеспечить поддержку проверки подлинности отпечатков пальцев на современных устройствах.
- **Расширенная запись.**  для поддержки записи субтитров или игрового процесса включите [запись воспроизведения звука](https://developer.android.com/preview/features/playback-capture). Это отличный способ получить доступ к большему кругу пользователей и сделать приложение более доступным.
- **Лучшие кодеки:**  для мультимедийных приложений попробуйте [AV1](https://en.wikipedia.org/wiki/AV1) для потоковой передачи видео и [HDR10 +](https://en.wikipedia.org/wiki/High-dynamic-range_video#HDR10+) для динамического диапазона видео. Для потоковой передачи речи и музыки можно использовать кодировку [опус](http://opus-codec.org/) , а для «musicians» доступен [собственный API](https://developer.android.com/preview/features/midi) MIDI.
- **Улучшенные сетевые интерфейсы API.**  если ваше приложение управляет устройствами Интернета вещей через Wi-Fi, попробуйте использовать новые  [интерфейсы API сетевого подключения](https://developer.android.com/preview/features#peer2peer)для таких функций, как Настройка, скачивание или печать.

Это лишь некоторые из многих новых функций и API-интерфейсов в Android 10. Чтобы увидеть их все, посетите [сайт Android 10 для разработчиков](https://developer.android.com/about/versions/10/highlights).

## <a name="behavior-changes"></a>Изменения в поведении

Если для целевой версии Android задан уровень API 29, то существует несколько изменений платформы, которые затронет поведение приложения, даже если вы не реализуете новые функции, описанные выше. Ниже приведен краткий обзор этих изменений.

- [Чтобы обеспечить стабильность и совместимость приложений, платформа Android теперь ограничена интерфейсами, не являющимися пакетами SDK, которые приложение может использовать в Android 10](https://developer.android.com/about/versions/10/behavior-changes-10#non-sdk-restrictions).
- [Общая память изменена](https://developer.android.com/about/versions/10/behavior-changes-10#shared-memory).
- [Среда выполнения Android & правильность AOT](https://developer.android.com/about/versions/10/behavior-changes-10#system-only-oat).
- [Должны `USE_FULL_SCREEN_INTENT`быть запрошены разрешения для полноэкранных целей ](https://developer.android.com/about/versions/10/behavior-changes-10#full-screen-intents).
- [Поддержка фолдаблес](https://developer.android.com/about/versions/10/behavior-changes-10#foldables).

## <a name="summary"></a>Сводка

В этой статье мы рассмотрели Android 10 и объяснили, как установить и настроить новейшие средства и пакеты для разработки Xamarin. Android с Android 10. В нем представлены общие сведения о ключевых функциях, доступных в Android 10. В нем содержатся ссылки на разделы документации по API и разработчиков Android, которые помогут вам приступить к созданию приложений для Android 10. Он также выделяет наиболее важные изменения в работе Android 10, которые могут повлиять на существующие приложения.

## <a name="related-links"></a>Связанные ссылки

- [Android 10](https://developer.android.com/about/versions/10)
