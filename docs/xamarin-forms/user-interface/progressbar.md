---
title: Xamarin.Forms ProgressBar
description: Xamarin.Forms ProgressBar — это элемент управления, который визуально представляет ход выполнения в виде горизонтальной полосы, заполняемую основанное на свойстве, число с плавающей запятой.
ms.prod: xamarin
ms.assetId: C2F85FED-797C-466B-A0FD-E73CFB79B267
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/09/2019
ms.openlocfilehash: 2e2917077575ebc78dbdda8ae55aa230a39a7ba1
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67837014"
---
# <a name="xamarinforms-progressbar"></a>Xamarin.Forms ProgressBar
[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ProgressBarDemos)

Xamarin.Forms [ `ProgressBar` ](xref:Xamarin.Forms.ProgressBar) является элемент управления, который визуально представляет ход выполнения в виде горизонтальной полосы, заполняемую в процентах, представленный `float` значение. `ProgressBar` Класс наследует от [ `View` ](xref:Xamarin.Forms.View).

На следующем снимке экрана показан `ProgressBar` в iOS и Android:

![Снимок экрана ProgressBar в iOS и Android](progressbar-images/progressbars-default.png "ProgressBar в iOS и Android")

`ProgressBar` Управления определяет два свойства:

* [`Progress`](xref:Xamarin.Forms.ProgressBar.Progress) является `float` значение, представляющее текущий ход выполнения как значение от 0 до 1. `Progress` значения менее 0 может находиться значение 0, значения больше 1 может находиться до 1.
* [`ProgressColor`](xref:Xamarin.Forms.ProgressBar.ProgressColor) является `Color` , влияющее на внутреннюю цвет, представляющий текущее состояние столбца.

Эти свойства поддерживаются [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) объектов, что означает, что `ProgressBar` можно применить различные стили и быть целевым объектом привязки данных.

`ProgressBar` Управления также определяет `ProgressTo` метод, который анимирует панели текущее значение к указанному значению. Дополнительные сведения см. в разделе [анимация ProgressBar](#animate-a-progressbar).

> [!NOTE]
> `ProgressBar` Не принимает операции пользователя, поэтому он пропускается при помощи клавиши Tab для выбора элементов управления.

## <a name="create-a-progressbar"></a>Создание элемента управления ProgressBar

Объект `ProgressBar` может быть создано в XAML. Его `Progress` свойство может быть присвоено определить процент заполнения внутреннее, цветные полосы. Если `Progress` свойство не задано, по умолчанию равно 0. В следующем примере показано, как создать экземпляр `ProgressBar` в XAML с помощью необязательного `Progress` набор свойств:

```xaml
<ProgressBar Progress="0.5" />
```

Объект `ProgressBar` также могут создаваться в коде:

```csharp
ProgressBar progressBar = new ProgressBar { Progress = 0.5f };
```

> [!WARNING]
> Не использовать параметры неограниченного горизонтальном расположении, например `Center`, `Start`, или `End` с `ProgressBar`. На UWP `ProgressBar` сворачивает панель нулевую ширину. Сохраните значение по умолчанию `HorizontalOptions` значение `Fill` и не используйте в ширину `Auto` при помещении `ProgressBar` в `Grid` макета.

## <a name="progressbar-appearance-properties"></a>Свойства внешнего вида элемента управления ProgressBar

`ProgressColor` Для определения панели внутреннее свойство можно задать цвет при `Progress` свойства не равно нулю. В следующем примере показано, как создать экземпляр `ProgressBar` в XAML с `ProgressColor` набор свойств:

```xaml
<ProgressBar OnColor="Orange" />
```

`ProgressColor` Также можно задать свойство, при создании `ProgressBar` в коде:

```csharp
ProgressBar progressBar = new ProgressBar { ProgressColor = Color.Orange };
```

На следующем снимке экрана показан `ProgressBar` с `ProgressColor` свойство значение `Color.Orange` в iOS и Android:

![Снимок экрана оформленного элемента управления ProgressBar в iOS и Android](progressbar-images/progressbars-styled.png "стиль элемента управления ProgressBar в iOS и Android")

## <a name="animate-a-progressbar"></a>Анимация ProgressBar

`ProgressTo` Анимирует метод `ProgressBar` его `Progress` значение до указанного значения со временем. Этот метод принимает `float` хода выполнения значение, `uint` длительность в миллисекундах, `Easing` значение перечисления и возвращает `Task<bool>`. Следующий код демонстрирует, как анимировать `ProgressBar`:

```csharp
// animate to 75% progress over 500 milliseconds with linear easing
await progressBar.ProgressTo(0.75, 500, Easing.Linear);
```

Дополнительные сведения о `Easing` перечисления, см. в разделе [функции плавности в Xamarin.Forms](~/xamarin-forms/user-interface/animation/easing.md).

## <a name="related-links"></a>Связанные ссылки

* [Демонстрации ProgressBar](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ProgressBarDemos)
