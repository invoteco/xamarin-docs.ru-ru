---
title: Словари ресурсов Xamarin.Forms
description: Ресурсы Xamarin.Forms XAML являются объектами, которые могут быть общими и повторно использованы в приложении Xamarin.Forms.
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2020
ms.custom: video
ms.openlocfilehash: 8dd3c7f36ddd436a812927816a1326dbb7c48341
ms.sourcegitcommit: ee9e48e2ec643915f42a6c1641077970ae20cb17
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/01/2020
ms.locfileid: "80523212"
---
# <a name="xamarinforms-resource-dictionaries"></a>Словари ресурсов Xamarin.Forms

[![Скачать](~/media/shared/download.png) образец Скачать образец](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)

A [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) — это хранилище ресурсов, используемых приложением Xamarin.Forms. Типичные ресурсы, `ResourceDictionary` которые хранятся в [включают стили,](~/xamarin-forms/user-interface/styles/index.md) [шаблоны управления,](~/xamarin-forms/app-fundamentals/templates/control-template.md) [шаблоны данных,](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)цвета и преобразователи.

В XAML ресурсы, которые [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) хранятся в а, могут `StaticResource` быть `DynamicResource` отосланы и применены к элементам с помощью расширения или разметки. В C, ресурсы также могут `ResourceDictionary` быть определены в, а затем ссылки и применяться к элементам с помощью строки на основе индекса. Тем не менее, использование `ResourceDictionary` c-, так как общие объекты могут храниться в виде полей или свойств, и получить доступ непосредственно без необходимости сначала извлекать их из словаря.

## <a name="create-resources-in-xaml"></a>Создание ресурсов в XAML

Каждый полученный [`VisualElement`](xref:Xamarin.Forms.VisualElement) [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) объект имеет свойство, которое может [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) содержать ресурсы. Аналогичным образом, полученный [`Application`](xref:Xamarin.Forms.Application) [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) объект имеет свойство, которое может [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) содержать ресурсы.

Приложение Xamarin.Forms содержит только класс, [`Application`](xref:Xamarin.Forms.Application)который вытекает из , но [`VisualElement`](xref:Xamarin.Forms.VisualElement)часто использует многие классы, которые вытекают из , включая страницы, макеты и элементы управления. Любой из этих `Resources` объектов может [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) иметь свойство, установленное на содержащие ресурсы. Выбор места для `ResourceDictionary` конкретного воздействия, в котором можно использовать ресурсы:

- Ресурсы `ResourceDictionary` в представлении, которое [`Button`](xref:Xamarin.Forms.Button) [`Label`](xref:Xamarin.Forms.Label) прикрепляется к представлению, например или могут быть применены только к данному объекту.
- Ресурсы `ResourceDictionary` в прикрепленном макете, [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`Grid`](xref:Xamarin.Forms.Grid) например, или могут быть применены к макету и всем детям этой планировки.
- Ресурсы `ResourceDictionary` в определенном уровне страницы могут быть применены к странице и ко всем ее детям.
- Ресурсы `ResourceDictionary` в определенном на уровне приложения могут быть применены на протяжении всего приложения.

За исключением неявных стилей, каждый ресурс в словаре ресурсов `x:Key` должен иметь уникальный ключ строки, определяемый с атрибутом.

Следующие XAML показывает ресурсы, `ResourceDictionary` определенные в уровне приложения в файле **App.xaml:**

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ResourceDictionaryDemo.App">
    <Application.Resources>

        <Thickness x:Key="PageMargin">20</Thickness>

        <!-- Colors -->
        <Color x:Key="AppBackgroundColor">AliceBlue</Color>
        <Color x:Key="NavigationBarColor">#1976D2</Color>
        <Color x:Key="NavigationBarTextColor">White</Color>
        <Color x:Key="NormalTextColor">Black</Color>

        <!-- Implicit styles -->
        <Style TargetType="{x:Type NavigationPage}">
            <Setter Property="BarBackgroundColor"
                    Value="{StaticResource NavigationBarColor}" />
            <Setter Property="BarTextColor"
                    Value="{StaticResource NavigationBarTextColor}" />
        </Style>

        <Style TargetType="{x:Type ContentPage}"
               ApplyToDerivedTypes="True">
            <Setter Property="BackgroundColor"
                    Value="{StaticResource AppBackgroundColor}" />
        </Style>

    </Application.Resources>
</Application>
```

В этом примере словарь ресурсов [`Thickness`](xref:Xamarin.Forms.Thickness) определяет [`Color`](xref:Xamarin.Forms.Color) ресурс, несколько [`Style`](xref:Xamarin.Forms.Style) ресурсов и два неявных ресурса. Для получения дополнительной `App` информации о классе, см [Xamarin.Forms App класса](~/xamarin-forms/app-fundamentals/application-class.md).

> [!NOTE]
> Также допустимо размещать все ресурсы `ResourceDictionary` между явными тегами. Однако, поскольку Xamarin.Forms `ResourceDictionary` 3.0 теги не требуются. Вместо этого `ResourceDictionary` объект создается автоматически, и вы можете `Resources` вставить ресурсы непосредственно между тегами элемента свойства.

## <a name="consume-resources-in-xaml"></a>Потребляйте ресурсы в XAML

Каждый ресурс имеет ключ, `x:Key` который указан с помощью атрибута, который становится его ключом словаря в `ResourceDictionary`. Ключ используется для ссылки на [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ресурс [`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension) [`DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension) из расширения или разметки.

Расширение `StaticResource` разметки аналогично `DynamicResource` расширению разметки в том смысле, что оба используют ключ словаря для ссылки на значение из словаря ресурсов. Однако, `StaticResource` в то время как расширение разметки выполняет поиск одного словаря, расширение `DynamicResource` разметки поддерживает ссылку на ключ словаря. Поэтому при замене записи словаря, связанной с ключом, изменение применяется к визуальному элементу. Это позволяет вносить изменения ресурса времени выполнения в приложение. Для получения дополнительной информации о [XAML Markup Extensions](~/xamarin-forms/xaml/markup-extensions/index.md)расширениях разметки см.

Следующий пример XAML показывает, как потреблять ресурсы, [`StackLayout`](xref:Xamarin.Forms.StackLayout)а также определяет дополнительные ресурсы в:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ResourceDictionaryDemo.HomePage"
             Title="Home Page">
    <StackLayout Margin="{StaticResource PageMargin}">
        <StackLayout.Resources>
            <!-- Implicit style -->
            <Style TargetType="Button">
                <Setter Property="FontSize" Value="Medium" />
                <Setter Property="BackgroundColor" Value="#1976D2" />
                <Setter Property="TextColor" Value="White" />
                <Setter Property="CornerRadius" Value="5" />
            </Style>
        </StackLayout.Resources>

        <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries." />
        <Button Text="Navigate"
                Clicked="OnNavigateButtonClicked" />
    </StackLayout>
</ContentPage>
```

В этом примере [`ContentPage`](xref:Xamarin.Forms.ContentPage) объект потребляет неявный стиль, определенный в словаре ресурсов уровня приложения. Объект [`StackLayout`](xref:Xamarin.Forms.StackLayout) потребляет `PageMargin` ресурс, определенный в словаре ресурса [`Button`](xref:Xamarin.Forms.Button) уровня приложения, в то [`StackLayout`](xref:Xamarin.Forms.StackLayout) время как объект потребляет неявный стиль, определенный в словаре ресурса. Результат показан на следующих снимках экрана.

[![Потребление ресурсныхсловарных ресурсов](resource-dictionaries-images/consuming.png "Потребление ресурсов ResourceDictionary")](resource-dictionaries-images/consuming-large.png#lightbox "Потребление ресурсов ResourceDictionary")

> [!IMPORTANT]
> Ресурсы, специфичные для одной страницы, не должны быть включены в словарь ресурсов уровня приложения, поскольку такие ресурсы будут разобираться при запуске приложения, а не при необходимости страницы. Для получения дополнительной [информации см.](~/xamarin-forms/deploy-test/performance.md)

## <a name="resource-lookup-behavior"></a>Поведение поиска ресурсов

Следующий процесс поиска происходит, когда ресурс [`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension) ссылается с расширением или [`DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension) разметкой:

- Запрашиваемый ключ проверяется в словаре ресурса, если он существует, для элемента, который устанавливает свойство. Если запрашиваемый ключ найден, его значение возвращается и процесс поиска завершается.
- Если совпадение не найдено, процесс поиска выполняет поиск визуального дерева вверх, проверяя словарь ресурсов каждого родительского элемента. Если запрашиваемый ключ найден, его значение возвращается и процесс поиска завершается. В противном случае процесс продолжается вверх до тех пор, пока не будет достигнут корневой элемент.
- Если в корневом элементе не найдено совпадение, исследуется словарь ресурсов уровня приложения.
- Если совпадение до сих `XamlParseException` пор не найдено, a брошен.

Поэтому, когда parser XAML `StaticResource` сталкивается с расширением или `DynamicResource` разметкой, он ищет соответствующий ключ, путешествуя вверх по визуальному дереву, используя первый матч, который он находит. Если этот поиск заканчивается на странице и ключ до сих пор не `ResourceDictionary` найден, parser XAML выполняет поиск при соединенного `App` объекта. Если ключ все еще не найден, выбрасывается исключение.

## <a name="override-resources"></a>Переопределение ресурсов

Когда ресурсы разделяют ключи, ресурсы, определяемые ниже в визуальном дереве, будут иметь приоритет над теми, которые определены выше. Например, `AppBackgroundColor` настройка `AliceBlue` ресурса на уровне приложения будет перекрыта набором ресурсов уровня `AppBackgroundColor` страницы. `Teal` Аналогичным образом, `AppBackgroundColor` ресурс уровня страницы будет `AppBackgroundColor` переопределен ресурсом уровня управления.

## <a name="stand-alone-resource-dictionaries"></a>Словари автономных ресурсов

Класс, полученный [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) из нее, также может находиться в отдельном отдельном файле. Полученный файл может быть распространен среди приложений.

Чтобы создать такой файл, добавьте в проект новый элемент **Content View** или **Content Page** (но не **Страницу содержимого** или **Страницу содержимого** только с файлом C'). Удалите файл с кодом, а в файле XAML [`ContentView`](xref:Xamarin.Forms.ContentView) [`ContentPage`](xref:Xamarin.Forms.ContentPage) измените название базового класса с или до [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary). Кроме того, `x:Class` удалите атрибут из корневого тега файла.

На следующем примере XAML показан [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) **myResourceDictionary.xaml:**

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml">
    <DataTemplate x:Key="PersonDataTemplate">
        <ViewCell>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.5*" />
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.3*" />
                </Grid.ColumnDefinitions>
                <Label Text="{Binding Name}"
                       TextColor="{StaticResource NormalTextColor}"
                       FontAttributes="Bold" />
                <Label Grid.Column="1"
                       Text="{Binding Age}"
                       TextColor="{StaticResource NormalTextColor}" />
                <Label Grid.Column="2"
                       Text="{Binding Location}"
                       TextColor="{StaticResource NormalTextColor}"
                       HorizontalTextAlignment="End" />
            </Grid>
        </ViewCell>
    </DataTemplate>
</ResourceDictionary>
```

В этом примере содержится [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) один ресурс, который [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)является объектом типа. **MyResourceDictionary.xaml** можно использовать путем слияния его в другой словарь ресурсов.

## <a name="merged-resource-dictionaries"></a>Объединенные словари ресурсов

Слияние словарей ресурсов объединяет [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) один `ResourceDictionary`или несколько объектов в другой.

### <a name="merge-local-resource-dictionaries"></a>Слияние словарей местных ресурсов

Локальный [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) файл можно объединить `ResourceDictionary` в другой, создав `ResourceDictionary` объект, свойство которого [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) устанавливается в файловое имя файла XAML с ресурсами:

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

Этот синтаксис не мгновенно `MyResourceDictionary` класс. Вместо этого он ссылается на файл XAML. По этой причине при [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) настройке свойства файл с кодом не `x:Class` требуется, и атрибут может быть удален из корневого тега файла **MyResourceDictionary.xaml.**

> [!IMPORTANT]
> Свойство [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) может быть установлено только из XAML.

### <a name="merge-resource-dictionaries-from-other-assemblies"></a>Слияние словарей ресурсов из других собраний

A [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) также может быть `ResourceDictionary` объединен в другой, добавив `ResourceDictionary`его в свойство [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) . Этот метод позволяет объединять словари ресурсов, независимо от сборки, в которой они находятся. Слияние словарей ресурсов из внешних сборок требует [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) наличия набора действий сборки для **EmbeddedResource,** иметь `x:Class` файл с кодом и определить атрибут в корневом теге файла.

> [!WARNING]
> Класс [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) также определяет [`MergedWith`](xref:Xamarin.Forms.ResourceDictionary.MergedWith) свойство. Однако это свойство было обесточено и больше не должно использоваться.

Следующий пример кода показывает два словаря [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) ресурсов, добавленных в коллекцию уровня [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)страницы:

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

В этом примере словарь ресурсов из той же сборки и словарь ресурсов из внешней сборки объединяются в словарь ресурсов уровня страницы. Кроме того, можно также `ResourceDictionary` добавить [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) другие объекты в теги элемента свойств и другие ресурсы за пределами этих тегов.

> [!IMPORTANT]
> Там может быть `MergedDictionaries` только один тег [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)элемента свойства в, но вы можете поместить столько `ResourceDictionary` объектов там, как это требуется.

Когда объединенные [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ресурсы имеют одинаковые `x:Key` значения атрибутов, Xamarin.Forms использует следующий приоритет ресурса:

1. Ресурсы локальные в словаре ресурса.
1. Ресурсы, содержащиеся в словарях ресурсов, которые были объединены через `MergedDictionaries` `MergedDictionaries` коллекцию, в обратном порядке они перечислены в свойстве.

> [!NOTE]
> Поиск словарей ресурсов может быть вычислительно-интенсивной задачей, если приложение содержит несколько больших словарей ресурсов. Поэтому, чтобы избежать ненужного поиска, следует убедиться, что каждая страница в приложении использует только словари ресурсов, подходящие для страницы.

## <a name="related-links"></a>Связанные ссылки

- [Словари ресурсов (образец)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)
- [Расширения разметки XAML](~/xamarin-forms/xaml/markup-extensions/index.md)
- [Стили Xamarin.Forms](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Application-Resources/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
