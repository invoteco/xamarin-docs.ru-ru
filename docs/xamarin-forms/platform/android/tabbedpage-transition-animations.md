---
title: Анимация перехода на страницу Таббедпаже в Android
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу Android, которая отключает анимацию переходов при переходе по страницам в Таббедпаже.
ms.prod: xamarin
ms.assetid: 2DB4EA6D-9CED-4137-BAB2-B20A457B1CA3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 4f8d6ec2b06855364970bc9b672c3d3f7b9bfdfc
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68649841"
---
# <a name="tabbedpage-page-transition-animations-on-android"></a>Анимация перехода на страницу Таббедпаже в Android

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа для Android используется для отключения анимации переходов при переходе по страницам либо программно, либо при использовании панели вкладок в [`TabbedPage`](xref:Xamarin.Forms.TabbedPage). Он используется в XAML, задав `TabbedPage.IsSmoothScrollEnabled` свойство, используемое для `false`:

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

- [PlatformSpecifics (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
