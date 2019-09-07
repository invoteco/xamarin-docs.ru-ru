---
title: Часто задаваемые вопросы о Xamarin. Android
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: c14c03d4f618644382aa80b5e0e7fc5b7a46fa9b
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760880"
---
# <a name="android-frequently-asked-questions"></a>Часто задаваемые вопросы о Android

## <a name="installation--setup"></a>Установка & установки

### <a name="which-android-sdk-packages-should-i-installinstall-android-sdk-packagesmd"></a>[Какие пакеты SDK Android следует установить?](install-android-sdk-packages.md)

При установке пакет SDK для Android не будет автоматически включаться все необходимые пакеты для разработки. Хотя отдельные потребности разработчика различаются, в этом руководством обсуждаются пакеты, которые обычно необходимы для разработки с помощью Xamarin. Android.

### <a name="where-can-i-set-my-android-sdk-locationsandroid-sdk-locationmd"></a>[Где можно задать свои расположения пакета SDK для Android?](android-sdk-location.md)

В этом разделе описываются параметры по умолчанию пакет SDK для Android, которые должны работать для большинства установок. и как изменить эти значения по умолчанию в Visual Studio для Mac или Visual Studio, если это необходимо.

### <a name="how-do-i-update-the-java-development-kit-jdk-versionupdate-jdkmd"></a>[Как обновить версию Java Development Kit (JDK)?](update-jdk.md)

В этой статье показано, как обновить версию пакета Java Development Kit (JDK) в Windows и Mac.

### <a name="can-i-use-java-development-kit-jdk-version-9-or-laterjdk9-errorsmd"></a>[Можно ли использовать Java Development Kit (JDK) версии 9 или более поздней?](jdk9-errors.md)

Для Xamarin. Android требуется JDK 8 или Microsoft Mobile OpenJDK. В этой статье перечислены некоторые распространенные сообщения об ошибках, которые могут появиться при установке JDK 9 или более поздней версии, а также инструкции по проверке версий JDK.

### <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packagesinstall-android-support-librarymd"></a>[Как вручную установить вспомогательные библиотеки Android, необходимые для пакетов Xamarin.Android.Support?](install-android-support-library.md)

В этом руководстве приведены примеры действий по установке `Xamarin.Android.Support.v4` библиотеки поддержки в Windows & Mac.

### <a name="what-usb-drivers-do-i-need-to-debug-android-on-windowsandroid-drivers-debug-windowsmd"></a>[Какие драйверы USB нужны для отладки Android в Windows?](android-drivers-debug-windows.md)

Отладка на устройстве Android при разработке в Windows; необходимо установить совместимый драйвер USB. Диспетчер пакет SDK для Android по умолчанию включает в себя "драйвер Google USB", который добавляет поддержку для устройств хранилища.
Для других устройств требуются драйверы USB, опубликованные производителем устройства. В этом разделе содержатся сведения о поиске этих драйверов, а также о других методах тестирования.

### <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vmconnect-android-emulator-mac-windowsmd"></a>[Можно ли подключиться с виртуальной машины Windows к эмуляторам Android под управлением Mac?](connect-android-emulator-mac-windows.md)

В этом руководстве рассматриваются методы использования эмулятора Android.

## <a name="general-questions"></a>Общие вопросы

### <a name="how-do-i-automate-an-android-nunit-test-projectautomate-android-nunit-testmd"></a>[Как автоматизировать тестовый проект Android NUnit?](automate-android-nunit-test.md)

В этом руководстве описаны шаги по настройке проекта тестирования для Android NUnit, а _не_ проекта Xamarin. uitest. Руководства по Xamarin. UITest можно найти [здесь](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

### <a name="why-cant-my-android-release-build-connect-to-the-internetandroid-internetmd"></a>[Почему моя сборка выпуска Android не подключается к Интернету?](android-internet.md)

Наиболее распространенной причиной этой проблемы является то, что **Интернет** -разрешение автоматически включается в отладочную сборку, но его необходимо задать вручную для сборки выпуска. В этом руководство описано, как включить разрешение для сборок выпуска.

### <a name="smarter-xamarin-android-support-v4--v13-nuget-packagesandroid-support-v4v13-librariesmd"></a>[Оптимизированные пакеты NuGet поддержки Android v4 и v13 в Xamarin](android-support-v4v13-libraries.md)

`Support-v4`и `Support-v13` не могут использоваться вместе в одном приложении, то есть они являются взаимоисключающими. Это связано с `Support-v13` тем, что фактически содержит все типы и `Support-v4`реализацию. Если вы попробуете и ссылаетесь на оба проекта в одном проекте, возникнут ошибки с повторяющимися типами.

### <a name="how-do-i-resolve-a-pathtoolongexception-errorpath-too-long-exceptionmd"></a>[Разделы справки устранить ошибку PathTooLongException?](path-too-long-exception.md)

В этой статье объясняется, как устранить ошибку **PathTooLongException** , которая может возникнуть при построении проекта Xamarin. Android.

## <a name="deprecated"></a>Нерекомендуемые

> [!NOTE]
> Приведенные ниже статьи относятся к проблемам, которые были решены в последних версиях Xamarin. Однако если проблема возникает в последней версии программного обеспечения, запишите [новую ошибку](~/cross-platform/troubleshooting/questions/howto-file-bug.md) с полными сведениями о версии и полным выходным файлом журнала сборки.

### <a name="what-version-of-xamarinandroid-added-lollipop-supportxa-lollipopmd"></a>[В какой версии Xamarin.Android добавлена поддержка Lollipop?](xa-lollipop.md)

Это руководством изначально было написано для предварительной версии Android L. В Xamarin. Android 4,17 добавлена поддержка Android L Preview & в Xamarin. Android 4,20 добавлена поддержка интерфейса поддержки Android.

### <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawablemissing-action-mode-share-drawablemd"></a>[Android.Support.v7.AppCompat — не удалось найти ресурс, который соответствует указанному имени: attr 'android:actionModeShareDrawable'](missing-action-mode-share-drawable.md)

Эта ошибка может возникать в более ранних версиях Xamarin, если отсутствуют некоторые из необходимых пакет SDK для Android пакетов.

### <a name="adjusting-java-memory-parameters-for-the-android-designerandroid-designer-java-memorymd"></a>[Настройка параметров памяти Java для конструктора Android](android-designer-java-memory.md)

Параметры памяти по умолчанию, используемые при запуске `java` процесса для конструктора Android, могут быть несовместимы с некоторыми конфигурациями системы. Начиная с Xamarin Studio 5.7.2.7 и Xamarin для Visual Studio 3.9.344 эти параметры можно настраивать отдельно для каждого проекта.

### <a name="my-android-resourcedesignercs-file-will-not-updateresource-designer-wont-updatemd"></a>[Не обновляется файл Resource.designer.cs Android](resource-designer-wont-update.md)

Ошибка в Xamarin. Studio 5,1 ранее повредила CSPROJ-файлы, частично или полностью удаляя код XML в файле. csproj. Это приведет к сбою важных частей системы сборки Android (например, при обновлении Android Resource.designer.cs). По состоянию на 5.1.4 стабильной версии 15 июля эта ошибка была исправлена. но во многих случаях файл проекта необходимо исправить вручную, как описано в этом разделе.
