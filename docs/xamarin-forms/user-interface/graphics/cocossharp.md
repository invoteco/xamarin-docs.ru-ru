---
title: Использование CocosSharp в Xamarin.Forms
description: CocosSharp может использоваться для добавления точное фигуры, изображения и отрисовку текста в приложение для расширенного представления
ms.prod: xamarin
ms.assetid: E0F404D5-5C6B-4288-92EC-78996C674E4E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/03/2016
ms.openlocfilehash: 8ba9e4b119384db401fc631f58c37a28cd2b8004
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724428"
---
# <a name="using-cocossharp-in-xamarinforms"></a>Использование CocosSharp в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-samples/tree/master/CocosSharpForms)

_CocosSharp можно использовать для добавления точных визуализаций форм, изображений и текста в приложение для расширенной визуализации._

> [!VIDEO https://youtube.com/embed/eYCx63FeqVU]

**Развитие 2016: кокосовых # в Xamarin. Forms**

## <a name="overview"></a>Обзор

CocosSharp — это гибкая и мощная технология для отображения графики, чтение сенсорный ввод, воспроизведение аудио-и управление. В этом руководстве объясняется, как добавить CocosSharp в приложения Xamarin.Forms. Здесь рассматривается следующее:

- [Что такое CocosSharp?](#what)
- [Добавление пакетов NuGet CocosSharp](#nuget)
- [Пошаговое руководство. Добавление CocosSharp в приложение Xamarin. Forms](#add)

<a name="what" />

## <a name="what-is-cocossharp"></a>Что такое CocosSharp

[CocosSharp](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md) — это модуль игры с открытым исходным кодом, доступный на платформе Xamarin.
CocosSharp является эффективным для среды выполнения библиотеки, которая содержит следующие компоненты:

- Визуализация изображений с помощью класса `CCSprite`
- Отрисовка форм с помощью класса `CCDrawNode`
- Логика каждого фрейма с использованием класса `CCNode.Schedule`
- Управление содержимым (Загрузка и выгрузка ресурсов, таких как PNG-файлы) с помощью `CCTextureCache`
- Анимация с использованием класса `CCAction`

В CocosSharp основное внимание уделяется упрощают создание кросс платформенных игр 2D; Тем не менее он может быть отличным добавлением к приложения на основе форм Xamarin. Поскольку игр обычно требуют эффективной визуализации и точный контроль над визуальных элементов, CocosSharp можно использовать для добавления эффективную визуализацию и эффекты не игровых приложений.

Xamarin.Forms построена на основе собственного, специфические для платформы пользовательского интерфейса системы. Например, [`Button`s](xref:Xamarin.Forms.Button) по-разному отображаются в iOS и Android и даже может отличаться в зависимости от версии операционной системы. Напротив CocosSharp не используют все визуальные объекты платформы, поэтому все визуальные объекты выглядят одинаково на всех платформах. Само собой разрешение и пропорции отличаются между устройствами, и это может повлиять на прорисовку CocosSharp его визуальные элементы. Эти сведения будут рассматриваться далее в этом руководстве.

Более подробные сведения можно найти в [разделе CocosSharp](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md).

<a name="nuget" />

## <a name="adding-the-cocossharp-nuget-packages"></a>Добавление пакетов NuGet CocosSharp

Прежде чем использовать CocosSharp, разработчики должны сделать ряд дополнений в свои проекты Xamarin.Forms.
В этом руководстве предполагается, что проект Xamarin.Forms с помощью iOS, Android и .NET Standard проект библиотеки.
Весь код будет записываться в проекте библиотеки .NET Standard; Тем не менее библиотеки должны добавляться проекты iOS и Android.

Пакет NuGet CocosSharp содержит все объекты, необходимые для создания объектов CocosSharp.
Пакет NuGet CocosSharp. Forms включает класс `CocosSharpView`, который используется для размещения CocosSharp в Xamarin. Forms.
Добавьте **CocosSharp. Forms** NuGet и **CocosSharp** будет автоматически добавляться.
Для этого щелкните правой кнопкой мыши папку **packages** в проекте библиотеки .NET Standard и выберите **Добавить пакеты...** . Введите условие поиска **CocosSharp. Forms**, выберите **CocosSharp для Xamarin. Forms**, а затем нажмите кнопку **Добавить пакет**.

![](cocossharp-images/image1.png "Add Packages Dialog")

В проект будут добавлены пакеты NuGet **CocosSharp** и **CocosSharp. Forms** :

![](cocossharp-images/image2.png "Packages Folder")

Повторите описанные выше действия для проектов для конкретной платформы (например, iOS и Android).

<a name="add" />

## <a name="walkthrough-adding-cocossharp-to-a-xamarinforms-app"></a>Пошаговое руководство: Добавление CocosSharp в приложение Xamarin.Forms

Выполните следующие действия, чтобы добавить простое представление CocosSharp в приложение Xamarin.Forms.

1. [Создание страницы Xamarin Forms](#1)
1. [Добавление Кокосшарпвиев](#2)
1. [Создание Гамесцене](#3)
1. [Добавление круга](#4)
1. [Взаимодействие с CocosSharp](#5)

После успешного добавления представления CocosSharp в приложение Xamarin. Forms ознакомьтесь с [документацией по CocosSharp](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md) , чтобы узнать больше о создании содержимого с помощью CocosSharp.

<a name="1" />

### <a name="1-creating-a-xamarin-forms-page"></a>1. Создание страницы Xamarin Forms

CocosSharp может размещаться в любом контейнере Xamarin.Forms. В этом примере для этой страницы используется страница с именем `HomePage`. `HomePage` разбивается на `Grid`, чтобы продемонстрировать, как Xamarin. Forms и CocosSharp могут быть отображены одновременно на одной странице.

Сначала настройте страницу, чтобы она содержала `Grid` и два экземпляра `Button`:

```csharp
public class HomePage : ContentPage
{
public HomePage ()
    {
        // This is the top-level grid, which will split our page in half
        var grid = new Grid ();
        this.Content = grid;
        grid.RowDefinitions = new RowDefinitionCollection {
            // Each half will be the same size:
            new RowDefinition{ Height = new GridLength(1, GridUnitType.Star)},
            new RowDefinition{ Height = new GridLength(1, GridUnitType.Star)},
        };
        CreateTopHalf (grid);
        CreateBottomHalf (grid);
    }
    void CreateTopHalf(Grid grid)
    {
        // We'll be adding our CocosSharpView here:
    }
    void CreateBottomHalf(Grid grid)
    {
        // We'll use a StackLayout to organize our buttons
        var stackLayout = new StackLayout();
        // The first button will move the circle to the left when it is clicked:
        var moveLeftButton = new Button {
            Text = "Move Circle Left"
        };
        stackLayout.Children.Add (moveLeftButton);

        // The second button will move the circle to the right when clicked:
        var moveCircleRight = new Button {
            Text = "Move Circle Right"
        };
        stackLayout.Children.Add (moveCircleRight);
        // The stack layout will be in the bottom half (row 1):

        grid.Children.Add (stackLayout, 0, 1);
    }
}
```

В iOS `HomePage` отображается, как показано на следующем рисунке:

![](cocossharp-images/image3.png "HomePage Screenshot")

<a name="2" />

### <a name="2-adding-a-cocossharpview"></a>2. Добавление Кокосшарпвиев

Класс `CocosSharpView` используется для внедрения CocosSharp в приложение Xamarin. Forms. Поскольку `CocosSharpView` наследуется от класса [Xamarin. Forms. View](xref:Xamarin.Forms.View) , он предоставляет знакомый интерфейс для макета и может использоваться в контейнерах макета, таких как [Xamarin. Forms. Grid](xref:Xamarin.Forms.Grid). Добавьте новый `CocosSharpView` в проект, выполнив метод `CreateTopHalf`:

```csharp
void CreateTopHalf(Grid grid)
{
    // This hosts our game view.
    var gameView = new CocosSharpView () {
        // Notice it has the same properties as other XamarinForms Views
        HorizontalOptions = LayoutOptions.FillAndExpand,
        VerticalOptions = LayoutOptions.FillAndExpand,
        // This gets called after CocosSharp starts up:
        ViewCreated = HandleViewCreated
    };
    // We'll add it to the top half (row 0)
    grid.Children.Add (gameView, 0, 0);
}
```

Инициализация CocosSharp не выполняется немедленно, поэтому Зарегистрируйте событие, когда `CocosSharpView` завершит создание. Это делается в методе `HandleViewCreated`:

```csharp
void HandleViewCreated (object sender, EventArgs e)
{
    var gameView = sender as CCGameView;
    if (gameView != null)
    {
        // This sets the game "world" resolution to 100x100:
        gameView.DesignResolution = new CCSizeI (100, 100);
        // GameScene is the root of the CocosSharp rendering hierarchy:
        gameScene = new GameScene (gameView);
        // Starts CocosSharp:
        gameView.RunWithScene (gameScene);
    }
}
```

Метод `HandleViewCreated` имеет две важные сведения, которые мы будем искать. Первый — это класс `GameScene`, который будет создан в следующем разделе. Важно отметить, что приложение не будет компилироваться до создания `GameScene` и разрешения ссылки на экземпляр `gameScene`.

Вторым важным описанием является свойство `DesignResolution`, определяющее видимую область игры для объектов CocosSharp. После создания `GameScene`будет рассмотрено свойство `DesignResolution`.

<a name="3" />

### <a name="3-creating-the-gamescene"></a>3. Создание Гамесцене

Класс `GameScene` наследует от `CCScene`CocosSharp. `GameScene` — это первая точка, где мы работаем исключительно с CocosSharp. Код, содержащийся в `GameScene`, будет работать в любом приложении CocosSharp, независимо от того, размещено ли оно в проекте Xamarin. Forms.

Класс `CCScene` является корнем визуального элемента для всех CocosSharp отрисовки. Любой видимый объект CocosSharp должен содержаться в `CCScene`. В частности, визуальные объекты должны быть добавлены в `CCLayer` экземпляры, и эти экземпляры `CCLayer` должны быть добавлены в `CCScene`.

Приведенный ниже график могут помочь визуализировать типичный CocosSharp иерархии:

![](cocossharp-images/image4.png "Typical CocosSharp Hierarchy")

В один момент времени может быть активно только одно `CCScene`. Большинство игр используют несколько экземпляров `CCLayer` для сортировки содержимого, но наше приложение использует только одно. Аналогичным образом большинстве игр используется несколько визуальных объектов, но у нас будет только один в наше приложение. Более подробное обсуждение визуальной иерархии CocosSharp можно найти в [разделе Пошаговое руководство по баунЦинггаме](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/bouncing-game.md).

Изначально класс `GameScene` будет почти пустым — мы просто создадим его для удовлетворения ссылки в `HomePage`. Добавьте новый класс в проект библиотеки .NET Standard с именем `GameScene`. Он должен наследовать от класса `CCScene` следующим образом:

```csharp
public class GameScene : CCScene
{
    public GameScene (CCGameView gameView) : base(gameView)
    {

    }
}
```

Теперь, когда `GameScene` определен, можно вернуться к `HomePage` и добавить поле:

```csharp
// Keep the GameScene at class scope
// so the button click events can access it:
GameScene gameScene;
```

Теперь можно скомпилировать наш проект и запустить его, чтобы увидеть CocosSharp под управлением. Мы не добавили что-либо в наш `GameScene,`, поэтому верхняя половина нашей страницы является черным: цвет по умолчанию для CocosSharp сцены.

![](cocossharp-images/image5.png "Blank GameScene")

<a name="4" />

### <a name="4-adding-a-circle"></a>4. Добавление круга

В настоящее время приложение имеет выполняющийся экземпляр ядра CocosSharp, отображая пустой `CCScene`. Далее мы добавим визуальный объект: круг. Класс `CCDrawNode` можно использовать для рисования различных геометрических фигур, как описано в разделе [Рисование геометрии с помощью ккдравноде Guide](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/ccdrawnode.md).

Добавьте окружность в наш класс `GameScene` и создайте его экземпляр в конструкторе, как показано в следующем коде:

```csharp
public class GameScene : CCScene
{
    CCDrawNode circle;
    public GameScene (CCGameView gameView) : base(gameView)
    {
        var layer = new CCLayer ();
        this.AddLayer (layer);
        circle = new CCDrawNode ();
        layer.AddChild (circle);
        circle.DrawCircle (
            // The center to use when drawing the circle,
            // relative to the CCDrawNode:
            new CCPoint (0, 0),
            radius:15,
            color:CCColor4B.White);
        circle.PositionX = 20;
        circle.PositionY = 50;
    }
}
```

Выполнение приложения теперь показывает круг в левой части области отображения CocosSharp:

![](cocossharp-images/image6.png "Circle in GameScene")

#### <a name="understanding-designresolution"></a>Основные сведения о DesignResolution

Теперь, когда отображается объект визуального объекта CocosSharp, можно исследовать свойство `DesignResolution`.

`DesignResolution` представляет ширину и высоту области CocosSharp для размещения и изменения размеров объектов. Фактическое разрешение области измеряется в *пикселях* , а `DesignResolution` измеряется в *единицах*мира. На следующей схеме показана разрешение различные части представления, отображаться на iPhone 5 с разрешением экрана 640 x 1136 пикселей:

![](cocossharp-images/image7.png "iPhone 5s Design Resolution")

На схеме выше показаны измерения, пикселей с внешней стороны экрана черным шрифтом. Единицы отображаются внутри схемы в белым текстом. Ниже приведены некоторые важные сведения, отображенный выше.

- Источником отображение CocosSharp является слева внизу. В правой увеличивает значение X и перемещении вверх увеличивает значение Y. Обратите внимание, что значение Y инвертируется по сравнению с некоторые другие обработчики плоского макета, где (0,0) находится в левой верхней части холста.
- Чтобы сохранить пропорции его представления является CocosSharp по умолчанию. Поскольку шире, чем высота первой строки в сетке, CocosSharp не заполняет всю ширину элемента свою ячейку, как показано пунктирной белого прямоугольника. Это поведение можно изменить, как описано в разделе [Обработка нескольких разрешений в CocosSharp Guide](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/resolutions.md).
- В этом примере CocosSharp сохранит область отображения 100 единиц ширину, высоту, независимо от размера или соотношение сторон своего устройства. Это означает, что код можно предположить, что X = 100 представляет правую привязан CocosSharp отображения, сохранение макета, согласованные на всех устройствах.

#### <a name="ccdrawnode-details"></a>Сведения о CCDrawNode

В нашем простом приложении для рисования окружности используется класс `CCDrawNode`. Этот класс может быть очень полезно для бизнес-приложений, так как он обеспечивает отрисовки геометрического объекта на основе векторов — это функция, отсутствующие на Xamarin.Forms. Помимо кругов, класс `CCDrawNode` можно использовать для рисования прямоугольников, линий, линий и пользовательских многоугольников. `CCDrawNode` также легко использовать, так как не требует использования файлов изображений (например, PNG). Более подробное обсуждение Ккдравноде можно найти в разделе [Рисование геометрии с помощью ккдравноде Guide](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/ccdrawnode.md).

<a name="5" />

### <a name="5-interacting-with-cocossharp"></a>5. взаимодействие с CocosSharp

Визуальные элементы CocosSharp (такие как `CCDrawNode`) наследуются от класса `CCNode`. `CCNode` предоставляет два свойства, которые можно использовать для размещения объекта относительно его родительского элемента: `PositionX` и `PositionY`. В настоящее время наш код использует эти два свойства для размещения центра круга, как показано в этом фрагменте кода:

```csharp
circle.PositionX = 20;
circle.PositionY = 50;
```

Это важно отметить, что CocosSharp объекты располагаются с явно указанными значениями позиции, в отличие от большинства представления Xamarin.Forms, которые автоматически располагаются в соответствии с поведение их родительские элементы управления макета.

Мы добавим код, чтобы разрешить пользователю щелкните одну из двух кнопок, чтобы переместить элемент управления circle влево или вправо, 10 единиц (не пикселей, поскольку Рисует элемент управления circle в абсолютном единицы CocosSharp пространстве). Сначала мы создадим два открытых метода в классе `GameScene`:

```csharp
public void MoveCircleLeft()
{
    circle.PositionX -= 10;
}

public void MoveCircleRight()
{
    circle.PositionX += 10;
}
```

Далее мы добавим обработчики для двух кнопок в `HomePage`, чтобы реагировать на нажатия. По завершении наш метод `CreateBottomHalf` содержит следующий код:

```csharp
void CreateBottomHalf(Grid grid)
{
    // We'll use a StackLayout to organize our buttons
    var stackLayout = new StackLayout();

    // The first button will move the circle to the left when it is clicked:
    var moveLeftButton = new Button {
        Text = "Move Circle Left"
    };
    moveLeftButton.Clicked += (sender, e) => gameScene.MoveCircleLeft ();
    stackLayout.Children.Add (moveLeftButton);

    // The second button will move the circle to the right when clicked:
    var moveCircleRight = new Button {
        Text = "Move Circle Right"
    };
    moveCircleRight.Clicked += (sender, e) => gameScene.MoveCircleRight ();
    stackLayout.Children.Add (moveCircleRight);

    // The stack layout will be in the bottom half (row 1):
    grid.Children.Add (stackLayout, 0, 1);
}
```

Элемент управления circle CocosSharp теперь перемещает в ответ на нажатия. Также четко видно границы CocosSharp полотна, перемещая элемент управления circle достаточно далеко влево или вправо:

![](cocossharp-images/image8.png "GameScene with Moving Circle")

## <a name="summary"></a>Сводка

В этом руководстве показано, как добавить CocosSharp в Xamarin.Forms существующего проекта, как создать взаимодействие между Xamarin.Forms и CocosSharp и рассматриваются различные вопросы, при создании макетов в CocosSharp.

Игровое ядро CocosSharp и предлагает множество функциональные возможности и глубина, поэтому в этом руководстве представлены только общие действия CocosSharp. Разработчики, желающие ознакомиться с дополнительными сведениями о CocosSharp, могут найти множество статей в [архиве CocosSharp](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/).

## <a name="related-links"></a>Связанные ссылки

- [Кокосшарпформс (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/master/CocosSharpForms)
