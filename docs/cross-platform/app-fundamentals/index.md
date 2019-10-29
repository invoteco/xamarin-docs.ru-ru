---
title: Совместное использование кода на нескольких платформах
description: Этот документ содержит ссылки на различные руководства, описывающие методы совместного использования кода, включая переносимые библиотеки классов, общие проекты, .NET Standard и NuGet.
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
author: davidortinau
ms.author: daortin
ms.date: 07/18/2018
ms.openlocfilehash: a91fba3cd1fba3bcf2317e8f9cb25631c62491cb
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016826"
---
# <a name="sharing-code-on-multiple-platforms"></a>Совместное использование кода на нескольких платформах

В этих статьях объясняются различные варианты совместного использования кода на разных платформах, включая Windows, Android, iOS и другие.

## <a name="code-sharing-overviewcode-sharingmd"></a>[Общие сведения о совместном использовании кода](code-sharing.md)

Узнайте о различных вариантах совместного использования кода, доступных для проектов Xamarin, включая библиотеки .NET Standard и общие проекты. Также поддерживаются переносимые библиотеки классов, однако они считаются нерекомендуемыми в пользу .NET Standard.

## <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET Standard является предпочтительным вариантом совместного использования кода на разных платформах. Код создан на основе определенной версии (2,0 обеспечивает лучшую совместимость API с существующим кодом .NET Framework) и может использоваться другими проектами, поддерживающими этот уровень или выше. .NET Standard проекты поддерживаются как в Visual Studio 2019, так и в Visual Studio 2019 для Mac.

## <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[Общие проекты](~/cross-platform/app-fundamentals/shared-projects.md)

Общие проекты позволяют писать общий код, на который ссылается несколько различных проектов приложений. Код компилируется как часть каждого ссылающегося проекта и может включать директивы компилятора, которые помогают внедрить функции, зависящие от платформы, в общей базе кода. В этой статье рассматривается работа общих проектов и способы их создания и использования в проектах Xamarin.

## <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[Переносимые библиотеки классов](~/cross-platform/app-fundamentals/pcl.md)

Проекты переносимых библиотек классов позволяют создавать и распределять сборки, содержащие общий код, для запуска на нескольких платформах. Чтобы создать переносимую библиотеку классов (или PCL), сначала выберите целевые платформы, а затем напишите код для поднабора .NET Framework, доступного в профиле, определенном для этих платформ. PCL считаются устаревшими в последних версиях Visual Studio; разработчикам рекомендуется использовать вместо .NET Standard 2,0.

## <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[Проекты NuGet: многоплатформенные библиотеки для совместного использования кода](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

Пакеты NuGet могут быть автоматически созданы из проектов PCL или .NET Standard. и общие проекты можно упаковывать в пакеты NuGet "Bait и Switch", используя отдельный тип проекта NuGet. В этом разделе объясняется, как создать пакеты NuGet для каждого сценария совместного использования кода.

## <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[Создание пакетов NuGet для Xamarin вручную](~/cross-platform/app-fundamentals/nuget-manual.md)

Советы по созданию пакетов NuGet, работающих с платформой Xamarin.

## <a name="use-cc-libraries-in-cross-platform-xamarin-projectscross-platformcppindexmd"></a>[Использование C/C++ Library в кросс-платформенных проектах Xamarin](~/cross-platform/cpp/index.md)

Эта методика позволяет отделить развитие библиотек C/C++ , C# привязки в NuGet и ваших приложений Xamarin. Функциональные возможности обеспечиваются с помощью собственной платформы C/C++ библиотеки, но весь код, зависящий от платформы, изолирован от окончательного приложения Xamarin, что позволяет максимально возможной производительности без дублирования кода. 
