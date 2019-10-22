---
title: Часть 3. Расширения разметки XAML
description: Расширения разметки XAML составляют важную функцию в XAML, которая позволяет задавать свойства для объектов или значений, на которые косвенно ссылаются другие источники.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F4A37564-B18B-42FF-B841-9A1949895AB6
author: davidbritch
ms.author: dabritch
ms.date: 03/27/2018
ms.openlocfilehash: 1321f08cba4bf4cf23759ebb179a603b544ffc2e
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696649"
---
# <a name="part-3-xaml-markup-extensions"></a>Часть 3. Расширения разметки XAML

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_Расширения разметки XAML составляют важную функцию в XAML, которая позволяет задавать свойства для объектов или значений, на которые косвенно ссылаются другие источники. Расширения разметки XAML особенно важны для совместного использования объектов и ссылок на константы, используемые в приложении, но они находят наибольшую служебную программу в привязках данных._

## <a name="xaml-markup-extensions"></a>Расширения разметки XAML

Как правило, XAML используется для задания свойств объекта для явных значений, таких как строка, число, член перечисления или строка, которая преобразуется в значение в фоновом режиме.

Однако иногда свойства должны ссылаться на значения, определенные где-либо еще, или что может потребовать небольшой обработки кода во время выполнения. В этих целях *расширения разметки* XAML доступны.

Эти расширения разметки XAML не являются расширениями XML. XAML является полностью юридическим XML. Они называются "расширениями", так как они поддерживаются кодом в классах, реализующих `IMarkupExtension`. Вы можете написать собственные пользовательские расширения разметки.

Во многих случаях расширения разметки XAML мгновенно распознаются в файлах XAML, так как они отображаются как параметры атрибутов, разделенные фигурными скобками: {и}, но иногда расширения разметки отображаются в разметке как обычные элементы.

## <a name="shared-resources"></a>Общие ресурсы

Некоторые страницы XAML содержат несколько представлений со свойствами, для которых заданы одни и те же значения. Например, многие параметры свойств для этих `Button` объектов одинаковы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <StackLayout>
        <Button Text="Do this!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

        <Button Text="Do that!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

        <Button Text="Do the other thing!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

    </StackLayout>
</ContentPage>
```

Если необходимо изменить одно из этих свойств, можно внести изменение только один раз, а не три раза. Если бы это был код, скорее всего, вы будете использовать константы и статические объекты только для чтения, чтобы обеспечить единообразие и простоту изменения этих значений.

Одним популярным решением в XAML является хранение таких значений или объектов в *словаре ресурсов*. Класс `VisualElement` определяет свойство с именем `Resources` типа `ResourceDictionary`, которое представляет собой словарь с ключами типа `string` и значениями типа `object`. Вы можете разместить объекты в этом словаре, а затем ссылаться на них из разметки, все в XAML.

Чтобы использовать словарь ресурсов на странице, включите пару тегов элементов свойств `Resources`. Наиболее удобный способ разместить их в верхней части страницы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>

    </ContentPage.Resources>
    ...
</ContentPage>
```

Также необходимо явно включить Теги `ResourceDictionary`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>

        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Теперь объекты и значения различных типов можно добавить в словарь ресурсов. Эти типы должны быть поддерживающими создание экземпляров. Например, они не могут быть абстрактными классами. Эти типы также должны иметь открытый конструктор без параметров. Для каждого элемента требуется ключ словаря, указанный с помощью атрибута `x:Key`. Пример:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <LayoutOptions x:Key="horzOptions"
                           Alignment="Center" />

            <LayoutOptions x:Key="vertOptions"
                           Alignment="Center"
                           Expands="True" />
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Эти два элемента являются значениями типа структуры `LayoutOptions`, каждый из которых имеет уникальный ключ и одно или два набора свойств. В коде и разметке гораздо чаще используются статические поля `LayoutOptions`, но здесь удобнее устанавливать свойства.

Теперь необходимо задать `HorizontalOptions` и `VerticalOptions` свойства этих кнопок для этих ресурсов, а также расширение разметки XAML `StaticResource`:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="3"
        Rotation="-15"
        TextColor="Red"
        FontSize="24" />
```

Расширение разметки `StaticResource` всегда отделяется фигурными скобками и включает ключ словаря.

Имя `StaticResource` отличает его от `DynamicResource`, который также поддерживает Xamarin. Forms. `DynamicResource` предназначен для ключей словаря, связанных со значениями, которые могут изменяться во время выполнения, а `StaticResource` обращается к элементам из словаря только один раз при создании элементов на странице.

Для свойства `BorderWidth` необходимо сохранить значение Double в словаре. XAML удобно определяет теги для типов данных, таких как `x:Double` и `x:Int32`:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <LayoutOptions x:Key="horzOptions"
                       Alignment="Center" />

        <LayoutOptions x:Key="vertOptions"
                       Alignment="Center"
                       Expands="True" />

        <x:Double x:Key="borderWidth">
            3
        </x:Double>
    </ResourceDictionary>
</ContentPage.Resources>
```

Его не нужно размещать в трех строках. Эта запись словаря для этого угла вращения занимает всего одну строку:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <LayoutOptions x:Key="horzOptions"
                       Alignment="Center" />

        <LayoutOptions x:Key="vertOptions"
                       Alignment="Center"
                       Expands="True" />

         <x:Double x:Key="borderWidth">
            3
         </x:Double>

        <x:Double x:Key="rotationAngle">-15</x:Double>
    </ResourceDictionary>
</ContentPage.Resources>
```

На эти два ресурса можно ссылаться таким же образом, как и `LayoutOptions` значения:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="Red"
        FontSize="24" />
```

Для ресурсов типа `Color` можно использовать те же строковые представления, которые используются при непосредственном назначении атрибутов этих типов. Преобразователи типов вызываются при создании ресурса. Ниже приведен ресурс типа `Color`.

```xaml
<Color x:Key="textColor">Red</Color>
```

Часто программы задают `FontSize` свойства члену перечисления `NamedSize`, например `Large`. Класс `FontSizeConverter` работает в фоновом режиме, чтобы преобразовать его в значение, зависящее от платформы, с помощью метода `Device.GetNamedSized`. Однако при определении ресурса размера шрифта имеет смысл использовать числовое значение, показанное здесь как тип `x:Double`:

```xaml
<x:Double x:Key="fontSize">24</x:Double>
```

Теперь все свойства, кроме `Text`, определяются параметрами ресурсов:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="{StaticResource textColor}"
        FontSize="{StaticResource fontSize}" />
```

Можно также использовать `OnPlatform` в словаре ресурсов, чтобы определить различные значения для платформ. Вот как объект `OnPlatform` может быть частью словаря ресурсов для различных цветов текста:

```xaml
<OnPlatform x:Key="textColor"
            x:TypeArguments="Color">
    <On Platform="iOS" Value="Red" />
    <On Platform="Android" Value="Aqua" />
    <On Platform="UWP" Value="#80FF80" />
</OnPlatform>
```

Обратите внимание, что `OnPlatform` получает атрибут `x:Key`, так как он является объектом в словаре и атрибутом `x:TypeArguments`, так как это универсальный класс. Атрибуты `iOS`, `Android` и `UWP` преобразуются в значения `Color` при инициализации объекта.

Вот окончательный полный XAML файл с тремя кнопками, обращающимися к шести общим значениям:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <LayoutOptions x:Key="horzOptions"
                           Alignment="Center" />

            <LayoutOptions x:Key="vertOptions"
                           Alignment="Center"
                           Expands="True" />

            <x:Double x:Key="borderWidth">3</x:Double>

            <x:Double x:Key="rotationAngle">-15</x:Double>

            <OnPlatform x:Key="textColor"
                        x:TypeArguments="Color">
                <On Platform="iOS" Value="Red" />
                <On Platform="Android" Value="Aqua" />
                <On Platform="UWP" Value="#80FF80" />
            </OnPlatform>

            <x:Double x:Key="fontSize">24</x:Double>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <Button Text="Do this!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

        <Button Text="Do that!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

        <Button Text="Do the other thing!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

    </StackLayout>
</ContentPage>
```

Снимки экрана проверяют единообразное оформление и стиль, зависящий от платформы:

[Элементы управления ![Styled](xaml-markup-extensions-images/sharedresources.png)](xaml-markup-extensions-images/sharedresources-large.png#lightbox)

Несмотря на то, что для определения коллекции `Resources` в верхней части страницы, следует помнить, что свойство `Resources` определяется `VisualElement`, а также можно иметь `Resources` коллекции на других элементах страницы. Например, попробуйте добавить его к `StackLayout` в этом примере:

```xaml
<StackLayout>
    <StackLayout.Resources>
        <ResourceDictionary>
            <Color x:Key="textColor">Blue</Color>
        </ResourceDictionary>
    </StackLayout.Resources>
    ...
</StackLayout>
```

Вы обнаружите, что цвет текста кнопок теперь синим. По сути, всякий раз, когда средство синтаксического анализа XAML встречает расширение разметки `StaticResource`, оно выполняет поиск по визуальному дереву и использует первый `ResourceDictionary`, где встречается этот ключ.

Одним из наиболее распространенных типов объектов, хранящихся в словарях ресурсов, является `Style` Xamarin. Forms, который определяет коллекцию параметров свойств. Стили обсуждаются в разделе [стили](~/xamarin-forms/user-interface/styles/index.md)статьи.

Иногда разработчики знакомы с XAML, если они могут разместить визуальный элемент, например `Label` или `Button` в `ResourceDictionary`. Хотя это и вполне возможно, это не имеет большого смысла. Целью `ResourceDictionary` является совместное использование объектов. Визуальный элемент не может быть общим. Один и тот же экземпляр не может дважды отображаться на одной странице.

## <a name="the-xstatic-markup-extension"></a>Расширение разметки x:Static

Несмотря на сходства их имен, `x:Static` и `StaticResource` очень различны. `StaticResource` возвращает объект из словаря ресурсов, в то время как `x:Static` обращается к одному из следующих объектов:

- открытое статическое поле
- открытое статическое свойство
- Открытое поле константы
- Элемент перечисления.

Расширение разметки `StaticResource` поддерживается реализациями XAML, которые определяют словарь ресурсов, тогда как `x:Static` является внутренней частью XAML, как показывает префикс `x`.

Ниже приведено несколько примеров, демонстрирующих, как `x:Static` может явно ссылаться на статические поля и члены перечисления:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="{x:Static LayoutOptions.Start}"
       HorizontalTextAlignment="{x:Static TextAlignment.Center}"
       TextColor="{x:Static Color.Aqua}" />
```

До сих пор это не очень впечатляет. Однако расширение разметки `x:Static` также может ссылаться на статические поля или свойства из собственного кода. Например, ниже приведен класс `AppConstants`, содержащий некоторые статические поля, которые можно использовать на нескольких страницах в приложении:

```csharp
using System;
using Xamarin.Forms;

namespace XamlSamples
{
    static class AppConstants
    {
        public static readonly Thickness PagePadding;

        public static readonly Font TitleFont;

        public static readonly Color BackgroundColor = Color.Aqua;

        public static readonly Color ForegroundColor = Color.Brown;

        static AppConstants()
        {
            switch (Device.RuntimePlatform)
            {
                case Device.iOS:
                    PagePadding = new Thickness(5, 20, 5, 0);
                    TitleFont = Font.SystemFontOfSize(35, FontAttributes.Bold);
                    break;

                case Device.Android:
                    PagePadding = new Thickness(5, 0, 5, 0);
                    TitleFont = Font.SystemFontOfSize(40, FontAttributes.Bold);
                    break;

                case Device.UWP:
                    PagePadding = new Thickness(5, 0, 5, 0);
                    TitleFont = Font.SystemFontOfSize(50, FontAttributes.Bold);
                    break;
            }
        }
    }
}
```

Чтобы сослаться на статические поля этого класса в XAML-файле, вам потребуется какой-то способ указать в файле XAML, где расположен этот файл. Это делается с помощью объявления пространства имен XML.

Помните, что файлы XAML, созданные как часть стандартного шаблона XAML Xamarin. Forms, содержат два объявления пространства имен XML: один для доступа к классам Xamarin. Forms и другой для ссылок на теги и атрибуты, встроенные в XAML:

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

Для доступа к другим классам потребуются дополнительные объявления пространства имен XML. Каждое дополнительное объявление пространства имен XML определяет новый префикс. Для доступа к классам, локальным для общей библиотеки .NET Standard приложений, например `AppConstants`, программисты XAML часто используют префикс `local`. Объявление пространства имен должно указывать имя пространства имен CLR (среда CLR), также известное как имя пространства имен .NET, которое является именем, которое отображается в определении C# `namespace` или в директиве `using`:

```csharp
xmlns:local="clr-namespace:XamlSamples"
```

Также можно определить объявления пространств имен XML для пространств имен .NET в любой сборке, на которую ссылается библиотека .NET Standard. Например, ниже приведен префикс `sys` для стандартного пространства имен .NET `System`, который находится в сборке **mscorlib** , который один раз стояли для "Библиотека общих объектов среды выполнения Microsoft", но теперь означает "Многоязыковая общая библиотека среды выполнения объектов". Так как это другая сборка, необходимо также указать имя сборки, в данном случае **mscorlib**:

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Обратите внимание, что за ключевым словом `clr-namespace` следует двоеточие, затем имя пространства имен .NET, затем — точку с запятой, ключевое слово `assembly`, знак равенства и имя сборки.

Да, двоеточие после `clr-namespace`, но знак равенства следует `assembly`. Синтаксис был определен таким образом, намеренно: большинство объявлений пространств имен XML ссылаются на URI, который начинает имя схемы URI, например `http`, за которым всегда следует двоеточие. @No__t_0 часть этой строки предназначена для имитации этого соглашения.

Эти объявления пространств имен включены в пример **статикконстантспаже** . Обратите внимание, что `BoxView` измерения имеют значение `Math.PI` и `Math.E`, но масштабируется по 100:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             x:Class="XamlSamples.StaticConstantsPage"
             Title="Static Constants Page"
             Padding="{x:Static local:AppConstants.PagePadding}">

    <StackLayout>
       <Label Text="Hello, XAML!"
              TextColor="{x:Static local:AppConstants.BackgroundColor}"
              BackgroundColor="{x:Static local:AppConstants.ForegroundColor}"
              Font="{x:Static local:AppConstants.TitleFont}"
              HorizontalOptions="Center" />

      <BoxView WidthRequest="{x:Static sys:Math.PI}"
               HeightRequest="{x:Static sys:Math.E}"
               Color="{x:Static local:AppConstants.ForegroundColor}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               Scale="100" />
    </StackLayout>
</ContentPage>
```

Размер результирующего `BoxView` относительно экрана зависит от платформы:

[![Controls с использованием расширения разметки x:Static](xaml-markup-extensions-images/staticconstants.png)](xaml-markup-extensions-images/staticconstants-large.png#lightbox)

## <a name="other-standard-markup-extensions"></a>Другие стандартные расширения разметки

Несколько расширений разметки являются встроенными для XAML и поддерживаются в файлах XAML Xamarin. Forms. Некоторые из них не используются очень часто, но они необходимы при необходимости:

- Если свойство имеет значение, отличное от `null` по умолчанию, но его необходимо задать для `null`, задайте для него расширение разметки `{x:Null}`.
- Если свойство имеет тип `Type`, его можно назначить `Type` объекту с помощью расширения разметки `{x:Type someClass}`.
- Вы можете определить массивы в XAML, используя расширение разметки `x:Array`. У этого расширения разметки есть обязательный атрибут с именем `Type`, указывающий тип элементов в массиве.
- Расширение разметки `Binding` рассматривается в [части 4. Основы привязки данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).
- Расширение разметки `RelativeSource` рассматривается в [относительных привязках](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md).

## <a name="the-constraintexpression-markup-extension"></a>Расширение разметки Констраинтекспрессион

Расширения разметки могут иметь свойства, но они не задаются как XML-атрибуты. В расширении разметки параметры свойств разделяются запятыми, и в фигурных скобках не отображаются кавычки.

Это можно проиллюстрировать с помощью расширения разметки Xamarin. Forms с именем `ConstraintExpression`, которое используется с классом `RelativeLayout`. Можно указать расположение или размер дочернего представления в виде константы или относительно родительского или другого именованного представления. Синтаксис `ConstraintExpression` позволяет задать расположение или размер представления, используя `Factor` раз свойства другого представления, а также `Constant`. Что-то более сложное, чем требует кода.

Далее приводится пример:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.RelativeLayoutPage"
             Title="RelativeLayout Page">

    <RelativeLayout>

        <!-- Upper left -->
        <BoxView Color="Red"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}" />
        <!-- Upper right -->
        <BoxView Color="Green"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=1,
                                            Constant=-40}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}" />
        <!-- Lower left -->
        <BoxView Color="Blue"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=1,
                                            Constant=-40}" />
        <!-- Lower right -->
        <BoxView Color="Yellow"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=1,
                                            Constant=-40}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=1,
                                            Constant=-40}" />

        <!-- Centered and 1/3 width and height of parent -->
        <BoxView x:Name="oneThird"
                 Color="Red"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=0.33}"
                 RelativeLayout.WidthConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.HeightConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=0.33}"  />

        <!-- 1/3 width and height of previous -->
        <BoxView Color="Blue"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=X}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Y}"
                 RelativeLayout.WidthConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.HeightConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Height,
                                            Factor=0.33}"  />
    </RelativeLayout>
</ContentPage>
```

Возможно, самым важным уроком, который следует взять из этого примера, является синтаксис расширения разметки: в фигурных скобках расширения разметки кавычки не должны состоять. При вводе расширения разметки в XAML-файл, естественно, необходимо заключить значения свойств в кавычки. Искушения!

Программа работает следующим образом:

[Разметка ![Relative с помощью ограничений](xaml-markup-extensions-images/relativelayout.png)](xaml-markup-extensions-images/relativelayout-large.png#lightbox)

## <a name="summary"></a>Сводка

Приведенные здесь расширения разметки XAML предоставляют важную поддержку для XAML файлов. Но, возможно, наиболее важным расширением разметки XAML является `Binding`, которое рассматривается в следующей части этой серии, [часть 4. Основы привязки данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

## <a name="related-links"></a>Связанные ссылки

- [ксамлсамплес](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [Часть 1. начало работы с XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Часть 2. Важный синтаксис XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Часть 4. Основы привязки данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Часть 5. Из привязки данных к MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
