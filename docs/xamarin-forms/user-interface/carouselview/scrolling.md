---
title: Прокрутка Xamarin. Forms Карауселвиев
description: Когда пользователь начинает прокручивать, можно управлять конечной позицией прокрутки, чтобы элементы отображались полностью. Кроме того, Карауселвиев определяет два метода Скроллто, которые программным путем прокрутки элементов на представление.
ms.prod: xamarin
ms.assetid: 92D7B618-07FA-4343-9D0F-212525E92C39
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/28/2020
ms.openlocfilehash: 735a572f4aadfc224e545e371525b96f29c9552e
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305687"
---
# <a name="xamarinforms-carouselview-scrolling"></a>Прокрутка Xamarin. Forms Карауселвиев

![](~/media/shared/preview.png "This API is currently pre-release")

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView) определяет следующие свойства, связанные с прокруткой:

- `HorizontalScrollBarVisibility`, типа `ScrollBarVisibility`, который указывает, когда отображается горизонтальная полоса прокрутки.
- `IsDragging`, типа `bool`, который указывает, прокручивается ли `CarouselView`. Это свойство доступно только для чтения, для которого значение по умолчанию — `false`.
- `IsScrollAnimated`, типа `bool`, который указывает, будет ли выполняться анимация при прокрутке `CarouselView`. Значение по умолчанию — `true`.
- `ItemsUpdatingScrollMode`, типа `ItemsUpdatingScrollMode`, который представляет поведение прокрутки `CarouselView` при добавлении к нему новых элементов.
- `VerticalScrollBarVisibility`, типа `ScrollBarVisibility`, который указывает, когда отображается вертикальная полоса прокрутки.

Все эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целями привязок данных.

[`CarouselView`](xref:Xamarin.Forms.CarouselView) также определяет два метода [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) , которые просматривают элементы в представлении. Одна из перегрузок прокручивается элемент по указанному индексу в представление, а другой Прокручивает указанный элемент в представление. Обе перегрузки имеют дополнительные аргументы, которые можно указать, чтобы указать точную позицию элемента после завершения прокрутки и следует ли анимировать прокрутку.

[`CarouselView`](xref:Xamarin.Forms.CarouselView) определяет событие [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) , которое возникает при вызове одного из методов [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) . Объект [`ScrollToRequestedEventArgs`](xref:Xamarin.Forms.ScrollToRequestedEventArgs) , сопровождающий событие `ScrollToRequested`, имеет множество свойств, включая `IsAnimated`, `Index`, `Item`и `ScrollToPosition`. Эти свойства задаются из аргументов, указанных в вызовах метода `ScrollTo`.

Кроме того, [`CarouselView`](xref:Xamarin.Forms.CarouselView) определяет событие `Scrolled`, которое срабатывает для указания на то, что прокрутка выполнена. Объект `ItemsViewScrolledEventArgs`, сопровождающий событие `Scrolled`, имеет много свойств. Дополнительные сведения см. в разделе [Определение прокрутки](#detect-scrolling).

Когда пользователь начинает прокручивать, можно управлять конечной позицией прокрутки, чтобы элементы отображались полностью. Эта функция называется привязкой, так как элементы привязываются к позиции при остановке прокрутки. Дополнительные сведения см. в разделе [точки привязки](#snap-points).

[`CarouselView`](xref:Xamarin.Forms.CarouselView) также может загружать данные постепенно по мере прокрутки пользователем. Дополнительные сведения см. в статье [добавочная загрузка данных](populate-data.md#load-data-incrementally).

## <a name="detect-scrolling"></a>Обнаружение прокрутки

Свойство `IsDragging` можно проверить, чтобы определить, прокручивается ли [`CarouselView`](xref:Xamarin.Forms.CarouselView) в данный момент через элементы.

Кроме того, [`CarouselView`](xref:Xamarin.Forms.CarouselView) определяет событие `Scrolled`, которое срабатывает для указания на то, что прокрутка выполнена. Это событие следует использовать, если требуется получить данные о прокрутке.

В следующем примере XAML показан `CarouselView`, который задает обработчик событий для события `Scrolled`:

```xaml
<CarouselView Scrolled="OnCollectionViewScrolled">
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.Scrolled += OnCarouselViewScrolled;
```

В этом примере кода обработчик событий `OnCarouselViewScrolled` выполняется при срабатывании события `Scrolled`:

```csharp
void OnCarouselViewScrolled(object sender, ItemsViewScrolledEventArgs e)
{
    Debug.WriteLine("HorizontalDelta: " + e.HorizontalDelta);
    Debug.WriteLine("VerticalDelta: " + e.VerticalDelta);
    Debug.WriteLine("HorizontalOffset: " + e.HorizontalOffset);
    Debug.WriteLine("VerticalOffset: " + e.VerticalOffset);
    Debug.WriteLine("FirstVisibleItemIndex: " + e.FirstVisibleItemIndex);
    Debug.WriteLine("CenterItemIndex: " + e.CenterItemIndex);
    Debug.WriteLine("LastVisibleItemIndex: " + e.LastVisibleItemIndex);
}
```

В этом примере обработчик событий `OnCarouselViewScrolled` выводит значения объекта `ItemsViewScrolledEventArgs`, сопровождающего событие.

> [!IMPORTANT]
> Событие `Scrolled` срабатывает для запуска прокрутки пользователем, а также для программной прокрутки.

## <a name="scroll-an-item-at-an-index-into-view"></a>Прокрутка элемента по индексу в представлении

Первая перегрузка метода [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) прокручивает элемент по указанному индексу в представлении. При наличии объекта [`CarouselView`](xref:Xamarin.Forms.CarouselView) с именем `carouselView`в следующем примере показано, как прокручивать элемент с индексом 6 в представление:

```csharp
carouselView.ScrollTo(6);
```

> [!NOTE]
> Событие [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) возникает при вызове метода [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) .

## <a name="scroll-an-item-into-view"></a>Прокрутить элемент на представление

Вторая перегрузка метода [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) Прокручивает указанный элемент на представление. При наличии объекта [`CarouselView`](xref:Xamarin.Forms.CarouselView) с именем `carouselView`в следующем примере показано, как прокручивать элемент пробосЦис обезьяны в представление:

```csharp
MonkeysViewModel viewModel = BindingContext as MonkeysViewModel;
Monkey monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
carouselView.ScrollTo(monkey);
```

> [!NOTE]
> Событие [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) возникает при вызове метода [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) .

## <a name="disable-scroll-animation"></a>Отключить анимацию прокрутки

При перемещении между элементами в [`CarouselView`](xref:Xamarin.Forms.CarouselView)отображается анимированная прокрутка. Эта анимация возникает как для прокрутки, инициированной пользователем, так и для программной прокрутки. Если задать для свойства `IsScrollAnimated` значение `false`, анимация будет отключена для обеих категорий прокрутки.

Кроме того, аргумент `animate` метода `ScrollTo` может иметь значение `false`, чтобы отключить анимацию прокрутки при программной прокрутке:

```csharp
carouselView.ScrollTo(monkey, animate: false);
```

## <a name="control-scroll-position"></a>Управление положением прокрутки

При прокрутке элемента в представлении точное расположение элемента после прокрутки можно указать с помощью аргумента `position` методов [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) . Этот аргумент принимает член перечисления [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition) .

### <a name="makevisible"></a>макевисибле

Элемент [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition) указывает, что элемент должен быть прокручиваться, пока он не будет виден в представлении:

```csharp
carouselView.ScrollTo(monkey, position: ScrollToPosition.MakeVisible);
```

В этом примере кода создается минимальная прокрутка, необходимая для прокрутки элемента на представление.

> [!NOTE]
> Элемент [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition) используется по умолчанию, если аргумент `position` не указан при вызове метода `ScrollTo`.

### <a name="start"></a>Запуск

Элемент [`ScrollToPosition.Start`](xref:Xamarin.Forms.ScrollToPosition) указывает, что элемент должен быть прокручиваться до начала представления:

```csharp
carouselView.ScrollTo(monkey, position: ScrollToPosition.Start);
```

В этом примере кода создается элемент, который прокручивается до начала представления.

### <a name="center"></a>Центр.

Элемент [`ScrollToPosition.Center`](xref:Xamarin.Forms.ScrollToPosition) указывает, что элемент должен быть прокручиваться по центру представления:

```csharp
carouselViewView.ScrollTo(monkey, position: ScrollToPosition.Center);
```

Этот пример кода приводит к переходу элемента в центр представления.

### <a name="end"></a>Конец

Элемент [`ScrollToPosition.End`](xref:Xamarin.Forms.ScrollToPosition) указывает, что элемент должен быть прокручиваться до конца представления:

```csharp
carouselViewView.ScrollTo(monkey, position: ScrollToPosition.End);
```

Этот пример кода приводит к прокрутке элемента до конца представления.

## <a name="control-scroll-position-when-new-items-are-added"></a>Управление позицией прокрутки при добавлении новых элементов

[`CarouselView`](xref:Xamarin.Forms.CarouselView) определяет свойство `ItemsUpdatingScrollMode`, которое поддерживается связываемым свойством. Это свойство получает или задает значение перечисления `ItemsUpdatingScrollMode`, представляющее поведение прокрутки `CarouselView` при добавлении в него новых элементов. Перечисление `ItemsUpdatingScrollMode` определяет следующие члены:

- `KeepItemsInView` регулирует смещение прокрутки, чтобы при добавлении новых элементов отображался первый видимый элемент.
- `KeepScrollOffset` поддерживает смещение прокрутки относительно начала списка при добавлении новых элементов.
- `KeepLastItemInView` регулирует смещение прокрутки, чтобы последний элемент отображался при добавлении новых элементов.

Значение свойства `ItemsUpdatingScrollMode` по умолчанию — `KeepItemsInView`. Поэтому при добавлении новых элементов в [`CarouselView`](xref:Xamarin.Forms.CarouselView) по-прежнему отображается первый видимый элемент в списке. Чтобы убедиться, что новые добавленные элементы всегда видны в нижней части списка, свойству `ItemsUpdatingScrollMode` должно быть присвоено значение `KeepLastItemInView`.

```xaml
<CarouselView ItemsUpdatingScrollMode="KeepLastItemInView">
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView
{
    ItemsUpdatingScrollMode = ItemsUpdatingScrollMode.KeepLastItemInView
};
```

## <a name="scroll-bar-visibility"></a>Видимость полосы прокрутки

[`CarouselView`](xref:Xamarin.Forms.CarouselView) определяет свойства `HorizontalScrollBarVisibility` и `VerticalScrollBarVisibility`, которые поддерживаются с помощью привязки свойств. Эти свойства получают или задают значение перечисления [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) , которое представляет, когда отображается горизонтальная или вертикальная полоса прокрутки. Перечисление `ScrollBarVisibility` определяет следующие члены:

- [`Default`](xref:Xamarin.Forms.ScrollBarVisibility) указывает поведение полосы прокрутки по умолчанию для платформы и является значением по умолчанию для свойств `HorizontalScrollBarVisibility` и `VerticalScrollBarVisibility`.
- [`Always`](xref:Xamarin.Forms.ScrollBarVisibility) указывает, что полосы прокрутки будут видимы, даже если содержимое умещается в представлении.
- [`Never`](xref:Xamarin.Forms.ScrollBarVisibility) указывает, что полосы прокрутки не будут видны, даже если содержимое не умещается в представлении.

## <a name="snap-points"></a>Точки привязки

Когда пользователь начинает прокручивать, можно управлять конечной позицией прокрутки, чтобы элементы отображались полностью. Эта функция называется привязкой, так как элементы привязываются к позиции при остановке прокрутки и контролируются следующими свойствами класса [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) :

- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)типа [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)задает поведение точек привязки при прокрутке.
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)типа [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment)задает способ выравнивания точек привязки по элементам.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами. Это означает, что свойства могут быть целевыми объектами привязок данных.

> [!NOTE]
> При возникновении привязки она будет выполняться в направлении, которое создает наименьший объем движения.

### <a name="snap-points-type"></a>Тип точек привязки

Перечисление [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType) определяет следующие члены:

- `None` указывает, что прокрутка не привязывается к элементам.
- `Mandatory` указывает, что содержимое всегда привязывается к ближайшей точке привязки, в которой будет естественно останавливаться прокрутка, а также направление инерции.
- `MandatorySingle` указывает на то же поведение, что и `Mandatory`, но только прокручивает по одному элементу за раз.

По умолчанию в [`CarouselView`](xref:Xamarin.Forms.CarouselView)свойству [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) присваивается значение `SnapPointsType.MandatorySingle`, что гарантирует прокрутку только по одному элементу за раз.

На следующих снимках экрана показан [`CarouselView`](xref:Xamarin.Forms.CarouselView) с отключенной привязкой:

[![Снимок экрана Карауселвиев без точек привязки в iOS и Android](scrolling-images/snappoints-none.png "Карауселвиев без точек привязки")](scrolling-images/snappoints-none-large.png#lightbox "Карауселвиев без точек привязки")

### <a name="snap-points-alignment"></a>Выравнивание точек привязки

Перечисление [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment) определяет члены `Start`, `Center`и `End`.

> [!IMPORTANT]
> Значение свойства [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) учитывается только в том случае, если свойство [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) имеет значение `Mandatory`или `MandatorySingle`.

#### <a name="start"></a>Запуск

Элемент `SnapPointsAlignment.Start` указывает, что точки привязки выравниваться с ведущим ребром элементов. В следующем примере XAML показано, как задать этот элемент перечисления:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="Start" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Horizontal)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Start
    },
    // ...
};
```

Когда пользователь настраивается для инициации прокрутки в [`CarouselView`](xref:Xamarin.Forms.CarouselView)горизонтальной прокрутки, левый элемент будет выставляться слева от представления:

[![Снимок экрана Карауселвиев с начальной точкой привязки в iOS и Android](scrolling-images/snappoints-start.png "Карауселвиев с начальной точкой привязки")](scrolling-images/snappoints-start-large.png#lightbox "Карауселвиев с начальной точкой привязки")

#### <a name="center"></a>Центр.

Элемент `SnapPointsAlignment.Center` указывает, что точки привязки выровнены по центру элементов.

По умолчанию в [`CarouselView`](xref:Xamarin.Forms.CarouselView)свойству [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) присваивается значение `Center`. Однако для полноты в следующем примере XAML показано, как задать этот элемент перечисления:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="Center" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Horizontal)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Center
    },
    // ...
};
```

Когда пользователь настраивается для инициации прокрутки в [`CarouselView`](xref:Xamarin.Forms.CarouselView)горизонтальной прокрутки, центральный элемент будет выровнен по центру представления:

[![Снимок экрана Карауселвиев с центральными точками привязки в iOS и Android](scrolling-images/snappoints-center.png "Карауселвиев с центральными точками привязки")](scrolling-images/snappoints-center-large.png#lightbox "Карауселвиев с центральными точками привязки")

#### <a name="end"></a>Конец

Элемент `SnapPointsAlignment.End` указывает, что точки привязки согласованы с конечным ребром элементов. В следующем примере XAML показано, как задать этот элемент перечисления:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="End" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Horizontal)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.End
    },
    // ...
};
```

Когда пользователь настраивается для инициации прокрутки в [`CarouselView`](xref:Xamarin.Forms.CarouselView)горизонтальной прокрутки, правый элемент будет выставляться справа от представления.

[![Снимок экрана Карауселвиев с конечными точками привязки в iOS и Android](scrolling-images/snappoints-end.png "Карауселвиев с конечными точками привязки")](scrolling-images/snappoints-end-large.png#lightbox "Карауселвиев с конечными точками привязки")

## <a name="related-links"></a>Связанные ссылки

- [Карауселвиев (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
