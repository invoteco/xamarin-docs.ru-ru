---
title: WebView смешанное содержимое в Android
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать зависящую от платформы Android платформу, которая отображает смешанное содержимое в WebView в приложениях, предназначенных для API 21 или более поздней версии.
ms.prod: xamarin
ms.assetid: 68F908F3-04C5-4B91-B6E5-B7E8357B4154
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: f9b4a12d3049cea37235cc04805a7411a9bdb236
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696491"
---
# <a name="webview-mixed-content-on-android"></a>WebView смешанное содержимое в Android

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Этот элемент управления для платформы Android определяет, может ли [`WebView`](xref:Xamarin.Forms.WebView) отображать смешанное содержимое в приложениях, предназначенных для API 21 или более поздней версии. Смешанное содержимое — это содержимое, которое изначально загружается через HTTPS-соединение, но при этом ресурсы (например, изображения, аудио, видео, таблицы стилей и сценарии) загружаются по протоколу HTTP. Он используется в XAML путем установки присоединяемого свойства [`WebView.MixedContentMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty) в значение перечисления [`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) :

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

Кроме того, его можно использовать с C# помощью API Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>().SetMixedContentMode(MixedContentHandling.AlwaysAllow);
```

Метод `WebView.On<Android>` указывает, что эта платформа будет запускаться только в Android. Метод [`WebView.SetMixedContentMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.SetMixedContentMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.WebView},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)) в пространстве имен [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) используется для управления отображением смешанного содержимого, при этом перечисление [`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) предоставляет три возможных значения:

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow) — указывает, что [`WebView`](xref:Xamarin.Forms.WebView) разрешит источнику HTTPS загрузить содержимое из HTTP-источника.
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow) — указывает, что [`WebView`](xref:Xamarin.Forms.WebView) не позволит источнику HTTPS загружать содержимое из HTTP-источника.
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode) — указывает, что [`WebView`](xref:Xamarin.Forms.WebView) будет пытаться быть совместимым с подходом к последнему веб-браузеру устройства. Может быть разрешено загружать часть содержимого HTTP с помощью HTTPS-источника, а другие типы содержимого будут заблокированы. Блокируемые или разрешенные типы содержимого могут изменяться при каждом выпуске операционной системы.

В результате к [`WebView`у](xref:Xamarin.Forms.WebView)будет применено указанное [`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) значение, которое определяет, можно ли отобразить смешанное содержимое:

[![WebView смешанное содержимое, зависящее от платформы](webview-mixed-content-images/webview-mixedcontent.png "WebView смешанное содержимое, зависящее от платформы")](webview-mixed-content-images/webview-mixedcontent-large.png#lightbox "WebView смешанное содержимое, зависящее от платформы")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
