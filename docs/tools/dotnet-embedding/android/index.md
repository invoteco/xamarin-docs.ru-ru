---
title: Внедрение .NET в Android
ms.prod: xamarin
ms.assetid: EB2F967A-6D95-4448-994B-6D5C7BFAC2C7
author: conceptdev
ms.author: crdun
ms.date: 06/15/2018
ms.openlocfilehash: 1369d5cd901207618128da8b0111e488eae7b83e
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70772222"
---
# <a name="net-embedding-on-android"></a>Внедрение .NET в Android

В некоторых случаях может потребоваться добавить библиотеку Xamarin .NET к существующему собственному проекту Android. Для этого можно использовать средство [ембеддинатор-4000](https://www.nuget.org/packages/Embeddinator-4000/) , чтобы превратить библиотеку .NET в собственную библиотеку, которая может быть включена в собственное приложение Android на основе Java.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="xamarinandroid-requirements"></a>Требования Xamarin. Android

Для работы Xamarin. Android с внедрением .NET необходимо следующее:

- Необходимо установить **Xamarin. Android** &ndash; [Xamarin. Android 7,5](https://visualstudio.microsoft.com/xamarin/) или более поздней версии.

- **Android Studio** Необходимо установить [Android Studio 3. x](https://developer.android.com/studio/) или более поздней версии. &ndash;

- **Пакет Java Developer Kit** &ndash;   Необходимо установить [Java 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) или более поздней версии.

## <a name="using-embeddinator-4000"></a>Использование Ембеддинатор-4000

Чтобы использовать библиотеку .NET в собственном проекте Android, выполните следующие действия.

1. Создайте проект C# Библиотеки Android.

2. Установите [ембеддинатор-4000](https://www.nuget.org/packages/Embeddinator-4000/).

3. Откройте файл **Embeddinator-4000. exe** и добавьте его в свой **путь**. Например:

    ```cmd
    set PATH=%PATH%;C:\Users\USERNAME\.nuget\packages\embeddinator-4000\0.4.0\tools
    ```

4. Запустите Ембеддинатор-4000 в сборке библиотеки. Например:

    ```cmd
    Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5. Используйте созданный файл AAR в проекте Java в Android Studio.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

## <a name="xamarinandroid-requirements"></a>Требования Xamarin. Android

Для работы Xamarin. Android с внедрением .NET необходимо следующее:

- Необходимо установить **Xamarin. Android** &ndash; [Xamarin. Android 7,5](https://visualstudio.microsoft.com/xamarin/) или более поздней версии.

- **Android Studio** Необходимо установить [Android Studio 3. x](https://developer.android.com/studio/) или более поздней версии. &ndash;

- **Пакет Java Developer Kit** &ndash;   Необходимо установить [Java 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) или более поздней версии.

- **Моно** Должен быть установлен [моно 5,0](https://www.mono-project.com/download/) или более поздняя версия (моно установлен с Visual Studio для Mac). &ndash;

## <a name="using-embeddinator-4000"></a>Использование Ембеддинатор-4000

Чтобы использовать библиотеку .NET в собственном проекте Android, выполните следующие действия.

1. Создайте проект C# Библиотеки Android.

2. Установите [ембеддинатор-4000](https://www.nuget.org/packages/Embeddinator-4000/).

3. Откройте **файл Embeddinator-4000. exe** и добавьте **Mono** в свой путь. Например:

    ```bash
    export TOOLS=~/.nuget/packages/embeddinator-4000/0.4.0/tools
    export PATH=$PATH:/Library/Frameworks/Mono.framework/Commands
    ```

4. Запустите Ембеддинатор-4000 в сборке библиотеки. Например:

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
