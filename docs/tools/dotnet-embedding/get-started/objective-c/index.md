---
title: Приступая к работе с объективом-C
description: В этом документе описывается, как приступить к использованию внедрения .NET с помощью цели-C. В нем обсуждаются требования, установка внедрения .NET из NuGet и поддерживаемых платформ.
ms.prod: xamarin
ms.assetid: 4ABC0247-B608-42D4-89CB-D2E598097142
author: conceptdev
ms.author: crdun
ms.date: 11/14/2017
ms.openlocfilehash: 66b99e0da574c50df32afedb1dd6dc9de315b347
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70278455"
---
# <a name="getting-started-with-objective-c"></a>Приступая к работе с объективом-C

Это страница Приступая к работе для цели-C, в которой рассматриваются основные сведения обо всех поддерживаемых платформах.

## <a name="requirements"></a>Требования

Чтобы использовать внедрение .NET с целью-C, вам потребуется компьютер под управлением Mac:

- macOS 10,12 (Сьерра) или более поздней версии
- Xcode 8.3.2 или более поздняя версия
- [Моно 5,0](https://www.mono-project.com/download/)

Вы можете установить [Visual Studio для Mac](https://visualstudio.microsoft.com/vs/mac/) , чтобы изменить и скомпилировать C# код.

> [!NOTE]
> - Более ранние версии macOS, Xcode и Mono _могут_ работать, но они не являются тестируемыми и не поддерживаются.
> - Создание кода может выполняться в Windows, но его можно скомпилировать только на компьютере Mac, где установлена XCode

## <a name="installing-net-embedding-from-nuget"></a>Установка внедрения .NET из NuGet

Выполните эти [инструкции](~/tools/dotnet-embedding/get-started/install/install.md) , чтобы установить и настроить внедрение .NET для проекта.

Пример вызова команды приведен в руководствах по началу работы с [macOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md) и [iOS](~/tools/dotnet-embedding/get-started/objective-c/ios.md) .

## <a name="platforms"></a>Платформы

Цель-C — это язык, который чаще всего используется для написания приложений для macOS, iOS, tvOS и watchOS; внедрение .NET поддерживает все эти платформы. Работа с каждой платформой подразумевает некоторые [Ключевые отличия, которые описаны здесь](~/tools/dotnet-embedding/objective-c/platforms.md).

### <a name="macos"></a>macOS

[Создание приложения macOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md) проще, так как оно не требует выполнения множества дополнительных действий, например настройки удостоверений, профилей подготовку, симуляторов и устройств. Рекомендуется начать с документа macOS до того, как он будет создан для iOS.

### <a name="ios--tvos"></a>iOS и tvOS

Убедитесь, что вы уже настроили разработку приложений iOS, прежде чем пытаться создать их с помощью внедрения .NET. В [следующих инструкциях](~/tools/dotnet-embedding/get-started/objective-c/ios.md) предполагается, что вы уже успешно выполнили сборку и развертывание приложения iOS на компьютере.

Поддержка tvOS аналогична работе iOS с помощью проектов tvOS в IDE (как Visual Studio, так и Xcode) вместо проектов iOS.

> [!NOTE]
> Поддержка watchOS будет доступна в следующем выпуске и будет очень похожа на iOS/tvOS.

## <a name="further-reading"></a>Дополнительные сведения

- [Функции внедрения .NET, относящиеся к цели-C](~/tools/dotnet-embedding/objective-c/index.md)
- [Рекомендации по цели-C](~/tools/dotnet-embedding/objective-c/best-practices.md)
- [Ограничения внедрения .NET](~/tools/dotnet-embedding/limitations.md)
- [Вклад в проект с открытым исходным кодом](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
- [Коды ошибок и описания](~/tools/dotnet-embedding/errors.md)
- [Целевые платформы](~/tools/dotnet-embedding/objective-c/platforms.md)

## <a name="related-links"></a>Связанные ссылки

- [Образец Weather (iOS & macOS)](https://github.com/jamesmontemagno/embeddinator-weather)
