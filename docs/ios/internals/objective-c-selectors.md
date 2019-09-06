---
title: Селекторы цели-C в Xamarin. iOS
description: В этом документе рассматривается взаимодействие с селекторами цели-C из C#. Здесь описывается, как вызывать селекторы и технические рекомендации, которые необходимо учитывать при этом.
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/12/2017
ms.openlocfilehash: 17b845345175d80237bcfdb171461f2c763c364e
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291853"
---
# <a name="objective-c-selectors-in-xamarinios"></a>Селекторы цели-C в Xamarin. iOS

Язык цели-C основан на *селекторах*. Селектор — это сообщение, которое может быть отправлено объекту или *классу*. [Xamarin. iOS](~/ios/internals/api-design/index.md) сопоставляет селекторы экземпляров с методами экземпляра, а селекторы классов — со статическими методами.

В отличие от обычных функций C (и C++ аналогичных функций-членов) нельзя напрямую вызывать селектор с помощью [P/Invoke](https://www.mono-project.com/docs/advanced/pinvoke/) , методы выбора отправляются в класс или экземпляр класса цели-C с помощью метода[`objc_msgSend`](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend)
функций.

Дополнительные сведения о сообщениях в цели-C см. в разделе " [Работа с объектами](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithObjects/WorkingwithObjects.html#//apple_ref/doc/uid/TP40011210-CH4-SW2) " в Apple.

## <a name="example"></a>Пример

Предположим, что нужно вызвать метод[`sizeWithFont:forWidth:lineBreakMode:`](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont)
переключатель в [`NSString`](https://developer.apple.com/documentation/foundation/nsstring).
Объявление (из документации Apple):

```objc
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

Этот API имеет следующие характеристики.

- Тип возвращаемого значения `CGSize` — для Unified API.
- Параметр является [уифонт](xref:UIKit.UIFont) (и типом (косвенным), производным от [нсобжект](xref:Foundation.NSObject), и сопоставляется с [System. IntPtr.](xref:System.IntPtr) `font`
- Параметр — сопоставляется с `nfloat`. `width` `CGFloat`
- `lineBreakMode` [Параметр`UILineBreakMode`](https://developer.apple.com/documentation/uikit/uilinebreakmode?language=objc), уже привязан в Xamarin. iOS в качестве[`UILineBreakMode`](xref:UIKit.UILineBreakMode)
цело.

Поместив все вместе, `objc_msgSend` объявление должно соответствовать следующим условиям:

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

Если возвращенное значение было структурой размером менее 8 байт (например, `SizeF` ранее использовался перед переключением на унифицированные API), приведенный выше код будет выполнен в симуляторе, но на устройстве произошел сбой. Чтобы вызвать селектор, возвращающий значение размером менее 8 бит, объявите `objc_msgSend_stret` функцию:

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

Целью селектора является либо экземпляр объекта, либо класс цели-C. Если целевой объект является экземпляром и получен из связанного типа Xamarin. iOS, используйте [`ObjCRuntime.INativeObject.Handle`](xref:ObjCRuntime.INativeObject.Handle) свойство.

Если целевой объект является классом, используйте [`ObjCRuntime.Class`](xref:ObjCRuntime.Class) для получения ссылки на экземпляр класса, а затем [`Class.Handle`](xref:ObjCRuntime.Class.Handle) используйте свойство.

### <a name="selector-names"></a>Имена селекторов

Имена селекторов перечислены в документации Apple. Например, [`NSString`](https://developer.apple.com/documentation/foundation/nsstring?language=objc) включает [`sizeWithFont:`](https://developer.apple.com/documentation/foundation/nsstring/1619917-sizewithfont?language=objc) и [`sizeWithFont:forWidth:lineBreakMode:`](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont?language=objc) селекторы. Встроенные и замыкающие двоеточия являются частью имени селектора и не могут быть опущены.

После получения имени селектора можно создать [`ObjCRuntime.Selector`](xref:ObjCRuntime.Selector) для него экземпляр.

### <a name="calling-objc_msgsend"></a>Вызов objc_msgSend

`objc_msgSend`отправляет сообщение (Selector) в объект. Это семейство функций принимает по крайней мере два обязательных аргумента: целевой объект селектора (экземпляр или обработчик класса), сам селектор и все аргументы, необходимые для селектора. Аргументы экземпляра и селектора должны быть `System.IntPtr`, а все остальные аргументы должны соответствовать типу, ожидаемому селектором, `nint` например `System.IntPtr` для `int`типа или для всех `NSObject`типов, производных от. Используйте[`NSObject.Handle`](xref:Foundation.NSObject.Handle)
свойство для получения `IntPtr` экземпляра типа цели-C.

Существует более одной `objc_msgSend` функции:

- Используется [`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc) для селекторов, возвращающих структуру. В ARM сюда входят все типы возвращаемых значений, не являющиеся перечислениями, или любые встроенные типы C (`char`, `short`, `int`, `long` `float`,, `double`). В архитектуре x86 (симулятор) Этот метод необходимо использовать для всех структур, размер которых превышает 8 байт (`CGSize` составляет 8 байт и не используется `objc_msgSend_stret` в симуляторе). 
- Используется [`objc_msgSend_fpret`](https://developer.apple.com/documentation/objectivec/1456697-objc_msgsend_fpret?language=objc) для селекторов, возвращающих значение с плавающей запятой только на платформе x86. Эту функцию не нужно использовать в ARM; Вместо этого используйте `objc_msgSend`. 
- Функция Main [objc_msgSend](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend) используется для всех остальных селекторов.

После того как вы решили `objc_msgSend` , какие функции нужно вызвать (симулятор и устройство могут требовать другого метода), можно использовать стандартный [`[DllImport]`](xref:System.Runtime.InteropServices.DllImportAttribute) метод, чтобы объявить функцию для последующего вызова.

Набор предварительно подготовленных `objc_msgSend` объявлений можно найти в `ObjCRuntime.Messaging`.

## <a name="different-invocations-on-simulator-and-device"></a>Разные вызовы в симуляторе и устройстве

Как описано выше, цель-C имеет три вида `objc_msgSend` методов: один для обычных вызовов, один для вызовов, возвращающих значения с плавающей запятой (только x86), и один для вызовов, возвращающих значения структуры. Последний включает суффикс `_stret` в `ObjCRuntime.Messaging`.

При вызове метода, который будет возвращать определенные структуры (описанные ниже правила), необходимо вызвать метод с возвращаемым значением в качестве первого параметра в качестве `out` значения:

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

Правило, когда следует использовать метод, `_stret_` отличается в архитектурах x86 и ARM.
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

При создании для ARM используйте[`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)
для любого типа значения, который не является перечислением или любым из`int`базовых типов перечисления (, `byte`, `short`, `long` `double`,, `float`).

При построении для x86 используйте[`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)
для любого типа значения, который не является перечислением или любым из`int`базовых типов перечисления (, `byte`, `short`, `long` `double`,, `float`), чей собственный размер больше 8 байт.

### <a name="creating-your-own-signatures"></a>Создание собственных подписей

Для создания собственных подписей, если это необходимо, можно [использовать следующее.](https://gist.github.com/rolfbjarne/981b778a99425a6e630c)

## <a name="related-links"></a>Связанные ссылки

- Образец [селекторов "цель-C](https://developer.xamarin.com/samples/mac-ios/Objective-C/) "
