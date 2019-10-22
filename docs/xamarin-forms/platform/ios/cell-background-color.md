---
title: Цвет фона ячейки в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать зависящую от платформы iOS, которая задает цвет фона по умолчанию для ячеек в iOS.
ms.prod: xamarin
ms.assetid: 2A3FDACF-5AE2-40DE-8488-6FE41733712F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 24276dce97e4935ba41d7012cf6a9aa8fa2658a8
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "68651374"
---
# <a name="cell-background-color-on-ios"></a>Цвет фона ячейки в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS задает цвет фона по умолчанию для экземпляров [`Cell`](xref:Xamarin.Forms.Cell) . Он используется в XAML путем установки для свойства `Cell.DefaultBackgroundColor` BIND значения [`Color`](xref:Xamarin.Forms.Color):

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  IsGroupingEnabled="true">
            <ListView.GroupHeaderTemplate>
                <DataTemplate>
                    <ViewCell ios:Cell.DefaultBackgroundColor="Teal">
                        <Label Margin="10,10"
                               Text="{Binding Key}"
                               FontAttributes="Bold" />
                    </ViewCell>
                </DataTemplate>
            </ListView.GroupHeaderTemplate>
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

var viewCell = new ViewCell { View = ... };
viewCell.On<iOS>().SetDefaultBackgroundColor(Color.Teal);
```

Метод `ListView.On<iOS>` указывает, что эта платформа будет запускаться только в iOS. Метод `Cell.SetDefaultBackgroundColor` в пространстве имен [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) задает цвет фона ячейки указанным [`Color`](xref:Xamarin.Forms.Color). Кроме того, можно использовать метод `Cell.DefaultBackgroundColor` для получения текущего цвета фона ячейки.

В результате цвет фона в [`Cell`](xref:Xamarin.Forms.Cell) может быть задан для определенного [`Color`](xref:Xamarin.Forms.Color):

[![Снимок экрана: ячейки заголовка синего группы в iOS](cell-background-color-images/group-header-cell-color.png "ListView с синей ячейкой заголовка группы")](cell-background-color-images/group-header-cell-color-large.png#lightbox "ListView с синей ячейкой заголовка группы")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
