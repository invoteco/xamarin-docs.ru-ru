---
title: Прокрутки элемента в представлении
description: Когда пользователь вставляет, чтобы инициировать прокрутки, конечное положение прокрутки можно управлять, чтобы полностью отображаются элементы. Кроме того CollectionView определяет два метода ScrollTo, которые программно прокрутки элементов в представлении.
ms.prod: xamarin
ms.assetid: 2ED719AF-33D2-434D-949A-B70B479C9BA5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/19/2019
ms.openlocfilehash: da7f379076b8e193deddc99e9004f051ba006cbb
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61367661"
---
# <a name="scroll-an-item-into-view"></a>Прокрутки элемента в представлении

![Предварительный просмотр](~/media/shared/preview.png)

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

> [!IMPORTANT]
> `CollectionView` В настоящее время доступна в предварительной версии, а не имеет части плановой функциональных возможностей. Кроме того API может измениться, как реализация завершилась.

`CollectionView` Определяет два `ScrollTo` методы, которые прокрутки элементов в представлении. Одной из перегрузок прокручивает элемент по указанному индексу в представление, пока другой прокрутке указанного элемента. Обе перегрузки имеют дополнительные аргументы, которые можно задать, чтобы указать точную позицию элемента, после завершения прокрутки и следует ли анимировать прокрутки.

`CollectionView` Определяет `ScrollToRequested` событие, возникающее, когда один из `ScrollTo` вызова методов. `ScrollToRequestedEventArgs` Объект, который прилагается к `ScrollToRequested` событий имеет множество свойств, включая `IsAnimated`, `Index`, `Item`, и `ScrollToPosition`. Эти свойства устанавливаются из аргументов, указанных в `ScrollTo` вызовов методов.

Когда пользователь вставляет, чтобы инициировать прокрутки, конечное положение прокрутки можно управлять, чтобы полностью отображаются элементы. Эта функция называется привязка, так как элементы привязки для размещения во время прокрутки останавливается. Дополнительные сведения см. в разделе [привязка точек](#snap-points).

## <a name="scroll-an-item-at-an-index-into-view"></a>Прокрутки элемента по индексу в представлении

Первый `ScrollTo` перегрузку метода прокручивает элемент по указанному индексу в представление. Учитывая `CollectionView` объект с именем `collectionView`, приведенный ниже показано, как выполнить прокрутку элемента по индексу 12 в представление:

```csharp
collectionView.ScrollTo(12);
```

## <a name="scroll-an-item-into-view"></a>Прокрутки элемента в представлении

Второй `ScrollTo` перегрузку метода прокрутке указанного элемента. Учитывая `CollectionView` объект с именем `collectionView`, приведенный ниже показано, как прокрутки указанный элемент в представлении:

```csharp
MonkeysViewModel viewModel = BindingContext as MonkeysViewModel;
Monkey monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
collectionView.ScrollTo(monkey);
```

## <a name="control-scroll-position"></a>Позиция прокрутки элемента управления

Во время прокрутки элемента в представлении, точную позицию элемента после завершения прокрутки можно задать `position` аргумент `ScrollTo` методы. Этот аргумент принимает [ `ScrollToPosition` ](xref:Xamarin.Forms.ScrollToPosition) член перечисления.

### <a name="makevisible"></a>MakeVisible

[ `ScrollToPosition.MakeVisible` ](xref:Xamarin.Forms.ScrollToPosition) Элемент указывает, что элемент должен выполнить прокрутку, пока он не отображается в представлении:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.MakeVisible);
```

Этот пример кода приводит к минимальной прокрутки, необходимой для прокрутки элемента в представлении:

[![Снимок экрана CollectionView вертикального списка с элементом в результате прокрутки, в iOS и Android](scrolling-images/scrolltoposition-makevisible.png "CollectionView вертикального списка с элементом прокручиваемого")](scrolling-images/scrolltoposition-makevisible-large.png#lightbox "CollectionView вертикального списка с Прокрутка элемента")

> [!NOTE]
> [ `ScrollToPosition.MakeVisible` ](xref:Xamarin.Forms.ScrollToPosition) Член используется по умолчанию, если `position` аргумент не задан, при вызове `ScrollTo` метод.

### <a name="start"></a>Запуск

[ `ScrollToPosition.Start` ](xref:Xamarin.Forms.ScrollToPosition) Элемент указывает, что элемент должен выполнить прокрутку в начало представления:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Start);
```

Этот пример кода приводит к элемент с возможностью прокрутки в начало представления:

[![Снимок экрана CollectionView вертикального списка с элементом в результате прокрутки, в iOS и Android](scrolling-images/scrolltoposition-start.png "CollectionView вертикального списка с элементом прокручиваемого")](scrolling-images/scrolltoposition-start-large.png#lightbox "CollectionView вертикального списка с Прокрутка элемента")

### <a name="center"></a>Центр

[ `ScrollToPosition.Center` ](xref:Xamarin.Forms.ScrollToPosition) Элемент указывает, что элемент должен выполнить прокрутку в центр представления:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Center);
```

Этот пример кода приводит к элемента с возможностью прокрутки по центру представления:

[![Снимок экрана CollectionView вертикального списка с элементом в результате прокрутки, в iOS и Android](scrolling-images/scrolltoposition-center.png "CollectionView вертикального списка с элементом прокручиваемого")](scrolling-images/scrolltoposition-center-large.png#lightbox "CollectionView вертикального списка с Прокрутка элемента")

### <a name="end"></a>Конец

[ `ScrollToPosition.End` ](xref:Xamarin.Forms.ScrollToPosition) Элемент указывает, что элемент должен выполнить прокрутку в конец представления:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.End);
```

Этот пример кода приводит к элемента с возможностью прокрутки в конец представления:

[![Снимок экрана CollectionView вертикального списка с элементом в результате прокрутки, в iOS и Android](scrolling-images/scrolltoposition-end.png "CollectionView вертикального списка с элементом прокручиваемого")](scrolling-images/scrolltoposition-end-large.png#lightbox "CollectionView вертикального списка с Прокрутка элемента")

## <a name="disable-scroll-animation"></a>Отключить прокрутки анимации

Анимированный значок прокрутки отображается во время прокрутки элемента в представлении. Тем не менее, эта анимация можно отключить, задав `animate` аргумент `ScrollTo` метод `false`:

```csharp
collectionView.ScrollTo(monkey, animate: false);
```

## <a name="snap-points"></a>Точек привязки

Когда пользователь вставляет, чтобы инициировать прокрутки, конечное положение прокрутки можно управлять, чтобы полностью отображаются элементы. Эта функция называется привязку, поскольку элементы привязки для размещения в режиме прокрутки останавливает и управляется следующими свойствами из `ItemsLayout` класса:

- `SnapPointsType`, типа `SnapPointsType`, определяет поведение точки прикрепления во время прокрутки.
- `SnapPointsAlignment`, типа `SnapPointsAlignment`, выравнивание точек прикрепления с элементами.

Эти свойства поддерживаются [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) объектов, что означает, что свойства могут быть целями привязки данных.

> [!NOTE]
> Когда происходит привязка, произойдет в направлении, который создает наименьший объем движения.

### <a name="snap-points-type"></a>Тип точки прикрепления

`SnapPointsType` Перечисление определяет следующие члены:

- `None` Указывает, что прокрутка не привязываются к элементам.
- `Mandatory` Указывает, что содержимое всегда указывать где прокрутка естественным образом перестанет, вдоль направления инерции позволяет выполнить привязку к ближайшей оснастки.
- `MandatorySingle` Указывает, аналогично `Mandatory`, но только прокручивает одного элемента за раз.

По умолчанию `SnapPointsType` свойству `SnapPointsType.None`, которое гарантирует, что прокрутка не привязывать элементы, как показано на следующем снимке экрана:

[![Снимок экрана CollectionView вертикальный список без точки прикрепления, iOS и Android](scrolling-images/snappoints-none.png "CollectionView вертикальный список без точки прикрепления")](scrolling-images/snappoints-none-large.png#lightbox "CollectionView вертикальный список без привязки точки")

### <a name="snap-points-alignment"></a>Выравнивание точек привязки

`SnapPointsAlignment` Перечисление определяет `Start`, `Center`, и `End` членов.

> [!IMPORTANT]
> Значение `SnapPointsAlignment` свойство можно использовать только соблюдаться при `SnapPointsType` свойству `Mandatory`, или `MandatorySingle`.

#### <a name="start"></a>Запуск

`SnapPointsAlignment.Start` Элемент указывает, что точки прикрепления выровнены с переднего края элементов.

По умолчанию свойство `SnapPointsAlignment` имеет значение `SnapPointsAlignment.Start`. Однако для полноты информации, в следующем примере XAML показано, как установить значение этого члена перечисления:

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

Ниже приведен аналогичный код C#:

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

Когда пользователь вставляет, чтобы инициировать прокрутки, верхний элемент выравнивается по верхнему краю представления:

[![Снимок экрана CollectionView вертикальный список с начала точки прикрепления, iOS и Android](scrolling-images/snappoints-start.png "CollectionView вертикального списка с начала точки прикрепления")](scrolling-images/snappoints-start-large.png#lightbox "CollectionView вертикального списка с начала Привязка точки")

#### <a name="center"></a>Центр

`SnapPointsAlignment.Center` Элемент указывает, что точки прикрепления выравниваются по центру элементы. В следующем примере XAML показано, как установить значение этого члена перечисления:

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

Ниже приведен аналогичный код C#:

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

Когда пользователь вставляет, чтобы инициировать прокрутки, верхний элемент будет по центру в верхней части представления:

[![Снимок экрана CollectionView вертикальный список с центральными точками привязки, в iOS и Android](scrolling-images/snappoints-center.png "CollectionView вертикального списка с точки прикрепления center")](scrolling-images/snappoints-center-large.png#lightbox "CollectionView вертикального списка с точек прикрепления Center")

#### <a name="end"></a>Конец

`SnapPointsAlignment.End` Элемент указывает, что точки прикрепления выровнены по заднего края элементов. В следующем примере XAML показано, как установить значение этого члена перечисления:

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

Ниже приведен аналогичный код C#:

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

Когда пользователь вставляет, чтобы инициировать прокрутки, нижний элемент выравнивается по нижнему краю представления:

[![Снимок экрана CollectionView вертикальный список с конечными точками привязки, в iOS и Android](scrolling-images/snappoints-end.png "CollectionView вертикального списка с помощью конечных точек прикрепления")](scrolling-images/snappoints-end-large.png#lightbox "CollectionView вертикального списка с помощью оснастки end точки")

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
