---
title: ContentView Xamarin. Forms
description: В этой статье объясняется, как использовать класс ContentView для создания пользовательского элемента управления, такого как пример Кардвиев.
ms.prod: xamarin
ms.assetid: 638402E7-CA44-456B-863B-791F6B6B561D
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/14/2019
ms.openlocfilehash: e340b45148c7528eff1aa511ee9902a4ac2658c0
ms.sourcegitcommit: 9178e2e689f027212ea3e623b556b312985d79fe
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/21/2019
ms.locfileid: "69658161"
---
# <a name="xamarinforms-contentview"></a>ContentView Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/en-us/samples/xamarin/xamarin-forms-samples/userinterface-cardview/)

Класс Xamarin. Forms [`ContentView`](xref:Xamarin.Forms.ContentView) является `Layout` типом, который содержит один дочерний элемент и обычно используется для создания настраиваемых, многократно используемых элементов управления. Класс наследует от [`TemplatedView`.](xref:Xamarin.Forms.TemplatedView) `ContentView` В этой статье и связанном примере объясняется, как создать пользовательский `CardView` элемент управления на основе `ContentView` класса.

На следующем снимке экрана `CardView` показан элемент управления, производный `ContentView` от класса:

[![Снимок экрана примера приложения Кардвиев](contentview-images/cardview-list-cropped.png)](contentview-images/cardview-list.png#lightbox)

`ContentView` Класс определяет одно свойство:

* [`Content`](xref:Xamarin.Forms.ContentView.Content)— Это `View` объект. Это свойство поддерживается [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектом, поэтому он может быть целевым объектом привязок данных.

Объект `ContentView` также наследует свойство `TemplatedView` от класса:

* [`ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate)Объект `ControlTemplate` , который может определять или переопределять внешний вид элемента управления.

Дополнительные сведения о `ControlTemplate` свойстве см. в разделе [Настройка внешнего вида с помощью ControlTemplate](#customize-appearance-with-a-controltemplate).

## <a name="create-a-custom-control"></a>Создание пользовательского элемента управления

`ContentView` Класс предлагает небольшую функциональность, но может использоваться для создания пользовательского элемента управления. Пример проекта определяет `CardView` элемент управления — элемент пользовательского интерфейса, который отображает изображение, заголовок и описание в макете, похожем на карту.

Процесс создания пользовательского элемента управления состоит в следующих целях.

1. Создайте новый класс с помощью `ContentView` шаблона в Visual Studio 2019.
1. Определите уникальные свойства или события в файле кода программной части для нового пользовательского элемента управления.
1. Создайте пользовательский интерфейс для пользовательского элемента управления.

> [!NOTE]
> Можно создать пользовательский элемент управления, макет которого определяется в коде вместо XAML. Для простоты пример приложения определяет только один `CardView` класс с макетом XAML. Однако пример приложения содержит класс **кардвиевкодепаже** , который показывает процесс использования пользовательского элемента управления в коде.

### <a name="create-code-behind-properties"></a>Создание свойств кода программной части

`CardView` Пользовательский элемент управления определяет следующие свойства:

* `CardTitle``string` : объект, представляющий заголовок, отображаемый на карточке.
* `CardDescription``string` : объект, представляющий описание, отображаемое на карточке.
* `IconImageSource``ImageSource` : объект, представляющий изображение, отображаемое на карточке.
* `IconBackgroundColor``Color` : объект, представляющий цвет фона для изображения, отображаемого на карточке.
* `BorderColor``Color` : объект, представляющий цвет границы карточки, границы изображения и линии разделителя.
* `CardColor``Color` : объект, представляющий цвет фона карточки.

> [!NOTE]
> `BorderColor` Свойство влияет на несколько элементов в целях демонстрации. При необходимости это свойство можно разделить на три свойства.

Каждое свойство поддерживается `BindableProperty` экземпляром. Резервное копирование `BindableProperty` позволяет создавать и привязывать каждое свойство к стилю с помощью шаблона MVVM. Дополнительные сведения см. в статье [BIND Data with MVVM](#bind-data-with-mvvm).

В следующем примере показано, как создать резервную копию `BindableProperty`:

```csharp
public static readonly BindableProperty CardTitleProperty = BindableProperty.Create(
    "CardTitle",        // the name of the bindable property
    typeof(string),     // the bindable property type
    typeof(CardView),   // the parent object type
    string.Empty);      // the default value for the property
```

Пользовательское свойство использует `GetValue` методы и `SetValue` `BindableProperty` для получения и задания значений объекта:

```csharp
public string CardTitle
{
    get => (string)GetValue(CardView.CardTitleProperty);
    set => SetValue(CardView.CardTitleProperty, value);
}
```

Дополнительные сведения об `BindableProperty` объектах см. в разделе [свойства, допускающие привязку](~/xamarin-forms/xaml/bindable-properties.md).

### <a name="define-ui"></a>Определение пользовательского интерфейса

Пользовательский интерфейс пользовательского элемента управления использует `ContentView` в качестве корневого элемента `CardView` для элемента управления. В следующем примере показан `CardView` XAML:

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
            <Frame BorderColor="{Binding BorderColor}"
                   BackgroundColor="{Binding IconBackgroundColor}"
                   ...>
                <Image Source="{Binding IconImageSource}"
                       .. />
            </Frame>
            <Label Text="{Binding CardTitle}"
                   ... />
            <BoxView BackgroundColor="{Binding BorderColor}"
                     ... />
            <Label Text="{Binding CardDescription}"
                   ... />
        </Grid>
    </Frame>
</ContentView>
```

Элемент задает для `CardView`свойствазначение this, которое можно использовать для доступа к объекту, привязанному к экземпляру. `ContentView` `x:Name` Элементы в привязках набора макетов для своих свойств к значениям, определенным для привязанного объекта.

Дополнительные сведения о привязке данных см. в разделе [Привязки данных в Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md).

## <a name="instantiate-a-custom-control"></a>Создание экземпляра пользовательского элемента управления

Ссылка на пространство имен пользовательского элемента управления должна быть добавлена на страницу, которая создает экземпляр пользовательского элемента управления. В следующем примере показана ссылка на пространство имен под названием `ContentPage` **элементы управления** , добавленные в экземпляр в XAML:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CardViewDemo.Controls" >
```

После добавления `CardView` ссылки можно создать экземпляр в XAML и определить его свойства:

```xaml
<controls:CardView BorderColor="DarkGray"
                   CardTitle="Slavko Vlasic"
                   CardDescription="Lorem ipsum dolor sit..."
                   IconBackgroundColor="SlateGray"
                   IconImageSource="user.png"/>
```

Также `CardView` можно создать экземпляр в коде:

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

### <a name="bind-data-with-mvvm"></a>Привязка данных с помощью MVVM

`BindableProperty` Объекты`CardView` в классе разрешают привязки стилей Model-View-ViewModel (MVVM). Пример приложения содержит `PersonCollectionViewModel` класс, определяющий одно свойство коллекции:

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{
    ...
    public List<PersonViewModel> Items
    {
        get
        {
            return items;
        }
        set
        {
            items = value;
            NotifyPropertyChanged();
        }
    }
    ...
}
```

`PersonViewModel` Класс представляет личный профиль:

```csharp
public class PersonViewModel : INotifyPropertyChanged
{
    ...

    string photo;
    public string Photo
    {
        get
        {
            return photo;
        }
        set
        {
            photo = value;
            NotifyPropertyChanged();
        }
    }

    string name;
    public string Name
    {
        get
        {
            return name;
        }
        set
        {
            name = value;
            NotifyPropertyChanged();
        }
    }

    string bio;
    public string Bio
    {
        get
        {
            return bio;
        }
        set
        {
            bio = value;
            NotifyPropertyChanged();
        }
    }
    ...
}
```

Можно использовать для отображения `PersonViewModel` коллекции объектов в виде списка карточек. `CardView` В следующем примере показано, как привязать `PersonViewCollection` экземпляр `StackLayout` к экземпляру в XAML:

```xaml
<StackLayout HorizontalOptions="Fill"
             VerticalOptions="Fill"
             BindableLayout.ItemsSource="{Binding Items}">
    <BindableLayout.ItemTemplate>
        <DataTemplate>
            <controls:CardView Margin="4"
                               BorderColor="DarkGray"
                               IconBackgroundColor="SlateGray"
                               BindingContext="{Binding .}"
                               CardTitle="{Binding Name}"
                               CardDescription="{Binding Bio}"
                               IconImageSource="{Binding Photo}"/>
        </DataTemplate>
    </BindableLayout.ItemTemplate>
</StackLayout>
```

`Items` Свойство в`PersonViewCollection` экземплярепривязанокспомощьюмакета`StackLayout` с возможностью привязки. Определяет внешний вид каждого `CardView` объекта и привязывает данные к свойствам в `PersonViewModel`. `DataTemplate` Если задано значение `CardView` `PersonView` ,`Items` для каждого объекта в коллекции будет создан объект. `BindingContext` `BindingContext` Задается, как показано в следующем примере:

```csharp
public partial class CardViewMvvmPage : ContentPage
{
    public CardViewMvvmPage()
    {
        InitializeComponent();
        BindingContext = DataService.GetPersonCollection();
    }
}
```

Дополнительные сведения о привязке данных см. в разделе [Привязка данных в Xamarin. Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md). Дополнительные сведения об `BindableProperty` объектах см. в разделе [свойства, допускающие привязку](~/xamarin-forms/xaml/bindable-properties.md).

## <a name="customize-appearance-with-a-controltemplate"></a>Настройка внешнего вида с помощью ControlTemplate

Пользовательский элемент управления, производный от `ContentView` класса, может определять внешний вид с помощью XAML, кода или вообще не определять внешний вид. Независимо от того, как определен внешний вид, `ControlTemplate` объект может переопределить внешний вид с помощью пользовательского макета.

`CardView` Макет может занимать слишком много пространства для некоторых вариантов использования. `ControlTemplate` Может`CardView` переопределить макет, чтобы обеспечить более компактное представление, подходящее для уплотненного списка:

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

Привязка данных в `ControlTemplate` `TemplateBinding` использует расширение разметки для указания привязок. Для свойства можно задать определенный объект ControlTemplate, используя его `x:Key` значение. `ControlTemplate` В следующем примере показано свойство `ControlTemplate` , заданное `CardView` для экземпляра.

```xaml
<controls:CardView ControlTemplate="{StaticResource CardViewCompressed}"/>
```

На следующих снимках экрана показан `CardView` стандартный экземпляр `CardView` `ControlTemplate` , который был переопределен:

[![Снимок экрана ControlTemplate Кардвиев](contentview-images/cardview-controltemplates-cropped.png)](contentview-images/cardview-controltemplates.png#lightbox)

Дополнительные сведения о шаблонах элементов управления см. в разделе [шаблоны элементов управления Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md).

## <a name="related-links"></a>Связанные ссылки

* [Пример приложения Кардвиев](https://docs.microsoft.com/en-us/samples/xamarin/xamarin-forms-samples/userinterface-cardview/)
* [Привязка данных Xamarin. Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md)
* [Привязываемые свойства](~/xamarin-forms/xaml/bindable-properties.md).
* [Шаблоны элементов управления Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md)
