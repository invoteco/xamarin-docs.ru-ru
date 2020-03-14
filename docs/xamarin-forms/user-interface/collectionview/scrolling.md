---
title: Прокрутка Xamarin. Forms CollectionView
description: Когда пользователь начинает прокручивать, можно управлять конечной позицией прокрутки, чтобы элементы отображались полностью. Кроме того, CollectionView определяет два метода Скроллто, которые программным путем прокрутки элементов на представление.
ms.prod: xamarin
ms.assetid: 2ED719AF-33D2-434D-949A-B70B479C9BA5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/17/2019
ms.openlocfilehash: 7aef14cbb854d89a2088a450353b943402f76a86
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305753"
---
# <a name="xamarinforms-collectionview-scrolling"></a>Прокрутка Xamarin. Forms CollectionView

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) определяет два метода [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) , которые просматривают элементы в представлении. Одна из перегрузок прокручивается элемент по указанному индексу в представление, а другой Прокручивает указанный элемент в представление. Обе перегрузки имеют дополнительные аргументы, которые могут указывать на группу, к которой принадлежит элемент, точное расположение элемента после завершения прокрутки, а также следует ли анимировать прокрутку.

[`CollectionView`](xref:Xamarin.Forms.CollectionView) определяет событие [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) , которое возникает при вызове одного из методов [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) . Объект [`ScrollToRequestedEventArgs`](xref:Xamarin.Forms.ScrollToRequestedEventArgs) , сопровождающий событие `ScrollToRequested`, имеет множество свойств, включая `IsAnimated`, `Index`, `Item`и `ScrollToPosition`. Эти свойства задаются из аргументов, указанных в вызовах метода `ScrollTo`.

Кроме того, [`CollectionView`](xref:Xamarin.Forms.CollectionView) определяет событие `Scrolled`, которое срабатывает для указания на то, что прокрутка выполнена. Объект `ItemsViewScrolledEventArgs`, сопровождающий событие `Scrolled`, имеет много свойств. Дополнительные сведения см. в разделе [Определение прокрутки](#detect-scrolling).

[`CollectionView`](xref:Xamarin.Forms.CollectionView) также определяет свойство `ItemsUpdatingScrollMode`, представляющее поведение прокрутки `CollectionView` при добавлении к нему новых элементов. Дополнительные сведения об этом свойстве см. в разделе [управление позицией прокрутки при добавлении новых элементов](#control-scroll-position-when-new-items-are-added).

Когда пользователь начинает прокручивать, можно управлять конечной позицией прокрутки, чтобы элементы отображались полностью. Эта функция называется привязкой, так как элементы привязываются к позиции при остановке прокрутки. Дополнительные сведения см. в разделе [точки привязки](#snap-points).

[`CollectionView`](xref:Xamarin.Forms.CollectionView) также может загружать данные постепенно по мере прокрутки пользователем. Дополнительные сведения см. в статье [добавочная загрузка данных](populate-data.md#load-data-incrementally).

## <a name="detect-scrolling"></a>Обнаружение прокрутки

[`CollectionView`](xref:Xamarin.Forms.CollectionView) определяет `Scrolled` событие, которое срабатывает для указания на то, что прокрутка выполнена. В следующем примере XAML показан `CollectionView`, который задает обработчик событий для события `Scrolled`:

```xaml
<CollectionView Scrolled="OnCollectionViewScrolled">
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView();
collectionView.Scrolled += OnCollectionViewScrolled;
```

В этом примере кода обработчик событий `OnCollectionViewScrolled` выполняется при срабатывании события `Scrolled`:

```csharp
void OnCollectionViewScrolled(object sender, ItemsViewScrolledEventArgs e)
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

В этом примере обработчик событий `OnCollectionViewScrolled` выводит значения объекта `ItemsViewScrolledEventArgs`, сопровождающего событие.

> [!IMPORTANT]
> Событие `Scrolled` срабатывает для запуска прокрутки пользователем, а также для программной прокрутки.

## <a name="scroll-an-item-at-an-index-into-view"></a>Прокрутка элемента по индексу в представлении

Первая перегрузка метода [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) прокручивает элемент по указанному индексу в представлении. При наличии объекта [`CollectionView`](xref:Xamarin.Forms.CollectionView) с именем `collectionView`в следующем примере показано, как прокручивать элемент с индексом 12 в представление:

```csharp
collectionView.ScrollTo(12);
```

Кроме того, элемент в сгруппированных данных можно прокручивать в представлении, указав индексы элементов и групп. В следующем примере показано, как прокрутить третий элемент во второй группе на представление:

```csharp
// Items and groups are indexed from zero.
collectionView.ScrollTo(2, 1);
```

> [!NOTE]
> Событие [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) возникает при вызове метода [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) .

## <a name="scroll-an-item-into-view"></a>Прокрутить элемент на представление

Вторая перегрузка метода [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) Прокручивает указанный элемент на представление. При наличии объекта [`CollectionView`](xref:Xamarin.Forms.CollectionView) с именем `collectionView`в следующем примере показано, как прокручивать элемент пробосЦис обезьяны в представление:

```csharp
MonkeysViewModel viewModel = BindingContext as MonkeysViewModel;
Monkey monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
collectionView.ScrollTo(monkey);
```

Кроме того, элемент в сгруппированных данных можно прокручивать в представлении, указав элемент и группу. В следующем примере показано, как прокручивать элемент ПробосЦис обезьяны в группе Монкэйс в представление:

```csharp
GroupedAnimalsViewModel viewModel = BindingContext as GroupedAnimalsViewModel;
AnimalGroup group = viewModel.Animals.FirstOrDefault(a => a.Name == "Monkeys");
Animal monkey = group.FirstOrDefault(m => m.Name == "Proboscis Monkey");
collectionView.ScrollTo(monkey, group);
```

> [!NOTE]
> Событие [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) возникает при вызове метода [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) .

## <a name="disable-scroll-animation"></a>Отключить анимацию прокрутки

Анимация с прокруткой отображается при прокрутке элемента в представлении. Однако эту анимацию можно отключить, задав для аргумента `animate` метода `ScrollTo` значение `false`:

```csharp
collectionView.ScrollTo(monkey, animate: false);
```

## <a name="control-scroll-position"></a>Управление положением прокрутки

При прокрутке элемента в представлении точное расположение элемента после прокрутки можно указать с помощью аргумента `position` методов [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) . Этот аргумент принимает член перечисления [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition) .

### <a name="makevisible"></a>макевисибле

Элемент [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition) указывает, что элемент должен быть прокручиваться, пока он не будет виден в представлении:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.MakeVisible);
```

В этом примере кода создается минимальная прокрутка, необходимая для прокрутки элемента на представление:

[![Снимок экрана CollectionViewого вертикального списка с прокруткой элемента в представлении в iOS и Android](scrolling-images/scrolltoposition-makevisible.png "Вертикальный список CollectionView с прокруткой элемента")](scrolling-images/scrolltoposition-makevisible-large.png#lightbox "Вертикальный список CollectionView с прокруткой элемента")

> [!NOTE]
> Элемент [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition) используется по умолчанию, если аргумент `position` не указан при вызове метода `ScrollTo`.

### <a name="start"></a>Запуск

Элемент [`ScrollToPosition.Start`](xref:Xamarin.Forms.ScrollToPosition) указывает, что элемент должен быть прокручиваться до начала представления:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Start);
```

Этот пример кода приводит к прокрутке элемента до начала представления:

[![Снимок экрана CollectionViewого вертикального списка с прокруткой элемента в представлении в iOS и Android](scrolling-images/scrolltoposition-start.png "Вертикальный список CollectionView с прокруткой элемента")](scrolling-images/scrolltoposition-start-large.png#lightbox "Вертикальный список CollectionView с прокруткой элемента")

### <a name="center"></a>Центр.

Элемент [`ScrollToPosition.Center`](xref:Xamarin.Forms.ScrollToPosition) указывает, что элемент должен быть прокручиваться по центру представления:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Center);
```

В этом примере кода показано, что элемент прокручивается в центр представления:

[![Снимок экрана CollectionViewого вертикального списка с прокруткой элемента в представлении в iOS и Android](scrolling-images/scrolltoposition-center.png "Вертикальный список CollectionView с прокруткой элемента")](scrolling-images/scrolltoposition-center-large.png#lightbox "Вертикальный список CollectionView с прокруткой элемента")

### <a name="end"></a>Конец

Элемент [`ScrollToPosition.End`](xref:Xamarin.Forms.ScrollToPosition) указывает, что элемент должен быть прокручиваться до конца представления:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.End);
```

Этот пример кода приводит к прокрутке элемента до конца представления:

[![Снимок экрана CollectionViewого вертикального списка с прокруткой элемента в представлении в iOS и Android](scrolling-images/scrolltoposition-end.png "Вертикальный список CollectionView с прокруткой элемента")](scrolling-images/scrolltoposition-end-large.png#lightbox "Вертикальный список CollectionView с прокруткой элемента")

## <a name="control-scroll-position-when-new-items-are-added"></a>Управление позицией прокрутки при добавлении новых элементов

[`CollectionView`](xref:Xamarin.Forms.CollectionView) определяет свойство `ItemsUpdatingScrollMode`, которое поддерживается связываемым свойством. Это свойство получает или задает значение перечисления `ItemsUpdatingScrollMode`, представляющее поведение прокрутки `CollectionView` при добавлении в него новых элементов. Перечисление `ItemsUpdatingScrollMode` определяет следующие члены:

- `KeepItemsInView` регулирует смещение прокрутки, чтобы при добавлении новых элементов отображался первый видимый элемент.
- `KeepScrollOffset` поддерживает смещение прокрутки относительно начала списка при добавлении новых элементов.
- `KeepLastItemInView` регулирует смещение прокрутки, чтобы последний элемент отображался при добавлении новых элементов.

Значение свойства `ItemsUpdatingScrollMode` по умолчанию — `KeepItemsInView`. Поэтому при добавлении новых элементов в [`CollectionView`](xref:Xamarin.Forms.CollectionView) по-прежнему отображается первый видимый элемент в списке. Чтобы убедиться, что новые добавленные элементы всегда видны в нижней части списка, свойству `ItemsUpdatingScrollMode` должно быть присвоено значение `KeepLastItemInView`.

```xaml
<CollectionView ItemsUpdatingScrollMode="KeepLastItemInView">
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsUpdatingScrollMode = ItemsUpdatingScrollMode.KeepLastItemInView
};
```

## <a name="scroll-bar-visibility"></a>Видимость полосы прокрутки

[`CollectionView`](xref:Xamarin.Forms.CollectionView) определяет свойства `HorizontalScrollBarVisibility` и `VerticalScrollBarVisibility`, которые поддерживаются с помощью привязки свойств. Эти свойства получают или задают значение перечисления [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) , которое представляет, когда отображается горизонтальная или вертикальная полоса прокрутки. Перечисление `ScrollBarVisibility` определяет следующие члены:

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

По умолчанию свойство [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) имеет значение `SnapPointsType.None`, которое гарантирует, что при прокрутке не привязываются элементы, как показано на следующих снимках экрана:

[![Снимок экрана с вертикальным списком CollectionView без точек привязки в iOS и Android](scrolling-images/snappoints-none.png "Вертикальный список CollectionView без точек привязки")](scrolling-images/snappoints-none-large.png#lightbox "Вертикальный список CollectionView без точек привязки")

### <a name="snap-points-alignment"></a>Выравнивание точек привязки

Перечисление [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment) определяет члены `Start`, `Center`и `End`.

> [!IMPORTANT]
> Значение свойства [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) учитывается только в том случае, если свойство [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) имеет значение `Mandatory`или `MandatorySingle`.

#### <a name="start"></a>Запуск

Элемент `SnapPointsAlignment.Start` указывает, что точки привязки выравниваться с ведущим ребром элементов.

По умолчанию свойство [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) имеет значение `SnapPointsAlignment.Start`. Однако для полноты в следующем примере XAML показано, как задать этот элемент перечисления:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="Start" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Start
    },
    // ...
};
```

Когда пользователь настраивается для инициации прокрутки, верхний элемент будет соответствовать верхней части представления:

[![Снимок экрана с CollectionView вертикальным списком с точками привязки для запуска в iOS и Android](scrolling-images/snappoints-start.png "Вертикальный список CollectionView с начальной точкой привязки")](scrolling-images/snappoints-start-large.png#lightbox "Вертикальный список CollectionView с начальной точкой привязки")

#### <a name="center"></a>Центр.

Элемент `SnapPointsAlignment.Center` указывает, что точки привязки выровнены по центру элементов. В следующем примере XAML показано, как задать этот элемент перечисления:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="Center" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Center
    },
    // ...
};
```

Когда пользователь настраивается для инициации прокрутки, верхний элемент выравнивается по центру в верхней части представления:

[![Снимок экрана с вертикальным CollectionView списком с центральными точками привязки в iOS и Android](scrolling-images/snappoints-center.png "Вертикальный список CollectionView с центральными точками привязки")](scrolling-images/snappoints-center-large.png#lightbox "Вертикальный список CollectionView с центральными точками привязки")

#### <a name="end"></a>Конец

Элемент `SnapPointsAlignment.End` указывает, что точки привязки согласованы с конечным ребром элементов. В следующем примере XAML показано, как задать этот элемент перечисления:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="End" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.End
    },
    // ...
};
```

Когда пользователь настраивается для инициации прокрутки, нижний элемент будет соответствовать нижней части представления:

[![Снимок экрана с CollectionView вертикальным списком с конечными точками привязки в iOS и Android](scrolling-images/snappoints-end.png "Вертикальный список CollectionView с конечными точками привязки")](scrolling-images/snappoints-end-large.png#lightbox "Вертикальный список CollectionView с конечными точками привязки")

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
