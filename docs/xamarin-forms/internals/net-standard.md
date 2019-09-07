---
title: Поддержка .NET Standard 2,0 в Xamarin. Forms
description: В этой статье объясняется, как преобразовать приложение Xamarin. Forms для использования .NET Standard 2,0. .NET Standard — спецификация API-интерфейсов .NET, которые должны быть доступны во всех реализациях .NET.
ms.prod: xamarin
ms.assetid: 95805355-63a7-44e7-a3c6-6487a6276ab2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: eaef607e3e6095ccc7dfb1f5831d2384d11cab5f
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760063"
---
# <a name="net-standard-20-support-in-xamarinforms"></a>Поддержка .NET Standard 2,0 в Xamarin. Forms

_В этой статье объясняется, как преобразовать приложение Xamarin. Forms для использования .NET Standard 2,0._

.NET Standard — спецификация API-интерфейсов .NET, которые должны быть доступны во всех реализациях .NET. Это упрощает совместное использование кода в настольных приложениях, мобильных приложениях и играх, а также в облачных службах, применяя идентичные API к различным платформам. Сведения о платформах, поддерживаемых .NET Standard, см. в разделе [Поддержка реализации .NET](/dotnet/standard/net-standard#net-implementation-support).

Библиотеки .NET Standard являются заменой переносимых библиотек классов (PCL). Однако библиотека, предназначенная для .NET Standard, по-прежнему является PCL и называется PCL на основе .NET Standard. Некоторые профили PCL сопоставляются с .NET Standard версиями, а для профилей с сопоставлением эти два типа библиотек могут ссылаться друг на друга. Дополнительные сведения см. в разделе [Совместимость с PCL](/dotnet/standard/net-standard#pcl-compatibility).

Xamarin. Forms 2,4 позволяет приложениям Xamarin. Forms нацелиться на .NET Standard 2,0, заменяя PCL на библиотеку .NET Standard 2,0. Это можно сделать следующим образом.

- Убедитесь, что установлен [.NET Core 2,0](https://www.microsoft.com/net/download/core) .
- Обновите решение Xamarin. Forms, чтобы использовать Xamarin. Forms 2,4 или более поздней версии.
- Добавьте в решение библиотеку .NET Standard, предназначенную для .NET Standard 2,0.
- Удалите класс, добавленный в библиотеку .NET Standard.
- Добавьте пакет NuGet Xamarin. Forms 2,4 (или более позднюю) в библиотеку .NET Standard.
- В проектах платформы добавьте ссылку на библиотеку .NET Standard и удалите ссылку на проект PCL, содержащий логику пользовательского интерфейса Xamarin. Forms.
- Скопируйте файлы из проекта PCL в библиотеку .NET Standard.
- Удалите проект PCL, содержащий логику пользовательского интерфейса Xamarin. Forms.

## <a name="related-links"></a>Связанные ссылки

- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [Варианты общего доступа к коду](~/cross-platform/app-fundamentals/code-sharing.md)
