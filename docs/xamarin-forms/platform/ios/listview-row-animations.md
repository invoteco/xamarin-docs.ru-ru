---
title: Анимация строк ListView в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать особенности платформы iOS, которые определяют, отключаются ли анимации строк при обновлении коллекции элементов ListView.
ms.prod: xamarin
ms.assetid: E8F5103F-4D8E-4A5A-A16C-7FA14EE786AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/21/2019
ms.openlocfilehash: 9e44c22e670847102cf0bc0f79a860abcac30760
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68652836"
---
# <a name="listview-row-animations-on-ios"></a>Анимация строк ListView в iOS

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS определяет, отключается ли анимация строк при [`ListView`](xref:Xamarin.Forms.ListView) обновлении коллекции Items. Он используется в XAML, задав `ListView.RowAnimationsEnabled` свойство, используемое для `false`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.RowAnimationsEnabled="false">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Кроме того его можно будет использовать с помощью C# с помощью текучего API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetRowAnimationsEnabled(false);
```

`ListView.On<iOS>` Метод указывает, что этой платформы будет выполняться только на устройствах iOS. Метод в пространстве имен используется для управления тем, отключается ли [`ListView`](xref:Xamarin.Forms.ListView) анимация строк при обновлении коллекции Items. [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) `ListView.SetRowAnimationsEnabled` Кроме того, `ListView.GetRowAnimationsEnabled` метод можно использовать для возврата к отключению `ListView`анимации строк в.

> [!NOTE]
> [`ListView`](xref:Xamarin.Forms.ListView)анимация по строкам включена по умолчанию. Таким образом, анимация возникает при вставке новой строки в `ListView`.

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
