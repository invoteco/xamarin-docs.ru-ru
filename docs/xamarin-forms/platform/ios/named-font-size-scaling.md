---
title: Специальные возможности масштабирования для с именем размеры шрифтов, в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать iOS конкретных платформ, отключает специальные возможности масштабирования для размеров шрифта именованный.
ms.prod: xamarin
ms.assetid: B443BAF6-E6F6-4A0E-80B5-CAACE6B550EF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/28/2019
ms.openlocfilehash: 1fc6fefe1f9fe48fe2abb367b376a5a6f3484462
ms.sourcegitcommit: 0fd04ea3af7d6a6d6086525306523a5296eec0df
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2019
ms.locfileid: "67517953"
---
# <a name="accessibility-scaling-for-named-font-sizes-on-ios"></a>Специальные возможности масштабирования для с именем размеры шрифтов, в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

Этот iOS платформы отключает специальные возможности масштабирования для размеров шрифта именованный. Он используется в XAML, задав `Application.EnableAccessibilityScalingForNamedFontSizes` свойство, используемое для `false`:

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.EnableAccessibilityScalingForNamedFontSizes="false">
    ...
</Application>
```

Кроме того его можно будет использовать с помощью C# с помощью текучего API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetEnableAccessibilityScalingForNamedFontSizes(false);
```

`Application.On<iOS>` Метод указывает, что этой платформы будет выполняться только на устройствах iOS. `Application.SetEnableAccessibilityScalingForNamedFontSizes` Метод в [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространства имен, используется для отключения размеры шрифта именованный масштабирование с помощью параметров специальных возможностей iOS. Кроме того `Application.GetEnableAccessibilityScalingForNamedFontSizes` метод может использоваться для возврата, уменьшается ли размеры шрифта именованный параметры специальных возможностей iOS.

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
