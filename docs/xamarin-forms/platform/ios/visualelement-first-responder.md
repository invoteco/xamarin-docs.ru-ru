---
title: Первый ответчик Висуалелемент в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS, которая позволяет объекту Висуалелемент стать первым отвечающим на события касания.
ms.prod: xamarin
ms.assetid: 3A77BA02-B87A-44EC-AC51-9D3130EF314C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/15/2020
ms.openlocfilehash: be6c233b63d172d2fcacb1cea7f5e9aeeb7faed1
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/26/2020
ms.locfileid: "77646716"
---
# <a name="visualelement-first-responder-on-ios"></a>Первый ответчик Висуалелемент в iOS

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS позволяет объекту [`VisualElement`](xref:Xamarin.Forms.VisualElement) стать первым отвечающим на события касания, а не со страницы, содержащей элемент. Он используется в XAML путем установки для свойства `VisualElement.CanBecomeFirstResponder` BIND значения `true`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Entry Placeholder="Enter text" />
        <Button ios:VisualElement.CanBecomeFirstResponder="True"
                Text="OK" />
    </StackLayout>
</ContentPage>
```

Кроме того его можно будет использовать с помощью C# с помощью текучего API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Entry entry = new Entry { Placeholder = "Enter text" };
Button button = new Button { Text = "OK" };
button.On<iOS>().SetCanBecomeFirstResponder(true);
```

Метод `VisualElement.On<iOS>` указывает, что эта платформа будет запускаться только в iOS. Метод `VisualElement.SetCanBecomeFirstResponder` в пространстве имен [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) используется для установки `VisualElement` в качестве первого респондента для событий касания. Кроме того, можно использовать метод `VisualElement.CanBecomeFirstResponder`, чтобы возвращать `VisualElement` является первым респондентом для событий касания.

В результате [`VisualElement`](xref:Xamarin.Forms.VisualElement) может стать первым респондентом для событий касания, а не со страницы, содержащей элемент. Это позволяет таким сценариям, как чат-приложения, не закрывать клавиатуру при касании [`Button`](xref:Xamarin.Forms.Button) .

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
