---
title: Индикатор активности в Xamarin.Forms
description: Управления ActivityIndicator указывает пользователей, что приложение участвует в длительной операции, без предоставления какого-либо указания хода выполнения. В этой статье описываются способы использования ActivityIndicator в XAML и коде.
ms.prod: xamarin
ms.assetid: 4CEED02D-5CA3-4C3A-B7ED-3193FC272261
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/10/2019
ms.openlocfilehash: abd8150e3aa4ec347c8d956004993340630208bf
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67837064"
---
# <a name="xamarinforms-activityindicator"></a>Xamarin.Forms ActivityIndicator
[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ActivityIndicatorDemos)

Xamarin.Forms[ `ActivityIndicator` ](xref:Xamarin.Forms.ActivityIndicator) является элемент управления, отображающий анимации, чтобы показать, что оно участвует в длительных действий. В отличие от [ `ProgressBar` ](xref:Xamarin.Forms.ProgressBar), `ActivityIndicator` не содержит объяснения хода выполнения. `ActivityIndicator` Наследует от [ `View` ](xref:Xamarin.Forms.View).

На следующем снимке экрана показан `ActivityIndicator` элемента управления в iOS и Android:

![Снимок экрана ActivityIndicator в iOS и Android](activityindicator-images/activityindicators-default.png "экрана ActivityIndicator в iOS и Android")

`ActivityIndicator` Управления определяет следующие свойства:

* [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning) является `bool` значение, указывающее, является ли `ActivityIndicator` должен быть видимым и анимации или скрытым. Если значение равно `false` `ActivityIndicator` не будут отображаться.
* [`Color`](xref:Xamarin.Forms.ActivityIndicator.Color) — `Color` значение, определяющее цвет отображения `ActivityIndicator`.

Эти свойства поддерживаются [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) объектов, что означает, что `ActivityIndicator` можно применить различные стили и быть целевым объектом привязки данных.

## <a name="create-an-activityindicator"></a>Создание ActivityIndicator

`ActivityIndicator` Может быть создано в XAML. Его `IsRunning` может быть установлено для определения, является ли элемент управления видимым и анимации. Если `IsRunning` свойства не задано, по умолчанию используется `false` и `ActivityIndicator` не будут отображаться. В следующем примере показано, как создать экземпляр `ActivityIndicator` в XAML с помощью необязательного `IsRunning` набор свойств:

```xaml
<ActivityIndicator IsRunning="true" />
```

`ActivityIndicator` Также могут создаваться в коде:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { IsRunning = true };
```

## <a name="activityindicator-appearance-properties"></a>Свойства внешнего вида ActivityIndicator

`Color` Для определения можно задать свойство `ActivityIndicator` цвет. В следующем примере показано, как создать экземпляр `ActivityIndicator` в XAML с `Color` набор свойств:

```xaml
<ActivityIndicator Color="Orange" />
```

`Color` Также можно задать свойство, при создании `ActivityIndicator` в коде:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { Color = Color.Orange };
```

На следующем снимке экрана показан `ActivityIndicator` с `Color` свойство значение `Color.Orange` в iOS и Android:

![Снимок экрана со стилем ActivityIndicator в iOS и Android](activityindicator-images/activityindicators-styled.png "снимок экрана со стилем ActivityIndicator в iOS и Android")

## <a name="related-links"></a>Связанные ссылки

* [Демонстрации ActivityIndicator](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ActivityIndicatorDemos)
* [ProgressBar](~/xamarin-forms/user-interface/progressbar.md)
