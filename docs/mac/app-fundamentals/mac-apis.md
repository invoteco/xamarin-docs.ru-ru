---
title: API-интерфейсы macOS для разработчиков Xamarin. Mac
description: В этом документе описывается чтение селекторов цели-C и способы поиска соответствующих C# методов в приложении Xamarin. Mac.
ms.prod: xamarin
ms.assetid: 9F7451FA-E07E-4C7B-B5CF-27AFC157ECDA
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/02/2017
ms.openlocfilehash: cd427d13bb79fd31e1e814726aaaf61788ae10ec
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306275"
---
# <a name="macos-apis-for-xamarinmac-developers"></a>API-интерфейсы macOS для разработчиков Xamarin. Mac

## <a name="overview"></a>Обзор

В C# значительной мере время разработки с помощью Xamarin. Mac вы можете думать, читать и писать, не заботясь о базовых API-интерфейсах целевой платформы. Однако иногда вам потребуется прочитать документацию по API от Apple, преобразовать ответ из Stack Overflow в решение проблемы или сравнить с существующим примером.

## <a name="reading-enough-objective-c-to-be-dangerous"></a>Чтение достаточной цели-C, чтобы быть опасной

Иногда потребуется прочитать определение цели или вызов метода и перевести его в эквивалентный C# метод. Давайте взглянем на определение функции-C и разбейте его на части. Этот метод ( *селектор* в цели-C) можно найти на `NSTableView`:

```objc
- (BOOL)canDragRowsWithIndexes:(NSIndexSet *)rowIndexes atPoint:(NSPoint)mouseDownPoint
```

Объявление может быть прочитано слева направо:

- Префикс `-` означает, что это метод экземпляра (не статический). + означает, что это метод класса (статический)
- `(BOOL)` является типом возвращаемого значения (bool C#в)
- `canDragRowsWithIndexes` является первой частью имени.
- `(NSIndexSet *)rowIndexes` является первым параметром и имеет тип. Первый параметр имеет формат: `(Type) pararmName`
- `atPoint:(NSPoint)mouseDownPoint` является вторым параметром и его типом. Каждый параметр после первого является форматом: `selectorPart:(Type) pararmName`
- Полное имя этого селектора сообщений: `canDragRowsWithIndexes:atPoint:`. Обратите внимание на `:` в конце — важно.
- Фактическая привязка Xamarin. C# Mac: `bool CanDragRows (NSIndexSet rowIndexes, PointF mouseDownPoint)`

Этот вызов селектора может быть прочитан таким же образом:

```objc
[v canDragRowsWithIndexes:set atPoint:point];
```

- `v` экземпляра имеет свой селектор `canDragRowsWithIndexes:atPoint`, вызываемый с двумя параметрами: `set` и `point`, переданные в.
- В C#метод вызов метода выглядит следующим образом: `x.CanDragRows (set, point);`

<a name="finding_selector" />

## <a name="finding-the-c-member-for-a-given-selector"></a>Поиск C# элемента для данного селектора

Теперь, когда вы нашли селектор цели-C, который необходимо вызвать, следующий шаг выполняет сопоставление с эквивалентным C# элементом. Существует четыре подхода, которые можно попробовать (продолжить с `NSTableView CanDragRows` примере):

1. Используйте список автозавершения, чтобы быстро проверить наличие что-то с тем же именем. Так как известно, что это экземпляр `NSTableView` можно ввести:

    - `NSTableView x;`
    - `x.` [Ctrl + пробел, если список не отображается).
    - `CanDrag` [ВВОД]
    - Щелкните метод правой кнопкой мыши, перейдите к объявлению, чтобы открыть браузер сборок, где можно сравнить `Export` атрибут с выбранным селектором.

2. Поиск по всей привязке класса. Так как известно, что это экземпляр `NSTableView` можно ввести:

    - `NSTableView x;`
    - Щелкните правой кнопкой мыши `NSTableView`, перейдите к объявлению в браузере сборки.
    - Поиск рассматриваемого селектора

3. Вы можете использовать [интерактивную документацию по API Xamarin. Mac](https://docs.microsoft.com/dotnet/api/?view=xamarinmac-3.0) .

4. Мигель предоставляет представление "Розеттском" для API Xamarin. Mac [здесь](https://tirania.org/tmp/rosetta.html) , где можно выполнять поиск по заданному API. Если ваш API не является AppKit или macOS, его можно найти там.

<!--
Note: In some cases, the assembly browser can hit a bug where it will open but not jump to the right definition. Keep that tab open, switch back to your source code and try again.
Note: The assembly browser tricks currently only works with Xamarin.Mac Classic. This will be fixed in a future version.
-->
