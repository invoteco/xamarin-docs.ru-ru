---
title: Как можно просмотреть, какие библиотеки поддерживаются в PCL
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 14FF03BD-AF41-4DB1-B307-2349C13DE7E4
author: davidortinau
ms.author: daortin
ms.date: 07/27/2018
ms.openlocfilehash: 6d6519fc058c91a0f3519be034f8840469f87f0e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73013321"
---
# <a name="how-can-i-view-what-libraries-are-supported-in-a-pcl"></a>Как можно просмотреть, какие библиотеки поддерживаются в PCL

- Общие сведения о различных функциях, поддерживаемых различными целевыми платформами PCL, можно найти в разделе *Поддерживаемые функции* этой страницы: [https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)

- Другой вариант — использовать [анализатор переносимости .NET](https://visualstudiogallery.msdn.microsoft.com/1177943e-cfb7-4822-a8a6-e56c7905292b) для оценки возможности преобразования существующей библиотеки в профиль PCL.

- Третья возможность — Просмотр содержимого действительного профиля, который можно использовать. Например, с помощью профиля 78 вы можете перейти по этой ссылке: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\Profile\Profile78\` и просмотреть все сборки в ней.

При выборе выбранного метода обратите внимание, что некоторые функциональные возможности должны быть загружены с помощью NuGet и библиотеки Microsoft BCL.
