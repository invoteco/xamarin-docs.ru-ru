---
title: Часто задаваемые вопросы о Xamarin.Android
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
ms.openlocfilehash: 28b13951a0d681ffdb8e643667e496e0b3606628
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2020
ms.locfileid: "76724006"
---
# <a name="android-frequently-asked-questions"></a>Вопросы и ответы по Android

## <a name="installation--setup"></a>Установка и настройка

### <a name="which-android-sdk-packages-should-i-install"></a>[Какие пакеты SDK Android следует установить?](install-android-sdk-packages.md)

Установка пакета SDK для Android не предусматривает автоматическую установку всех минимально необходимых для разработки пакетов. Хотя потребности отдельных разработчиков отличаются, в данном руководстве обсуждаются пакеты, которые, как правило, необходимы для разработки с Xamarin.Android.

### <a name="where-can-i-set-my-android-sdk-locations"></a>[Где можно задать свои расположения пакета SDK для Android?](android-sdk-location.md)

В этом разделе описываются параметры по умолчанию пакета SDK для Android, которые должны работать для большинства установок, а также способы изменения этих значений по умолчанию в Visual Studio для Mac или Visual Studio, если это необходимо.

### <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>[Как обновить версию Java Development Kit (JDK)?](update-jdk.md)

В этой статье показано, как обновить версию пакета Java Development Kit (JDK) в Windows и Mac.

### <a name="can-i-use-java-development-kit-jdk-version-9-or-later"></a>[Можно ли использовать Java Development Kit (JDK) версии 9 или более поздней?](jdk9-errors.md)

Для Xamarin.Android требуется JDK 8 или Microsoft Mobile OpenJDK. В этой статье перечислены некоторые распространенные сообщения об ошибках, которые могут появиться при установке JDK 9 или более поздней версии, а также инструкции по проверке версий JDK.

### <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packages"></a>[Как вручную установить вспомогательные библиотеки Android, необходимые для пакетов Xamarin.Android.Support?](install-android-support-library.md)

В этом руководстве приведены примеры действий по установке библиотеки поддержки `Xamarin.Android.Support.v4` для Windows и Mac.

### <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>[Какие драйверы USB нужны для отладки Android в Windows?](android-drivers-debug-windows.md)

Для отладки на устройстве Android при разработке в Windows необходимо установить совместимый драйвер USB. Диспетчер SDK Android по умолчанию включает в себя Google USB-драйвер, который добавляет поддержку для устройств Nexus.
Для других устройств требуются драйверы USB, опубликованные производителем устройства. В этом разделе содержатся сведения о поиске этих драйверов, а также о других методах тестирования.

### <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>[Можно ли подключиться с виртуальной машины Windows к эмуляторам Android под управлением Mac?](connect-android-emulator-mac-windows.md)

В этом руководстве рассматриваются методы использования эмулятора Android.

## <a name="general-questions"></a>Общие вопросы

### <a name="how-do-i-automate-an-android-nunit-test-project"></a>[Как автоматизировать тестовый проект Android NUnit?](automate-android-nunit-test.md)

В этом руководстве описаны действия по настройке тестового проекта Android NUnit, а _не_ проекта Xamarin.UITest. Руководства по Xamarin.UITest можно найти [здесь](/appcenter/test-cloud/preparing-for-upload).

### <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>[Почему моя сборка выпуска Android не подключается к Интернету?](android-internet.md)

Чаще всего эта проблема возникает из-за автоматического включения разрешения **INTERNET** в отладочную сборку, когда для сборки выпуска его необходимо задать вручную. В этом руководстве описано, как включить разрешение для сборок выпуска.

### <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>[Оптимизированные пакеты NuGet поддержки Android v4 и v13 в Xamarin](android-support-v4v13-libraries.md)

`Support-v4` и `Support-v13` нельзя использовать в одном приложении, то есть они являются взаимоисключающими. Это связано с тем, что `Support-v13` фактически содержит все типы и реализацию `Support-v4`. Если вы попробуете сослаться на обе версии в одном и том же проекте, то столкнетесь с ошибками повторяющегося типа.

### <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>[Как устранить ошибку PathTooLongException?](path-too-long-exception.md)

В этой статье объясняется, как разрешить ошибки **PathTooLongException**, которые могут возникнуть при создании проекта Xamarin.Android.

## <a name="deprecated"></a>Нерекомендуемые

> [!NOTE]
> Приведенные ниже статьи относятся к проблемам, которые были решены в последних версиях Xamarin. Тем не менее, если проблема возникает в последней версии программного обеспечения, создайте файл [новой ошибки](~/cross-platform/troubleshooting/questions/howto-file-bug.md) с полными сведениями о версиях и полным выводом журнала сборки.

### <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>[В какой версии Xamarin.Android добавлена поддержка Lollipop?](xa-lollipop.md)

Это руководство изначально написано для предварительной версии Android L. Xamarin.Android 4.17. В Xamarin.Android 4.17 добавлена поддержка предварительного просмотра на Android L, а в Xamarin.Android 4.20 — поддержка Android Lollipop.

### <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>[Android.Support.v7.AppCompat — не удалось найти ресурс, который соответствует указанному имени: attr 'android:actionModeShareDrawable'](missing-action-mode-share-drawable.md)

Эта ошибка может возникать в более ранних версиях Xamarin, если отсутствуют некоторые из необходимых пакетов SDK для Android.

### <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>[Настройка параметров памяти Java для конструктора Android](android-designer-java-memory.md)

Параметры памяти по умолчанию, используемые при запуске процесса `java`для конструктора Android, могут быть несовместимы с некоторыми конфигурациями системы. Начиная с Xamarin Studio 5.7.2.7 и Xamarin для Visual Studio 3.9.344 эти параметры можно настраивать отдельно для каждого проекта.

### <a name="my-android-resourcedesignercs-file-will-not-update"></a>[Не обновляется файл Resource.designer.cs Android](resource-designer-wont-update.md)

Ошибка в Xamarin.Studio 5.1 ранее повреждала CSPROJ-файлы путем частичного или полного удаления XML-кода в CSPROJ-файле. Это приводило к тому, что важные части системы сборки Android (такие как обновление Android Resource.design.cs) выходили из строя. Начиная со стабильной версии 5.1.4, вышедшей 15 июля, эту ошибку исправлено; однако во многих случаях файл проекта приходится восстанавливать вручную, как описано в этом руководстве.
