---
title: Масштабирование специальных возможностей для именованных размеров шрифтов в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS, которая отключает масштабирование специальных возможностей для именованных размеров шрифтов.
ms.prod: xamarin
ms.assetid: B443BAF6-E6F6-4A0E-80B5-CAACE6B550EF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/28/2019
ms.openlocfilehash: 13ca4945ed447ae811ee95e308238f04e58ac0bd
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/31/2019
ms.locfileid: "70200081"
---
# <a name="accessibility-scaling-for-named-font-sizes-on-ios"></a>Масштабирование специальных возможностей для именованных размеров шрифтов в iOS

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS отключает масштабирование доступа для именованных размеров шрифтов. Он используется в XAML, задав `Application.EnableAccessibilityScalingForNamedFontSizes` свойство, используемое для `false`:

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

`Application.On<iOS>` Метод указывает, что этой платформы будет выполняться только на устройствах iOS. `Application.SetEnableAccessibilityScalingForNamedFontSizes` [Метод`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) в пространстве имен используется для отключения именованных размеров шрифтов, масштабируемых с помощью параметров специальных возможностей iOS. Кроме того, `Application.GetEnableAccessibilityScalingForNamedFontSizes` метод можно использовать для того, чтобы получить возможность масштабирования именованных размеров шрифтов с помощью параметров специальных возможностей iOS.

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
