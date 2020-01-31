---
title: ContentView Xamarin. Forms
description: В этой статье объясняется, как использовать класс ContentView для создания пользовательского элемента управления, такого как пример Кардвиев.
ms.prod: xamarin
ms.assetid: 638402E7-CA44-456B-863B-791F6B6B561D
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/14/2019
ms.openlocfilehash: 712aa43fb4959b766786c8fd0969ef2c2c8f00ef
ms.sourcegitcommit: 211fed94fb96127a3e158ae1ff5d7eb831a203d8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/15/2020
ms.locfileid: "75955736"
---
# <a name="xamarinforms-contentview"></a>ContentView Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-contentviewdemos/)

Класс [`ContentView`](xref:Xamarin.Forms.ContentView) Xamarin. Forms — это тип `Layout`, который содержит один дочерний элемент и обычно используется для создания настраиваемых, многократно используемых элементов управления. Класс `ContentView` наследует от [`TemplatedView`](xref:Xamarin.Forms.TemplatedView). В этой статье и связанном примере объясняется, как создать пользовательский элемент управления `CardView` на основе класса `ContentView`.

На следующем снимке экрана показан элемент управления `CardView`, производный от класса `ContentView`:

[![снимок экрана примера приложения Кардвиев](contentview-images/cardview-list-cropped.png)](contentview-images/cardview-list.png#lightbox)

Класс `ContentView` определяет одно свойство:

* [`Content`](xref:Xamarin.Forms.ContentView.Content) является объектом `View`. Это свойство поддерживается объектом [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) , поэтому он может быть целевым объектом привязок данных.

`ContentView` также наследует свойство от класса `TemplatedView`:

* [`ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate) — это `ControlTemplate`, который может определять или переопределять внешний вид элемента управления.

Дополнительные сведения о свойстве `ControlTemplate` см. в разделе [Настройка внешнего вида с помощью ControlTemplate](#customize-appearance-with-a-controltemplate).

## <a name="create-a-custom-control"></a>Создание пользовательского элемента управления

Класс `ContentView` предлагает небольшую функциональность, но может использоваться для создания пользовательского элемента управления. В примере проекта определяется `CardView` элемент управления — элемент пользовательского интерфейса, который отображает изображение, заголовок и описание в макете, похожем на карту.

Процесс создания пользовательского элемента управления состоит в следующих целях.

1. Создайте новый класс с помощью шаблона `ContentView` в Visual Studio 2019.
1. Определите уникальные свойства или события в файле кода программной части для нового пользовательского элемента управления.
1. Создайте пользовательский интерфейс для пользовательского элемента управления.

> [!NOTE]
> Можно создать пользовательский элемент управления, макет которого определяется в коде вместо XAML. Для простоты пример приложения определяет только один класс `CardView` с макетом XAML. Однако пример приложения содержит класс **кардвиевкодепаже** , который показывает процесс использования пользовательского элемента управления в коде.

### <a name="create-code-behind-properties"></a>Создание свойств кода программной части

Пользовательский элемент управления `CardView` определяет следующие свойства:

* `CardTitle`: объект `string`, представляющий заголовок, отображаемый на карточке.
* `CardDescription`: объект `string`, представляющий описание, отображаемое на карточке.
* `IconImageSource`: объект `ImageSource`, представляющий изображение, отображаемое на карточке.
* `IconBackgroundColor`: объект `Color`, представляющий цвет фона для изображения, отображаемого на карточке.
* `BorderColor`: объект `Color`, представляющий цвет границы карточки, границы изображения и линии разделителя.
* `CardColor`: объект `Color`, представляющий цвет фона карточки.

> [!NOTE]
> Свойство `BorderColor` влияет на несколько элементов в целях демонстрации. При необходимости это свойство можно разделить на три свойства.

Каждое свойство поддерживается экземпляром `BindableProperty`. Резервный `BindableProperty` позволяет применять к каждому свойству стили и привязывать их с помощью шаблона MVVM.

В следующем примере показано, как создать резервную `BindableProperty`.

```csharp
public static readonly BindableProperty CardTitleProperty = BindableProperty.Create(
    "CardTitle",        // the name of the bindable property
    typeof(string),     // the bindable property type
    typeof(CardView),   // the parent object type
    string.Empty);      // the default value for the property
```

Пользовательское свойство использует методы `GetValue` и `SetValue` для получения и задания значений `BindableProperty` объектов:

```csharp
public string CardTitle
{
    get => (string)GetValue(CardView.CardTitleProperty);
    set => SetValue(CardView.CardTitleProperty, value);
}
```

Дополнительные сведения об объектах `BindableProperty` см. в разделе [свойства, допускающие привязку](~/xamarin-forms/xaml/bindable-properties.md).

### <a name="define-ui"></a>Определение пользовательского интерфейса

Пользовательский интерфейс пользовательского элемента управления использует `ContentView` в качестве корневого элемента для элемента управления `CardView`. В следующем примере показан `CardView` XAML:

```XAML
<ContentView ...
             x:Name="this"
             x:Class="CardViewDemo.Controls.CardView">
    <Frame BindingContext="{x:Reference this}"
            BackgroundColor="{Binding CardColor}"
            BorderColor="{Binding BorderColor}"
            ...>
        <Grid>
            ...
            <Frame BorderColor="{Binding BorderColor, FallbackValue='Black'}"
                   BackgroundColor="{Binding IconBackgroundColor, FallbackValue='Grey'}"
                   ...>
                <Image Source="{Binding IconImageSource}"
                       .. />
            </Frame>
            <Label Text="{Binding CardTitle, FallbackValue='Card Title'}"
                   ... />
            <BoxView BackgroundColor="{Binding BorderColor, FallbackValue='Black'}"
                     ... />
            <Label Text="{Binding CardDescription, FallbackValue='Card description text.'}"
                   ... />
        </Grid>
    </Frame>
</ContentView>
```

Элемент `ContentView` задает для свойства `x:Name` значение **this**, которое можно использовать для доступа к объекту, привязанному к экземпляру `CardView`. Элементы в привязках набора макетов для своих свойств к значениям, определенным для привязанного объекта.

Дополнительные сведения о привязке данных см. в разделе [Привязки данных в Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md).

> [!NOTE]
> Свойство `FallbackValue` предоставляет значение по умолчанию, если привязка `null`. Это также позволяет средству [предварительного просмотра XAML](~/xamarin-forms/xaml/xaml-previewer/index.md) в Visual Studio визуализировать элемент управления `CardView`.

## <a name="instantiate-a-custom-control"></a>Создание экземпляра пользовательского элемента управления

Ссылка на пространство имен пользовательского элемента управления должна быть добавлена на страницу, которая создает экземпляр пользовательского элемента управления. В следующем примере показана ссылка на пространство имен под названием **элементы управления** , добавленные в экземпляр `ContentPage` в XAML:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CardViewDemo.Controls" >
```

После добавления ссылки на `CardView` можно создать экземпляр в XAML и определить его свойства:

```xaml
<controls:CardView BorderColor="DarkGray"
                   CardTitle="Slavko Vlasic"
                   CardDescription="Lorem ipsum dolor sit..."
                   IconBackgroundColor="SlateGray"
                   IconImageSource="user.png"/>
```

В коде также можно создать экземпляр `CardView`:

```csharp
CardView card = new CardView
{
    BorderColor = Color.DarkGray,
    CardTitle = "Slavko Vlasic",
    CardDescription = "Lorem ipsum dolor sit...",
    IconBackgroundColor = Color.SlateGray,
    IconImageSource = ImageSource.FromFile("user.png")
};
```

## <a name="customize-appearance-with-a-controltemplate"></a>Настройка внешнего вида с помощью ControlTemplate

Пользовательский элемент управления, производный от класса `ContentView`, может определять внешний вид с помощью XAML, Code или вообще не определять внешний вид. Независимо от того, как определен внешний вид, объект `ControlTemplate` может переопределить внешний вид с помощью пользовательского макета.

Макет `CardView` может занимать слишком много пространства для некоторых вариантов использования. `ControlTemplate` может переопределить макет `CardView`, чтобы обеспечить более компактное представление, подходящее для уплотненного списка:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <ControlTemplate x:Key="CardViewCompressed">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="100" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="100" />
                    <ColumnDefinition Width="100*" />
                </Grid.ColumnDefinitions>
                <Image Grid.Row="0"
                       Grid.Column="0"
                       Source="{TemplateBinding IconImageSource}"
                       BackgroundColor="{TemplateBinding IconBackgroundColor}"
                       WidthRequest="100"
                       HeightRequest="100"
                       Aspect="AspectFill"
                       HorizontalOptions="Center"
                       VerticalOptions="Center"/>
                <StackLayout Grid.Row="0"
                             Grid.Column="1">
                    <Label Text="{TemplateBinding CardTitle}"
                           FontAttributes="Bold" />
                    <Label Text="{TemplateBinding CardDescription}" />
                </StackLayout>
            </Grid>
        </ControlTemplate>
    </ResourceDictionary>
</ContentPage.Resources>
```

Привязка данных в `ControlTemplate` использует расширение разметки `TemplateBinding` для указания привязок. В качестве значения свойства `ControlTemplate` можно задать определенный объект ControlTemplate, используя его значение `x:Key`. В следующем примере показано свойство `ControlTemplate`, заданное для экземпляра `CardView`.

```xaml
<controls:CardView ControlTemplate="{StaticResource CardViewCompressed}"/>
```

На следующих снимках экрана показан стандартный экземпляр `CardView` и `CardView`, чьи `ControlTemplate` были переопределены:

[![снимок экрана Кардвиев ControlTemplate ](contentview-images/cardview-controltemplates-cropped.png)](contentview-images/cardview-controltemplates.png#lightbox)

Дополнительные сведения о шаблонах элементов управления см. в разделе [Шаблоны элементов управления Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/control-template.md).

## <a name="related-links"></a>Связанные ссылки

* [Пример приложения ContentView](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-contentviewdemos/)
* [Привязка данных Xamarin. Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md)
* [Привязываемые свойства](~/xamarin-forms/xaml/bindable-properties.md).
* [Шаблоны элементов управления Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/control-template.md)
