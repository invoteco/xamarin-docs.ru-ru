---
title: Выбор Xamarin.Forms CollectionView
description: По умолчанию выбор CollectionView отключен. Тем не менее можно включить одиночное и множественное выделение.
ms.prod: xamarin
ms.assetid: 423D91C7-1E58-4735-9E80-58F11CDFD953
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: febd48f2ffad86ab8b00bafca8c296377f74a07b
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/21/2019
ms.locfileid: "65970693"
---
# <a name="xamarinforms-collectionview-selection"></a>Выбор Xamarin.Forms CollectionView

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) определяет следующие свойства, определяющие выбор элементов:

- [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode), типа [ `SelectionMode` ](xref:Xamarin.Forms.SelectionMode), режим выделения.
- [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem), типа `object`, элемент, выбранный в списке. Это свойство имеет `null` значение, если элемент не выбран.
- [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems), типа `IList<object>`, выбранные элементы в списке. Это свойство доступно только для чтения и имеет `null` значение при отсутствии выбранных элементов.
- [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand), типа `ICommand`, который выполняется при изменении выделенного элемента.
- [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter), типа `object`, который является параметром, который передается `SelectionChangedCommand`.

Все эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть их можно указывать в качестве целевых для привязки данных.

По умолчанию [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) выбор отключен. Тем не менее, это поведение можно изменить, задав [ `SelectionMode` ](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) значение свойства вида [ `SelectionMode` ](xref:Xamarin.Forms.SelectionMode) членов перечисления:

- `None` — Указывает, что элементы не могут быть выбраны. Это значение по умолчанию.
- `Single` — Указывает, что элемент можно выбрать, с выбранным элементом выделена.
- `Multiple` — Указывает, что несколько элементов можно выбрать, с выбранными элементами выделена.

[`CollectionView`](xref:Xamarin.Forms.CollectionView) Определяет [ `SelectionChanged` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) событие, возникающее, когда [ `SelectedItem` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) изменения свойств, из-за пользователя, выбирающего элемент, из списка, или когда приложение задает свойство. Кроме того, это событие также возникает, когда [ `SelectedItems` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) изменения свойств. [ `SelectionChangedEventArgs` ](xref:Xamarin.Forms.SelectionChangedEventArgs) Объект, который прилагается к `SelectionChanged` событий имеет два свойства: тип `IReadOnlyList<object>`:

- `PreviousSelection` — список элементов, которые были выбраны, прежде чем изменить выбор.
- `CurrentSelection` — список элементов, которые выбираются после изменения выделения.

## <a name="single-selection"></a>Выбрать один элемент

Когда [ `SelectionMode` ](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) свойству `Single`, один элемент в [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) могут быть выбраны. При выборе элемента [ `SelectedItem` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) свойству будет присвоено значение выбранного элемента. При изменении свойства, [ `SelectionChangedCommand` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand) выполняется (со значением [ `SelectionChangedCommandParameter` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter) , передаваемые `ICommand`) и [ `SelectionChanged` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) вызывает событие.

В следующем примере показан XAML [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) , может отвечать на выбор одного элемента:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectionChanged="OnCollectionViewSelectionChanged">
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SelectionChanged += OnCollectionViewSelectionChanged;
```

В этом примере кода `OnCollectionViewSelectionChanged` выполняется обработчик события при [ `SelectionChanged` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) событие с обработчиком событий, извлечение данных о ранее выбранного элемента и текущего выбранного элемента:

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string previous = (e.PreviousSelection.FirstOrDefault() as Monkey)?.Name;
    string current = (e.CurrentSelection.FirstOrDefault() as Monkey)?.Name;
    ...
}
```

> [!IMPORTANT]
> [ `SelectionChanged` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Событие может генерироваться от изменений, которые происходят в результате изменения [ `SelectionMode` ](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) свойство.

На следующих снимках экрана показано выделение одного элемента в [ `CollectionView` ](xref:Xamarin.Forms.CollectionView):

[![Снимок экрана CollectionView вертикальный список одного выделения, в iOS и Android](selection-images/single-selection.png "CollectionView вертикальный список одного выделения")](selection-images/single-selection-large.png#lightbox "CollectionView вертикального списка с одним Выбор")

## <a name="multiple-selection"></a>Выбор нескольких элементов

Когда [ `SelectionMode` ](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) свойству `Multiple`, несколько элементов в [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) могут быть выбраны. При выборе элементов, [ `SelectedItems` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) будет установлено для выбранных элементов. При изменении свойства, [ `SelectionChangedCommand` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand) выполняется (со значением [ `SelectionChangedCommandParameter` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter) , передаваемые `ICommand`) и [ `SelectionChanged` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) вызывает событие.

В следующем примере показан XAML [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) , может отвечать на несколько элементов:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Multiple"
                SelectionChanged="OnCollectionViewSelectionChanged">
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Multiple
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SelectionChanged += OnCollectionViewSelectionChanged;
```

В этом примере кода `OnCollectionViewSelectionChanged` выполняется обработчик события при [ `SelectionChanged` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) событие с обработчиком событий, извлечение данных о ранее выбранных элементов и элементам, выделенным:

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var previous = e.PreviousSelection;
    var current = e.CurrentSelection;
    ...
}
```

> [!IMPORTANT]
> [ `SelectionChanged` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Событие может генерироваться от изменений, которые происходят в результате изменения [ `SelectionMode` ](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) свойство.

На следующих снимках экрана показано несколько элементов в [ `CollectionView` ](xref:Xamarin.Forms.CollectionView):

[![Снимок экрана вертикальный список CollectionView Выбор нескольких элементов, в iOS и Android](selection-images/multiple-selection.png "CollectionView вертикальном списке Выбор нескольких элементов")](selection-images/multiple-selection-large.png#lightbox "CollectionView вертикального списка с Выбор нескольких элементов")

## <a name="single-pre-selection"></a>Единый предварительный выбор

При [ `SelectionMode` ](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) свойству `Single`, один элемент в [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) можно также предварительно выбран, задав [ `SelectedItem` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) свойство к элементу. В следующем примере показан XAML `CollectionView` , предварительно выбирает один элемент:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectedItem="{Binding SelectedMonkey, Mode=TwoWay}">
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SetBinding(SelectableItemsView.SelectedItemProperty, "SelectedMonkey", BindingMode.TwoWay);
```

[ `SelectedItem` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) Свойство данных привязывает к `SelectedMonkey` свойство связанное представление модели, которые имеют тип `Monkey`. Объект `TwoWay` привязка используется, поэтому, если пользователь изменяет выбранный элемент, значение `SelectedMonkey` будет установлено к выбранному `Monkey` объекта. `SelectedMonkey` Свойство определено в `MonkeysViewModel` и укажите четвертый элемент `Monkeys` коллекции:

```csharp
public class MonkeysViewModel : INotifyPropertyChanged
{
    ...
    public ObservableCollection<Monkey> Monkeys { get; private set; }

    Monkey selectedMonkey;
    public Monkey SelectedMonkey
    {
        get
        {
            return selectedMonkey;
        }
        set
        {
            if (selectedMonkey != value)
            {
                selectedMonkey = value;
            }
        }
    }

    public MonkeysViewModel()
    {
        ...
        selectedMonkey = Monkeys.Skip(3).FirstOrDefault();
    }
    ...
}
```

Таким образом, когда [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) появится, будет выбран профиль четвертый элемент в списке:

[![Снимок экрана CollectionView вертикальный список одного предварительного выделения, в iOS и Android](selection-images/single-pre-selection.png "CollectionView вертикальный список одного предварительного выделения")](selection-images/single-pre-selection-large.png#lightbox "CollectionView вертикальный список одного предварительного выделения")

## <a name="multiple-pre-selection"></a>Несколько предварительный выбор

Когда [ `SelectionMode` ](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) свойству `Multiple`, несколько элементов в [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) можно также предварительно выбран. В следующем примере показан XAML `CollectionView` , позволяющее предварительный выбор нескольких элементов:

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}"
                SelectionMode="Multiple">
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Multiple
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Несколько элементов в [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) можно, добавив их в предварительно выбрать [ `SelectedItems` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) свойство:

```csharp
collectionView.SelectedItems.Add(viewModel.Monkeys.Skip(1).FirstOrDefault());
collectionView.SelectedItems.Add(viewModel.Monkeys.Skip(3).FirstOrDefault());
collectionView.SelectedItems.Add(viewModel.Monkeys.Skip(4).FirstOrDefault());
```

> [!NOTE]
> [ `SelectedItems` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) Свойство доступно только для чтения, и поэтому не можно использовать два вида данных, привязка к предварительное выделение.

Таким образом, когда [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) появится второй, в-четвертых, и будут выбраны заранее, пятая элементов в списке:

[![Снимок экрана CollectionView вертикальный список нескольких предварительного выделения, в iOS и Android](selection-images/multiple-pre-selection.png "CollectionView вертикальный список с несколькими предварительный выбор")](selection-images/multiple-pre-selection-large.png#lightbox "CollectionView по вертикали список с несколькими предварительный выбор")

## <a name="change-selected-item-color"></a>Изменение цвета выбранного элемента

[`CollectionView`](xref:Xamarin.Forms.CollectionView) имеет `Selected` [ `VisualState` ](xref:Xamarin.Forms.VisualState) , можно использовать для запуска Визуальное изменение к выбранному элементу в `CollectionView`. Общий вариант использования для этого `VisualState` , как изменить цвет фона выбранного элемента, как показано в следующем примере XAML:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <Style TargetType="Grid">
            <Setter Property="VisualStateManager.VisualStateGroups">
                <VisualStateGroupList>
                    <VisualStateGroup x:Name="CommonStates">
                        <VisualState x:Name="Normal" />
                        <VisualState x:Name="Selected">
                            <VisualState.Setters>
                                <Setter Property="BackgroundColor"
                                        Value="LightSkyBlue" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateGroupList>
            </Setter>
        </Style>
    </ContentPage.Resources>
    <StackLayout Margin="20">
        <CollectionView ItemsSource="{Binding Monkeys}"
                        SelectionMode="Single">
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    <Grid Padding="10">
                        ...
                    </Grid>
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </StackLayout>
</ContentPage>
```

> [!IMPORTANT]
> [ `Style` ](xref:Xamarin.Forms.Style) , Содержащий `Selected` `VisualState` должен иметь [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) значение свойства, которое является типом корневого элемента [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), которое задается как `ItemTemplate` значение свойства.

В этом примере [ `Style.TargetType` ](xref:Xamarin.Forms.Style.TargetType) имеет значение `Grid` так как корневой элемент [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView.ItemTemplate) — [ `Grid` ](xref:Xamarin.Forms.Grid). `Selected` [ `VisualState` ](xref:Xamarin.Forms.VisualState) Указывает, что при создании записи в [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) выбран, [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) элемента будет присвоено `LightSkyBlue`:

[![Снимок экрана CollectionView вертикальный список пользовательских одного выделения цветом, в iOS и Android](selection-images/single-selection-color.png "CollectionView вертикальный список пользовательских одного выделения цветом")](selection-images/single-selection-color-large.png#lightbox "Вертикальный список CollectionView пользовательских одного выделения цветом")

Дополнительные сведения о визуальных состояниях см. в разделе [Диспетчер визуальных состояний Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="disable-selection"></a>Отключить выделение

[`CollectionView`](xref:Xamarin.Forms.CollectionView) Выбор отключен по умолчанию. Тем не менее если `CollectionView` выбор включен, его можно отключить, задав [ `SelectionMode` ](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) свойства `None`:

```xaml
<CollectionView ...
                SelectionMode="None" />
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    SelectionMode = SelectionMode.None
};
```

При [ `SelectionMode` ](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) свойству `None`, элементы в [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) не могут быть выбраны, [ `SelectedItem` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) будет свойство остаются `null`и [ `SelectionChanged` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) не будет создаваться событие.

> [!NOTE]
> Если был выбран элемент и [ `SelectionMode` ](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) свойство меняется с `Single` для `None`, [ `SelectedItem` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) свойству будет присвоено `null` и [ `SelectionChanged` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) событие будет запускаться с пустым `CurrentSelection` свойство.

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Диспетчер визуальных состояний Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md)
