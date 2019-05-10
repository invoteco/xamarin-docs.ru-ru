---
title: Привязка Objective-C
description: Этот документ содержит ссылки на различные руководства, описывающие создание C# привязки для кода Objective-C, что позволяет разработчикам использовать готовые библиотеки в приложениях Xamarin.
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: bdf284d66539da3eca35e79d761712012674de0d
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/01/2019
ms.locfileid: "64978458"
---
# <a name="binding-objective-c"></a>Привязка Objective-C

Этот раздел содержит широкий набор документов, которые покрывают Создание привязок для библиотеки Objective-C, поэтому они могут вызываться из C# приложения, созданные с помощью Xamarin.iOS и Xamarin.Mac.

##  <a name="overviewcross-platformmaciosbindingoverviewmd"></a>[Обзор](~/cross-platform/macios/binding/overview.md)

Этот документ содержит некоторые из внутренних как выполняется привязка. Он представляет собой расширенный документ некоторые технические сведения.

##  <a name="binding-objective-c-librariescross-platformmaciosbindingobjective-c-librariesmd"></a>[Привязка библиотек Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)

В этом документе описан процесс, используемый для создания C# привязки Objective-C API и как стили в Objective-C сопоставляются идиомы, используемые в .NET.
При связывании просто C API-интерфейсы, следует использовать стандартный механизм .NET для этого платформа P/Invoke.

##  <a name="binding-definition-reference-guidecross-platformmaciosbindingbinding-types-referencemd"></a>[Справочное руководство для определения привязки](~/cross-platform/macios/binding/binding-types-reference.md)

Это руководство, описывающее все атрибуты, доступные для авторов привязки для управления процессом создания привязки.


## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Цели Sharpie — это средство командной строки для начальной загрузки при первом проходе привязки. Он работает путем анализа файлов заголовков из собственной библиотеки для сопоставления открытого API-интерфейса в [определения привязки](~/cross-platform/macios/binding/objective-c-libraries.md) (этот процесс, можно также вручную).

## <a name="ios"></a>iOS

[Страницу привязки iOS](~/ios/platform/binding-objective-c/index.md) ссылается на общие ресурсы привязки, в дополнение к в приведенных ниже примерах.

### <a name="walkthrough-binding-an-objective-c-libraryiosplatformbinding-objective-cwalkthroughmd"></a>[Пошаговое руководство: Привязка библиотек Objective-C](~/ios/platform/binding-objective-c/walkthrough.md)

Статья содержит пошаговое руководство по созданию проекта привязки с помощью с открытым исходным кодом [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) проекта Objective-C в качестве примера. Библиотека InfColorPicker предоставляет контроллер многократно используемых представления, которые позволяют пользователю выбирать цвета в его представление HSB, выбора цвета более понятные. Цели Sharpie будет использоваться для помощи в процессе привязки.

### <a name="binding-sampleshttpsgithubcommonomonotouch-bindings"></a>[Примеры привязок](https://github.com/mono/monotouch-bindings)

Коллекция привязок независимых производителей, которые могут использоваться в качестве ссылки при создании новых проектов привязки.

## <a name="mac"></a>Mac

Исторически [привязки Mac](~/mac/platform/binding.md) была очень вручную. Сейчас [загружаемые preview](https://forums.xamarin.com/discussion/59760/xamarin-mac-binding-project-preview) поддержки проект привязки Mac в будущих выпусках Visual Studio для Mac.

## <a name="related-links"></a>Связанные ссылки

- [iOS привязки](~/ios/platform/binding-objective-c/index.md)
- [Привязка Mac](~/mac/platform/binding.md)
