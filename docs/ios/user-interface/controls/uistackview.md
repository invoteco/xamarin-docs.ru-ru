---
title: Представления стеков в Xamarin. iOS
description: В этой статье описывается использование нового элемента управления Уистакквиев в приложении Xamarin. iOS для управления набором подпредставлений в стеке по горизонтали или по вертикали.
ms.prod: xamarin
ms.assetid: 20246E87-2A49-438A-9BD7-756A1B50A617
ms.technology: xamarin-ios
ms.custom: xamu-video
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: b4a8507d4d1497964f6b60307622ca3e1dc4cd90
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021795"
---
# <a name="stack-views-in-xamarinios"></a>Представления стеков в Xamarin. iOS

_В этой статье описывается использование нового элемента управления Уистакквиев в приложении Xamarin. iOS для управления набором подпредставлений в стеке по горизонтали или по вертикали._

> [!IMPORTANT]
> Обратите внимание, что хотя Стакквиев поддерживается в конструкторе iOS, вы можете столкнуться с ошибками использования при использовании стабильного канала. Чтобы решить эту проблему, необходимо переключить бета-и видеоканалы. Мы решили показать это пошаговое руководство с помощью Xcode, пока необходимые исправления не будут реализованы в стабильном канале.

Элемент управления "представление стека" (`UIStackView`) использует возможности классов автоматического макета и размера для управления стеком подпредставлений (горизонтально или вертикально), который динамически реагирует на ориентацию и размер экрана устройства iOS.

Макет всех вложенных представлений, присоединенных к представлению «стек», управляется на основе свойств, определенных разработчиком, таких как ось, распределение, выравнивание и отступы:

[![](uistackview-images/stacked01.png "Stack View layout diagram")](uistackview-images/stacked01.png#lightbox)

При использовании `UIStackView` в приложении Xamarin. iOS разработчик может либо определять вложенные представления в раскадровке в конструкторе iOS, либо добавлять и удалять подпредставления в C# коде.

Этот документ состоит из двух частей: Краткое руководство по реализации первого представления стека, а также некоторые дополнительные технические сведения о его работе.

> [!VIDEO https://youtube.com/embed/p3po6507Ip8]

**Видео Уистакквиев**

## <a name="uistackview-quickstart"></a>Краткое руководство по Уистакквиев

В качестве краткого знакомства с элементом управления `UIStackView` мы создадим простой интерфейс, позволяющий пользователю ввести оценку от 1 до 5. Мы будем использовать два представления стека: один для вертикального расположения на экране устройства и один для упорядочивания значков рейтинга 1-5 по горизонтали на экране.

### <a name="define-the-ui"></a>Определение пользовательского интерфейса

Запустите новый проект Xamarin. iOS и измените файл **Main. Storyboard** в Interface Builder Xcode. Сначала перетащите одно **вертикальное представление стека** на **контроллере представления**:

[![](uistackview-images/quick01.png "Drag a single Vertical Stack View on the View Controller")](uistackview-images/quick01.png#lightbox)

В **инспекторе атрибутов**задайте следующие параметры.

[![](uistackview-images/quick02.png "Set the Stack View options")](uistackview-images/quick02.png#lightbox)

Где:

- **Axis** — определяет, будет ли представление стека упорядочивать подпредставления **горизонтально** или **вертикально**.
- **Выравнивание** — управляет выравниванием вложенных представлений в представлении стека.
- **Распространение** — управляет изменением размера вложенных представлений в представлении стека.
- **Пробел** — управляет минимальным пространством между каждым подпредставлением в представлении Stack.
- **Относительные базовые показатели** — если этот флажок установлен, то по вертикали между всеми подпредставлениями будет получен производный промежуток от базового.
- **Поля макета относительные** — размещает подпредставления относительно стандартных полей макета.

При работе с представлением стека можно представить **Выравнивание** в виде **координат X** и **Y** для подпредставления и **распределения** по **высоте** и **ширине**.

> [!IMPORTANT]
> `UIStackView` предназначено как представление контейнера без визуализации и поэтому не отображается на холсте, как другие подклассы `UIView`. Таким образом, установка таких свойств, как `BackgroundColor` или переопределение `DrawRect` не будет иметь визуального влияния.

Продолжайте размечать интерфейс приложения, добавляя метку, Имажевиев, две кнопки и горизонтальное представление стека, чтобы оно наглядело следующим образом:

[![](uistackview-images/quick03.png "Laying out the Stack View UI")](uistackview-images/quick03.png#lightbox)

Настройте горизонтальное представление стека со следующими параметрами:

[![](uistackview-images/quick04.png "Configure the Horizontal Stack View options")](uistackview-images/quick04.png#lightbox)

Так как мы не хотим, чтобы значок, представляющий каждую "точку" в рейтинге, был растянут при добавлении в горизонтальное представление стека, мы установили **Выравнивание** по **центру** , а **распределение** — **равномерно**.

Наконец, вы **можете** подключить следующие пути и **действия**:

[![](uistackview-images/quick05.png "The Stack View Outlets and Actions")](uistackview-images/quick05.png#lightbox)

### <a name="populate-a-uistackview-from-code"></a>Заполнение Уистакквиев из кода

Вернитесь к Visual Studio для Mac и измените файл **ViewController.CS** и добавьте следующий код:

```csharp
public int Rating { get; set;} = 0;
...

partial void IncreaseRating (Foundation.NSObject sender) {

    // Maximum of 5 "stars"
    if (++Rating > 5 ) {
        // Abort
        Rating = 5;
        return;
    }

    // Create new rating icon and add it to stack
    var icon = new UIImageView (new UIImage("icon.png"));
    icon.ContentMode = UIViewContentMode.ScaleAspectFit;
    RatingView.AddArrangedSubview(icon);

    // Animate stack
    UIView.Animate(0.25, ()=>{
        // Adjust stack view
        RatingView.LayoutIfNeeded();
    });

}

partial void DecreaseRating (Foundation.NSObject sender) {

    // Minimum of zero "stars"
    if (--Rating < 0) {
        // Abort
        Rating =0;
        return;
    }

    // Get the last subview added
    var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];

    // Remove from stack and screen
    RatingView.RemoveArrangedSubview(icon);
    icon.RemoveFromSuperview();

    // Animate stack
    UIView.Animate(0.25, ()=>{
        // Adjust stack view
        RatingView.LayoutIfNeeded();
    });
}
```

Давайте подробно рассмотрим несколько частей этого кода. Сначала мы используем операторы `if`, чтобы проверить наличие не более пяти звездочек или меньше нуля.

Чтобы добавить новую «звезду», мы загружаем его изображение и устанавливаем **режим содержимого** для **масштабирования аспекта**.

```csharp
var icon = new UIImageView (new UIImage("icon.png"));
icon.ContentMode = UIViewContentMode.ScaleAspectFit;
```

Это обеспечивает искажение значка звезды при добавлении в представление стека.

Далее мы добавим новый значок "звезда" в коллекцию вложенных представлений в представлении стека:

```csharp
RatingView.AddArrangedSubview(icon);
```

Обратите внимание, что мы добавили `UIImageView` в свойство `ArrangedSubviews` `UIStackView`, а не в `SubView`. Все представления, которые должны управляться представлением стека, должны быть добавлены в свойство `ArrangedSubviews`.

Чтобы удалить Подпредставление из представления "стек", сначала мы получаем Подпредставление для удаления:

```csharp
var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];
```

Затем необходимо удалить его из коллекции `ArrangedSubviews` и Super View:

```csharp
// Remove from stack and screen
RatingView.RemoveArrangedSubview(icon);
icon.RemoveFromSuperview();
```

Удаление вложенного представления только из `ArrangedSubviews` коллекции извлекает его из элемента управления представления стека, но не удаляет его с экрана.

### <a name="testing-the-ui"></a>Тестирование пользовательского интерфейса

Теперь, когда все необходимые элементы пользовательского интерфейса и код на месте, можно запустить и проверить интерфейс. При отображении пользовательского интерфейса все элементы в вертикальном представлении стека будут иметь одинаковое расстояние сверху вниз.

Когда пользователь нажмет кнопку **увеличения рейтинга** , на экран добавляется другая звездочка (не более 5):

[![](uistackview-images/intro01.png "The sample app run")](uistackview-images/intro01.png#lightbox)

«Звезды» будут автоматически выровнены по центру и равномерно распределяться в горизонтальном представлении стека. Когда пользователь нажмет кнопку " **уменьшить рейтинг** ", удаляется "звезда" (пока не останется ничего).

## <a name="stack-view-details"></a>Сведения о представлении стека

Теперь, когда у нас есть общее представление о том, что такое элемент управления `UIStackView` и как он работает, давайте более подробно рассмотрим некоторые его функции и сведения.

### <a name="auto-layout-and-size-classes"></a>Классы автоматического макета и размера

Как было показано выше, при добавлении вложенного представления в представление стека его макет полностью управляется этим представлением стека с помощью классов автомакета и размера для размещения и изменения размера упорядоченных представлений.

Представление "стек" _будет закреплять_ первое и Последнее вложенное представление в его коллекции по **верхнему** и **нижнему** краям для вертикальных представлений стека или по **левому** и **правому** краям для горизонтальных представлений стека. Если для свойства `LayoutMarginsRelativeArrangement` задано значение `true`, то представление закрепляет подпредставления на соответствующие поля, а не на границу.

В представлении "стек" используется свойство `IntrinsicContentSize` подпредставления при вычислении размера подпредставлений вместе с определенными `Axis` (за исключением `FillEqually Distribution`). `FillEqually Distribution` изменяет размеры всех подпросмотров таким образом, чтобы они совпадали с тем же размером, что приводит к заполнению представления стека вдоль `Axis`.

За исключением `Fill Alignment`, представление стека использует свойство `IntrinsicContentSize` подпредставления для вычисления размера представления, перпендикулярного заданному `Axis`. Для `Fill Alignment`все подпредставления имеют размер, чтобы они заполнили представление стека, перпендикулярное заданному `Axis`.

### <a name="positioning-and-sizing-the-stack-view"></a>Размещение и изменение размера представления стека

В то время как представление Stack имеет полный контроль над разметкой любого вложенного представления (на основе таких свойств, как `Axis` и `Distribution`), необходимо по-прежнему располагать представлением стека (`UIStackView`) в родительском представлении, используя классы автоматической разметки и размера.

Как правило, это означает закрепление по крайней мере двух краев представления стека для расширения и контракта, тем самым определяя его расположение. Без каких бы то ни было дополнительных ограничений размер представления стека будет автоматически изменен в соответствии со всеми вложенными представлениями следующим образом:

- Размер в `Axis` будет суммой всех размеров вложенного представления, а также любого пространства, которое было определено между каждым вложенным представлением.
- Если свойство `LayoutMarginsRelativeArrangement` `true`, размер представлений стека также будет включать в себя пространство для полей.
- Размер, перпендикулярный `Axis`, будет установлен для самого крупного вложенного представления в коллекции.

Кроме того, можно указать ограничения для **высоты** и **ширины**представления стека. В этом случае вложенные представления будут выставляться (изменять размер) для заполнения пространства, заданного представлением стека, как определено свойствами `Distribution` и `Alignment`.

Если свойство `BaselineRelativeArrangement` `true`, то подпредставления будут размещаться на основе первого или последнего базового уровня, а не с **верхней**, **нижней** или **Центральной**- координаты **Y** . Они рассчитываются в содержимом представления стека следующим образом:

- Представление с вертикальным стеком вернет первое Подпредставление для первого базового плана и последний для последнего. Если одно из этих вложенных представлений является представлением стека, то будет использоваться их первый или последний базовый уровень.
- Горизонтальное представление стека будет использовать его самое высокое для первого и последнего базовых показателей. Если максимально высокое представление является также представлением стека, оно будет использовать его в качестве базового.

> [!IMPORTANT]
> Выравнивание по базовому плану не работает для размеров с растяжением или сжатого представления, так как базовый план вычисляется в соответствии с неверной позицией. Для выравнивания по базовому плану убедитесь, что **Высота** подпредставления совпадает с **высотой**внутреннего представления содержимого.

### <a name="common-stack-view-uses"></a>Общее представление "стек" использует

Существует несколько типов макетов, которые хорошо подходят для элементов управления представления стека. В соответствии с Apple, вот несколько наиболее распространенных способов использования:

- **Определите размер вдоль оси** — закрепите обе границы вдоль `Axis` представления стека и один из соседних границ, чтобы задать эту точку, представление стека будет увеличиваться вдоль оси в соответствии с пространством, определенным во вложенных представлениях.
- **Определить расположение** вложенного представления. Закрепите к смежным краям представления стека до родительского представления, представление стека будет увеличиваться в обоих измерениях в соответствии с вложенными представлениями.
- **Определить размер и расположение стека** — закрепление всех четырех краев представления стека в родительском представлении. представление стека упорядочивает вложенные представления на основе пространства, определенного в представлении стека.
- **Определите размер перпендикулярно оси** — закрепите оба края, перпендикулярные `Axis` представления стека, и один из граней вдоль оси, чтобы задать ее расположение, представление стека будет увеличиваться перпендикулярно оси в соответствии с пространством, определенным во вложенных представлениях.

### <a name="managing-the-appearance"></a>Управление внешним видом

`UIStackView` предназначено как представление контейнера без визуализации и поэтому не отображается на холсте, как другие подклассы `UIView`. Установка таких свойств, как `BackgroundColor` или переопределение `DrawRect` не будет иметь визуального влияния.

Существует несколько свойств, которые управляют тем, как представление Stack будет упорядочивать коллекцию вложенных представлений:

- **Axis** — определяет, будет ли представление стека упорядочивать подпредставления **горизонтально** или **вертикально**.
- **Выравнивание** — управляет выравниванием вложенных представлений в представлении стека.
- **Распространение** — управляет изменением размера вложенных представлений в представлении стека.
- **Пробел** — управляет минимальным пространством между каждым подпредставлением в представлении Stack.
- **Относительные базовые показатели** — если `true`, то вертикальное расстояние каждого подпредставления будет получено из базового плана.
- **Поля макета относительные** — размещает подпредставления относительно стандартных полей макета.

Как правило, для размещения небольшого числа подпросмотров используется представление Stack. Более сложные пользовательские интерфейсы могут быть созданы путем вложения одного или нескольких представлений стека друг в друга (как мы делали в [кратком руководстве по уистакквиев](#uistackview-quickstart) выше).

Можно настроить внешний вид пользовательского интерфейса, добавив дополнительные ограничения для подпредставлений (например, для управления высотой или шириной). Однако следует соблюдать осторожность, чтобы не включать конфликтующие ограничения для тех, которые появились в представлении стека.

### <a name="maintaining-arranged-views-and-sub-views-consistency"></a>Поддержание согласованности упорядоченных представлений и подпредставлений

Представление "стек" гарантирует, что его свойство `ArrangedSubviews` всегда является подмножеством своего `Subviews`ного свойства, используя следующие правила:

- Если вложенное представление добавляется в коллекцию `ArrangedSubviews`, оно автоматически добавляется в коллекцию `Subviews` (если оно еще не включено в эту коллекцию).
- Если вложенное представление удаляется из коллекции `Subviews` (удалено из отображения), оно также удаляется из коллекции `ArrangedSubviews`.
- Удаление вложенного представления из коллекции `ArrangedSubviews` не приводит к его удалению из коллекции `Subviews`. Поэтому он больше не будет отображаться в представлении стека, но останется видимым на экране.

Коллекция `ArrangedSubviews` всегда является подмножеством коллекции `Subview`, однако порядок отдельных вложенных представлений в каждой коллекции разделяется и контролируется следующим образом:

- Порядок вложенных представлений в коллекции `ArrangedSubviews` определяет порядок их отображения в стеке.
- Порядок вложенных представлений в коллекции `Subview` определяет их Z-порядок (или уровень) в представлении обратно на передний план.

### <a name="dynamically-changing-content"></a>Динамическое изменение содержимого

Представление "стек" будет автоматически настраивать макет для подпросмотров при добавлении, удалении или скрытии подпредставления. Макет также будет скорректирован, если изменяется любое свойство представления стека (например, его `Axis`).

Изменения макета можно анимировать, поместив их в блок анимации, например:

```csharp
// Animate stack
UIView.Animate(0.25, ()=>{
    // Adjust stack view
    RatingView.LayoutIfNeeded();
});
```

Многие свойства представления стека можно указать с помощью классов размера в раскадровке. Эти свойства будут автоматически анимированы в ответ на изменения размера или ориентации.

## <a name="summary"></a>Сводка

В этой статье рассматривается новый элемент управления `UIStackView` (для iOS 9) для управления набором подпросмотров в стеке по горизонтали или по вертикали в приложении Xamarin. iOS.
Это начало работы с простым примером использования представлений стека для создания пользовательского интерфейса и завершения более подробного просмотра представлений стека, их свойств и функций.

## <a name="related-links"></a>Связанные ссылки

- [Примеры для iOS 9](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [iOS 9 для разработчиков](https://developer.apple.com/ios/pre-release/)
- [Новые возможности iOS 9,0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Справочник по Уистакквиев](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIStackView_Class_Reference/)
- [Введение в Уистакквиев (видео)](https://university.xamarin.com/lightninglectures/introducing-uistackview-on-ios9)
