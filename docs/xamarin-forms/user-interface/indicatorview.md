---
title: Индикаторвиев Xamarin. Forms
description: Индикаторвиев — это элемент управления, отображающий индикаторы, представляющие количество элементов и текущую позиции в Карауселвиев.
ms.prod: xamarin
ms.assetId: BBCC223B-4B02-46B7-80BB-EE0E86A67CE2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/27/2020
ms.openlocfilehash: e76cf6e766a95994fa2862deb9eb73928f4769a2
ms.sourcegitcommit: 5d22f37dfc358678df52a4d17c57261056a72cb7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2020
ms.locfileid: "77674539"
---
# <a name="xamarinforms-indicatorview"></a>Индикаторвиев Xamarin. Forms

![](~/media/shared/preview.png "This API is currently pre-release")

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)

`IndicatorView` — это элемент управления, отображающий индикаторы, представляющие количество элементов и текущую позиции в `CarouselView`:

[![Снимок экрана Карауселвиев и Индикаторвиев на iOS и Android](indicatorview-images/circles.png "Индикаторвиев круги")](indicatorview-images/circles-large.png#lightbox "Индикаторвиев круги")

`IndicatorView` доступен в Xamarin. Forms 4,4 на платформах iOS и Android, а также в 4,5 на универсальная платформа Windows. Однако в настоящее время это экспериментальное и может использоваться только путем добавления следующей строки кода в класс `AppDelegate` в iOS или в класс `MainActivity` в Android перед вызовом `Forms.Init`:

```csharp
Forms.SetFlags("IndicatorView_Experimental");
```

`IndicatorView` определяет следующие свойства:

- `Count`, типа `int`, количество индикаторов.
- `HideSingle`типа `bool`указывает, должен ли индикатор быть скрытым, если существует только один. Значение по умолчанию — `true`.
- `IndicatorColor`, тип `Color`, Цвет индикаторов.
- `IndicatorSize`, тип `double`, размер индикаторов. Значение по умолчанию — 6,0.
- `IndicatorLayout`типа `Layout<View>`определяет класс макета, используемый для визуализации `IndicatorView`. Это свойство задается Xamarin. Forms и обычно не должно устанавливаться разработчиками.
- `IndicatorTemplate`, тип `DataTemplate`, шаблон, определяющий внешний вид каждого индикатора.
- `IndicatorsShape`, тип `IndicatorShape`, форма каждого индикатора.
- `ItemsSource`, тип `IEnumerable`, коллекция, для которой будут отображаться индикаторы. Это свойство будет автоматически установлено при установке свойства `CarouselView.IndicatorView`.
- `MaximumVisible`, типа `int`, максимальное количество видимых индикаторов. Значение по умолчанию — `int.MaxValue`.
- `Position`, типа `int`, выбранного в данный момент индекса индикатора. Это свойство использует привязку `TwoWay`. Это свойство будет автоматически установлено при установке свойства `CarouselView.IndicatorView`.
- `SelectedIndicatorColor`, типа `Color`, цвет индикатора, представляющий текущий элемент в `CarouselView`.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

## <a name="create-an-indicatorview"></a>Создание Индикаторвиев

В следующем примере показано, как создать экземпляр `IndicatorView` в XAML:

```xaml
<StackLayout>
    <CarouselView ItemsSource="{Binding Monkeys}"
                  IndicatorView="indicatorView">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView x:Name="indicatorView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="DarkGray"
                   HorizontalOptions="Center" />
</StackLayout>
```

В этом примере `IndicatorView` отображается под `CarouselView`, с индикатором для каждого элемента в `CarouselView`. `IndicatorView` заполняется данными путем установки свойства `CarouselView.IndicatorView` в объект `IndicatorView`. Каждый индикатор является светло-серым кругом, а индикатор, представляющий текущий элемент в `CarouselView`, темно-серый.

> [!IMPORTANT]
> Установка свойства `CarouselView.IndicatorView` приводит к привязке свойства `IndicatorView.Position` к свойству `CarouselView.Position`, а также к привязке свойства `IndicatorView.ItemsSource` к свойству `CarouselView.ItemsSource`.

## <a name="change-indicator-shape"></a>Изменить форму индикатора

Класс `IndicatorView` имеет свойство `IndicatorsShape`, которое определяет форму индикаторов. Этому свойству может быть присвоено одно из членов перечисления `IndicatorShape`:

- `Circle` указывает, что фигуры индикаторов будут циклическими. Это значение по умолчанию для свойства `IndicatorView.IndicatorsShape`.
- `Square` указывает, что фигуры индикаторов будут квадратными.

В следующем примере показан `IndicatorView`, настроенный для использования квадратных индикаторов:

```xaml
<IndicatorView x:Name="indicatorView"
               IndicatorsShape="Square"
               IndicatorColor="LightGray"
               SelectedIndicatorColor="DarkGray" />
```

## <a name="change-indicator-size"></a>Изменить размер индикатора

Класс `IndicatorView` имеет свойство `IndicatorSize` типа `double`, которое определяет размер индикаторов в аппаратно-независимых единицах. Значение этого свойства по умолчанию равно 6,0.

В следующем примере показан `IndicatorView`, настроенный для отображения более крупных индикаторов:

```xaml
<IndicatorView x:Name="indicatorView"
               IndicatorSize="18" />
```

## <a name="limit-the-number-of-indicators-displayed"></a>Ограничение числа отображаемых индикаторов

Класс `IndicatorView` имеет свойство `MaximumVisible` типа `int`, которое определяет максимальное количество видимых индикаторов.

В следующем примере показан `IndicatorView`, настроенный для отображения не более шести индикаторов:

```xaml
<IndicatorView x:Name="indicatorView"
               MaximumVisible="6" />
```

## <a name="define-indicator-appearance"></a>Определение внешнего вида индикатора

Внешний вид каждого индикатора можно определить, задав для свойства `IndicatorView.IndicatorTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<StackLayout>
    <CarouselView ItemsSource="{Binding Monkeys}"
                  IndicatorView="indicatorView">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView x:Name="indicatorView"
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
- [Расширение разметки FontImage](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension)
