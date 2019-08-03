---
title: Индикатор действия в Xamarin. Forms
description: Элемент управления Активитиндикатор указывает пользователям, что приложение вовлечено в длительное действие без указания хода выполнения. В этой статье объясняется, как использовать Активитиндикатор в XAML и коде.
ms.prod: xamarin
ms.assetid: 4CEED02D-5CA3-4C3A-B7ED-3193FC272261
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/10/2019
ms.openlocfilehash: e13a46e1022f4e33ace6f9f19bb5cea5d1ac784b
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739165"
---
# <a name="xamarinforms-activityindicator"></a>Активитиндикатор Xamarin. Forms
[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/)

Xamarin. Forms[`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) — это элемент управления, отображающий анимацию, которая показывает, что приложение вовлечено в длительное действие. В отличие [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)от, параметр `ActivityIndicator` не дает указания о ходе выполнения. Объект `ActivityIndicator` наследует от [`View`](xref:Xamarin.Forms.View).

На следующем снимке экрана `ActivityIndicator` показан элемент управления в iOS и Android:

![Снимок экрана активитиндикатор в iOS и Android](activityindicator-images/activityindicators-default.png "Снимок экрана активитиндикатор в iOS и Android")

`ActivityIndicator` Элемент управления определяет следующие свойства:

* [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning)значение, указывающее, должен ли объект `ActivityIndicator` быть видимым, анимировать или скрываться. `bool` Если значение параметра является `false` `ActivityIndicator` невидимым.
* [`Color`](xref:Xamarin.Forms.ActivityIndicator.Color)значение, определяющее цвет `ActivityIndicator`дисплея. `Color`

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами. Это означает `ActivityIndicator` , что можно использовать стиль и цель привязок данных.

## <a name="create-an-activityindicator"></a>Создание Активитиндикатор

Экземпляр `ActivityIndicator` можно создать в XAML. Его `IsRunning` свойство можно задать, чтобы определить видимость элемента управления и его анимацию. Если свойство не задано, по умолчанию используется `false` значение, `ActivityIndicator` а элемент не будет видимым. `IsRunning` В следующем примере показано, как создать экземпляр `ActivityIndicator` в XAML с необязательным `IsRunning` набором свойств:

```xaml
<ActivityIndicator IsRunning="true" />
```

Также `ActivityIndicator` можно создать в коде:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { IsRunning = true };
```

## <a name="activityindicator-appearance-properties"></a>Свойства внешнего вида Активитиндикатор

Для определения цвета можно задать `Color`свойство `ActivityIndicator` . В следующем примере показано, как создать экземпляр `ActivityIndicator` в XAML `Color` с набором свойств:

```xaml
<ActivityIndicator Color="Orange" />
```

Это `Color` свойство также может быть задано при `ActivityIndicator` создании в коде:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { Color = Color.Orange };
```

На следующем снимке экрана `ActivityIndicator` показано, `Color` со свойством `Color.Orange` , установленным в iOS и Android:

![Снимок экрана с стилем активитиндикатор в iOS и Android](activityindicator-images/activityindicators-styled.png "Снимок экрана с стилем активитиндикатор в iOS и Android")

## <a name="related-links"></a>Связанные ссылки

* [Демонстрации Активитиндикатор](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/)
* [ProgressBar](~/xamarin-forms/user-interface/progressbar.md)
