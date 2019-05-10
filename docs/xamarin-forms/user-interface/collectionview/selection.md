---
title: Выбор Xamarin.Forms CollectionView
description: По умолчанию выбор CollectionView отключен. Тем не менее можно включить одиночное и множественное выделение.
ms.prod: xamarin
ms.assetid: 423D91C7-1E58-4735-9E80-58F11CDFD953
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 1ffed60253889491636fa105dd444ced9c2bedf5
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65048211"
---
# <a name="xamarinforms-collectionview-selection"></a>Выбор Xamarin.Forms CollectionView

![](~/media/shared/preview.png "Этот API в настоящее время в предварительной версии")

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

`CollectionView` определяет следующие свойства, определяющие выбор элементов:

- `SelectionMode`, типа `SelectionMode`, режим выделения.
- `SelectedItem`, типа `object`, элемент, выбранный в списке. Это свойство имеет `null` значение, если элемент не выбран.
- `SelectedItems`, типа `IList<object>`, выбранные элементы в списке. Это свойство доступно только для чтения и имеет `null` значение при отсутствии выбранных элементов.
- `SelectionChangedCommand`, типа `ICommand`, который выполняется при изменении выделенного элемента.
- `SelectionChangedCommandParameter`, типа `object`, который является параметром, который передается `SelectionChangedCommand`.

Все эти свойства, обеспечиваются [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) объектов, что означает, что свойства могут быть целями привязки данных.

По умолчанию `CollectionView` выбор отключен. Тем не менее, это поведение можно изменить, задав `SelectionMode` значение свойства вида `SelectionMode` членов перечисления:

- `None` — Указывает, что элементы не могут быть выбраны. Это значение по умолчанию.
- `Single` — Указывает, что элемент можно выбрать, с выбранным элементом выделена.
- `Multiple` — Указывает, что несколько элементов можно выбрать, с выбранными элементами выделена.

`CollectionView` Определяет `SelectionChanged` событие, возникающее, когда `SelectedItem` изменения свойств, из-за пользователя, выбирающего элемент, из списка, или когда приложение задает свойство. Кроме того, это событие также возникает, когда `SelectedItems` изменения свойств. `SelectionChangedEventArgs` Объект, который прилагается к `SelectionChanged` событий имеет два свойства: тип `IReadOnlyList<object>`:

- `PreviousSelection` — список элементов, которые были выбраны, прежде чем изменить выбор.
- `CurrentSelection` — список элементов, которые выбираются после изменения выделения.

## <a name="single-selection"></a>Выбрать один элемент

Когда `SelectionMode` свойству `Single`, один элемент в `CollectionView` могут быть выбраны. При выборе элемента `SelectedItem` свойству будет присвоено значение выбранного элемента. При изменении свойства, `SelectionChangedCommand` выполняется (со значением `SelectionChangedCommandParameter` , передаваемые `ICommand`) и `SelectionChanged` вызывает событие.

В следующем примере показан XAML `CollectionView` , может отвечать на выбор одного элемента:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectionChanged="OnCollectionViewSelectionChanged">
    ...
</CollectionView>
```

Ниже приведен аналогичный код C#:

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SelectionChanged += OnCollectionViewSelectionChanged;
```

В этом примере кода `OnCollectionViewSelectionChanged` выполняется обработчик события при `SelectionChanged` событие с обработчиком событий, извлечение данных о ранее выбранного элемента и текущего выбранного элемента:

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string previous = (e.PreviousSelection.FirstOrDefault() as Monkey)?.Name;
    string current = (e.CurrentSelection.FirstOrDefault() as Monkey)?.Name;
    ...
}
```

> [!IMPORTANT]
> `SelectionChanged` Событие может генерироваться от изменений, которые происходят в результате изменения `SelectionMode` свойство.

На следующих снимках экрана показано выделение одного элемента в `CollectionView`:

[![Снимок экрана CollectionView вертикальный список одного выделения, в iOS и Android](selection-images/single-selection.png "CollectionView вертикальный список одного выделения")](selection-images/single-selection-large.png#lightbox "CollectionView вертикального списка с одним Выбор")

## <a name="multiple-selection"></a>Выбор нескольких элементов

Когда `SelectionMode` свойству `Multiple`, несколько элементов в `CollectionView` могут быть выбраны. При выборе элементов, `SelectedItems` будет установлено для выбранных элементов. При изменении свойства, `SelectionChangedCommand` выполняется (со значением `SelectionChangedCommandParameter` , передаваемые `ICommand`) и `SelectionChanged` вызывает событие.

В следующем примере показан XAML `CollectionView` , может отвечать на несколько элементов:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Multiple"
                SelectionChanged="OnCollectionViewSelectionChanged">
    ...
</CollectionView>
```

Ниже приведен аналогичный код C#:

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Multiple
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SelectionChanged += OnCollectionViewSelectionChanged;
```

В этом примере кода `OnCollectionViewSelectionChanged` выполняется обработчик события при `SelectionChanged` событие с обработчиком событий, извлечение данных о ранее выбранных элементов и элементам, выделенным:

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var previous = e.PreviousSelection;
    var current = e.CurrentSelection;
    ...
}
```

> [!IMPORTANT]
> `SelectionChanged` Событие может генерироваться от изменений, которые происходят в результате изменения `SelectionMode` свойство.

На следующих снимках экрана показано несколько элементов в `CollectionView`:

[![Снимок экрана вертикальный список CollectionView Выбор нескольких элементов, в iOS и Android](selection-images/multiple-selection.png "CollectionView вертикальном списке Выбор нескольких элементов")](selection-images/multiple-selection-large.png#lightbox "CollectionView вертикального списка с Выбор нескольких элементов")

## <a name="single-pre-selection"></a>Единый предварительный выбор

При `SelectionMode` свойству `Single`, один элемент в `CollectionView` можно также предварительно выбран, задав `SelectedItem` свойство к элементу. В следующем примере показан XAML `CollectionView` , предварительно выбирает один элемент:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectedItem="{Binding SelectedMonkey, Mode=TwoWay}">
    ...
</CollectionView>
```

Ниже приведен аналогичный код C#:

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SetBinding(SelectableItemsView.SelectedItemProperty, "SelectedMonkey", BindingMode.TwoWay);
```

`SelectedItem` Свойство данных привязывает к `SelectedMonkey` свойство связанное представление модели, которые имеют тип `Monkey`. Объект `TwoWay` привязка используется, поэтому, если пользователь изменяет выбранный элемент, значение `SelectedMonkey` будет установлено к выбранному `Monkey` объекта. `SelectedMonkey` Свойство определено в `MonkeysViewModel` и укажите четвертый элемент `Monkeys` коллекции:

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

Таким образом, если `CollectionView` появится, будет выбран профиль четвертый элемент в списке:

[![Снимок экрана CollectionView вертикальный список одного предварительного выделения, в iOS и Android](selection-images/single-pre-selection.png "CollectionView вертикальный список одного предварительного выделения")](selection-images/single-pre-selection-large.png#lightbox "CollectionView вертикальный список одного предварительного выделения")

## <a name="multiple-pre-selection"></a>Несколько предварительный выбор

Когда `SelectionMode` свойству `Multiple`, несколько элементов в `CollectionView` можно также предварительно выбран. В следующем примере показан XAML `CollectionView` , позволяющее предварительный выбор нескольких элементов:

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}"
                SelectionMode="Multiple">
    ...
</CollectionView>
```

Ниже приведен аналогичный код C#:

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Multiple
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Несколько элементов в `CollectionView` можно, добавив их в предварительно выбрать `SelectedItems` свойство:

```csharp
collectionView.SelectedItems.Add(viewModel.Monkeys.Skip(1).FirstOrDefault());
collectionView.SelectedItems.Add(viewModel.Monkeys.Skip(3).FirstOrDefault());
collectionView.SelectedItems.Add(viewModel.Monkeys.Skip(4).FirstOrDefault());
```

> [!NOTE]
> `SelectedItems` Свойство доступно только для чтения, и поэтому не можно использовать два вида данных, привязка к предварительное выделение.

Таким образом, если `CollectionView` появится второй, в-четвертых, и будут выбраны заранее, пятая элементов в списке:

[![Снимок экрана CollectionView вертикальный список нескольких предварительного выделения, в iOS и Android](selection-images/multiple-pre-selection.png "CollectionView вертикальный список с несколькими предварительный выбор")](selection-images/multiple-pre-selection-large.png#lightbox "CollectionView по вертикали список с несколькими предварительный выбор")

## <a name="change-selected-item-color"></a>Изменение цвета выбранного элемента

`CollectionView` имеет `Selected` [ `VisualState` ](xref:Xamarin.Forms.VisualState) , можно использовать для запуска Визуальное изменение к выбранному элементу в `CollectionView`. Общий вариант использования для этого `VisualState` , как изменить цвет фона выбранного элемента, как показано в следующем примере XAML:

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

В этом примере [ `Style.TargetType` ](xref:Xamarin.Forms.Style.TargetType) имеет значение `Grid` так как корневой элемент `ItemTemplate` — [ `Grid` ](xref:Xamarin.Forms.Grid). `Selected` [ `VisualState` ](xref:Xamarin.Forms.VisualState) Указывает, что при создании записи в `CollectionView` выбран, [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) элемента будет присвоено `LightSkyBlue`:

[![Снимок экрана CollectionView вертикальный список пользовательских одного выделения цветом, в iOS и Android](selection-images/single-selection-color.png "CollectionView вертикальный список пользовательских одного выделения цветом") ] (selection-images/single-selection-color-large.png#lightbox " Вертикальный список CollectionView пользовательских одного выделения цветом")

Дополнительные сведения о визуальных состояниях см. в разделе [Диспетчер визуальных состояний Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="disable-selection"></a>Отключить выделение

`CollectionView` Выбор отключен по умолчанию. Тем не менее если `CollectionView` выбор включен, его можно отключить, задав `SelectionMode` свойства `None`:

```xaml
<CollectionView ...
                SelectionMode="None" />
```

Ниже приведен аналогичный код C#:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    SelectionMode = SelectionMode.None
};
```

При `SelectionMode` свойству `None`, элементы в `CollectionView` не могут быть выбраны, `SelectedItem` свойство останется `null`и `SelectionChanged` не будет создаваться событие.

> [!NOTE]
> Если был выбран элемент и `SelectionMode` свойство меняется с `Single` для `None`, `SelectedItem` свойству будет присвоено `null` и `SelectionChanged` событие будет запускаться с пустым `CurrentSelection` свойство.

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Диспетчер визуальных состояний Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md)
