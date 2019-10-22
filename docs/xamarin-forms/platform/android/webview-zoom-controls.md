---
title: WebView Zoom в Android
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу Android, которая позволяет увеличить масштаб WebView.
ms.prod: xamarin
ms.assetid: DC1A3762-6A42-4298-929C-445F416C3E60
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
ms.openlocfilehash: 2142882add91d613263d11fa4c1e6d7ad142c7c7
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "68656007"
---
# <a name="webview-zoom-on-android"></a>WebView Zoom в Android

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа Android включает контроль сжатия и масштабирования для [`WebView`](xref:Xamarin.Forms.WebView). Он используется в XAML путем задания `WebView.EnableZoomControls` и `WebView.DisplayZoomControls` связываемых свойств для `boolean` значений:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView Source="https://www.xamarin.com"
             android:WebView.EnableZoomControls="true"
             android:WebView.DisplayZoomControls="true" />
</ContentPage>
```

@No__t_0 свойство, доступное для привязки, определяет, включено ли сжатие сжатием на [`WebView`](xref:Xamarin.Forms.WebView), а свойство `WebView.DisplayZoomControls` привязки определяет, будут ли в `WebView` наложены элементы управления масштабом.

Кроме того, можно использовать интерфейс, зависящий от C# платформы, с помощью API Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>()
    .EnableZoomControls(true)
    .DisplayZoomControls(true);
```

Метод `WebView.On<Android>` указывает, что эта платформа будет запускаться только в Android. Метод `WebView.EnableZoomControls` в пространстве имен [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) используется для управления включением сжатия в [`WebView`](xref:Xamarin.Forms.WebView). Метод `WebView.DisplayZoomControls` в том же пространстве имен используется для управления тем, наложены ли на `WebView` элементы управления масштабом. Кроме того, можно использовать методы `WebView.ZoomControlsEnabled` и `WebView.ZoomControlsDisplayed`, чтобы вернуться к включению элементов управления сжатием в масштабе и масштабом соответственно.

В результате можно включить сжатие сжатия в [`WebView`](xref:Xamarin.Forms.WebView), и элементы управления масштабом можно будет заменять на `WebView`:

[![Снимок экрана с масштабным WebView в Android](webview-zoom-controls-images/webview-zoom.png "Масштабное WebView")](webview-zoom-controls-images/webview-zoom-large.png#lightbox "Масштабное WebView")

> [!IMPORTANT]
> Элементы управления масштабом должны быть включены и отображены с помощью соответствующих связываемых свойств или методов, которые будут наложены на [`WebView`](xref:Xamarin.Forms.WebView).

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
