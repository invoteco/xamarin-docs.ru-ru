---
title: Разделы справки устранить ошибку PathTooLongException?
description: В этой статье объясняется, как разрешить PathTooLongException, которые могут возникнуть при создании приложения.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 60EE1C8D-BE44-4612-B3B5-70316D71B1EA
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/29/2018
ms.openlocfilehash: ffe88546ff58387865d71268bd64ec05c8aec3c5
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73026791"
---
# <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>Разделы справки устранить ошибку PathTooLongException?

## <a name="cause"></a>Причина

Созданные имена путей в проекте Xamarin. Android могут быть довольно длинными.
Например, во время сборки можно создать следующий путь:

**В.\\некоторые\\Directory\\решение\\проект\\obj\\Debug\\__library_projects__\\Xamarin. Forms. Platform. Android\\library_project_imports\\ресурсов**

В Windows (где максимальная длина пути составляет [260 символов](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx)), **PathTooLongException** может быть создан при сборке проекта, если размер созданного пути превышает максимальную длину. 

## <a name="fix"></a>Исправление

Для свойства `UseShortFileNames` MSBuild установлено значение `True`, чтобы обойти эту ошибку по умолчанию. Если это свойство имеет значение `True`, процесс сборки использует более короткие имена путей, чтобы снизить вероятность создания **PathTooLongException**.
Например, если `UseShortFileNames` имеет значение `True`, то приведенный выше путь сокращается до пути, аналогичного следующему:

**В.\\некоторые\\Directory\\решение\\проект\\obj\\Debug\\LP\\1\\ЖЛ\\активы**

Чтобы задать это свойство вручную, добавьте следующее свойство MSBuild в файл Project **. csproj** :

```xml
<PropertyGroup>
    <UseShortFileNames>True</UseShortFileNames>
</PropertyGroup>
```

Если установка этого флага не приводит к устранению ошибки **PathTooLongException** , другой подход заключается в указании [общего корневого корня выходных данных](https://blogs.msdn.microsoft.com/kirillosenkov/2015/04/04/using-a-common-intermediate-and-output-directory-for-your-solution/) для проектов в решении путем установки `IntermediateOutputPath` в файле Project **. csproj** . Попробуйте использовать относительно короткий путь. Пример:

```xml
<PropertyGroup>
    <IntermediateOutputPath>C:\Projects\MyApp</IntermediateOutputPath>
</PropertyGroup>
```

Дополнительные сведения о настройке свойств сборки см. в разделе [процесс сборки](~/android/deploy-test/building-apps/build-process.md).
