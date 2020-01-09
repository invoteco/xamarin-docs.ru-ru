---
title: Режим перехода Свипевиевного считывания в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS, которая управляет переходом, используемым при открытии Свипевиев.
ms.prod: xamarin
ms.assetid: C667F24C-BAD8-47E0-9285-D3546BEF703B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2019
ms.openlocfilehash: d5ba92d008cf3431bce2c197aca45c894eb3d5c7
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75490454"
---
# <a name="swipeview-swipe-transition-mode-on-ios"></a>Режим перехода Свипевиевного считывания в iOS

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Этот элемент управления для конкретной платформы iOS управляет переходом, который используется при открытии `SwipeView`. Он используется в XAML путем установки для свойства `SwipeView.SwipeTransitionMode` BIND значения `SwipeTransitionMode` перечисления:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SwipeView ios:SwipeView.SwipeTransitionMode="Drag">
            <SwipeView.LeftItems>
                <SwipeItems>
                    <SwipeItem Text="Delete"
                               IconImageSource="delete.png"
                               BackgroundColor="LightPink"
                               Invoked="OnDeleteSwipeItemInvoked" />
                </SwipeItems>
            </SwipeView.LeftItems>
            <!-- Content -->
        </SwipeView>
    </StackLayout>
</ContentPage>
```

Кроме того его можно будет использовать с помощью C# с помощью текучего API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

SwipeView swipeView = new Xamarin.Forms.SwipeView();
swipeView.On<iOS>().SetSwipeTransitionMode(SwipeTransitionMode.Drag);
// ...
```

`SwipeView.On<iOS>` Метод указывает, что этой платформы будет выполняться только на устройствах iOS. Метод `SwipeView.SetSwipeTransitionMode` в пространстве имен [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) используется для управления переходом, используемым при открытии `SwipeView`. Перечисление `SwipeTransitionMode` предоставляет два возможных значения:

- `Reveal` указывает, что прокрутка элементов будет выдаваться по мере прокрутки `SwipeView` содержимого, и является значением по умолчанию свойства `SwipeView.SwipeTransitionMode`.
- `Drag` указывает, что прокрутка элементов будет перемещена в представление по мере прокрутки содержимого `SwipeView`.

Кроме того, можно использовать метод `SwipeView.GetSwipeTransitionMode` для возврата `SwipeTransitionMode`, применяемого к `SwipeView`.

В результате к `SwipeView`у будет применено указанное `SwipeTransitionMode` значение, которое управляет переходом, используемым при открытии `SwipeView`:

[![Снимок экрана Свипевиев Свипетранситионмодес в iOS](swipeview-swipetransitionmode-images/swipetransitionmode.png "Свипетранситионмодес в iOS")](swipeview-swipetransitionmode-images/swipetransitionmode-large.png#lightbox "Свипетранситионмодес в iOS")

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
