---
title: SpriteKit в Xamarin. iOS
description: В этом документе описывается SpriteKit, платформа двухмерной графики Apple, которая интегрируется с SceneKit, включает в себя графическую и анимацию, поддерживает освещение и заливку, а также многое другое. SpriteKit можно использовать для создания двумерных игр.
ms.prod: xamarin
ms.assetid: 93971DAE-ED6B-48A8-8E61-15C0C79786BB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/14/2017
ms.openlocfilehash: e8829211ebf06eea224eade3f1b9d836207cdd64
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032474"
---
# <a name="spritekit-in-xamarinios"></a>SpriteKit в Xamarin. iOS

SpriteKit, платформа двухмерной графики от Apple, имеет некоторые интересные новые функции в iOS 8 и OS X Yosemite. К ним относятся интеграция с SceneKit, поддержка шейдера, освещение, тени, ограничения, нормальное создание карт и усовершенствования физических систем. В частности, новые физические функции позволяют легко добавлять реалистичные эффекты в игру.

## <a name="physics-bodies"></a>Физические тексты

SpriteKit включает в себя 2D-и жесткий API физикы тела. У каждого спрайта есть связанная физическая основная часть (`SKPhysicsBody`), которая определяет свойства физикы, такие как масса и трение, а также геометрию тела в физике мира.

## <a name="creating-a-physics-body-from-a-texture"></a>Создание основного текста из текстуры
SpriteKit теперь поддерживает получение от текстуры основной части спрайта. Это позволяет легко реализовать конфликты, которые выглядят более естественными.

Например, обратите внимание, что в следующем конфликте, как «полукруглая» и обезьяна конфликтуют почти на поверхности каждого изображения:

![](spritekit-images/image13.png "The banana and monkey collide nearly at the surface of each image")

SpriteKit создает такое физическое тело с одной строкой кода. Просто вызовите `SKPhysicsBody.Create` с текстурой и размером: спрайт. Фисиксбоди = Скфисиксбоди. Create (спрайт. Текстура, спрайт. Размер);

## <a name="alpha-threshold"></a>Пороговое значение Alpha

Помимо просто установки свойства `PhysicsBody` непосредственно к геометрии, производной от текстуры, приложения могут устанавливать и пороговое значение Alpha для управления наследованием геометрии. 

Порог Alpha определяет минимальное альфа-значение, которое необходимо добавить в результирующий основной текст. Например, следующий код приводит к слегка отличающимся основным текстом:

```chsarp
sprite.PhysicsBody = SKPhysicsBody.Create (sprite.Texture, 0.7f, sprite.Size);
```

Изменение порога альфа, подобного, настраивает предыдущее противоречие, так что обезьяна падает при конфликте с «некотором»:

![](spritekit-images/image14.png "The monkey falls over when colliding with the banana")

## <a name="physics-fields"></a>Поля физикы

Еще одним отличным дополнением к SpriteKit является поддержка новых полей физики. Они позволяют добавлять такие элементы, как поля вортекс, поля и пружины радиальных полей, чтобы наименовать всего несколько.

Физические поля создаются с помощью класса Скфиелдноде, который добавляется в сцену так же, как и любые другие `SKNode`. В `SKFieldNode` есть разнообразные заводские методы для создания различных физических полей. Можно создать пружинное поле, вызвав `SKFieldNode.CreateSpringField()`, поле "сила притяжения", вызвав `SKFieldNode.CreateRadialGravityField()`и т. д.

`SKFieldNode` также имеет свойства для управления атрибутами полей, такими как сила поля, область поля и затухание для принудительного применения полей.

## <a name="spring-field"></a>Пружинное поле

Например, следующий код создает поле пружины и добавляет его в сцену:

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateSpringField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 0.5f;
fieldNode.Region = new SKRegion(Frame.Size);
AddChild (fieldNode);
```

Затем можно добавить спрайты и задать их свойства `PhysicsBody`, чтобы поле физика влияло на спрайты, как показано в следующем коде, когда пользователь касается экрана:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    var touch = touches.AnyObject as UITouch;
    var pt = touch.LocationInNode (this);
    var node = SKSpriteNode.FromImageNamed ("TinyBanana");
    node.PhysicsBody = SKPhysicsBody.Create (node.Texture, node.Size);
    node.PhysicsBody.AffectedByGravity = false;
    node.PhysicsBody.AllowsRotation = true;
    node.PhysicsBody.Mass = 0.03f;
    node.Position = pt;
    AddChild (node);
}
```

Это приводит к тому, что «осЦиллате» в «весной» вокруг узла поля:

![](spritekit-images/image15.png "The bananas oscillate like a spring around the field node")

## <a name="radial-gravity-field"></a>Поле тяжести радиуса

Добавление другого поля аналогично. Например, следующий код создает поле с радиальной тяжестью:

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateRadialGravityField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 10.0f;
fieldNode.Falloff = 1.0f;
```

В результате получается другое поле силы, в котором «полукруглые» извлекаются радиально по полю:

![](spritekit-images/image16.png "The bananas are pulled radially around the field")
