---
title: Как скопировать выходные файлы IPA в папку сброса TFS?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B0F1E09E-7315-45BA-B7FF-44D2063EE19C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 4493b1a0d06e2f44ee9a11a250395f058baa0548
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031018"
---
# <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folder"></a>Как скопировать выходные файлы IPA в папку сброса TFS?

Откройте файл `.csproj` для проекта приложения iOS в текстовом редакторе, а затем добавьте следующие строки в конце (непосредственно перед закрывающим тегом `</Project>`):

```xml
<PropertyGroup>
    <CreateIpaDependsOn>
        $(CreateIpaDependsOn);
        CopyIpa
    </CreateIpaDependsOn>
</PropertyGroup>

<Target Name="CopyIpa"
    Condition="'$(OutputType)' == 'Exe'
        And '$(ComputedPlatform)' == 'iPhone'
        And '$(BuildIpa)' == 'true'
        And '$(TF_BUILD)' == 'true'">
    <Copy
        SourceFiles="$(OutputPath)$(IpaPackageName)"
        DestinationFolder="$(TF_BUILD_BINARIESDIRECTORY)"
    />
</Target>
```

## <a name="notes"></a>Примечания

- Это та же общая методика, о которой [можно изменить выходной путь к файлу IPA?](~/ios/troubleshooting/questions/ipa-output-path.md). Два важных момента задают `$(TF_BUILD_BINARIESDIRECTORY)` в качестве папки назначения и добавляют дополнительное условие, поэтому `CopyIpa` будет выполняться только для сборок TFS.

- Описание `TF_BUILD_BINARIESDIRECTORY` см. в разделе [стандартные переменные сборки](https://docs.microsoft.com/azure/devops/pipelines/build/variables).

## <a name="additional-references"></a>Дополнительные ссылки

- [Документация по установке TFS для использования с Xamarin](https://docs.microsoft.com/azure/devops/repos/tfvc/overview)
- [Задача сборки Azure DevOps: Xamarin. Android](https://docs.microsoft.com/azure/devops/pipelines/tasks/build/xamarin-android)
- [Задача сборки Azure DevOps: Xamarin. iOS](https://docs.microsoft.com/azure/devops/pipelines/tasks/build/xamarin-ios)

### <a name="next-steps"></a>Следующие шаги

В этом документе рассматривается текущее поведение 3.11.666 Xamarin для Visual Studio и Xamarin. iOS 8.10.3 на узле сборки Mac. Для получения дополнительной помощи, для связи с нами или если проблема остается даже после использования приведенных выше сведений, ознакомьтесь с возможностями [поддержки Xamarin?](~/cross-platform/troubleshooting/support-options.md) дополнительные сведения о вариантах контактов, предложениях и о том, как создать новую ошибку при необходимости .
