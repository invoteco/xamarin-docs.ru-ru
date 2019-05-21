---
title: Масштаб веб-представление Android
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать Android конкретных платформ, позволяющий масштаб на веб-представление.
ms.prod: xamarin
ms.assetid: DC1A3762-6A42-4298-929C-445F416C3E60
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
ms.openlocfilehash: 0e180ca5019bd4e5d1351cfb2f7a8c28ade2afe2
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/21/2019
ms.locfileid: "65971646"
---
# <a name="webview-zoom-on-android"></a>Масштаб веб-представление Android

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Это Android платформы включает масштаба жестом сжатия и элемент управления масштабирования на [ `WebView` ](xref:Xamarin.Forms.WebView). Он используется в XAML, задав `WebView.EnableZoomControls` и `WebView.DisplayZoomControls` привязываемые свойства для `boolean` значения:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView Source="https://www.xamarin.com"
             android:WebView.EnableZoomControls="true"
             android:WebView.DisplayZoomControls="true" />
</ContentPage>
```

`WebView.EnableZoomControls` Свойство, используемое определяет, включено ли сжатие для увеличения [ `WebView` ](xref:Xamarin.Forms.WebView)и `WebView.DisplayZoomControls` свойство, используемое управляет ли элементы управления масштабом наложенные на `WebView`.

Кроме того, можно использовать с платформой из C# с помощью текучего API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>()
    .EnableZoomControls(true)
    .DisplayZoomControls(true);
```

`WebView.On<Android>` Метод указывает, что этой платформы будет выполняться только в Android. `WebView.EnableZoomControls` Метод в [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) пространства имен, используется для управления отвечает за включение масштаба жестом сжатия на [ `WebView` ](xref:Xamarin.Forms.WebView). `WebView.DisplayZoomControls` Метода, в то же пространство имен используется для управления появляется ли элементы управления масштабом в `WebView`. Кроме того `WebView.ZoomControlsEnabled` и `WebView.ZoomControlsDisplayed` методы можно использовать для возврата, элементы управления масштаба жестом сжатия и масштабирования, включены ли, соответственно.

Результатом является этот жест сжатия для масштабирования можно включить на [ `WebView` ](xref:Xamarin.Forms.WebView), и элементы управления масштабом можно накладывается на `WebView`:

[![Снимок экрана увеличенной WebView в Android](webview-zoom-controls-images/webview-zoom.png "измененный в масштабе WebView")](webview-zoom-controls-images/webview-zoom-large.png#lightbox "измененный в масштабе веб-представления")

> [!IMPORTANT]
> Элементы управления масштабом необходимо включена и отображается с помощью соответствующих привязываемые свойства или методы для накладывается на [ `WebView` ](xref:Xamarin.Forms.WebView).

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
