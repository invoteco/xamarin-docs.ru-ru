---
title: Цветовой режим текста на панели Навигатионпаже в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS, которая определяет, совпадает ли цвет текста строки состояния в Навигатионпаже с яркостью панели навигации.
ms.prod: xamarin
ms.assetid: 03698A44-39F1-4030-9AF5-F10A6713828A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 5b12fbb3f7a0284dc78c51159b173dfdda300dc7
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68651695"
---
# <a name="navigationpage-bar-text-color-mode-on-ios"></a>Цветовой режим текста на панели Навигатионпаже в iOS

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта определяет специфические для платформы ли текст строки состояния цвета на [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) настраивается в соответствии с яркость панели навигации. Он используется в XAML, задав [ `NavigationPage.StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty) присоединенное свойство в значение [ `StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) перечисления:

```xaml
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    x:Class="PlatformSpecifics.iOSStatusBarTextColorModePage">
    <MasterDetailPage.Master>
        <ContentPage Title="Master Page Title" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <NavigationPage BarBackgroundColor="Blue" BarTextColor="White"
                        ios:NavigationPage.StatusBarTextColorMode="MatchNavigationBarTextLuminosity">
            <x:Arguments>
                <ContentPage>
                    <Label Text="Slide the master page to see the status bar text color mode change." />
                </ContentPage>
            </x:Arguments>
        </NavigationPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>

```

Кроме того его можно будет использовать с помощью C# с помощью текучего API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

IsPresentedChanged += (sender, e) =>
{
    var mdp = sender as MasterDetailPage;
    if (mdp.IsPresented)
        ((Xamarin.Forms.NavigationPage)mdp.Detail)
          .On<iOS>()
          .SetStatusBarTextColorMode(StatusBarTextColorMode.DoNotAdjust);
    else
        ((Xamarin.Forms.NavigationPage)mdp.Detail)
          .On<iOS>()
          .SetStatusBarTextColorMode(StatusBarTextColorMode.MatchNavigationBarTextLuminosity);
};
```

`NavigationPage.On<iOS>` Метод указывает, что этой платформы будет выполняться только на устройствах iOS. [ `NavigationPage.SetStatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetStatusBarTextColorMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage},Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode)) Метод в [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространства имен, элементы управления цветом ли текст строки состояния на [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) настраивается в соответствии с яркость панели навигации с [ `StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) перечисления, предоставляя два возможных значения:

- [`DoNotAdjust`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust) — Указывает, что состояние цвет текста столбца не должно изменяться.
- [`MatchNavigationBarTextLuminosity`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity) — Указывает, что строки цвет текста состояния должен искать яркость панели навигации.

Кроме того [ `GetStatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.GetStatusBarTextColorMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage})) метод может использоваться для получения текущего значения [ `StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) перечисления, который применяется к [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage).

Результатом является, строке цвета текста и состояния [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) можно изменить в соответствии с яркость панели навигации. В этом примере состояние изменения цвета текста строки как пользователь переключается между [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) и [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) страницы [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage):

![](status-bar-text-color-images/status-bar-text-color-mode.png "Строка состояния текста цветов режим платформы")

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
