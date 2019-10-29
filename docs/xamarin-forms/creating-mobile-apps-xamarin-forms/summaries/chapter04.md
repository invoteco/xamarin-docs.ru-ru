---
title: Общие сведения о главе 4. Прокрутка стека
description: 'Создание мобильных приложений с помощью Xamarin. Forms: сводка раздела 4. Прокрутка стека'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 7A39FD4F-15AD-4F94-960E-9FEEB63FFD44
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: bda9d5cb323524981bed9c3bb55998513dd69aab
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032876"
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>Общие сведения о главе 4. Прокрутка стека

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)

В этой главе в первую очередь рассматривается понятие *макета*, которое является общим термином для классов и методик, используемых Xamarin. Forms для организации визуального отображения нескольких представлений на странице.

Макет включает несколько классов, производных от [`Layout`](xref:Xamarin.Forms.Layout) и [`Layout<T>`](xref:Xamarin.Forms.Layout`1). Эта глава посвящена [`StackLayout`](xref:Xamarin.Forms.StackLayout).

> [!NOTE]
> [`FlexLayout`](~/xamarin-forms/user-interface/layouts/flex-layout.md) , введенные в Xamarin. forms 3,0, могут использоваться способами, аналогичными `StackLayout` но с большей гибкостью.

В этой главе также представлены классы [`ScrollView`](xref:Xamarin.Forms.ScrollView), [`Frame`](xref:Xamarin.Forms.Frame)и [`BoxView`](xref:Xamarin.Forms.BoxView) .

## <a name="stacks-of-views"></a>Стеки представлений

[`StackLayout`](xref:Xamarin.Forms.StackLayout) является производным от `Layout<View>` и наследует свойство [`Children`](xref:Xamarin.Forms.Layout`1) типа `IList<View>`. В эту коллекцию добавляется несколько элементов представления, а `StackLayout` отображаются в горизонтальном или вертикальном стеке.

Задайте для свойства [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) `StackLayout` элемент перечисления [`StackOrientation`](xref:Xamarin.Forms.StackOrientation) либо [`Vertical`](xref:Xamarin.Forms.StackOrientation.Vertical) , либо [`Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal). Значение по умолчанию: `Vertical`.

Задайте для свойства [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) `StackLayout` значение `double`, чтобы указать интервал между дочерними элементами. Значение по умолчанию — 6.

В коде можно добавлять элементы в коллекцию `Children` `StackLayout` в цикле `for` или `foreach`, как показано в примере [**колорлуп**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop) , или инициализировать коллекцию `Children` списком отдельных представлений, как показано в [**колорлист** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList). Дочерние элементы должны быть производными от `View`, но могут содержать другие объекты `StackLayout`.

## <a name="scrolling-content"></a>Прокрутка содержимого

Если `StackLayout` содержит слишком много дочерних элементов для отображения на странице, можно разместить `StackLayout` в [`ScrollView`](xref:Xamarin.Forms.ScrollView) , чтобы разрешить прокрутку.

Задайте для свойства [`Content`](xref:Xamarin.Forms.ScrollView.Content) `ScrollView` представление, которое нужно прокрутить. Часто это `StackLayout`, но это может быть любое представление.

Задайте для свойства [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation) `ScrollView` элемент свойства [`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation) , [`Vertical`](xref:Xamarin.Forms.ScrollOrientation.Vertical), [`Horizontal`](xref:Xamarin.Forms.ScrollOrientation.Horizontal)или [`Both`](xref:Xamarin.Forms.ScrollOrientation.Both). Значение по умолчанию: `Vertical`. Если содержимое `ScrollView` является `StackLayout`, обе ориентации должны быть единообразными.

В примере [**рефлектедколорс**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors) демонстрируется использование `ScrollView` и `StackLayout` для вывода доступных цветов. В этом примере также показано, как использовать отражение .NET для получения всех открытых статических свойств и полей структуры `Color` без необходимости их явного перечисления.

## <a name="the-expands-option"></a>Параметр Expand

Когда `StackLayout` помещает свои дочерние элементы в стек, каждый дочерний элемент занимает определенную область в пределах общей высоты `StackLayout`, которая зависит от размера дочернего элемента и параметров его `HorizontalOptions` и свойств `VerticalOptions`. Этим свойствам присваиваются значения типа [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions).

Структура `LayoutOptions` определяет два свойства:

- [`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment) типа перечисления [`LayoutAlignment`](xref:Xamarin.Forms.LayoutAlignment) с четырьмя элементами, [`Start`](xref:Xamarin.Forms.LayoutAlignment.Start), [`Center`](xref:Xamarin.Forms.LayoutAlignment.Center), [`End`](xref:Xamarin.Forms.LayoutAlignment.End)и [`Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill)
- [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) типа `bool`

Для удобства структура `LayoutOptions` также определяет восемь статических полей только для чтения типа `LayoutOptions`, которые охватывают все сочетания двух свойств экземпляра:

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

В следующем обсуждении предполагается `StackLayout` с вертикальной ориентацией по умолчанию. Горизонтальный `StackLayout` является аналогом.

Для вертикального `StackLayout`параметр `HorizontalOptions` определяет горизонтальное положение дочернего элемента в пределах ширины `StackLayout`. Параметр `Alignment` `Start`, `Center`или `End` приводит к горизонтальному отсутствию ограничений для дочернего элемента. Дочерний элемент определяет свою собственную ширину и располагается слева, по центру или справа от `StackLayout`. Параметр `Fill` приводит к тому, что дочерний элемент будет горизонтально ограничен и заполнит ширину `StackLayout`.

Для вертикального `StackLayout`каждый дочерний элемент по вертикали не ограничен и получает вертикальный слот в зависимости от высоты дочернего элемента, в этом случае параметр `VerticalOptions` не важен.

Если вертикальный `StackLayout` сам по себе неограниченный&mdash;то есть `VerticalOptions` параметр имеет значение `Start`, `Center`или `End`, то высота `StackLayout`а будет общей высотой дочерних элементов.

Однако, если вертикальный `StackLayout` ограничивается вертикально&mdash;если `VerticalOptions` параметр `Fill`&mdash;то высота `StackLayout` будет высотой контейнера, что может быть больше, чем общая высота его дочерних элементов. Если это так, и если по крайней мере один дочерний элемент имеет параметр `VerticalOptions` с флагом `Expands` `true`, дополнительное пространство `StackLayout` выделяется одинаково между всеми дочерними элементами с флагом `Expands` `true`. После этого общая высота дочернего элемента будет равна высоте `StackLayout`, а `Alignment` часть параметра `VerticalOptions` определяет, как дочерний элемент располагается вертикально в своем слоте.

Это продемонстрировано в примере [**вертикалоптионсдемо**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo) .

## <a name="frame-and-boxview"></a>Frame и Боксвиев

Эти два прямоугольных представления часто используются в целях представления.

Представление [`Frame`](xref:Xamarin.Forms.Frame) отображает прямоугольную рамку вокруг другого представления, которая может быть макетом, например `StackLayout`. `Frame` наследует свойство [`Content`](xref:Xamarin.Forms.ContentView.Content) от [`ContentView`](xref:Xamarin.Forms.ContentView) , для которого задано представление, отображаемое в `Frame`. По умолчанию `Frame` прозрачна. Задайте следующие три свойства для настройки внешнего вида рамки:

- Свойство [`OutlineColor`](xref:Xamarin.Forms.Frame.OutlineColor) , чтобы сделать его видимым. Обычно для `OutlineColor` задано `Color.Accent`, если не известна базовая цветовая схема.
- Для свойства [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow) можно задать значение `true` для показа черной тени на устройствах iOS.
- Задайте для свойства [`Padding`](xref:Xamarin.Forms.Layout.Padding) значение `Thickness`, чтобы оставить пробел между фреймом и содержимым фрейма. Значение по умолчанию — 20 единиц на всех сторонах.

`Frame` по умолчанию имеет `HorizontalOptions` и `VerticalOptions` значения `LayoutOptions.Fill`. Это означает, что `Frame` будет заполнен контейнер. При использовании других параметров Размер `Frame` зависит от размера его содержимого.

`Frame` показано в примере [**фрамедтекст**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText) .

[`BoxView`](xref:Xamarin.Forms.BoxView) отображает прямоугольную область цвета, заданную свойством [`Color`](xref:Xamarin.Forms.BoxView.Color) .

Если `BoxView` ограничивается (свойства `HorizontalOptions` и `VerticalOptions` имеют параметры по умолчанию `LayoutOptions.Fill`), `BoxView` заполняет пространство, доступное для него. Если `BoxView` не ограничена (с параметрами `HorizontalOptions` и `LayoutOptions` `Start`, `Center`или `End`), по умолчанию используется измерение, равное 40 единиц. `BoxView` может быть ограничена в одном измерении и не ограничена в другом.

Часто можно задать свойства [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) и [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) `BoxView`, чтобы присвоить ему конкретный размер. Это продемонстрировано в примере [**сизедбоксвиев**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView) .

Можно использовать несколько экземпляров `StackLayout` для объединения `BoxView` и нескольких экземпляров `Label` в `Frame` для отображения определенного цвета, а затем поместив каждое из этих представлений в `StackLayout` в `ScrollView`, чтобы создать привлекательный список цветов. показано в образце [**колорблоккс**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks) :

[![Тройной снимок экрана цветовых блоков](images/ch04fg11-small.png "Список цветов")](images/ch04fg11-large.png#lightbox "Список цветов")

## <a name="a-scrollview-in-a-stacklayout"></a>Скроллвиев в StackLayout?

Размещение `StackLayout` в `ScrollView` является типичным, но иногда бывает удобно поместить `ScrollView` в `StackLayout`. Теоретически, это не должно быть возможным, так как дочерние элементы вертикального `StackLayout` не ограничены по вертикали. Но `ScrollView` должны быть ограничены по вертикали. Ей необходимо присвоить определенную высоту, чтобы она могла затем определить размер дочернего элемента для прокрутки.

Хитрость заключается в предоставлении `ScrollView` дочернего элемента `StackLayout` `VerticalOptions` параметра `FillAndExpand`. Это продемонстрировано в примере [**блакккат**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat) .

В примере **блакккат** также показано, как определить и получить доступ к ресурсам программы, внедренным в общую библиотеку. Это также можно сделать с помощью проектов общих ресурсов (SAPs), но процесс немного сложнее, как показано в примере [**блакккатсап**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap) .

## <a name="related-links"></a>Связанные ссылки

- [Глава 4. полный текст (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [Глава 4. примеры](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [Глава 4 F# . примеры](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
- [BoxView](~/xamarin-forms/user-interface/boxview.md)
