---
title: Универсальные подклассы Нсобжект в Xamarin. iOS
description: В этом документе описывается создание универсальных подклассов Нсобжект. Он изучает, что может и не может быть сделано, обсуждает статический регистратор и рассматривает производительность.
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: 136efbd936bc39563c419a87ed48f6fc5436efa9
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768546"
---
# <a name="generic-subclasses-of-nsobject-in-xamarinios"></a>Универсальные подклассы Нсобжект в Xamarin. iOS

## <a name="using-generics-with-nsobjects"></a>Использование универсальных шаблонов с Нсобжектс

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

Поскольку объекты, `NSObject` регистрируемые в подклассе, зарегистрированы в среде выполнения цели-C, существуют некоторые ограничения на то, что возможно `NSObject` с универсальными подклассами типов.

## <a name="considerations-for-generic-subclasses-of-nsobject"></a>Рекомендации для универсальных подклассов Нсобжект

В этом документе описаны ограничения ограниченной поддержки универсальных подклассов `NSObjects`.

### <a name="generic-type-arguments-in-member-signatures"></a>Аргументы универсального типа в сигнатурах членов

Все аргументы универсального типа в сигнатуре члена, доступного для цели-C `NSObject` , должны иметь ограничение.

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

**Причина**: Параметр универсального типа — `NSObject`это, поэтому сигнатура селектора для `myMethod:` может быть безопасно предоставлена цели-C `NSObject` (она всегда будет или является подклассом).

**Плохое**:

```csharp
class Generic<T> : NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**Причина**: невозможно создать подпись цели-c для экспортируемых элементов, которые может вызывать целевой код цели-c, так как сигнатура будет отличаться в зависимости от конкретного типа универсального типа `T`.

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

**Причина**: возможно наличие неограниченных аргументов универсального типа, если они не участвуют в сигнатуре экспортированного элемента.

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

**Причина**: `T` параметр в экспортированном `MyMethod` целевом приложении `NSObject`-C имеет ограничение, но неограниченный тип `U` не является частью сигнатуры.

### <a name="instantiations-of-generic-types-from-objective-c"></a>Создание экземпляров универсальных типов из цели-C

Создание экземпляра универсальных типов из цели-C не допускается. Обычно это происходит, когда управляемый тип используется в XIB или раскадровки.

Рассмотрим это определение класса, которое предоставляет конструктор, принимающий `IntPtr` C# объект (способ создания объекта с помощью Xamarin. IOS) из машинного экземпляра с целевым экземпляром-C):

```csharp
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}
```

Хотя приведенная выше конструкция прекрасно подходит, во время выполнения это вызовет исключение, когда цель-C пытается создать его экземпляр.

Это происходит потому, что цель-C не имеет концепции универсальных типов и не может указывать точный универсальный тип для создания.

Эту проблему можно устранить, создав специализированный подкласс универсального типа. Например:

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

Теперь неоднозначность больше не существует, класс `GenericUIView` можно использовать в XIB или раскадровки.

## <a name="no-support-for-generic-methods"></a>Универсальные методы не поддерживаются

### <a name="generic-methods-are-not-allowed"></a>Универсальные методы не допускаются.

Следующий код не будет компилироваться:

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyMethod<T> (T argument)
    {
    }
}
```

**Причина**: Это недопустимо, поскольку Xamarin. iOS не знает, какой тип использовать для аргумента `T` типа при вызове метода из цели-C.

Альтернативой является создание специализированного метода и экспорт, который вместо этого:

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

### <a name="no-exported-static-members-allowed"></a>Нет разрешенных экспортированных статических членов

Нельзя предоставлять статические члены цели, если она размещена внутри универсального подкласса `NSObject`.

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

**Оправдан** Как и универсальные методы, среда выполнения Xamarin. iOS должна иметь возможность узнать, какой тип использовать для аргумента `T`универсального типа.

Для членов экземпляра используется сам экземпляр (поскольку он никогда не будет экземпляром `Generic<T>`, он `Generic<SomeSpecificClass>`всегда будет), но для статических членов эта информация отсутствует.

Обратите внимание, что это применимо, даже если рассматриваемый элемент не использует `T` аргумент типа каким бы то ни было образом.

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

Статический регистратор не может разрешить экспортируемый элемент в универсальном типе во время сборки, так как он обычно имеет значение, он должен быть просмотрен во время выполнения. Это означает, что вызов такого метода из цели-C немного медленнее, чем вызов членов из неуниверсальных классов.
