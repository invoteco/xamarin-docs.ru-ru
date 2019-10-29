---
title: Внедрение .NET в Android
ms.prod: xamarin
ms.assetid: EB2F967A-6D95-4448-994B-6D5C7BFAC2C7
author: davidortinau
ms.author: daortin
ms.date: 06/15/2018
ms.openlocfilehash: fef422b799ab5280aef205f4d5e55fd91050da39
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73007337"
---
# <a name="net-embedding-on-android"></a>Внедрение .NET в Android

В некоторых случаях может потребоваться добавить библиотеку Xamarin .NET к существующему собственному проекту Android. Для этого можно использовать средство [ембеддинатор-4000](https://www.nuget.org/packages/Embeddinator-4000/) , чтобы превратить библиотеку .NET в собственную библиотеку, которая может быть включена в собственное приложение Android на основе Java.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="xamarinandroid-requirements"></a>Требования Xamarin. Android

Для работы Xamarin. Android с внедрением .NET необходимо следующее:

- Необходимо установить **Xamarin. android** &ndash;[xamarin. Android 7,5](https://visualstudio.microsoft.com/xamarin/) или более поздней версии.

- Необходимо установить **Android Studio** &ndash;[Android Studio 3. x](https://developer.android.com/studio/) или более поздней версии.

- **Пакет Java Developer Kit** &ndash;   Необходимо установить [Java 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) или более поздней версии.

## <a name="using-embeddinator-4000"></a>Использование Ембеддинатор-4000

Чтобы использовать библиотеку .NET в собственном проекте Android, выполните следующие действия.

1. Создайте проект C# Библиотеки Android.

2. Установите [ембеддинатор-4000](https://www.nuget.org/packages/Embeddinator-4000/).

3. Откройте файл **Embeddinator-4000. exe** и добавьте его в свой **путь**. Пример:

    ```cmd
    set PATH=%PATH%;C:\Users\USERNAME\.nuget\packages\embeddinator-4000\0.4.0\tools
    ```

4. Запустите Ембеддинатор-4000 в сборке библиотеки. Пример:

    ```cmd
    Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5. Используйте созданный файл AAR в проекте Java в Android Studio.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

## <a name="xamarinandroid-requirements"></a>Требования Xamarin. Android

Для работы Xamarin. Android с внедрением .NET необходимо следующее:

- Необходимо установить **Xamarin. android** &ndash;[xamarin. Android 7,5](https://visualstudio.microsoft.com/xamarin/) или более поздней версии.

- Необходимо установить **Android Studio** &ndash;[Android Studio 3. x](https://developer.android.com/studio/) или более поздней версии.

- **Пакет Java Developer Kit** &ndash;   Необходимо установить [Java 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) или более поздней версии.

- Необходимо установить **mono** &ndash;[Mono 5,0](https://www.mono-project.com/download/) или более поздней версии (моно установлен с Visual Studio для Mac).

## <a name="using-embeddinator-4000"></a>Использование Ембеддинатор-4000

Чтобы использовать библиотеку .NET в собственном проекте Android, выполните следующие действия.

1. Создайте проект C# Библиотеки Android.

2. Установите [ембеддинатор-4000](https://www.nuget.org/packages/Embeddinator-4000/).

3. Откройте **файл Embeddinator-4000. exe** и добавьте **Mono** в свой путь. Пример:

    ```bash
    export TOOLS=~/.nuget/packages/embeddinator-4000/0.4.0/tools
    export PATH=$PATH:/Library/Frameworks/Mono.framework/Commands
    ```

4. Запустите Ембеддинатор-4000 в сборке библиотеки. Пример:

    ```bash
    mono $TOOLS/Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5. Используйте созданный файл AAR в проекте Java в Android Studio.

-----

Параметры использования и командной строки описаны в документации по [ембеддинатор-4000](https://github.com/mono/Embeddinator-4000/blob/master/Usage.md#java--c) .

## <a name="callbacks"></a>Обратные вызовы

Сведения о [выполнении вызовов между C# и Java](callbacks.md).

## <a name="samples"></a>Примеры

- [Пример приложения Weather](https://github.com/jamesmontemagno/embeddinator-weather)
