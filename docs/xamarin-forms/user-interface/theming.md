---
title: Их применение в приложении Xamarin. Forms
description: Их можно реализовать в приложениях Xamarin. Forms, создав ResourceDictionary для каждой темы, а затем загрузив ресурсы с расширением разметки DynamicResource.
ms.prod: xamarin
ms.assetId: B7B17F66-4E37-4B50-9A57-351B62BE4FED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2019
ms.openlocfilehash: 3e0f508a9c980c02681f1be581846f9f2f25e2d0
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "69529288"
---
# <a name="theming-a-xamarinforms-application"></a>Их применение в приложении Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)

Приложения Xamarin. Forms могут динамически реагировать на изменения стиля во время выполнения с помощью расширения разметки `DynamicResource`. Это расширение разметки аналогично расширению разметки `StaticResource`, в котором оба используют ключ словаря для выборки значения из [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary). Однако, хотя расширение разметки `StaticResource` выполняет поиск по одному словарю, расширение разметки `DynamicResource` сохраняет ссылку на ключ словаря. Таким образом, если значение, связанное с ключом, заменяется, это изменение применяется к [`VisualElement`](xref:Xamarin.Forms.VisualElement). Это позволяет реализовать среду выполнения в приложениях Xamarin. Forms.

Процесс реализации среды выполнения в приложении Xamarin. Forms выглядит следующим образом:

1. Определите ресурсы для каждой темы в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary).
1. Использовать ресурсы темы в приложении с помощью расширения разметки `DynamicResource`.
1. Задайте тему по умолчанию в файле **app. XAML** приложения.
1. Добавьте код для загрузки темы во время выполнения.

На следующих снимках экрана показаны страницы с темами и приложение iOS с использованием светлой темы и приложения Android с использованием темной темы:

[![Снимок экрана: Главная страница приложения с темой, на снимке экрана iOS и android](theming-images/main-page-both-themes.png "Главная страница приложения с темой")](theming-images/main-page-both-themes-large.png#lightbox "Главная страница приложения с темой")
на [ ![странице сведений в приложении с темой, в iOS и Android](theming-images/detail-page-both-themes.png "Страница сведений о приложении с темой")](theming-images/detail-page-both-themes-large.png#lightbox "Страница сведений о приложении с темой")

## <a name="define-themes"></a>Определение тем

Тема определяется как коллекция объектов ресурсов, хранящихся в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary).

В следующем примере показаны `LightTheme` из примера приложения:

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                    x:Class="ThemingDemo.LightTheme">
    <Color x:Key="PageBackgroundColor">White</Color>
    <Color x:Key="NavigationBarColor">WhiteSmoke</Color>
    <Color x:Key="PrimaryColor">WhiteSmoke</Color>
    <Color x:Key="SecondaryColor">Black</Color>
    <Color x:Key="PrimaryTextColor">Black</Color>
    <Color x:Key="SecondaryTextColor">White</Color>
    <Color x:Key="TertiaryTextColor">Gray</Color>
    <Color x:Key="TransparentColor">Transparent</Color>
</ResourceDictionary>
```

В следующем примере показаны `DarkTheme` из примера приложения:

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                    x:Class="ThemingDemo.DarkTheme">
    <Color x:Key="PageBackgroundColor">Black</Color>
    <Color x:Key="NavigationBarColor">Teal</Color>
    <Color x:Key="PrimaryColor">Teal</Color>
    <Color x:Key="SecondaryColor">White</Color>
    <Color x:Key="PrimaryTextColor">White</Color>
    <Color x:Key="SecondaryTextColor">White</Color>
    <Color x:Key="TertiaryTextColor">WhiteSmoke</Color>
    <Color x:Key="TransparentColor">Transparent</Color>
</ResourceDictionary>
```

Каждый [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) содержит ресурсы [`Color`](xref:Xamarin.Forms.Color) , которые определяют их соответствующие темы, при этом каждый `ResourceDictionary` использует идентичные значения ключа. Дополнительные сведения о словарях ресурсов, см. в разделе [словари ресурсов](~/xamarin-forms/xaml/resource-dictionaries.md).

> [!IMPORTANT]
> Для каждого `ResourceDictionary`требуется файл кода программной части, который вызывает метод `InitializeComponent`. Это необходимо, чтобы объект CLR, представляющий выбранную тему, можно было создать во время выполнения.

## <a name="set-a-default-theme"></a>Задать тему по умолчанию

Приложению требуется тема по умолчанию, чтобы элементы управления имели значения для используемых ресурсов. Тему по умолчанию можно задать, объединив [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) темы с `ResourceDictionary` уровня приложения, который определен в **app. XAML**:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ThemingDemo.App">
    <Application.Resources>
        <ResourceDictionary Source="Themes/LightTheme.xaml" />
    </Application.Resources>
</Application>
```

Дополнительные сведения о слиянии словарей ресурсов см. в разделе [Объединенные словари ресурсов](~/xamarin-forms/xaml/resource-dictionaries.md#merged-resource-dictionaries).

## <a name="consume-theme-resources"></a>Использование ресурсов темы

Когда приложению требуется использовать ресурс, хранящийся в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) , который представляет тему, он должен сделать это с помощью расширения разметки `DynamicResource`. Это гарантирует, что если во время выполнения выбрана другая тема, будут применены значения из новой темы.

В следующем примере показаны три стиля из примера приложения, которые можно применить к [`Label`](xref:Xamarin.Forms.Label) объектам.

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ThemingDemo.App">
    <Application.Resources>

        <Style x:Key="LargeLabelStyle"
               TargetType="Label">
            <Setter Property="TextColor"
                    Value="{DynamicResource SecondaryTextColor}" />
            <Setter Property="FontSize"
                    Value="30" />
        </Style>

        <Style x:Key="MediumLabelStyle"
               TargetType="Label">
            <Setter Property="TextColor"
                    Value="{DynamicResource PrimaryTextColor}" />
            <Setter Property="FontSize"
                    Value="25" />
        </Style>

        <Style x:Key="SmallLabelStyle"
               TargetType="Label">
            <Setter Property="TextColor"
                    Value="{DynamicResource TertiaryTextColor}" />
            <Setter Property="FontSize"
                    Value="15" />
        </Style>

    </Application.Resources>
</Application>
```

Эти стили определяются в словаре ресурсов уровня приложения, чтобы их можно было использовать на нескольких страницах. Каждый стиль использует ресурсы темы с расширением разметки `DynamicResource`.

Затем эти стили используются страницами:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ThemingDemo"
             x:Class="ThemingDemo.UserSummaryPage"
             Title="User Summary"
             BackgroundColor="{DynamicResource PageBackgroundColor}">
    ...
    <ScrollView>
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="200" />
                <RowDefinition Height="120" />
                <RowDefinition Height="70" />
            </Grid.RowDefinitions>
            <Grid BackgroundColor="{DynamicResource PrimaryColor}">
                <Label Text="Face-Palm Monkey"
                       VerticalOptions="Center"
                       Margin="15"
                       Style="{StaticResource MediumLabelStyle}" />
                ...
            </Grid>
            <StackLayout Grid.Row="1"
                         Margin="10">
                <Label Text="This monkey reacts appropriately to ridiculous assertions and actions."
                       Style="{StaticResource SmallLabelStyle}" />
                <Label Text="  &#x2022; Cynical but not unfriendly."
                       Style="{StaticResource SmallLabelStyle}" />
                <Label Text="  &#x2022; Seven varieties of grimaces."
                       Style="{StaticResource SmallLabelStyle}" />
                <Label Text="  &#x2022; Doesn't laugh at your jokes."
                       Style="{StaticResource SmallLabelStyle}" />
            </StackLayout>
            ...
        </Grid>
    </ScrollView>
</ContentPage>
```

Когда ресурс темы используется напрямую, его следует использовать с расширением разметки `DynamicResource`. Однако при использовании стиля, использующего расширение разметки `DynamicResource`, его следует использовать с расширением разметки `StaticResource`.

Дополнительные сведения о стилях см. в разделе [стилизация приложений Xamarin. Forms с помощью стилей XAML](~/xamarin-forms/user-interface/styles/xaml/index.md). Дополнительные сведения о расширении разметки `DynamicResource` см. [в разделе динамические стили в Xamarin. Forms](~/xamarin-forms/user-interface/styles/xaml/dynamic.md).

## <a name="load-a-theme-at-runtime"></a>Загрузка темы во время выполнения

При выборе темы во время выполнения приложение должно:

1. Удалить текущую тему из приложения. Это достигается путем очистки свойства [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)уровня приложения.
2. Загрузить выбранную тему. Это достигается путем добавления экземпляра выбранной темы к свойству `MergedDictionaries` `ResourceDictionary`уровня приложения.

Все [`VisualElement`](xref:Xamarin.Forms.VisualElement) объекты, которые задают свойства с расширением разметки `DynamicResource`, будут применять новые значения темы. Это происходит потому, что расширение разметки `DynamicResource` поддерживает ссылку на ключи словаря. Поэтому при замене значений, связанных с ключами, изменения применяются к `VisualElement`ным объектам.

В примере приложения тема выбирается с помощью модальной страницы, содержащей [`Picker`](xref:Xamarin.Forms.Picker). В следующем коде показан метод `OnPickerSelectionChanged`, который выполняется при изменении выбранной темы:

```csharp
void OnPickerSelectionChanged(object sender, EventArgs e)
{
    Picker picker = sender as Picker;
    Theme theme = (Theme)picker.SelectedItem;

    ICollection<ResourceDictionary> mergedDictionaries = Application.Current.Resources.MergedDictionaries;
    if (mergedDictionaries != null)
    {
        mergedDictionaries.Clear();

        switch (theme)
        {
            case Theme.Dark:
                mergedDictionaries.Add(new DarkTheme());
                break;
            case Theme.Light:
            default:
                mergedDictionaries.Add(new LightTheme());
                break;
        }
    }
}
```

## <a name="related-links"></a>Связанные ссылки

- [Их (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)
- [Словари ресурсов](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Динамические стили в Xamarin. Forms](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [Задание стиля приложений Xamarin.Forms с помощью стилей XAML](~/xamarin-forms/user-interface/styles/xaml/index.md)
