---
title: Выбор элемента TimePicker в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать особенности платформы iOS, которые используются для управления выбором элементов в TimePicker.
ms.prod: xamarin
ms.assetid: 554AC877-8698-4B8C-A676-80DD64305A06
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/15/2020
ms.openlocfilehash: 818f368da8ebb375fbacd97d3d48185ba60470d4
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/26/2020
ms.locfileid: "77646680"
---
# <a name="timepicker-item-selection-on-ios"></a>Выбор элемента TimePicker в iOS

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Элементы управления для конкретных платформ iOS, когда выбор элементов происходит в [`TimePicker`](xref:Xamarin.Forms.TimePicker), позволяя пользователю указать, что выбор элементов происходит при просмотре элементов в элементе управления, или только после нажатия кнопки **done (Готово** ). Он используется в XAML путем установки присоединяемого свойства `TimePicker.UpdateMode` в значение перечисления `UpdateMode`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
       <TimePicker Time="14:00:00"
                   ios:TimePicker.UpdateMode="WhenFinished" />
       ...
    </StackLayout>
</ContentPage>
```

Кроме того его можно будет использовать с помощью C# с помощью текучего API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

timePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

Метод `TimePicker.On<iOS>` указывает, что эта платформа будет запускаться только в iOS. Метод `TimePicker.SetUpdateMode` в пространстве имен [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) используется для управления тем, когда происходит выбор элементов, а перечисление `UpdateMode` предоставляет два возможных значения:

- `Immediately` — выбор элементов происходит при просмотре пользователем элементов в [`TimePicker`](xref:Xamarin.Forms.TimePicker). Это поведение по умолчанию в Xamarin.Forms.
- `WhenFinished` — выбор элементов происходит только после нажатия пользователем кнопки **done (Готово** ) в [`TimePicker`](xref:Xamarin.Forms.TimePicker).

Кроме того, можно использовать метод `SetUpdateMode` для переключения значений перечисления путем вызова метода `UpdateMode`, который возвращает текущий `UpdateMode`:

```csharp
switch (timePicker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        timePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        timePicker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

В результате заданная `UpdateMode` применяется к [`TimePicker`](xref:Xamarin.Forms.TimePicker), который управляет тем, когда происходит выбор элементов:

[![Снимок экрана: режимы обновления TimePicker](timepicker-selection-images/timepicker-updatemode.png "TimePicker UpdateMode для конкретной платформы")](timepicker-selection-images/timepicker-updatemode-large.png#lightbox "TimePicker UpdateMode для конкретной платформы")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
