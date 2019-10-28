---
title: Простая анимация в Xamarin. Forms
description: Класс Виевекстенсионс предоставляет методы расширения, которые можно использовать для создания простых анимаций. В этой статье показано, как создавать и отменять анимацию с помощью класса Виевекстенсионс.
ms.prod: xamarin
ms.assetid: 4A6FAE5A-848F-4CE0-BFA1-22A6309B5225
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2019
ms.openlocfilehash: 116911787db128b103fb555554076704a0549db5
ms.sourcegitcommit: f8583585c501607fdfa061b95e9a9f385ed1d591
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/26/2019
ms.locfileid: "72959167"
---
# <a name="simple-animations-in-xamarinforms"></a>Простая анимация в Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-basic)

_Класс Виевекстенсионс предоставляет методы расширения, которые можно использовать для создания простых анимаций. В этой статье показано, как создавать и отменять анимацию с помощью класса Виевекстенсионс._

Класс [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) предоставляет следующие методы расширения, которые можно использовать для создания простых анимаций:

- [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) анимирует свойства [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) и [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) [`VisualElement`](xref:Xamarin.Forms.VisualElement).
- [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) анимируется свойство [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) [`VisualElement`](xref:Xamarin.Forms.VisualElement).
- [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) применяет анимированное добавочное увеличение или уменьшение к свойству [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) [`VisualElement`](xref:Xamarin.Forms.VisualElement).
- [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) анимируется свойство [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) [`VisualElement`](xref:Xamarin.Forms.VisualElement).
- [`RelRotateTo`](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) применяет анимированное добавочное увеличение или уменьшение к свойству [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) [`VisualElement`](xref:Xamarin.Forms.VisualElement).
- [`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) анимируется свойство [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX) [`VisualElement`](xref:Xamarin.Forms.VisualElement).
- [`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) анимируется свойство [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY) [`VisualElement`](xref:Xamarin.Forms.VisualElement).
- [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) анимируется свойство [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) [`VisualElement`](xref:Xamarin.Forms.VisualElement).

По умолчанию каждая анимация займет 250 миллисекунд. Однако длительность каждой анимации может быть задана при создании анимации.

Класс [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) также включает метод [`CancelAnimations`](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) , который можно использовать для отмены любой анимации.

> [!NOTE]
> Класс [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) предоставляет метод расширения [`LayoutTo`](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing)) . Однако этот метод предназначен для использования в макетах для анимации переходов между состояниями макета, которые содержат изменения размера и расположения. Поэтому он должен использоваться только [`Layout`](xref:Xamarin.Forms.Layout) подклассах.

Методы расширения анимации в классе [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) являются асинхронными и возвращают объект `Task<bool>`. Возвращаемое значение `false`, если анимация завершается, и `true` при отмене анимации. Таким образом, методы анимации обычно следует использовать с оператором `await`, что позволяет легко определить время завершения анимации. Кроме того, можно создать последовательную анимацию с последующими методами анимации, выполняемыми после завершения предыдущего метода. Дополнительные сведения см. в разделе [Составные анимации](#compound).

Если есть требование разрешить выполнение анимации в фоновом режиме, то оператор `await` можно опустить. В этом сценарии методы расширения анимации быстро возвращаются после запуска анимации с анимацией, которая происходит в фоновом режиме. Эту операцию можно использовать при создании составных анимаций. Дополнительные сведения см. в разделе [Составные анимации](#composite).

Дополнительные сведения об операторе `await` см. в разделе [Общие сведения о поддержке асинхронных](~/cross-platform/platform/async.md)операций.

## <a name="single-animations"></a>Отдельные анимации

Каждый метод расширения в [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) реализует одну операцию анимации, которая постепенно изменяет свойство из одного значения на другое в течение определенного периода времени. В этом разделе рассматриваются все операции анимации.

### <a name="rotation"></a>Вращение

В следующем примере кода показано использование метода [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) для анимации свойства [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) [`Image`](xref:Xamarin.Forms.Image):

```csharp
await image.RotateTo (360, 2000);
image.Rotation = 0;
```

Этот код анимирует экземпляр [`Image`](xref:Xamarin.Forms.Image) , поворачивая до 360 градусов в течение 2 секунд (2000 миллисекунд). Метод [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) получает текущее значение свойства [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) для начала анимации, а затем поворачивает из этого значения в первый аргумент (360). После завершения анимации свойство [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) изображения сбрасывается в 0. Это гарантирует, что свойство `Rotation` не останется в 360 после завершения анимации, что помешает дополнительному повороту.

На следующих снимках экрана показан ход вращения на каждой платформе:

![](simple-images/rotateto.png "Rotation Animation")

### <a name="relative-rotation"></a>Относительный поворот

В следующем примере кода показано использование метода [`RelRotateTo`](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) для постепенного увеличения или уменьшения свойства [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) [`Image`](xref:Xamarin.Forms.Image):

```csharp
await image.RelRotateTo (360, 2000);
```

Этот код анимирует экземпляр [`Image`](xref:Xamarin.Forms.Image) , поворачивая 360 градусов от его начальной должности в течение 2 секунд (2000 миллисекунд). Метод [`RelRotateTo`](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) получает текущее значение свойства [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) для начала анимации, а затем поворачивает из этого значения в значение плюс его первый аргумент (360). Это гарантирует, что каждая анимация всегда будет начинаться с 360 градусов с начальной позицией. Таким образом, если новая анимация вызывается, пока анимация уже выполняется, она начнется с текущей позиции и может заканчиваться на позиции, которая не является инкрементом в 360 градусов.

На следующих снимках экрана показано, как выполняется относительное вращение на каждой платформе.

![](simple-images/relrotateto.png "Relative Rotation Animation")

### <a name="scaling"></a>Масштабирование

В следующем примере кода показано использование метода [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) для анимации свойства [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) [`Image`](xref:Xamarin.Forms.Image):

```csharp
await image.ScaleTo (2, 2000);
```

Этот код анимирует экземпляр [`Image`](xref:Xamarin.Forms.Image) путем увеличения масштаба до двух секунд (2000 миллисекунд). Метод [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) получает текущее значение свойства [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) (значение по умолчанию 1) для начала анимации, а затем масштабируется от этого значения до первого аргумента (2). Это приведет к увеличению размера изображения в два раза больше его размера.

На следующих снимках экрана показано, как выполняется масштабирование на каждой платформе:

![](simple-images/scaleto.png "Scaling Animation")

> [!NOTE]
> Класс [`VisualElement`](xref:Xamarin.Forms.VisualElement) также определяет свойства [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX) и [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY), которые могут масштабировать `VisualElement` по-разному в горизонтальном и вертикальном направлениях. Эти свойства можно анимировать с помощью класса [`Animation`](xref:Xamarin.Forms.Animation) . Дополнительные сведения см. [в статье пользовательские анимации в Xamarin. Forms](custom.md).

### <a name="relative-scaling"></a>Относительное масштабирование

В следующем примере кода показано использование метода [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) для анимации свойства [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) [`Image`](xref:Xamarin.Forms.Image):

```csharp
await image.RelScaleTo (2, 2000);
```

Этот код анимирует экземпляр [`Image`](xref:Xamarin.Forms.Image) путем увеличения масштаба до двух секунд (2000 миллисекунд). Метод [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) получает текущее значение свойства [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) для начала анимации, а затем масштабирует от этого значения до значения плюс его первый аргумент (2). Это гарантирует, что каждая анимация всегда будет масштабироваться на 2 из начального положения.

### <a name="scaling-and-rotation-with-anchors"></a>Масштабирование и поворот с помощью привязок

Свойства [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX) и [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) устанавливают центр масштабирования или вращения для свойств [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) и [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) . Поэтому их значения также влияют на методы [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) и [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) .

При наличии [`Image`](xref:Xamarin.Forms.Image) , размещенного в центре макета, в следующем примере кода показано вращение изображения вокруг центра макета путем установки его свойства [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) :

```csharp
double radius = Math.Min(absoluteLayout.Width, absoluteLayout.Height) / 2;
image.AnchorY = radius / image.Height;
await image.RotateTo(360, 2000);
```

Чтобы поворачивать экземпляр [`Image`](xref:Xamarin.Forms.Image) вокруг центра макета, свойствам [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX) и [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) необходимо присвоить значения, которые относятся к ширине и высоте `Image`. В этом примере центр `Image` определяется как центр макета, поэтому значение по умолчанию `AnchorX` 0,5 не требует изменения. Однако свойство `AnchorY` переопределяется как значение от верхнего края `Image` до центральной точки макета. Это гарантирует, что `Image` выполняет полный поворот на 360 градусов вокруг центральной точки макета, как показано на следующих снимках экрана:

![](simple-images/rotate-anchors.png "Rotation Animation with Anchors")

### <a name="translation"></a>Перевод

В следующем примере кода показано использование метода [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) для анимации [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) и [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) свойств [`Image`](xref:Xamarin.Forms.Image):

```csharp
await image.TranslateTo (-100, -100, 1000);
```

Этот код анимирует экземпляр [`Image`](xref:Xamarin.Forms.Image) , переобразуя его по горизонтали и по вертикали в течение 1 секунды (1000 миллисекунд). Метод [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) одновременно преобразовывает изображение 100 пикселей влево и 100 пикселей в сторону. Это связано с тем, что первый и второй аргументы являются отрицательными числами. При предоставлении положительных чисел изображение будет переводиться вправо и вниз.

На следующих снимках экрана показано, как выполняется трансляция на каждой платформе:

![](simple-images/translateto.png "Translation Animation")

> [!NOTE]
> Если элемент изначально размещается на экране, а затем преобразуется на экран, после перевода входной макет элемента остается вне экрана и пользователь не может взаимодействовать с ним. Поэтому рекомендуется размещать представление в окончательном положении, а затем выполнять все необходимые переводы.

### <a name="fading"></a>Исчезание

В следующем примере кода показано использование метода [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) для анимации свойства [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) [`Image`](xref:Xamarin.Forms.Image):

```csharp
image.Opacity = 0;
await image.FadeTo (1, 4000);
```

Этот код анимирует экземпляр [`Image`](xref:Xamarin.Forms.Image) , поменяя его на более 4 секунд (4000 миллисекунд). Метод [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) получает текущее значение свойства [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) для начала анимации, а затем исчезает из этого значения в первый аргумент (1).

На следующих снимках экрана показано, как выполняется выцветание на каждой платформе:

![](simple-images/fadeto.png "Fading Animation")

<a name="compound" />

## <a name="compound-animations"></a>Составные анимации

Составная анимация — это последовательное сочетание анимации, которую можно создать с помощью оператора `await`, как показано в следующем примере кода:

```csharp
await image.TranslateTo (-100, 0, 1000);    // Move image left
await image.TranslateTo (-100, -100, 1000); // Move image up
await image.TranslateTo (100, 100, 2000);   // Move image diagonally down and right
await image.TranslateTo (0, 100, 1000);     // Move image left
await image.TranslateTo (0, 0, 1000);       // Move image up
```

В этом примере [`Image`](xref:Xamarin.Forms.Image) переводится более 6 секунд (6000 миллисекунд). В переводе `Image` используется пять анимаций с оператором `await`, указывающим, что каждая анимация выполняется последовательно. Поэтому последующие методы анимации выполняются после завершения предыдущего метода.

<a name="composite" />

## <a name="composite-animations"></a>Составные анимации

Составная анимация — это сочетание анимаций, в которых одновременно выполняются две или более анимаций. Составные анимации можно создавать путем смешивания ожидающих и неожидаемых анимаций, как показано в следующем примере кода:

```csharp
image.RotateTo (360, 4000);
await image.ScaleTo (2, 2000);
await image.ScaleTo (1, 2000);
```

В этом примере [`Image`](xref:Xamarin.Forms.Image) масштабируется и одновременно поворачивается более 4 секунд (4000 миллисекунд). Масштабирование `Image` использует две последовательные анимации, которые происходят одновременно с поворотом. Метод [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) выполняется без оператора `await` и возвращает немедленно, с первой [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) анимацией, начиная с начала. Оператор `await` в первом вызове метода `ScaleTo` задерживает второй вызов метода `ScaleTo` до тех пор, пока не завершится первый вызов метода `ScaleTo`. На этом этапе анимация `RotateTo` является половиной, а `Image` будет повернуты на 180 градусов. В течение последних 2 секунд (2000 миллисекунд) вторая анимация `ScaleTo` и `RotateTo` анимация завершена.

### <a name="running-multiple-asynchronous-methods-concurrently"></a>Параллельное выполнение нескольких асинхронных методов

Методы `static` `Task.WhenAny` и `Task.WhenAll` используются для параллельного выполнения нескольких асинхронных методов, поэтому их можно использовать для создания составных анимаций. Оба метода возвращают объект `Task` и принимают коллекцию методов, каждый из которых возвращает объект `Task`. Метод `Task.WhenAny` завершается, когда выполнение любого метода в его коллекции завершается, как показано в следующем примере кода:

```csharp
await Task.WhenAny<bool>
(
  image.RotateTo (360, 4000),
  image.ScaleTo (2, 2000)
);
await image.ScaleTo (1, 2000);
```

В этом примере вызов метода `Task.WhenAny` содержит две задачи. Первая задача поворачивает изображение в течение 4 секунд (4000 миллисекунд), а вторая задача масштабирует изображение в течение 2 секунд (2000 миллисекунд). По завершении второй задачи вызов метода `Task.WhenAny` завершается. Однако несмотря на то, что метод [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) еще работает, может начаться второй метод [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) .

Метод `Task.WhenAll` завершается после завершения всех методов в его коллекции, как показано в следующем примере кода:

```csharp
// 10 minute animation
uint duration = 10 * 60 * 1000;

await Task.WhenAll (
  image.RotateTo (307 * 360, duration),
  image.RotateXTo (251 * 360, duration),
  image.RotateYTo (199 * 360, duration)
);
```

В этом примере вызов метода `Task.WhenAll` содержит три задачи, каждый из которых выполняется более 10 минут. Каждый `Task` устанавливает разное число 307 поворотов на 360 градусов для [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), 251 повороты для [`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))и 199 для [`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)). Эти значения являются простыми числами, поэтому они гарантируют, что вращение не синхронизированы и, следовательно, не будут приводить к созданию повторяющихся шаблонов.

На следующих снимках экрана показано, как выполняется несколько поворотов на каждой платформе:

![](simple-images/multiple-rotations.png "Composite Animation")

## <a name="canceling-animations"></a>Отмена анимации

Приложение может отменить одну или несколько анимаций с помощью вызова метода `static` [`ViewExtensions.CancelAnimations`](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) , как показано в следующем примере кода:

```csharp
ViewExtensions.CancelAnimations (image);
```

Это немедленно отменит все анимации, которые в данный момент выполняются на экземпляре [`Image`](xref:Xamarin.Forms.Image) .

## <a name="summary"></a>Сводка

В этой статье показано, как создавать и отменять анимацию с помощью класса [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) . Этот класс предоставляет методы расширения, которые можно использовать для создания простых анимаций, которые используются для вращения, масштабирования, преобразования и появления [`VisualElement`](xref:Xamarin.Forms.VisualElement) экземпляров.

## <a name="related-links"></a>Связанные ссылки

- [Общие сведения о поддержке асинхронного выполнения](~/cross-platform/platform/async.md)
- [Базовая анимация (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-basic)
- [виевекстенсионс](xref:Xamarin.Forms.ViewExtensions)
