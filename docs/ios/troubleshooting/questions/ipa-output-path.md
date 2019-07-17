---
title: Можно изменить путь к выходной IPA-файл?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5E5DCC6-F7CC-48E2-89E8-709E9C269502
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 8b0686a91f18b41aa8e2e7db071123c0d96723a0
ms.sourcegitcommit: 32c7cf8b0d00464779e4b0ea43e2fd996632ebe0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2019
ms.locfileid: "68290105"
---
# <a name="can-i-change-the-output-path-of-the-ipa-file"></a>Можно изменить путь к выходной IPA-файл?

## <a name="for-cycle-7-and-higher"></a>Для цикла 7 и более поздних версий
Да, можно использовать настраиваемые целевые объекты MSBuild для достижения этой цели. Самый простой вариант является, вероятно, скопируйте `.ipa` файл после его построения.

Эти действия будут работать для любого проекта iOS, которое использует подсистема сборки MSBuild на компьютере Mac или Windows. (Примечание: все проекты единый API используют подсистема сборки MSBuild.)

1. Откройте `.csproj` файл для проекта приложения iOS в текстовом редакторе и затем добавьте следующие строки в конце (непосредственно перед закрывающим `</Project>` тега):
    
    ```
    <PropertyGroup>
           <CreateIpaDependsOn>
           $(CreateIpaDependsOn);
            CopyIpa
           </CreateIpaDependsOn>
    </PropertyGroup>
    
    <Target Name="CopyIpa"
        Condition="'$(OutputType)' == 'Exe'
            And '$(ComputedPlatform)' == 'iPhone'
            And '$(BuildIpa)' == 'true'">
        <Copy
            SourceFiles="$(IpaPackagePath)"
            DestinationFolder="$(OutputPath)"
        />
    </Target>
    ```

2. Присвоить папке поставленной DestinationFolder. Как обычно вы можете использовать свойства MSBuild (например, $(OutputPath)) в этот аргумент, при необходимости.

## <a name="notes"></a>Примечания
- `CreateIpaDependsOn` Свойство определено в `Xamarin.iOS.Common.targets` файл, который является частью Xamarin.iOS. Он ведет себя, как описано в разделе [переопределение предопределенных целевых объектов](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process#overriding-predefined-targets) статьи [как: Расширение процесса построения Visual Studio](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process).

- Можно использовать **переместить** задач вместо **копирования** задач при желании вы. Если выбран этот вариант и создаете на Windows, необходимо использовать имя полного задачи `<Microsoft.Build.Tasks.Move>` во избежание неоднозначности с XamarinVS задачи сборки.

## <a name="for-versions-before-xamarin-studio-6005174--xamarin-for-visual-studio-410530"></a>Для версий до Xamarin Studio 6.0.0.5174 | Xamarin для Visual Studio 4.1.0.530

Да, можно использовать настраиваемые целевые объекты MSBuild для достижения этой цели. Самый простой вариант является, вероятно, скопируйте `.ipa` файл после его построения.

Эти действия будут работать для любого проекта iOS, которое использует подсистема сборки MSBuild на компьютере Mac или Windows. (Примечание: все проекты единый API используют подсистема сборки MSBuild.)

1. Откройте `.csproj` файл для проекта приложения iOS в текстовом редакторе и затем добавьте следующие строки в конце (непосредственно перед закрывающим `</Project>` тега).

    ```csharp
    <PropertyGroup>
        <CreateIpaDependsOn>
            $(CreateIpaDependsOn);
            CopyIpa
        </CreateIpaDependsOn>
    </PropertyGroup>
    
    <Target Name="CopyIpa"
        Condition="'$(OutputType)' == 'Exe'
            And '$(ComputedPlatform)' == 'iPhone'
            And '$(BuildIpa)' == 'true'">
        <Copy
            SourceFiles="$(OutputPath)$(IpaPackageName)"
            DestinationFolder="/Users/macuser/Desktop/"
        />
    </Target>
    ```

2. Задайте `DestinationFolder` к требуемой папке выходных данных. Как обычно вы можете использовать свойства MSBuild (например `$(OutputPath)`) в этот аргумент, при необходимости.

## <a name="notes"></a>Примечания
- `CreateIpaDependsOn` Свойство определено в `Xamarin.iOS.Common.targets` файл, который является частью Xamarin.iOS. t ведет себя так, как описано в разделе [переопределение предопределенных целевых объектов](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process#overriding-predefined-targets) статьи [как: Расширение процесса построения Visual Studio](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process).

- Можно использовать **переместить** задач вместо **копирования** задач при желании вы. Если выбран этот вариант и создаете на Windows, необходимо использовать имя полного задачи `<Microsoft.Build.Tasks.Move>` во избежание неоднозначности с XamarinVS задачи сборки.
