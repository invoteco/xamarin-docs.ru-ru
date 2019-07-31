---
title: Инпутвиев порядок чтения в Windows
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу Windows, которая позволяет динамически определять порядок чтения двунаправленного текста.
ms.prod: xamarin
ms.assetid: E61BAEE0-C8B7-4F33-8DDC-FA1B9CA8E81D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: c184424a982aa82712685dbc33ad57422f2f8338
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68651428"
---
# <a name="inputview-reading-order-on-windows"></a>Инпутвиев порядок чтения в Windows

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта универсальная платформа Windows зависит от конкретной платформы [`Entry`](xref:Xamarin.Forms.Entry), а также от того, в каком экземпляре, и [`Label`](xref:Xamarin.Forms.Label) в каком-то конкретном случае [`Editor`](xref:Xamarin.Forms.Editor)будет определяться динамический текст (слева направо или справа налево). Он используется в XAML, задав [ `InputView.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.DetectReadingOrderFromContentProperty) (для `Entry` и `Editor` экземпляры) или [ `Label.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Label.DetectReadingOrderFromContentProperty) вложенное свойство, чтобы `boolean` значение:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Editor ... windows:InputView.DetectReadingOrderFromContent="true" />
        ...
    </StackLayout>
</ContentPage>
```

Кроме того его можно будет использовать с помощью C# с помощью текучего API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

editor.On<Windows>().SetDetectReadingOrderFromContent(true);
```

`Editor.On<Windows>` Метод указывает, что этой платформы будет выполняться только на универсальной платформе Windows. [ `InputView.SetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.SetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView},System.Boolean)) Метод в [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространства имен, используется для управления ли порядок чтения определяется из содержимого [ `InputView` ](xref:Xamarin.Forms.InputView). Кроме того `InputView.SetDetectReadingOrderFromContent` метод может использоваться для переключения между режимами порядок чтения определяется из содержимого путем вызова [ `InputView.GetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.GetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView})) метод для возврата текущего значения:

```csharp
editor.On<Windows>().SetDetectReadingOrderFromContent(!editor.On<Windows>().GetDetectReadingOrderFromContent());
```

В результате [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), и [ `Label` ](xref:Xamarin.Forms.Label) экземпляров могут быть порядок чтения их содержимое, динамически обнаружены:

[![Обнаружение порядок чтения из содержимого платформы InputView](inputview-reading-order-images/editor-readingorder.png "InputView, обнаружение порядок чтения из содержимого платформы")](inputview-reading-order-images/editor-readingorder-large.png#lightbox "InputView, обнаружение порядок чтения из содержимое конкретной платформы")

> [!NOTE]
> В отличие от параметра [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) свойство, логика для представлений, которые позволяют обнаружить порядок чтения из их текстовое содержимое не повлияет на способ выравнивания текста в представлении. Вместо этого он корректирует порядок, в котором расположены блоки двунаправленного текста.

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ВиндовсспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
