---
title: Отслеживание пальцев с несколькими касаниями в Xamarin. Android
description: В этом разделе показано, как отвести события касания от нескольких пальцев
ms.prod: xamarin
ms.assetid: 048D51F9-BD6C-4B44-8C53-CCEF276FC5CC
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
ms.openlocfilehash: f960c3cec90bd331f5a1433a869c7720b40c9680
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73024271"
---
# <a name="multi-touch-finger-tracking"></a>Отслеживание пальцев с несколькими касаниями

_В этом разделе показано, как отвести события касания от нескольких пальцев_

Иногда многоприкосновенное приложение должно относиться к отдельным пальцам, когда они одновременно перемещаются на экране. Одним из типичных приложений является программа рисования пальца. Вы хотите, чтобы пользователь мог рисовать с одним пальцем, а также рисовать с несколькими пальцами одновременно. По мере того как программа обрабатывает несколько событий касания, ей необходимо определить, какие события соответствуют каждому пальцю. В Android предоставляется код идентификатора для этой цели, но получение и обработка этого кода может быть немного сложным.

Для всех событий, связанных с конкретным пальцем, код идентификатора остается неизменным. Код идентификатора назначается, когда палец впервые соприкасается с экраном и становится недействительным после того, как палец отрывается от экрана.
Эти коды ИДЕНТИФИКАТОРов обычно являются очень мелкими целыми числами, и Android использует их для последующего события касания.

Почти всегда программа, которая отслеживает отдельных пальцев, поддерживает словарь для отслеживания касаний. Ключ словаря — это код идентификатора, определяющий конкретного пальца. Значение словаря зависит от приложения. В программе [финжерпаинт](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint) каждый палец (от прикосновения к выпуску) связан с объектом, который содержит всю информацию, необходимую для отрисовки линии, нарисованной с помощью этого пальца. Для этой цели программа определяет небольшой `FingerPaintPolyline` класс:

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

Каждая Ломаная линия имеет цвет, толщину штриха и графический [`Path`](xref:Android.Graphics.Path) Android для накопления и отрисовки нескольких точек линии по мере ее прорисовки.

Оставшаяся часть кода, показанного ниже, содержится в `View` производном с именем `FingerPaintCanvasView`. Этот класс поддерживает словарь объектов типа `FingerPaintPolyline` в течение времени, когда они активно нарисованы одним или несколькими пальцами:

```csharp
Dictionary<int, FingerPaintPolyline> inProgressPolylines = new Dictionary<int, FingerPaintPolyline>();
```

Этот словарь позволяет быстро получить сведения о `FingerPaintPolyline`, связанные с конкретным пальцем.

Класс `FingerPaintCanvasView` также поддерживает объект `List` для завершенных ломаных линий.

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Объекты в этом `List` находятся в том же порядке, в котором они были нарисованы.

`FingerPaintCanvasView` переопределяет два метода, определенных `View`: [`OnDraw`](xref:Android.Views.View.OnDraw*)
и [`OnTouchEvent`](xref:Android.Views.View.OnTouchEvent*).
При переопределении `OnDraw` в представлении рисуется завершенные ломаные линии, а затем выполняется рисование ломаных линий.

Переопределение метода `OnTouchEvent` начинается с получения значения `pointerIndex` из свойства `ActionIndex`. Это `ActionIndex`ное значение различает несколько пальцев, но не согласуется между несколькими событиями. По этой причине используйте `pointerIndex`, чтобы получить указатель `id` значение из метода `GetPointerId`. Этот идентификатор *согласован по нескольким* событиям:

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

Обратите внимание, что переопределение использует свойство `ActionMasked` в инструкции `switch`, а не в свойстве `Action`. Далее описывается, почему это происходит:

При работе с несколькими касаниями свойство `Action` имеет значение `MotionEventsAction.Down` для первого пальца, чтобы коснуться экрана, а затем значения `Pointer2Down` и `Pointer3Down`, как второй и третий пальцы также касаются экрана. Так как четвертый и пятый пальцы делают контакт, свойство `Action` имеет числовые значения, которые даже не соответствуют членам перечисления `MotionEventsAction`. Необходимо изучить значения битовых флагов в значениях, чтобы понять, что они означают.

Аналогично, когда пальцы оставляют контакт с экраном, свойство `Action` имеет значения `Pointer2Up` и `Pointer3Up` для второго и третьего пальца и `Up` для первого пальца.

Свойство `ActionMasked` принимает меньшее количество значений, поскольку оно предназначено для использования вместе со свойством `ActionIndex`, чтобы различать несколько пальцев. Когда пальцы надвигаются на экран, свойство может быть равно только `MotionEventActions.Down` для первого пальца и `PointerDown` для последующих пальцев. Когда пальцы покидают экран, `ActionMasked` имеет значения `Pointer1Up` для последующих пальцев и `Up` для первого пальца.

При использовании `ActionMasked``ActionIndex` различает следующие пальцы для сенсорного ввода и выхода из экрана, но обычно нет необходимости использовать это значение, за исключением аргументов других методов в объекте `MotionEvent`. Для нескольких касаний один из наиболее важных методов `GetPointerId` вызывается в приведенном выше коде. Этот метод возвращает значение, которое можно использовать для ключа словаря, чтобы связать определенные события с пальцами.

Переопределение `OnTouchEvent` в программе [финжерпаинт](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint) обрабатывает события `MotionEventActions.Down` и `PointerDown` идентично, создавая новый объект `FingerPaintPolyline` и добавляя его в словарь:

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

Обратите внимание, что `pointerIndex` также используется для получения позиций пальца в пределах представления. Все сенсорные сведения связаны со значением `pointerIndex`. `id` однозначно определяет пальцы в нескольких сообщениях, так что они используются для создания записи словаря.

Аналогично, переопределение `OnTouchEvent` также обрабатывает `MotionEventActions.Up` и `Pointer1Up` идентично, передавая заполненную ломаную линию в коллекцию `completedPolylines`, чтобы их можно было рисовать во время переопределения `OnDraw`. Код также удаляет запись `id` из словаря:

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

Между событиями «вниз» и «вверх» обычно существует много `MotionEventActions.Move` событий. Они объединены в один вызов `OnTouchEvent`и должны обрабатываться иначе, чем события `Down` и `Up`. Значение `pointerIndex`, полученное ранее из свойства `ActionIndex`, должно игнорироваться. Вместо этого метод должен получить несколько `pointerIndex` значений путем цикла между 0 и свойством `PointerCount`, а затем получить `id` для каждого из этих `pointerIndex` значений:

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

[Пример снимка экрана![примера из Финжерпаинт](touch-tracking-images/image01.png)](touch-tracking-images/image01.png#lightbox)

Теперь вы узнали, как можно отключать отдельные пальцы на экране и отличать их друг от друга.

## <a name="related-links"></a>Связанные ссылки

- [Эквивалентное руководством по Xamarin iOS](~/ios/app-fundamentals/touch/touch-tracking.md)
- [Финжерпаинт (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)
