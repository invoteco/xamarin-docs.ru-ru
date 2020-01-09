---
title: Режим перехода Свипевиевного считывания в Android
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать зависящую от платформы Android платформу, управляющую переходом, который используется при открытии Свипевиев.
ms.prod: xamarin
ms.assetid: 6B1F8213-9D62-4C40-9F04-881F1371B5AA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2019
ms.openlocfilehash: 077d4a8a9530bf074fde710dd08c1fbea49668ef
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75490478"
---
# <a name="swipeview-swipe-transition-mode-on-android"></a>Режим перехода Свипевиевного считывания в Android

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Этот элемент управления для конкретной платформы Android управляет переходом, который используется при открытии `SwipeView`. Он используется в XAML путем установки для свойства `SwipeView.SwipeTransitionMode` BIND значения `SwipeTransitionMode` перечисления:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core" >
    <StackLayout>
        <SwipeView android:SwipeView.SwipeTransitionMode="Drag">
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
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

SwipeView swipeView = new Xamarin.Forms.SwipeView();
swipeView.On<Android>().SetSwipeTransitionMode(SwipeTransitionMode.Drag);
// ...
```

`SwipeView.On<Android>` Метод указывает, что этой платформы будет выполняться только в Android. Метод `SwipeView.SetSwipeTransitionMode` в пространстве имен [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) используется для управления переходом, используемым при открытии `SwipeView`. Перечисление `SwipeTransitionMode` предоставляет два возможных значения:

- `Reveal` указывает, что прокрутка элементов будет выдаваться по мере прокрутки `SwipeView` содержимого, и является значением по умолчанию свойства `SwipeView.SwipeTransitionMode`.
- `Drag` указывает, что прокрутка элементов будет перемещена в представление по мере прокрутки содержимого `SwipeView`.

Кроме того, можно использовать метод `SwipeView.GetSwipeTransitionMode` для возврата `SwipeTransitionMode`, применяемого к `SwipeView`.

В результате к `SwipeView`у будет применено указанное `SwipeTransitionMode` значение, которое управляет переходом, используемым при открытии `SwipeView`:

[![Снимок экрана Свипевиев Свипетранситионмодес на Android](swipeview-swipetransitionmode-images/swipetransitionmode.png "Свипетранситионмодес в Android")](swipeview-swipetransitionmode-images/swipetransitionmode-large.png#lightbox "Свипетранситионмодес в Android")

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
