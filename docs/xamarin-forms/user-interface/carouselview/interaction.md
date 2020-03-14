---
title: Взаимодействие Карауселвиев Xamarin. Forms
description: Доступ к отображаемому в данный момент элементу в Карауселвиев можно получить с помощью свойств CurrentItem и позиции.
ms.prod: xamarin
ms.assetid: 854D97E5-D119-4BE2-AE7C-BD428792C992
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/11/2020
ms.openlocfilehash: 150c358346f90a513e1558dc847ad7eb6dd6e6e2
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305963"
---
# <a name="xamarinforms-carouselview-interaction"></a>Взаимодействие Карауселвиев Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView) определяет следующие свойства, управляющие взаимодействием с пользователем.

- `CurrentItem`, тип `object`, текущий отображаемый элемент. Это свойство имеет режим привязки по умолчанию `TwoWay`и имеет `null` значение, если нет данных для вывода.
- `CurrentItemChangedCommand`, типа `ICommand`, который выполняется при изменении текущего элемента.
- `CurrentItemChangedCommandParameter` с типом `object`, который передается как параметр в `CurrentItemChangedCommand`.
- `IsBounceEnabled`, тип `bool`, указывающий, будет ли `CarouselView` возникнет на границе содержимого. Значение по умолчанию — `true`.
- `IsSwipeEnabled`, тип `bool`, определяющий, будет ли жест прокрутки изменять отображаемый элемент. Значение по умолчанию — `true`.
- `Position`, типа `int`, индекс текущего элемента в базовой коллекции. Это свойство имеет режим привязки по умолчанию `TwoWay`и имеет значение 0, если нет данных для вывода.
- `PositionChangedCommand`, типа `ICommand`, который выполняется при изменении расположения.
- `PositionChangedCommandParameter` с типом `object`, который передается как параметр в `PositionChangedCommand`.
- `VisibleViews`, типа `ObservableCollection<View>`, который является свойством только для чтения и содержит объекты для элементов, видимых в данный момент.

Все эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть их можно указывать в качестве целевых для привязки данных.

[`CarouselView`](xref:Xamarin.Forms.CarouselView) определяет событие `CurrentItemChanged`, которое возникает при изменении свойства `CurrentItem` либо из-за прокрутки пользователем, либо когда приложение задает свойство. Объект `CurrentItemChangedEventArgs`, сопровождающий событие `CurrentItemChanged`, имеет два свойства типа `object`:

- `PreviousItem` — предыдущий элемент после изменения свойства.
- `CurrentItem` — текущий элемент после изменения свойства.

[`CarouselView`](xref:Xamarin.Forms.CarouselView) также определяет `PositionChanged` событие, которое возникает при изменении свойства `Position` либо из-за прокрутки пользователем, либо когда приложение задает свойство. Объект `PositionChangedEventArgs`, сопровождающий событие `PositionChanged`, имеет два свойства типа `int`:

- `PreviousPosition` — предыдущее расположение после изменения свойства.
- `CurrentPosition` — текущее расположение после изменения свойства.

## <a name="respond-to-the-current-item-changing"></a>Ответ на изменение текущего элемента

При изменении отображаемого в данный момент элемента свойству `CurrentItem` будет присвоено значение элемента. При изменении этого свойства `CurrentItemChangedCommand` выполняется со значением `CurrentItemChangedCommandParameter`, передаваемого в `ICommand`. После этого обновляется свойство `Position`, и возникает событие `CurrentItemChanged`.

> [!IMPORTANT]
> Свойство `Position` меняется при изменении свойства `CurrentItem`. Это приведет к выполнению `PositionChangedCommand` и срабатыванию `PositionChanged` события.

### <a name="event"></a>Событие

В следующем примере XAML показан [`CarouselView`](xref:Xamarin.Forms.CarouselView) , использующий обработчик событий для реагирования на изменение текущего элемента:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              CurrentItemChanged="OnCurrentItemChanged">
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.CurrentItemChanged += OnCurrentItemChanged;
```

В этом примере обработчик событий `OnCurrentItemChanged` выполняется при срабатывании события `CurrentItemChanged`:

```csharp
void OnCurrentItemChanged(object sender, CurrentItemChangedEventArgs e)
{
    Monkey previousItem = e.PreviousItem as Monkey;
    Monkey currentItem = e.CurrentItem as Monkey;
}
```

В этом примере обработчик событий `OnCurrentItemChanged` предоставляет предыдущий и текущий элементы:

[![Снимок экрана Карауселвиев с предыдущими и текущими элементами в iOS и Android](interaction-images/current-item-events.png "Карауселвиев с текущими и предыдущими элементами")](interaction-images/current-item-events-large.png#lightbox "Карауселвиев с текущими и предыдущими элементами")

### <a name="command"></a>Get-Help

В следующем примере XAML показан [`CarouselView`](xref:Xamarin.Forms.CarouselView) , использующий команду для реагирования на изменение текущего элемента:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              CurrentItemChangedCommand="{Binding ItemChangedCommand}"
              CurrentItemChangedCommandParameter="{Binding Source={RelativeSource Self}, Path=CurrentItem}">
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.SetBinding(CarouselView.CurrentItemChangedCommandProperty, "ItemChangedCommand");
carouselView.SetBinding(CarouselView.CurrentItemChangedCommandParameterProperty, new Binding("CurrentItem", source: RelativeBindingSource.Self));
```

В этом примере свойство `CurrentItemChangedCommand` привязывается к свойству `ItemChangedCommand`, передавая ему значение свойства `CurrentItem` в качестве аргумента. После этого `ItemChangedCommand` может реагировать на изменение текущего элемента по мере необходимости:

```csharp
public ICommand ItemChangedCommand => new Command<Monkey>((item) =>
{
    PreviousMonkey = CurrentMonkey;
    CurrentMonkey = item;
});
```

В этом примере `ItemChangedCommand` обновляет объекты, в которых хранятся предыдущие и текущие элементы.

## <a name="respond-to-the-position-changing"></a>Реагирование на изменение расположения

Когда отображаемый в данный момент элемент изменяется, для свойства `Position` будет задан индекс текущего элемента в базовой коллекции. При изменении этого свойства `PositionChangedCommand` выполняется со значением `PositionChangedCommandParameter`, передаваемого в `ICommand`. Затем срабатывает событие `PositionChanged`. Если свойство `Position` было изменено программно, [`CarouselView`](xref:Xamarin.Forms.CarouselView) будет прокручиваться до элемента, соответствующего значению `Position`.

> [!NOTE]
> Установка свойства `Position` в значение 0 приведет к отображению первого элемента в базовой коллекции.

### <a name="event"></a>Событие

В следующем примере XAML показан [`CarouselView`](xref:Xamarin.Forms.CarouselView) , использующий обработчик событий для реагирования на изменение свойства `Position`:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"              
              PositionChanged="OnPositionChanged">
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.PositionChanged += OnPositionChanged;
```

В этом примере обработчик событий `OnPositionChanged` выполняется при срабатывании события `PositionChanged`:

```csharp
void OnPositionChanged(object sender, PositionChangedEventArgs e)
{
    int previousItemPosition = e.PreviousPosition;
    int currentItemPosition = e.CurrentPosition;
}
```

В этом примере обработчик событий `OnCurrentItemChanged` предоставляет предыдущую и текущую позиции:

[![Снимок экрана Карауселвиев с предыдущими и текущими позициями в iOS и Android](interaction-images/current-position-events.png "Карауселвиев с текущими и предыдущими позициями")](interaction-images/current-position-events-large.png#lightbox "Карауселвиев с текущими и предыдущими позициями")

### <a name="command"></a>Get-Help

В следующем примере XAML показан [`CarouselView`](xref:Xamarin.Forms.CarouselView) , использующий команду для реагирования на изменение свойства `Position`:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PositionChangedCommand="{Binding PositionChangedCommand}"
              PositionChangedCommandParameter="{Binding Source={RelativeSource Self}, Path=Position}">
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.SetBinding(CarouselView.PositionChangedCommandProperty, "PositionChangedCommand");
carouselView.SetBinding(CarouselView.PositionChangedCommandParameterProperty, new Binding("Position", source: RelativeBindingSource.Self));
```

В этом примере свойство `PositionChangedCommand` привязывается к свойству `PositionChangedCommand`, передавая ему значение свойства `Position` в качестве аргумента. После этого `PositionChangedCommand` может реагировать на изменение расположения, как это необходимо:

```csharp
public ICommand PositionChangedCommand => new Command<int>((position) =>
{
    PreviousPosition = CurrentPosition;
    CurrentPosition = position;
});
```

В этом примере `PositionChangedCommand` обновляет объекты, в которых хранятся предыдущие и текущие позиции.

## <a name="preset-the-current-item"></a>Предустановка текущего элемента

Текущий элемент в [`CarouselView`](xref:Xamarin.Forms.CarouselView) может быть установлен программным путем задания свойства `CurrentItem` для элемента. В следующем примере XAML показан `CarouselView`, который предварительно выбирает текущий элемент:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              CurrentItem="{Binding CurrentItem}">
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.SetBinding(CarouselView.CurrentItemProperty, "CurrentItem");
```

> [!NOTE]
> Свойство `CurrentItem` имеет режим привязки по умолчанию `TwoWay`.

Данные свойства `CarouselView.CurrentItem` привязываются к свойству `CurrentItem` подключенной модели представления, которая имеет тип `Monkey`. По умолчанию используется привязка `TwoWay` таким образом, что если пользователь изменяет текущий элемент, значение свойства `CurrentItem` будет установлено в текущий объект `Monkey`. Свойство `CurrentItem` определено в классе `MonkeysViewModel`:

```csharp
public class MonkeysViewModel : INotifyPropertyChanged
{
    // ...
    public ObservableCollection<Monkey> Monkeys { get; private set; }

    public Monkey CurrentItem { get; set; }

    public MonkeysViewModel()
    {
        // ...
        CurrentItem = Monkeys.Skip(3).FirstOrDefault();
        OnPropertyChanged("CurrentItem");
    }
}
```

В этом примере свойству `CurrentItem` присвоено четвертый элемент в коллекции `Monkeys`:

[![Снимок экрана Карауселвиев с элементом предустановки в iOS и Android](interaction-images/preset-item.png "Карауселвиев с предварительно заданным элементом")](interaction-images/preset-item-large.png#lightbox "Карауселвиев с предварительно заданным элементом")

## <a name="preset-the-position"></a>Предустановка расположения

Отображаемый элемент [`CarouselView`](xref:Xamarin.Forms.CarouselView) может быть задан программно путем установки свойства `Position` в индекс элемента в базовой коллекции. В следующем примере XAML показан `CarouselView`, который задает отображаемый элемент:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              Position="{Binding Position}">
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.SetBinding(CarouselView.PositionProperty, "Position");
```

> [!NOTE]
> Свойство `Position` имеет режим привязки по умолчанию `TwoWay`.

Данные свойства `CarouselView.Position` привязываются к свойству `Position` подключенной модели представления, которая имеет тип `int`. По умолчанию используется привязка `TwoWay`, так что если пользователь прокручивается по [`CarouselView`](xref:Xamarin.Forms.CarouselView), то значение свойства `Position` будет равно индексу отображаемого элемента. Свойство `Position` определено в классе `MonkeysViewModel`:

```csharp
public class MonkeysViewModel : INotifyPropertyChanged
{
    // ...
    public int Position { get; set; }

    public MonkeysViewModel()
    {
        // ...
        Position = 3;
        OnPropertyChanged("Position");
    }
}
```

В этом примере свойству `Position` присвоено четвертый элемент в коллекции `Monkeys`:

[![Снимок экрана Карауселвиев с заданной позицией в iOS и Android](interaction-images/preset-position.png "Карауселвиев с заданной позицией")](interaction-images/preset-position-large.png#lightbox "Карауселвиев с заданной позицией")

## <a name="define-visual-states"></a>Определение визуальных состояний

[`CarouselView`](xref:Xamarin.Forms.CarouselView) определяет четыре визуальных состояния:

- `CurrentItem` представляет визуальное состояние для отображаемого в данный момент элемента.
- `PreviousItem` представляет визуальное состояние для ранее отображаемого элемента.
- `NextItem` представляет визуальное состояние для следующего элемента.
- `DefaultItem` представляет визуальное состояние для оставшейся части элементов.

Эти визуальные состояния можно использовать для инициации визуальных изменений элементов, отображаемых [`CarouselView`](xref:Xamarin.Forms.CarouselView).

В следующем примере XAML показано, как определить `CurrentItem`, `PreviousItem`, `NextItem`и `DefaultItem` визуальные состояния.

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    <CarouselView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                <VisualStateManager.VisualStateGroups>
                    <VisualStateGroup x:Name="CommonStates">
                        <VisualState x:Name="CurrentItem">
                            <VisualState.Setters>
                                <Setter Property="Scale"
                                        Value="1.1" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState x:Name="PreviousItem">
                            <VisualState.Setters>
                                <Setter Property="Opacity"
                                        Value="0.5" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState x:Name="NextItem">
                            <VisualState.Setters>
                                <Setter Property="Opacity"
                                        Value="0.5" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState x:Name="DefaultItem">
                            <VisualState.Setters>
                                <Setter Property="Opacity"
                                        Value="0.25" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateManager.VisualStateGroups>

                <!-- Item template content -->
                <Frame HasShadow="true">
                    ...
                </Frame>
            </StackLayout>
        </DataTemplate>
    </CarouselView.ItemTemplate>
</CarouselView>
```

В этом примере `CurrentItem` визуальное состояние указывает, что для текущего элемента, отображаемого [`CarouselView`](xref:Xamarin.Forms.CarouselView) , свойство [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) будет изменено со значения по умолчанию от 1 до 1,1. Визуальные состояния `PreviousItem` и `NextItem` указывают, что элементы, окружающие текущий элемент, будут отображаться с [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) значением 0,5. `DefaultItem` визуальное состояние указывает, что оставшаяся часть элементов, отображаемых `CarouselView`, будет отображаться с `Opacity` значением 0,25.

> [!NOTE]
> Кроме того, визуальные состояния можно определить в [`Style`](xref:Xamarin.Forms.Style) со значением свойства [`TargetType`](xref:Xamarin.Forms.Style.TargetType) , которое является типом корневого элемента [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), который задан как значение свойства `ItemTemplate`.

На следующих снимках экрана показаны `CurrentItem`, `PreviousItem`и визуальные состояния `NextItem`.

[![Снимок экрана Карауселвиев с использованием визуальных состояний в iOS и Android](interaction-images/visual-states.png "Визуальные состояния Карауселвиев")](interaction-images/visual-states-large.png#lightbox "Визуальные состояния Карауселвиев")

Дополнительные сведения о визуальных состояниях см. в разделе [Диспетчер визуальных состояний Xamarin. Forms](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="clear-the-current-item"></a>Очистить текущий элемент

Свойство `CurrentItem` можно очистить, задавая его или объект, к которому он привязан, для `null`.

## <a name="disable-bounce"></a>Отключить Bounce

По умолчанию [`CarouselView`](xref:Xamarin.Forms.CarouselView) посылает элементы на границах содержимого. Это можно отключить, задав для свойства `IsBounceEnabled` значение `false`.

## <a name="disable-swipe-interaction"></a>Отключить взаимодействие при прокрутке

По умолчанию [`CarouselView`](xref:Xamarin.Forms.CarouselView) позволяет пользователям перемещаться по элементам с помощью жеста прокрутки. Это взаимодействие по прокрутке можно отключить, задав для свойства `IsSwipeEnabled` значение `false`.

## <a name="related-links"></a>Связанные ссылки

- [Карауселвиев (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [Диспетчер визуальных состояний Xamarin. Forms](~/xamarin-forms/user-interface/visual-state-manager.md)
