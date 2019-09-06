---
title: Как скопировать выходные файлы IPA в папку сброса TFS?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B0F1E09E-7315-45BA-B7FF-44D2063EE19C
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: 9c7b7e598f66d930b5e16ef19b8bc108d8b6701a
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291056"
---
# <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folder"></a>Как скопировать выходные файлы IPA в папку сброса TFS?

Откройте файл для проекта приложения iOS в текстовом редакторе, а затем добавьте в конец файла следующие строки (непосредственно перед закрывающим `</Project>` тегом): `.csproj`

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

- Это та же общая методика, о которой [можно изменить выходной путь к файлу IPA?](~/ios/troubleshooting/questions/ipa-output-path.md). Две важные точки `$(TF_BUILD_BINARIESDIRECTORY)` задаются в качестве папки назначения и добавляют дополнительное условие, `CopyIpa` которое будет выполняться только для сборок TFS.

- Описание `TF_BUILD_BINARIESDIRECTORY` см. в разделе [стандартные переменные сборки](https://docs.microsoft.com/azure/devops/pipelines/build/variables).

## <a name="additional-references"></a>Дополнительные ссылки

- [Документация по установке TFS для использования с Xamarin](https://docs.microsoft.com/azure/devops/repos/tfvc/overview)
- [Задача сборки DevOps Azure: Xamarin.Android](https://docs.microsoft.com/azure/devops/pipelines/tasks/build/xamarin-android)
- [Задача сборки DevOps Azure: Xamarin.iOS](https://docs.microsoft.com/azure/devops/pipelines/tasks/build/xamarin-ios)

### <a name="next-steps"></a>Следующие шаги

В этом документе рассматривается текущее поведение 3.11.666 Xamarin для Visual Studio и Xamarin. iOS 8.10.3 на узле сборки Mac. Для получения дополнительной помощи, для связи с нами или если проблема остается даже после использования приведенных выше сведений, ознакомьтесь с возможностями [поддержки Xamarin?](~/cross-platform/troubleshooting/support-options.md) дополнительные сведения о вариантах контактов, предложениях и о том, как создать новую ошибку при необходимости .
