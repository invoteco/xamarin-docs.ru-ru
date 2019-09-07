---
title: Как обновить версию Java Development Kit (JDK)?
description: В этой статье показано, как обновить версию пакета Java Development Kit (JDK) в Windows и Mac.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 4b3ac51d-18dd-4034-87b4-4365194e4ece
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/07/2018
ms.openlocfilehash: d3f4c602f7e581cab74b61072e248a22eede9a22
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762016"
---
# <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>Как обновить версию Java Development Kit (JDK)?

_В этой статье показано, как обновить версию пакета Java Development Kit (JDK) в Windows и Mac._

## <a name="overview"></a>Обзор

Xamarin. Android использует пакет средств разработки Java (JDK) для интеграции с пакет SDK для Android для создания приложений Android и запуска конструктора Android. Для последних версий пакет SDK для Android (API 24 и выше) требуется JDK 8 (1,8). Кроме того, можно установить [предварительную версию Microsoft Mobile OpenJDK](~/android/get-started/installation/openjdk.md). Microsoft Mobile OpenJDK, в конечном итоге, заменит JDK 8 для разработки Xamarin. Android.

Чтобы обновить Microsoft Mobile OpenJDK, см. статью [Предварительная версия Microsoft Mobile OpenJDK](~/android/get-started/installation/openjdk.md). Чтобы выполнить обновление до JDK 8, выполните следующие действия.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Скачайте JDK 8 (1,8) с [веб-сайта Oracle](https://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Снимок экрана со страницей загрузки JDK на веб-сайте Oracle](update-jdk-images/image1.png)

2. Выберите 64-разрядную версию, чтобы разрешить рендеринг [пользовательских элементов управления](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/vs/xamarin.vs_4/xamarin.vs_4.2/index.md#androiddesignercustomcontrols) в конструкторе Xamarin Android:

    ![Выбор пакета Windows x64 JDK для скачивания со страницы скачивания JDK](update-jdk-images/image2.png)

3. Запустите exe и установите **средства разработки**:

    ![Установка средств разработки в установщике JDK](update-jdk-images/image3.png)

4. Откройте Visual Studio и обновите **расположение пакета разработки Java** , чтобы оно указывало на новый JDK в разделе **сервис > параметры > Xamarin > Android параметры > расположение пакета Java Development Kit**:

    [![Параметр пути для JDK на странице параметров Android](update-jdk-images/image4-sml.png)](update-jdk-images/image4.png#lightbox)

Не забудьте перезапустить Visual Studio после обновления расположения.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

1. Скачайте JDK 8 (1,8) с [веб-сайта Oracle](https://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Снимок экрана со страницей загрузки JDK на веб-сайте Oracle](update-jdk-images/image1.png)

2. Откройте файл DMG и запустите установщик. pkg:

    ![Запуск установщика JDK на macOS](update-jdk-images/image5.png)

Mac OS автоматически установит новую версию JDK в качестве значения по умолчанию, обновив **/систем/либрари/фрамеворкс/жававм.фрамеворк/версионс/куррент**. Затем можно дважды проверить, что в качестве расположения **пакета SDK для Java (JDK)** задано ожидаемое значение по умолчанию **/usr** в разделе **Visual Studio для Mac параметры > > проекты > расположения пакета SDK > расположения пакетов Android > > Java SDK (JDK)** :

[![Настройка расположения JDK на вкладке "расположения Android"](update-jdk-images/image6-sml.png)](update-jdk-images/image6.png#lightbox)

-----
