---
title: Словари ресурсов
description: Ресурсы XAML — это объекты, которые можно совместно использовать и повторно использоваться на протяжении всего приложения Xamarin.Forms.
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2020
ms.custom: video
ms.openlocfilehash: f5266049e1498d902864185a61cba6d3730f9594
ms.sourcegitcommit: 2836f2003a5b745b042ee6003a3d6a11b9139e44
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/26/2020
ms.locfileid: "77618864"
---
# <a name="resource-dictionaries"></a>Словари ресурсов

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)

_Ресурсы XAML — это определения объектов, которые можно совместно использовать и повторно применять во всем приложении Xamarin. Forms. Эти объекты ресурсов хранятся в словаре ресурсов._

[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) — это репозиторий для ресурсов, используемых приложением Xamarin. Forms. К типичным ресурсам, хранящимся в `ResourceDictionary`, относятся [стили](~/xamarin-forms/user-interface/styles/index.md), [шаблоны элементов управления](~/xamarin-forms/app-fundamentals/templates/control-template.md), [шаблоны данных](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), цвета и конвертеры.

В XAML ресурсы, которые хранятся в `ResourceDictionary`, затем можно извлечь и применить к элементам с помощью расширения разметки `StaticResource`. В C#ресурсы также могут быть определены в `ResourceDictionary`, а затем извлечены и применены к элементам с помощью индексатора на основе строк. Однако существует немало преимуществ использования `ResourceDictionary` в, так как C#общие объекты могут просто храниться в виде полей или свойств, а доступ к ним осуществляется напрямую без необходимости их извлечения из словаря.

## <a name="create-and-consume-a-resourcedictionary"></a>Создание и использование ResourceDictionary

Ресурсы определяются в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) , где затем устанавливается одно из следующих `Resources` свойств:

- Свойство [`Resources`](xref:Xamarin.Forms.Application.Resources) любого класса, производного от [`Application`](xref:Xamarin.Forms.Application)
- Свойство [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) любого класса, производного от [`VisualElement`](xref:Xamarin.Forms.Application)

Программа Xamarin. Forms содержит только один класс, который является производным от `Application` но часто использует многие классы, производные от `VisualElement`, включая страницы, макеты и элементы управления. Для любого из этих объектов свойство `Resources` может иметь значение `ResourceDictionary`. Выбор места размещения определенных `ResourceDictionary` влияет на то, где можно использовать ресурсы:

- Ресурсы в `ResourceDictionary`, присоединенном к представлению, например `Button` или `Label`, могут быть применены только к этому конкретному объекту, поэтому это не очень полезно.
- Ресурсы в `ResourceDictionary`, прикрепленные к макету, например `StackLayout` или `Grid`, можно применить к макету и всем дочерним элементам этого макета.
- Ресурсы в `ResourceDictionary`, определенные на уровне страницы, могут быть применены к странице и ко всем ее дочерним элементам.
- Ресурсы в `ResourceDictionary`, определенные на уровне приложения, можно применять ко всему приложению.

В следующем коде XAML показаны ресурсы, определенные на уровне приложения `ResourceDictionary` в файле **app. XAML** , созданном в составе стандартной программы Xamarin. Forms:

```xaml
<Application ...>
    <Application.Resources>
        <ResourceDictionary>
            <Color x:Key="PageBackgroundColor">Yellow</Color>
            <Color x:Key="HeadingTextColor">Black</Color>
            <Color x:Key="NormalTextColor">Blue</Color>
            <Style x:Key="LabelPageHeadingStyle" TargetType="Label">
                <Setter Property="FontAttributes" Value="Bold" />
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="TextColor" Value="{StaticResource HeadingTextColor}" />
            </Style>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Этот `ResourceDictionary` определяет три [`Color`](xref:Xamarin.Forms.Color) ресурсов и [`Style`](xref:Xamarin.Forms.Style) ресурс. Дополнительные сведения о классе `App` см. в разделе [класс App](~/xamarin-forms/app-fundamentals/application-class.md).

Начиная с Xamarin. Forms 3,0, явные Теги `ResourceDictionary` не являются обязательными. Объект `ResourceDictionary` создается автоматически, и можно вставить ресурсы непосредственно между тегами элемента свойства `Resources`.

```xaml
<Application ...>
    <Application.Resources>
        <Color x:Key="PageBackgroundColor">Yellow</Color>
        <Color x:Key="HeadingTextColor">Black</Color>
        <Color x:Key="NormalTextColor">Blue</Color>
        <Style x:Key="LabelPageHeadingStyle" TargetType="Label">
            <Setter Property="FontAttributes" Value="Bold" />
            <Setter Property="HorizontalOptions" Value="Center" />
            <Setter Property="TextColor" Value="{StaticResource HeadingTextColor}" />
        </Style>
    </Application.Resources>
</Application>
```

Каждый ресурс имеет ключ, который задается с помощью атрибута `x:Key`, который становится ключом словаря в `ResourceDictionary`. Ключ используется для получения ресурса из `ResourceDictionary` расширения разметки [`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension) , как показано в следующем примере кода XAML, в котором показаны дополнительные ресурсы, определенные в `StackLayout`:

```xaml
<StackLayout Margin="0,20,0,0">
  <StackLayout.Resources>
    <ResourceDictionary>
      <Style x:Key="LabelNormalStyle" TargetType="Label">
        <Setter Property="TextColor" Value="{StaticResource NormalTextColor}" />
      </Style>
      <Style x:Key="MediumBoldText" TargetType="Button">
        <Setter Property="FontSize" Value="Medium" />
        <Setter Property="FontAttributes" Value="Bold" />
      </Style>
    </ResourceDictionary>
  </StackLayout.Resources>
  <Label Text="ResourceDictionary Demo" Style="{StaticResource LabelPageHeadingStyle}" />
    <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries."
           Margin="10,20,10,0"
           Style="{StaticResource LabelNormalStyle}" />
    <Button Text="Navigate"
            Clicked="OnNavigateButtonClicked"
            TextColor="{StaticResource NormalTextColor}"
            Margin="0,20,0,0"
            HorizontalOptions="Center"
            Style="{StaticResource MediumBoldText}" />
</StackLayout>
```

Первый экземпляр [`Label`](xref:Xamarin.Forms.Label) извлекает и потребляет ресурс `LabelPageHeadingStyle`, определенный на `ResourceDictionary`уровня приложения, со вторым экземпляром `Label`, который извлекает и потребляет ресурс `LabelNormalStyle`, определенный в `ResourceDictionary`уровня элемента управления. Аналогичным образом экземпляр [`Button`](xref:Xamarin.Forms.Button) извлекает и потребляет `NormalTextColor` ресурс, определенный на уровне приложения `ResourceDictionary`, и ресурс `MediumBoldText`, определенный в `ResourceDictionary`уровня элемента управления. Результат показан на следующих снимках экрана:

[![использования ресурсов ResourceDictionary](resource-dictionaries-images/screenshots-sml.png)](resource-dictionaries-images/screenshots.png#lightbox)

> [!NOTE]
> Ресурсы, относящиеся к одной странице не должно быть включено в приложение ресурсов на уровне словарю, таким образом ресурсы будут анализироваться при запуске приложения, а не, затем при необходимости в страницу. Дополнительные сведения см. [в разделе сокращение размера словаря ресурсов приложения](~/xamarin-forms/deploy-test/performance.md).

## <a name="override-resources"></a>Переопределение ресурсов

Если ресурсы `ResourceDictionary` имеют общий доступ к `x:Key` значениям атрибутов, ресурсы, определенные ниже в иерархии представлений, имеют приоритет над теми, которые определены выше. Например, задание `PageBackgroundColor`ного ресурса для `Blue` на уровне приложения будет переопределено на уровне страницы `PageBackgroundColor` ресурсом, равным `Yellow`. Аналогичным образом `PageBackgroundColor` ресурс на уровне страницы будет переопределен `PageBackgroundColor`ным ресурсом уровня элемента управления. Этот приоритет демонстрируется в следующем примере кода XAML:

```xaml
<ContentPage ... BackgroundColor="{StaticResource PageBackgroundColor}">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Color x:Key="PageBackgroundColor">Blue</Color>
            <Color x:Key="NormalTextColor">Yellow</Color>
        </ResourceDictionary>
    </ContentPage.Resources>
    <StackLayout Margin="0,20,0,0">
        ...
        <Label Text="ResourceDictionary Demo" Style="{StaticResource LabelPageHeadingStyle}" />
        <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries."
               Margin="10,20,10,0"
               Style="{StaticResource LabelNormalStyle}" />
        <Button Text="Navigate"
                Clicked="OnNavigateButtonClicked"
                TextColor="{StaticResource NormalTextColor}"
                Margin="0,20,0,0"
                HorizontalOptions="Center"
                Style="{StaticResource MediumBoldText}" />
    </StackLayout>
</ContentPage>
```

Исходные `PageBackgroundColor` и `NormalTextColor` экземпляры, определенные на уровне приложения, переопределяются экземплярами `PageBackgroundColor` и `NormalTextColor`, определенными на уровне страницы. Таким образом, цвет фона страницы становится синий, а текст на странице желтый, как показано на следующем снимке экрана:

[Переопределение ресурсов ResourceDictionary ![](resource-dictionaries-images/overridding-screenshots-sml.png)](resource-dictionaries-images/overridding-screenshots.png#lightbox)

Однако обратите внимание, что фоновая панель [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) по-прежнему является желтой, так как для свойства [`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) задано значение ресурса `PageBackgroundColor`, определенного на `ResourceDictionary`уровня приложения.

Ниже приведен еще один способ подумать о приоритете `ResourceDictionary`. когда средство синтаксического анализа XAML встречает `StaticResource`, оно выполняет поиск совпадающего ключа, переполняя его на визуальное дерево, используя первое найденное совпадение. Если этот поиск заканчивается на странице, а ключ по-прежнему не найден, средство синтаксического анализа XAML выполняет поиск в `ResourceDictionary`, присоединенном к объекту `App`. Если ключ не найден, возникает исключение.

## <a name="stand-alone-resource-dictionaries"></a>Независимые словари ресурсов

Класс, производный от `ResourceDictionary`, также может находиться в отдельном автономном файле. (Точнее говоря, класс, производный от `ResourceDictionary`, обычно требует _пары_ файлов, так как ресурсы определены в файле XAML, но также требуется файл кода программной части с вызовом `InitializeComponent`.) Затем результирующий файл может совместно использоваться приложениями.

Чтобы создать такой файл, добавьте в проект новое **представление содержимого** или элемент **страницы содержимого** (но не **представление содержимого** или **страницу содержимого** с C# файлом). В файле и C# файле XAML измените имя базового класса с `ContentView` или `ContentPage` на `ResourceDictionary`. В файле XAML имя базового класса является элементом верхнего уровня.

В следующем примере XAML показан [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) с именем `MyResourceDictionary`:

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                    x:Class="ResourceDictionaryDemo.MyResourceDictionary">
    <DataTemplate x:Key="PersonDataTemplate">
        <ViewCell>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.5*" />
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.3*" />
                </Grid.ColumnDefinitions>
                <Label Text="{Binding Name}" TextColor="{StaticResource NormalTextColor}" FontAttributes="Bold" />
                <Label Grid.Column="1" Text="{Binding Age}" TextColor="{StaticResource NormalTextColor}" />
                <Label Grid.Column="2" Text="{Binding Location}" TextColor="{StaticResource NormalTextColor}" HorizontalTextAlignment="End" />
            </Grid>
        </ViewCell>
    </DataTemplate>
</ResourceDictionary>
```

Этот `ResourceDictionary` содержит один ресурс, который является объектом типа `DataTemplate`.

Можно создать экземпляр `MyResourceDictionary`, поместив его между парой `Resources` тегов элементов свойства, например, в `ContentPage`:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <local:MyResourceDictionary />
    </ContentPage.Resources>
    ...
</ContentPage>
```

Экземпляр `MyResourceDictionary` устанавливается в свойство `Resources` объекта `ContentPage`.

Однако этот подход имеет некоторые ограничения: свойство `Resources` `ContentPage` ссылается только на этот `ResourceDictionary`. В большинстве случаев необходимо также включить другие экземпляры `ResourceDictionary` и, возможно, другие ресурсы.

Эта задача требует объединенные словари ресурсов.

## <a name="merged-resource-dictionaries"></a>Объединенные словари ресурсов

Объединенные словари ресурсов объединяют один или несколько [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) объектов в другой `ResourceDictionary`.

### <a name="merge-local-resource-dictionaries"></a>Объединить словари локальных ресурсов

Локальный [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) можно объединить в другой `ResourceDictionary`, задав свойству [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) имя файла XAML с ресурсами:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <!-- Add more resources here -->
        <ResourceDictionary Source="MyResourceDictionary.xaml" />
        <!-- Add more resources here -->
    </ContentPage.Resources>
    ...
</ContentPage>
```

Этот синтаксис не создает экземпляр класса `MyResourceDictionary`. Вместо этого он ссылается на файл XAML. По этой причине при установке свойства [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) не требуется файл кода программной части (**MyResourceDictionary.XAML.CS**), а атрибут `x:Class` можно удалить из корневого тега файла **миресаурцедиктионари. XAML** . Кроме того, при слиянии словарей ресурсов с использованием этого подхода Xamarin. Forms автоматически создает экземпляр [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), поэтому внешние теги `ResourceDictionary` не требуются.

> [!IMPORTANT]
> Свойство [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) может быть задано только из XAML.

### <a name="merge-resource-dictionaries-from-other-assemblies"></a>Объединить словари ресурсов из других сборок

[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) также можно объединить в другой `ResourceDictionary`, добавив его в свойство [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) `ResourceDictionary`. Этот метод позволяет объединять словари ресурсов независимо от сборки, в которой они находятся.

> [!IMPORTANT]
> Класс [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) также определяет свойство [`MergedWith`](xref:Xamarin.Forms.ResourceDictionary.MergedWith) . Однако это свойство является устаревшим и больше не должно использоваться.

В следующем примере кода показано, `MyResourceDictionary` добавляется в коллекцию [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)уровня страницы:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:ResourceDictionaryDemo">
    <ContentPage.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <local:MyResourceDictionary />
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

В этом примере показан экземпляр `MyResourceDictionary`, который находится в той же сборке и добавляется в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary). Кроме того, можно добавлять словари ресурсов из других сборок, других `ResourceDictionary` объектов в [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) тегов элементов свойств и других ресурсов за пределами этих тегов:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:ResourceDictionaryDemo"
             xmlns:theme="clr-namespace:MyThemes;assembly=MyThemes">
    <ContentPage.Resources>
        <ResourceDictionary>
            <!-- Add more resources here -->
            <ResourceDictionary.MergedDictionaries>
                <!-- Add more resource dictionaries here -->
                <local:MyResourceDictionary />
                <theme:LightTheme />
                <!-- Add more resource dictionaries here -->
            </ResourceDictionary.MergedDictionaries>
            <!-- Add more resources here -->
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

При помещении [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) во внешнюю сборку убедитесь, что для ее действия сборки задано значение **EmbeddedResource**. Кроме того, убедитесь, что у него есть файл кода программной части.

> [!IMPORTANT]
> В [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)может быть только один тег элемента свойства `MergedDictionaries`, но в нем можно поместить столько `ResourceDictionary` объектов, сколько нужно.

Если объединенные [`ResourceDictionary`ные](xref:Xamarin.Forms.ResourceDictionary) ресурсы имеют одинаковое `x:Key` значений атрибутов, Xamarin. Forms использует следующий приоритет ресурсов:

1. Ресурсы локального в словарь ресурсов.
1. Ресурсы, содержащиеся в словарях ресурсов, Объединенных с помощью коллекции `MergedDictionaries`, в порядке, в котором они перечислены в свойстве `MergedDictionaries`.

> [!NOTE]
> Поиск словари ресурсов может оказаться задачей с большим объемом вычислений, если приложение содержит несколько словарей больших ресурсов. Таким образом Чтобы избежать ненужного поиска, должны убедитесь, что каждая страница в приложение только использует словари ресурсов, которые подходят для страницы.

## <a name="related-links"></a>Связанные ссылки

- [Словари ресурсов (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)
- [Стили](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Application-Resources/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
