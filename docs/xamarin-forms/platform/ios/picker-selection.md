---
title: Выбор элемента средства выбора в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать особенности платформы iOS, которые используются для управления выбором элементов в средстве выбора.
ms.prod: xamarin
ms.assetid: 26B0604A-BD30-49FD-83A6-F0EDFBB0524B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 57420921100c99db1e2c3a5259ece30cfda719f2
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68651844"
---
# <a name="picker-item-selection-on-ios"></a>Выбор элемента средства выбора в iOS

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Элементы управления для конкретных платформ iOS при выборе элементов в [`Picker`](xref:Xamarin.Forms.Picker), позволяя пользователю указать, что выбор элементов выполняется при просмотре элементов в элементе управления, или только после нажатия кнопки Done ( **Готово** ). Он используется в XAML, задав `Picker.UpdateMode` присоединенное свойство в значение `UpdateMode` перечисления:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <Picker ... Title="Select a monkey" ios:Picker.UpdateMode="WhenFinished">
          ...
        </Picker>
        ...
    </StackLayout>
</ContentPage>
```

Кроме того его можно будет использовать с помощью C# с помощью текучего API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

`Picker.On<iOS>` Метод указывает, что этой платформы будет выполняться только на устройствах iOS. `Picker.SetUpdateMode` Метод в [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространства имен, используется для управления, когда происходит выбор элементов, с помощью `UpdateMode` перечисления, предоставляя два возможных значения:

- `Immediately` — Выбор элемента происходит, когда пользователь выбирает элементы в [ `Picker` ](xref:Xamarin.Forms.Picker). Это поведение по умолчанию в Xamarin.Forms.
- `WhenFinished` — Выбор элементов только происходит, когда пользователь нажал **сделать** кнопку [ `Picker` ](xref:Xamarin.Forms.Picker).

Кроме того `SetUpdateMode` метод может использоваться для включения значений перечисления, вызвав `UpdateMode` метод, который возвращает текущий `UpdateMode`:

```csharp
switch (picker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        picker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

Результатом является то, что указанный `UpdateMode` применяется к [ `Picker` ](xref:Xamarin.Forms.Picker), какие элементы управления, когда происходит выбор элементов:

[![](picker-selection-images/picker-updatemode.png "Средство выбора UpdateMode платформы")](picker-selection-images/picker-updatemode-large.png#lightbox "выбора UpdateMode специфические для платформы")

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
