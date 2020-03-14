---
title: Выбор CollectionView Xamarin. Forms
description: По умолчанию выбор CollectionView отключен. Однако можно включить один и несколько элементов.
ms.prod: xamarin
ms.assetid: 423D91C7-1E58-4735-9E80-58F11CDFD953
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: a4cc237ef738edeccf66f1a91a010e4831c1c72f
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305915"
---
# <a name="xamarinforms-collectionview-selection"></a>Выбор CollectionView Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) определяет следующие свойства, управляющие выбором элементов:

- [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode), тип [`SelectionMode`](xref:Xamarin.Forms.SelectionMode), режим выбора.
- [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem), тип `object`, выбранный элемент в списке. Это свойство имеет режим привязки по умолчанию `TwoWay`и имеет `null` значение, если ни один элемент не выбран.
- [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems), тип `IList<object>`, выбранные элементы в списке. Это свойство имеет режим привязки по умолчанию `OneWay`и имеет `null` значение, если ни один элемент не выбран.
- [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand), типа `ICommand`, который выполняется при изменении выбранного элемента.
- [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter), типа `object`, который является параметром, который передается в `SelectionChangedCommand`.

Все эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть их можно указывать в качестве целевых для привязки данных.

По умолчанию [`CollectionView`](xref:Xamarin.Forms.CollectionView) выбор отключен. Однако это поведение можно изменить, задав для свойства [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) значение одного из членов перечисления [`SelectionMode`](xref:Xamarin.Forms.SelectionMode) .

- `None` — указывает, что элементы не могут быть выбраны. Это значение по умолчанию.
- `Single` — указывает, что можно выбрать один элемент, при этом выделенный элемент выделяется.
- `Multiple` — указывает, что можно выбрать несколько элементов с выделенными элементами.

[`CollectionView`](xref:Xamarin.Forms.CollectionView) определяет событие [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) , которое возникает при изменении свойства [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) либо из-за выбора пользователем элемента из списка, либо когда приложение задает свойство. Кроме того, это событие возникает при изменении свойства [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) . Объект [`SelectionChangedEventArgs`](xref:Xamarin.Forms.SelectionChangedEventArgs) , сопровождающий событие `SelectionChanged`, имеет два свойства типа `IReadOnlyList<object>`:

- `PreviousSelection` — список элементов, которые были выбраны до изменения выбора.
- `CurrentSelection` — список выбранных элементов после изменения выбора.

Кроме того, [`CollectionView`](xref:Xamarin.Forms.CollectionView) имеет метод `UpdateSelectedItems`, который обновляет свойство [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) списком выбранных элементов, в то время как обрабатывалось только одно уведомление об изменении.

## <a name="single-selection"></a>Один выбор

Если свойство [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) имеет значение `Single`, можно выбрать один элемент в [`CollectionView`](xref:Xamarin.Forms.CollectionView) . Если выбран элемент, свойству [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) будет присвоено значение выбранного элемента. При изменении этого свойства [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand) выполняется (со значением [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter) передается `ICommand`), а также вызывается событие [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) .

В следующем примере XAML показан [`CollectionView`](xref:Xamarin.Forms.CollectionView) , который может отвечать на выбор одного элемента:

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

В этом примере кода обработчик событий `OnCollectionViewSelectionChanged` выполняется при срабатывании события [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) , когда обработчик событий получает выбранный ранее элемент и текущий выбранный элемент:

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string previous = (e.PreviousSelection.FirstOrDefault() as Monkey)?.Name;
    string current = (e.CurrentSelection.FirstOrDefault() as Monkey)?.Name;
    ...
}
```

> [!IMPORTANT]
> Событие [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) может быть вызвано изменениями, происходящими в результате изменения свойства [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) .

На следующих снимках экрана показано выделение одного элемента в [`CollectionView`](xref:Xamarin.Forms.CollectionView):

[![Снимок экрана с одним выделенным вертикальным списком CollectionView в iOS и Android](selection-images/single-selection.png "Вертикальный список CollectionView с одним выделением")](selection-images/single-selection-large.png#lightbox "Вертикальный список CollectionView с одним выделением")

## <a name="multiple-selection"></a>Выбор нескольких элементов

Если для свойства [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) задано значение `Multiple`, можно выбрать несколько элементов в [`CollectionView`](xref:Xamarin.Forms.CollectionView) . При выборе элементов свойству [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) будут присвоены выбранные элементы. При изменении этого свойства [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand) выполняется (со значением [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter) передается `ICommand`), а также вызывается событие [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) .

В следующем примере XAML показан [`CollectionView`](xref:Xamarin.Forms.CollectionView) , который может отвечать на выбор нескольких элементов:

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

В этом примере кода обработчик событий `OnCollectionViewSelectionChanged` выполняется при срабатывании события [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) , когда обработчик событий извлекает ранее выбранные элементы и выбранные элементы:

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var previous = e.PreviousSelection;
    var current = e.CurrentSelection;
    ...
}
```

> [!IMPORTANT]
> Событие [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) может быть вызвано изменениями, происходящими в результате изменения свойства [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) .

На следующих снимках экрана показано выделение нескольких элементов в [`CollectionView`](xref:Xamarin.Forms.CollectionView):

[![Снимок экрана: вертикальный список CollectionView с множественным выделением в iOS и Android](selection-images/multiple-selection.png "Вертикальный список CollectionView с множественным выделением")](selection-images/multiple-selection-large.png#lightbox "Вертикальный список CollectionView с множественным выделением")

## <a name="single-pre-selection"></a>Один предварительный выбор

Если свойство [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) имеет значение `Single`, один элемент в [`CollectionView`](xref:Xamarin.Forms.CollectionView) можно предварительно выбрать, установив для свойства [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) элемент. В следующем примере XAML показан `CollectionView`, который предварительно выбирает один элемент:

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
> Свойство [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) имеет режим привязки по умолчанию `TwoWay`.

Данные свойства [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) привязываются к свойству `SelectedMonkey` подключенной модели представления, которая имеет тип `Monkey`. По умолчанию используется привязка `TwoWay` таким образом, что если пользователь изменяет выбранный элемент, значение свойства `SelectedMonkey` будет установлено на выбранный объект `Monkey`. Свойство `SelectedMonkey` определено в классе `MonkeysViewModel` и устанавливается в четвертый элемент коллекции `Monkeys`:

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

Поэтому при появлении [`CollectionView`](xref:Xamarin.Forms.CollectionView) четвертый элемент в списке предварительно выбирается:

[![Снимок экрана CollectionViewого вертикального списка с одним предварительным выбором, в iOS и Android](selection-images/single-pre-selection.png "Вертикальный список CollectionView с одним предварительным выбором")](selection-images/single-pre-selection-large.png#lightbox "Вертикальный список CollectionView с одним предварительным выбором")

## <a name="multiple-pre-selection"></a>Множественный предварительный выбор

Если свойство [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) имеет значение `Multiple`, можно заранее выбрать несколько элементов в [`CollectionView`](xref:Xamarin.Forms.CollectionView) . В следующем примере XAML показан `CollectionView`, который позволяет выбрать несколько элементов.

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
> Свойство [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) имеет режим привязки по умолчанию `OneWay`.

Данные свойства [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) привязываются к свойству `SelectedMonkeys` подключенной модели представления, которая имеет тип `ObservableCollection<object>`. Свойство `SelectedMonkeys` определено в классе `MonkeysViewModel` и устанавливается для второго, четвертого и пятого элементов в коллекции `Monkeys`:

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

Таким образом, при появлении [`CollectionView`](xref:Xamarin.Forms.CollectionView) в списке будут предварительно выбраны второй, четвертый и пятый элементы.

[![Снимок экрана: вертикальный список CollectionView с несколькими предварительными выбранными элементами в iOS и Android](selection-images/multiple-pre-selection.png "Вертикальный список CollectionView с несколькими предварительными выбранными элементами")](selection-images/multiple-pre-selection-large.png#lightbox "Вертикальный список CollectionView с несколькими предварительными выбранными элементами")

## <a name="clear-selections"></a>Очистить выделенные элементы

Свойства [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) и [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) можно очистить, установив их или объекты, к которым они привязаны, для `null`.

## <a name="change-selected-item-color"></a>Изменить цвет выбранного элемента

[`CollectionView`](xref:Xamarin.Forms.CollectionView) имеет `Selected` [`VisualState`](xref:Xamarin.Forms.VisualState) , который можно использовать для инициации визуального изменения выбранного элемента в `CollectionView`. Распространенным вариантом использования этого `VisualState` является изменение цвета фона выбранного элемента, который показан в следующем примере XAML:

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
> [`Style`](xref:Xamarin.Forms.Style) , содержащий `Selected` `VisualState`, должно иметь значение свойства [`TargetType`](xref:Xamarin.Forms.Style.TargetType) , которое является типом корневого элемента [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), который задан как значение свойства `ItemTemplate`.

В этом примере свойству [`Style.TargetType`](xref:Xamarin.Forms.Style.TargetType) присвоено значение `Grid`, так как корневой элемент [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) является [`Grid`](xref:Xamarin.Forms.Grid). `Selected` [`VisualState`](xref:Xamarin.Forms.VisualState) указывает, что при выборе элемента в [`CollectionView`](xref:Xamarin.Forms.CollectionView) для [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) элемента будет установлено значение `LightSkyBlue`.

[![Снимок экрана CollectionViewого вертикального списка с пользовательским цветом одного выделения в iOS и Android](selection-images/single-selection-color.png "Вертикальный список CollectionView с пользовательским одним цветом выделения")](selection-images/single-selection-color-large.png#lightbox "Вертикальный список CollectionView с пользовательским одним цветом выделения")

Дополнительные сведения о визуальных состояниях см. в разделе [Диспетчер визуальных состояний Xamarin. Forms](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="disable-selection"></a>Отключить выделение

по умолчанию выбор [`CollectionView`](xref:Xamarin.Forms.CollectionView) отключен. Однако если `CollectionView` включен выбор, его можно отключить, задав свойству [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) значение `None`.

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

Если свойство [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) имеет значение `None`, то элементы в [`CollectionView`](xref:Xamarin.Forms.CollectionView) не могут быть выбраны, свойство [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) остается `null`, а событие [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) не будет запущено.

> [!NOTE]
> При выборе элемента и изменении свойства [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) с `Single` на `None`свойству [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) будет присвоено значение `null`, а событие [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) будет запущено с пустым свойством `CurrentSelection`.

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Диспетчер визуальных состояний Xamarin. Forms](~/xamarin-forms/user-interface/visual-state-manager.md)
