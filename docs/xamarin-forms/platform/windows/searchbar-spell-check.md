---
title: " Сеарчбар Проверка орфографии в Windows"
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу Windows, которая позволяет Сеарчбар взаимодействовать с модулем проверки орфографии.
ms.prod: xamarin
ms.assetid: 7974C91F-7502-4DB3-B0E9-C45E943DDA26
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: da75d42e0e0bcf38dd2ff50999b705125dc4e2b5
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723616"
---
# <a name="searchbar-spell-check-on-windows"></a>Сеарчбар Проверка орфографии в Windows

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта универсальная платформа Windows, зависящая от платформы, позволяет [`SearchBar`](xref:Xamarin.Forms.SearchBar) взаимодействовать с модулем проверки орфографии. Он используется в XAML, задав [ `SearchBar.IsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.IsSpellCheckEnabledProperty) вложенное свойство, чтобы `boolean` значение:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ... windows:SearchBar.IsSpellCheckEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Кроме того его можно будет использовать с помощью C# с помощью текучего API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

searchBar.On<Windows>().SetIsSpellCheckEnabled(true);
```

`SearchBar.On<Windows>` Метод указывает, что этой платформы будет выполняться только на универсальной платформе Windows. [ `SearchBar.SetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.SetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar},System.Boolean)) Метод в [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространства имен, включает средство проверки орфографии и выключает. Кроме того `SearchBar.SetIsSpellCheckEnabled` метод может использоваться для включения проверки орфографии, вызвав [ `SearchBar.GetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.GetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar})) метод для возврата, включен ли средство проверки орфографии:

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

Результатом является этот текст, введенный в [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) можно проверять орфографию, с помощью неправильное написание указан для пользователя:

![Сеарчбар проверка орфографии для конкретной платформы](searchbar-spell-check-images/searchbar-spellcheck.png "Сеарчбар проверка орфографии для конкретной платформы")

> [!NOTE]
> `SearchBar` В класс [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространство имен также содержит [ `EnableSpellCheck` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*) и [ `DisableSpellCheck` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*) методы, которые могут использоваться для включения и отключения Средство проверки орфографии на `SearchBar`, соответственно.

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ВиндовсспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
