---
title: Действия контекста ViewCell в Android
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать зависящую от платформы Android платформу, которая позволяет выполнять действия контекста ViewCell в устаревшем режиме.
ms.prod: xamarin
ms.assetid: 8BD71B10-5037-443F-9975-B941CE393E5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/24/2019
ms.openlocfilehash: b040c7c5b7f132b0832469efba91dd89d6b2ddbd
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697749"
---
# <a name="viewcell-context-actions-on-android"></a>Действия контекста ViewCell в Android

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

По умолчанию в Xamarin. Forms 4,3, когда [`ViewCell`](xref:Xamarin.Forms.ViewCell) в приложении Android определяет контекстные действия для каждого элемента в [`ListView`](xref:Xamarin.Forms.ListView), меню контекстных действий обновляется при изменении выбранного элемента в `ListView`. Однако в предыдущих версиях Xamarin. Forms меню контекстных действий не обновлялось, и это поведение называется `ViewCell`ным режимом. Этот устаревший режим может привести к неправильному поведению, если `ListView` использует [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) для задания `ItemTemplate` из [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) объектов, определяющих различные контекстные действия.

Эта платформа для Android включает режим " [`ViewCell`](xref:Xamarin.Forms.ViewCell) контекстных действий" для обеспечения обратной совместимости, чтобы меню контекстных действий не обновлялось при изменении выбранного элемента в [`ListView`](xref:Xamarin.Forms.ListView) . Он используется в XAML путем установки для свойства `ViewCell.IsContextActionsLegacyModeEnabled` BIND значения `true`:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ItemsSource="{Binding Items}">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell android:ViewCell.IsContextActionsLegacyModeEnabled="true">
                        <ViewCell.ContextActions>
                            <MenuItem Text="{Binding Item1Text}" />
                            <MenuItem Text="{Binding Item2Text}" />
                        </ViewCell.ContextActions>
                        <Label Text="{Binding Text}" />
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать с C# помощью API Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

viewCell.On<Android>().SetIsContextActionsLegacyModeEnabled(true);
```

Метод `ViewCell.On<Android>` указывает, что эта платформа будет запускаться только в Android. Метод `ViewCell.SetIsContextActionsLegacyModeEnabled` в пространстве имен [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) используется для включения устаревшего режима меню контекстных действий [`ViewCell`](xref:Xamarin.Forms.ViewCell) , чтобы меню контекстных действий не обновлялось при изменении выбранного элемента в [`ListView`](xref:Xamarin.Forms.ListView) . Кроме того, можно использовать метод `ViewCell.GetIsContextActionsLegacyModeEnabled`, чтобы возвращать, включен ли режим прежних контекстных действий.

На следующих снимках экрана показано [`ViewCell`](xref:Xamarin.Forms.ViewCell) контекстных действий, включенных в режиме Legacy:

![Снимок экрана: включен устаревший режим ViewCell на Android](viewcell-context-actions-images/legacy-mode-enabled.png "Включен устаревший режим ViewCell")

В этом режиме отображаемые элементы меню контекстных действий идентичны для ячейки 1 и ячейки 2, несмотря на то, что для ячейки 2 определены разные элементы контекстного меню.

На следующих снимках экрана показано, [`ViewCell`](xref:Xamarin.Forms.ViewCell) контекстные действия отключены, что является поведением Xamarin. Forms по умолчанию:

![Снимок экрана: устаревший режим ViewCell отключен на Android](viewcell-context-actions-images/legacy-mode-disabled.png "Устаревший режим ViewCell отключен")

В этом режиме для ячейки 1 и ячейки 2 отображаются правильные элементы контекстного меню действий.

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
