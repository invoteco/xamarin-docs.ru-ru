---
title: Поддержка цели-C
description: В этом документе представлено описание поддержки цели-C в внедрении .NET. В нем рассматриваются автоматические подсчеты ссылок, NSString, протоколы, протоколы Нсобжект, исключения и многое другое.
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
author: conceptdev
ms.author: crdun
ms.date: 11/14/2017
ms.openlocfilehash: 8798e0acf7b1184c64c7012b2f724e2fa7d2c816
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292773"
---
# <a name="objective-c-support"></a>Поддержка цели-C

## <a name="specific-features"></a>Отдельные функции

Поколение цели-C имеет несколько специальных функций, которые стоит отметить.

### <a name="automatic-reference-counting"></a>Автоматический подсчет ссылок

Для вызова созданных привязок **необходимо** использовать автоматический подсчет ссылок (ARC). Проект с библиотекой, основанной на внедрении .NET, должен `-fobjc-arc`быть скомпилирован с помощью.

### <a name="nsstring-support"></a>Поддержка NSString

Интерфейсы API, `System.String` которые предоставляют типы, `NSString`преобразуются в. Это упрощает управление памятью, чем при работе `char*`с.

### <a name="protocols-support"></a>Поддержка протоколов

Управляемые интерфейсы преобразуются в протоколы цели-C, где `@required`все члены являются.

### <a name="nsobject-protocol-support"></a>Поддержка протокола Нсобжект

По умолчанию хэширование и равенство как .NET, так и среды выполнения цели-C предполагаются взаимозаменяемыми, так как они имеют одинаковую семантику.

При переопределении `Equals(Object)` управляемого типа `GetHashCode`или, как правило, это означает, что поведение по умолчанию (.NET) не было достаточным. Это означает, что поведение цели по умолчанию, скорее всего, не является достаточным.

В таких случаях генератор переопределяет [`isEqual:`](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc) метод и [`hash`](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc) свойство, определенные в [ `NSObject` протоколе](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc). Это позволяет прозрачно использовать пользовательскую управляемую реализацию из кода цели-C.

### <a name="exceptions-support"></a>Поддержка исключений

Передача `--nativeexception` в качестве аргумента `objcgen` позволяет преобразовывать управляемые исключения в исключения цели-C, которые могут быть перехвачены и обработаны. 

### <a name="comparison"></a>Сравнение

`IComparable` Управляемые типы, реализующие (или его универсальную версию `IComparable<T>`), создают `NSComparisonResult` удобные методы цели-C, которые `nil` возвращают и принимают аргумент. Это делает созданный API более понятным для разработчиков цели-C. Например:

```objc
- (NSComparisonResult)compare:(XAMComparableType * _Nullable)other;
```

### <a name="categories"></a>Категории

Методы управляемых расширений преобразуются в категории. Например, следующие методы расширения для `Collection`:

```csharp
public static class SomeExtensions {
    public static int CountNonNull (this Collection collection) { ... }
    public static int CountNull (this Collection collection) { ... }
}
```

создаст категорию цели-C следующим образом:

```objc
@interface Collection (SomeExtensions)

- (int)countNonNull;
- (int)countNull;

@end
```

Если один управляемый тип расширяет несколько типов, создается несколько категорий «цель-C».

### <a name="subscripting"></a>Индексация ;

Управляемые индексированные свойства преобразуются в индексы объектов. Например:

```csharp
public bool this[int index] {
    get { return c[index]; }
    set { c[index] = value; }
}
```

создаст цель-C примерно так:

```objc
- (id)objectAtIndexedSubscript:(int)idx;
- (void)setObject:(id)obj atIndexedSubscript:(int)idx;
```

который можно использовать с помощью синтаксиса индекса цели-C:

```objc
if ([intCollection [0] isEqual:@42])
    intCollection[0] = @13;
```

В зависимости от типа индексатора при необходимости будет создаваться индекс индексированного или ключевого знака.

Эта [статья](http://nshipster.com/object-subscripting/) является хорошим введением в подстрочность.

## <a name="main-differences-with-net"></a>Основные отличия от .NET

### <a name="constructors-vs-initializers"></a>Конструкторы VS инициализаторы

В цели-C можно вызвать любой из прототипов инициализатора любого из родительских классов в цепочке наследования, если он не помечен как недоступный (`NS_UNAVAILABLE`).

В C# необходимо явно объявить член конструктора внутри класса, что означает, что конструкторы не наследуются.

Чтобы предоставить доступ к нужному представлению C# API для цели-C `NS_UNAVAILABLE` , он добавляется в любой инициализатор, который отсутствует в дочернем классе из родительского класса.

C#API

```csharp
public class Unique {
    public Unique () : this (1)
    {
    }

    public Unique (int id)
    {
    }
}

public class SuperUnique : Unique {
    public SuperUnique () : base (911)
    {
    }
}
```

API с контактом цели-C:

```objc
@interface SuperUnique : Unique

- (instancetype)initWithId:(int)id NS_UNAVAILABLE;
- (instancetype)init;

@end
```

`initWithId:` Здесь помечена как недоступная.

### <a name="operator"></a>Оператор

Цель-C не поддерживает перегрузку операторов как C# есть, поэтому операторы преобразуются в селекторы классов:

```csharp
public static AllOperators operator + (AllOperators c1, AllOperators c2)
{
    return new AllOperators (c1.Value + c2.Value);
}
```

в

```objc
+ (instancetype)add:(Overloads_AllOperators *)anObjectC1 c2:(Overloads_AllOperators *)anObjectC2;
```

Однако некоторые языки .NET не поддерживают перегрузку операторов, поэтому часто в дополнение к перегрузке оператора можно включить [«дружественный»](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads) именованный метод.

Если обнаружена и версия оператора, и "дружественная" версия, будет создана только понятная версия, так как они будут формироваться с одним и тем же именем цели-C.

```csharp
public static AllOperatorsWithFriendly operator + (AllOperatorsWithFriendly c1, AllOperatorsWithFriendly c2)
{
    return new AllOperatorsWithFriendly (c1.Value + c2.Value);
}

public static AllOperatorsWithFriendly Add (AllOperatorsWithFriendly c1, AllOperatorsWithFriendly c2)
{
    return new AllOperatorsWithFriendly (c1.Value + c2.Value);
}
```

обретает

```objc
+ (instancetype)add:(Overloads_AllOperatorsWithFriendly *)anObjectC1 c2:(Overloads_AllOperatorsWithFriendly *)anObjectC2;
```

### <a name="equality-operator"></a>Оператор равенства

В операторе `==` General C# в обрабатывается как оператор общего типа, как указано выше.

Однако если найден "дружественный" оператор Equals, то в поколении `!=` оператор `==` и оператор будут пропущены.

### <a name="datetime-vs-nsdate"></a>Сравнение DateTime и Нсдате

[`NSDate`](https://developer.apple.com/reference/foundation/nsdate?language=objc) Из документации:

> `NSDate`объекты инкапсулируют один момент времени независимо от конкретной календрикал системы или часового пояса. Объекты даты являются неизменяемыми, представляющими инвариантный интервал времени относительно абсолютной ссылочной даты (00:00:00 UTC в 1 января 2001).

Из-за даты `DateTime` ссылкивсепреобразованиямеждунейидолжнывыполнятьсявформатеUTC.`NSDate`

#### <a name="datetime-to-nsdate"></a>DateTime в Нсдате

При преобразовании `DateTime` из `NSDate`в `Kind` в учитывается `DateTime` свойство в учетной записи:

|Тип|Результаты|
|---|---|
|`Utc`|Преобразование выполняется с помощью предоставленного `DateTime` объекта, как есть.|
|`Local`|Результат вызова `ToUniversalTime()` в предоставленном `DateTime` объекте используется для преобразования.|
|`Unspecified`|Предполагается `DateTime` , что предоставленный объект имеет формат UTC, поэтому такое `Kind` же `Utc`поведение имеет значение.|

Преобразование использует следующую формулу:

```
TimeInterval = DateTimeObjectTicks - NSDateReferenceDateTicks / TicksPerSecond
```

В этой формуле: 

- `NSDateReferenceDateTicks`вычисляется на основе `NSDate` даты ссылки 00:00:00 UTC в 1 января 2001: 

    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```

- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond)определено для[`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

Чтобы создать `NSDate` объект `TimeInterval` , используется с `NSDate` [датевистимеинтервалсинцереференцедате:](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc) Selector.

#### <a name="nsdate-to-datetime"></a>Нсдате до DateTime

Преобразование из `NSDate` в `DateTime` использует следующую формулу:

```
DateTimeTicks = NSDateTimeIntervalSinceReferenceDate * TicksPerSecond + NSDateReferenceDateTicks
```

В этой формуле: 

- `NSDateReferenceDateTicks`вычисляется на основе `NSDate` даты ссылки 00:00:00 UTC в 1 января 2001: 

    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```

- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond)определено для[`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

После вычисления `DateTimeTicks` вызывается [конструктор](https://docs.microsoft.com/dotnet/api/system.datetime.-ctor?#System_DateTime__ctor_System_Int64_System_DateTimeKind_) `DateTime` и устанавливается для него `kind` значение `DateTimeKind.Utc`.

> [!NOTE]
> `NSDate`может быть `nil`, `DateTime` но является структурой в .NET, которая по определению не может `null`быть. Если `nil` вы выдаете `NSDate`, он будет преобразован в значение по `DateTime` умолчанию, которое сопоставляется с `DateTime.MinValue`.

`NSDate`поддерживает большее максимальное и меньшее минимальное значение, чем `DateTime`. При преобразовании `NSDate` из `DateTime`в эти верхние и нижние `DateTime` значения меняются на [MaxValue](https://docs.microsoft.com/dotnet/api/system.datetime.maxvalue) или [MinValue](https://docs.microsoft.com/dotnet/api/system.datetime.minvalue)соответственно.
