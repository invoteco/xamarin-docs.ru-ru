---
title: Взаимодействие Карауселвиев Xamarin. Forms
description: Доступ к отображаемому в данный момент элементу в Карауселвиев можно получить с помощью свойств CurrentItem и позиции.
ms.prod: xamarin
ms.assetid: 854D97E5-D119-4BE2-AE7C-BD428792C992
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/14/2019
ms.openlocfilehash: fd85876b38c21c7ff1ea85d7a61c1449395d56f5
ms.sourcegitcommit: e71474f91639bb43159b22f5d534325c3270ba93
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/22/2019
ms.locfileid: "72749791"
---
# <a name="xamarinforms-carouselview-interaction"></a>Взаимодействие Карауселвиев Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView) определяет следующие свойства, управляющие взаимодействием с пользователем.

- `CurrentItem`, тип `object`, текущий отображаемый элемент. Это свойство имеет режим привязки по умолчанию `TwoWay` и имеет `null` значение, если нет данных для вывода.
- `CurrentItemChangedCommand`, типа `ICommand`, который выполняется при изменении текущего элемента.
- `CurrentItemChangedCommandParameter` с типом `object`, который передается как параметр в `CurrentItemChangedCommand`.
- `IsBounceEnabled`, тип `bool`, указывающий, будет ли `CarouselView` возникнет на границе содержимого. Значение по умолчанию — `true`.
- `IsSwipeEnabled`, тип `bool`, определяющий, будет ли жест прокрутки изменять отображаемый элемент. Значение по умолчанию — `true`.
- `Position`, типа `int`, индекс текущего элемента в базовой коллекции. Это свойство имеет режим привязки по умолчанию `TwoWay` и имеет значение 0, если нет данных для вывода.
- `PositionChangedCommand`, типа `ICommand`, который выполняется при изменении расположения.
- `PositionChangedCommandParameter` с типом `object`, который передается как параметр в `PositionChangedCommand`.

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

### <a name="event"></a>событие

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

В этом примере обработчик событий `OnCurrentItemChanged` выполняется при срабатывании события `CurrentItemChanged` с обработчиком событий, который предоставляет предыдущий и текущий элементы:

```csharp
void OnCurrentItemChanged(object sender, CurrentItemChangedEventArgs e)
{
    Monkey previousItem = e.PreviousItem as Monkey;
    Monkey currentItem = e.CurrentItem as Monkey;
}
```

### <a name="command"></a>Команда

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

### <a name="event"></a>событие

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

В этом примере обработчик событий `OnPositionChanged` выполняется при срабатывании события `PositionChanged`, при этом обработчик событий предоставляет предыдущую и текущую позиции:

```csharp
void OnPositionChanged(object sender, PositionChangedEventArgs e)
{
    int previousItemPosition = e.PreviousPosition;
    int currentItemPosition = e.CurrentPosition;
}
```

### <a name="command"></a>Команда

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

Данные свойства `CarouselView.CurrentItem` привязываются к свойству `CurrentItem` подключенной модели представления, которая имеет тип `Monkey`. По умолчанию используется привязка `TwoWay` таким образом, что если пользователь изменяет текущий элемент, значение свойства `CurrentItem` будет установлено в текущий объект `Monkey`. Свойство `CurrentItem` определено в классе `MonkeysViewModel` и устанавливается в четвертый элемент в коллекции `Monkeys`:

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

Данные свойства `CarouselView.Position` привязываются к свойству `Position` подключенной модели представления, которая имеет тип `int`. По умолчанию используется привязка `TwoWay`, так что если пользователь прокручивается по [`CarouselView`](xref:Xamarin.Forms.CarouselView), то значение свойства `Position` будет равно индексу отображаемого элемента. Свойство `Position` определено в классе `MonkeysViewModel` и устанавливается в четвертый элемент в коллекции `Monkeys`:

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

## <a name="clear-the-current-item"></a>Очистить текущий элемент

Свойство `CurrentItem` можно очистить, задавая его или объект, к которому он привязан, для `null`.

## <a name="disable-bounce"></a>Отключить Bounce

По умолчанию [`CarouselView`](xref:Xamarin.Forms.CarouselView) посылает элементы на границах содержимого. Это можно отключить, задав для свойства `IsBounceEnabled` значение `false`.

## <a name="disable-swipe-interaction"></a>Отключить взаимодействие при прокрутке

По умолчанию [`CarouselView`](xref:Xamarin.Forms.CarouselView) позволяет пользователям перемещаться по элементам с помощью жеста прокрутки. Это взаимодействие по прокрутке можно отключить, задав для свойства `IsSwipeEnabled` значение `false`.

## <a name="related-links"></a>Связанные ссылки

- [Карауселвиев (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
