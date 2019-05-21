---
title: Домашний видимость индикатора на устройстве iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать iOS конкретных платформ, задает видимость индикатора домашней страницы.
ms.prod: xamarin
ms.assetid: F81022E0-3C6C-49C0-A000-FAF6574D3FB7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
ms.openlocfilehash: c0d6717cd8f89344be7df3dc3ec687a5f1b375f4
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/21/2019
ms.locfileid: "65971626"
---
# <a name="home-indicator-visibility-on-ios"></a>Домашний видимость индикатора на устройстве iOS

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Этот iOS платформы задает видимость домашней индикатора на [ `Page` ](xref:Xamarin.Forms.Page). Он используется в XAML, задав [ `Page.PrefersHomeIndicatorAutoHidden` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Page.PrefersHomeIndicatorAutoHiddenProperty) свойство, используемое для `boolean`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersHomeIndicatorAutoHidden="true">
    ...
</ContentPage>
```

Кроме того его можно будет использовать с помощью C# с помощью текучего API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersHomeIndicatorAutoHidden(true);
```

`Page.On<iOS>` Метод указывает, что этой платформы будет выполняться только на устройствах iOS. [ `Page.SetPrefersHomeIndicatorAutoHidden` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Page.SetPrefersHomeIndicatorAutoHidden(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Page},System.Boolean)) Метод в [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространства имен, управляет видимостью индикатора home. Кроме того [ `Page.PrefersHomeIndicatorAutoHidden` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Page.PrefersHomeIndicatorAutoHidden(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Page})) метод может использоваться для получения видимость домашней индикатора.

Результатом является, видимость домашней индикатора на [ `Page` ](xref:Xamarin.Forms.Page) можно управлять:

![Снимок экрана включена видимость индикатора домашней страницы iOS](page-home-indicator-images/home-indicator-visibility.png "видимость индикатора на домашней странице")

> [!NOTE]
> Этой платформы, которые могут применяться к [ `ContentPage` ](xref:Xamarin.Forms.ContentPage), [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage), [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), и [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)объектов.

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
