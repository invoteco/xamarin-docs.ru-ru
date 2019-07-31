---
title: Стиль заголовка группы ListView в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS, которая управляет тем, будут ли ячейки заголовка ListView перемещаться во время прокрутки.
ms.prod: xamarin
ms.assetid: 099B2C7F-727F-4BCF-903B-87E728108C24
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: f40737799f63c6e0c61fcc6f4f59584222a49d6d
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648325"
---
# <a name="listview-group-header-style-on-ios"></a>Стиль заголовка группы ListView в iOS

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS определяет, будут ли [`ListView`](xref:Xamarin.Forms.ListView) ячейки заголовка перемещаться во время прокрутки. Он используется в XAML путем задания `ListView.GroupHeaderStyle` свойству BIND значения `GroupHeaderStyle` перечисления.

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.GroupHeaderStyle="Grouped">
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

listView.On<iOS>().SetGroupHeaderStyle(GroupHeaderStyle.Grouped);
```

`ListView.On<iOS>` Метод указывает, что этой платформы будет выполняться только на устройствах iOS. Метод в пространстве имен используется для управления тем, будут ли [`ListView`](xref:Xamarin.Forms.ListView) ячейки заголовка перемещаться во время прокрутки. [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) `ListView.SetGroupHeaderStyle` `GroupHeaderStyle` Перечисление предоставляет два возможных значения:

- `Plain`— Указывает, что ячейки заголовка имеют плавающее значение [`ListView`](xref:Xamarin.Forms.ListView) при прокрутке (по умолчанию).
- `Grouped`— Указывает, что ячейки заголовка не перемещаются при [`ListView`](xref:Xamarin.Forms.ListView) прокрутке.

Кроме того, `ListView.GetGroupHeaderStyle` метод можно использовать для возврата объекта `GroupHeaderStyle` [`ListView`](xref:Xamarin.Forms.ListView), который применяется к.

Результатом является то, что указанное `GroupHeaderStyle` значение применяется к элементу [`ListView`](xref:Xamarin.Forms.ListView), который определяет, будут ли ячейки заголовка перемещаться во время прокрутки:

[ ![Снимок экрана с плавающими и неплавающими ячейками заголовков ListView в ListView iOS](listview-group-header-style-images/group-header-styles.png "с плавающими и неплавающими ячейками заголовка") ] (listview-group-header-style-images/group-header-styles-large.png#lightbox "ListView с плавающими и не плавающими ячейками заголовка")

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
