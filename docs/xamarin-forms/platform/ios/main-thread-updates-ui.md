---
title: Основные обновления управления потоком в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать особенности платформы iOS, которые позволяют выполнять макет элемента управления и обновления отрисовки в основном потоке.
ms.prod: xamarin
ms.assetid: 945E711D-9BD2-4BF9-9FB3-CBE0D5B25A49
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: d55ef4a97d5d4f320bf152ba05c86aff82eb2f1e
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/31/2019
ms.locfileid: "70200126"
---
# <a name="main-thread-control-updates-on-ios"></a>Основные обновления управления потоком в iOS

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS позволяет выполнять обновления элементов управления и отрисовки в основном потоке, а не выполнять в фоновом потоке. Он понадобится редко, но в некоторых случаях может предотвратить сбои. Его использования в XAML, задав `Application.HandleControlUpdatesOnMainThread` свойство, используемое для `true`:

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.HandleControlUpdatesOnMainThread="true">
    ...
</Application>
```

Кроме того его можно будет использовать с помощью C# с помощью текучего API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetHandleControlUpdatesOnMainThread(true);
```

`Application.On<iOS>` Метод указывает, что этой платформы будет выполняться только на устройствах iOS. `Application.SetHandleControlUpdatesOnMainThread` Метод в [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространства имен, используется для управления ли макет элемента управления и отрисовки обновления выполняются в основном потоке, а не выполняется в фоновом потоке. Кроме того `Application.GetHandleControlUpdatesOnMainThread` метод может использоваться для возврата, которые выполняются ли структура элемента управления и Подготовка к просмотру обновлений в основном потоке.

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
