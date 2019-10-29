---
title: Начало работы с Java
description: В этом документе описывается, как приступить к использованию внедрения .NET с помощью Java. В нем обсуждаются требования к системе, установка и поддерживаемые платформы.
ms.prod: xamarin
ms.assetid: B9A25E9B-3EC2-489A-8AD3-F78287609747
author: davidortinau
ms.author: daortin
ms.date: 03/28/2018
ms.openlocfilehash: 09ea33724c2b1184654ce7768ea1cb2525b62c28
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73007369"
---
# <a name="getting-started-with-java"></a>Начало работы с Java

Это страница «Приступая к работе» для Java, в которой рассматриваются основные сведения обо всех поддерживаемых платформах.

## <a name="requirements"></a>Требования

Чтобы использовать внедрение .NET с Java, вам потребуется:

* Java 1,8 или более поздней версии
* [Моно 5,0](https://www.mono-project.com/download/)

Для Mac:

* Xcode 8.3.2 или более поздняя версия

Для Windows:

* Visual Studio 2017 с C++ поддержкой
* Пакет SDK для Windows 10

Для Android:

* [Xamarin. Android 7,5](https://visualstudio.microsoft.com/xamarin/) или более поздней версии
* [Android Studio 3. x](https://developer.android.com/studio/index.html) с Java 1,8

Для редактирования и компиляции C# кода можно использовать [Visual Studio для Mac](https://visualstudio.microsoft.com/vs/mac/).

> [!NOTE]
> Более ранние версии Xcode, Visual Studio, Xamarin. Android, Android Studio и Mono _могут_ работать, но не являются тестируемыми и не поддерживаются.

## <a name="installation"></a>Установка

Внедрение .NET в настоящее время доступно в [NuGet](https://www.nuget.org/packages/Embeddinator-4000/):

```shell
nuget install Embeddinator-4000
```

**Файл Embeddinator-4000. exe** будет размещен в каталоге **packages/ембеддинатор-4000/Tools** .

Кроме того, можно создать внедрение .NET из источника. инструкции см. в нашем [репозитории Git](https://github.com/mono/Embeddinator-4000/) и в [сопутствующем](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md) документе.

## <a name="platforms"></a>Платформы

В настоящее время Java находится в состоянии предварительной версии для macOS, Windows и Android.

Платформа выбирается путем передачи аргумента командной строки `--platform=<platform>` в средство внедрения .NET. В настоящее время поддерживаются `macOS`, `Windows`и `Android`.

### <a name="macos-and-windows"></a>macOS и Windows

Для разработки вы можете использовать любые Java IDE, поддерживающие Java 1,8. При необходимости можно даже использовать Android Studio. [см. здесь](https://stackoverflow.com/questions/16626810/can-android-studio-be-used-to-run-standard-java-projects). Выходные данные JAR-файла можно использовать как любой стандартный JAR-файл Java.

### <a name="android"></a>Android

Убедитесь, что вы уже настроили разработку приложений Android, прежде чем пытаться создать их с помощью внедрения .NET. В [следующих инструкциях](~/tools/dotnet-embedding/get-started/java/android.md) предполагается, что вы уже успешно выполнили сборку и развертывание приложения Android с компьютера.

Android Studio рекомендуется для разработки, но другие IDE должны работать при условии поддержки [формата файлов AAR](https://developer.android.com/studio/projects/android-library.html).

## <a name="further-reading"></a>Дополнительные сведения

* [начало работы в Android](~/tools/dotnet-embedding/get-started/java/android.md)
* [Обратные вызовы на Android](~/tools/dotnet-embedding/android/callbacks.md)
* [Предварительные исследования Android](~/tools/dotnet-embedding/android/index.md)
* [Ограничения внедрения .NET](~/tools/dotnet-embedding/limitations.md)
* [Вклад в проект с открытым исходным кодом](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Коды ошибок и описания](~/tools/dotnet-embedding/errors.md)
