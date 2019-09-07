---
title: Отслеживание пальцев с несколькими касаниями в Xamarin. Android
description: В этом разделе показано, как отвести события касания от нескольких пальцев
ms.prod: xamarin
ms.assetid: 048D51F9-BD6C-4B44-8C53-CCEF276FC5CC
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 6dd3bf848d38f0211dcda100994f6f7ec8831fce
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70754692"
---
# <a name="multi-touch-finger-tracking"></a>Отслеживание пальцев с несколькими касаниями

_В этом разделе показано, как отвести события касания от нескольких пальцев_

Иногда многоприкосновенное приложение должно относиться к отдельным пальцам, когда они одновременно перемещаются на экране. Одним из типичных приложений является программа рисования пальца. Вы хотите, чтобы пользователь мог рисовать с одним пальцем, а также рисовать с несколькими пальцами одновременно. По мере того как программа обрабатывает несколько событий касания, ей необходимо определить, какие события соответствуют каждому пальцю. В Android предоставляется код идентификатора для этой цели, но получение и обработка этого кода может быть немного сложным.

Для всех событий, связанных с конкретным пальцем, код идентификатора остается неизменным. Код идентификатора назначается, когда палец впервые соприкасается с экраном и становится недействительным после того, как палец отрывается от экрана.
Эти коды ИДЕНТИФИКАТОРов обычно являются очень мелкими целыми числами, и Android использует их для последующего события касания.

Почти всегда программа, которая отслеживает отдельных пальцев, поддерживает словарь для отслеживания касаний. Ключ словаря — это код идентификатора, определяющий конкретного пальца. Значение словаря зависит от приложения. В программе [финжерпаинт](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint) каждый палец (от прикосновения к выпуску) связан с объектом, который содержит всю информацию, необходимую для отрисовки линии, нарисованной с помощью этого пальца. Для этой цели программа определяет `FingerPaintPolyline` небольшой класс:

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new Path();
    }

    public Color Color { set; get; }

    public float StrokeWidth { set; get; }

    public Path Path { private set; get; }
}
```

Каждая Ломаная линия имеет цвет, толщину штриха и графический [`Path`](xref:Android.Graphics.Path) объект Android для накопления и отрисовки нескольких точек линии при ее прорисовке.

Оставшаяся часть кода, показанного ниже, содержится в `View` производном `FingerPaintCanvasView`с именем. Этот класс поддерживает словарь объектов типа `FingerPaintPolyline` в течение времени, когда они активно нарисованы одним или несколькими пальцами:

```csharp
Dictionary<int, FingerPaintPolyline> inProgressPolylines = new Dictionary<int, FingerPaintPolyline>();
```

Этот словарь позволяет быстро получить `FingerPaintPolyline` информацию, связанную с конкретным пальцем.

`FingerPaintCanvasView` Класс также`List` поддерживает объект для завершенных ломаных линий:

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Объекты в этой `List` области находятся в том же порядке, в котором они были выведены.

`FingerPaintCanvasView`переопределяет два метода, `View`определяемых:[`OnDraw`](xref:Android.Views.View.OnDraw*)
и [`OnTouchEvent`](xref:Android.Views.View.OnTouchEvent*).
В своем `OnDraw` переопределении представление рисует завершенные ломаные линии, а затем рисует выполняющиеся ломаные линии.

Переопределение `OnTouchEvent` метода начинается с `pointerIndex` получения значения из `ActionIndex` свойства. Это `ActionIndex` значение различает несколько пальцев, но не согласуется между несколькими событиями. По этой причине `pointerIndex` для получения значения указателя `id` из `GetPointerId` метода используется метод. Этот идентификатор *согласован по нескольким* событиям:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // Get the pointer index
    int pointerIndex = args.ActionIndex;

    // Get the id to identify a finger over the course of its progress
    int id = args.GetPointerId(pointerIndex);

    // Use ActionMasked here rather than Action to reduce the number of possibilities
    switch (args.ActionMasked)
    {
        // ...
    }

    // Invalidate to update the view
    Invalidate();

    // Request continued touch input
    return true;
}
```

Обратите внимание, что переопределение использует `ActionMasked` свойство в инструкции, `switch` а `Action` не свойство. Далее описывается, почему это происходит:

При `Action` работе с несколькими касаниями свойство имеет `MotionEventsAction.Down` значение для первого пальца `Pointer2Down` , чтобы коснуться экрана, а затем значения и `Pointer3Down` , как второй и третий пальцы, также касаются экрана. Так как четвертый и пятый пальцы делают контакт, `Action` свойство имеет числовые значения, которые даже не соответствуют членам `MotionEventsAction` перечисления! Необходимо изучить значения битовых флагов в значениях, чтобы понять, что они означают.

Аналогично, когда пальцы оставляют контакт `Action` с экраном, свойство имеет `Pointer2Up` значения и `Pointer3Up` для второго и третьего пальца, а `Up` также для первого пальца.

Свойство принимает меньшее число значений, так как оно предназначено для использования вместе `ActionIndex` со свойством для различения нескольких пальцев. `ActionMasked` Когда пальцы надвигаются на экран, свойство может быть равно `MotionEventActions.Down` только для первого пальца `PointerDown` и для последующих пальцев. Когда пальцы покидают экран, `ActionMasked` они имеют `Pointer1Up` значения для последующих пальцев и `Up` для первого пальца.

При использовании `ActionMasked` `ActionIndex` метод различает следующие пальцы для сенсорного ввода и выхода из экрана, но обычно не требуется использовать это значение, за исключением аргументов других методов в `MotionEvent` объекте. Для нескольких касаний один из наиболее важных методов `GetPointerId` вызывается в приведенном выше коде. Этот метод возвращает значение, которое можно использовать для ключа словаря, чтобы связать определенные события с пальцами.

Переопределение `OnTouchEvent` в программе [FingerPaint](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint) обрабатывает события `MotionEventActions.Down` и `PointerDown` идентично, создавая новый объект `FingerPaintPolyline` и добавляя его в словарь:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // Get the pointer index
    int pointerIndex = args.ActionIndex;

    // Get the id to identify a finger over the course of its progress
    int id = args.GetPointerId(pointerIndex);

    // Use ActionMasked here rather than Action to reduce the number of possibilities
    switch (args.ActionMasked)
    {
        case MotionEventActions.Down:
        case MotionEventActions.PointerDown:

            // Create a Polyline, set the initial point, and store it
            FingerPaintPolyline polyline = new FingerPaintPolyline
            {
                Color = StrokeColor,
                StrokeWidth = StrokeWidth
            };

            polyline.Path.MoveTo(args.GetX(pointerIndex),
                                 args.GetY(pointerIndex));

            inProgressPolylines.Add(id, polyline);
            break;
        // ...
    }
    // ...        
}
```

Обратите внимание `pointerIndex` , что объект также используется для получения позиций пальца в пределах представления. Все сенсорные сведения связаны со `pointerIndex` значением. `id` Уникально определяет пальцы в нескольких сообщениях, поэтому для создания записи словаря используется.

`completedPolylines` `MotionEventActions.Up` `Pointer1Up` Аналогичным образом, `OnDraw` переопределение также обрабатывает и идентично, передавая заполненную ломаную линию в коллекцию, чтобы их можно было рисовать во время переопределения. `OnTouchEvent` Код также удаляет `id` запись из словаря:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // ...
    switch (args.ActionMasked)
    {
        // ...
        case MotionEventActions.Up:
        case MotionEventActions.Pointer1Up:

            inProgressPolylines[id].Path.LineTo(args.GetX(pointerIndex),
                                                args.GetY(pointerIndex));

            // Transfer the in-progress polyline to a completed polyline
            completedPolylines.Add(inProgressPolylines[id]);
            inProgressPolylines.Remove(id);
            break;

        case MotionEventActions.Cancel:
            inProgressPolylines.Remove(id);
            break;
    }
    // ...        
}
```

Теперь для несложной части.

Между событиями «вниз» и «вверх» обычно `MotionEventActions.Move` существует много событий. Они объединены в один вызов `OnTouchEvent`и должны обрабатываться иначе `Down` , чем события и `Up` . Значение `pointerIndex` , полученное ранее из `ActionIndex` свойства, должно игнорироваться. Вместо этого метод `pointerIndex` должен получить несколько значений путем цикла между 0 `PointerCount` и свойством `id` , а затем получить для каждого из этих `pointerIndex` значений:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // ...
    switch (args.ActionMasked)
    {
        // ...
        case MotionEventActions.Move:

            // Multiple Move events are bundled, so handle them differently
            for (pointerIndex = 0; pointerIndex < args.PointerCount; pointerIndex++)
            {
                id = args.GetPointerId(pointerIndex);

                inProgressPolylines[id].Path.LineTo(args.GetX(pointerIndex),
                                                    args.GetY(pointerIndex));
            }
            break;
        // ...
    }
    // ...        
}
```

Этот тип обработки позволяет программе [финжерпаинт](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint) отменять отдельные пальцы и отображать результаты на экране:

[![Пример снимка экрана из примера Финжерпаинт](touch-tracking-images/image01.png)](touch-tracking-images/image01.png#lightbox)

Теперь вы узнали, как можно отключать отдельные пальцы на экране и отличать их друг от друга.

## <a name="related-links"></a>Связанные ссылки

- [Эквивалентное руководством по Xamarin iOS](~/ios/app-fundamentals/touch/touch-tracking.md)
- [Финжерпаинт (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)
