---
title: Задание стиля приложений Xamarin.Forms с помощью каскадных таблиц стилей (CSS)
description: Xamarin. Forms поддерживает визуальные элементы стиля с помощью каскадные таблицы стилей (CSS).
ms.prod: xamarin
ms.assetid: C89D57A6-DAB9-4C42-963F-26D67627DDC2
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 09/19/2019
ms.openlocfilehash: fdee070021b22f82cb69571f0fa2f396831b14e6
ms.sourcegitcommit: 6781967baeed4fe2c58f070476e7c21d01c25c30
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/13/2019
ms.locfileid: "74052806"
---
# <a name="styling-xamarinforms-apps-using-cascading-style-sheets-css"></a>Стилизация приложений Xamarin. Forms с помощью каскадные таблицы стилей (CSS)

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss)

_Xamarin. Forms поддерживает визуальные элементы стиля с помощью каскадные таблицы стилей (CSS)._

Приложения Xamarin. Forms можно использовать в стиле CSS. Таблица стилей состоит из списка правил с каждым правилом, состоящим из одного или нескольких селекторов и блока объявления. Блок объявления состоит из списка объявлений в фигурных скобках с каждым объявлением, состоящим из свойства, двоеточия и значения. При наличии нескольких объявлений в блоке в качестве разделителя вставляется точка с запятой. В следующем примере кода показаны некоторые CSS, совместимые с Xamarin. Forms:

```css
navigationpage {
    -xf-bar-background-color: lightgray;
}

^contentpage {
    background-color: lightgray;
}

#listView {
    background-color: lightgray;
}

stacklayout {
    margin: 20;
}

.mainPageTitle {
    font-style: bold;
    font-size: medium;
}

.mainPageSubtitle {
    margin-top: 15;
}

.detailPageTitle {
    font-style: bold;
    font-size: medium;
    text-align: center;
}

.detailPageSubtitle {
    text-align: center;
    font-style: italic;
}

listview image {
    height: 60;
    width: 60;
}

stacklayout>image {
    height: 200;
    width: 200;
}
```

В Xamarin. Forms таблицы стилей CSS анализируются и оцениваются во время выполнения, а не во время компиляции, а таблицы стилей повторно анализируются при использовании.

> [!NOTE]
> В настоящее время все стили, которые можно реализовать с помощью стиля XAML, не могут быть выполнены с помощью CSS. Однако стили XAML можно использовать для дополнения CSS для свойств, которые в настоящее время не поддерживаются Xamarin. Forms. Дополнительные сведения о стилях XAML см. в руководстве по [оформлению приложений Xamarin.Forms с использованием стилей XAML](~/xamarin-forms/user-interface/styles/xaml/index.md).

В примере [монкэйаппксс](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss) показано использование CSS для стиля простого приложения и показано на следующих снимках экрана:

[![Главная страница Монкэйапп с стилизацией CSS](css-images/MonkeyAppMainPage.png "Главная страница Монкэйапп с стилизацией CSS")](css-images/MonkeyAppMainPage-Large.png#lightbox "Главная страница Монкэйапп с стилизацией CSS")

[![Страница сведений о Монкэйапп с стилизацией CSS](css-images/MonkeyAppDetailPage.png "Страница сведений о Монкэйапп с стилизацией CSS")](css-images/MonkeyAppDetailPage-Large.png#lightbox "Страница сведений о Монкэйапп с стилизацией CSS")

## <a name="consuming-a-style-sheet"></a>Использование таблицы стилей

Процесс добавления таблицы стилей в решение выглядит следующим образом:

1. Добавьте пустой CSS файл в проект библиотеки .NET Standard.
1. Задайте для действия сборки файла CSS значение **EmbeddedResource**.

### <a name="loading-a-style-sheet"></a>Загрузка таблицы стилей

Существует ряд подходов, которые можно использовать для загрузки таблицы стилей.

### <a name="xaml"></a>XAML

Таблицу стилей можно загрузить и проанализировать с помощью класса [`StyleSheet`](xref:Xamarin.Forms.StyleSheets.StyleSheet) , прежде чем добавлять в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary):

```xaml
<Application ...>
    <Application.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </Application.Resources>
</Application>
```

Свойство [`StyleSheet.Source`](xref:Xamarin.Forms.Xaml.StyleSheetExtension.Source) указывает таблицу стилей как URI относительно расположения ВКЛЮЧАЮЩего XAML-файла или относительно корневого каталога проекта, если URI начинается с `/`.

> [!WARNING]
> Если для действия сборки не задано значение **EmbeddedResource**, Загрузка CSS-файла будет невозможна.

Кроме того, таблицу стилей можно загрузить и проанализировать с помощью класса [`StyleSheet`](xref:Xamarin.Forms.StyleSheets.StyleSheet) , прежде чем добавлять его в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)путем встраивания в `CDATA` разделе:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet>
            <![CDATA[
            ^contentpage {
                background-color: lightgray;
            }
            ]]>
        </StyleSheet>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Дополнительные сведения о словарях ресурсов см. в разделе [словари ресурсов](~/xamarin-forms/xaml/resource-dictionaries.md).

### <a name="c"></a>C\#

В C#таблица стилей может быть загружена из `StringReader` и добавлена в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary):

```csharp
public partial class MyPage : ContentPage
{
    public MyPage()
    {
        InitializeComponent();

        using (var reader = new StringReader("^contentpage { background-color: lightgray; }"))
        {
            this.Resources.Add(StyleSheet.FromReader(reader));
        }
    }
}
```

Аргументом метода `StyleSheet.FromReader` является `TextReader`, который считывает таблицу стилей.

## <a name="selecting-elements-and-applying-properties"></a>Выбор элементов и применение свойств

CSS использует селекторы для определения целевых элементов. Стили с соответствующими селекторами применяются последовательно в порядке определения. Стили, определенные для конкретного элемента, всегда применяются последними. Дополнительные сведения о поддерживаемых селекторах см. в разделе [Справочник по селектору](#selector-reference).

В CSS для стиля выбранного элемента используются свойства. Каждое свойство имеет набор возможных значений, а некоторые свойства могут влиять на любой тип элемента, а другие — на группы элементов. Дополнительные сведения о поддерживаемых свойствах см. в разделе [Справочник по свойствам](#property-reference).

### <a name="selecting-elements-by-type"></a>Выбор элементов по типу

Элементы в визуальном дереве можно выбрать по типу с нечувствительным к регистру `element` Selector:

```css
stacklayout {
    margin: 20;
}
```

Этот селектор определяет все элементы [`StackLayout`](xref:Xamarin.Forms.StackLayout) на страницах, использующих таблицу стилей, и устанавливает для их полей однородную толщину в 20 символов.

> [!NOTE]
> Селектор `element` не определяет подклассы указанного типа.

### <a name="selecting-elements-by-base-class"></a>Выбор элементов по базовому классу

Элементы в визуальном дереве могут быть выбраны базовым классом с нечувствительным к регистру `^base` Selector:

```css
^contentpage {
    background-color: lightgray;
}
```

Этот селектор определяет все [`ContentPage`](xref:Xamarin.Forms.ContentPage) элементы, использующие таблицу стилей, и задает для них цвет фона `lightgray`.

> [!NOTE]
> Селектор `^base` зависит от Xamarin. Forms и не является частью спецификации CSS.

### <a name="selecting-an-element-by-name"></a>Выбор элемента по имени

Отдельные элементы в визуальном дереве можно выбрать с помощью селектора `#id` с учетом регистра:

```css
#listView {
    background-color: lightgray;
}
```

Этот селектор определяет элемент, свойство [`StyleId`](xref:Xamarin.Forms.Element.StyleId) которого имеет значение `listView`. Однако если свойство `StyleId` не задано, селектор будет возвращаться к использованию `x:Name` элемента. Таким образом, в следующем примере кода XAML `#listView` селектор определит [`ListView`](xref:Xamarin.Forms.ListView) , атрибут `x:Name` которого имеет значение `listView`, и установит для него цвет фона, равный `lightgray`.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <StackLayout>
        <ListView x:Name="listView" ...>
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

### <a name="selecting-elements-with-a-specific-class-attribute"></a>Выбор элементов с указанным атрибутом класса

Элементы с указанным атрибутом класса можно выбрать с помощью селектора `.class` с учетом регистра:

```css
.detailPageTitle {
    font-style: bold;
    font-size: medium;
    text-align: center;
}

.detailPageSubtitle {
    text-align: center;
    font-style: italic;
}
```

Класс CSS может быть назначен элементу XAML путем присвоения свойству [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass) элемента имени класса CSS. Поэтому в следующем примере XAML стили, определенные классом `.detailPageTitle`, присваиваются первой [`Label`](xref:Xamarin.Forms.Label), а стили, определенные классом `.detailPageSubtitle`, назначаются второму `Label`.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <ScrollView>
        <StackLayout>
            <Label ... StyleClass="detailPageTitle" />
            <Label ... StyleClass="detailPageSubtitle"/>
            ...
        </StackLayout>
    </ScrollView>
</ContentPage>
```

### <a name="selecting-child-elements"></a>Выбор дочерних элементов

Дочерние элементы в визуальном дереве можно выбрать с помощью селектора `element element` без учета регистра:

```css
listview image {
    height: 60;
    width: 60;
}
```

Этот селектор определяет все элементы [`Image`](xref:Xamarin.Forms.Image) , являющиеся дочерними элементами [`ListView`](xref:Xamarin.Forms.ListView) элементов, и устанавливает их высоту и ширину 60. Поэтому в следующем примере кода XAML `listview image` селектор определит [`Image`](xref:Xamarin.Forms.Image) , являющийся дочерним по отношению к [`ListView`](xref:Xamarin.Forms.ListView), и установит высоту и ширину 60.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <StackLayout>
        <ListView ...>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <Grid>
                            ...
                            <Image ... />
                            ...
                        </Grid>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

> [!NOTE]
> Селектор `element element` не требует, чтобы дочерний элемент был _прямым_ дочерним элементом родительского элемента — дочерний элемент может иметь другой родитель. Выполняется выбор, если предком является указанный первый элемент.

### <a name="selecting-direct-child-elements"></a>Выбор прямых дочерних элементов

Прямые дочерние элементы в визуальном дереве можно выбрать с помощью селектора `element>element` без учета регистра:

```css
stacklayout>image {
    height: 200;
    width: 200;
}
```

Этот селектор определяет все элементы [`Image`](xref:Xamarin.Forms.Image) , которые являются прямыми дочерними элементами [`StackLayout`](xref:Xamarin.Forms.StackLayout) элементов, и устанавливает их высоту и ширину 200. Поэтому в следующем примере кода XAML `stacklayout>image` селектор определит [`Image`](xref:Xamarin.Forms.Image) , который является прямым дочерним элементом [`StackLayout`](xref:Xamarin.Forms.StackLayout), и устанавливает высоту и ширину 200.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <ScrollView>
        <StackLayout>
            ...
            <Image ... />
            ...
        </StackLayout>
    </ScrollView>
</ContentPage>
```

> [!NOTE]
> Селектор `element>element` требует, чтобы дочерний элемент был _прямым_ дочерним элементом родительского элемента.

## <a name="selector-reference"></a>Справочник по селектору

В Xamarin. Forms поддерживаются следующие селекторы CSS:

|Селектор|Пример|Описание|
|---|---|---|
|`.class`|`.header`|Выбирает все элементы со свойством `StyleClass`, содержащим "заголовок". Обратите внимание, что в этом селекторе учитывается регистр.|
|`#id`|`#email`|Выбирает все элементы, для которых `StyleId` задано значение `email`. Если параметр `StyleId` не установлен, для перехода на `x:Name`. При использовании XAML `x:Name` предпочтительнее, чем `StyleId`. Обратите внимание, что в этом селекторе учитывается регистр.|
|`*`|`*`|Выбирает все элементы.|
|`element`|`label`|Выбирает все элементы типа `Label`, но не подклассы. Обратите внимание, что этот селектор не учитывает регистр.|
|`^base`|`^contentpage`|Выбирает все элементы с `ContentPage` в качестве базового класса, включая `ContentPage` самого себя. Обратите внимание, что этот селектор не учитывает регистр и не является частью спецификации CSS.|
|`element,element`|`label,button`|Выбирает все элементы `Button` и все элементы `Label`. Обратите внимание, что этот селектор не учитывает регистр.|
|`element element`|`stacklayout label`|Выбирает все элементы `Label` внутри `StackLayout`. Обратите внимание, что этот селектор не учитывает регистр.|
|`element>element`|`stacklayout>label`|Выбирает все элементы `Label` с `StackLayout` в качестве прямого родителя. Обратите внимание, что этот селектор не учитывает регистр.|
|`element+element`|`label+entry`|Выбирает все элементы `Entry` непосредственно после `Label`. Обратите внимание, что этот селектор не учитывает регистр.|
|`element~element`|`label~entry`|Выбирает все элементы `Entry`, предшествующие `Label`. Обратите внимание, что этот селектор не учитывает регистр.|

Стили с соответствующими селекторами применяются последовательно в порядке определения. Стили, определенные для конкретного элемента, всегда применяются последними.

> [!TIP]
> Селекторы можно сочетать без ограничений, например `StackLayout>ContentView>label.email`.

Следующие селекторы в настоящее время не поддерживаются:

- `[attribute]`
- `@media` и `@supports`.
- `:` и `::`.

> [!NOTE]
> Определение приоритетности и переопределений с определенными параметрами не поддерживаются.

## <a name="property-reference"></a>Справочник по свойствам

Xamarin. Forms поддерживают следующие свойства CSS (в столбце « **значения** » типы имеют _курсив_, а строковые литералы — `gray`):

|свойство;|Применение|Значения|Пример|
|---|---|---|---|
|`align-content`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \| `initial` |`align-content: space-between;`|
|`align-items`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `flex-start` \| 0 1 2 |`align-items: flex-start;`|
|`align-self`|`VisualElement`| `auto` \| `stretch` \| `center` \| `start` \| `end` \| 0 1 2 3 4|`align-self: flex-end;`|
|`background-color`|`VisualElement`|_цветовая_ \| `initial` |`background-color: springgreen;`|
|`background-image`|`Page`|_строка_ \| `initial` |`background-image: bg.png;`|
|`border-color`|`Button`значение `Frame`значение `ImageButton`|_цветовая_ \| `initial`|`border-color: #9acd32;`|
|`border-radius`|`BoxView`, `Button`, `Frame`, `ImageButton`|_двойной_ \| `initial` |`border-radius: 10;`|
|`border-width`|`Button`, `ImageButton`|_двойной_ \| `initial` |`border-width: .5;`|
|`color`|`ActivityIndicator`, `BoxView`, `Button`, `CheckBox`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `ProgressBar`, `SearchBar`, `Switch`, `TimePicker`|_цветовая_ \| `initial` |`color: rgba(255, 0, 0, 0.3);`|
|`column-gap`|`Grid`|_двойной_ \| `initial`|`column-gap: 9;`|
|`direction`|`VisualElement`|`ltr` \| `rtl` \| `inherit` \| `initial` |`direction: rtl;`|
|`flex-direction`|`FlexLayout`| `column` \| `columnreverse` \| `row` \| `rowreverse` \| `row-reverse` \| 0 1 2|`flex-direction: column-reverse;`|
|`flex-basis`|`VisualElement`|\| с _плавающей запятой_ `auto` \| `initial`. Кроме того, значение в процентах от 0% до 100% можно указать с помощью `%` знака.|`flex-basis: 25%;`|
|`flex-grow`|`VisualElement`|`initial` \| с _плавающей запятой_|`flex-grow: 1.5;`|
|`flex-shrink`|`VisualElement`|`initial` \| с _плавающей запятой_|`flex-shrink: 1;`|
|`flex-wrap`|`VisualElement`| `nowrap` \| `wrap` \| `reverse` \| `wrap-reverse` \| `initial`|`flex-wrap: wrap-reverse;`|
|`font-family`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_строка_ \| `initial` |`font-family: Consolas;`|
|`font-size`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_double_ \| _намедсизе_ \| `initial` |`font-size: 12;`|
|`font-style`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|`bold` \| `italic` \| `initial` |`font-style: bold;`|
|`height`|`VisualElement`|_двойной_ \| `initial` |`min-height: 250;`|
|`justify-content`|`FlexLayout`| `start` \| `center` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \| `initial`|`justify-content: flex-end;`|
|`line-height`|`Label`, `Span`|_двойной_ \| `initial` |`line-height: 1.8;`|
|`margin`|`View`|_толщина_ \| `initial` |`margin: 6 12;`|
|`margin-left`|`View`|_толщина_ \| `initial` |`margin-left: 3;`|
|`margin-top`|`View`|_толщина_ \| `initial` |`margin-top: 2;`|
|`margin-right`|`View`|_толщина_ \| `initial` |`margin-right: 1;`|
|`margin-bottom`|`View`|_толщина_ \| `initial` |`margin-bottom: 6;`|
|`max-lines`|`Label`|`initial` \| _int_|`max-lines: 2;`|
|`min-height`|`VisualElement`|_двойной_ \| `initial` |`min-height: 50;`|
|`min-width`|`VisualElement`|_двойной_ \| `initial` |`min-width: 112;`|
|`opacity`|`VisualElement`|_двойной_ \| `initial` |`opacity: .3;`|
|`order`|`VisualElement`|`initial` \| _int_|`order: -1;`|
|`padding`|`Button`, `ImageButton`, `Layout`, `Page`|_толщина_ \| `initial` |`padding: 6 12 12;`|
|`padding-left`|`Button`, `ImageButton`, `Layout`, `Page`|_двойной_ \| `initial`|`padding-left: 3;`|
|`padding-top`|`Button`, `ImageButton`, `Layout`, `Page`| _двойной_ \| `initial` |`padding-top: 4;`|
|`padding-right`|`Button`, `ImageButton`, `Layout`, `Page`| _двойной_ \| `initial` |`padding-right: 2;`|
|`padding-bottom`|`Button`, `ImageButton`, `Layout`, `Page`| _двойной_ \| `initial` |`padding-bottom: 6;`|
|`position`|`FlexLayout`| `relative` \| `absolute` \| `initial`|`position: absolute;`|
|`row-gap`|`Grid`| _двойной_ \| `initial`|`row-gap: 12;`|
|`text-align`| `Entry`, `EntryCell`, `Label`, `SearchBar`|`left` \| `top` \| `right` \| `bottom` \| `start` \| 0 1 2 3 4 5 6. в средах с письмом справа налево следует избегать `left` и `right`.| `text-align: right;`|
|`text-decoration`|`Label`, `Span`|`none` \| `underline` \| `strikethrough` \| `line-through` \| `initial`|`text-decoration: underline, line-through;`|
|`transform`|`VisualElement`| `none`, `rotate`, `rotateX`, `rotateY`, `scale`, `scaleX`, `scaleY`, `translate`, `translateX`, `translateY`, `initial` |`transform: rotate(180), scaleX(2.5);`|
|`transform-origin`|`VisualElement`| _Double_, _Двойное_ \| `initial` |`transform-origin: 7.5, 12.5;`|
|`vertical-align`|`Label`|`left` \| `top` \| `right` \| `bottom` \| `start` \| 0 1 2 3 4 5 6|`vertical-align: bottom;`|
|`visibility`|`VisualElement`|`true` \| `visible` \| `false` \| `hidden` \| `collapse` \| 0|`visibility: hidden;`|
|`width`|`VisualElement`|_двойной_ \| `initial`|`min-width: 320;`|

> [!NOTE]
> `initial` является допустимым значением для всех свойств. Он очищает значение (сбрасывается до значения по умолчанию), установленное из другого стиля.

Следующие свойства в настоящее время не поддерживаются:

- `all: initial`
- Свойства макета (Box или Grid).
- Сокращенные свойства, такие как `font` и `border`.

Кроме того, отсутствует `inherit` значение, поэтому наследование не поддерживается. Поэтому вы не можете, например, задать свойство `font-size` в макете и ожидает, что все экземпляры [`Label`](xref:Xamarin.Forms.Label) в макете наследуют это значение. Единственным исключением является свойство `direction`, которое имеет значение по умолчанию `inherit`.

Целевые элементы `Span` имеют известную ошибку, препятствующую нацеливанию стилей CSS на элемент и имя (с помощью `#` символа). Элемент `Span` является производным от `GestureElement`, у которого нет свойства `StyleClass`, поэтому диапазоны не поддерживают целевые классы CSS. Дополнительные сведения см. в разделе [не удается применить стилизацию CSS для управления охватом](https://github.com/xamarin/Xamarin.Forms/issues/5979).

### <a name="xamarinforms-specific-properties"></a>Свойства, относящиеся к Xamarin. Forms

Также поддерживаются следующие свойства CSS, относящиеся к Xamarin. Forms (в столбце **значения** типы имеют _курсив_, а строковые литералы `gray`):

|свойство;|Применение|Значения|Пример|
|---|---|---|---|
|`-xf-bar-background-color`|`NavigationPage`, `TabbedPage`|_цветовая_ \| `initial` |`-xf-bar-background-color: teal;`|
|`-xf-bar-text-color`|`NavigationPage`, `TabbedPage`|_цветовая_ \| `initial` |`-xf-bar-text-color: gray`|
|`-xf-horizontal-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-horizontal-scroll-bar-visibility: never;`|
|`-xf-max-length`|`Entry`, `Editor`|`initial` \| _int_ |`-xf-max-length: 20;`|
|`-xf-max-track-color`|`Slider`|_цветовая_ \| `initial` |`-xf-max-track-color: red;`|
|`-xf-min-track-color`|`Slider`|_цветовая_ \| `initial` |`-xf-min-track-color: yellow;`|
|`-xf-orientation`|`ScrollView`, `StackLayout`| `horizontal` \| `vertical` \| `both` \| `initial`. `both` поддерживается только в `ScrollView`. |`-xf-orientation: horizontal;`|
|`-xf-placeholder`|`Entry`значение `Editor`значение `SearchBar`|`initial` \| _текста в кавычках_ |`-xf-placeholder: Enter name;`|
|`-xf-placeholder-color`|`Entry`значение `Editor`значение `SearchBar`|_цветовая_ \| `initial` |`-xf-placeholder-color: green;`|
|`-xf-spacing`|`StackLayout`|_двойной_ \| `initial` |`-xf-spacing: 8;`|
|`-xf-thumb-color`|`Slider`, `Switch`|_цветовая_ \| `initial` |`-xf-thumb-color: limegreen;`|
|`-xf-vertical-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-vertical-scroll-bar-visibility: always;`|
|`-xf-vertical-text-alignment`|`Label`| `start` \| `center` \| `end` \| `initial`|`-xf-vertical-text-alignment: end;`|
|`-xf-visual`|`VisualElement`|_строка_ \| `initial` |`-xf-visual: material;`|

### <a name="xamarinforms-shell-specific-properties"></a>Специальные свойства оболочки Xamarin. Forms

Также поддерживаются следующие свойства CSS, относящиеся к оболочке Xamarin. Forms (в столбце **значения** типы имеют _курсив_, а строковые литералы `gray`):

|свойство;|Применение|Значения|Пример|
|---|---|---|---|
|`-xf-flyout-background`|`Shell`|_цветовая_ \| `initial` |`-xf-flyout-background: red;`|
|`-xf-shell-background`|`Element`|_цветовая_ \| `initial` |`-xf-shell-background: green;`|
|`-xf-shell-disabled`|`Element`|_цветовая_ \| `initial` |`-xf-shell-disabled: blue;`|
|`-xf-shell-foreground`|`Element`|_цветовая_ \| `initial` |`-xf-shell-foreground: yellow;`|
|`-xf-shell-tabbar-background`|`Element`|_цветовая_ \| `initial` |`-xf-shell-tabbar-background: white;`|
|`-xf-shell-tabbar-disabled`|`Element`|_цветовая_ \| `initial` |`-xf-shell-tabbar-disabled: black;`|
|`-xf-shell-tabbar-foreground`|`Element`|_цветовая_ \| `initial` |`-xf-shell-tabbar-foreground: gray;`|
|`-xf-shell-tabbar-title`|`Element`|_цветовая_ \| `initial` |`-xf-shell-tabbar-title: lightgray;`|
|`-xf-shell-tabbar-unselected`|`Element`|_цветовая_ \| `initial` |`-xf-shell-tabbar-unselected: cyan;`|
|`-xf-shell-title`|`Element`|_цветовая_ \| `initial` |`-xf-shell-title: teal;`|
|`-xf-shell-unselected`|`Element`|_цветовая_ \| `initial` |`-xf-shell-unselected: limegreen;`|

### <a name="color"></a>Цвет

Поддерживаются следующие значения `color`:

- `X11` [цвета](https://en.wikipedia.org/wiki/X11_color_names/), соответствующие цветам CSS, предварительно определенным цветам UWP и цветам Xamarin. Forms. Обратите внимание, что в этих цветовых значениях регистр не учитывается.
- Шестнадцатеричные цвета: `#rgb`, `#argb`, `#rrggbb`, `#aarrggbb`
- цвета RGB: `rgb(255,0,0)`, `rgb(100%,0%,0%)`. Значения находятся в диапазоне 0-255 или 0%-100%.
- цвета RGBA: `rgba(255, 0, 0, 0.8)`, `rgba(100%, 0%, 0%, 0.8)`. Значение непрозрачности находится в диапазоне от 0,0 до 1,0.
- цвета HSL: `hsl(120, 100%, 50%)`. Значение h находится в диапазоне 0-360, а s и l — в диапазоне 0%-100%.
- цвета хсла: `hsla(120, 100%, 50%, .8)`. Значение непрозрачности находится в диапазоне от 0,0 до 1,0.

### <a name="thickness"></a>Thickness

Поддерживаются одно, два, три или четыре значения `thickness`, каждое из которых отделяется пробелом:

- Одно значение обозначает однородную толщину.
- Два значения указывают вертикальную толщину по горизонтали.
- Три значения указываются сверху, затем горизонтально (слева и справа), затем толщина нижней части.
- Четыре значения указываются сверху, справа, снизу, затем слева.

> [!NOTE]
> Значения `thickness` CSS отличаются от значений [`Thickness`](xref:Xamarin.Forms.Thickness) XAML. Например, в XAML `Thickness` с двумя значениями обозначает горизонтальную и вертикальную толщину, а `Thickness` четыре значения — слева, сверху, справа и снизу. Кроме того, значения XAML `Thickness` разделяются запятыми.

### <a name="namedsize"></a>намедсизе

Поддерживаются следующие `namedsize` значения без учета регистра:

- `default`
- `micro`
- `small`
- `medium`
- `large`

Точное значение каждого `namedsize` значения зависит от платформы и от представления.

## <a name="css-in-xamarinforms-with-xamarinuniversity"></a>CSS в Xamarin. Forms с Xamarin. Университет

> [!VIDEO https://youtube.com/embed/va-Vb7vtan8]

**Видео о CSS Xamarin. Forms 3,0**

## <a name="related-links"></a>Связанные ссылки

- [Монкэйаппксс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss)
- [Словари ресурсов](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Задание стиля приложений Xamarin.Forms с помощью стилей XAML](~/xamarin-forms/user-interface/styles/xaml/index.md)
