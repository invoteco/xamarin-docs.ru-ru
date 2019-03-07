---
title: Стиль заголовка группы ListView на iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать специфические для платформы iOS, управляет ли ячеек заголовков ListView число с плавающей запятой при прокрутке.
ms.prod: xamarin
ms.assetid: 099B2C7F-727F-4BCF-903B-87E728108C24
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 58b3787b71cff9b78f1c6b577be6c320367f1cee
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557456"
---
# <a name="listview-group-header-style-on-ios"></a>Стиль заголовка группы ListView на iOS

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Эта определяет специфические для платформы iOS ли [ `ListView` ](xref:Xamarin.Forms.ListView) ячеек заголовков, число с плавающей запятой при прокрутке. Он используется в XAML, задав `ListView.GroupHeaderStyle` свойство, используемое значение `GroupHeaderStyle` перечисления:

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

`ListView.On<iOS>` Метод указывает, что этой платформы будет выполняться только на устройствах iOS. `ListView.SetGroupHeaderStyle` Метод в [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространства имен, используемый для управления ли [ `ListView` ](xref:Xamarin.Forms.ListView) ячеек заголовков, число с плавающей запятой при прокрутке. `GroupHeaderStyle` Перечисление предоставляет два возможных значения:

- `Plain` — Указывает, что заголовку число с плавающей запятой при [ `ListView` ](xref:Xamarin.Forms.ListView) является прокручивать (по умолчанию).
- `Grouped` — Указывает, что ячейки заголовков не перемещаются при [ `ListView` ](xref:Xamarin.Forms.ListView) прокручена.

Кроме того `ListView.GetGroupHeaderStyle` метод может использоваться для возврата `GroupHeaderStyle` , применяемый к [ `ListView` ](xref:Xamarin.Forms.ListView).

Результатом является то, что указанный `GroupHeaderStyle` значение применяется к [ `ListView` ](xref:Xamarin.Forms.ListView), которой управляет ли ячеек заголовков перемещаться при прокрутке:

[![Снимок экрана с плавающей запятой и неплавающего ListView ячеек заголовка, в iOS](listview-group-header-style-images/group-header-styles.png "ListView с ячеек заголовков с плавающей запятой и неплавающего")](listview-group-header-style-images/group-header-styles-large.png#lightbox "ListView с ячеек заголовков с плавающей запятой и не с плавающей запятой")

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
