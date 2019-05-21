---
title: Цвет фона ячейки в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать iOS конкретных платформ, задающее цвет фона ячеек на iOS.
ms.prod: xamarin
ms.assetid: 2A3FDACF-5AE2-40DE-8488-6FE41733712F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 6b1e2fe534c8b7d0c3346a18d1b82d797e52dba1
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/20/2019
ms.locfileid: "65926763"
---
# <a name="cell-background-color-on-ios"></a>Цвет фона ячейки в iOS

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

Этот iOS платформы задает цвет фона по умолчанию [ `Cell` ](xref:Xamarin.Forms.Cell) экземпляров. Он используется в XAML, задав `Cell.DefaultBackgroundColor` свойство, используемое для [ `Color` ](xref:Xamarin.Forms.Color):

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

Кроме того его можно будет использовать с помощью C# с помощью текучего API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var viewCell = new ViewCell { View = ... };
viewCell.On<iOS>().SetDefaultBackgroundColor(Color.Teal);
```

`ListView.On<iOS>` Метод указывает, что этой платформы будет выполняться только на устройствах iOS. `Cell.SetDefaultBackgroundColor` Метод в [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространства имен, задает цвет фона ячейки с заданным [ `Color` ](xref:Xamarin.Forms.Color). Кроме того `Cell.DefaultBackgroundColor` метод может использоваться для получения текущий цвет фона ячейки.

Результатом является, цвет фона в [ `Cell` ](xref:Xamarin.Forms.Cell) можно задать с определенным [ `Color` ](xref:Xamarin.Forms.Color):

[![Снимок экрана ячейки заголовков групп сине-зеленый, в iOS](cell-background-color-images/group-header-cell-color.png "ListView с ячейки заголовков групп сине-зеленый")](cell-background-color-images/group-header-cell-color-large.png#lightbox "ListView с ячейки заголовков групп сине-зеленый")

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
