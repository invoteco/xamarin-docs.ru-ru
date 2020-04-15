---
title: Как обновить версию Java Development Kit (JDK)?
description: В этой статье показано, как обновить версию пакета Java Development Kit (JDK) в Windows и Mac.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 4b3ac51d-18dd-4034-87b4-4365194e4ece
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/07/2018
ms.openlocfilehash: 0f7499551db7d86d7978b9c3e1f562a2f054c202
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "73019521"
---
# <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>Как обновить версию Java Development Kit (JDK)?

_В этой статье показано, как обновить версию пакета Java Development Kit (JDK) в Windows и Mac._

## <a name="overview"></a>Обзор

Xamarin.Android использует Java Development Kit (JDK) для интеграции с пакетом SDK для Android в целях создания приложений Android и запуска конструктора Android. Последние версии пакетов SDK для Android (API 24 и более поздних версий) требуют JDK 8 (1.8). Кроме того, можно установить [предварительную версию Microsoft Mobile OpenJDK](~/android/get-started/installation/openjdk.md). Microsoft Mobile OpenJDK, в конечном итоге, заменит JDK 8 для разработки Xamarin.Android.

Сведения об обновлении до Microsoft Mobile OpenJDK см. в разделе [Обзор](~/android/get-started/installation/openjdk.md). Чтобы обновить до JDK 8, выполните приведенные ниже действия.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Загрузите JDK 8 (1.8) с веб-сайта [Oracle](https://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Снимок экрана со страницей загрузки JDK на веб-сайте Oracle](update-jdk-images/image1.png)

2. Выберите 64-разрядную версию, чтобы разрешить визуализацию [пользовательских элементов управления](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/vs/xamarin.vs_4/xamarin.vs_4.2/index.md#androiddesignercustomcontrols) в конструкторе Xamarin Android:

    ![Выбор пакета Windows x64 JDK для загрузки со страницы загрузки JDK](update-jdk-images/image2.png)

3. Запустите EXE-файл и установите **средства разработки**.

    ![Установка средств разработки в установщике JDK](update-jdk-images/image3.png)

4. Откройте Visual Studio и обновите **расположение пакета SDK для Java** таким образом, чтобы оно ссылалось на новый JDK в **Средства > Параметры > Xamarin > Параметры Android > Расположение пакета SDK для Java**:

    [![Настройка пути для JDK на странице "Параметры Android"](update-jdk-images/image4-sml.png)](update-jdk-images/image4.png#lightbox)

Не забудьте перезапустить Visual Studio после обновления расположения.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. Загрузите JDK 8 (1.8) с веб-сайта [Oracle](https://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Снимок экрана со страницей загрузки JDK на веб-сайте Oracle](update-jdk-images/image1.png)

2. Откройте DMG-файл и запустите установщик PKG-файлов:

    ![Запуск установщика JDK на macOS](update-jdk-images/image5.png)

Mac OS автоматически установит новую версию JDK в качестве значения по умолчанию, обновив **/System/Library/Frameworks/JavaVM.framework/Versions/Current**. Вы можете затем перепроверить, установлено ли расположение **Java SDK (JDK)** согласно стандартному **/usr** в **Visual Studio для Mac > Предпочтения > Проекты > Расположения пакетов SDK > Android > Расположения > Пакет SDK (JDK) для Java**:

[![Настройка расположения JDK на вкладке "Расположения Android"](update-jdk-images/image6-sml.png)](update-jdk-images/image6.png#lightbox)

-----
