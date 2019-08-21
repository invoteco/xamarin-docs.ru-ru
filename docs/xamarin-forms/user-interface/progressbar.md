---
title: Компонент ProgressBar для Xamarin. Forms
description: Элемент управления Xamarin. Forms ProgressBar представляет собой визуальное представление хода выполнения в виде горизонтальной панели, заполняемой на основе свойства float.
ms.prod: xamarin
ms.assetId: C2F85FED-797C-466B-A0FD-E73CFB79B267
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/09/2019
ms.openlocfilehash: a4cfc6c54eb2864707f328106761af029e0734cf
ms.sourcegitcommit: 9178e2e689f027212ea3e623b556b312985d79fe
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/21/2019
ms.locfileid: "69658083"
---
# <a name="xamarinforms-progressbar"></a>Компонент ProgressBar для Xamarin. Forms
[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/)

Элемент управления Xamarin. [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) Forms визуально представляет ход выполнения в виде горизонтальной панели, заполненной в процентах, представленном `float` значением. Класс наследует от [`View`.](xref:Xamarin.Forms.View) `ProgressBar`

`ProgressBar` На следующих снимках экрана показано на iOS и Android:

![Снимок экрана элемента ProgressBar в iOS и Android](progressbar-images/progressbars-default.png "ProgressBar в iOS и Android")

`ProgressBar` Элемент управления определяет два свойства:

* [`Progress`](xref:Xamarin.Forms.ProgressBar.Progress)`float` значение, представляющее текущий ход выполнения в виде значения от 0 до 1. `Progress`значения меньше 0 будут относиться к 0, а значения больше 1 будут относиться к 1.
* [`ProgressColor`](xref:Xamarin.Forms.ProgressBar.ProgressColor)значение типа `Color` , которое влияет на цвет внутренней полосы, представляющей текущий ход выполнения.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами. Это означает `ProgressBar` , что можно использовать стиль и цель привязок данных.

Элемент управления также `ProgressTo` определяет метод, который анимирует отрезок от текущего значения до указанного значения. `ProgressBar` Дополнительные сведения см. [в разделе Анимация элемента ProgressBar](#animate-a-progressbar).

> [!NOTE]
> Объект `ProgressBar` не принимает манипуляции с пользователем, поэтому он пропускается при использовании клавиши TAB для выбора элементов управления.

## <a name="create-a-progressbar"></a>Создание элемента ProgressBar

Экземпляр `ProgressBar` можно создать в XAML. Его `Progress` свойство определяет процент заполнения внутренней цветной полосы. Значение свойства `Progress` по умолчанию — 0. В следующем примере показано, как создать экземпляр `ProgressBar` в XAML с помощью необязательного `Progress` набора свойств:

```xaml
<ProgressBar Progress="0.5" />
```

Также `ProgressBar` можно создать в коде:

```csharp
ProgressBar progressBar = new ProgressBar { Progress = 0.5f };
```

> [!WARNING]
> Не используйте неограниченные горизонтальные параметры макета, такие `Center`как `Start`, или `End` с `ProgressBar`. В UWP объект `ProgressBar` сворачивается в полоску нулевой ширины. `HorizontalOptions` Неиспользуйте`Fill` значение по умолчанию и `ProgressBar` `Grid` при размещении в макете не следует использовать ширину. `Auto`

## <a name="progressbar-appearance-properties"></a>Свойства внешнего вида ProgressBar

Свойство определяет цвет внутреннего столбца, `Progress` если свойство больше нуля. `ProgressColor` В следующем примере показано, как создать экземпляр `ProgressBar` в XAML `ProgressColor` с помощью набора свойств:

```xaml
<ProgressBar OnColor="Orange" />
```

Это свойство также может быть задано при `ProgressBar` создании в коде: `ProgressColor`

```csharp
ProgressBar progressBar = new ProgressBar { ProgressColor = Color.Orange };
```

На следующих снимках экрана `ProgressBar` показано, `ProgressColor` для `Color.Orange` чего свойство имеет значение в iOS и Android:

![Снимок экрана с стилем ProgressBar в iOS и Android](progressbar-images/progressbars-styled.png "Стили ProgressBar в iOS и Android")

## <a name="animate-a-progressbar"></a>Анимация элемента ProgressBar

Метод выполняет анимацию `ProgressBar` от текущего `Progress` значения до указанного значения с течением времени. `ProgressTo` Метод принимает `float` значение хода выполнения `uint` , продолжительность `Easing` в миллисекундах, значение перечисления и возвращает `Task<bool>`. В следующем коде показано, как анимировать `ProgressBar`:

```csharp
// animate to 75% progress over 500 milliseconds with linear easing
await progressBar.ProgressTo(0.75, 500, Easing.Linear);
```

Дополнительные сведения о `Easing` перечислении см. [в разделе ускорение функций в Xamarin. Forms](~/xamarin-forms/user-interface/animation/easing.md).

## <a name="related-links"></a>Связанные ссылки

* [Демонстрации ProgressBar](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/)
