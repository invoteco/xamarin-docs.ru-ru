---
title: Прокрутка Xamarin. Forms CollectionView
description: Когда пользователь начинает прокручивать, можно управлять конечной позицией прокрутки, чтобы элементы отображались полностью. Кроме того, CollectionView определяет два метода Скроллто, которые программным путем прокрутки элементов на представление.
ms.prod: xamarin
ms.assetid: 2ED719AF-33D2-434D-949A-B70B479C9BA5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 89bbe402f056b875a7dadd96527364847ad470e8
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2019
ms.locfileid: "68738923"
---
# <a name="xamarinforms-collectionview-scrolling"></a>Прокрутка Xamarin. Forms CollectionView

![](~/media/shared/preview.png "Этот API в настоящее время предоставляется в режиме предварительной версии")

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)определяет два [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) метода: прокрутка элементов в представление. Одна из перегрузок прокручивается элемент по указанному индексу в представление, а другой Прокручивает указанный элемент в представление. Обе перегрузки имеют дополнительные аргументы, которые можно указать, чтобы указать точную позицию элемента после завершения прокрутки и следует ли анимировать прокрутку.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)Определяет событие, которое возникает при вызове одного [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) из методов. [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) `IsAnimated` `Index` `Item` `ScrollToPosition`Объект, `ScrollToRequested` сопровождающий событие, имеет множество свойств, включая,, и. [`ScrollToRequestedEventArgs`](xref:Xamarin.Forms.ScrollToRequestedEventArgs) Эти свойства задаются из аргументов, указанных в `ScrollTo` вызовах метода.

Когда пользователь начинает прокручивать, можно управлять конечной позицией прокрутки, чтобы элементы отображались полностью. Эта функция называется привязкой, так как элементы привязываются к позиции при остановке прокрутки. Дополнительные сведения см. в разделе [точки привязки](#snap-points).

## <a name="scroll-an-item-at-an-index-into-view"></a>Прокрутка элемента по индексу в представлении

Первая [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) перегрузка метода выполняет прокрутку элемента по указанному индексу в представлении. `collectionView`При наличии [`CollectionView`](xref:Xamarin.Forms.CollectionView) объекта с именем в следующем примере показано, как прокручивать элемент с индексом 12 в представление:

```csharp
collectionView.ScrollTo(12);
```

## <a name="scroll-an-item-into-view"></a>Прокрутить элемент на представление

Вторая [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) перегрузка метода выполняет прокрутку указанного элемента в представлении. `collectionView`При наличии [`CollectionView`](xref:Xamarin.Forms.CollectionView) объекта с именем в следующем примере показано, как прокручивать указанный элемент на представление:

```csharp
MonkeysViewModel viewModel = BindingContext as MonkeysViewModel;
Monkey monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
collectionView.ScrollTo(monkey);
```

## <a name="control-scroll-position"></a>Управление положением прокрутки

При прокрутке элемента в представлении точное расположение элемента после прокрутки можно указать с помощью `position` аргумента [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) методов. Этот аргумент принимает [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition) член перечисления.

### <a name="makevisible"></a>макевисибле

[`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition) Элемент указывает, что элемент должен быть прокручиваться, пока он не будет виден в представлении:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.MakeVisible);
```

В этом примере кода создается минимальная прокрутка, необходимая для прокрутки элемента на представление:

[ ![Снимок экрана CollectionViewого вертикального списка с прокруткой элемента в представлении, в iOS и Android](scrolling-images/scrolltoposition-makevisible.png "CollectionView Vertical List с прокруткой элемента") ] (scrolling-images/scrolltoposition-makevisible-large.png#lightbox "Вертикальный список CollectionView с прокруткой элемента")

> [!NOTE]
> Элемент используется по умолчанию, `position` Если аргумент `ScrollTo` не указан при вызове метода. [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition)

### <a name="start"></a>Запуск

[`ScrollToPosition.Start`](xref:Xamarin.Forms.ScrollToPosition) Элемент указывает, что элемент должен быть прокручиваться до начала представления:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Start);
```

Этот пример кода приводит к прокрутке элемента до начала представления:

[ ![Снимок экрана CollectionViewого вертикального списка с прокруткой элемента в представлении, в iOS и Android](scrolling-images/scrolltoposition-start.png "CollectionView Vertical List с прокруткой элемента") ] (scrolling-images/scrolltoposition-start-large.png#lightbox "Вертикальный список CollectionView с прокруткой элемента")

### <a name="center"></a>Центр

[`ScrollToPosition.Center`](xref:Xamarin.Forms.ScrollToPosition) Элемент указывает, что элемент должен быть прокручиваться по центру представления:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Center);
```

В этом примере кода показано, что элемент прокручивается в центр представления:

[ ![Снимок экрана CollectionViewого вертикального списка с прокруткой элемента в представлении, в iOS и Android](scrolling-images/scrolltoposition-center.png "CollectionView Vertical List с прокруткой элемента") ] (scrolling-images/scrolltoposition-center-large.png#lightbox "Вертикальный список CollectionView с прокруткой элемента")

### <a name="end"></a>Конец

[`ScrollToPosition.End`](xref:Xamarin.Forms.ScrollToPosition) Элемент указывает, что элемент должен быть прокручиваться до конца представления:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.End);
```

Этот пример кода приводит к прокрутке элемента до конца представления:

[ ![Снимок экрана CollectionViewого вертикального списка с прокруткой элемента в представлении, в iOS и Android](scrolling-images/scrolltoposition-end.png "CollectionView Vertical List с прокруткой элемента") ] (scrolling-images/scrolltoposition-end-large.png#lightbox "Вертикальный список CollectionView с прокруткой элемента")

## <a name="disable-scroll-animation"></a>Отключить анимацию прокрутки

Анимация с прокруткой отображается при прокрутке элемента в представлении. Однако эту анимацию можно отключить, задав `animate` для `false`аргумента `ScrollTo` метода значение:

```csharp
collectionView.ScrollTo(monkey, animate: false);
```

## <a name="snap-points"></a>Точки привязки

Когда пользователь начинает прокручивать, можно управлять конечной позицией прокрутки, чтобы элементы отображались полностью. Эта функция называется привязкой, так как элементы привязываются к позиции при остановке прокрутки и контролируются следующими свойствами [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) класса:

- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)Тип [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)— задает поведение точек привязки при прокрутке.
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment), типа [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment), задает способ выравнивания точек привязки по элементам.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что свойства могут быть целевыми объектами привязок данных.

> [!NOTE]
> При возникновении привязки она будет выполняться в направлении, которое создает наименьший объем движения.

### <a name="snap-points-type"></a>Тип точек привязки

[`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType) Перечисление определяет следующие члены:

- `None`Указывает, что прокрутка не привязывается к элементам.
- `Mandatory`Указывает, что содержимое всегда привязывается к ближайшей точке привязки, в которой будет естественно останавливаться прокрутка, а также направление инерции.
- `MandatorySingle`Указывает на то же поведение `Mandatory`, что и, но только прокручивает по одному элементу за раз.

По умолчанию [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) свойство имеет `SnapPointsType.None`значение, которое гарантирует, что прокрутка не привязывает элементы, как показано на следующих снимках экрана:

[ ![Снимок экрана: вертикальный список CollectionView без точек привязки, в iOS и Android](scrolling-images/snappoints-none.png "CollectionView по вертикали без точек привязки") ] (scrolling-images/snappoints-none-large.png#lightbox "Вертикальный список CollectionView без точек привязки")

### <a name="snap-points-alignment"></a>Выравнивание точек привязки

Перечисление `Start`определяет `Center`члены, `End`и. [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment)

> [!IMPORTANT]
> Значение [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) свойства учитывается только в том [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) случае `Mandatory`, если свойство имеет значение, или `MandatorySingle`.

#### <a name="start"></a>Запуск

`SnapPointsAlignment.Start` Элемент указывает, что точки привязки выравниваться с ведущим ребром элементов.

По умолчанию [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) свойство имеет `SnapPointsAlignment.Start`значение. Однако для полноты в следующем примере XAML показано, как задать этот элемент перечисления:

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout SnapPointsType="MandatorySingle"
                         SnapPointsAlignment="Start">
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            ...
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new ListItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Start
    },
    ItemTemplate = new DataTemplate(() =>
    {
        return null;
    })
};
```

Когда пользователь настраивается для инициации прокрутки, верхний элемент будет соответствовать верхней части представления:

[ ![Снимок экрана: CollectionView вертикальный список с точками привязки для запуска, в iOS и Android](scrolling-images/snappoints-start.png "CollectionView в вертикальном списке с начальной точкой привязки") ] (scrolling-images/snappoints-start-large.png#lightbox "Вертикальный список CollectionView с начальной точкой привязки")

#### <a name="center"></a>Центр

`SnapPointsAlignment.Center` Элемент указывает, что точки привязки выровнены по центру элементов. В следующем примере XAML показано, как задать этот элемент перечисления:

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout SnapPointsType="MandatorySingle"
                         SnapPointsAlignment="Center">
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            ...
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new ListItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Center
    },
    ItemTemplate = new DataTemplate(() =>
    {
        return null;
    })
};
```

Когда пользователь настраивается для инициации прокрутки, верхний элемент выравнивается по центру в верхней части представления:

[ ![Снимок экрана CollectionView списка с центральными точками привязки, в iOS и Android CollectionView в](scrolling-images/snappoints-center.png "вертикальном списке с центральными точками привязки") ] (scrolling-images/snappoints-center-large.png#lightbox "Вертикальный список CollectionView с центральными точками привязки")

#### <a name="end"></a>Конец

`SnapPointsAlignment.End` Элемент указывает, что точки привязки выравниваться с конечным ребром элементов. В следующем примере XAML показано, как задать этот элемент перечисления:

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout SnapPointsType="MandatorySingle"
                         SnapPointsAlignment="End">
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            ...
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new ListItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.End
    },
    ItemTemplate = new DataTemplate(() =>
    {
        return null;
    })
};
```

Когда пользователь настраивается для инициации прокрутки, нижний элемент будет соответствовать нижней части представления:

[ ![Снимок экрана: вертикальный список CollectionView с конечными точками привязки, в iOS и Android CollectionView в](scrolling-images/snappoints-end.png "вертикальном списке с конечными точками привязки") ] (scrolling-images/snappoints-end-large.png#lightbox "Вертикальный список CollectionView с конечными точками привязки")

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
