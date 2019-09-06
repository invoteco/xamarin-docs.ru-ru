---
title: Привязка Objective-C
description: Этот документ содержит ссылки на различные руководства, в которых описывается создание C# привязок к коду цели-C, позволяя разработчикам использовать готовые библиотеки в приложениях Xamarin.
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
author: conceptdev
ms.author: crdun
ms.date: 01/25/2016
ms.openlocfilehash: 8975bb899127d0761359bec695cd084f89c12972
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290562"
---
# <a name="binding-objective-c"></a>Привязка Objective-C

В этом разделе содержатся разнообразные документы, посвященные созданию привязок к библиотекам цели-C, которые можно вызывать из C# приложений, созданных с помощью Xamarin. iOS или Xamarin. Mac.

## <a name="overviewcross-platformmaciosbindingoverviewmd"></a>[Обзор](~/cross-platform/macios/binding/overview.md)

В этом документе содержатся некоторые из внутренних компонентов того, как происходит привязка. Это расширенный документ с некоторыми техническими сведениями.

## <a name="binding-objective-c-librariescross-platformmaciosbindingobjective-c-librariesmd"></a>[Привязка библиотек Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)

В этом документе описывается процесс создания C# привязок интерфейсов API цели-c и то, как идиомы в цели-c сопоставляются с идиомами, используемыми в .NET.
При привязке только интерфейсов API C следует использовать стандартный механизм .NET для этого, а также платформу P/Invoke.

## <a name="binding-definition-reference-guidecross-platformmaciosbindingbinding-types-referencemd"></a>[Справочное руководство по определению привязки](~/cross-platform/macios/binding/binding-types-reference.md)

Это справочное руководство, описывающее все атрибуты, доступные авторам для привязки к процессу создания привязки.


## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Цель Шарпие — это средство командной строки, помогающее выполнить начальную загрузку первого прохода привязки. Он работает путем анализа файлов заголовков собственной библиотеки, чтобы сопоставлять открытый API с [определением привязки](~/cross-platform/macios/binding/objective-c-libraries.md) (процесс, который также можно выполнить вручную).

## <a name="ios"></a>iOS

В дополнение к приведенным ниже примерам на [странице привязки iOS](~/ios/platform/binding-objective-c/index.md) возвращаются ссылки на эти общие ресурсы привязки.

### <a name="walkthrough-binding-an-objective-c-libraryiosplatformbinding-objective-cwalkthroughmd"></a>[Пошаговое руководство: Привязка библиотеки цели-C](~/ios/platform/binding-objective-c/walkthrough.md)

В этой статье приводятся пошаговые инструкции по созданию проекта привязки с использованием проекта [инфколорпиккер](https://github.com/InfinitApps/InfColorPicker) цели-C с открытым исходным кодом в качестве примера. Библиотека Инфколорпиккер предоставляет доступный для повторного использования контроллер представлений, позволяющий пользователю выбирать цвет на основе его представления HSB, что делает выбор цветов более удобным для пользователя. Целевое Шарпие будет использоваться для помощи в процессе привязки.

### <a name="binding-sampleshttpsgithubcommonomonotouch-bindings"></a>[Примеры привязки](https://github.com/mono/monotouch-bindings)

Коллекция привязок сторонних разработчиков, которую можно использовать в качестве ссылки при создании новых проектов привязки.

## <a name="mac"></a>Mac

Историческая [Привязка Mac](~/mac/platform/binding.md) выполняется в ручном режиме. Сейчас доступна [Предварительная](https://forums.xamarin.com/discussion/59760/xamarin-mac-binding-project-preview) версия поддержки проекта привязки Mac для будущего выпуска Visual Studio для Mac.

## <a name="related-links"></a>Связанные ссылки

- [Привязка iOS](~/ios/platform/binding-objective-c/index.md)
- [Привязка Mac](~/mac/platform/binding.md)
