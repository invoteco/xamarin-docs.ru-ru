---
title: Поддержка .NET standard 2.0 в Xamarin.Forms
description: В этой статье объясняется, как преобразование приложения Xamarin.Forms с помощью .NET Standard 2.0. .NET standard представляет собой спецификацию интерфейсов API .NET, которые должны быть доступны во всех реализациях .NET.
ms.prod: xamarin
ms.assetid: 95805355-63a7-44e7-a3c6-6487a6276ab2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: c3e46592bb8760ff85eaeb5dce119897a97dfe89
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2019
ms.locfileid: "57667859"
---
# <a name="net-standard-20-support-in-xamarinforms"></a>Поддержка .NET standard 2.0 в Xamarin.Forms

_В этой статье объясняется, как преобразование приложения Xamarin.Forms с помощью .NET Standard 2.0._

.NET standard представляет собой спецификацию интерфейсов API .NET, которые должны быть доступны во всех реализациях .NET. Он упрощает совместное использование кода на классические приложения, мобильные приложения и игры и облачные службы, предложив идентичные API-интерфейсы для различных платформ. Сведения о платформах, поддерживаемых в .NET Standard, см. в разделе [поддержка реализации .NET](/dotnet/standard/net-standard#net-implementation-support).

Библиотеки .NET standard являются заменой для переносимых библиотек классов (PCL). Тем не менее библиотека, нацеленная .NET Standard по-прежнему переносимую библиотеку Классов и называется переносимой библиотеки Классов .NET Standard на базе. Некоторые профили PCL, сопоставляются с версии .NET Standard, а для профилей, сопоставленных типов двух библиотек будут иметь возможность ссылаться друг на друга. Дополнительные сведения см. в разделе [совместимость библиотек PCL](/dotnet/standard/net-standard#pcl-compatibility).

Xamarin.Forms 2.4 позволяет приложениям Xamarin.Forms целевой .NET Standard 2.0, заменив в переносимую библиотеку Классов с помощью библиотеки .NET Standard 2.0. Это можно сделать следующим образом:

- Убедитесь, [.NET Core 2.0](https://www.microsoft.com/net/download/core) установлен.
- Обновите решение Xamarin.Forms использовать Xamarin.Forms 2.4 или более поздней версии.
- Добавьте в решение, ориентированном на .NET Standard 2.0 библиотеки .NET Standard.
- Удалите класс, который добавляется в библиотеку .NET Standard.
- Добавьте пакет NuGet Xamarin.Forms 2,4 (или более поздней версии), библиотеку .NET Standard.
- В проектах платформы добавьте ссылку на библиотеку .NET Standard и удалите ссылку на проект переносимой библиотеки Классов, содержащий логика пользовательского интерфейса Xamarin.Forms.
- Скопируйте файлы из проекта переносимой библиотеки Классов для библиотеки .NET Standard.
- Удалите проект переносимой библиотеки Классов, содержащий логика пользовательского интерфейса Xamarin.Forms.


## <a name="related-links"></a>Связанные ссылки

- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [Варианты общего доступа к коду](~/cross-platform/app-fundamentals/code-sharing.md)
