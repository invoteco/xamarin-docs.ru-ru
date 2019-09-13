---
title: Индикатор действия в Xamarin. Forms
description: Элемент управления Активитиндикатор указывает пользователям, что приложение вовлечено в длительное действие без указания хода выполнения. В этой статье объясняется, как использовать Активитиндикатор в XAML и коде.
ms.prod: xamarin
ms.assetid: 4CEED02D-5CA3-4C3A-B7ED-3193FC272261
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/10/2019
ms.openlocfilehash: 0694439f5e363399e0442c9883426c0f0bf5d989
ms.sourcegitcommit: ab51d32f4ea0e0d4701f0bf2f1465c9323cd070b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/11/2019
ms.locfileid: "70887438"
---
# <a name="xamarinforms-activityindicator"></a>Активитиндикатор Xamarin. Forms
[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/)

Элемент управления Xamarin. [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) Forms отображает анимацию, чтобы показать, что приложение вовлечено в длительное действие. В отличие [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)от, параметр `ActivityIndicator` не дает указания о ходе выполнения. Объект `ActivityIndicator` наследует от [`View`](xref:Xamarin.Forms.View).

На следующих снимках экрана `ActivityIndicator` показан элемент управления iOS и Android:

![Снимок экрана активитиндикатор в iOS и Android](activityindicator-images/activityindicators-default.png "Снимок экрана активитиндикатор в iOS и Android")

`ActivityIndicator` Элемент управления определяет следующие свойства:

* [`Color`](xref:Xamarin.Forms.ActivityIndicator.Color)значение, определяющее цвет `ActivityIndicator`дисплея. `Color`
* [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning)значение, указывающее, должен ли объект `ActivityIndicator` быть видимым, анимировать или скрываться. `bool` Если значение параметра является `false` `ActivityIndicator` невидимым.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами. Это означает `ActivityIndicator` , что можно использовать стиль и цель привязок данных.

## <a name="create-an-activityindicator"></a>Создание Активитиндикатор

Экземпляр `ActivityIndicator` класса можно создать на языке XAML. Его `IsRunning` свойство определяет, является ли элемент управления видимым и анимацией. По умолчанию `false`свойствоимеетзначение `IsRunning` . В следующем примере показано, как создать экземпляр `ActivityIndicator` в XAML с необязательным `IsRunning` набором свойств:

```xaml
<ActivityIndicator IsRunning="true" />
```

Также `ActivityIndicator` можно создать в коде:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { IsRunning = true };
```

## <a name="activityindicator-appearance-properties"></a>Свойства внешнего вида Активитиндикатор

`Color` Свойство`ActivityIndicator` определяет цвет. В следующем примере показано, как создать экземпляр `ActivityIndicator` в XAML `Color` с набором свойств:

```xaml
<ActivityIndicator Color="Orange" />
```

Это `Color` свойство также может быть задано при `ActivityIndicator` создании в коде:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { Color = Color.Orange };
```

На следующих снимках экрана `ActivityIndicator` показано, `Color` для `Color.Orange` чего свойство имеет значение в iOS и Android:

![Снимок экрана с стилем активитиндикатор в iOS и Android](activityindicator-images/activityindicators-styled.png "Снимок экрана с стилем активитиндикатор в iOS и Android")

## <a name="related-links"></a>Связанные ссылки

* [Демонстрации Активитиндикатор](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/)
* [ProgressBar](~/xamarin-forms/user-interface/progressbar.md)
