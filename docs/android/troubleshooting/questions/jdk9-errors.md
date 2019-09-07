---
title: Xamarin. Android и Java Development Kit 9
description: В этой статье объясняется, как устранить ошибки в пакете Java Development Kit (JDK) 9 или более поздней версии в Xamarin. Android.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: 58b1b29a34bfb03661959af4dea8ed57b8f504cc
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760864"
---
# <a name="xamarinandroid-and-java-development-kit-9-or-later"></a>Xamarin. Android и Java Development Kit 9 или более поздней версии

_В этой статье объясняется, как устранить ошибки в пакете Java Development Kit (JDK) 9 или более поздней версии в Xamarin. Android._

## <a name="overview"></a>Обзор

Xamarin. Android использует пакет средств разработки Java (JDK) для интеграции с пакет SDK для Android для создания приложений Android и запуска конструктора Android. Для последних версий пакет SDK для Android (API 24 и выше) требуется JDK 8 (1,8) или предварительная версия Microsoft Mobile OpenJDK. **Поскольку средства пакет SDK для Android, доступные в Google, еще не совместимы с JDK 9, Xamarin. Android не работает с JDK 9 или более поздней версии.**

## <a name="jdk-errors"></a>Ошибки JDK

При попытке построить проект Xamarin. Android с версией JDK более поздней, чем JDK 8, вы получите явную ошибку, указывающую, что эта версия JDK не поддерживается. Например:

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors
```

Чтобы устранить эти ошибки, необходимо установить JDK 8 (1,8), как описано в [разделы справки обновлении версии пакета Java Development Kit (JDK)](~/android/troubleshooting/questions/update-jdk.md).
Кроме того, вы можете установить [предварительную версию Microsoft Mobile OpenJDK](~/android/get-started/installation/openjdk.md) , которую Microsoft Mobile OpenJDK будет заменять JDK 8 для разработки Xamarin. Android.

## <a name="checking-the-jdk-version"></a>Проверка версии JDK

Чтобы узнать, какая версия Java установлена, введите следующую команду (каталог JDK `bin` должен находиться `PATH`в папке):

```shell
java -version
```

Если JDK 9 установлен, вы увидите примерно следующее сообщение:

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

Если установлен JDK 9 или более поздней версии, необходимо установить Java JDK 8 (1,8) или предварительную версию Microsoft Mobile OpenJDK. Сведения об установке JDK 8 см. [в разделе разделы справки обновление версии пакета Java Development Kit (JDK)](~/android/troubleshooting/questions/update-jdk.md). Дополнительные сведения об установке Microsoft Mobile OpenJDK см. в [статье Предварительная версия Microsoft Mobile OpenJDK](~/android/get-started/installation/openjdk.md).

Обратите внимание, что удаление более поздней версии JDK не требуется; Однако необходимо убедиться, что Xamarin использует JDK 8, а не более позднюю версию JDK. В Visual Studio щелкните **сервис > параметры > параметры Xamarin > Android**. Если для **расположения пакета Java Development Kit** не задано расположение JDK 8 (например, **C:\\Program Files\\\\Java JDK 1.8.0 _111**), нажмите кнопку **изменить** и укажите расположение, где установлена JDK 8. В Visual Studio для Mac перейдите к **параметрам > проекты > расположения пакетов sdk > Android > Java SDK (JDK)** и нажмите кнопку **Обзор** , чтобы обновить этот путь.

## <a name="known-issues-with-jdk-9"></a>Известные проблемы с JDK 9

### <a name="apksigner"></a>apksigner

Существует известная ошибка с apksigner и JDK 9, в которой `apksigner.bat` файл вызывает с помощью `-Djava.ext.dirs` , `apksigner.jar` а не `-classpath` какой JDK 9. Рекомендуется использовать JDK 8 (1,8). Сведения об установке JDK 8 см [. в разделе разделы справки обновление версии пакета Java Development Kit (JDK).](~/android/troubleshooting/questions/update-jdk.md)

Если вы установили JDK 9, убедитесь, что для переменной `PATH` среды не задан следующий путь, так как он по-прежнему будет указывать на JDK 9:. `C:\ProgramData\Oracle\Java\javapath` После удаления `java-version` в командной строке должен отобразиться JDK 8.
