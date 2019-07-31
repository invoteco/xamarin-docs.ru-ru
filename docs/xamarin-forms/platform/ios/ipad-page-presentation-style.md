---
title: Стиль представления модальной страницы iPad
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать специфические для платформы iOS стили представления модальной страницы на iPad.
ms.prod: xamarin
ms.assetid: C791F7CF-330A-44BA-987A-4CFCCBB9278B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: c8962c46e0b496844bd3fc00346a117b6753f818
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68651944"
---
# <a name="ipad-modal-page-presentation-style"></a>Стиль представления модальной страницы iPad

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS используется для установки стиля представления модальной страницы на iPad. Он используется в XAML, задав `Page.ModalPresentationStyle` свойство, используемое для `UIModalPresentationStyle` значение перечисления:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.ModalPresentationStyle="FormSheet">
    ...
</ContentPage>
```

Кроме того его можно будет использовать с помощью C# с помощью текучего API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSModalFormSheetPageCS : ContentPage
{
    public iOSModalFormSheetPageCS()
    {
        On<iOS>().SetModalPresentationStyle(UIModalPresentationStyle.FormSheet);
        ...
    }
}
```

`Page.On<iOS>` Метод указывает, что этой платформы будет выполняться только на устройствах iOS. `Page.SetModalPresentationStyle` Метод в [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространства имен, используемый для задания стиля модальное презентации на [ `Page` ](xref:Xamarin.Forms.Page) , указав одно из следующих `UIModalPresentationStyle` перечисления значения:

- `FullScreen`, который задает стиль модальное презентации, охватывают весь экран. По умолчанию модальные страницы отображаются при использовании этого стиля представления.
- `FormSheet`, который задает стиль модальное презентации по центру на и меньше, чем экрана.

Кроме того `GetModalPresentationStyle` метод может использоваться для получения текущего значения `UIModalPresentationStyle` перечисления, который применяется к [ `Page` ](xref:Xamarin.Forms.Page).

Результатом является, стиль модальное презентации на [ `Page` ](xref:Xamarin.Forms.Page) можно задать:

[![](page-presentation-style-images/modal-presentation-style-small.png "Модальное стили презентаций на iPad")](page-presentation-style-images/modal-presentation-style-large.png#lightbox "модальное стили презентаций на iPad")

> [!NOTE]
> Страниц с использованием этой платформы требуется задать стиль модальные представления необходимо использовать модальной навигации. Дополнительные сведения см. в разделе [модальные страницы Xamarin.Forms](~/xamarin-forms/app-fundamentals/navigation/modal.md).

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
