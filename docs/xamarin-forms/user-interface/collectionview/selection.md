---
title: Выбор CollectionView Xamarin. Forms
description: По умолчанию выбор CollectionView отключен. Однако можно включить один и несколько элементов.
ms.prod: xamarin
ms.assetid: 423D91C7-1E58-4735-9E80-58F11CDFD953
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: f1a3e8bb8959588e64339f70268370440f356be9
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2019
ms.locfileid: "68738963"
---
# <a name="xamarinforms-collectionview-selection"></a>Выбор CollectionView Xamarin. Forms

![](~/media/shared/preview.png "Этот API в настоящее время предоставляется в режиме предварительной версии")

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)определяет следующие свойства, управляющие выбором элемента:

- [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode), типа [`SelectionMode`](xref:Xamarin.Forms.SelectionMode), режим выбора.
- [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)Тип `object`, выбранный элемент в списке. Это свойство имеет режим `TwoWay`привязки по умолчанию и `null` имеет значение, если ни один элемент не выбран.
- [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems), типа `IList<object>`, выбранные элементы в списке. Это свойство имеет режим `OneWay`привязки по умолчанию и `null` имеет значение, если ни один элемент не выбран.
- [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand)Тип `ICommand`, который выполняется при изменении выбранного элемента.
- [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter)Тип `object`, который является параметром, который передается в `SelectionChangedCommand`.

Все эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть их можно указывать в качестве целевых для привязки данных.

По умолчанию [`CollectionView`](xref:Xamarin.Forms.CollectionView) выделение отключено. Однако это поведение можно изменить, задав [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) для свойства значение одного [`SelectionMode`](xref:Xamarin.Forms.SelectionMode) из членов перечисления:

- `None`— Указывает, что элементы не могут быть выбраны. Это значение по умолчанию.
- `Single`— Указывает, что можно выбрать один элемент, выделив выделенный элемент.
- `Multiple`— Указывает, что можно выбрать несколько элементов с выделенными элементами.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)Определяет событие, которое возникает [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) при изменении свойства либо из-за выбора пользователем элемента из списка, либо когда приложение задает свойство. [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Кроме того, это событие возникает при [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) изменении свойства. Объект, сопровождающий событие, имеет два свойства типа `IReadOnlyList<object>`: `SelectionChanged` [`SelectionChangedEventArgs`](xref:Xamarin.Forms.SelectionChangedEventArgs)

- `PreviousSelection`— список элементов, которые были выбраны до изменения выбора.
- `CurrentSelection`— список элементов, выбранных после изменения выбора.

## <a name="single-selection"></a>Один выбор

Если свойство имеет `Single`значение, в поле [`CollectionView`](xref:Xamarin.Forms.CollectionView) можно выбрать один элемент. [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) Если выбран элемент, [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) свойству будет присвоено значение выбранного элемента. При изменении [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand) этого свойства выполняется (со значением, [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter) передаваемым `ICommand`в), и [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) вызывается событие.

В следующем примере XAML показано [`CollectionView`](xref:Xamarin.Forms.CollectionView) , что может отвечать на выбор одного элемента:

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

В этом примере `OnCollectionViewSelectionChanged` кода обработчик событий выполняется [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) при срабатывании события, когда обработчик событий получает выбранный ранее элемент и текущий выбранный элемент:

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string previous = (e.PreviousSelection.FirstOrDefault() as Monkey)?.Name;
    string current = (e.CurrentSelection.FirstOrDefault() as Monkey)?.Name;
    ...
}
```

> [!IMPORTANT]
> Событие может быть вызвано изменениями, происходящими в результате [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) изменения свойства. [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)

На следующих снимках экрана показан выбор одного элемента [`CollectionView`](xref:Xamarin.Forms.CollectionView)в:

[ ![Снимок экрана: вертикальный список CollectionView с одним выделением в списке по вертикали для iOS и Android](selection-images/single-selection.png "CollectionView с одним выбором") ] (selection-images/single-selection-large.png#lightbox "Вертикальный список CollectionView с одним выделением")

## <a name="multiple-selection"></a>Выбор нескольких элементов

Если свойство имеет `Multiple`значение, можно выбрать несколько элементов в поле [`CollectionView`.](xref:Xamarin.Forms.CollectionView) [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) При выборе [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) элементов свойству будут присвоены выбранные элементы. При изменении [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand) этого свойства выполняется (со значением, [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter) передаваемым `ICommand`в), и [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) вызывается событие.

В следующем примере XAML показано [`CollectionView`](xref:Xamarin.Forms.CollectionView) , что может отвечать на выбор нескольких элементов:

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

В этом примере `OnCollectionViewSelectionChanged` кода обработчик событий выполняется [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) при срабатывании события, когда обработчик событий извлекает ранее выбранные элементы и выделенные элементы.

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var previous = e.PreviousSelection;
    var current = e.CurrentSelection;
    ...
}
```

> [!IMPORTANT]
> Событие может быть вызвано изменениями, происходящими в результате [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) изменения свойства. [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)

На следующих снимках экрана показано выделение нескольких элементов [`CollectionView`](xref:Xamarin.Forms.CollectionView)в:

[ ![Снимок экрана: вертикальный список CollectionView с множественным выделением, в списках iOS и Android](selection-images/multiple-selection.png "CollectionView по вертикали с множественным выделением") ] (selection-images/multiple-selection-large.png#lightbox "Вертикальный список CollectionView с множественным выделением")

## <a name="single-pre-selection"></a>Один предварительный выбор

Если свойство имеет `Single`значение, один элемент в [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) можно предварительно выбрать, установив для свойства элемент. [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) В следующем примере кода XAML показано `CollectionView` , что предварительно выбирает один элемент:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectedItem="{Binding SelectedMonkey}">
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
collectionView.SetBinding(SelectableItemsView.SelectedItemProperty, "SelectedMonkey");
```

> [!NOTE]
> Свойство имеет режим привязки по умолчанию. `TwoWay` [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)

[ `SelectedItem` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) Свойство данных привязывает к `SelectedMonkey` свойство связанное представление модели, которые имеют тип `Monkey`. По умолчанию `TwoWay` используется привязка, так что если пользователь изменяет выбранный элемент, значение `SelectedMonkey` свойства будет установлено для выбранного `Monkey` объекта. Свойство определено `MonkeysViewModel` в классе и устанавливается в четвертый элемент `Monkeys` коллекции: `SelectedMonkey`

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

Таким образом, когда [`CollectionView`](xref:Xamarin.Forms.CollectionView) появится четвертый элемент в списке, он будет предварительно выбран:

[ ![Снимок экрана CollectionViewного вертикального списка с одним предварительным выбором, в](selection-images/single-pre-selection.png "списке по вертикали iOS и Android CollectionView с одним предварительным выбором") ] (selection-images/single-pre-selection-large.png#lightbox "Вертикальный список CollectionView с одним предварительным выбором")

## <a name="multiple-pre-selection"></a>Множественный предварительный выбор

Если свойство имеет `Multiple`значение, можно заранее выбрать несколько элементов в [`CollectionView`поле.](xref:Xamarin.Forms.CollectionView) [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) В следующем примере XAML показано `CollectionView` , в котором будет включен предварительный выбор нескольких элементов:

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}"
                SelectionMode="Multiple"
                SelectedItems="{Binding SelectedMonkeys}">
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
collectionView.SetBinding(SelectableItemsView.SelectedItemsProperty, "SelectedMonkeys");
```

> [!NOTE]
> Свойство имеет режим привязки по умолчанию. `OneWay` [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)

[ `SelectedItems` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) Свойство данных привязывает к `SelectedMonkeys` свойство связанное представление модели, которые имеют тип `ObservableCollection<object>`. Свойство определено `MonkeysViewModel` в классе и устанавливается для второго, четвертого `Monkeys` и пятого элементов в коллекции: `SelectedMonkeys`

```csharp
namespace CollectionViewDemos.ViewModels
{
    public class MonkeysViewModel : INotifyPropertyChanged
    {
        ...
        ObservableCollection<object> selectedMonkeys;
        public ObservableCollection<object> SelectedMonkeys
        {
            get
            {
                return selectedMonkeys;
            }
            set
            {
                if (selectedMonkeys != value)
                {
                    selectedMonkeys = value;
                }
            }
        }

        public MonkeysViewModel()
        {
            ...
            SelectedMonkeys = new ObservableCollection<object>()
            {
                Monkeys[1], Monkeys[3], Monkeys[4]
            };
        }
        ...
    }
}
```

Таким образом, при [`CollectionView`](xref:Xamarin.Forms.CollectionView) появлении второго, четвертого и пятого элементов в списке будут предварительно выбраны:

[ ![Снимок экрана: вертикальный список CollectionView с несколькими предварительными выбранными элементами в списке по вертикали для iOS и Android](selection-images/multiple-pre-selection.png "CollectionView с несколькими предварительными вариантами") ] (selection-images/multiple-pre-selection-large.png#lightbox "Вертикальный список CollectionView с несколькими предварительными выбранными") элементами

## <a name="clearing-selections"></a>Очистка выделенных элементов

Свойства [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) `null`и [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) можно очистить, установив их или объекты, к которым они привязаны, к.

## <a name="change-selected-item-color"></a>Изменить цвет выбранного элемента

[`CollectionView`](xref:Xamarin.Forms.CollectionView)имеет объект `Selected` [`VisualState`](xref:Xamarin.Forms.VisualState) , который можно использовать для инициации визуального изменения выбранного элемента в `CollectionView`. Распространенным вариантом использования этого `VisualState` варианта является изменение цвета фона выбранного элемента, который показан в следующем примере XAML:

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
> Объект [`Style`](xref:Xamarin.Forms.Style) , содержащий объект `Selected`,должен иметь [`TargetType`значениесвойства](xref:Xamarin.Forms.Style.TargetType) , которое является типом корневого элемента объекта `ItemTemplate` [,которыйзадаетсякакзначениесвойства.`DataTemplate`](xref:Xamarin.Forms.DataTemplate) `VisualState`

В этом примере [`Style.TargetType`](xref:Xamarin.Forms.Style.TargetType) свойству присваивается значение `Grid` , поскольку корневым [`Grid`](xref:Xamarin.Forms.Grid)элементом [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) объекта является. `Selected` `LightSkyBlue`Указывает, [что`CollectionView`](xref:Xamarin.Forms.CollectionView) при [выборе`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) элемента в параметре для элемента будет установлено значение: [`VisualState`](xref:Xamarin.Forms.VisualState)

[![Снимок экрана CollectionView вертикальный список пользовательских одного выделения цветом, в iOS и Android](selection-images/single-selection-color.png "CollectionView вертикальный список пользовательских одного выделения цветом")](selection-images/single-selection-color-large.png#lightbox "Вертикальный список CollectionView пользовательских одного выделения цветом")

Дополнительные сведения о визуальных состояниях см. в разделе [Диспетчер визуальных состояний Xamarin. Forms](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="disable-selection"></a>Отключить выделение

[`CollectionView`](xref:Xamarin.Forms.CollectionView)по умолчанию выделение отключено. Однако если `CollectionView` включен выбор, его можно отключить, [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) задав для `None`свойства значение:

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

[`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) [`CollectionView`](xref:Xamarin.Forms.CollectionView) `null` [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Если свойство имеет `None`значение, элементы в не могут быть выбраны, свойство останется, а событие не будет запущено. [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)

> [!NOTE]
> При выборе [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) элемента и изменении свойства с `Single` на `None` [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) свойство будет установлено на `null` , а [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) событие будет запущено с пустым `CurrentSelection` свойством. .

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Диспетчер визуальных состояний Xamarin. Forms](~/xamarin-forms/user-interface/visual-state-manager.md)
