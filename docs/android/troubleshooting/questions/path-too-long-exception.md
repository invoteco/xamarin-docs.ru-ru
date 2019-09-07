---
title: Разделы справки устранить ошибку PathTooLongException?
description: В этой статье объясняется, как разрешить PathTooLongException, которые могут возникнуть при создании приложения.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 60EE1C8D-BE44-4612-B3B5-70316D71B1EA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/29/2018
ms.openlocfilehash: 915f557db7955dc7b8b9f1bc5e014a683740052b
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760818"
---
# <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>Разделы справки устранить ошибку PathTooLongException?

## <a name="cause"></a>Причина

Созданные имена путей в проекте Xamarin. Android могут быть довольно длинными.
Например, во время сборки можно создать следующий путь:

**C:\\Some\\Directory\\Solution\\Project\\obj\\Debug\\__library_projects__\\Xamarin.Forms.Platform.Android\\library_project_imports\\assets**

В Windows (где максимальная длина пути составляет [260 символов](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx)), **PathTooLongException** может быть создан при сборке проекта, если размер созданного пути превышает максимальную длину. 

## <a name="fix"></a>Fix

По умолчанию свойство `True` MSBuildимеетзначение,чтобыобойтиэтуошибку.`UseShortFileNames` Если это свойство имеет значение `True`, процесс сборки использует более короткие имена путей, чтобы снизить вероятность создания **PathTooLongException**.
Например, если `UseShortFileNames` имеет `True`значение, приведенный выше путь сокращается до пути, аналогичного следующему:

**В.\\некоторые\\ресурсы\\для\\\\отладкиLP\\1 ЖЛресурсов\\проекта решения каталога\\\\\\**

Чтобы задать это свойство вручную, добавьте следующее свойство MSBuild в файл Project **. csproj** :

```xml
<PropertyGroup>
    <UseShortFileNames>True</UseShortFileNames>
</PropertyGroup>
```

Если установка этого флага не приводит к устранению ошибки **PathTooLongException** , то другой подход заключается в указании [общего корневого корня выходных данных](https://blogs.msdn.microsoft.com/kirillosenkov/2015/04/04/using-a-common-intermediate-and-output-directory-for-your-solution/) для проектов `IntermediateOutputPath` в решении путем установки в файле Project **. csproj** . Попробуйте использовать относительно короткий путь. Например:

```xml
<PropertyGroup>
    <IntermediateOutputPath>C:\Projects\MyApp</IntermediateOutputPath>
</PropertyGroup>
```

Дополнительные сведения о настройке свойств сборки см. в разделе [процесс сборки](~/android/deploy-test/building-apps/build-process.md).
