---
title: Введение в UrhoSharp
description: В этом документе описывается базовая структура приложения UrhoSharp и ссылки на различные руководства и примеры приложений, демонстрирующие использование UrhoSharp.
ms.prod: xamarin
ms.assetid: 18041443-5093-4AF7-8B20-03E00478EF35
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 441a3cc19b4246fb2bdea54508142a894af5c051
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "67832545"
---
# <a name="introduction-to-urhosharp"></a>Введение в UrhoSharp

![Логотип UrhoSharp](introduction-images/urhosharp-icon.png)

UrhoSharp — это мощное средство трехмерной игры для разработчиков Xamarin и .NET.  Он похож на SceneKit и SpriteKit Apple и включает в себя физику, навигацию, сети и многое другое, в то же время перекрестно на платформу.

Это привязка .NET к подсистеме [Urho3D](http://urho3d.github.io/) , позволяющая разработчикам писать код на различных платформах, предназначенных для Android, iOS, Windows и Mac, с одной и той же базой кода и может быть отображен в системах OpenGL и Direct3D.

UrhoSharp — это модуль игры с множеством функциональных возможностей.

- Мощная трехмерная графическая визуализация
- Физическое моделирование (с использованием библиотеки маркеров)
- Обработка сцены
- Поддержка await/Async
- API-интерфейс понятных действий
- Двухмерная интеграция в трехмерные сцены
- Отрисовка шрифта с помощью Фритипе
- Сетевые возможности клиента и сервера
- Импорт широкого спектра ресурсов (с помощью Open Asset Library)
- Сетка навигации и пасфиндинг (использование повторного приведения или деобзора)
- Создание поверхности выпуклой для обнаружения столкновений (с помощью Станхулл)
- Воспроизведение звука (с **либворбис**)

## <a name="get-started"></a>Начало работы

UrhoSharp удобно распространять как [пакет NuGet](https://www.nuget.org/) , и его можно добавить в проекты C# или F# , предназначенные для Windows, Mac, Android или iOS.  NuGet поставляется с библиотеками, необходимыми для запуска программы, а также с базовыми ресурсами (CoreData), используемыми ядром.

### <a name="urho-as-a-portable-class-library"></a>Урхо как Переносимая библиотека классов

Пакет Урхо может использоваться либо из проекта для конкретной платформы, либо из переносимого проекта библиотеки классов, что позволяет повторно использовать весь код на всех платформах.  Это означает, что все, что нужно сделать на каждой платформе, — написать конкретную точку входа для конкретной платформы, а затем передавать управление в код общей игры.

### <a name="samples"></a>Примеры

Вы можете получить представление о возможностях Урхо, открыв в Visual Studio для Mac или Visual Studio пример решения из:

[https://github.com/xamarin/urho-samples](https://github.com/xamarin/urho-samples)

Решение по умолчанию содержит проекты для Android, iOS, Windows и Mac.  Мы состроили это решение, так что у нас есть незначительное средство запуска для определенной платформы, и все примеры кода и игрового кода находятся в переносимой библиотеке классов, где показано, как добиться максимально возможного повторного использования кода на всех платформах.

Дополнительные сведения о создании собственных решений см. на странице [Урхо и платформы](~/graphics-games/urhosharp/platform/index.md) .

Поскольку все примеры совместно используют общий набор элементов пользовательского интерфейса, примеры имеют абстракцию базовой настройки в этом файле:

[https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs)

Это предоставляет пример базового класса, который обрабатывает некоторые основные нажатия клавиш и события касания, настраивает камеру, предоставляет основные элементы пользовательского интерфейса, и это позволяет каждому примеру сосредоточиться на определенных функциональных возможностях.

В следующем примере показано, что может делать механизм:

- Пример [игры](https://github.com/xamarin/urho-samples/tree/master/SamplyGame) . простой клон шутискиес.

В других примерах показаны отдельные свойства каждого образца.

## <a name="basic-structure"></a>Базовая структура

Игра должна подделать подкласс классу `Application`, именно здесь будет настроена игра (в методе `Setup`) и запущена игра (в методе `Start`).  Затем создается основной интерфейс пользователя.  Мы рассмотрим небольшой пример, демонстрирующий интерфейсы API для настройки трехмерной сцены, некоторых элементов пользовательского интерфейса и присоединения к нему простого поведения.

```csharp
class MySample : Application {
    protected override void Start ()
    {
        CreateScene ();
        Input.KeyDown += (args) => {
            if (args.Key == Key.Esc) Exit ();
        };
    }

    async void CreateScene()
    {
        // UI text
        var helloText = new Text()
        {
            Value = "Hello World from MySample",
            HorizontalAlignment = HorizontalAlignment.Center,
            VerticalAlignment = VerticalAlignment.Center
        };
        helloText.SetColor(new Color(0f, 1f, 1f));
        helloText.SetFont(
            font: ResourceCache.GetFont("Fonts/Font.ttf"),
            size: 30);
        UI.Root.AddChild(helloText);

        // Create a top-level scene, must add the Octree
        // to visualize any 3D content.
        var scene = new Scene();
        scene.CreateComponent<Octree>();
        // Box
        Node boxNode = scene.CreateChild();
        boxNode.Position = new Vector3(0, 0, 5);
        boxNode.Rotation = new Quaternion(60, 0, 30);
        boxNode.SetScale(0f);
        StaticModel modelObject = boxNode.CreateComponent<StaticModel>();
        modelObject.Model = ResourceCache.GetModel("Models/Box.mdl");
        // Light
        Node lightNode = scene.CreateChild(name: "light");
        lightNode.SetDirection(new Vector3(0.6f, -1.0f, 0.8f));
        lightNode.CreateComponent<Light>();
        // Camera
        Node cameraNode = scene.CreateChild(name: "camera");
        Camera camera = cameraNode.CreateComponent<Camera>();
        // Viewport
        Renderer.SetViewport(0, new Viewport(scene, camera, null));
        // Perform some actions
        await boxNode.RunActionsAsync(
            new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));
        await boxNode.RunActionsAsync(
            new RepeatForever(new RotateBy(duration: 1,
                deltaAngleX: 90, deltaAngleY: 0, deltaAngleZ: 0)));
     }
}
```

Если запустить это приложение, вы быстро обнаружите, что среда выполнения сообщает о ваших ресурсах.  Вам нужно создать в проекте иерархию, которая начинается с специального имени каталога "Data", и в этом случае вы поместите ресурсы, на которые вы ссылаетесь в программе.  Затем необходимо задать в свойствах элемента для каждого ресурса "Копировать в выходной каталог" в "копировать, если новее", что обеспечит их наличие.

Дайте нам объяснение, что происходит здесь.

Чтобы запустить приложение, необходимо вызвать функцию инициализации ядра, а затем создать новый экземпляр класса приложения следующим образом:

```csharp
new MySample().Run();
```

Среда выполнения будет вызывать методы `Setup` и `Start`.  При переопределении `Setup` можно настроить параметры ядра (не показывать в этом примере).

Необходимо переопределить `Start`, так как это приведет к запуску игры.  В этом методе вы загружаете ресурсы, подключаете обработчики событий, настраиваете сцену и запускаете нужные вам действия.  В нашем примере мы создадим пользовательский интерфейс для отображения пользователю, а также настройку трехмерной сцены.

В следующем фрагменте кода используется платформа пользовательского интерфейса для создания текстового элемента и его добавления в приложение:

```csharp
// UI text
var helloText = new Text()
{
    Value = "Hello World from UrhoSharp",
    HorizontalAlignment = HorizontalAlignment.Center,
    VerticalAlignment = VerticalAlignment.Center
};
helloText.SetColor(new Color(0f, 1f, 1f));
helloText.SetFont(
    font: ResourceCache.GetFont("Fonts/Font.ttf"),
    size: 30);
UI.Root.AddChild(helloText);
```

Платформа пользовательского интерфейса заключается в том, чтобы предоставить очень простой пользовательский интерфейс для игр, и он работает путем добавления новых узлов в `UI.Root` узел.

Во второй части нашего примера настраивается Основная сцена.  Это включает ряд шагов, создание трехмерной сцены, создание трехмерного прямоугольника на экране, добавление лампочки, камеры и окна просмотра.  Дополнительные сведения см. в разделе [сцена, узлы, компоненты и камеры](~/graphics-games/urhosharp/using.md#scenenodescomponentsandcameras).

Третья часть нашего примера запускает несколько действий.  Действия — это рецепты, описывающие конкретный результат, и после создания они могут быть выполнены узлом по запросу путем вызова метода `RunActionAsync` в `Node`.

Первое действие масштабирует поле с отскоком, а второе — бесконечно:

```csharp
await boxNode.RunActionsAsync(
    new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));
```

В приведенном выше примере показано, как первое действие, которое мы создаем, является `ScaleTo` действием. это просто рецепт, указывающий, что необходимо масштабировать в сторону значения, равного одному свойству Scale узла.  Это действие, в свою очередь, заключено вокруг действия плавности, `EaseBounceOut` действия.  Действия по ускорению искажают линейное выполнение действия и применяют результат, в данном случае он обеспечивает покрывающий результат.
Наш рецепт можно написать так:

```csharp
var recipe = new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1));
```

После создания рецепта мы выполним рецепт:

```csharp
await boxNode.RunActionsAsync (recipe)
```

Значение await указывает, что после завершения действия необходимо возобновить выполнение после этой строки.  После завершения действия мы активируем вторую анимацию.

В документе [using UrhoSharp](~/graphics-games/urhosharp/using.md) рассматриваются более подробные понятия, лежащие в основе Урхо, и способы структурирования кода для создания игры.

## <a name="copyrights"></a>Авторские права

В этой документации содержится первоначальное содержимое от Xamarin Inc, но оно широко выводится из документации по с открытым исходным кодом для проекта Urho3D и содержит снимки экрана из проекта Cocos2D.
