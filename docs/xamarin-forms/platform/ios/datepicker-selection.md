---
title: Выбор элемента DatePicker в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS, которая управляет выбором элементов в DatePicker.
ms.prod: xamarin
ms.assetid: 847E69D1-6AE0-4E82-B9C8-919E009C2014
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/15/2020
ms.openlocfilehash: df84cf01909cec564edc9c6c8bb55382a2b9dfe3
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/26/2020
ms.locfileid: "77646692"
---
# <a name="datepicker-item-selection-on-ios"></a>Выбор элемента DatePicker в iOS

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Элементы управления для конкретных платформ iOS, когда выбор элементов происходит в [`DatePicker`](xref:Xamarin.Forms.DatePicker), позволяя пользователю указать, что выбор элементов происходит при просмотре элементов в элементе управления, или только после нажатия кнопки **done (Готово** ). Он используется в XAML путем установки присоединяемого свойства `DatePicker.UpdateMode` в значение перечисления `UpdateMode`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
       <DatePicker MinimumDate="01/01/2020"
                   MaximumDate="12/31/2020"
                   ios:DatePicker.UpdateMode="WhenFinished" />
       ...
    </StackLayout>
</ContentPage>
```

Кроме того его можно будет использовать с помощью C# с помощью текучего API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

datePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

Метод `DatePicker.On<iOS>` указывает, что эта платформа будет запускаться только в iOS. Метод `DatePicker.SetUpdateMode` в пространстве имен [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) используется для управления тем, когда происходит выбор элементов, а перечисление `UpdateMode` предоставляет два возможных значения:

- `Immediately` — выбор элементов происходит при просмотре пользователем элементов в [`DatePicker`](xref:Xamarin.Forms.DatePicker). Это поведение по умолчанию в Xamarin.Forms.
- `WhenFinished` — выбор элементов происходит только после нажатия пользователем кнопки **done (Готово** ) в [`DatePicker`](xref:Xamarin.Forms.DatePicker).

Кроме того, можно использовать метод `SetUpdateMode` для переключения значений перечисления путем вызова метода `UpdateMode`, который возвращает текущий `UpdateMode`:

```csharp
switch (datePicker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        datePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        datePicker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

В результате заданная `UpdateMode` применяется к [`DatePicker`](xref:Xamarin.Forms.DatePicker), который управляет тем, когда происходит выбор элементов:

[![Снимок экрана режимов обновления DatePicker](datepicker-selection-images/datepicker-updatemode.png "DatePicker UpdateMode для конкретной платформы")](datepicker-selection-images/datepicker-updatemode-large.png#lightbox "DatePicker UpdateMode для конкретной платформы")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
