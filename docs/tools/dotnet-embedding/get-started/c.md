---
title: Начало работы с C#
description: В этом документе описывается, как использовать внедрение .NET для внедрения кода .NET в приложение C. В нем обсуждается использование внедрения .NET в Visual Studio 2019 и Visual Studio для Mac.
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
author: conceptdev
ms.author: crdun
ms.date: 04/19/2018
ms.openlocfilehash: 1dc68a709f8e1f864961bbe87af112b648b0dd2a
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70278737"
---
# <a name="getting-started-with-c"></a>Начало работы с C#

## <a name="requirements"></a>Требования

Чтобы использовать внедрение .NET с C, вам потребуется компьютер Mac или Windows, на котором выполняется:

### <a name="macos"></a>macOS

* macOS 10,12 (Сьерра) или более поздней версии
* Xcode 8.3.2 или более поздняя версия
* [Mono](https://www.mono-project.com/download/)

### <a name="windows"></a>Windows

* Windows 7, 8, 10 или более поздние версии
* Visual Studio 2015 или более поздней версии

## <a name="installing-net-embedding-from-nuget"></a>Установка внедрения .NET из NuGet

Выполните эти [инструкции](~/tools/dotnet-embedding/get-started/install/install.md) , чтобы установить и настроить внедрение .NET для проекта.

Вызов команды, который следует настроить, будет выглядеть как (возможно, с разными номерами версий и путями):

### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

```shell
mono {SolutionDir}/packages/Embeddinator-4000.0.4.0.0/tools/Embeddinator-4000.exe --gen=c --outdir=managed_c --platform=macos --compile managed.dll
```

### <a name="visual-studio-2017"></a>Visual Studio 2017

```shell
$(SolutionDir)\packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe --gen=c --outdir=managed_c --platform=windows --compile managed.dll
```

## <a name="generation"></a>Создание

### <a name="output-files"></a>выходные файлы

Если все работает правильно, будут выведены следующие выходные данные:

```shell
Parsing assemblies...
    Parsed 'managed.dll'
Processing assemblies...
Generating binding code...
    Generated: managed.h
    Generated: managed.c
    Generated: mscorlib.h
    Generated: mscorlib.c
    Generated: embeddinator.h
    Generated: glib.c
    Generated: glib.h
    Generated: mono-support.h
    Generated: mono_embeddinator.c
    Generated: mono_embeddinator.h
```

Так как флаг был передан средству, при внедрении .NET также должны быть скомпилированы выходные файлы в общую библиотеку, которую можно найти рядом с созданными файлами, файлом либманажед. dylib на macOS и **Managed. dll** в Windows. `--compile`

Для использования общей библиотеки можно включить **управляемый h** -файл заголовка c, который предоставляет объявления c, соответствующие соответствующим управляемым API-интерфейсам управляемых библиотек, и связать с ранее упомянутой общей библиотекой.

## <a name="further-reading"></a>Дополнительные сведения

* [Ограничения внедрения .NET](~/tools/dotnet-embedding/limitations.md)
* [Вклад в проект с открытым исходным кодом](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Коды ошибок и описания](~/tools/dotnet-embedding/errors.md)
