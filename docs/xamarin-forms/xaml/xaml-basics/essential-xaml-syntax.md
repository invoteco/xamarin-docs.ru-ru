---
title: Часть 2. Основной синтаксис XAML
description: В этой статье объясняется, самых важных особенностей синтаксиса XAML из элементов свойства и вложенные свойства.
ms.prod: xamarin
ms.assetid: 4022F1DC-3802-4635-A553-688ABD3F0D5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: f79a07a04eddeea1441f7938fdef210a37fb920a
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306575"
---
# <a name="part-2-essential-xaml-syntax"></a>Часть 2. Основной синтаксис XAML

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_XAML в основном предназначен для создания экземпляров и инициализации объектов. Но часто свойства должны быть заданы для сложных объектов, которые не могут быть представлены в виде XML-строк, а иногда свойства, определяемые одним классом, должны быть установлены в дочернем классе. Для этих двух нужд требуются ключевые функции синтаксиса XAML элементов свойств и вложенных свойств._

## <a name="property-elements"></a>Свойства элементов

В XAML обычно задаются свойства классов как атрибуты XML:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large"
       TextColor="Aqua" />
```

Тем не менее есть альтернативный способ задать свойство в XAML. Чтобы использовать эту альтернативу с `TextColor`, сначала удалите существующий параметр `TextColor`:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large" />
```

Откройте тег `Label` пустого элемента, разделив его на начальный и конечный теги:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">

</Label>
```

В этих тегов добавьте открывающий и закрывающий теги, которые состоят из имени класса и имя свойства, разделенные точкой.

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>

    </Label.TextColor>
</Label>
```

Задайте значение свойства как содержимое этих новых тегов, следующим образом:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>
        Aqua
    </Label.TextColor>
</Label>
```

Эти два способа указания свойства `TextColor` функционально эквивалентны, но не используют два способа для одного и того же свойства, так как это значение эффективно настраивает свойство дважды и может быть неоднозначным.

С помощью этого нового синтаксиса могут быть введены под рукой термины:

- `Label` является *элементом объекта*. Это объекта Xamarin.Forms, выраженный в виде XML-элемента.
- `Text`, `VerticalOptions`, `FontAttributes` и `FontSize` являются *атрибутами свойств*. Они являются свойствами Xamarin.Forms выразить в виде атрибутов XML.
- В этом конечном фрагменте `TextColor` стала *элементом свойства*. Это свойство Xamarin.Forms, однако сейчас это XML-элемента.

Определение свойства, элементы могут в сначала кажется воспринимает как нарушение синтаксиса XML, но это не так. Период не имеет специального значения в формате XML. Для декодера XML `Label.TextColor` является просто обычным дочерним элементом.

В XAML этот синтаксис очень специфический. Одно из правил для элементов свойств заключается в том, что в теге `Label.TextColor` не может присутствовать ничего другого. Значение свойства всегда определяется как содержимое между элемента свойства открывающих и закрывающих тегах.

Можно использовать синтаксис элемента свойства на более чем одно свойство:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center">
    <Label.FontAttributes>
        Bold
    </Label.FontAttributes>
    <Label.FontSize>
        Large
    </Label.FontSize>
    <Label.TextColor>
        Aqua
    </Label.TextColor>
</Label>
```

Или можно использовать синтаксис элемента свойства для всех свойств:

```xaml
<Label>
    <Label.Text>
        Hello, XAML!
    </Label.Text>
    <Label.FontAttributes>
        Bold
    </Label.FontAttributes>
    <Label.FontSize>
        Large
    </Label.FontSize>
    <Label.TextColor>
        Aqua
    </Label.TextColor>
    <Label.VerticalOptions>
        Center
    </Label.VerticalOptions>
</Label>
```

На первый синтаксис элемента свойства может показаться замещающего ненужные длиннющее для сравнительно довольно простой и в этих примерах это определенно так.

Однако синтаксис элемента свойства становится жизненно важным, когда значение свойства слишком сложен, представлены в виде простой строки. Внутри тегов элемента свойства можно создать экземпляр другому объекту и задать его свойства. Например, можно явно задать свойству, например `VerticalOptions`, значение `LayoutOptions` с параметрами свойства:

```xaml
<Label>
    ...
    <Label.VerticalOptions>
        <LayoutOptions Alignment="Center" />
    </Label.VerticalOptions>
</Label>
```

Еще один пример: у `Grid` есть два свойства с именами `RowDefinitions` и `ColumnDefinitions`. Эти два свойства имеют тип `RowDefinitionCollection` и `ColumnDefinitionCollection`, которые являются коллекциями объектов `RowDefinition` и `ColumnDefinition`. Необходимо использовать синтаксис элемента свойства для задания этих коллекций.

Вот начало XAML-файла для `GridDemoPage` класса, в котором показаны теги элемента свойства для коллекций `RowDefinitions` и `ColumnDefinitions`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.GridDemoPage"
             Title="Grid Demo Page">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="100" />
        </Grid.ColumnDefinitions>
        ...
    </Grid>
</ContentPage>
```

Обратите внимание, что сокращенный синтаксис для определения автоматическим размером ячейки, ячейки пикселей ширины и высоты и параметры типа "звезда".

## <a name="attached-properties"></a>Вложенные свойства

Вы только что видели, что `Grid` требуются элементы свойств для коллекций `RowDefinitions` и `ColumnDefinitions` для определения строк и столбцов. Однако для программиста также должен быть определен способ указания строки и столбца, где размещается каждый дочерний элемент `Grid`.

В теге для каждого дочернего элемента `Grid` указать строку и столбец этого дочернего объекта, используя следующие атрибуты:

- `Grid.Row`
- `Grid.Column`

Значения по умолчанию эти атрибуты являются 0. Можно также указать, если дочерний элемент охватывает более одной строки или столбца со следующими атрибутами:

- `Grid.RowSpan`
- `Grid.ColumnSpan`

Эти атрибуты имеют значения по умолчанию 1.

Ниже приведен полный файл GridDemoPage.xaml.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.GridDemoPage"
             Title="Grid Demo Page">

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="100" />
        </Grid.ColumnDefinitions>

        <Label Text="Autosized cell"
               Grid.Row="0" Grid.Column="0"
               TextColor="White"
               BackgroundColor="Blue" />

        <BoxView Color="Silver"
                 HeightRequest="0"
                 Grid.Row="0" Grid.Column="1" />

        <BoxView Color="Teal"
                 Grid.Row="1" Grid.Column="0" />

        <Label Text="Leftover space"
               Grid.Row="1" Grid.Column="1"
               TextColor="Purple"
               BackgroundColor="Aqua"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Span two rows (or more if you want)"
               Grid.Row="0" Grid.Column="2" Grid.RowSpan="2"
               TextColor="Yellow"
               BackgroundColor="Blue"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Span two columns"
               Grid.Row="2" Grid.Column="0" Grid.ColumnSpan="2"
               TextColor="Blue"
               BackgroundColor="Yellow"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Fixed 100x100"
               Grid.Row="2" Grid.Column="2"
               TextColor="Aqua"
               BackgroundColor="Red"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

    </Grid>
</ContentPage>
```

Параметры `Grid.Row` и `Grid.Column`, равные 0, не являются обязательными, но обычно включаются в целях ясности.

Вот, как оно выглядит:

[Макет сетки ![](essential-xaml-syntax-images/griddemo.png)](essential-xaml-syntax-images/griddemo-large.png#lightbox)

Судя только из синтаксиса, эти `Grid.Row`, `Grid.Column`, `Grid.RowSpan`и `Grid.ColumnSpan` атрибуты выглядят как статические поля или свойства `Grid`, но любопытно достаточно, `Grid` не определяет ничего с именем `Row`, `Column`, `RowSpan`или `ColumnSpan`.

Вместо этого `Grid` определяет четыре связываемых свойства с именами `RowProperty`, `ColumnProperty`, `RowSpanProperty`и `ColumnSpanProperty`. Это специальные типы связываемых свойств, называемые *присоединенными свойствами*. Они определяются классом `Grid`, но задаются для дочерних элементов `Grid`.

Если вы хотите использовать эти вложенные свойства в коде, класс `Grid` предоставляет статические методы с именами `SetRow`, `GetColumn`и т. д. Но в XAML эти вложенные свойства задаются как атрибуты в дочерних элементах `Grid` с помощью имен простых свойств.

Присоединенные свойства являются всегда распознается в файлы XAML как атрибуты, содержащий класс и имя свойства, разделенные точкой. Они называются *присоединенными свойствами* , так как они определены одним классом (в данном случае `Grid`), но присоединены к другим объектам (в данном случае дочерние элементы `Grid`). Во время компоновки `Grid` может опрашивать значения этих вложенных свойств, чтобы понять, куда поместить каждый дочерний элемент.

Класс `AbsoluteLayout` определяет два присоединенных свойства с именами `LayoutBounds` и `LayoutFlags`. Ниже приведен шаблон шахматной доски, реализованный с использованием пропорционального позиционирования и функций изменения размера `AbsoluteLayout`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.AbsoluteDemoPage"
             Title="Absolute Demo Page">

    <AbsoluteLayout BackgroundColor="#FF8080">
        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.33, 0, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="1, 0, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0, 0.33, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.67, 0.33, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.33, 0.67, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="1, 0.67, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0, 1, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.67, 1, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

  </AbsoluteLayout>
</ContentPage>
```

А вот она:

[![абсолютный макет](essential-xaml-syntax-images/absolutedemo-large.png)](essential-xaml-syntax-images/absolutedemo-large.png#lightbox)

Для нечто подобное могут возникнуть вопросы на подсказку с помощью XAML. Конечно, повторение и регулярные `LayoutBounds` прямоугольника предполагает, что он может быть лучше реализован в коде.

Это определенно обоснованная озабоченность, и нет никаких проблем с балансировкой работу с кодом и разметкой, при определении интерфейсов пользователя. Это легко определить определенных визуальных элементов в XAML и затем использовать конструктор файл с выделенным кодом для добавления некоторые дополнительные визуальные элементы, которые могут быть созданы лучше в циклах.

## <a name="content-properties"></a>Свойства содержимого

В предыдущих примерах объекты `StackLayout`, `Grid`и `AbsoluteLayout` задаются свойством `Content` `ContentPage`, а дочерние элементы этих макетов фактически являются элементами в коллекции `Children`. Однако эти свойства `Content` и `Children` нигде не находятся в файле XAML.

В качестве элементов свойств можно включать свойства `Content` и `Children`, например, в примере **ксамлплускоде** :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout.Children>
                <Slider VerticalOptions="CenterAndExpand"
                        ValueChanged="OnSliderValueChanged" />

                <Label x:Name="valueLabel"
                       Text="A simple Label"
                       FontSize="Large"
                       HorizontalOptions="Center"
                       VerticalOptions="CenterAndExpand" />

                <Button Text="Click Me!"
                      HorizontalOptions="Center"
                      VerticalOptions="CenterAndExpand"
                      Clicked="OnButtonClicked" />
            </StackLayout.Children>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Реальный вопрос: почему эти элементы свойств *не* требуются в файле XAML?

Элементы, определенные в Xamarin. Forms для использования в XAML, могут иметь одно свойство, помеченное атрибутом `ContentProperty` класса. Если вы ищете класс `ContentPage` в документации по Xamarin. Forms в Интернете, вы увидите следующий атрибут:

```csharp
[Xamarin.Forms.ContentProperty("Content")]
public class ContentPage : TemplatedPage
```

Это означает, что теги элементов свойств `Content` не являются обязательными. Предполагается, что любое XML-содержимое, отображаемое между тегами Start и End `ContentPage`, назначается свойству `Content`.

 `StackLayout`, `Grid`, `AbsoluteLayout`и `RelativeLayout` все являются производными от `Layout<View>`и при поиске `Layout<T>` в документации по Xamarin. Forms вы увидите другой атрибут `ContentProperty`:

```csharp
[Xamarin.Forms.ContentProperty("Children")]
public abstract class Layout<T> : Layout ...
```

Это позволяет автоматически добавлять содержимое макета в коллекцию `Children` без явного `Children` тегов элементов свойств.

Другие классы также имеют `ContentProperty` определения атрибутов. Например, свойство Content объекта `Label` `Text`. Проверьте документацию по API для других пользователей.

## <a name="platform-differences-with-onplatform"></a>Различия между платформами с OnPlatform

В одностраничных приложениях обычно устанавливается свойство `Padding` на странице, чтобы избежать перезаписи строки состояния iOS. В коде для этой цели можно использовать свойство `Device.RuntimePlatform`:

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    Padding = new Thickness(0, 20, 0, 0);
}
```

Вы также можете сделать что-то подобное в XAML, используя классы [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) и [`On`](xref:Xamarin.Forms.On) . Сначала включите элементы свойств для свойства `Padding` в верхней части страницы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>

    </ContentPage.Padding>
    ...
</ContentPage>
```

Внутри этих тегов добавьте тег `OnPlatform`. `OnPlatform` является универсальным классом. Необходимо указать аргумент универсального типа, в данном случае `Thickness`, который является типом свойства `Padding`. К счастью, существует атрибут XAML, предназначенный специально для определения универсальных аргументов, которые называются `x:TypeArguments`. Это должен совпадать с типом свойства, которое вы изменяете:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">

        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

`OnPlatform` имеет свойство с именем `Platforms`, которое является `IList` объектов `On`. Используйте теги элементов свойства для этого свойства:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <OnPlatform.Platforms>

            </OnPlatform.Platforms>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Теперь добавьте `On` элементы. Для каждого из них установите свойство `Platform` и свойство `Value` в разметку для свойства `Thickness`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <OnPlatform.Platforms>
                <On Platform="iOS" Value="0, 20, 0, 0" />
                <On Platform="Android" Value="0, 0, 0, 0" />
                <On Platform="UWP" Value="0, 0, 0, 0" />
            </OnPlatform.Platforms>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Можно упростить эту разметку. Свойство Content объекта `OnPlatform` `Platforms`, поэтому теги элементов свойств можно удалить:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
            <On Platform="Android" Value="0, 0, 0, 0" />
            <On Platform="UWP" Value="0, 0, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Свойство `Platform` `On` имеет тип `IList<string>`, поэтому можно включить несколько платформ, если значения одинаковы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
            <On Platform="Android, UWP" Value="0, 0, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Так как для Android и UWP задано значение по умолчанию `Padding`, этот тег можно удалить:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Это стандартный способ задания свойства `Padding`, зависящего от платформы, в XAML. Если параметр `Value` не может быть представлен одной строкой, можно определить для него элементы свойств:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS">
                <On.Value>
                    0, 20, 0, 0
                </On.Value>
            </On>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

> [!NOTE]
> Расширение разметки `OnPlatform` также можно использовать в XAML для настройки внешнего вида пользовательского интерфейса на уровне платформы. Он предоставляет те же функциональные возможности, что и классы `OnPlatform` и `On`, но с более кратким представлением. Дополнительные сведения см. в разделе [расширение разметки для платформы](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform).

## <a name="summary"></a>Сводка

С помощью свойств элементов и вложенные свойства большая часть основной синтаксис XAML установлено. Тем не менее иногда необходимо задать свойства к объектам косвенных способом, например, из словаря ресурсов. Этот подход рассматривается в следующей части, часть [3. Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md).

## <a name="related-links"></a>Связанные ссылки

- [ксамлсамплес](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [Часть 1. начало работы с XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Часть 3. Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Часть 4. Основы привязки данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Часть 5. Из привязки данных к MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
