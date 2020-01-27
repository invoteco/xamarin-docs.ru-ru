---
title: Селекторы цели-C в Xamarin. iOS
description: В этом документе рассматривается взаимодействие с селекторами цели-C из C#. Здесь описывается, как вызывать селекторы и технические рекомендации, которые необходимо учитывать при этом.
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/12/2017
ms.openlocfilehash: 2a4d255500f68497fe7cb0cc439c5f9c0504b0f2
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725189"
---
# <a name="objective-c-selectors-in-xamarinios"></a>Селекторы цели-C в Xamarin. iOS

Язык цели-C основан на *селекторах*. Селектор — это сообщение, которое может быть отправлено объекту или *классу*. [Xamarin. iOS](~/ios/internals/api-design/index.md) сопоставляет селекторы экземпляров с методами экземпляра, а селекторы классов — со статическими методами.

В отличие от обычных функций C (и C++ аналогичных функций-членов) нельзя напрямую вызывать селектор с помощью [P/Invoke](https://www.mono-project.com/docs/advanced/pinvoke/) , методы выбора отправляются в класс или экземпляр класса цели-C с помощью [`objc_msgSend`](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend)
Функция .

Дополнительные сведения о сообщениях в цели-C см. в разделе " [Работа с объектами](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithObjects/WorkingwithObjects.html#//apple_ref/doc/uid/TP40011210-CH4-SW2) " в Apple.

## <a name="example"></a>Пример

Предположим, вы хотите вызвать [`sizeWithFont:forWidth:lineBreakMode:`](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont)
селектор на [`NSString`](https://developer.apple.com/documentation/foundation/nsstring).
Объявление (из документации Apple):

```objc
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

Этот API имеет следующие характеристики.

- Тип возвращаемого значения `CGSize` для Unified API.
- Параметр `font` является [уифонт](xref:UIKit.UIFont) (и типом (косвенно), производным от [нсобжект](xref:Foundation.NSObject), и сопоставляется с [System. IntPtr](xref:System.IntPtr).
- Параметр `width`, `CGFloat`, сопоставляется с `nfloat`.
- Параметр `lineBreakMode`, [`UILineBreakMode`](https://developer.apple.com/documentation/uikit/uilinebreakmode?language=objc), уже привязан в Xamarin. iOS в качестве [`UILineBreakMode`](xref:UIKit.UILineBreakMode)
цело.

Поместив все вместе, объявление `objc_msgSend` должно совпадать:

```csharp
CGSize objc_msgSend(
    IntPtr target,
    IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode
);
```

Объявите его следующим образом:

```csharp
[DllImport (Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend")]
static extern CGSize cgsize_objc_msgSend_IntPtr_float_int (
    IntPtr target,
    IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode
);
```

Чтобы вызвать этот метод, используйте следующий код:

```csharp
NSString target = ...
Selector selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont font = ...
nfloat width = ...
UILineBreakMode mode = ...

CGSize size = cgsize_objc_msgSend_IntPtr_float_int(
    target.Handle,
    selector.Handle,
    font == null ? IntPtr.Zero : font.Handle,
    width,
    mode
);
```

Если возвращенное значение было структурой размером менее 8 байт (например, старый `SizeF`, используемый перед переключением на унифицированные API-интерфейсы), приведенный выше код будет выполнен в симуляторе, но на устройстве произошел сбой. Чтобы вызвать селектор, возвращающий значение размером менее 8 бит, объявите функцию `objc_msgSend_stret`:

```csharp
[DllImport (MonoTouch.Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend_stret")]
static extern void cgsize_objc_msgSend_stret_IntPtr_float_int (
    out CGSize retval,
    IntPtr target,
    IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode
);
```

Чтобы вызвать этот метод, используйте следующий код:

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size;

if (Runtime.Arch == Arch.SIMULATOR)
    size = cgsize_objc_msgSend_IntPtr_float_int(
        target.Handle,
        selector.Handle,
        font == null ? IntPtr.Zero : font.Handle,
        width,
        mode
    );
else
    cgsize_objc_msgSend_stret_IntPtr_float_int(
        out size,
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero: font.Handle,
        width,
        mode
    );
```

## <a name="invoking-a-selector"></a>Вызов селектора

Вызов селектора включает три шага:

1. Получение целевого объекта Selector.
2. Получите имя селектора.
3. Вызовите `objc_msgSend` с соответствующими аргументами.

### <a name="selector-targets"></a>Целевые объекты селектора

Целью селектора является либо экземпляр объекта, либо класс цели-C. Если целевой объект является экземпляром и поступил из связанного типа Xamarin. iOS, используйте свойство [`ObjCRuntime.INativeObject.Handle`](xref:ObjCRuntime.INativeObject.Handle) .

Если целевой объект является классом, используйте [`ObjCRuntime.Class`](xref:ObjCRuntime.Class) , чтобы получить ссылку на экземпляр класса, а затем используйте свойство [`Class.Handle`](xref:ObjCRuntime.Class.Handle) .

### <a name="selector-names"></a>Имена селекторов

Имена селекторов перечислены в документации Apple. Например, [`NSString`](https://developer.apple.com/documentation/foundation/nsstring?language=objc) включает селекторы [`sizeWithFont:`](https://developer.apple.com/documentation/foundation/nsstring/1619917-sizewithfont?language=objc) и [`sizeWithFont:forWidth:lineBreakMode:`](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont?language=objc) . Встроенные и замыкающие двоеточия являются частью имени селектора и не могут быть опущены.

После получения имени селектора можно создать для него экземпляр [`ObjCRuntime.Selector`](xref:ObjCRuntime.Selector) .

### <a name="calling-objc_msgsend"></a>Вызов objc_msgSend

`objc_msgSend` отправляет сообщение (Selector) в объект. Это семейство функций принимает по крайней мере два обязательных аргумента: целевой объект селектора (экземпляр или обработчик класса), сам селектор и все аргументы, необходимые для селектора. Аргументы экземпляра и селектора должны быть `System.IntPtr`, а все остальные аргументы должны соответствовать типу, ожидаемому селектором, например `nint` для `int`или `System.IntPtr` для всех типов, производных от `NSObject`. Использование [`NSObject.Handle`](xref:Foundation.NSObject.Handle)
свойство, чтобы получить `IntPtr` для экземпляра типа цели-C.

Существует более одной функции `objc_msgSend`:

- Используйте [`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc) для селекторов, возвращающих структуру. В ARM сюда входят все типы возвращаемых значений, не являющиеся перечислениями, или любые встроенные типы C (`char`, `short`, `int`, `long`, `float`, `double`). В архитектуре x86 (симулятор) Этот метод необходимо использовать для всех структур, размер которых превышает 8 байт (`CGSize` составляет 8 байт и не использует `objc_msgSend_stret` в симуляторе).
- Используйте [`objc_msgSend_fpret`](https://developer.apple.com/documentation/objectivec/1456697-objc_msgsend_fpret?language=objc) для селекторов, возвращающих значение с плавающей запятой только на платформе x86. Эту функцию не нужно использовать в ARM; Вместо этого используйте `objc_msgSend`.
- Функция Main [objc_msgSend](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend) используется для всех остальных селекторов.

После того как вы решили, какие `objc_msgSend` функции нужно вызвать (симулятор и устройство могут требовать другого метода), можно использовать стандартный метод [`[DllImport]`](xref:System.Runtime.InteropServices.DllImportAttribute) , чтобы объявить функцию для последующего вызова.

Набор предопределенных объявлений `objc_msgSend` можно найти в `ObjCRuntime.Messaging`.

## <a name="different-invocations-on-simulator-and-device"></a>Разные вызовы в симуляторе и устройстве

Как описано выше, цель-C имеет три вида методов `objc_msgSend`: один для обычных вызовов, один для вызовов, возвращающих значения с плавающей запятой (только x86), а второй — для вызовов, возвращающих значения структуры. Последний включает суффикс, `_stret` в `ObjCRuntime.Messaging`.

При вызове метода, который будет возвращать определенные структуры (описанные ниже правила), необходимо вызвать метод с возвращаемым значением в качестве первого параметра в качестве значения `out`:

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

Правило, когда следует использовать метод `_stret_`, отличается в архитектурах x86 и ARM.
Если вы хотите, чтобы привязки работали как на симуляторе, так и на устройстве, добавьте следующий код:

```csharp
if (Runtime.Arch == Arch.DEVICE)
{
    PointF ret;
    Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, myHandle, selector.Handle);
    return ret;
}
else
{
    return Messaging.PointF_objc_msgSend_PointF_IntPtr (myHandle, selector.Handle);
}
```

### <a name="using-the-objc_msgsend_stret-method"></a>Использование метода objc_msgSend_stret

При создании для ARM используйте [`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)
для любого типа значения, который не является перечислением или любым из базовых типов перечисления (`int`, `byte`, `short`, `long`, `double`, `float`).

При построении для x86 используйте [`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)
для любого типа значения, который не является перечислением или любым из базовых типов перечисления (`int`, `byte`, `short`, `long`, `double`, `float`), а собственный размер превышает 8 байт.

### <a name="creating-your-own-signatures"></a>Создание собственных подписей

Для создания собственных подписей, если это необходимо, можно [использовать следующее.](https://gist.github.com/rolfbjarne/981b778a99425a6e630c)
