---
title: Компонент ProgressBar для Xamarin. Forms
description: Элемент управления Xamarin. Forms ProgressBar представляет собой визуальное представление хода выполнения в виде горизонтальной панели, заполняемой на основе свойства float.
ms.prod: xamarin
ms.assetId: C2F85FED-797C-466B-A0FD-E73CFB79B267
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/09/2019
ms.openlocfilehash: 40f3c9a5af29d1782249775b9f3166698eb6221a
ms.sourcegitcommit: 7acff5c5a03a0351962c05beebfc347503d83fe6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/31/2019
ms.locfileid: "73201943"
---
# <a name="xamarinforms-progressbar"></a>Компонент ProgressBar для Xamarin. Forms
[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/)

Элемент управления Xamarin. Forms [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) визуально представляет ход выполнения в виде горизонтальной панели, заполненной в процентах, представленном значением `float`. Класс `ProgressBar` наследует от [`View`](xref:Xamarin.Forms.View).

На следующих снимках экрана показано `ProgressBar` на iOS и Android:

![Снимок экрана элемента ProgressBar в iOS и Android](progressbar-images/progressbars-default.png "ProgressBar в iOS и Android")

Элемент управления `ProgressBar` определяет два свойства:

* [`Progress`](xref:Xamarin.Forms.ProgressBar.Progress) — это `float` значение, представляющее текущий ход выполнения в виде значения от 0 до 1. `Progress` значения меньше 0 будут относиться к 0, а значения больше 1 будут относиться к 1.
* [`ProgressColor`](xref:Xamarin.Forms.ProgressBar.ProgressColor) — это `Color`, влияющие на цвет внутренней полосы, представляющей текущий ход выполнения.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами. Это означает, что `ProgressBar` может быть применен к стилю и быть целевым объектом привязок данных.

Элемент управления `ProgressBar` также определяет метод `ProgressTo`, который выполняет анимацию линейки от текущего значения до указанного значения. Дополнительные сведения см. [в разделе Анимация элемента ProgressBar](#animate-a-progressbar).

> [!NOTE]
> `ProgressBar` не принимает манипуляции с пользователем, поэтому она пропускается при использовании клавиши TAB для выбора элементов управления.

## <a name="create-a-progressbar"></a>Создание элемента ProgressBar

В XAML можно создать экземпляр `ProgressBar`. Его свойство `Progress` определяет процент заполнения внутренней, цветной полосы. Значение свойства `Progress` по умолчанию — 0. В следующем примере показано, как создать экземпляр `ProgressBar` в XAML с помощью необязательного набора свойств `Progress`:

```xaml
<ProgressBar Progress="0.5" />
```

`ProgressBar` также можно создать в коде:

```csharp
ProgressBar progressBar = new ProgressBar { Progress = 0.5f };
```

> [!WARNING]
> Не используйте неограниченные горизонтальные параметры макета, такие как `Center`, `Start`или `End` с `ProgressBar`. В UWP `ProgressBar` сворачивается в полоску нулевой ширины. `HorizontalOptions` значение по умолчанию `Fill` и не следует использовать ширину `Auto` при помещении `ProgressBar` в макет `Grid`.

## <a name="progressbar-appearance-properties"></a>Свойства внешнего вида ProgressBar

Свойство `ProgressColor` определяет цвет внутреннего столбца, если свойство `Progress` больше нуля. В следующем примере показано, как создать экземпляр `ProgressBar` в XAML с набором свойств `ProgressColor`:

```xaml
<ProgressBar ProgressColor="Orange" />
```

Свойство `ProgressColor` также может быть задано при создании `ProgressBar` в коде:

```csharp
ProgressBar progressBar = new ProgressBar { ProgressColor = Color.Orange };
```

На следующих снимках экрана показано `ProgressBar` со свойством `ProgressColor`, для которого задано значение `Color.Orange` в iOS и Android:

![Снимок экрана с стилем ProgressBar в iOS и Android](progressbar-images/progressbars-styled.png "Стили ProgressBar в iOS и Android")

## <a name="animate-a-progressbar"></a>Анимация элемента ProgressBar

Метод `ProgressTo` анимирует `ProgressBar` от текущего значения `Progress` к предоставленному значению с течением времени. Метод принимает значение хода выполнения `float`, продолжительность `uint` в миллисекундах, `Easing`ое значение перечисления и возвращает `Task<bool>`. В следующем коде показано, как анимировать `ProgressBar`:

```csharp
// animate to 75% progress over 500 milliseconds with linear easing
await progressBar.ProgressTo(0.75, 500, Easing.Linear);
```

Дополнительные сведения о перечислении `Easing` см. [в разделе функции плавности в Xamarin. Forms](~/xamarin-forms/user-interface/animation/easing.md).

## <a name="related-links"></a>Связанные ссылки

* [Демонстрации ProgressBar](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/)
