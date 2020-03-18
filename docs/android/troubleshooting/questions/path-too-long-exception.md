---
title: Как устранить ошибку PathTooLongException?
description: В этой статье объясняется, как разрешить ошибки PathTooLongException, которые могут возникнуть при создании приложения.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 60EE1C8D-BE44-4612-B3B5-70316D71B1EA
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/29/2018
ms.openlocfilehash: ffe88546ff58387865d71268bd64ec05c8aec3c5
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2020
ms.locfileid: "73026791"
---
# <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>Как устранить ошибку PathTooLongException?

## <a name="cause"></a>Причина:

Созданные имена путей в проекте Xamarin.Android могут быть довольно длинными.
Например, во время сборки может быть создан следующий путь:

**C:\\Some\\Directory\\Solution\\Project\\obj\\Debug\\__library_projects__\\Xamarin.Forms.Platform.Android\\library_project_imports\\assets**

В Windows (где максимальная длина пути — [260 символов](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx)), **PathTooLongException** может появиться при построении проекта, если размер созданного пути превышает максимальную длину. 

## <a name="fix"></a>Исправление

Чтобы обойти эту ошибку по умолчанию, для свойства `UseShortFileNames` MSBuild установлено значение `True`. Если это свойство имеет значение `True`, процесс сборки использует более короткие имена путей, чтобы снизить вероятность создания **PathTooLongException**.
Например, если `UseShortFileNames` имеет значение `True`, то приведенный выше путь сокращается до пути, аналогичного следующему:

**C:\\Some\\Directory\\Solution\\Project\\obj\\Debug\\lp\\1\\jl\\assets**

Чтобы задать это свойство вручную, добавьте следующее свойство MSBuild в файл проекта **.csproj**:

```xml
<PropertyGroup>
    <UseShortFileNames>True</UseShortFileNames>
</PropertyGroup>
```

Если установка этого флага не приводит к устранению ошибки **PathTooLongException**, другой подход заключается в указании [общего промежуточного корневого каталога](https://blogs.msdn.microsoft.com/kirillosenkov/2015/04/04/using-a-common-intermediate-and-output-directory-for-your-solution/) для проектов в решении путем установки `IntermediateOutputPath` в файле проекта **.csproj**. Попробуйте использовать относительно короткий путь. Пример:

```xml
<PropertyGroup>
    <IntermediateOutputPath>C:\Projects\MyApp</IntermediateOutputPath>
</PropertyGroup>
```

Дополнительные сведения о настройке свойств сборки см. в статье [Процесс сборки](~/android/deploy-test/building-apps/build-process.md).
