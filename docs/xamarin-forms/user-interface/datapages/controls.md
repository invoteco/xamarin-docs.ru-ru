---
title: Справочник по элементам управления "страницы на странице"
description: В этой статье описываются элементы управления, доступные в пакете NuGet страниц Xamarin. Forms.
ms.prod: xamarin
ms.assetid: 891615D0-E8BD-4ACC-A7F0-4C3725FBCC31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 2af4bc3bd4dc6ae47f573c58c84aad54f41d42cd
ms.sourcegitcommit: 41a029c69925e3a9d2de883751ebfd649e8747cd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/13/2019
ms.locfileid: "68980818"
---
# <a name="datapages-controls-reference"></a>Справочник по элементам управления "страницы на странице"

![](~/media/shared/preview.png "Этот API доступна в предварительной версии")

> [!IMPORTANT]
> Для подготовки страниц к просмотру требуется ссылка на тему Xamarin. Forms. Это включает в себя установку пакета NuGet [Xamarin. Forms. Theme. base](https://www.nuget.org/packages/Xamarin.Forms.Theme.Base/) в проект, за которым следуют пакеты NuGet [Xamarin. Forms. Theme. light](https://www.nuget.org/packages/Xamarin.Forms.Theme.Light/) или [Xamarin. Forms. Theme. темно](https://www.nuget.org/packages/Xamarin.Forms.Theme.Dark/) .

В число страниц данных Xamarin. Forms NuGet входит ряд элементов управления, которые могут использовать преимущества привязки к источнику.

Чтобы использовать эти элементы управления в XAML, убедитесь, что пространство имен включено, например, см `xmlns:pages` . описание ниже:

```xaml
<ContentPage
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:pages="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
    x:Class="DataPagesDemo.Detail">
```

В приведенных ниже `DynamicResource` примерах содержатся ссылки, которые должны существовать в словаре ресурсов проекта для работы. Также есть пример создания [пользовательского элемента управления](#custom)

## <a name="built-in-controls"></a>Встроенные элементы управления

* [HeroImage](#heroimage)
* [SPListItem](#listitem)

<a name="heroimage" />

### <a name="heroimage"></a>хероимаже

`HeroImage` Элемент управления имеет четыре свойства:

* Текст
* Подробно
* ImageSource
* Сторона

```xaml
<pages:HeroImage
    ImageSource="{ DynamicResource HeroImageImage }"
    Text="Keith Ballinger"
    Detail="Xamarin"
/>
```

**Android**

![](controls-images/heroimage-light-android.png "Элемент управления имиджевое изображение на Android") ![](controls-images/heroimage-dark-android.png "имиджевое изображение элемента управления на устройстве Android")

**iOS**

![](controls-images/heroimage-light-ios.png "Имиджевое изображение элемента управления в iOS") ![](controls-images/heroimage-dark-ios.png "имиджевое изображение элемента управления в iOS")


<a name="listitem" />

### <a name="listitem"></a>ListItem

Макет `ListItem` элемента управления похож на собственный список строк в формате iOS и Android или строки таблицы, однако его также можно использовать в качестве обычного представления. В приведенном ниже примере кода он показан `StackLayout`в, но его также можно использовать в элементах управления "список сколлинг с привязкой к данным".

Существует пять свойств:

* Заголовок
* Подробно
* ImageSource
* плацехолдимажесаурце
* Сторона

```xaml
<StackLayout Spacing="0">
    <pages:ListItemControl
        Detail="Xamarin"
        ImageSource="{ DynamicResource UserImage }"
        Title="Miguel de Icaza"
        PlaceholdImageSource="{ DynamicResource IconImage }"
    />
```

`ListItem` На этих снимках экрана показаны платформы iOS и Android с использованием светлых и темных тем:

**Android**

![](controls-images/listitem-light-android.png "Элемента управления ListItem в Android") ![](controls-images/listitem-dark-android.png "элемента управления ListItem в Android")

**iOS**

![](controls-images/listitem-light-ios.png "Элемента управления ListItem в iOS") ![](controls-images/listitem-dark-ios.png "элемента управления ListItem в iOS")


## <a name="custom-control-example"></a>Пример пользовательского элемента управления

Цель этого пользовательского `CardView` элемента управления — это похоже на собственный кардвиев Android.

Он будет содержать три свойства:

* Текст
* Подробно
* ImageSource

Целью является пользовательский элемент управления, который будет выглядеть, как в приведенном ниже коде (Обратите внимание, что требуется пользовательское `xmlns:local` значение, которое ссылается на текущую сборку):

```xaml
<local:CardView
  ImageSource="{ DynamicResource CardViewImage }"
  Text="CardView Text"
  Detail="CardView Detail"
/>
```

Он должен выглядеть, как показано ниже, используя цвета, соответствующие встроенным светлым и темным темам:

**Android**

![](controls-images/cardview-light-android.png "CardView пользовательский элемент управления на устройстве Android") ![](controls-images/cardview-dark-android.png "CardView пользовательский элемент управления на устройстве Android")

**iOS**

![](controls-images/cardview-light-ios.png "CardView пользовательский элемент управления в iOS") ![](controls-images/cardview-dark-ios.png "CardView пользовательский элемент управления в iOS")

<a name="custom" />

### <a name="building-the-custom-cardview"></a>Создание пользовательского Кардвиев

1. [Подкласс DataView](#1)
2. [Определение шрифта, макета и полей](#2)
3. [Создание стилей для дочерних элементов элемента управления](#3)
4. [Создание шаблона макета элемента управления](#4)
5. [Добавление ресурсов для конкретной темы](#5)
6. [Установка ControlTemplate для класса Кардвиев](#6)
7. [Добавление элемента управления на страницу](#7)

<a name="1" />

#### <a name="1-dataview-subclass"></a>1. Подкласс DataView

C# Подкласс`DataView` определяет связываемые свойства для элемента управления.

```csharp
public class CardView : DataView
{
    public static readonly BindableProperty TextProperty =
        BindableProperty.Create ("Text", typeof (string), typeof (CardView), null, BindingMode.TwoWay);

    public string Text
    {
        get { return (string)GetValue (TextProperty); }
        set { SetValue (TextProperty, value); }
    }

    public static readonly BindableProperty DetailProperty =
        BindableProperty.Create ("Detail", typeof (string), typeof (CardView), null, BindingMode.TwoWay);

    public string Detail
    {
        get { return (string)GetValue (DetailProperty); }
        set { SetValue (DetailProperty, value); }
    }

    public static readonly BindableProperty ImageSourceProperty =
        BindableProperty.Create ("ImageSource", typeof (ImageSource), typeof (CardView), null, BindingMode.TwoWay);

    public ImageSource ImageSource
    {
        get { return (ImageSource)GetValue (ImageSourceProperty); }
        set { SetValue (ImageSourceProperty, value); }
    }

    public CardView()
    {
    }
}
```

<a name="2" />

#### <a name="2-define-font-layout-and-margins"></a>2. Определение шрифта, макета и полей

Конструктор элементов управления будет вычислить эти значения как часть структуры пользовательского интерфейса для пользовательского элемента управления. Когда требуются спецификации для конкретной платформы, `OnPlatform` используется элемент.

Обратите внимание, что некоторые `StaticResource`значения ссылаются на s — они будут определены на [шаге 5](#5).

```xml
<!-- CARDVIEW FONT SIZES -->
<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewTextFontSize">
        <On Platform="iOS, Android" Value="15" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewDetailFontSize">
        <On Platform="iOS, Android" Value="13" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewTextTextColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewTextTextColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewTextTextColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="Thickness"    x:Key="CardViewTextlMargin">
        <On Platform="iOS" Value="12,10,12,4" />
        <On Platform="Android" Value="20,0,20,5" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewDetailTextColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewDetailTextColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewDetailTextColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="Thickness"    x:Key="CardViewDetailMargin">
        <On Platform="iOS" Value="12,0,10,12" />
        <On Platform="Android" Value="20,0,20,20" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewBackgroundColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewBackgroundColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewBackgroundColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewShadowSize">
        <On Platform="iOS" Value="2" />
        <On Platform="Android" Value="5" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewCornerRadius">
        <On Platform="iOS" Value="0" />
        <On Platform="Android" Value="4" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewShadowColor">
        <On Platform="iOS, Android" Value="#CDCDD1" />
</OnPlatform>
```

<a name="3" />

#### <a name="3-create-styles-for-the-controls-children"></a>3. Создание стилей для дочерних элементов элемента управления

Чтобы создать дочерние элементы, которые будут использоваться в пользовательском элементе управления, выполните ссылки на все определенные сведения об элементах.

```xml
<!-- EXPLICIT STYLES (will be Classes) -->
<Style TargetType="Label" x:Key="CardViewTextStyle">
    <Setter Property="FontSize" Value="{ StaticResource CardViewTextFontSize }" />
    <Setter Property="TextColor" Value="{ StaticResource CardViewTextTextColor }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="Margin" Value="{ StaticResource CardViewTextlMargin }" />
    <Setter Property="HorizontalTextAlignment" Value="Start" />
</Style>

<Style TargetType="Label" x:Key="CardViewDetailStyle">
    <Setter Property="HorizontalTextAlignment" Value="Start" />
    <Setter Property="TextColor" Value="{ StaticResource CardViewDetailTextColor }" />
    <Setter Property="FontSize" Value="{ StaticResource CardViewDetailFontSize }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="Margin" Value="{ StaticResource CardViewDetailMargin }" />
</Style>

<Style TargetType="Image" x:Key="CardViewImageImageStyle">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="Center" />
    <Setter Property="WidthRequest" Value="220"/>
    <Setter Property="HeightRequest" Value="165"/>
</Style>
```

<a name="4" />

#### <a name="4-create-the-control-layout-template"></a>4. Создание шаблона макета элемента управления

Визуальная структура пользовательского элемента управления явно объявлена в шаблоне элемента управления с использованием ресурсов, определенных выше:

```xml
<!--- CARDVIEW -->
<ControlTemplate x:Key="CardViewControlControlTemplate">
  <StackLayout
    Spacing="0"
    BackgroundColor="{ TemplateBinding BackgroundColor }"
  >

    <!-- CARDVIEW IMAGE -->
    <Image
      Source="{ TemplateBinding ImageSource }"
      HorizontalOptions="FillAndExpand"
      VerticalOptions="StartAndExpand"
      Aspect="AspectFill"
      Style="{ StaticResource CardViewImageImageStyle }"
    />

    <!-- CARDVIEW TEXT -->
    <Label
      Text="{ TemplateBinding Text }"
      LineBreakMode="WordWrap"
      VerticalOptions="End"
      Style="{ StaticResource CardViewTextStyle }"
    />


    <!-- CARDVIEW DETAIL -->
    <Label
      Text="{ TemplateBinding Detail }"
      LineBreakMode="WordWrap"
      VerticalOptions="End"
      Style="{ StaticResource CardViewDetailStyle }" />

  </StackLayout>

</ControlTemplate>
```

<a name="5" />

#### <a name="5-add-the-theme-specific-resources"></a>5. Добавление ресурсов для конкретной темы

Так как это пользовательский элемент управления, добавьте ресурсы, соответствующие теме, в которой используется словарь ресурсов.

##### <a name="light-theme-colors"></a>Цвета светлой темы

```xaml
<Color x:Key="iOSCardViewBackgroundColor">#FFFFFF</Color>
<Color x:Key="AndroidCardViewBackgroundColor">#FFFFFF</Color>

<Color x:Key="AndroidCardViewTextTextColor">#030303</Color>
<Color x:Key="iOSCardViewTextTextColor">#030303</Color>

<Color x:Key="AndroidCardViewDetailTextColor">#8F8E94</Color>
<Color x:Key="iOSCardViewDetailTextColor">#8F8E94</Color>
```

##### <a name="dark-theme-colors"></a>Темные цвета темы

```xaml
<!-- CARD VIEW COLORS -->
            <Color x:Key="iOSCardViewBackgroundColor">#404040</Color>
            <Color x:Key="AndroidCardViewBackgroundColor">#404040</Color>

            <Color x:Key="AndroidCardViewTextTextColor">#FFFFFF</Color>
            <Color x:Key="iOSCardViewTextTextColor">#FFFFFF</Color>

            <Color x:Key="AndroidCardViewDetailTextColor">#B5B4B9</Color>
            <Color x:Key="iOSCardViewDetailTextColor">#B5B4B9</Color>
```

<a name="6" />

#### <a name="6-set-the-controltemplate-for-the-cardview-class"></a>6. Установка ControlTemplate для класса Кардвиев

Наконец, убедитесь, C# что класс, созданный на [шаге 1](#1) , использует шаблон элемента управления, определенный на `Style` `Setter` [шаге 4](#4) , с помощью элемента.

```xml
<Style TargetType="local:CardView">
    <Setter Property="ControlTemplate" Value="{ StaticResource CardViewControlControlTemplate }" />
  ... some custom styling omitted
  <Setter Property="BackgroundColor" Value="{ StaticResource CardViewBackgroundColor }" />
</Style>
```

<a name="7" />

#### <a name="7-add-the-control-to-a-page"></a>7. Добавление элемента управления на страницу

Теперь `CardView` элемент управления можно добавить на страницу. В примере ниже показано, `StackLayout`что он размещен в:

```xaml
<StackLayout Spacing="0">
  <local:CardView
    Margin="12,6"
    ImageSource="{ DynamicResource CardViewImage }"
    Text="CardView Text"
    Detail="CardView Detail"
  />
</StackLayout>

```
