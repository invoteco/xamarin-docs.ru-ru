---
title: Установка внедрение .NET
description: В этом документе описывается установка внедрение .NET. Он описывает, как для запуска средства вручную, как создать привязки автоматически, как использовать пользовательские целевые объекты MSBuild и необходимые действия после сборки.
ms.prod: xamarin
ms.assetid: 47106AF3-AC6E-4A0E-B30B-9F73C116DDB3
author: chamons
ms.author: chhamo
ms.date: 04/18/2018
ms.openlocfilehash: 2a572748c21d2a640add3346d1162f4b6bdc8e99
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61076643"
---
# <a name="installing-net-embedding"></a>Установка внедрение .NET

## <a name="installing-net-embedding-from-nuget"></a>Установка внедрение .NET из NuGet

Выберите **Добавить > Добавить пакеты NuGet...**  и установить **Embeddinator до 4000** из диспетчера пакетов NuGet:

![Диспетчер пакетов NuGet](images/visualstudionuget.png)

Эта команда установит **Embeddinator 4000.exe** и **objcgen** в **пакетов/Embeddinator-4000/tools** каталога.

Как правило последний выпуск Embeddinator до 4000 должен быть выбран для загрузки. Objective-C для поддержки требуется 0,4 или более поздней версии.

## <a name="running-manually"></a>Запуск вручную

Теперь, когда установлен NuGet, средства можно запустить вручную.

- Откройте терминал (macOS) или командную строку (Windows)
- Перейдите в каталог в корневой каталог решений
- Инструментарий устанавливается в:
    - **. / пакеты/Embeddinator-4000. [VERSION] / tools/objcgen** (Objective-C)
    - **. / пакеты/Embeddinator-4000. [VERSION]/tools/Embeddinator-4000.exe** (Java/C)
- В Mac OS **objcgen** можно запустить напрямую.
- В Windows **Embeddinator 4000.exe** можно запустить напрямую.
- В Mac OS **Embeddinator 4000.exe** должно запускаться с **mono**:
    - `mono ./packages/Embeddinator-4000.[VERSION]/tools/Embeddinator-4000.exe`

Каждый вызов команды потребуется ряд параметров, перечисленных в документации по конкретной платформы.

## <a name="automatic-binding-generation"></a>Автоматическая привязка поколения

Существует два подхода для автоматического запуска внедрение .NET частью процесса сборки.

- Пользовательские целевые объекты MSBuild
- Действия после сборки

Хотя в этом документе описываются оба, используя пользовательский целевой объект MSBuild является предпочтительным. Шаги построения после не будет обязательно будет запущен при построении из командной строки.

### <a name="custom-msbuild-targets"></a>Пользовательские целевые объекты MSBuild

Чтобы настроить сборки с помощью целевых объектов MSbuild, сначала создайте **Embeddinator 4000.targets** файл рядом с вашей csproj, аналогичную следующей:

```xml
<Project DefaultTargets="Build" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <Target Name="RunEmbeddinator" AfterTargets="AfterBuild" Inputs="$(OutputPath)/$(AssemblyName).dll" Outputs="$(IntermediateOutputPath)/Embeddinator/$(AssemblyName).framework/$(AssemblyName)">
        <Exec Command="" />
    </Target>
</Project>
```

Здесь текст команды должен быть заполнен один из вызовов внедрение .NET, которые перечислены в документации платформы.

Для использования этой цели:

- Закройте проект в Visual Studio 2017 или Visual Studio для Mac
- Откройте csproj библиотеки в текстовом редакторе
- Добавьте следующую строку внизу выше последней `</Project>` строки:

```xml
 <Import Project="Embeddinator-4000.targets" />
```

- Повторно откройте проект

### <a name="post-build-steps"></a>Действия после сборки

Шаги, чтобы добавить действие после сборки для запуска, внедрение .NET зависят от интегрированной среды разработки.

#### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

В Visual Studio для Mac, перейдите в раздел **параметры проекта > Создать > пользовательских команд** и добавьте **после построения** шаг.

Настройка команды, перечисленные в документации по конкретной платформы.

> [!NOTE]
> Обязательно используйте номер версии, установленный на NuGet

Если вы собираетесь делать текущих операций разработки на C# проекта, можно также добавить пользовательскую команду для очистки **вывода** каталог до выполнения внедрение .NET:

```shell
rm -Rf '${SolutionDir}/output/'
```

![Настраиваемое действие](images/visualstudiocustombuild.png)

> [!NOTE]
> Созданную привязку, помещаются в каталог, указанный `--outdir` или `--out` параметра. Имя созданного привязки могут отличаться, когда некоторые платформы имеют ограничения на имена пакетов.

#### <a name="visual-studio-2017"></a>Visual Studio 2017

По сути мы определим то же самое, но будут немного другими меню в Visual Studio 2017. Команды оболочки также могут немного отличаться.

Перейдите к **параметры проекта > события построения** и введите следующую команду в документации по конкретной платформы в **Командная строка события после построения** поле. Пример:

![.NET, на основе Windows](images/visualstudiowindows.png)
