---
title: Индикаторвиев Xamarin. Forms
description: Индикаторвиев — это элемент управления, отображающий индикаторы, представляющие количество элементов и текущую позиции в Карауселвиев.
ms.prod: xamarin
ms.assetId: BBCC223B-4B02-46B7-80BB-EE0E86A67CE2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/17/2019
ms.openlocfilehash: 6b7845011470d83d8f2187e0227950c23e46d52d
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75490520"
---
# <a name="xamarinforms-indicatorview"></a>Индикаторвиев Xamarin. Forms

![](~/media/shared/preview.png "This API is currently pre-release")

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)

`IndicatorView` — это элемент управления, отображающий индикаторы, представляющие количество элементов и текущую позиции в `CarouselView`:

[![Снимок экрана Карауселвиев и Индикаторвиев на iOS и Android](indicatorview-images/circles.png "Индикаторвиев круги")](indicatorview-images/circles-large.png#lightbox "Индикаторвиев круги")

`IndicatorView` доступен в Xamarin. Forms 4,4 на платформах iOS и Android. Однако в настоящее время это экспериментальное и может использоваться только путем добавления следующей строки кода в класс `AppDelegate` в iOS или в класс `MainActivity` в Android перед вызовом `Forms.Init`:

```csharp
Forms.SetFlags("IndicatorView_Experimental");
```

`IndicatorView` определяет следующие свойства:

- `Count`, типа `int`, количество индикаторов.
- `HideSingle`типа `bool`указывает, должен ли индикатор быть скрытым, если существует только один. Значение по умолчанию — `true`.
- `IndicatorColor`, тип `Color`, Цвет индикаторов.
- `IndicatorSize`, тип `double`, размер индикаторов. Значение по умолчанию — 6,0.
- `IndicatorLayout`типа `Layout<View>`определяет класс макета, используемый для визуализации `IndicatorView`. Это свойство задается Xamarin. Forms и обычно не должно устанавливаться разработчиками.
- `IndicatorTemplate`, тип `DataTemplate`, шаблон, определяющий внешний вид каждого индикатора.
- `IndicatorsShape`, тип `IndicatorShape`, форма каждого индикатора.
- `ItemsSource`, тип `IEnumerable`, коллекция, для которой будут отображаться индикаторы. Это свойство будет автоматически установлено при установке свойства `ItemsSourceBy`.
- `ItemsSourceBy`, типа `VisualElement`, объекта `CarouselView`, для которого отображаются индикаторы.
- `MaximumVisible`, типа `int`, максимальное количество видимых индикаторов. Значение по умолчанию — `int.MaxValue`.
- `Position`, типа `int`, выбранного в данный момент индекса индикатора. Это свойство использует привязку `TwoWay`. Это свойство будет автоматически установлено при установке свойства `ItemsSourceBy`.
- `SelectedIndicatorColor`, типа `Color`, цвет индикатора, представляющий текущий элемент в `CarouselView`.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

## <a name="create-an-indicatorview"></a>Создание Индикаторвиев

В следующем примере показано, как создать экземпляр `IndicatorView` в XAML:

```xaml
<StackLayout>
    <CarouselView x:Name="carouselView"
                  ItemsSource="{Binding Monkeys}">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView ItemsSourceBy="carouselView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="DarkGray"
                   HorizontalOptions="Center" />
</StackLayout>
```

В этом примере `IndicatorView` отображается под `CarouselView`, с индикатором для каждого элемента в `CarouselView`. `IndicatorView` заполняется данными путем установки свойства `ItemsSourceBy` в объект `CarouselView`. Каждый индикатор является светло-серым кругом, а индикатор, представляющий текущий элемент в `CarouselView`, темно-серый.

> [!IMPORTANT]
> Установка свойства `ItemsSourceBy` приводит к привязке свойства `Position` к свойству `CarouselView.Position`, а также к привязке свойства `ItemsSource` к свойству `CarouselView.ItemsSource`.

## <a name="change-indicator-shape"></a>Изменить форму индикатора

Класс `IndicatorView` имеет свойство `IndicatorsShape`, которое указывает форму индикаторов. Этому свойству может быть присвоено одно из членов перечисления `IndicatorShape`:

- `Circle` указывает, что фигуры индикаторов будут циклическими. Это значение по умолчанию для свойства `IndicatorView.IndicatorsShape`.
- `Square` указывает, что фигуры индикаторов будут квадратными.

В следующем примере показан `IndicatorView`, настроенный для использования квадратных индикаторов:

```xaml
<IndicatorView IndicatorsShape="Square"
               ItemsSourceBy="carouselView"
               IndicatorColor="LightGray"
               SelectedIndicatorColor="DarkGray" />
```

## <a name="define-indicator-appearance"></a>Определение внешнего вида индикатора

Внешний вид каждого индикатора можно определить, задав для свойства `IndicatorView.IndicatorTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<StackLayout>
    <CarouselView x:Name="carouselView"
                  ItemsSource="{Binding Monkeys}">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView ItemsSourceBy="carouselView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="Black"
                   HorizontalOptions="Center">
        <IndicatorView.IndicatorTemplate>
            <DataTemplate>
                <Image Source="{FontImage &#xf30c;, FontFamily={OnPlatform iOS=Ionicons, Android=ionicons.ttf#}, Size=12}" />
            </DataTemplate>
        </IndicatorView.IndicatorTemplate>
    </IndicatorView>
</StackLayout>
```

Элементы, указанные в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , определяют внешний вид каждого индикатора. В этом примере каждый индикатор является [`Image`](xref:Xamarin.Forms.Image) , который отображает значок шрифта с помощью расширения разметки `FontImage`.

На следующих снимках экрана показаны индикаторы, отображаемые с помощью значка шрифта:

[![Снимок экрана с шаблоном Индикаторвиев в iOS и Android](indicatorview-images/templated.png "Шаблонные Индикаторвиев")](indicatorview-images/templated-large.png#lightbox "Шаблонные Индикаторвиев")

Дополнительные сведения о расширении разметки `FontImage` см. в разделе [расширение разметки фонтимаже](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension).

## <a name="related-links"></a>Связанные ссылки

- [Индикаторвиев (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)
- [Расширение разметки Фонтимаже](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension)
