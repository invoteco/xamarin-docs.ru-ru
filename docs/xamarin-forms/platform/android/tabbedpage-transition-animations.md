---
title: Страница TabbedPage анимация перехода на Android
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать Android конкретных платформ, отключает анимация перехода при навигации по страницам в TabbedPage.
ms.prod: xamarin
ms.assetid: 2DB4EA6D-9CED-4137-BAB2-B20A457B1CA3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: b57718038c43f7bc2a9e27e780a0624eb50a54fe
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/20/2019
ms.locfileid: "65926420"
---
# <a name="tabbedpage-page-transition-animations-on-android"></a>Страница TabbedPage анимация перехода на Android

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

Это Android платформы используется для отключения анимации перехода, при переходе по страницам, либо программно или при использовании панели вкладки в [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Он используется в XAML, задав `TabbedPage.IsSmoothScrollEnabled` свойство, используемое для `false`:

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.IsSmoothScrollEnabled="false">
    ...
</TabbedPage>
```

Кроме того его можно будет использовать с помощью C# с помощью текучего API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetIsSmoothScrollEnabled(false);
```

`TabbedPage.On<Android>` Метод указывает, что этой платформы будет выполняться только в Android. `TabbedPage.SetIsSmoothScrollEnabled` Метод в [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) пространства имен, используется для управления ли анимация перехода будет отображаться при переходе между страниц в [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Кроме того `TabbedPage` в класс `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` пространство имен также содержит следующие методы:

- `IsSmoothScrollEnabled`, который используется для получения ли анимация перехода будет отображаться при переходе между страниц в `TabbedPage`.
- `EnableSmoothScroll`, который используется для включения анимации перехода, при переходе между страницами в `TabbedPage`.
- `DisableSmoothScroll`, который используется для отключения анимации перехода, при переходе между страницами в `TabbedPage`.

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
