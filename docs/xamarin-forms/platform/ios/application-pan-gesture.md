---
title: Одновременный распознавание жестов панорамирования в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS, которая позволяет использовать одновременный распознавание жестов в приложении.
ms.prod: xamarin
ms.assetid: 883D89DA-F8FF-4B97-9C3F-2DD05C96A495
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 8e86141ac27999a71a84ae7150b19ef3f60c117f
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655983"
---
# <a name="simultaneous-pan-gesture-recognition-on-ios"></a>Одновременный распознавание жестов панорамирования в iOS

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Когда [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) прикреплено к представлению внутри прокрутки, все pan, охватывает жесты `PanGestureRecognizer` и не передаются представления с прокруткой. Таким образом больше не прокручивается представления с прокруткой.

Эта платформа iOS позволяет `PanGestureRecognizer` в представлении прокрутки захватывать и совместно использовать жест панорамирования с прокруткой. Он используется в XAML, задав [ `Application.PanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.PanGestureRecognizerShouldRecognizeSimultaneouslyProperty) вложенное свойство, чтобы `true`:

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.PanGestureRecognizerShouldRecognizeSimultaneously="true">
    ...
</Application>
```

Кроме того его можно будет использовать с помощью C# с помощью текучего API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetPanGestureRecognizerShouldRecognizeSimultaneously(true);
```

`Application.On<iOS>` Метод указывает, что этой платформы будет выполняться только на устройствах iOS. [ `Application.SetPanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.SetPanGestureRecognizerShouldRecognizeSimultaneously(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Application},System.Boolean)) Метод в [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространства имен, используется для управления ли распознавателя жестов pan в области прокрутки будет захват жест pan, или записи, так и совместное использование pan жестов с помощью представления с прокруткой. Кроме того [ `Application.GetPanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.GetPanGestureRecognizerShouldRecognizeSimultaneously(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Application})) метод может использоваться для возврата ли жест pan используется совместно с прокруткой представление, содержащее [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer).

Таким образом, с помощью этой платформы включен, когда [ `ListView` ](xref:Xamarin.Forms.ListView) содержит [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer), оба `ListView` и `PanGestureRecognizer` получит жест pan и Обрабатывайте его. Однако при использовании этой платформы отключен, если `ListView` содержит `PanGestureRecognizer`, `PanGestureRecognizer` помещается жест pan и обработать его и `ListView` не будут получать pan жест.

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
