---
title: Xamarin.Android и комплект SDK версии 9 для Java
description: В этой статье объясняется, как устранить ошибки в комплекте SDK для Java (JDK) версии 9 или более поздней версии в Xamarin.Android.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
ms.openlocfilehash: 2ea7c9b9f900bc339d183c2f5b317792ebec5232
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "73026830"
---
# <a name="xamarinandroid-and-java-development-kit-9-or-later"></a>Xamarin.Android и комплект SDK версии 9 для Java или более поздней версии

_В этой статье объясняется, как устранить ошибки в комплекте SDK для Java (JDK) версии 9 или более поздней версии в Xamarin.Android._

## <a name="overview"></a>Обзор

Xamarin.Android использует Java Development Kit (JDK) для интеграции с пакетом SDK для Android в целях создания приложений Android и запуска конструктора Android. Для последних версий комплект SDK для Android (API 24 и выше) требуется JDK 8 (1.8) или предварительная версия Microsoft Mobile OpenJDK. **Поскольку средства пакета SDK для Android, доступные в Google, пока не совместимы с JDK версии 9, Xamarin.Android не работает с JDK версии 9 или более поздней версии.** .

## <a name="jdk-errors"></a>Ошибки JDK

При попытке построить проект Xamarin.Android с версией JDK более поздней, чем JDK 8, вы получите явную ошибку, указывающую, что эта версия JDK не поддерживается. Пример:

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors
```

Чтобы устранить эти ошибки, необходимо установить JDK 8 (1.8), как описано в разделе [Как обновить версию Java Development Kit (JDK)?](~/android/troubleshooting/questions/update-jdk.md).
Кроме того, можно установить [предварительную версию Microsoft Mobile OpenJDK](~/android/get-started/installation/openjdk.md) в конечном итоге Microsoft Mobile OpenJDK заменит JDK 8 для разработки Xamarin.Android.

## <a name="checking-the-jdk-version"></a>Проверка версии JDK

Узнать, какая версия Java установлена, можно с помощью следующей команды (каталог JDK `bin` должен находиться в `PATH`):

```shell
java -version
```

Если установлен JDK 9, вы увидите примерно следующее сообщение:

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

Если установлен JDK 9 или более поздней версии, необходимо установить Java JDK 8 (1.8) или предварительную версию Microsoft Mobile OpenJDK. Сведения об установке JDK 8 см. в разделе [Как обновить версию Java Development Kit (JDK)?](~/android/troubleshooting/questions/update-jdk.md). Дополнительные сведения об установке Microsoft Mobile OpenJDK см. в статье [Дистрибутив OpenJDK от Майкрософт для разработки мобильных приложений](~/android/get-started/installation/openjdk.md).

Обратите внимание, что удаление более поздней версии JDK не требуется; однако необходимо убедиться, что Xamarin использует JDK 8, а не более позднюю версию JDK. В Visual Studio, выберите элементы **Средства > Параметры > Xamarin > Параметры Android**. Если для **Расположения пакета разработки Java** не задано расположение JDK 8 (например, **C:\\Program Files\\Java\\JDK 1.8.0_111**), щелкните **Изменить** и укажите расположение, где установлен JDK 8. В Visual Studio для Mac перейдите к **Предпочтения > Проекты > Расположения пакетов SDK > Android > Java SDK (JDK)** и щелкните **Обзор**, чтобы обновить этот путь.

## <a name="known-issues-with-jdk-9"></a>Известные проблемы с пакетом JDK 9

### <a name="apksigner"></a>apksigner

Существует известная ошибка с apksigner и JDK 9, в которой файл `apksigner.bat` вызывает `apksigner.jar` с помощью `-Djava.ext.dirs`, а не `-classpath`, как ожидает JDK 9. Мы рекомендуем использовать JDK 8 (1.8). Сведения об установке JDK 8 см. в разделе [Как обновить версию Java Development Kit (JDK)?](~/android/troubleshooting/questions/update-jdk.md)

Если вы установили JDK 9, убедитесь, что для переменной среды `PATH` не задан следующий путь, так как он по-прежнему будет указывать на JDK 9: `C:\ProgramData\Oracle\Java\javapath`. После удаления `java-version` в командной строке должен отобразиться JDK 8.
