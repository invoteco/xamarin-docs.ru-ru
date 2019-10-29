---
title: Трехмерные координаты в коигру
description: Понимание трехмерной системы координат является важным шагом при разработке трехмерных игр. В игре предусмотрено несколько классов для позиционирования, ориентации и масштабирования объектов в трехмерном пространстве.
ms.prod: xamarin
ms.assetid: A4130995-48FD-4E2E-9C2B-ADCEFF35BE3A
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: a38f4b81f684d6d416e6abe017bc463e3097c6b1
ms.sourcegitcommit: 93697a20e6fc7da547a8714ac109d7953b61d63f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/28/2019
ms.locfileid: "72980856"
---
# <a name="3d-coordinates-in-monogame"></a>Трехмерные координаты в коигру

_Понимание трехмерной системы координат является важным шагом при разработке трехмерных игр. В игре предусмотрено несколько классов для позиционирования, ориентации и масштабирования объектов в трехмерном пространстве._

Для разработки трехмерных игр необходимо понимать, как работать с объектами в трехмерной системе координат. В этом пошаговом руководстве рассматривается обработка визуальных объектов (в частности, модели). Мы создадим концепцию управления моделью для создания класса 3D Camera.

Представленные концепции берутся из линейной перебора, но мы будем использовать практический подход, чтобы любой пользователь без сложных математических вычислений мог применить эти концепции в своих играх.

Мы будем обсуждать следующие темы:

- Создание проекта
- Создание сущности робота
- Перемещение объекта робота
- Умножение матрицы
- Создание сущности камеры
- Перемещение камеры с входными данными

По завершении у нас будет проект с роботом, перемещающийся на окружность и камеру, которыми можно управлять с помощью сенсорного ввода:

![](part3-images/image1.gif "Once finished, the app will include a project with a robot moving in a circle and a camera which can be controlled by touch input")

## <a name="creating-a-project"></a>Создание проекта

В этом пошаговом руководстве рассматривается перемещение объектов в трехмерном пространстве. Начнем с проекта для подготовки к просмотру моделей и массивов вершин [, которые можно найти здесь](https://docs.microsoft.com/samples/xamarin/mobile-samples/modelsandvertsmg/). После скачивания распакуйте и откройте проект, чтобы убедиться, что он выполняется, и мы должны увидеть следующее:

![](part3-images/image2.png "Once downloaded, unzip and open the project to make sure it runs and this view should be displayed")

## <a name="creating-a-robot-entity"></a>Создание сущности робота

Прежде чем приступить к перемещению нашего робота, мы создадим `Robot` класс, который будет содержать логику для рисования и перемещения. Разработчики игр обращаются к этой инкапсуляции логики и данных в виде *сущности*.

Добавьте новый пустой файл класса в переносимую библиотеку классов **MonoGame3D** (не зависящую от платформы Моделандвертс. Android). Назовите его **робота** и нажмите кнопку **создать**:

![](part3-images/image3.png "Name it Robot and click New")

Измените класс `Robot` следующим образом:

```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Content;

namespace MonoGame3D
{
    public class Robot
    {
        Model model;

        public void Initialize(ContentManager contentManager)
        {
            model = contentManager.Load<Model> ("robot");

        }

        // For now we'll take these values in, eventually we'll
        // take a Camera object
        public void Draw(Vector3 cameraPosition, float aspectRatio)
        {
            foreach (var mesh in model.Meshes)
            {
                foreach (BasicEffect effect in mesh.Effects)
                {
                    effect.EnableDefaultLighting ();
                    effect.PreferPerPixelLighting = true;

                    effect.World = Matrix.Identity; 
                    var cameraLookAtVector = Vector3.Zero;
                    var cameraUpVector = Vector3.UnitZ;

                    effect.View = Matrix.CreateLookAt (
                        cameraPosition, cameraLookAtVector, cameraUpVector);

                    float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                    float nearClipPlane = 1;
                    float farClipPlane = 200;

                    effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                        fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

                }

                // Now that we've assigned our properties on the effects we can
                // draw the entire mesh
                mesh.Draw ();
            }
        }
    }
}
```

Код `Robot` — это, по сути, тот же код в `Game1` для рисования `Model`. Дополнительные сведения о `Model` загрузке и рисовании см. [в этом разделе, посвященном работе с моделями](~/graphics-games/monogame/3d/part1.md). Теперь можно удалить все `Model` загрузки и отображения кода из `Game1`и заменить его экземпляром `Robot`:

```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        Vector3 cameraPosition = new Vector3(15, 10, 10);

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;

            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

            floorVerts [0].Position = new Vector3 (-20, -20, 0);
            floorVerts [1].Position = new Vector3 (-20,  20, 0);
            floorVerts [2].Position = new Vector3 ( 20, -20, 0);

            floorVerts [3].Position = floorVerts[1].Position;
            floorVerts [4].Position = new Vector3 ( 20,  20, 0);
            floorVerts [5].Position = floorVerts[2].Position;

            int repetitions = 20;

            floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
            floorVerts [1].TextureCoordinate = new Vector2 (0, repetitions);
            floorVerts [2].TextureCoordinate = new Vector2 (repetitions, 0);

            floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
            floorVerts [4].TextureCoordinate = new Vector2 (repetitions, repetitions);
            floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

            effect = new BasicEffect (graphics.GraphicsDevice);

            robot = new Robot ();
            robot.Initialize (Content);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            float aspectRatio = 
                graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
            robot.Draw (cameraPosition, aspectRatio);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector);

            float aspectRatio = 
                graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
            float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
            float nearClipPlane = 1;
            float farClipPlane = 200;

            effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

            effect.TextureEnabled = true;
            effect.Texture = checkerboardTexture;

            foreach (var pass in effect.CurrentTechnique.Passes)
            {
                pass.Apply ();

                graphics.GraphicsDevice.DrawUserPrimitives (
                            PrimitiveType.TriangleList,
                    floorVerts,
                    0,
                    2);
            }
        }
    }
}
```

Если мы выполним код, то у нас будет сцена с одним роботом, который рисуется главным образом под пол:

![](part3-images/image4.png "If the code is run now, the app will display a scene with only one robot which is drawn mostly under the floor")

## <a name="moving-the-robot"></a>Перемещение робота

Теперь, когда у нас есть класс `Robot`, можно добавить логику перемещения в робот. В этом случае мы просто перейдем робота в круге в соответствии со временем игры. Это непрактичная реализация для реальной игры, так как символ обычно может реагировать на входные или искусственные аналитики, но он предоставляет среду для изучения трехмерного позиционирования и вращения.

Единственными сведениями, которые потребуются за пределами класса `Robot`, является текущее время игры. Мы добавим метод `Update`, который будет принимать `GameTime` параметр. Этот параметр `GameTime` будет использоваться для увеличения переменной угла, которая будет использоваться для определения конечной должности робота.

Сначала мы добавим поле Angle в класс `Robot` в поле `model`:

```csharp
public class Robot
{
    public Model model;

    // new code:
    float angle;
    ...
```

 Теперь можно увеличить это значение в функции `Update`:

```csharp
public void Update(GameTime gameTime)
{
    // TotalSeconds is a double so we need to cast to float
    angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
}
```

Необходимо убедиться, что метод `Update` вызывается из `Game1.Update`:

```csharp
protected override void Update(GameTime gameTime)
{
    robot.Update (gameTime);
    base.Update(gameTime);
}
```

Конечно, на этом этапе поле угла не выполняет никаких действий — необходимо написать код для его использования. Мы изменим метод `Draw`, чтобы мы могли вычислить `Matrix` мира в выделенном методе: 

```csharp
public void Draw(Vector3 cameraPosition, float aspectRatio)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;
            // We’ll be doing our calculations here...
            effect.World = GetWorldMatrix();

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector);

            float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
            float nearClipPlane = 1;
            float farClipPlane = 200;

            effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
        }

        mesh.Draw ();
    }
}
```

Далее мы реализуем метод `GetWorldMatrix` в классе `Robot`:

```csharp
Matrix GetWorldMatrix()
{
    const float circleRadius = 8;
    const float heightOffGround = 3;

    // this matrix moves the model "out" from the origin
    Matrix translationMatrix = Matrix.CreateTranslation (
        circleRadius, 0, heightOffGround);

    // this matrix rotates everything around the origin
    Matrix rotationMatrix = Matrix.CreateRotationZ (angle);

    // We combine the two to have the model move in a circle:
    Matrix combined = translationMatrix * rotationMatrix;

    return combined;
}
```

Результат выполнения этого кода приводит к тому, что робот перемещается по окружности:

![](part3-images/image5.gif "Running this code results in the robot moving in a circle")

## <a name="matrix-multiplication"></a>Умножение матрицы

Приведенный выше код поворачивает робот, создавая `Matrix` в методе `GetWorldMatrix`. Структура `Matrix` содержит 16 значений float, которые можно использовать для перевода (задания расположения), вращения и масштабирования (Задание размера). При назначении свойства `effect.World` мы сообщаем базовой системе отрисовки, как позиционировать, масштабировать и ориентироваться, как это происходит (`Model` или геометрия из вершин). 

К счастью, структура `Matrix` включает ряд методов, упрощающих создание общих типов матриц. Первый использованный в приведенном выше коде — `Matrix.CreateTranslation`. *Преобразование* математического термина относится к операции, которая приводит к перемещению точки (или в нашем случае модели) из одного места в другое без каких-либо других изменений (например, при повороте или изменении размера). Функция принимает значение X, Y и Z для перевода. Если просмотреть сцену сверху вниз, наш метод `CreateTranslation` (в изоляции) выполняет следующие действия:

![](part3-images/image6.png "The CreateTranslation method in isolation performs this action")

Вторая создаваемая матрица — это матрица вращения, использующая матрицу `CreateRotationZ`. Это один из трех методов, которые можно использовать для создания вращения:

- `CreateRotationX`
- `CreateRoationY`
- `CreateRotationZ`

Каждый метод создает матрицу вращения путем поворота по заданной оси. В нашем случае мы вращающимися по оси Z, которая указывает на «up». Ниже приведены сведения о том, как работает вращение на основе осей.

![](part3-images/image7.png "This can help visualize how axis-based rotation works")

Мы также используем метод `CreateRotationZ` с полем угла, которое увеличивается с течением времени из-за вызова метода `Update`. В результате метод `CreateRotationZ` приводит к орбите нашего робота вокруг источника по мере того, как прошло время.

Последняя строка кода объединяет две матрицы в одну:

```csharp
Matrix combined = translationMatrix * rotationMatrix;
```

Это называется умножением матрицы, которое работает немного иначе, чем регулярное умножение. *Свойство коммутативной для умножения* указывает, что порядок чисел в операции умножения не изменяет результат. То есть 3 \* 4 эквивалентны 4 \* 3. Умножение матрицы отличается тем, что оно не коммутативной. Таким образом, приведенная выше строка может быть считана как «Apply Транслатионматрикс To Move a Model, а затем вращать все, применяя Ротатионматрикс». Можно визуализировать, как следующая строка влияет на расположение и поворот следующим образом:

![](part3-images/image8.png "A visualization pf the way that the above line affects the position and rotation")

Чтобы понять, как порядок умножения матрицы может повлиять на результат, рассмотрите следующее, где перемножение матрицы будет инвертировано:

```csharp
Matrix combined = rotationMatrix * translationMatrix;
```

Приведенный выше код сначала поместит модель на месте, а затем преобразует ее:

![](part3-images/image9.png "The code above would first rotate the model in-place, then translate it")

При выполнении кода с инвертированным умножением мы заметите, что поскольку поворот применяется первыми, он влияет только на ориентацию модели, и положение модели остается неизменным. Иными словами, модель поворачивается на месте:

![](part3-images/image10.gif "The model rotates in place")

## <a name="creating-the-camera-entity"></a>Создание сущности камеры

Сущность `Camera` будет содержать всю логику, необходимую для выполнения перемещения на основе ввода и предоставления свойств для назначения свойств классу `BasicEffect`.

Во-первых, мы реализуем статическую камеру (без перемещения на основе ввода) и свяжем ее с существующим проектом. Добавьте новый класс в переносимую библиотеку классов **MonoGame3D** (тот же проект с `Robot.cs`) и назовите ее **Camera**. Замените все содержимое этого файла следующим кодом:

```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Camera
    {
        // We need this to calculate the aspectRatio
        // in the ProjectionMatrix property.
        GraphicsDevice graphicsDevice;

        Vector3 position = new Vector3(15, 10, 10);

        public Matrix ViewMatrix
        {
            get
            {
                var lookAtVector = Vector3.Zero;
                var upVector = Vector3.UnitZ;

                return Matrix.CreateLookAt (
                    position, lookAtVector, upVector);
            }
        }

        public Matrix ProjectionMatrix
        {
            get
            {
                float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                float nearClipPlane = 1;
                float farClipPlane = 200;
                float aspectRatio = graphicsDevice.Viewport.Width / (float)graphicsDevice.Viewport.Height;

                return Matrix.CreatePerspectiveFieldOfView(
                    fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
            }
        }

        public Camera(GraphicsDevice graphicsDevice)
        {
            this.graphicsDevice = graphicsDevice;
        }

        public void Update(GameTime gameTime)
        {
            // We'll be doing some input-based movement here
        }
    }
}
```

Приведенный выше код очень похож на код из `Game1` и `Robot`, который назначает матрицы `BasicEffect`. 

Теперь мы можем интегрировать новый класс `Camera` в существующие проекты. Во-первых, мы изменим класс `Robot`, чтобы взять `Camera` экземпляр в своем методе `Draw`, что приведет к исключению большого объема повторяющегося кода. Замените метод `Robot.Draw` следующим:

```csharp
public void Draw(Camera camera)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = GetWorldMatrix();
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;
        }

        mesh.Draw ();
    }
}
```

Затем измените файл `Game1.cs`:

```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        // New camera code
        Camera camera;

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;

            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

            floorVerts [0].Position = new Vector3 (-20, -20, 0);
            floorVerts [1].Position = new Vector3 (-20,  20, 0);
            floorVerts [2].Position = new Vector3 ( 20, -20, 0);

            floorVerts [3].Position = floorVerts[1].Position;
            floorVerts [4].Position = new Vector3 ( 20,  20, 0);
            floorVerts [5].Position = floorVerts[2].Position;

            int repetitions = 20;

            floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
            floorVerts [1].TextureCoordinate = new Vector2 (0, repetitions);
            floorVerts [2].TextureCoordinate = new Vector2 (repetitions, 0);

            floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
            floorVerts [4].TextureCoordinate = new Vector2 (repetitions, repetitions);
            floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

            effect = new BasicEffect (graphics.GraphicsDevice);

            robot = new Robot ();
            robot.Initialize (Content);

            // New camera code
            camera = new Camera (graphics.GraphicsDevice);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            robot.Update (gameTime);
            // New camera code
            camera.Update (gameTime);
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            // New camera code
            robot.Draw (camera);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
            // New camera code
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;

            effect.TextureEnabled = true;
            effect.Texture = checkerboardTexture;

            foreach (var pass in effect.CurrentTechnique.Passes)
            {
                pass.Apply ();

                graphics.GraphicsDevice.DrawUserPrimitives (
                            PrimitiveType.TriangleList,
                    floorVerts,
                    0,
                    2);
            }
        }
    }
}
```

Изменения `Game1` из предыдущей версии (которая определяется `// New camera code`):

- `Camera` поле в `Game1`
- Создание экземпляра `Camera` в `Game1.Initialize`
- `Camera.Update` вызов в `Game1.Update`
- `Robot.Draw` теперь принимает `Camera` параметр
- `Game1.Draw` теперь использует `Camera.ViewMatrix` и `Camera.ProjectionMatrix`

## <a name="moving-the-camera-with-input"></a>Перемещение камеры с входными данными

До сих пор мы добавили сущность `Camera`, но ничего не сделали с ней, чтобы изменить поведение во время выполнения. Мы добавим поведение, которое позволит пользователю:

- Коснитесь левой стороны экрана, чтобы превратить камеру в сторону левого
- Коснитесь правой части экрана, чтобы повернуть камеру вправо
- Коснитесь центра экрана, чтобы переместить камеру вперед

### <a name="making-lookat-relative"></a>Создание относительных Лукат

Сначала мы изменим класс `Camera`, включив в него поле `angle`, которое будет использоваться для задания направления `Camera`. В настоящее время наш `Camera` определяет направление, с которым он проходит через локальную `lookAtVector`, назначенную `Vector3.Zero`. Другими словами, наши `Camera` всегда просматривают источник. Если камера перемещается, будет также изменен угол, на который направлена камера.

![](part3-images/image11.gif "If the Camera moves, then the angle that the camera is facing will also change")

Нам нужно, чтобы `Camera` было направлено в одно и то же направление независимо от его позиции — по крайней мере до реализации логики для поворота `Camera` с помощью входных данных. Первое изменение заключается в настройке `lookAtVector` переменной, которая будет основываться на текущем месте, а не на абсолютной позиции:

```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    // Let's start at X = 0 so we're looking at things head-on
    Vector3 position = new Vector3(0, 20, 10);

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    }
    ...
```

Это приводит к `Camera`ному просмотру мира. Обратите внимание, что начальное `position` значение было изменено на `(0, 20, 10)`, поэтому `Camera` выравнивается по оси X. При запуске игры отображается следующее:

![](part3-images/image12.png "Running the game displays this view")

### <a name="creating-an-angle-variable"></a>Создание переменной угла

Переменная `lookAtVector` определяет угол, который просматривает камера. В настоящее время она исправлена для просмотра отрицательной оси Y и немного наклонена вниз (от `-.5f` Z). Мы создадим переменную `angle`, которая будет использоваться для настройки свойства `lookAtVector`. 

В предыдущих разделах этого пошагового руководства мы показали, что матрицы можно использовать для поворота способа рисования объектов. Можно также использовать матрицы для вращения векторов, таких как `lookAtVector`, с помощью метода `Vector3.Transform`. 

Добавьте поле `angle` и измените свойство `ViewMatrix` следующим образом:

```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    Vector3 position = new Vector3(0, 20, 10);

    float angle;

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            // We'll create a rotation matrix using our angle
            var rotationMatrix = Matrix.CreateRotationZ (angle);
            // Then we'll modify the vector using this matrix:
            lookAtVector = Vector3.Transform (lookAtVector, rotationMatrix);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    }
    ...
```

### <a name="reading-input"></a>Чтение входных данных

Теперь наша сущность `Camera` можно полностью контролировать с помощью ее переменных "расположение" и "угол" — нам просто нужно изменить их в соответствии с входными данными.

Во-первых, мы получаем состояние `TouchPanel`, чтобы найти место, в котором пользователь касается экрана. Дополнительные сведения об использовании класса `TouchPanel` см. [в справочнике по API таучпанел](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_Touch_TouchPanel).

Если пользователь касается левого третьего угла, то мы изменим значение `angle`, чтобы `Camera` поворачиваться влево, а если пользователь касается правой трети, мы покажем другой способ. Если пользователь касается в среднем третьем углу экрана, мы перейдем `Camera` вперед.

Сначала добавьте оператор using для уточнения классов `TouchPanel` и `TouchCollection` в `Camera.cs`:

```csharp
using Microsoft.Xna.Framework.Input.Touch; 
```

Затем измените метод `Update`, чтобы прочесть сенсорную панель и правильно настроить `angle` и `position` переменные:

```csharp
public void Update(GameTime gameTime)
{
    TouchCollection touchCollection = TouchPanel.GetState();

    bool isTouchingScreen = touchCollection.Count > 0;
    if (isTouchingScreen)
    {
        var xPosition = touchCollection [0].Position.X;

        float xRatio = xPosition / (float)graphicsDevice.Viewport.Width;

        if (xRatio < 1 / 3.0f)
        {
            angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
        else if (xRatio < 2 / 3.0f)
        {
            var forwardVector = new Vector3 (0, -1, 0);

            var rotationMatrix = Matrix.CreateRotationZ (angle);
            forwardVector = Vector3.Transform (forwardVector, rotationMatrix);

            const float unitsPerSecond = 3;

            this.position += forwardVector * unitsPerSecond *
                (float)gameTime.ElapsedGameTime.TotalSeconds ;
        }
        else
        {
            angle -= (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
    }
}
```

Теперь `Camera` будет отвечать на сенсорный ввод:

![](part3-images/image1.gif "Now the Camera will respond to touch input")

Метод Update начинается с вызова `TouchPanel.GetState`, который возвращает коллекцию касаний. Хотя `TouchPanel.GetState` может возвращать несколько точек касания, для простоты мы будем беспокоиться только о первой.

Если пользователь касается экрана, код проверяет, находится ли первое касание в левой, средней или правой третьей части экрана. Левая и правая третья стороны поворачивают камеру путем увеличения или уменьшения `angle` переменной в соответствии со значением `TotalSeconds` (так что игра ведет себя одинаково независимо от частоты кадров).

Если пользователь касается Центральной трети экрана, то Камера перейдет вперед. Сначала это достигается путем получения прямого вектора, который изначально определен как указатель на отрицательную ось Y, затем поворачивается матрицей, созданной с помощью `Matrix.CreateRotationZ` и значения `angle`. Наконец, `forwardVector` применяется к `position` с использованием коэффициента `unitsPerSecond`.

## <a name="summary"></a>Сводка

В этом пошаговом руководстве описывается, как перемещать и поворачивать `Models` в трехмерном пространстве с помощью `Matrices` и свойства `BasicEffect.World`. Такая форма перемещения предоставляет базу для перемещения объектов в трехмерных играх. В этом пошаговом руководстве также описано, как реализовать сущность `Camera` для просмотра мира из любой точки и угла.

## <a name="related-links"></a>Связанные ссылки

- [Ссылка на API-интерфейс на коигру](http://www.monogame.net/documentation/?page=api)
- [Завершенный проект (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/monogame3dcamera)
