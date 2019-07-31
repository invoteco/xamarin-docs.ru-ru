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
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656007"
---
# <a name="webview-zoom-on-android"></a>WebView Zoom в Android

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа Android включает контроль [`WebView`](xref:Xamarin.Forms.WebView)сжатия и масштабирования для. Он используется в XAML путем задания `WebView.EnableZoomControls` свойству и `WebView.DisplayZoomControls` связываемым свойствам `boolean` значений:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView Source="https://www.xamarin.com"
             android:WebView.EnableZoomControls="true"
             android:WebView.DisplayZoomControls="true" />
</ContentPage>
```

Свойство, доступное для `WebView.DisplayZoomControls` `WebView`привязки, определяет, включено ли сжатие сжатия в, а свойство, доступное для привязки, определяет, будут ли элементы управления масштабом наложены `WebView.EnableZoomControls` [`WebView`](xref:Xamarin.Forms.WebView)на.

Кроме того, можно использовать интерфейс, зависящий от C# платформы, с помощью API Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>()
    .EnableZoomControls(true)
    .DisplayZoomControls(true);
```

`WebView.On<Android>` Метод указывает, что этой платформы будет выполняться только в Android. Метод в пространстве имен используется для управления включением сжатия в [`WebView`.](xref:Xamarin.Forms.WebView) `WebView.EnableZoomControls` [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) Метод в том же пространстве имен используется для управления тем `WebView`, наложены ли элементы управления Zoom на. `WebView.DisplayZoomControls` Кроме того, `WebView.ZoomControlsEnabled` можно использовать `WebView.ZoomControlsDisplayed` методы и для возврата к включению элементов управления сжатием в масштабе и масштабом соответственно.

В результате можно включить [`WebView`](xref:Xamarin.Forms.WebView)сжатие сжатия в, и элементы управления масштабом можно заменять `WebView`на:

[ ![Снимок экрана с масштабным WebView в Android](webview-zoom-controls-images/webview-zoom.png "Zoom WebView") ] (webview-zoom-controls-images/webview-zoom-large.png#lightbox "Масштабное WebView")

> [!IMPORTANT]
> Элементы управления масштабом должны быть включены и отображены с помощью соответствующих связываемых свойств или методов, которые будут наложены [`WebView`](xref:Xamarin.Forms.WebView)на.

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
