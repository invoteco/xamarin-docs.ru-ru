---
title: Свипевиев Xamarin. Forms
description: Свипевиев Xamarin. Forms — это контейнерный элемент управления, который создает оболочку вокруг элемента содержимого и предоставляет элементы контекстного меню, которые выводятся с помощью жеста прокрутки.
ms.prod: xamarin
ms.assetId: 602456B5-701B-4948-B454-B1F31283F1CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/11/2020
ms.openlocfilehash: 6131287b200846a033e0c476d7039dfd774cab68
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635593"
---
# <a name="xamarinforms-swipeview"></a>Свипевиев Xamarin. Forms

![](~/media/shared/preview.png "This API is currently pre-release")

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-swipeviewdemos/)

`SwipeView` представляет собой контейнерный элемент управления, который обходит элемент содержимого и предоставляет элементы контекстного меню, которые выводятся с помощью жеста прокрутки:

[![Снимок экрана Свипевиев: считывание элементов в CollectionView на iOS и Android](swipeview-images/swipeview-collectionview.png "Свипевиев считывание элементов")](swipeview-images/swipeview-collectionview-large.png#lightbox "Свипевиев считывание элементов")

`SwipeView` доступен в Xamarin. Forms 4,4. Однако в настоящее время это экспериментальное и может использоваться только путем добавления следующей строки кода в класс `AppDelegate` в iOS, в класс `MainActivity` в Android или в класс `App` в UWP перед вызовом `Forms.Init`:

```csharp
Forms.SetFlags("SwipeView_Experimental");
```

`SwipeView` определяет следующие свойства:

- `LeftItems`, типа `SwipeItems`, который представляет элементы считывания, которые могут быть вызваны при считывании элемента управления с левой стороны.
- `RightItems`, типа `SwipeItems`, который представляет элементы считывания, которые могут быть вызваны при считывании элемента управления с правой стороны.
- `TopItems`, типа `SwipeItems`, который представляет элементы считывания, которые могут быть вызваны при прокрутке элемента управления сверху вниз.
- `BottomItems`, типа `SwipeItems`, который представляет элементы считывания, которые могут быть вызваны при прокрутке элемента управления снизу вверх.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

Кроме того, `SwipeView` наследует свойство [`Content`](xref:Xamarin.Forms.ContentView.Content) из класса [`ContentView`](xref:Xamarin.Forms.ContentView) . Свойство `Content` является свойством Content класса `SwipeView`, поэтому его не нужно задавать явно.

Класс `SwipeView` также определяет четыре события:

- `SwipeStarted` запускается при начале считывания. Объект `SwipeStartedEventArgs`, сопровождающий это событие, имеет свойство `SwipeDirection` типа `SwipeDirection`.
- `SwipeChanging` запускается при перемещении прокрутки. Объект `SwipeChangingEventArgs`, сопровождающий это событие, имеет свойство `SwipeDirection` типа `SwipeDirection`и свойство `Offset` типа `double`.
- `SwipeEnded` запускается по окончании прокрутки. Объект `SwipeEndedEventArgs`, сопровождающий это событие, имеет свойство `SwipeDirection` типа `SwipeDirection`.
- `CloseRequested` срабатывает при закрытии считывания элементов.

Кроме того, `SwipeView` определяет метод `Close`, который закрывает прокрутку элементов.

> [!NOTE]
> `SwipeView` имеет зависящую от платформы платформу iOS и Android, которая управляет переходом, используемым при открытии `SwipeView`. Дополнительные сведения см. в разделе [Свипевиев считывание режима переходов в iOS](~/xamarin-forms/platform/ios/swipeview-swipetransitionmode.md) и [Свипевиев, проведите переход на Android](~/xamarin-forms/platform/android/swipeview-swipetransitionmode.md).

## <a name="create-a-swipeview"></a>Создание Свипевиев

`SwipeView` должен определять содержимое, вокруг которого `SwipeView`ся оболочка, и прокрутки элементов, которые выводятся с помощью жеста прокрутки. Единицами прокрутки являются один или несколько `SwipeItem` объектов, помещенных в одну из четырех `SwipeView` направлений `LeftItems`, `RightItems`, `TopItems`или `BottomItems`.

В следующем примере показано, как создать экземпляр `SwipeView` в XAML:

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems>
            <SwipeItem Text="Favorite"
                       IconImageSource="favorite.png"
                       BackgroundColor="LightGreen"
                       Invoked="OnFavoriteSwipeItemInvoked" />
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Invoked="OnDeleteSwipeItemInvoked" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
    <Grid HeightRequest="60"
          WidthRequest="300"
          BackgroundColor="LightGray">
        <Label Text="Swipe right"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
    </Grid>
</SwipeView>
```

Эквивалентный код на C# выглядит так:

```csharp
// SwipeItems
SwipeItem favoriteSwipeItem = new SwipeItem
{
    Text = "Favorite",
    IconImageSource = "favorite.png",
    BackgroundColor = Color.LightGreen
};
favoriteSwipeItem.Invoked += OnFavoriteSwipeItemInvoked;

SwipeItem deleteSwipeItem = new SwipeItem
{
    Text = "Delete",
    IconImageSource = "delete.png",
    BackgroundColor = Color.LightPink
};
deleteSwipeItem.Invoked += OnDeleteSwipeItemInvoked;

List<SwipeItem> swipeItems = new List<SwipeItem>() { favoriteSwipeItem, deleteSwipeItem };

// SwipeView content
Grid grid = new Grid
{
    HeightRequest = 60,
    WidthRequest = 300,
    BackgroundColor = Color.LightGray
};
grid.Children.Add(new Label
{
    Text = "Swipe right",
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center
});

SwipeView swipeView = new SwipeView
{
    LeftItems = new SwipeItems(swipeItems),
    Content = grid
};
```

В этом примере содержимое `SwipeView` — это [`Grid`](xref:Xamarin.Forms.Grid) , содержащий [`Label`](xref:Xamarin.Forms.Label):

[![Снимок экрана с содержимым Свипевиев в iOS и Android](swipeview-images/swipeview-content.png "Свипевиев содержимое")](swipeview-images/swipeview-content-large.png#lightbox "Свипевиев содержимое")

Элементы считывания используются для выполнения действий с `SwipeView` содержимым и отображаются при считывании элемента управления с левой стороны:

[![Снимок экрана с Свипевиев считывания элементов в iOS и Android](swipeview-images/swipeview-swipeitems.png "Свипевиев считывание элементов")](swipeview-images/swipeview-swipeitems-large.png#lightbox "Свипевиев считывание элементов")

По умолчанию элемент прокрутки выполняется при касании пользователем. Хотя это можно изменить. Дополнительные сведения см. в разделе [режим прокрутки](#swipe-mode).

После выполнения элемента считывания элементы прокрутки скрываются, а содержимое `SwipeView` снова отображается. Хотя это можно изменить. Дополнительные сведения см. в разделе [поведение при прокрутке](#swipe-behavior).

> [!NOTE]
> Прокрутка содержимого и считывание элементов можно разместить в строке или определить как ресурсы.

## <a name="swipe-items"></a>Прокрутка элементов

Коллекции `LeftItems`, `RightItems`, `TopItems`и `BottomItems` имеют тип `SwipeItems`. Класс `SwipeItems` определяет следующие свойства:

- `Mode`, типа `SwipeMode`, который указывает на результат взаимодействия при считывании. Дополнительные сведения о режиме прокрутки см. в разделе [режим прокрутки](#swipe-mode).
- `SwipeBehaviorOnInvoked`, типа `SwipeBehaviorOnInvoked`, который указывает, как `SwipeView` ведет себя после вызова элемента считывания. Дополнительные сведения о поведении при прокрутке см. в разделе [поведение при прокрутке](#swipe-behavior).

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

Каждый элемент прокрутки определяется как объект `SwipeItem`, который помещается в одну из четырех `SwipeItems` направленных коллекций. Класс `SwipeItem` является производным от класса [`MenuItem`](xref:Xamarin.Forms.MenuItem) и добавляет следующие члены:

- Свойство `BackgroundColor` типа `Color`, определяющее цвет фона для элемента прокрутки. Это свойство поддерживается связываемым свойством.
- Событие `Invoked`, которое срабатывает при выполнении элемента считывания.

> [!IMPORTANT]
> Класс [`MenuItem`](xref:Xamarin.Forms.MenuItem) определяет несколько свойств, включая `Command`, `CommandParameter`, `IconImageSource`и `Text`. Эти свойства можно задать для объекта `SwipeItem`, чтобы определить его внешний вид, и определить `ICommand`, который выполняется при вызове элемента считывания. Дополнительные сведения см. в разделе [меню Xamarin. Forms MenuItem](~/xamarin-forms/user-interface/menuitem.md).

В следующем примере показаны два `SwipeItem` объектов в коллекции `LeftItems` `SwipeView`:

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems>
            <SwipeItem Text="Favorite"
                       IconImageSource="favorite.png"
                       BackgroundColor="LightGreen"
                       Invoked="OnFavoriteSwipeItemInvoked" />
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Invoked="OnDeleteSwipeItemInvoked" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
</SwipeView>
```

Внешний вид каждого `SwipeItem` определяется сочетанием свойств `Text`, `IconImageSource`и `BackgroundColor`:

[![Снимок экрана с Свипевиев считывания элементов в iOS и Android](swipeview-images/swipeview-swipeitems.png "Свипевиев считывание элементов")](swipeview-images/swipeview-swipeitems-large.png#lightbox "Свипевиев считывание элементов")

При касании `SwipeItem` срабатывает его `Invoked` событие, которое обрабатывается его зарегистрированным обработчиком событий. Кроме того, свойству `Command` можно присвоить `ICommand` реализацию, которая будет выполняться при вызове `SwipeItem`.

> [!NOTE]
> Если внешний вид `SwipeItem` определяется только с помощью свойств `Text` или `IconImageSource`, содержимое всегда выравнивается по центру.

Помимо определения элементов считывания в качестве `SwipeItem` объектов, можно также определить пользовательские представления элементов прокрутки. Дополнительные сведения см. в разделе [настраиваемое считывание элементов](#custom-swipe-items).

## <a name="swipe-direction"></a>Направление прокрутки

`SwipeView` поддерживает четыре разных направления прокрутки с направлением считывания, определяемым направленной `SwipeItems`ной коллекцией, к которой добавляются объекты `SwipeItem`. Каждое направление прокрутки может содержать собственные элементы для считывания. Например, в следующем примере показаны `SwipeView`, для которых элементы прокрутки зависят от направления прокрутки:

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems>
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Command="{Binding DeleteCommand}" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <SwipeView.RightItems>
        <SwipeItems>
            <SwipeItem Text="Favorite"
                       IconImageSource="favorite.png"
                       BackgroundColor="LightGreen"
                       Command="{Binding FavoriteCommand}" />
            <SwipeItem Text="Share"
                       IconImageSource="share.png"
                       BackgroundColor="LightYellow"
                       Command="{Binding ShareCommand}" />
        </SwipeItems>
    </SwipeView.RightItems>
    <!-- Content -->
</SwipeView>
```

В этом примере содержимое `SwipeView` можно прокрутить вправо или влево. В результате прокрутки вправо отображается элемент **Удалить** прокрутка, а при прокрутке влево — элементы " **Избранное** " и " **Общий** Просмотр".

> [!WARNING]
> В `SwipeView`может быть задан только один экземпляр направленной `SwipeItems`ной коллекции. Поэтому в `SwipeView`не может быть двух определений `LeftItems`.

События `SwipeStarted`, `SwipeChanging`и `SwipeEnded` сообщают направление прокрутки через свойство `SwipeDirection` в аргументах события. Это свойство имеет тип `SwipeDirection`, который представляет собой перечисление, состоящее из четырех членов:

- `Right` указывает, что произошло право прокрутки.
- `Left` указывает, что произошло прокрутка влево.
- `Up` указывает, что произошло предыдущее считывание.
- `Down` указывает, что произошло прокрутка вниз.

## <a name="swipe-mode"></a>Режим прокрутки

Класс `SwipeItems` имеет свойство `Mode`, которое указывает на результат взаимодействия при считывании. Этому свойству должно быть присвоено значение одного из членов перечисления `SwipeMode`:

- `Reveal` указывает, что прокрутка показывает элементы считывания. Это значение по умолчанию для свойства `SwipeItems.Mode`.
- `Execute` указывает, что прокрутка выполняет прокрутку элементов.

В режиме открытия пользователь выполняет `SwipeView`, чтобы открыть меню, состоящее из одного или нескольких элементов считывания, и для его выполнения необходимо явно коснуться элемента для прокрутки. После выполнения элемента прокрутки элементы прокрутки закрываются, а содержимое `SwipeView` снова отображается. В режиме выполнения пользователь выполняет `SwipeView`, чтобы открыть меню, состоящее из одного большего количества элементов, которые затем автоматически выполняются. После выполнения прокрутка элементов закрывается, а содержимое `SwipeView` снова отображается.

В следующем примере показан `SwipeView`, настроенный для использования режима выполнения.

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems Mode="Execute">
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Command="{Binding DeleteCommand}" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
</SwipeView>
```

В этом примере содержимое `SwipeView` можно прокрутить вправо, чтобы отобразить элемент считывания, который выполняется немедленно. После выполнения `SwipeView` содержимое снова отображается.

## <a name="swipe-behavior"></a>Поведение при прокрутке

Класс `SwipeItems` имеет свойство `SwipeBehaviorOnInvoked`, которое указывает, как `SwipeView` ведет себя после вызова элемента считывания. Этому свойству должно быть присвоено значение одного из членов перечисления `SwipeBehaviorOnInvoked`:

- `Auto` указывает, что в режиме отображения `SwipeView` закрывается после вызова элемента считывания и в режиме выполнения `SwipeView` остается открытым после вызова элемента считывания. Это значение по умолчанию для свойства `SwipeItems.SwipeBehaviorOnInvoked`.
- `Close` указывает, что `SwipeView` закрывается после вызова элемента считывания.
- `RemainOpen` указывает, что `SwipeView` остается открытым после вызова элемента считывания.

В следующем примере показан `SwipeView`, настроенный, чтобы оставаться открытым после вызова элемента считывания:

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems SwipeBehaviorOnInvoked="RemainOpen">
            <SwipeItem Text="Favorite"
                       IconImageSource="favorite.png"
                       BackgroundColor="LightGreen"
                       Invoked="OnFavoriteSwipeItemInvoked" />
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Invoked="OnDeleteSwipeItemInvoked" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
</SwipeView>
```

## <a name="custom-swipe-items"></a>Пользовательские элементы прокрутки

Пользовательские элементы прокрутки можно определить с помощью типа `SwipeItemView`. Класс `SwipeItemView` является производным от класса [`ContentView`](xref:Xamarin.Forms.ContentView) и добавляет следующие свойства:

- `Command`, типа `ICommand`, который выполняется при касании элемента прокрутки.
- `CommandParameter` с типом `object`, который передается как параметр в `Command`.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

Класс `SwipeItemView` также определяет событие `Invoked`, которое возникает при касании элемента, после выполнения `Command`.

В следующем примере показан объект `SwipeItemView` в коллекции `LeftItems` `SwipeView`.

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems>
            <SwipeItemView Command="{Binding CheckAnswerCommand}"
                           CommandParameter="{Binding Source={x:Reference resultEntry}, Path=Text}">
                <StackLayout Margin="10"
                             WidthRequest="300">
                    <Entry x:Name="resultEntry"
                           Placeholder="Enter answer"
                           HorizontalOptions="CenterAndExpand" />
                    <Label Text="Check"
                           FontAttributes="Bold"
                           HorizontalOptions="Center" />
                </StackLayout>
            </SwipeItemView>
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
</SwipeView>
```

В этом примере `SwipeItemView` состоит из [`StackLayout`](xref:Xamarin.Forms.StackLayout) , содержащего [`Entry`](xref:Xamarin.Forms.Entry) и [`Label`](xref:Xamarin.Forms.Label). После того как пользователь введет входные данные в `Entry`, остальную часть `SwipeViewItem` можно коснуться, которая выполняет `ICommand`, определяемую свойством `SwipeItemView.Command`.

## <a name="disable-a-swipeview"></a>Отключение Свипевиев

Приложение может ввести состояние, при котором считывание элемента содержимого не является допустимой операцией. В таких случаях `SwipeView` можно отключить, задав свойству `IsEnabled` значение `false`. Это не позволит пользователям выполнять считывание содержимого для отображения элементов считывания.

Кроме того, при определении `Command` свойства `SwipeItem` или `SwipeItemView`можно указать делегат `CanExecute` `ICommand`, чтобы включить или отключить элемент прокрутки.

## <a name="related-links"></a>Связанные ссылки

- [Свипевиев (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-swipeviewdemos/)
- [Xamarin.Forms MenuItem](~/xamarin-forms/user-interface/menuitem.md)
