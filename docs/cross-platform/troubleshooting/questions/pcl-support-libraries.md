---
title: Как можно просмотреть, какие библиотеки поддерживаются в PCL
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 14FF03BD-AF41-4DB1-B307-2349C13DE7E4
author: asb3993
ms.author: amburns
ms.date: 07/27/2018
ms.openlocfilehash: 7e1017baf7daed68b5e55319a9ce13a4a2df5f2e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61341269"
---
# <a name="how-can-i-view-what-libraries-are-supported-in-a-pcl"></a>Как можно просмотреть, какие библиотеки поддерживаются в PCL

- Вы найдете обзор различных функций, поддерживаемых в различных целевых платформ в переносимой библиотеки Классов в разделе *поддерживаемые функции* часть этой страницы: [https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)

- Другой вариант — использовать [анализатор переносимости .NET](https://visualstudiogallery.msdn.microsoft.com/1177943e-cfb7-4822-a8a6-e56c7905292b) для оценки, можно ли преобразовать существующие библиотеки на профиль PCL.

- Третье — это для просмотра содержимого фактическое профиля, который можно использовать. Например с помощью профиля 78 может открыться здесь: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\Profile\Profile78\` И просмотреть все сборки в ней.

Какой бы метод вы выбрали, пожалуйста Обратите внимание, что некоторые функциональные возможности должен быть загружен с помощью NuGet и библиотеки Microsoft BCL.
