---
title: Общие сведения о Опентк в Xamarin. Mac
description: В этой статье приводятся общие сведения о работе с Опентк в приложении Xamarin. Mac. Здесь рассматривается создание и обслуживание игрового окна, Визуализация простого объекта и отображение объекта пользователю.
ms.prod: xamarin
ms.assetid: BDE05645-7273-49D3-809B-8642347678D2
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 806f58ae6a79f935916008e93deed0136920951a
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/03/2019
ms.locfileid: "70227815"
---
# <a name="introduction-to-opentk-in-xamarinmac"></a>Общие сведения о Опентк в Xamarin. Mac

Опентк (Open Toolkit) — это Расширенная библиотека низкого уровня C# , которая упрощает работу с OpenGL, OpenCL и открытым. Опентк можно использовать для игр, научных приложений или других проектов, требующих трехмерной графики, аудио или вычислительной функциональности. В этой статье приводятся краткие сведения об использовании Опентк в приложении Xamarin. Mac.

[![](opentk-images/intro01.png "Пример выполнения приложения")](opentk-images/intro01.png#lightbox)

В этой статье рассматриваются основы Опентк в приложении Xamarin. Mac. Мы настоятельно рекомендуем сначала ознакомиться со статьей [Hello, Mac](~/mac/get-started/hello-mac.md) , в частности [Знакомство с Xcode и Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) , а также с разделом "возможности [и действия](~/mac/get-started/hello-mac.md#outlets-and-actions) ", так как в нем рассматриваются основные понятия и методы, которые мы будем использовать в Эта статья.

Возможно, вы захотите ознакомиться с представлением [ C# классов и методов для цели-C](~/mac/internals/how-it-works.md) в документе о внутренних компонентах [Xamarin. Mac](~/mac/internals/how-it-works.md) , а `Register` также объясняются команды и `Export` , используемые для подключения C# классов к Объекты цели-C и элементы пользовательского интерфейса.

<a name="About_OpenTK" />

## <a name="about-opentk"></a>О Опентк

Как упоминалось выше, Опентк (открытый набор средств) — это Расширенная библиотека низкого C# уровня, которая упрощает работу с OpenGL, OpenCL и открытым. Использование Опентк в приложении Xamarin. Mac предоставляет следующие возможности:

- **Быстрая разработка** — опентк предоставляет надежные типы данных и встроенную документацию для более простого и быстрого перехвата потока кода и выявления ошибок.
- **Простая интеграция** — опентк была разработана для простой интеграции с приложениями .NET.
- **Разрешительное License** -опентк распространяется по лицензиям MIT/X11 и полностью бесплатный.
- **Широкие** строго типизированные привязки — опентк поддерживают новейшие версии OpenGL, OpenGL | ES, Open и OpenCL с автоматической загрузкой расширений, проверкой ошибок и встроенной документацией.
- **Гибкие параметры графического пользовательского интерфейса** — опентк предоставляет собственное, высокопроизводительное окно игры, разработанное специально для игр и Xamarin. Mac.
- **Полностью управляемый CLS-совместимый код** — опентк поддерживает 32-разрядные и 64-разрядные версии macOS без неуправляемых библиотек.
- **набор средств для 3D-математических вычислений** Опентк предоставляет `Vector`структуры `Matrix` ,`Quaternion` и с`Bezier` помощью набора трехмерных математических инструментов.

Опентк можно использовать для игр, научных приложений или других проектов, требующих трехмерной графики, аудио или вычислительной функциональности.

Дополнительные сведения см. на веб-сайте [Open Toolkit](http://www.opentk.com) .

<a name="OpenTK_Quickstart" />

## <a name="opentk-quickstart"></a>Краткое руководство по Опентк

В качестве краткого знакомства с использованием Опентк в приложении Xamarin. Mac мы создадим простое приложение, которое открывает игровое представление, визуализирует простой треугольник в этом представлении и присоединяет представление игры к главному окну приложения Mac для отображения треугольника пользователю.

<a name="Starting_a_New_Project" />

### <a name="starting-a-new-project"></a>Запуск нового проекта

Запустите Visual Studio для Mac и создайте новое решение Xamarin. Mac. Выберите > **приложение**Macобщие > **Cocoa приложение**: > 

[![](opentk-images/sample01.png "Добавление нового приложения Cocoa")](opentk-images/sample01.png#lightbox)

Введите `MacOpenTK` для **имени проекта**:

[![](opentk-images/sample02.png "Задание имени проекта")](opentk-images/sample02.png#lightbox)

Нажмите кнопку **создать** , чтобы создать новый проект.

<a name="Including_OpenTK" />

### <a name="including-opentk"></a>Включение Опентк

Прежде чем можно будет использовать Open TK в приложении Xamarin. Mac, необходимо включить ссылку на сборку Опентк. В **Обозреватель решений**щелкните правой кнопкой мыши папку **ссылки** и выберите **изменить ссылки.** ...

Установите флажок `OpenTK` и нажмите кнопку **ОК** :

[![](opentk-images/sample03.png "Изменение ссылок на проекты")](opentk-images/sample03.png#lightbox)

<a name="Using_OpenTK" />

### <a name="using-opentk"></a>Использование Опентк

После создания нового проекта дважды щелкните `MainWindow.cs` файл в **Обозреватель решений** , чтобы открыть его для редактирования. `MainWindow` Сделайте класс похожим на следующий:

```csharp
using System;
using System.Drawing;
using OpenTK;
using OpenTK.Graphics;
using OpenTK.Graphics.OpenGL;
using OpenTK.Platform.MacOS;
using Foundation;
using AppKit;
using CoreGraphics;

namespace MacOpenTK
{
    public partial class MainWindow : NSWindow
    {
        #region Computed Properties
        public MonoMacGameView Game { get; set; }
        #endregion

        #region Constructors
        public MainWindow (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindow (NSCoder coder) : base (coder)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Create new Game View and replace the window content with it
            Game = new MonoMacGameView(ContentView.Frame);
            ContentView = Game;

            // Wire-up any required Game events
            Game.Load += (sender, e) =>
            {
                // TODO: Initialize settings, load textures and sounds here
            };

            Game.Resize += (sender, e) =>
            {
                // Adjust the GL view to be the same size as the window
                GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
            };

            Game.UpdateFrame += (sender, e) =>
            {
                // TODO: Add any game logic or physics
            };

            Game.RenderFrame += (sender, e) =>
            {
                // Setup buffer
                GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
                GL.MatrixMode(MatrixMode.Projection);

                // Draw a simple triangle
                GL.LoadIdentity();
                GL.Ortho(-1.0, 1.0, -1.0, 1.0, 0.0, 4.0);
                GL.Begin(BeginMode.Triangles);
                GL.Color3(Color.MidnightBlue);
                GL.Vertex2(-1.0f, 1.0f);
                GL.Color3(Color.SpringGreen);
                GL.Vertex2(0.0f, -1.0f);
                GL.Color3(Color.Ivory);
                GL.Vertex2(1.0f, 1.0f);
                GL.End();

            };

            // Run the game at 60 updates per second
            Game.Run(60.0);
        }
        #endregion
    }
}
```

Давайте подробно рассмотрим этот код.

<a name="Required_APIs" />

### <a name="required-apis"></a>Требуемые API

Для использования Опентк в классе Xamarin. Mac требуется несколько ссылок. В начале определения мы включили следующие `using` инструкции:

```csharp
using System;
using System.Drawing;
using OpenTK;
using OpenTK.Graphics;
using OpenTK.Graphics.OpenGL;
using OpenTK.Platform.MacOS;
using Foundation;
using CoreGraphics;
```

Этот минимальный набор будет необходим для любого класса, использующего Опентк.

<a name="Adding_the_Game_View" />

### <a name="adding-the-game-view"></a>Добавление представления игры

Далее необходимо создать представление игры, которое будет содержать все взаимодействия с Опентк и отобразить результаты. Мы использовали следующий код:

```csharp
public MonoMacGameView Game { get; set; }
...

// Create new Game View and replace the window content with it
Game = new MonoMacGameView(ContentView.Frame);
ContentView = Game;
```

Мы предоставили игровое представление того же размера, что и главное окно Mac, и заменили представление содержимого окна новым `MonoMacGameView`. Так как мы заменили существующее содержимое окна, при изменении размера основного окна размер представления будет изменяться автоматически.

<a name="Responding_to_Events" />

### <a name="responding-to-events"></a>Реагирование на события

Существует несколько событий по умолчанию, на которые должно реагировать каждое представление игры. В этом разделе будут рассмотрены основные события, необходимые для.

<a name="The_Load_Event" />

### <a name="the-load-event"></a>Событие Load

`Load` Событие — это место для загрузки ресурсов с диска, например изображений, текстур или музыки. Для нашего простого тестового приложения мы не используем `Load` событие, но включили его для справки:

```csharp
Game.Load += (sender, e) =>
{
    // TODO: Initialize settings, load textures and sounds here
};
```

<a name="The_Resize_Event" />

### <a name="the-resize-event"></a>Событие изменения размера

`Resize` Событие должно вызываться при каждом изменении размера представления игры. Для нашего примера приложения мы создаем окно представления GL того же размера, что и наше представление игры (автоматическое изменение размера в главном окне Mac), с помощью следующего кода:

```csharp
Game.Resize += (sender, e) =>
{
    // Adjust the GL view to be the same size as the window
    GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
};
```

<a name="The_UpdateFrame_Event" />

### <a name="the-updateframe-event"></a>Событие Упдатефраме

`UpdateFrame` Событие используется для обработки вводимых пользователем данных, обновления позиций объектов, выполнения вычислений физикы или искусственного интеллекта. Для нашего простого тестового приложения мы не используем `UpdateFrame` событие, но включили его для справки:

```csharp
Game.UpdateFrame += (sender, e) =>
{
    // TODO: Add any game logic or physics
};
```

> [!IMPORTANT]
> Реализация опентк в Xamarin. Mac не включает `Input API`, поэтому для добавления поддержки клавиатуры и мыши потребуется использовать API, предоставляемые компанией Apple. При необходимости можно создать пользовательский экземпляр класса `MonoMacGameView` и `KeyDown` переопределить методы и `KeyUp` .

<a name="The_RenderFrame_Event" />

### <a name="the-renderframe-event"></a>Событие Рендерфраме

`RenderFrame` Событие содержит код, используемый для отрисовки (рисования) графики. Для нашего примера приложения мы заполняем представление игры простым треугольником:

```csharp
Game.RenderFrame += (sender, e) =>
{
    // Setup buffer
    GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
    GL.MatrixMode(MatrixMode.Projection);

    // Draw a simple triangle
    GL.LoadIdentity();
    GL.Ortho(-1.0, 1.0, -1.0, 1.0, 0.0, 4.0);
    GL.Begin(BeginMode.Triangles);
    GL.Color3(Color.MidnightBlue);
    GL.Vertex2(-1.0f, 1.0f);
    GL.Color3(Color.SpringGreen);
    GL.Vertex2(0.0f, -1.0f);
    GL.Color3(Color.Ivory);
    GL.Vertex2(1.0f, 1.0f);
    GL.End();

};
```

Как правило, код прорисовки будет содержать вызов `GL.Clear` для удаления всех существующих элементов до вывода новых элементов.

> [!IMPORTANT]
> Для версии опентк Xamarin. Mac **не** вызывайте `SwapBuffers` метод `MonoMacGameView` экземпляра в конце кода отрисовки. Это приведет к быстрому воспроизведению игры, а не к отображенному представлению.

<a name="Running_the_Game_View" />

### <a name="running-the-game-view"></a>Запуск представления игры

После определения всех необходимых событий и представления игры, присоединенного к главному окну Mac нашего приложения, мы читаем, чтобы запустить представление игры и отобразить нашу графику. Используйте следующий код:

```csharp
// Run the game at 60 updates per second
Game.Run(60.0);
```

Мы передаем желаемую частоту кадров, которую мы хотим обновить в игровом представлении. в нашем примере мы выбрали `60` кадры в секунду (одинаковую частоту обновления для обычного телевизора).

Давайте запустим приложение и увидите выходные данные:

[![](opentk-images/intro01.png "Пример выходных данных приложений")](opentk-images/intro01.png#lightbox)

Если изменить размер окна, представление игры также будет располагаться, а размер треугольника также будет изменен и обновлен в режиме реального времени.

<a name="Where_to_Next" />

### <a name="where-to-next"></a>Далее?

Используя основы работы с Опентк в приложении Xamarin. Mac, ниже приведены некоторые рекомендации по выполнению следующих действий.

- Попробуйте изменить цвет треугольника и цвет фона для представления игры в `Load` событиях и. `RenderFrame`
- Измените цвет треугольника, когда пользователь нажмет клавишу `UpdateFrame` в событиях и `RenderFrame` , или создайте собственный `KeyUp` пользовательский `MonoMacGameView` класс и переопределите методы и `KeyDown` .
- Сделайте треугольник передвигаться по экрану с помощью подзнающих ключей `UpdateFrame` в событии. Указание. Используйте `Matrix4.CreateTranslation` метод, чтобы создать матрицу перевода и `GL.LoadMatrix` вызвать метод, `RenderFrame` чтобы загрузить его в событие.
- Используйте цикл для отрисовки нескольких треугольников `RenderFrame` в событии. `for`
- Поверните камеру, чтобы дать другое представление треугольника в трехмерном пространстве. Указание. Используйте `Matrix4.CreateTranslation` метод, чтобы создать матрицу перевода и `GL.LoadMatrix` вызвать метод для его загрузки. Для манипуляций `Vector2`с камерой можно также использовать `Matrix4` классы `Vector4` , `Vector3`и.

Дополнительные примеры см. в репозитории [Опентк Samples GitHub](https://github.com/opentk/opentk/tree/master/Source/Examples) . Он содержит официальный список примеров использования Опентк. Вам потребуется адаптировать эти примеры для использования с версией Опентк для Xamarin. Mac.

Более сложный пример реализации Опентк в Xamarin. Mac см. в нашем примере [мономакгамевиев](https://docs.microsoft.com/samples/xamarin/mac-samples/monomacgamewindow) .

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье приведен краткий обзор работы с Опентк в приложении Xamarin. Mac. Мы увидели, как создать игровое окно, как подключить игровое окно к окну Mac и как визуализировать простую фигуру в окне игры.

## <a name="related-links"></a>Связанные ссылки

- [Макопентк (пример)](https://docs.microsoft.com/samples/xamarin/mac-samples/macopentk)
- [Мономакгамевиев (пример)](https://docs.microsoft.com/samples/xamarin/mac-samples/monomacgamewindow)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Работа с Windows](~/mac/user-interface/window.md)
- [Открытый набор средств](http://www.opentk.com)
- [Рекомендации по работе с человеческим интерфейсом OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Введение в Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
