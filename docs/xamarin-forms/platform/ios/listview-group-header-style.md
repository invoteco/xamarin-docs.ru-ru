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
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "68648325"
---
# <a name="listview-group-header-style-on-ios"></a>Стиль заголовка группы ListView в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Этот элемент управления для конкретной платформы iOS определяет, [`ListView`](xref:Xamarin.Forms.ListView) ли ячейки заголовка перемещаться во время прокрутки. Он используется в XAML путем установки для свойства `ListView.GroupHeaderStyle` BIND значения `GroupHeaderStyle` перечисления:

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

Кроме того, его можно использовать с C# помощью API Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetGroupHeaderStyle(GroupHeaderStyle.Grouped);
```

Метод `ListView.On<iOS>` указывает, что эта платформа будет запускаться только в iOS. Метод `ListView.SetGroupHeaderStyle` в пространстве имен [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) используется для управления тем, будут ли [`ListView`](xref:Xamarin.Forms.ListView) ячейки заголовка перемещаться во время прокрутки. Перечисление `GroupHeaderStyle` предоставляет два возможных значения:

- `Plain` — указывает, что ячейки заголовка имеют плавающее значение при прокрутке [`ListView`](xref:Xamarin.Forms.ListView) (по умолчанию).
- `Grouped` — указывает, что ячейки заголовка не перемещаются при прокрутке [`ListView`](xref:Xamarin.Forms.ListView) .

Кроме того, можно использовать метод `ListView.GetGroupHeaderStyle` для возврата `GroupHeaderStyle`, применяемого к [`ListView`](xref:Xamarin.Forms.ListView).

Результатом является то, что к [`ListView`у](xref:Xamarin.Forms.ListView)применяется указанное `GroupHeaderStyle` значение, которое управляет тем, будут ли ячейки заголовка перемещаться во время прокрутки:

[![Снимок экрана с плавающими и неплавающими ячейками заголовка ListView в iOS](listview-group-header-style-images/group-header-styles.png "ListView с плавающими и не плавающими ячейками заголовка")](listview-group-header-style-images/group-header-styles-large.png#lightbox "ListView с плавающими и не плавающими ячейками заголовка")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
