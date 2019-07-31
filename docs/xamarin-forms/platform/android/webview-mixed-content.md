---
title: WebView смешанное содержимое в Android
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать платформу Android, которая отображает смешанное содержимое в WebView в приложениях, предназначенных для API 21 или более поздней версии.
ms.prod: xamarin
ms.assetid: 68F908F3-04C5-4B91-B6E5-B7E8357B4154
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 286a7dceead327d727110d4ebbcecbc2341345b3
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656071"
---
# <a name="webview-mixed-content-on-android"></a>WebView смешанное содержимое в Android

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Этот элемент управления для платформы Android определяет, [`WebView`](xref:Xamarin.Forms.WebView) может ли объект отображать смешанное содержимое в приложениях, предназначенных для API 21 или более поздней версии. Смешанное содержимое является содержимым, изначально загружается через HTTPS-соединение, но который загружает ресурсы (например, изображения, аудио, видео, таблицы стилей, сценарии) по протоколу HTTP. Он используется в XAML, задав [ `WebView.MixedContentMode` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty) присоединенное свойство в значение [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) перечисления:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

Кроме того его можно будет использовать с помощью C# с помощью текучего API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>().SetMixedContentMode(MixedContentHandling.AlwaysAllow);
```

`WebView.On<Android>` Метод указывает, что этой платформы будет выполняться только в Android. [ `WebView.SetMixedContentMode` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.SetMixedContentMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.WebView},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)) Метод в [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) пространства имен, используется для управления ли смешанное содержимое может отображаться, с помощью [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) Перечисление, предоставляя три возможных значения:

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow) — Указывает, что [ `WebView` ](xref:Xamarin.Forms.WebView) позволит источник HTTPS для загрузки содержимого из источника HTTP.
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow) — Указывает, что [ `WebView` ](xref:Xamarin.Forms.WebView) не позволит источник HTTPS для загрузки содержимого из источника HTTP.
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode) — Указывает, что [ `WebView` ](xref:Xamarin.Forms.WebView) попытается совместимы с подходом последние веб-браузере устройства. Часть содержимого HTTP может быть разрешен загружается источник HTTPS и другие типы содержимого будет заблокирован. Типы содержимого, заблокированы или разрешены могут быть изменены с каждым выпуском операционной системы.

Результатом является то, что указанный [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) значение применяется к [ `WebView` ](xref:Xamarin.Forms.WebView), который управляет отображением смешанное содержимое:

[![WebView смешанный обработки содержимого платформы](webview-mixed-content-images/webview-mixedcontent.png "WebView смешанный обработки содержимого платформы")](webview-mixed-content-images/webview-mixedcontent-large.png#lightbox "WebView Смешанные платформы обработки содержимого")

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
