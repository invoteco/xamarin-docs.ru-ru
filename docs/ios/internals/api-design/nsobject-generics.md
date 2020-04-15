---
title: Универсальные подклассы NSObject в Xamarin.iOS
description: В этом документе описывается создание универсальных подклассов NSObject. Здесь описывается, что может и не может быть сделано, обсуждается статический регистратор и уделяется внимание производительности.
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 279fcac1611038613bf442e1b766fda45dd5a429
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "73022365"
---
# <a name="generic-subclasses-of-nsobject-in-xamarinios"></a>Универсальные подклассы NSObject в Xamarin.iOS

## <a name="using-generics-with-nsobjects"></a>Использование универсальных шаблонов совместно с NSObject

Универсальные шаблоны можно использовать в подклассах `NSObject`, например [UIView](xref:UIKit.UIView):

```csharp
class Foo<T> : UIView {
    public Foo (CGRect x) : base (x) {}
    public override void Draw (CoreGraphics.CGRect rect)
    {
        Console.WriteLine ("T: {0}. Type: {1}", typeof (T), GetType ().Name);
    }
}
```

Поскольку объекты, подкласс `NSObject` которых зарегистрирован в среде выполнения Objective-C, имеют некоторые ограничения на возможности универсальных подклассов типов `NSObject`.

## <a name="considerations-for-generic-subclasses-of-nsobject"></a>Рекомендации для универсальных подклассов NSObject

В этом документе описаны ограничения в ограниченной поддержке универсальных подклассов `NSObjects`.

### <a name="generic-type-arguments-in-member-signatures"></a>Аргументы универсального типа в сигнатурах элементов

Все аргументы универсального типа в сигнатуре элементов, доступных для Objective-C, должны иметь ограничение `NSObject`.

**Хорошо**:

```csharp
class Generic<T> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**Причина**: Параметр универсального типа — это `NSObject`, поэтому сигнатура селектора для `myMethod:` может быть безопасно предоставлена Objective-C (это всегда будет `NSObject` или его подкласс).

**Плохо**:

```csharp
class Generic<T> : NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**Причина**: невозможно создать сигнатуру Objective-C для экспортируемых элементов, которые может вызывать код Objective-C, так как сигнатура будет отличаться в зависимости от конкретного типа универсального типа `T`.

**Хорошо**:

```csharp
class Generic<T> : NSObject
{
    T storage;

    [Export ("myMethod:")]
    public void MyMethod (NSObject value)
    {
    }
}
```

**Причина**: возможны неограниченные аргументы универсального типа, если они не участвуют в сигнатуре экспортированного элемента.

**Хорошо**:

```csharp
class Generic<T, U> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
        Console.WriteLine (typeof (U));
    }
}
```

**Причина**: параметр `T` в экспортированном элементе `MyMethod` Objective-C ограничен до `NSObject`, неограниченный тип `U` не является частью сигнатуры.

### <a name="instantiations-of-generic-types-from-objective-c"></a>Создание экземпляров универсальных типов из Objective-C

Создание экземпляров универсальных типов из Objective-C не допускается. Обычно это происходит, когда управляемый тип используется в xib или storyboard.

Рассмотрим это определение класса, которое предоставляет конструктор, принимающий `IntPtr` (способ создания объекта C# в Xamarin.iOS из собственного экземпляра Objective-C):

```csharp
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}
```

Хотя приведенная выше конструкция хороша, во время выполнения, когда Objective-C попытается создать его экземпляр, возникнет исключение.

Это происходит потому, что Objective-C не имеет концепции универсальных типов и не может указывать точный универсальный тип для создания.

Эту проблему можно устранить, создав специализированный подкласс универсального типа. Пример:

```csharp
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}

class GenericUIView : Generic<UIView>
{
}
```

В таком случае неоднозначности больше не будет, а класс `GenericUIView` можно использовать в xib или storyboard.

## <a name="no-support-for-generic-methods"></a>Поддержки универсальных методов нет

### <a name="generic-methods-are-not-allowed"></a>Универсальные методы недопустимы.

Этот код не будет компилироваться:

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyMethod<T> (T argument)
    {
    }
}
```

**Причина**: Он недопустим, поскольку Xamarin.iOS не знает, какой тип использовать для аргумента типа `T` при вызове метода из Objective-C.

Решением здесь является создание специализированного метода и его экспорт:

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyUIViewMethod (UIView argument)
    {
        MyMethod<UIView> (argument);
    }
    public void MyMethod<T> (T argument)
    {
    }
}
```

### <a name="no-exported-static-members-allowed"></a>Экспортированные статические элементы недопустимы

Статический элемент невозможно передать в Objective-C, если он содержится в универсальном подклассе `NSObject`.

Пример неподдерживаемого сценария:

```csharp
class Generic<T> : NSObject where T : NSObject
{
    [Export ("myMethod:")]
    public static void MyMethod ()
    {
    }

    [Export ("myProperty")]
    public static T MyProperty { get; set; }
}
```

**Причина**: Как и универсальные методы, среда выполнения Xamarin.iOS должна иметь возможность узнать, какой тип использовать для аргумента универсального типа `T`.

Для элементов экземпляра используется сам экземпляр (поскольку это никогда не будет экземпляр `Generic<T>`, это всегда будет `Generic<SomeSpecificClass>`), но для статических элементов эти сведения отсутствуют.

Обратите внимание, что это справедливо, даже если рассматриваемый элемент не использует аргумент типа `T` каким бы то ни было способом.

Альтернативой в этом случае является создание специализированного подкласса:

```csharp
class GenericUIView : Generic<UIView>
{
    [Export ("myUIViewMethod")]
    public static void MyUIViewMethod ()
    {
        MyMethod ();
    }

    [Export ("myProperty")]
    public static UIView MyUIViewProperty {
        get { return MyProperty; }
        set { MyProperty = value; }
    }
}

class Generic<T> : NSObject where T : NSObject
{
    public static void MyMethod () {}
    public static T MyProperty { get; set; }
}
```

## <a name="performance"></a>Производительность

Статический регистратор не может разрешить экспортируемый элемент в универсальном типе во время сборки, как он обычно делает, он должен делать это во время выполнения. Это означает, что вызов такого метода из Objective-C немного медленнее, чем вызов элементов из неуниверсальных классов.
