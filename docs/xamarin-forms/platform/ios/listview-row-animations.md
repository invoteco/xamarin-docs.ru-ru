---
title: ListView строка анимаций в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать специфические для платформы iOS, управляющий, отключены ли строка анимаций при обновлении коллекции элементов ListView.
ms.prod: xamarin
ms.assetid: E8F5103F-4D8E-4A5A-A16C-7FA14EE786AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/21/2019
ms.openlocfilehash: 50480f5b21c6f0c855ff6f9aa22b6126c6a6787c
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557446"
---
# <a name="listview-row-animations-on-ios"></a>ListView строка анимаций в iOS

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Эта определяет специфические для платформы iOS ли строка анимаций отключены при [ `ListView` ](xref:Xamarin.Forms.ListView) обновляется коллекция элементов. Он используется в XAML, задав `ListView.RowAnimationsEnabled` свойство, используемое для `false`:

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

`ListView.On<iOS>` Метод указывает, что этой платформы будет выполняться только на устройствах iOS. `ListView.SetRowAnimationsEnabled` Метод в [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространства имен, используется для управления ли строка анимаций, отключен при [ `ListView` ](xref:Xamarin.Forms.ListView) обновляется коллекция элементов. Кроме того `ListView.GetRowAnimationsEnabled` метод может использоваться для возврата, отключены ли строка анимаций на `ListView`.

> [!NOTE]
> [`ListView`](xref:Xamarin.Forms.ListView) Строка анимаций включены по умолчанию. Таким образом, анимация происходит при вставке новой строки в `ListView`.

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
