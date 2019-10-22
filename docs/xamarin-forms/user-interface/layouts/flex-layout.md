---
title: Флекслайаут Xamarin. Forms
description: Используйте Флекслайаут для наложения или упаковки коллекции дочерних представлений.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 187befd88c115133a92aa90a711438e7754518d5
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "68648806"
---
# <a name="the-xamarinforms-flexlayout"></a>Флекслайаут Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)

_Используйте Флекслайаут для наложения или упаковки коллекции дочерних представлений._

[@No__t_1](xref:Xamarin.Forms.FlexLayout) Xamarin. Forms впервые помещается в Xamarin. Forms версии 3,0. Он основан на [модуле макета гибкой рамки](http://www.w3.org/TR/css-flexbox-1/)CSS, обычно известном как _Flex Layout_ или _Flex-Box_, поэтому вызывается, так как он включает множество гибких параметров для упорядочения дочерних элементов в макете.

`FlexLayout` похож на [`StackLayout`](~/xamarin-forms/user-interface/layouts/stack-layout.md) Xamarin. Forms в том, что его дочерние элементы могут быть упорядочены по горизонтали и вертикали в стеке. Однако `FlexLayout` также может заключать свои дочерние элементы, если слишком много подходит для одной строки или столбца, а также имеет множество параметров для ориентации, выравнивания и адаптации к различным размерам экрана.

`FlexLayout` является производным от [`Layout<View>`](xref:Xamarin.Forms.Layout`1) и наследует свойство [`Children`](xref:Xamarin.Forms.Layout`1.Children) типа `IList<View>`.

`FlexLayout` определяет шесть общедоступных связываемых свойств и пять присоединенных связываемых свойств, которые влияют на размер, ориентацию и выравнивание дочерних элементов. (Если вы не знакомы с присоединенными связанными свойствами, см. статью **[присоединенные свойства](~/xamarin-forms/xaml/attached-properties.md)** .) Эти свойства подробно описаны в разделах ниже в разделе сведения о **[связываемых свойствах](#bindable-properties)** , а **[также подробно присоединенные привязываемые свойства](#attached-properties)** . Однако эта статья начинается с раздела о некоторых **[распространенных сценариях использования](#common-scenarios)** `FlexLayout`, которые более подробно описывают многие из этих свойств. В конце этой статьи вы узнаете, как объединить `FlexLayout` с [таблицами стилей CSS](~/xamarin-forms/user-interface/styles/css/index.md).

<a name="common-scenarios" />

## <a name="common-usage-scenarios"></a>Распространенные сценарии использования

Пример программы **[флекслайаутдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** содержит несколько страниц, демонстрирующих некоторые распространенные способы использования `FlexLayout` и позволяет экспериментировать со своими свойствами.

### <a name="using-flexlayout-for-a-simple-stack"></a>Использование Флекслайаут для простого стека

На странице **Простая стопка** показано, как `FlexLayout` можно заменить на `StackLayout`, но с помощью более простой разметки. Все данные в этом примере определены на странице XAML. @No__t_0 содержит четыре дочерних элемента:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.SimpleStackPage"
             Title="Simple Stack">

    <FlexLayout Direction="Column"
                AlignItems="Center"
                JustifyContent="SpaceEvenly">

        <Label Text="FlexLayout in Action"
               FontSize="Large" />

        <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}" />

        <Button Text="Do-Nothing Button" />

        <Label Text="Another Label" />
    </FlexLayout>
</ContentPage>
```

Ниже приведена страница, работающая на iOS, Android и универсальная платформа Windows:

[![Страница «простая стопка»](flex-layout-images/SimpleStack.png "Страница «простая стопка»")](flex-layout-images/SimpleStack-Large.png#lightbox)

В файле **симплестаккпаже. XAML** показаны три свойства `FlexLayout`:

- Свойству [`Direction`](xref:Xamarin.Forms.FlexLayout.Direction) присвоено значение, равное перечислению [`FlexDirection`](xref:Xamarin.Forms.FlexDirection) . Значение по умолчанию: `Row`. Присвоение свойству значения `Column` приводит к тому, что дочерние элементы `FlexLayout` упорядочиваются в один столбец элементов.

    Если элементы в `FlexLayout` расположены в столбце, то `FlexLayout` называется вертикальной _основной_ _и горизонтальной осью._

- Свойство [`AlignItems`](xref:Xamarin.Forms.FlexLayout.AlignItems) имеет тип [`FlexAlignItems`](xref:Xamarin.Forms.FlexAlignItems) и указывает, как выводятся элементы по перекрестной оси. Параметр `Center` приводит к горизонтальному центрированию каждого элемента.

    Если вы использовали `StackLayout`, а не `FlexLayout` для этой задачи, то все элементы можно будет центрировать, назначив свойству `HorizontalOptions` каждого элемента значение `Center`. Свойство `HorizontalOptions` не работает для дочерних элементов `FlexLayout`, но одно свойство `AlignItems` достигает той же цели. При необходимости можно использовать присоединяемое свойство с привязкой `AlignSelf`, чтобы переопределить свойство `AlignItems` для отдельных элементов:

    ```xaml
    <Label Text="FlexLayout in Action"
           FontSize="Large"
           FlexLayout.AlignSelf="Start" />
    ```

    После такого изменения `Label` располагается на левом краю `FlexLayout`, когда порядок чтения слева направо.

- Свойство [`JustifyContent`](xref:Xamarin.Forms.FlexLayout.JustifyContent) имеет тип [`FlexJustify`](xref:Xamarin.Forms.FlexJustify)и указывает, как упорядочены элементы по главной оси. Параметр `SpaceEvenly` выделяет все оставшиеся вертикальные промежутки между всеми элементами, а также над первым элементом и после последнего элемента.

    Если вы использовали `StackLayout`, необходимо присвоить свойству `VerticalOptions` каждого элемента `CenterAndExpand` для достижения аналогичного результата. Но параметр `CenterAndExpand` выделит вдвое больше пространства между каждым элементом, чем перед первым элементом и после последнего элемента. Можно имитировать `CenterAndExpand` параметр `VerticalOptions`, установив для свойства `JustifyContent` `FlexLayout` значение `SpaceAround`.

Эти `FlexLayout` свойства более подробно описаны в разделе ниже приводятся **[связываемые свойства](#bindable-properties)** .

### <a name="using-flexlayout-for-wrapping-items"></a>Использование Флекслайаут для упаковки элементов

На странице " **Перенос фотографий** " в образце **[флекслайаутдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** показано, как `FlexLayout` может переносить свои дочерние элементы в дополнительные строки или столбцы. XAML файл создает экземпляр `FlexLayout` и присваивает ему два свойства:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FlexLayoutDemos.PhotoWrappingPage"
             Title="Photo Wrapping">
    <Grid>
        <ScrollView>
            <FlexLayout x:Name="flexLayout"
                        Wrap="Wrap"
                        JustifyContent="SpaceAround" />
        </ScrollView>

        <ActivityIndicator x:Name="activityIndicator"
                           IsRunning="True"
                           VerticalOptions="Center" />
    </Grid>
</ContentPage>
```

Свойство `Direction` этого `FlexLayout` не задано, поэтому оно имеет значение по умолчанию `Row`, означающее, что дочерние элементы упорядочены в строки, а главная ось — горизонтальная.

Свойство [`Wrap`](xref:Xamarin.Forms.FlexLayout.Wrap) имеет тип перечисления [`FlexWrap`](xref:Xamarin.Forms.FlexWrap). Если в строке слишком много элементов для размещения, то этот параметр свойства приводит к переносу элементов в следующую строку.

Обратите внимание, что `FlexLayout` является дочерним элементом `ScrollView`. Если на странице слишком много строк, то у `ScrollView` есть свойство `Orientation` по умолчанию `Vertical` и разрешена вертикальная прокрутка.

Свойство `JustifyContent` выделяет остаточное пространство на основной оси (горизонтальной оси), чтобы каждый элемент был заключен в один и тот же объем пустого пространства.

Файл кода программной части обращается к коллекции образцов фотографий и добавляет их в коллекцию `Children` `FlexLayout`:

```csharp
public partial class PhotoWrappingPage : ContentPage
{
    // Class for deserializing JSON list of sample bitmaps
    [DataContract]
    class ImageList
    {
        [DataMember(Name = "photos")]
        public List<string> Photos = null;
    }

    public PhotoWrappingPage ()
    {
        InitializeComponent ();

        LoadBitmapCollection();
    }

    async void LoadBitmapCollection()
    {
        using (WebClient webClient = new WebClient())
        {
            try
            {
                // Download the list of stock photos
                Uri uri = new Uri("https://raw.githubusercontent.com/xamarin/docs-archive/master/Images/stock/small/stock.json");
                byte[] data = await webClient.DownloadDataTaskAsync(uri);

                // Convert to a Stream object
                using (Stream stream = new MemoryStream(data))
                {
                    // Deserialize the JSON into an ImageList object
                    var jsonSerializer = new DataContractJsonSerializer(typeof(ImageList));
                    ImageList imageList = (ImageList)jsonSerializer.ReadObject(stream);

                    // Create an Image object for each bitmap
                    foreach (string filepath in imageList.Photos)
                    {
                        Image image = new Image
                        {
                            Source = ImageSource.FromUri(new Uri(filepath))
                        };
                        flexLayout.Children.Add(image);
                    }
                }
            }
            catch
            {
                flexLayout.Children.Add(new Label
                {
                    Text = "Cannot access list of bitmap files"
                });
            }
        }

        activityIndicator.IsRunning = false;
        activityIndicator.IsVisible = false;
    }
}
```

Вот программа, которая выполняется, прокручивается сверху вниз:

[![Страница «Перенос фотографий»](flex-layout-images/PhotoWrapping.png "Страница «Перенос фотографий»")](flex-layout-images/PhotoWrapping-Large.png#lightbox)

### <a name="page-layout-with-flexlayout"></a>Макет страницы с Флекслайаут

В веб-дизайне есть стандартный макет, именуемый « [_Великий граил_](https://en.wikipedia.org/wiki/Holy_grail_(web_design)) », так как это очень желательный формат макета, но часто трудно реализовать с помощью совершенство. Макет состоит из заголовка в верхней части страницы и нижнего колонтитула в нижней части, что расширяется до полной ширины страницы. Занимаясь центром страницы, это основное содержимое, но часто с меню в столбцах слева от содержимого и дополнительными сведениями (иногда называемыми незанятой _областью_ ) справа. [Раздел 5.4.1 спецификации гибкой рамки CSS](http://www.w3.org/TR/css-flexbox-1/#order-accessibility) описывает, как можно реализовать макет "Святой граил" с помощью Flex Box.

На странице **макета «Святой граил** » примера **[флекслайаутдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** показана простая реализация этого макета с использованием одного `FlexLayout` вложенного в другой. Так как эта страница предназначена для телефона в книжной ориентации, области слева и справа от области содержимого выводятся только в 50 пикселей в ширину.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FlexLayoutDemos.HolyGrailLayoutPage"
             Title="Holy Grail Layout">

    <FlexLayout Direction="Column">

        <!-- Header -->
        <Label Text="HEADER"
               FontSize="Large"
               BackgroundColor="Aqua"
               HorizontalTextAlignment="Center" />

        <!-- Body -->
        <FlexLayout FlexLayout.Grow="1">

            <!-- Content -->
            <Label Text="CONTENT"
                   FontSize="Large"
                   BackgroundColor="Gray"
                   HorizontalTextAlignment="Center"
                   VerticalTextAlignment="Center"
                   FlexLayout.Grow="1" />

            <!-- Navigation items-->
            <BoxView FlexLayout.Basis="50"
                     FlexLayout.Order="-1"
                     Color="Blue" />

            <!-- Aside items -->
            <BoxView FlexLayout.Basis="50"
                     Color="Green" />

        </FlexLayout>

        <!-- Footer -->
        <Label Text="FOOTER"
               FontSize="Large"
               BackgroundColor="Pink"
               HorizontalTextAlignment="Center" />
    </FlexLayout>
</ContentPage>
```

Здесь он работает:

[![Страница макета «Святой Граил»](flex-layout-images/HolyGrailLayout.png "Страница макета «Святой Граил»")](flex-layout-images/HolyGrailLayout-Large.png#lightbox)

Области навигации и перемещения в сторону отображаются с помощью `BoxView` слева и справа.

Первый `FlexLayout` в XAML-файле имеет вертикальную основную ось и содержит три дочерних элемента, расположенных в столбце. Это заголовок, текст страницы и нижний колонтитул. Вложенная `FlexLayout` имеет горизонтальную главную ось с тремя дочерними элементами, расположенными в строке.

В этой программе демонстрируются три присоединенных связываемых свойств:

- @No__t_0 присоединенное свойство привязки устанавливается для первого `BoxView`. Это свойство является целым числом со значением по умолчанию 0. Это свойство можно использовать для изменения порядка разметки. Обычно разработчики предпочитают, чтобы содержимое страницы отображалось в разметке до элементов навигации и элементов. Если задать для свойства `Order` первого `BoxView` значение, меньшее, чем его другие элементы того же уровня, то оно будет отображаться как первый элемент в строке. Аналогичным образом можно убедиться, что элемент отображается последним, присвоив свойству `Order` значение, большее, чем его элементы того же уровня.

- @No__t_0 присоединенное свойство привязки устанавливается для двух `BoxView` элементов, чтобы их ширина 50 пикселей. Это свойство имеет тип `FlexBasis`, структура, определяющая статическое свойство типа `FlexBasis` с именем `Auto`, которое является значением по умолчанию. Можно использовать `Basis`, чтобы указать размер в пикселях или процент, указывающий, сколько пространства элемент занимает главную ось. Он называется _базисом_ , поскольку он задает размер элемента, который является основанием для всех последующих макетов.

- Свойство `Grow` задается во вложенном `Layout` и на `Label` дочернем элементе, представляющем содержимое. Это свойство имеет тип `float` и имеет значение по умолчанию 0. Если задано положительное значение, все оставшееся пространство на главной оси выделено этому элементу и к родственным элементам с положительными значениями `Grow`. Пространство выделяется пропорционально значениям, что напоминает спецификацию Star в `Grid`.

    Первое `Grow` присоединенное свойство устанавливается во вложенном `FlexLayout`, указывающее, что этот `FlexLayout` должен занимать все неиспользуемое вертикальное пространство внутри внешнего `FlexLayout`. Второе `Grow` присоединенное свойство задается на `Label`, представляющем содержимое, что означает, что это содержимое займет все неиспользуемое по горизонтали пространство внутри внутреннего `FlexLayout`.

    Также существует аналогичное `Shrink` присоединенное свойство привязки, которое можно использовать, когда размер дочерних элементов превышает размер `FlexLayout` но не требуется перенос.

### <a name="catalog-items-with-flexlayout"></a>Элементы каталога с Флекслайаут

Страница « **элементы каталога** » в образце **[флекслайаутдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** похожа на [Пример 1 в разделе 1,1 спецификации CSS Flex Layout Box](http://www.w3.org/TR/css-flexbox-1/#overview) , за исключением того, что он отображает последовательности изображений и описания трех монкэйс по горизонтали. :

[![Страница «элементы каталога»](flex-layout-images/CatalogItems.png "Страница «элементы каталога»")](flex-layout-images/CatalogItems-Large.png#lightbox)

Каждая из трех монкэйс является `FlexLayout`, содержащейся в `Frame`, для которой задана явная высота и ширина, а также дочерний элемент более крупного `FlexLayout`. В этом XAML-файле большинство свойств дочерних элементов `FlexLayout` задаются в стилях, кроме одного неявного стиля:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.CatalogItemsPage"
             Title="Catalog Items">
    <ContentPage.Resources>
        <Style TargetType="Frame">
            <Setter Property="BackgroundColor" Value="LightYellow" />
            <Setter Property="BorderColor" Value="Blue" />
            <Setter Property="Margin" Value="10" />
            <Setter Property="CornerRadius" Value="15" />
        </Style>

        <Style TargetType="Label">
            <Setter Property="Margin" Value="0, 4" />
        </Style>

        <Style x:Key="headerLabel" TargetType="Label">
            <Setter Property="Margin" Value="0, 8" />
            <Setter Property="FontSize" Value="Large" />
            <Setter Property="TextColor" Value="Blue" />
        </Style>

        <Style TargetType="Image">
            <Setter Property="FlexLayout.Order" Value="-1" />
            <Setter Property="FlexLayout.AlignSelf" Value="Center" />
        </Style>

        <Style TargetType="Button">
            <Setter Property="Text" Value="LEARN MORE" />
            <Setter Property="FontSize" Value="Large" />
            <Setter Property="TextColor" Value="White" />
            <Setter Property="BackgroundColor" Value="Green" />
            <Setter Property="BorderRadius" Value="20" />
        </Style>
    </ContentPage.Resources>

    <ScrollView Orientation="Both">
        <FlexLayout>
            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Seated Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="This monkey is laid back and relaxed, and likes to watch the world go by." />
                    <Label Text="  &#x2022; Doesn't make a lot of noise" />
                    <Label Text="  &#x2022; Often smiles mysteriously" />
                    <Label Text="  &#x2022; Sleeps sitting up" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}"
                           WidthRequest="180"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Banana Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="Watch this monkey eat a giant banana." />
                    <Label Text="  &#x2022; More fun than a barrel of monkeys" />
                    <Label Text="  &#x2022; Banana not included" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.Banana.jpg}"
                           WidthRequest="240"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Face-Palm Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="This monkey reacts appropriately to ridiculous assertions and actions." />
                    <Label Text="  &#x2022; Cynical but not unfriendly" />
                    <Label Text="  &#x2022; Seven varieties of grimaces" />
                    <Label Text="  &#x2022; Doesn't laugh at your jokes" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.FacePalm.jpg}"
                           WidthRequest="180"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>
        </FlexLayout>
    </ScrollView>
</ContentPage>
```

Неявный стиль для `Image` включает параметры двух присоединенных связываемых свойств `Flexlayout`:

```xaml
<Style TargetType="Image">
    <Setter Property="FlexLayout.Order" Value="-1" />
    <Setter Property="FlexLayout.AlignSelf" Value="Center" />
</Style>
```

Параметр `Order` &ndash;1 заставляет элемент `Image` отображаться первым в каждом вложенном представлении `FlexLayout`, независимо от его позиции в коллекции потомков. Свойство `AlignSelf` `Center` вызывает центрирование `Image` в пределах `FlexLayout`. Это переопределяет значение свойства `AlignItems`, которое имеет значение по умолчанию `Stretch`, то есть дочерние элементы `Label` и `Button` растягиваются до полной ширины `FlexLayout`.

В каждом из трех `FlexLayout` представлений пустая `Label` предшествует `Button`, но имеет значение `Grow` 1. Это означает, что все дополнительное вертикальное пространство выделено для этого пустого `Label`, что фактически передает `Button` внизу.

<a name="bindable-properties" />

## <a name="the-bindable-properties-in-detail"></a>Подробные сведения о связываемых свойствах

Теперь, когда вы видели некоторые распространенные приложения `FlexLayout`, свойства `FlexLayout` можно просмотреть более подробно.
`FlexLayout` определяет шесть связываемых свойств, которые устанавливаются в самом `FlexLayout`, в коде или коде XAML, чтобы управлять ориентацией и выравниванием. (Одно из этих свойств, [`Position`](xref:Xamarin.Forms.FlexLayout.Position), не рассматривается в этой статье.)

Вы можете поэкспериментировать с пятью оставшимися связываемыми свойствами, используя страницу **эксперимента** образца **[флекслайаутдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** . На этой странице можно добавлять или удалять дочерние элементы из `FlexLayout` и задавать комбинации пяти связываемых свойств. Все дочерние элементы `FlexLayout` являются `Label` представлениями различных цветов и размеров, в которых свойству `Text` задано число, соответствующее его положению в коллекции `Children`.

При запуске программы в пяти `Picker` представлениях отображаются значения по умолчанию для этих пяти `FlexLayout` свойств. @No__t_0 в нижней части экрана содержит три дочерних элемента:

[![Страница эксперимента: по умолчанию](flex-layout-images/ExperimentDefault.png "Страница эксперимента — по умолчанию")](flex-layout-images/ExperimentDefault-Large.png#lightbox)

Каждое из `Label` представлений имеет серый фон, в котором отображается пространство, выделенное для `Label` в пределах `FlexLayout`. Фон самого `FlexLayout` — Мария Blue. Он занимает всю нижнюю область страницы, за исключением небольшого поля слева и справа.

<a name="direction" />

### <a name="the-direction-property"></a>Свойство Direction

Свойство [`Direction`](xref:Xamarin.Forms.FlexLayout.Direction) имеет тип [`FlexDirection`](xref:Xamarin.Forms.FlexDirection), перечисление с четырьмя членами:

- `Column`
- `ColumnReverse` (или "Column-Reverse" в XAML)
- `Row`, значение по умолчанию
- `RowReverse` (или "строка-обратная" в XAML)

В XAML можно указать значение этого свойства, используя имена членов перечисления в нижнем или верхнем регистре, или можно использовать две дополнительные строки, отображаемые в круглых скобках, которые совпадают с индикаторами CSS. (Строки "Column-Reverse" и "Row-Reverse" определены в [`FlexDirectionTypeConverter`](xref:Xamarin.Forms.FlexDirectionTypeConverter) классе, используемом средством синтаксического анализа XAML.)

Ниже показана страница **эксперимента** (слева направо), направление `Row`, направление `Column` и направление `ColumnReverse`:

[![Страница эксперимента: направление](flex-layout-images/ExperimentDirection.png "Направление страницы эксперимента")](flex-layout-images/ExperimentDirection-Large.png#lightbox)

Обратите внимание, что для параметров `Reverse` элементы начинаются справа или снизу.

<a name="wrap" />

### <a name="the-wrap-property"></a>Свойство Wrap

Свойство [`Wrap`](xref:Xamarin.Forms.FlexLayout.Wrap) имеет тип [`FlexWrap`](xref:Xamarin.Forms.FlexWrap), перечисление с тремя членами:

- `NoWrap`, значение по умолчанию
- `Wrap`
- `Reverse` (или «обернуть-Reverse» в XAML)

На этих экранах слева направо отображаются параметры `NoWrap`, `Wrap` и `Reverse` для 12 дочерних элементов:

[![Страница эксперимента: перенос](flex-layout-images/ExperimentWrap.png "Страница эксперимента — перенос")](flex-layout-images/ExperimentWrap-Large.png#lightbox)

Если свойство `Wrap` имеет значение `NoWrap` и главная ось ограничена (как в этой программе), а главная ось не является широкой или высотой, достаточной для размещения всех дочерних элементов, `FlexLayout` пытается уменьшить размер элементов. , как показано на снимке экрана iOS. Можно управлять сжатием элементов с помощью присоединяемого свойства с привязкой к [`Shrink`](#shrink) .

<a name="justify-content" />

### <a name="the-justifycontent-property"></a>Свойство Жустификонтент

Свойство [`JustifyContent`](xref:Xamarin.Forms.FlexLayout.JustifyContent) имеет тип [`FlexJustify`](xref:Xamarin.Forms.FlexJustify), перечисление с шестью членами:

- `Start` (или "Flex-Start" в XAML), по умолчанию
- `Center`
- `End` (или "Flex-End" в XAML)
- `SpaceBetween` (или "пробел-между" в XAML)
- `SpaceAround` (или «пробел вокруг» в XAML)
- `SpaceEvenly`

Это свойство указывает, как расположены элементы на главной оси, которая является горизонтальной осью в этом примере:

[![Страница эксперимента: выравнивание содержимого](flex-layout-images/ExperimentJustifyContent.png "Содержимое по ширине страницы эксперимента")](flex-layout-images/ExperimentJustifyContent-Large.png#lightbox)

Во всех трех снимках экрана свойство `Wrap` имеет значение `Wrap`. @No__t_0 умолчанию отображается на предыдущем снимке экрана Android. На снимке экрана iOS здесь показан параметр `Center`: все элементы перемещаются в центр. Три других параметра, начинающиеся с слова `Space` выделить дополнительное пространство, не занимаемое элементами. `SpaceBetween` равномерно распределяет интервал между элементами;  `SpaceAround` помещает равное пространство вокруг каждого элемента, в то время как `SpaceEvenly` помещает равное пространство между каждым элементом и до первого элемента в строке.

<a name="align-items" />

### <a name="the-alignitems-property"></a>Свойство Алигнитемс

Свойство [`AlignItems`](xref:Xamarin.Forms.FlexLayout.AlignItems) имеет тип [`FlexAlignItems`](xref:Xamarin.Forms.FlexAlignItems), перечисление с четырьмя членами:

- `Stretch`, значение по умолчанию
- `Center`
- `Start` (или "Flex-Start" в XAML)
- `End` (или "Flex-End" в XAML)

Это одно из двух свойств (другое [`AlignContent`](#align-content)), которое указывает, как дочерние элементы выводятся по перекрестной оси. В каждой строке дочерние элементы растягиваются (как показано на предыдущем снимке экрана) или выравниваются по началу, центру или концу каждого элемента, как показано на следующих трех снимках экрана:

[![Страница эксперимента: выровняйте элементы](flex-layout-images/ExperimentAlignItems.png "Элементы, выровняйте по страницам эксперимента")](flex-layout-images/ExperimentAlignItems-Large.png#lightbox)

На снимке экрана iOS верхние края всех дочерних элементов согласовываются. На снимках экрана Android элементы располагаются вертикально по центру в зависимости от самого высокого элемента. На снимке экрана UWP нижние части всех элементов вычисляются.

Для каждого отдельного элемента параметр `AlignItems` может быть переопределен [`AlignSelf`](#align-self) присоединенным свойством привязки.

<a name="align-content" />

### <a name="the-aligncontent-property"></a>Свойство Алигнконтент

Свойство [`AlignContent`](xref:Xamarin.Forms.FlexLayout.AlignContent) имеет тип [`FlexAlignContent`](xref:Xamarin.Forms.FlexAlignContent), перечисление с семью членами:

- `Stretch`, значение по умолчанию
- `Center`
- `Start` (или "Flex-Start" в XAML)
- `End` (или "Flex-End" в XAML)
- `SpaceBetween` (или "пробел-между" в XAML)
- `SpaceAround` (или «пробел вокруг» в XAML)
- `SpaceEvenly`

Как и `AlignItems`, свойство `AlignContent` также совмещает дочерние элементы на перекрестной оси, но влияет на целые строки или столбцы:

[![Страница эксперимента: Выровняйте содержимое](flex-layout-images/ExperimentAlignContent.png "Содержимое, выровняйте страницу эксперимента")](flex-layout-images/ExperimentAlignContent-Large.png#lightbox)

На снимке экрана iOS обе строки находятся вверху. на снимке экрана Android они находятся в центре; и на снимке экрана UWP они находятся внизу. Строки также можно разставлять по разным путям:

[![Страница эксперимента: Выровняйте содержимое 2](flex-layout-images/ExperimentAlignContent2.png "Страница эксперимента — выровняйте содержимое 2")](flex-layout-images/ExperimentAlignContent2-Large.png#lightbox)

@No__t_0 не действует, если имеется только одна строка или столбец.

<a name="attached-properties" />

## <a name="the-attached-bindable-properties-in-detail"></a>Подробно присоединенные связываемые свойства

`FlexLayout` определяет пять присоединенных привязанных свойств. Эти свойства задаются для дочерних элементов `FlexLayout` и относятся только к этому конкретному дочернему элементу.

<a name="align-self" />

### <a name="the-alignself-property"></a>Свойство Алигнселф

[@No__t_1](xref:Xamarin.Forms.FlexLayout.AlignSelfProperty) присоединенное свойство привязки имеет тип [`FlexAlignSelf`](xref:Xamarin.Forms.FlexAlignContent), перечисление с пятью членами:

- `Auto`, значение по умолчанию
- `Stretch`
- `Center`
- `Start` (или "Flex-Start" в XAML)
- `End` (или "Flex-End" в XAML)

Для любого отдельного дочернего элемента `FlexLayout` это свойство переопределяет свойство [`AlignItems`](#align-items) , установленное на самом `FlexLayout`. Значение по умолчанию `Auto` означает использование параметра `AlignItems`.

Для элемента `Label` с именем `label` (или example) можно задать свойство `AlignSelf` в коде следующим образом:

```csharp
FlexLayout.SetAlignSelf(label, FlexAlignSelf.Center);
```

Обратите внимание, что отсутствует ссылка на родительский `FlexLayout` `Label`. В XAML задается свойство следующим образом:

```xaml
<Label ... FlexLayout.AlignSelf="Center" ... />
```

### <a name="the-order-property"></a>Свойство Order

Свойство [`Order`](xref:Xamarin.Forms.FlexLayout.OrderProperty) имеет тип `int`. Значение по умолчанию — 0.

Свойство `Order` позволяет изменить порядок, в котором упорядочиваются дочерние элементы `FlexLayout`. Как правило, дочерние элементы `FlexLayout` располагаются в том же порядке, в котором они отображаются в коллекции `Children`. Этот порядок можно переопределить, задав `Order` присоединенному свойству BIND значение ненулевого целого числа для одного или нескольких дочерних элементов. @No__t_0 затем упорядочивает свои дочерние элементы на основе значения свойства `Order` для каждого дочернего элемента, но дочерние элементы с тем же параметром `Order` упорядочиваются в том порядке, в котором они отображаются в коллекции `Children`.

### <a name="the-basis-property"></a>Свойство "Базис"

[@No__t_1](xref:Xamarin.Forms.FlexLayout.BasisProperty) присоединенное свойство привязки указывает объем пространства, выделяемого дочернему элементу `FlexLayout` на главной оси. Размер, заданный свойством `Basis`, имеет размер вдоль основной оси родительского `FlexLayout`. Таким образом, `Basis` указывает ширину дочернего элемента, когда дочерние элементы упорядочиваются по строкам, или высоту, когда дочерние элементы упорядочиваются по столбцам.

Свойство `Basis` имеет тип [`FlexBasis`](xref:Xamarin.Forms.FlexBasis), структура. Размер можно указать либо в независимых от устройства единицах, либо в процентах от размера `FlexLayout`. Значением свойства `Basis` по умолчанию является статическое свойство `FlexBasis.Auto`. Это означает, что используется запрошенная ширина или высота дочернего элемента.

В коде можно задать свойство `Basis` для `Label` с именем `label` в 40 устройств, не зависящих от устройства, следующим образом:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40, false));
```

Второй аргумент конструктора `FlexBasis` называется `isRelative` и указывает, является ли размер относительным (`true`) или абсолютным (`false`). Аргумент имеет значение по умолчанию `false`, поэтому можно также использовать следующий код:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40));
```

Определено неявное преобразование из `float` в `FlexBasis`, поэтому его можно упростить еще больше:

```csharp
FlexLayout.SetBasis(label, 40);
```

Можно задать размер 25% `FlexLayout` родительского элемента следующим образом:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(0.25f, true));
```

Это дробное значение должно находиться в диапазоне от 0 до 1.

В XAML можно использовать число в качестве размера в единицах, не зависящих от устройства:

```xaml
<Label ... FlexLayout.Basis="40" ... />
```

Также можно указать процент в диапазоне от 0% до 100%:

```xaml
<Label ... FlexLayout.Basis="25%" ... />
```

На странице «основной **эксперимент** » образца **[флекслайаутдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** можно поэкспериментировать со свойством `Basis`. На странице отображается столбец с текстом, состоящий из пяти элементов `Label` с чередованием цветов фона и переднего плана. Два элемента `Slider` позволяют указать `Basis` значения для второго и четвертого `Label`:

[![Страница «эксперимент»](flex-layout-images/BasisExperiment.png "Страница «эксперимент»")](flex-layout-images/BasisExperiment-Large.png#lightbox)

На снимке экрана iOS слева показаны два `Label` элементов, для которых заданы значения высоты в единицах, не зависящих от устройства. Экран Android показывает, что высота задается в виде доли от общей высоты `FlexLayout`. Если `Basis` задано значение 100%, то дочерний элемент является высотой `FlexLayout` и будет переноситься к следующему столбцу и занимать всю высоту этого столбца, как показано на снимке экрана UWP: он выглядит так, как если бы пять потомков были расположены в строке. , но фактически расположены в пяти столбцах.

### <a name="the-grow-property"></a>Свойство "рост"

[@No__t_1](xref:Xamarin.Forms.FlexLayout.GrowProperty) присоединенное свойство привязки имеет тип `int`. Значение по умолчанию равно 0, а значение должно быть больше или равно 0.

Свойство `Grow` играет роль, если свойство `Wrap` имеет значение `NoWrap`, а строка дочерних элементов имеет общую ширину меньше ширины `FlexLayout` или столбец потомков имеет меньшую высоту, чем `FlexLayout`. Свойство `Grow` указывает, как распределять остаточное пространство между дочерними элементами.

На странице « **увеличение эксперимента** » пять `Label` элементов чередующихся цветов упорядочены по столбцам, а два элемента `Slider` позволяют настроить свойство `Grow` второго и четвертого `Label`. Снимок экрана iOS в дальнем левом окне показывает свойства `Grow` по умолчанию 0:

[![Страница "увеличение эксперимента"](flex-layout-images/GrowExperiment.png "Страница "увеличение эксперимента"")](flex-layout-images/GrowExperiment-Large.png#lightbox)

Если одному дочернему элементу присваивается положительное `Grow` значение, то этот дочерний элемент занимает все оставшееся пространство, как показано на снимке экрана Android. Это пространство также может быть распределено между двумя и более дочерними элементами. На снимке экрана UWP свойству `Grow` второго `Label` присвоено значение 0,5, а свойство `Grow` четвертого `Label` — 1,5, которое предоставляет четвертую `Label` в три раза больше, чем во втором `Label`.

Как дочернее представление использует это пространство, зависит от конкретного типа дочернего элемента. Для `Label` текст можно разместить в общем пространстве `Label` с помощью свойств `HorizontalTextAlignment` и `VerticalTextAlignment`.

<a name="shrink" />

### <a name="the-shrink-property"></a>Свойство Shrink

[@No__t_1](xref:Xamarin.Forms.FlexLayout.ShrinkProperty) присоединенное свойство привязки имеет тип `int`. Значение по умолчанию — 1, а значение должно быть больше или равно 0.

Свойство `Shrink` играет роль, если свойство `Wrap` имеет значение `NoWrap`, а суммарная ширина строки дочернего элемента превышает ширину `FlexLayout` или агрегатная высота одного столбца потомков больше, чем высота `FlexLayout`. Обычно `FlexLayout` отображает эти дочерние элементы, ограничиваюя их размеры. Свойство `Shrink` может указывать, какие дочерние элементы получают приоритет при отображении их полных размеров.

Страница **Сжатие эксперимента** создает `FlexLayout` с одной строкой из пяти `Label` потомков, для которых требуется больше пространства, чем ширина `FlexLayout`. Снимок экрана iOS в левой части отображает все элементы `Label` со значениями по умолчанию 1:

[![Страница сжатия эксперимента](flex-layout-images/ShrinkExperiment.png "Страница сжатия эксперимента")](flex-layout-images/ShrinkExperiment-Large.png#lightbox)

На снимке экрана Android `Shrink` значение второго `Label` равно 0, а `Label` отображается в полной ширине. Кроме того, четвертый `Label` получает значение `Shrink` больше единицы и сжимается. На снимке экрана UWP показаны элементы `Label`, для которых задано `Shrink` значение 0, что позволяет отображать их в полном размере, если это возможно.

Можно задать значения `Grow` и `Shrink`, чтобы в ситуациях, когда размеры дочерних элементов статистического выражения могли быть меньше или иногда больше, чем размер `FlexLayout`.

## <a name="css-styling-with-flexlayout"></a>Стилизация CSS с помощью Флекслайаут

Можно использовать функцию [стилизации CSS](~/xamarin-forms/user-interface/styles/css/index.md) , появившуюся в Xamarin. forms 3,0, в связи с `FlexLayout`. Страница « **элементы каталога CSS** » примера **[флекслайаутдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** дублирует макет страницы « **элементы каталога** », но с таблицей стилей CSS для многих стилей.

[![Страница «элементы каталога CSS»](flex-layout-images/CssCatalogItems.png "Страница «элементы каталога CSS»")](flex-layout-images/CssCatalogItems-Large.png#lightbox)

Исходный файл **каталогитемспаже. XAML** содержит пять определений `Style` в `Resources` разделе с 15 `Setter`ными объектами. В файле **ксскаталогитемспаже. XAML** , который был уменьшен до двух определений `Style` с четырьмя `Setter`ными объектами. Эти стили дополняют таблицу стилей CSS для свойств, которые сейчас не поддерживаются в функции стилизации CSS в Xamarin. Forms:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.CssCatalogItemsPage"
             Title="CSS Catalog Items">
    <ContentPage.Resources>
        <StyleSheet Source="CatalogItemsStyles.css" />

        <Style TargetType="Frame">
            <Setter Property="BorderColor" Value="Blue" />
            <Setter Property="CornerRadius" Value="15" />
        </Style>

        <Style TargetType="Button">
            <Setter Property="Text" Value="LEARN MORE" />
            <Setter Property="BorderRadius" Value="20" />
        </Style>
    </ContentPage.Resources>

    <ScrollView Orientation="Both">
        <FlexLayout>
            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Seated Monkey" StyleClass="header" />
                    <Label Text="This monkey is laid back and relaxed, and likes to watch the world go by." />
                    <Label Text="  &#x2022; Doesn't make a lot of noise" />
                    <Label Text="  &#x2022; Often smiles mysteriously" />
                    <Label Text="  &#x2022; Sleeps sitting up" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Banana Monkey" StyleClass="header" />
                    <Label Text="Watch this monkey eat a giant banana." />
                    <Label Text="  &#x2022; More fun than a barrel of monkeys" />
                    <Label Text="  &#x2022; Banana not included" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.Banana.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Face-Palm Monkey" StyleClass="header" />
                    <Label Text="This monkey reacts appropriately to ridiculous assertions and actions." />
                    <Label Text="  &#x2022; Cynical but not unfriendly" />
                    <Label Text="  &#x2022; Seven varieties of grimaces" />
                    <Label Text="  &#x2022; Doesn't laugh at your jokes" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.FacePalm.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>
        </FlexLayout>
    </ScrollView>
</ContentPage>
```

Ссылка на таблицу стилей CSS содержится в первой строке раздела `Resources`:

```xaml
<StyleSheet Source="CatalogItemsStyles.css" />
```

Кроме того, обратите внимание, что два элемента в каждом из трех элементов включают `StyleClass` параметры:

```xaml
<Label Text="Seated Monkey" StyleClass="header" />
···
<Label StyleClass="empty" />
```

Они ссылаются на селекторы в таблице стилей **каталогитемсстилес. CSS** :

```css
frame {
    width: 300;
    height: 480;
    background-color: lightyellow;
    margin: 10;
}

label {
    margin: 4 0;
}

label.header {
    margin: 8 0;
    font-size: large;
    color: blue;
}

label.empty {
    flex-grow: 1;
}

image {
    height: 180;
    order: -1;
    align-self: center;
}

button {
    font-size: large;
    color: white;
    background-color: green;
}
```

Здесь приведены ссылки на несколько `FlexLayout` присоединенных привязанных свойств. В селекторе `label.empty` вы увидите атрибут `flex-grow`, в котором стили пусты `Label` для предоставления пустого пространства над `Button`. Селектор `image` содержит атрибут `order` и атрибут `align-self`, которые соответствуют `FlexLayout` присоединенным связанным свойствам.

Вы видели, что вы можете задать свойства непосредственно в `FlexLayout` и задать присоединенные привязываемые свойства для дочерних элементов `FlexLayout`. Можно также задать эти свойства косвенно с помощью традиционных стилей на основе XAML или стилей CSS. Важно знать и понять эти свойства. Эти свойства делают `FlexLayout` действительно гибкими.

## <a name="flexlayout-with-xamarinuniversity"></a>Флекслайаут с Xamarin. Университет

> [!VIDEO https://youtube.com/embed/Ng3sel_5D_0]

**Видео о макете Xamarin. Forms 3,0 Flex**

## <a name="related-links"></a>Связанные ссылки

- [флекслайаутдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)
