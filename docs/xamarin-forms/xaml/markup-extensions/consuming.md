---
title: Использование расширений разметки XAML
description: В этой статье объясняется, как использовать расширения разметки XAML Xamarin. Forms для расширения возможностей и гибкости XAML, разрешая установку атрибутов элементов из различных источников.
ms.prod: xamarin
ms.assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/27/2019
ms.openlocfilehash: a8698975d2609599e1404fbb9c87c617a54f23d7
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696344"
---
# <a name="consuming-xaml-markup-extensions"></a>Использование расширений разметки XAML

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)

Расширения разметки XAML помогают повысить степень гибкости и гибкость XAML, разрешая установку атрибутов элементов из различных источников. Несколько расширений разметки XAML являются частью спецификации XAML 2009. Они отображаются в файлах XAML с префиксом пространства имен Custom `x` и обычно называются этим префиксом. В этой статье рассматриваются следующие расширения разметки:

- [`x:Static`](#static) — ссылки на статические свойства, поля или члены перечисления.
- [`x:Reference`](#reference) — ссылки на именованные элементы на странице.
- [`x:Type`](#type) — задание атрибута для `System.Type` объекта.
- [`x:Array`](#array) — создание массива объектов определенного типа.
- [`x:Null`](#null) — задайте для атрибута значение `null`.
- [`OnPlatform`](#onplatform) — Настройка внешнего вида пользовательского интерфейса для отдельных платформ.
- [`OnIdiom`](#onidiom) — Настройка внешнего вида пользовательского интерфейса на основе идиомы устройства, на котором работает приложение.
- [`DataTemplate`](#datatemplate-markup-extension) — преобразует тип в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate).
- [`FontImage`](#fontimage-markup-extension) — отображать значок шрифта в любом представлении, которое может отображать `ImageSource`.

Дополнительные расширения разметки XAML исторически поддерживаются другими реализациями XAML и также поддерживаются Xamarin. Forms. Они описаны более полно в других статьях:

- `StaticResource`-ссылки на объекты из словаря ресурсов, как описано в статье [**словари ресурсов**](~/xamarin-forms/xaml/resource-dictionaries.md).
- `DynamicResource` — реагирование на изменения объектов в словаре ресурсов, как описано в статье [**динамические стили**](~/xamarin-forms/user-interface/styles/dynamic.md).
- `Binding` — установите связь между свойствами двух объектов, как описано в статье [**Привязка данных**](~/xamarin-forms/app-fundamentals/data-binding/index.md).
- `TemplateBinding` — выполняет привязку данных из шаблона элемента управления, как описано в статье [**Привязка к шаблону элемента управления**](~/xamarin-forms/app-fundamentals/templates/control-templates/template-binding.md).
- `RelativeSource` — задает источник привязки относительно положения целевого объекта привязки, как описано в статье [относительные привязки](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md).

В [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) разметке используются пользовательские расширения разметки [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression). Это расширение разметки описано в статье [**RelativeLayout**](~/xamarin-forms/user-interface/layouts/relative-layout.md).

<a name="static" />

## <a name="xstatic-markup-extension"></a>расширение разметки x:Static

Расширение разметки `x:Static` поддерживается классом [`StaticExtension`](xref:Xamarin.Forms.Xaml.StaticExtension) . Класс содержит одно свойство с именем [`Member`](xref:Xamarin.Forms.Xaml.StaticExtension.Member) типа `string`, для которого задано имя общей константы, статического свойства, статического поля или члена перечисления.

Один из распространенных способов использования `x:Static` — сначала определить класс с некоторыми константами или статическими переменными, такими как этот небольшой `AppConstants` класс в программе [**расширений MarkupExtension**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions) :

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

**Демонстрационная Страница x:Static** демонстрирует несколько способов использования расширения разметки `x:Static`. Самый подробный подход создает экземпляр класса `StaticExtension` между `Label.FontSize` тегами элементов свойства.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.StaticDemoPage"
             Title="x:Static Demo">
    <StackLayout Margin="10, 0">
        <Label Text="Label No. 1">
            <Label.FontSize>
                <x:StaticExtension Member="local:AppConstants.NormalFontSize" />
            </Label.FontSize>
        </Label>

        ···

    </StackLayout>
</ContentPage>
```

Средство синтаксического анализа XAML также позволяет сократить класс `StaticExtension` как `x:Static`:

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

Это можно сделать еще более простым, но в результате изменения появился новый синтаксис: он состоит в размещении `StaticExtension` класса и параметра элемента в фигурных скобках. Результирующее выражение задается непосредственно в атрибуте `FontSize`:

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

Обратите внимание, что в фигурных скобках отсутствуют кавычки. Свойство `Member` `StaticExtension` больше не является XML-атрибутом. Вместо этого он является частью выражения для расширения разметки.

Так же, как можно сократить `x:StaticExtension` `x:Static` при использовании его в качестве объектного элемента, можно также сократить его в выражении в фигурных скобках:

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

Класс `StaticExtension` имеет атрибут `ContentProperty`, ссылающийся на свойство `Member`, которое помечает это свойство как свойство содержимого класса по умолчанию. Для расширений разметки XAML, выраженных фигурными скобками, можно исключить `Member=` часть выражения:

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

Это наиболее распространенная форма расширения разметки `x:Static`.

**Статическая демонстрационная** страница содержит два других примера. Корневой тег XAML-файла содержит объявление пространства имен XML для пространства имен .NET `System`.

```xaml
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Это позволяет задать для `Label` размера шрифта статическое поле `Math.PI`. Это приводит к небольшому тексту, поэтому свойство `Scale` имеет значение `Math.E`.

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

В последнем примере отображается значение `Device.RuntimePlatform`. Свойство `Environment.NewLine` static используется для вставки символа новой строки между двумя `Span`ными объектами:

```xaml
<Label HorizontalTextAlignment="Center"
       FontSize="{x:Static local:AppConstants.NormalFontSize}">
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Runtime Platform: " />
            <Span Text="{x:Static sys:Environment.NewLine}" />
            <Span Text="{x:Static Device.RuntimePlatform}" />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

Вот пример, в котором работает:

[![Демонстрация в виде x:Static](consuming-images/staticdemo-small.png "Демонстрация в виде x:Static")](consuming-images/staticdemo-large.png#lightbox "Демонстрация в виде x:Static")

<a name="reference" />

## <a name="xreference-markup-extension"></a>расширение разметки x:Reference

Расширение разметки `x:Reference` поддерживается классом [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension) . Класс имеет одно свойство с именем [`Name`](xref:Xamarin.Forms.Xaml.ReferenceExtension.Name) типа `string`, которому задается имя элемента на странице, которому присвоено имя `x:Name`. Это `Name` свойство является свойством Content `ReferenceExtension`, поэтому `Name=` не требуется, если `x:Reference` отображается в фигурных скобках.

Расширение разметки `x:Reference` используется исключительно с привязками данных, которые более подробно описаны в статье [**Привязка данных**](~/xamarin-forms/app-fundamentals/data-binding/index.md).

На **демонстрационной странице x:Reference** показаны два способа использования `x:Reference` с привязками данных, первый из которых используется для задания свойства `Source` объекта `Binding`, а второй — для задания свойства `BindingContext` для двух привязок данных. :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.ReferenceDemoPage"
             x:Name="page"
             Title="x:Reference Demo">

    <StackLayout Margin="10, 0">

        <Label Text="{Binding Source={x:Reference page},
                              StringFormat='The type of this page is {0}'}"
               FontSize="18"
               VerticalOptions="CenterAndExpand"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="Center" />

        <Label BindingContext="{x:Reference slider}"
               Text="{Binding Value, StringFormat='{0:F0}&#x00B0; rotation'}"
               Rotation="{Binding Value}"
               FontSize="24"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

    </StackLayout>
</ContentPage>
```

Оба выражения `x:Reference` используют сокращенную версию `ReferenceExtension` имени класса и удаляют `Name=` часть выражения. В первом примере расширение разметки `x:Reference` внедряется в расширение разметки `Binding`. Обратите внимание, что параметры `Source` и `StringFormat` разделены запятыми. Вот работающая программа:

[![Демонстрация x:Reference](consuming-images/referencedemo-small.png "Демонстрация x:Reference")](consuming-images/referencedemo-large.png#lightbox "Демонстрация x:Reference")

<a name="type" />

## <a name="xtype-markup-extension"></a>x:Type - расширение разметки

@No__t_0 расширением разметки является XAML-эквивалентом C# ключевого слова [`typeof`](/dotnet/csharp/language-reference/keywords/typeof/) . Он поддерживается классом [`TypeExtension`](xref:Xamarin.Forms.Xaml.TypeExtension) , который определяет одно свойство с именем [`TypeName`](xref:Xamarin.Forms.Xaml.TypeExtension.TypeName) типа `string`, для которого задано имя класса или структуры. Расширение разметки `x:Type` возвращает объект [`System.Type`](xref:System.Type) этого класса или структуры. `TypeName` является свойством содержимого `TypeExtension`, поэтому `TypeName=` не требуется, если `x:Type` отображается с фигурными скобками.

В Xamarin. Forms существует несколько свойств, которые имеют аргументы типа `Type`. Примеры включают свойство [`TargetType`](xref:Xamarin.Forms.Style.TargetType) `Style` и атрибут [x:TypeArguments](~/xamarin-forms/xaml/passing-arguments.md#generic_type_arguments) , используемый для указания аргументов в универсальных классах. Однако средство синтаксического анализа XAML автоматически выполняет `typeof` операцию, и в таких случаях не используется расширение разметки `x:Type`.

Одно из мест, *где требуется `x:Type`, —* это расширение разметки `x:Array`, которое описано в [следующем разделе](#array).

Расширение разметки `x:Type` также полезно при создании меню, в котором каждый элемент меню соответствует объекту определенного типа. Можно связать объект `Type` с каждым пунктом меню, а затем создать экземпляр объекта при выборе пункта меню.

Вот как работает меню навигации в `MainPage` в программе **расширения разметки** . Файл **MainPage. XAML** содержит `TableView` с каждой `TextCell`, соответствующей определенной странице в программе:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.MainPage"
             Title="Markup Extensions"
             Padding="10">
    <TableView Intent="Menu">
        <TableRoot>
            <TableSection>
                <TextCell Text="x:Static Demo"
                          Detail="Access constants or statics"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:StaticDemoPage}" />

                <TextCell Text="x:Reference Demo"
                          Detail="Reference named elements on the page"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:ReferenceDemoPage}" />

                <TextCell Text="x:Type Demo"
                          Detail="Associate a Button with a Type"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:TypeDemoPage}" />

                <TextCell Text="x:Array Demo"
                          Detail="Use an array to fill a ListView"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:ArrayDemoPage}" />

                ···                          

        </TableRoot>
    </TableView>
</ContentPage>
```

Вот как открыть главную страницу в **расширениях разметки**:

[![Главная страница](consuming-images/mainpage-small.png "Главная страница")](consuming-images/mainpage-large.png#lightbox "Главная страница")

Каждому свойству `CommandParameter` присваивается расширение разметки `x:Type`, ссылающееся на одну из других страниц. Свойство `Command` привязано к свойству с именем `NavigateCommand`. Это свойство определено в `MainPage` файле кода программной части:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(async (Type pageType) =>
        {
            Page page = (Page)Activator.CreateInstance(pageType);
            await Navigation.PushAsync(page);
        });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

Свойство `NavigateCommand` — это `Command` объект, реализующий команду Execute с аргументом типа `Type` &mdash; значение `CommandParameter`. Метод использует `Activator.CreateInstance` для создания экземпляра страницы, а затем переходит к ней. Конструктор завершает работу, устанавливая `BindingContext` страницы в себя, что позволяет `Binding` на `Command`. Дополнительные сведения об этом типе кода см. в статье [**Привязка данных**](~/xamarin-forms/app-fundamentals/data-binding/index.md) и в частности в статье о [**командном**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) коде.

На **демонстрационной странице x:Type** используется аналогичная методика для создания экземпляров элементов Xamarin. Forms и добавления их в `StackLayout`. XAML-файл изначально состоит из трех элементов `Button` с их `Command` свойствами, для которых задано `Binding`, а свойства `CommandParameter` установлены на типы трех представлений Xamarin. Forms:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.TypeDemoPage"
             Title="x:Type Demo">

    <StackLayout x:Name="stackLayout"
                 Padding="10, 0">

        <Button Text="Create a Slider"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Slider}" />

        <Button Text="Create a Stepper"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Stepper}" />

        <Button Text="Create a Switch"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Switch}" />
    </StackLayout>
</ContentPage>
```

Файл кода программной части определяет и инициализирует свойство `CreateCommand`:

```csharp
public partial class TypeDemoPage : ContentPage
{
    public TypeDemoPage()
    {
        InitializeComponent();

        CreateCommand = new Command<Type>((Type viewType) =>
        {
            View view = (View)Activator.CreateInstance(viewType);
            view.VerticalOptions = LayoutOptions.CenterAndExpand;
            stackLayout.Children.Add(view);
        });

        BindingContext = this;
    }

    public ICommand CreateCommand { private set; get; }
}
```

Метод, выполняемый при нажатии `Button`, создает новый экземпляр аргумента, устанавливает его свойство `VerticalOptions` и добавляет его в `StackLayout`. Три элемента `Button` затем совместно используют страницу с динамически созданными представлениями:

[![x:Type — демонстрация](consuming-images/typedemo-small.png "x:Type — демонстрация")](consuming-images/typedemo-large.png#lightbox "x:Type — демонстрация")

<a name="array" />

## <a name="xarray-markup-extension"></a>x:Array - расширение разметки

Расширение разметки `x:Array` позволяет определить массив в разметке. Он поддерживается классом [`ArrayExtension`](xref:Xamarin.Forms.Xaml.ArrayExtension) , который определяет два свойства:

- `Type` типа `Type`, который указывает тип элементов в массиве.
- `Items` типа `IList`, который является коллекцией самих элементов. Это свойство содержимого `ArrayExtension`.

Само расширение разметки `x:Array` не отображается в фигурных скобках. Вместо этого `x:Array` открывающий и закрывающий теги разделяют список элементов. Задайте для свойства `Type` расширение разметки `x:Type`.

На странице **демонстрации Расx:arrayов** показано, как использовать `x:Array` для добавления элементов в `ListView`, задав для свойства `ItemsSource` массив.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.ArrayDemoPage"
             Title="x:Array Demo Page">
    <ListView Margin="10">
        <ListView.ItemsSource>
            <x:Array Type="{x:Type Color}">
                <Color>Aqua</Color>
                <Color>Black</Color>
                <Color>Blue</Color>
                <Color>Fuchsia</Color>
                <Color>Gray</Color>
                <Color>Green</Color>
                <Color>Lime</Color>
                <Color>Maroon</Color>
                <Color>Navy</Color>
                <Color>Olive</Color>
                <Color>Pink</Color>
                <Color>Purple</Color>
                <Color>Red</Color>
                <Color>Silver</Color>
                <Color>Teal</Color>
                <Color>White</Color>
                <Color>Yellow</Color>
            </x:Array>
        </ListView.ItemsSource>

        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <BoxView Color="{Binding}"
                             Margin="3" />    
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>        
```

@No__t_0 создает простой `BoxView` для каждой записи цвета:

[![Демонстрация расx:Array](consuming-images/arraydemo-small.png "Демонстрация расx:Array")](consuming-images/arraydemo-large.png#lightbox "Демонстрация расx:Array")

Существует несколько способов указать отдельные элементы `Color` в этом массиве. Вы можете использовать расширение разметки `x:Static`:

```xaml
<x:Static Member="Color.Blue" />
```

Для получения цвета из словаря ресурсов можно также использовать `StaticResource`.

```xaml
<StaticResource Key="myColor" />
```

В конце этой статьи вы увидите собственное расширение разметки XAML, которое также создает новое значение цвета:

```xaml
<local:HslColor H="0.5" S="1.0" L="0.5" />
```

При определении массивов общих типов, таких как строки или числа, используйте теги, перечисленные в статье [**Передача аргументов конструктора**](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) , чтобы разделить значения.

<a name="null" />

## <a name="xnull-markup-extension"></a>x:Null - расширение разметки

Расширение разметки `x:Null` поддерживается классом [`NullExtension`](xref:Xamarin.Forms.Xaml.NullExtension) . Он не имеет свойств и является просто эквивалентом ключевого слова C# [`null`](/dotnet/csharp/language-reference/keywords/null/) в XAML.

Расширение разметки `x:Null` редко требуется и редко используется, но если вы нашли необходимость в нем, вы будете рады, что он существует.

В **демонстрационной странице x:NULL** показан один из сценариев, когда `x:Null` может быть удобным. Предположим, что вы определили неявные `Style` для `Label`, включающие `Setter`, которое задает для свойства `FontFamily` имя семейства, зависящее от платформы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.NullDemoPage"
             Title="x:Null Demo">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="48" />
                <Setter Property="FontFamily">
                    <Setter.Value>
                        <OnPlatform x:TypeArguments="x:String">
                            <On Platform="iOS" Value="Times New Roman" />
                            <On Platform="Android" Value="serif" />
                            <On Platform="UWP" Value="Times New Roman" />
                        </OnPlatform>
                    </Setter.Value>
                </Setter>
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <ContentPage.Content>
        <StackLayout Padding="10, 0">
            <Label Text="Text 1" />
            <Label Text="Text 2" />

            <Label Text="Text 3"
                   FontFamily="{x:Null}" />

            <Label Text="Text 4" />
            <Label Text="Text 5" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>   
```

Затем вы обнаружите, что для одного из элементов `Label` необходимо, чтобы все параметры свойств в неявном `Style`, за исключением `FontFamily`, которые должны быть значениями по умолчанию. Можно определить другой `Style` для этой цели, но более простой подход заключается в установке свойства `FontFamily` определенного `Label` на `x:Null`, как показано в центре `Label`.

Вот работающая программа:

[![Демонстрация x:Null](consuming-images/nulldemo-small.png "Демонстрация x:Null")](consuming-images/nulldemo-large.png#lightbox "Демонстрация x:Null")

Обратите внимание, что четыре элемента `Label` имеют шрифт с засечками, но Центральная `Label` имеет шрифт Sans-Serif по умолчанию.

<a name="onplatform" />

## <a name="onplatform-markup-extension"></a>Расширение разметки для платформы

Расширение разметки `OnPlatform` позволяет настраивать внешний вид пользовательского интерфейса для отдельных платформ. Он предоставляет те же функциональные возможности, что и классы [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) и [`On`](xref:Xamarin.Forms.On) , но с более кратким представлением.

Расширение разметки `OnPlatform` поддерживается классом [`OnPlatformExtension`](xref:Xamarin.Forms.Xaml.OnPlatformExtension) , который определяет следующие свойства:

- `Default` типа `object`, для которого задано значение по умолчанию, применяемое к свойствам, представляющим платформы.
- `Android` типа `object`, для которого задано значение, применяемое к Android.
- `GTK` типа `object`, для которого задано значение, применяемое к платформам GTK.
- `iOS` типа `object`, для которого задано значение, применяемое к iOS.
- `macOS` типа `object`, для которого задано значение, применяемое к macOS.
- `Tizen` типа `object`, для которого задано значение, применяемое к платформе Tizen.
- `UWP` типа `object`, для которого задано значение, применяемое к универсальная платформа Windows.
- `WPF` типа `object`, для которого задано значение, применяемое на Windows Presentation Foundationной платформе.
- `Converter` типа `IValueConverter`, для которого задана реализация `IValueConverter`.
- `ConverterParameter` типа `object`, для которого задается значение, передаваемое в реализацию `IValueConverter`.

> [!NOTE]
> Средство синтаксического анализа XAML позволяет сократить класс [`OnPlatformExtension`](xref:Xamarin.Forms.Xaml.OnPlatformExtension) как `OnPlatform`.

Свойство `Default` — это свойство содержимого `OnPlatformExtension`. Поэтому для выражений разметки XAML, выраженных с фигурными скобками, можно исключить `Default=` часть выражения, если она является первым аргументом. Если свойство `Default` не задано, по умолчанию будет установлено значение свойства [`BindableProperty.DefaultValue`](xref:Xamarin.Forms.BindableProperty.DefaultValue) , при условии, что расширение разметки нацелено на [`BindableProperty`](xref:Xamarin.Forms.BindableProperty).

> [!IMPORTANT]
> Средство синтаксического анализа XAML требует, чтобы значения правильного типа были предоставлены свойствам, которые занимают расширение разметки `OnPlatform`. Если требуется преобразование типа, расширение разметки `OnPlatform` будет пытаться выполнить его с помощью преобразователей по умолчанию, предоставляемых Xamarin. Forms. Однако существует несколько преобразований типов, которые невозможно выполнить с помощью преобразователей по умолчанию. в таких случаях для свойства `Converter` должно быть задано `IValueConverter` реализация.

На странице **демонстрационная страница для демонстрации архитектуры** показано, как использовать расширение разметки `OnPlatform`.

```xaml
<BoxView Color="{OnPlatform Yellow, iOS=Red, Android=Green, UWP=Blue}"
         WidthRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"  
         HeightRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"
         HorizontalOptions="Center" />
```

В этом примере все три выражения `OnPlatform` используют сокращенную версию имени класса `OnPlatformExtension`. Три `OnPlatform` расширения разметки задают свойства [`Color`](xref:Xamarin.Forms.BoxView.Color), [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)и [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) [`BoxView`](xref:Xamarin.Forms.BoxView) различным значениям в iOS, Android и UWP. Расширения разметки также предоставляют значения по умолчанию для этих свойств на неопределенных платформах, исключая `Default=` часть выражения. Обратите внимание, что заданные свойства расширения разметки разделяются запятыми.

Вот работающая программа:

[![Демонстрационная демонстрация](consuming-images/onplatformdemo-small.png "Демонстрационная демонстрация")](consuming-images/onplatformdemo-large.png#lightbox "Демонстрационная демонстрация")

<a name="onidiom" />

## <a name="onidiom-markup-extension"></a>Расширение разметки onidiomа

Расширение разметки `OnIdiom` позволяет настраивать внешний вид пользовательского интерфейса на основе идиомы устройства, на котором выполняется приложение. Он поддерживается классом [`OnIdiomExtension`](xref:Xamarin.Forms.Xaml.OnIdiomExtension) , который определяет следующие свойства:

- `Default` типа `object`, для которого задано значение по умолчанию, применяемое к свойствам, представляющим идиомы устройств.
- `Phone` типа `object`, для которого задано значение, применяемое к телефонам.
- `Tablet` типа `object`, для которого задано значение, применяемое к планшетам.
- `Desktop` типа `object`, для которого задано значение, применяемое к настольным платформам.
- `TV` типа `object`, для которого задано значение, применяемое на ТЕЛЕВИЗИОНных платформах.
- `Watch` типа `object`, для которого задано значение, применяемое к платформам наблюдения.
- `Converter` типа `IValueConverter`, для которого задана реализация `IValueConverter`.
- `ConverterParameter` типа `object`, для которого задается значение, передаваемое в реализацию `IValueConverter`.

> [!NOTE]
> Средство синтаксического анализа XAML позволяет сократить класс [`OnIdiomExtension`](xref:Xamarin.Forms.Xaml.OnIdiomExtension) как `OnIdiom`.

Свойство `Default` — это свойство содержимого `OnIdiomExtension`. Поэтому для выражений разметки XAML, выраженных с фигурными скобками, можно исключить `Default=` часть выражения, если она является первым аргументом.

> [!IMPORTANT]
> Средство синтаксического анализа XAML требует, чтобы значения правильного типа были предоставлены свойствам, которые занимают расширение разметки `OnIdiom`. Если требуется преобразование типа, расширение разметки `OnIdiom` будет пытаться выполнить его с помощью преобразователей по умолчанию, предоставляемых Xamarin. Forms. Однако существует несколько преобразований типов, которые невозможно выполнить с помощью преобразователей по умолчанию. в таких случаях для свойства `Converter` должно быть задано `IValueConverter` реализация.

На **демонстрационной странице Onidiomа** показано, как использовать расширение разметки `OnIdiom`.

```xaml
<BoxView Color="{OnIdiom Yellow, Phone=Red, Tablet=Green, Desktop=Blue}"
         WidthRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HeightRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HorizontalOptions="Center" />
```

В этом примере все три выражения `OnIdiom` используют сокращенную версию имени класса `OnIdiomExtension`. Три `OnIdiom` расширения разметки задают свойства [`Color`](xref:Xamarin.Forms.BoxView.Color), [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)и [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) для [`BoxView`](xref:Xamarin.Forms.BoxView) различным значениям в заметках телефона, планшета и рабочего стола. Расширения разметки также предоставляют значения по умолчанию для этих свойств в неуказанных идиомах, исключая `Default=` часть выражения. Обратите внимание, что заданные свойства расширения разметки разделяются запятыми.

Вот работающая программа:

[![Демонстрация onidiomы](consuming-images/onidiomdemo-small.png "Демонстрация onidiomы")](consuming-images/onidiomdemo-large.png#lightbox "Демонстрация onidiomы")

## <a name="datatemplate-markup-extension"></a>Расширение разметки DataTemplate

Расширение разметки `DataTemplate` позволяет преобразовать тип в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate). Он поддерживается классом `DataTemplateExtension`, который определяет свойство `TypeName` типа `string`, которому присваивается имя типа, преобразуемого в `DataTemplate`. Свойство `TypeName` — это свойство содержимого `DataTemplateExtension`. Таким образом, для выражений разметки XAML, выраженных с фигурными скобками, можно исключить `TypeName=` часть выражения.

> [!NOTE]
> Средство синтаксического анализа XAML позволяет сократить класс `DataTemplateExtension` как `DataTemplate`.

Типичное использование этого расширения разметки — в приложении оболочки, как показано в следующем примере:

```xaml
<ShellContent Title="Monkeys"
              Icon="monkey.png"
              ContentTemplate="{DataTemplate views:MonkeysPage}" />
```

В этом примере `MonkeysPage` преобразуется из [`ContentPage`](xref:Xamarin.Forms.ContentPage) в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), которое задается как значение свойства `ShellContent.ContentTemplate`. Это гарантирует, что `MonkeysPage` будет создаваться только при переходе на страницу, а не при запуске приложения.

Дополнительные сведения о приложениях оболочки см. в разделе [оболочка Xamarin. Forms](~/xamarin-forms/app-fundamentals/shell/index.md).

## <a name="fontimage-markup-extension"></a>Расширение разметки Фонтимаже

Расширение разметки `FontImage` позволяет отображать значок шрифта в любом представлении, которое может отображать `ImageSource`. Он предоставляет те же функциональные возможности, что и класс `FontImageSource`, но с более кратким представлением.

Расширение разметки `FontImage` поддерживается классом `FontImageExtension`, который определяет следующие свойства:

- `FontFamily` типа `string` — семейство шрифтов, которому принадлежит значок шрифта.
- `Glyph` типа `string`, значение символа Юникода со значком шрифта.
- `Color` типа `Color` — цвет, используемый при отображении значка шрифта.
- `Size` типа `double`, размер в единицах, независимых от устройства, значка отображаемого шрифта.

> [!NOTE]
> Средство синтаксического анализа XAML позволяет сократить класс `FontImageExtension` как `FontImage`.

Свойство `Glyph` — это свойство содержимого `FontImageExtension`. Поэтому для выражений разметки XAML, выраженных с фигурными скобками, можно исключить `Glyph=` часть выражения, если она является первым аргументом.

На **демонстрационной странице фонтимаже** показано, как использовать расширение разметки `FontImage`.

```xaml
<Image BackgroundColor="#D1D1D1"
       Source="{FontImage &#xf30c;, FontFamily={OnPlatform iOS=Ionicons, Android=ionicons.ttf#}, Size=44}" />
```

В этом примере сокращенная версия имени класса `FontImageExtension` используется для вывода значка XBox из семейства шрифтов Иониконс в [`Image`](xref:Xamarin.Forms.Image). Выражение также использует расширение разметки `OnPlatform` для указания различных значений свойств `FontFamily` в iOS и Android. Кроме того, `Glyph=` часть выражения исключена, а заданные свойства расширения разметки разделяются запятыми. Обратите внимание, что хотя символ Юникода для значка `\uf30c`, он должен быть экранирован в XAML, и поэтому становится `&#xf30c;`.

Вот работающая программа:

[![Снимок экрана расширения разметки Фонтимаже](consuming-images/fontimagedemo.png "Демонстрация Фонтимаже")](consuming-images/fontimagedemo-large.png#lightbox "Демонстрация Фонтимаже")

Сведения о отображении значков шрифтов путем указания данных значка шрифта в объекте `FontImageSource` см. в разделе [Отображение значков шрифтов](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons).

## <a name="define-your-own-markup-extensions"></a>Определение собственных расширений разметки

Если вы столкнулись с потребностью в расширении разметки XAML, которое недоступно в Xamarin. Forms, вы можете [создать собственный](creating.md).

## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)
- [Глава о расширениях разметки XAML из книги Xamarin. Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Словари ресурсов](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Динамические стили](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Привязка данных](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Оболочка Xamarin. Forms](~/xamarin-forms/app-fundamentals/shell/index.md).
