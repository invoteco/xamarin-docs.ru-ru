---
title: Стилизация кроссплатформенного приложения Xamarin.Forms
description: В этой статье объясняется, как можно застильировать кросс-платформенный приложение Xamarin. Forms с использованием стилей XAML.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: CCCF8E57-D021-4542-8709-5808570FC26A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/02/2019
ms.openlocfilehash: 688b0e87bb6281923d3099c0d269b1c2554b6c7a
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "70756761"
---
# <a name="style-a-cross-platform-xamarinforms-application"></a>Стиль для кросс-платформенного приложения Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)

В этом кратком руководстве вы узнаете, как выполнять следующие задачи:

- Стиль приложения Xamarin. Forms с использованием стилей XAML.

В кратком руководстве описывается, как можно присвоить различные типы приложений Xamarin. Forms в стиле XAML. Ниже показано итоговое приложение:

[![](styling-images/screenshots1-sml.png "Notes Page")](styling-images/screenshots1.png#lightbox "Notes Page")
[![](styling-images/screenshots2-sml.png "Note Entry Page")](styling-images/screenshots2.png#lightbox "Note Entry Page")

### <a name="prerequisites"></a>Необходимые компоненты

Перед выполнением этого краткого руководства необходимо успешно выполнить [предыдущее краткое руководство](database.md) . Также можно скачать [предыдущий пример краткого руководства](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/) и использовать его в качестве отправной точки для этого краткого руководства.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Обновление приложения с помощью Visual Studio

1. Запустите Visual Studio и откройте решение Notes.

2. В **Обозреватель решений**в проекте **Notes** дважды щелкните **app. XAML** , чтобы открыть его. Затем замените существующий код следующим кодом:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">
        <Application.Resources>

            <Thickness x:Key="PageMargin">20</Thickness>

            <!-- Colors -->
            <Color x:Key="AppBackgroundColor">WhiteSmoke</Color>
            <Color x:Key="iOSNavigationBarColor">WhiteSmoke</Color>
            <Color x:Key="AndroidNavigationBarColor">#2196F3</Color>
            <Color x:Key="iOSNavigationBarTextColor">Black</Color>
            <Color x:Key="AndroidNavigationBarTextColor">White</Color>

            <!-- Implicit styles -->
            <Style TargetType="{x:Type NavigationPage}">
                <Setter Property="BarBackgroundColor"
                        Value="{OnPlatform iOS={StaticResource iOSNavigationBarColor},
                                           Android={StaticResource AndroidNavigationBarColor}}" />
                 <Setter Property="BarTextColor"
                        Value="{OnPlatform iOS={StaticResource iOSNavigationBarTextColor},
                                           Android={StaticResource AndroidNavigationBarTextColor}}" />           
            </Style>

            <Style TargetType="{x:Type ContentPage}"
                   ApplyToDerivedTypes="True">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

        </Application.Resources>
    </Application>
    ```

    Этот код определяет [`Thickness`](xref:Xamarin.Forms.Thickness) значение, ряд [`Color`ных](xref:Xamarin.Forms.Color) значений и неявные стили для [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) и [`ContentPage`](xref:Xamarin.Forms.ContentPage). Обратите внимание, что эти стили, которые находятся на [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)уровня приложения, можно использовать в приложении. Дополнительные сведения о стилизации XAML см. в разделе [стилизация](deepdive.md#styling) в [кратком руководстве по Xamarin. Forms](deepdive.md).

    Сохраните изменения в файле **app. XAML** , нажав клавиши **CTRL + S**, и закройте файл.

3. В **Обозреватель решений**в проекте **Notes** дважды щелкните **нотеспаже. XAML** , чтобы открыть его. Затем замените существующий код следующим кодом:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NotesPage"
                 Title="Notes">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type ListView}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>
        </ContentPage.Resources>

        <ContentPage.ToolbarItems>
            <ToolbarItem Text="+"
                         Clicked="OnNoteAddedClicked" />
        </ContentPage.ToolbarItems>

        <ListView x:Name="listView"
                  Margin="{StaticResource PageMargin}"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Text}"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>

    </ContentPage>
    ```

    Этот код добавляет неявный стиль для [`ListView`](xref:Xamarin.Forms.ListView) на [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)уровня страницы и задает для свойства `ListView.Margin` значение, определенное в `ResourceDictionary` уровня приложения. Обратите внимание, что `ListView` неявный стиль был добавлен к `ResourceDictionary` на уровне страницы, так как он используется только `NotesPage`. Дополнительные сведения о стилизации XAML см. в разделе [стилизация](deepdive.md#styling) в [кратком руководстве по Xamarin. Forms](deepdive.md).

    Сохраните изменения в **нотеспаже. XAML** , нажав клавиши **CTRL + S**, и закройте файл.

4. В **Обозреватель решений**в проекте **Notes** дважды щелкните **нотинтрипаже. XAML** , чтобы открыть его. Затем замените существующий код следующим кодом:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NoteEntryPage"
                 Title="Note Entry">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type Editor}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

            <Style TargetType="Button"
                   ApplyToDerivedTypes="True"
                   CanCascade="True">
                <Setter Property="FontSize" Value="Medium" />
                <Setter Property="BackgroundColor" Value="LightGray" />
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="5" />
            </Style>
        </ContentPage.Resources>

        <StackLayout Margin="{StaticResource PageMargin}">
            <Editor Placeholder="Enter your note"
                    Text="{Binding Text}"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked" />
            </Grid>
        </StackLayout>

    </ContentPage>
    ```

    Этот код добавляет Неявные стили для [`Editor`](xref:Xamarin.Forms.Editor) и [`Button`](xref:Xamarin.Forms.Button) представлений в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)на уровне страницы и задает для свойства `StackLayout.Margin` значение, определенное в `ResourceDictionary` уровня приложения. Обратите внимание, что Неявные стили `Editor` и `Button` были добавлены в `ResourceDictionary` уровня страницы, так как они используются только `NoteEntryPage`. Дополнительные сведения о стилизации XAML см. в разделе [стилизация](deepdive.md#styling) в [кратком руководстве по Xamarin. Forms](deepdive.md).

    Сохраните изменения в **нотинтрипаже. XAML** , нажав клавиши **CTRL + S**, и закройте файл.

5. Создайте и запустите проект на каждой платформе. Дополнительные сведения см. [в разделе Создание краткого руководства](single-page.md#building-the-quickstart).

    В **нотеспаже** нажмите кнопку **+** , чтобы перейти к **нотинтрипаже** и ввести Примечание. На каждой странице Обратите внимание на то, как стили изменились в предыдущем кратком руководстве.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Обновление приложения с помощью Visual Studio для Mac

1. Запустите Visual Studio для Mac и откройте проект Notes.

2. В **панель решения**в проекте **Notes** дважды щелкните **app. XAML** , чтобы открыть его. Затем замените существующий код следующим кодом:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">
        <Application.Resources>

            <Thickness x:Key="PageMargin">20</Thickness>

            <!-- Colors -->
            <Color x:Key="AppBackgroundColor">WhiteSmoke</Color>
            <Color x:Key="iOSNavigationBarColor">WhiteSmoke</Color>
            <Color x:Key="AndroidNavigationBarColor">#2196F3</Color>
            <Color x:Key="iOSNavigationBarTextColor">Black</Color>
            <Color x:Key="AndroidNavigationBarTextColor">White</Color>

            <!-- Implicit styles -->
            <Style TargetType="{x:Type NavigationPage}">
                <Setter Property="BarBackgroundColor"
                        Value="{OnPlatform iOS={StaticResource iOSNavigationBarColor},
                                           Android={StaticResource AndroidNavigationBarColor}}" />
                 <Setter Property="BarTextColor"
                        Value="{OnPlatform iOS={StaticResource iOSNavigationBarTextColor},
                                           Android={StaticResource AndroidNavigationBarTextColor}}" />           
            </Style>

            <Style TargetType="{x:Type ContentPage}"
                   ApplyToDerivedTypes="True">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

        </Application.Resources>
    </Application>
    ```

    Этот код определяет [`Thickness`](xref:Xamarin.Forms.Thickness) значение, ряд [`Color`ных](xref:Xamarin.Forms.Color) значений и неявные стили для [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) и [`ContentPage`](xref:Xamarin.Forms.ContentPage). Обратите внимание, что эти стили, которые находятся на [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)уровня приложения, можно использовать в приложении. Дополнительные сведения о стилизации XAML см. в разделе [стилизация](deepdive.md#styling) в [кратком руководстве по Xamarin. Forms](deepdive.md).

    Сохраните изменения в файле **app. XAML** , выбрав **Файл > Сохранить** (или нажав клавиши  **&#8984; + S**) и закройте файл.

3. В **панель решения**в проекте **Notes** дважды щелкните **нотеспаже. XAML** , чтобы открыть его. Затем замените существующий код следующим кодом:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NotesPage"
                 Title="Notes">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type ListView}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>
        </ContentPage.Resources>

        <ContentPage.ToolbarItems>
            <ToolbarItem Text="+"
                         Clicked="OnNoteAddedClicked" />
        </ContentPage.ToolbarItems>

        <ListView x:Name="listView"
                  Margin="{StaticResource PageMargin}"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Text}"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>

    </ContentPage>
    ```

    Этот код добавляет неявный стиль для [`ListView`](xref:Xamarin.Forms.ListView) на [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)уровня страницы и задает для свойства `ListView.Margin` значение, определенное в `ResourceDictionary` уровня приложения. Обратите внимание, что `ListView` неявный стиль был добавлен к `ResourceDictionary` на уровне страницы, так как он используется только `NotesPage`. Дополнительные сведения о стилизации XAML см. в разделе [стилизация](deepdive.md#styling) в [кратком руководстве по Xamarin. Forms](deepdive.md).

    Сохраните изменения в **нотеспаже. XAML** , выбрав **Файл > Сохранить** (или нажав клавиши  **&#8984; + S**) и закройте файл.

4. В **панель решения**в проекте **Notes** дважды щелкните **нотинтрипаже. XAML** , чтобы открыть его. Затем замените существующий код следующим кодом:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NoteEntryPage"
                 Title="Note Entry">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type Editor}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

            <Style TargetType="Button"
                   ApplyToDerivedTypes="True"
                   CanCascade="True">
                <Setter Property="FontSize" Value="Medium" />
                <Setter Property="BackgroundColor" Value="LightGray" />
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="5" />
            </Style>
        </ContentPage.Resources>

        <StackLayout Margin="{StaticResource PageMargin}">
            <Editor Placeholder="Enter your note"
                    Text="{Binding Text}"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked" />
            </Grid>
        </StackLayout>

    </ContentPage>
    ```

    Этот код добавляет Неявные стили для [`Editor`](xref:Xamarin.Forms.Editor) и [`Button`](xref:Xamarin.Forms.Button) представлений в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)на уровне страницы и задает для свойства `StackLayout.Margin` значение, определенное в `ResourceDictionary` уровня приложения. Обратите внимание, что Неявные стили `Editor` и `Button` были добавлены в `ResourceDictionary` уровня страницы, так как они используются только `NoteEntryPage`. Дополнительные сведения о стилизации XAML см. в разделе [стилизация](deepdive.md#styling) в [кратком руководстве по Xamarin. Forms](deepdive.md).

    Сохраните изменения в **нотинтрипаже. XAML** , выбрав **Файл > Сохранить** (или нажав клавиши  **&#8984; + S**) и закройте файл.

5. Создайте и запустите проект на каждой платформе. Дополнительные сведения см. [в разделе Создание краткого руководства](single-page.md#building-the-quickstart).

    В **нотеспаже** нажмите кнопку **+** , чтобы перейти к **нотинтрипаже** и ввести Примечание. На каждой странице Обратите внимание на то, как стили изменились в предыдущем кратком руководстве.

::: zone-end

## <a name="next-steps"></a>Следующие шаги

В этом кратком руководстве вы узнали, как выполнять следующие задачи:

- Стиль приложения Xamarin. Forms с использованием стилей XAML.

Чтобы узнать больше об основах разработки приложений с помощью Xamarin. Forms, перейдите к краткому руководству.

> [!div class="nextstepaction"]
> [Вперед](deepdive.md)

## <a name="related-links"></a>Связанные ссылки

- [Заметки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)
- [Углубленное углубление в руководстве по Xamarin. Forms](deepdive.md)
