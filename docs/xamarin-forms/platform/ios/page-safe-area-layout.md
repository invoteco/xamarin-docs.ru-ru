---
title: Структура безопасного макета области в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS, которая обеспечивает расположение содержимого страницы в области экрана, которая является надежной для всех устройств, использующих iOS 11 и более поздние версии.
ms.prod: xamarin
ms.assetid: 2B6789C1-39B4-4C16-ADE1-3ED3378EAC63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: c6a2ec5a4d1466b7118e6cc7b03cc5518b27e2fb
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68644534"
---
# <a name="safe-area-layout-guide-on-ios"></a>Структура безопасного макета области в iOS

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS предназначена для того, чтобы обеспечить расположение содержимого страницы в области экрана, которая является надежной для всех устройств, использующих iOS 11 и более поздних версий. В частности, полезно убедитесь, что содержимое не попадают в устройство со скругленными углами, индикатор домашней или корпусе датчика на iPhone X. Он используется в XAML, задав `Page.UseSafeArea` вложенное свойство, чтобы `boolean` значение:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Safe Area"
             ios:Page.UseSafeArea="true">
    <StackLayout>
        ...
    </StackLayout>
</ContentPage>
```

Кроме того его можно будет использовать с помощью C# с помощью текучего API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetUseSafeArea(true);
```

`Page.On<iOS>` Метод указывает, что этой платформы будет выполняться только на устройствах iOS. `Page.SetUseSafeArea` Метод в [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространства имен, определяет, включена ли направляющая макета безопасной области.

Результатом является то, что содержимое страницы может располагаться на той части экрана, которое безопасно для всех iPhone:

[![](page-safe-area-images/safe-area-layout.png "Направляющая макета безопасной области")](page-safe-area-images/safe-area-layout-large.png#lightbox "направляющая макета безопасной области")

> [!NOTE]
> Безопасные области, определяемой Apple, используемый для задания в Xamarin.Forms [ `Page.Padding` ](xref:Xamarin.Forms.Page.Padding) свойство и переопределяет все предыдущие значения этого свойства, которые были установлены.

Безопасной области можно настраивать путем извлечения его [ `Thickness` ](xref:Xamarin.Forms.Thickness) со значением `Page.SafeAreaInsets` метода из [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространства имен. Его можно изменить как требуется и переназначается `Padding` свойства в конструкторе страниц или [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) переопределить:

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    var safeInsets = On<iOS>().SafeAreaInsets();
    safeInsets.Left = 20;
    Padding = safeInsets;
}
```

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
