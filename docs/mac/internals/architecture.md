---
title: Архитектура Xamarin. Mac
description: В этом пошаговом руководством рассматривается Xamarin. Mac и его отношение к цели-C на низком уровне. В нем объясняются такие концепции, как компиляция, селекторы, регистраторы, запуск приложений и генератор.
ms.prod: xamarin
ms.assetid: 74D1FF57-4F2A-4646-8669-003DE99671D4
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 04/12/2017
ms.openlocfilehash: 51900adb1dd15675e584671f3b06ad6d7572f47d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73017557"
---
# <a name="xamarinmac-architecture"></a>Архитектура Xamarin. Mac

_В этом пошаговом руководством рассматривается Xamarin. Mac и его отношение к цели-C на низком уровне. В нем объясняются такие концепции, как компиляция, селекторы, регистраторы, запуск приложений и генератор._

## <a name="overview"></a>Обзор

Приложения Xamarin. Mac выполняются в среде выполнения Mono и используют компилятор Xamarin для компиляции в промежуточный язык (IL), который затем JIT-компиляция выполняется в машинный код во время выполнения. Это выполняется параллельно с исполняющей средой цели-C. Обе среды выполнения работают поверх ядра, похожего на UNIX, в частности КСНУ, и предоставляют доступ к различным интерфейсам API для пользовательского кода, что позволяет разработчикам обращаться к базовой машинной или управляемой системе.

На схеме ниже показан базовый обзор этой архитектуры.

[![Схема, показывающая базовый обзор архитектуры](architecture-images/mac-arch.png "Схема, показывающая базовый обзор архитектуры")](architecture-images/mac-arch-large.png#lightbox)

### <a name="native-and-managed-code"></a>Машинный и управляемый код

При разработке для Xamarin часто используются термины *машинный* и *управляемый* код. Управляемый код — это код, выполнение которого осуществляется с помощью .NET Framework среды CLR или в случае Xamarin: среда выполнения Mono.

Машинный код — это код, который будет выполняться в собственном коде на конкретной платформе (например, в коде цели-C или даже скомпилированного кода AOT в микросхеме ARM). В этом руководство рассматривается Компиляция управляемого кода в машинный код и объясняется, как работает приложение Xamarin. Mac, что позволяет полностью использовать API макинтош Apple с помощью привязок, а также доступ к. BCL и сложный язык, например C#.

## <a name="requirements"></a>Требования

Для разработки приложения macOS с помощью Xamarin.Mac macOS требуется следующее:

- Компьютер Mac, на котором работает macOS Sierra (10,12) или более поздней версии.
- Последняя версия Xcode (устанавливается из [App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12))
- Последняя версия Xamarin. Mac и Visual Studio для Mac

Для запуска приложений Mac, созданных с помощью Xamarin.Mac, необходимо выполнить следующие системные требования:

- Компьютер Mac с Mac OS X 10,7 или более поздней версии.

## <a name="compilation"></a>Компиляция

При компиляции любого приложения платформы Xamarin (или) будет C# запущен Компилятор F#Mono (или), который будет компилировать C# код F# и в промежуточный язык Майкрософт (MSIL или IL). После этого Xamarin. Mac использует *JIT* -компилятор во время выполнения для компиляции машинного кода, обеспечивая при необходимости выполнение на правильной архитектуре.

Это отличается от Xamarin. iOS, в котором используется компиляция AOT. При использовании компилятора AOT все сборки и все методы внутри них компилируются во время сборки. При использовании JIT компиляция выполняется по требованию только для выполняемых методов.

В приложениях Xamarin. Mac моно обычно внедряется в набор приложений (и называется " **Embedded Mono**"). При использовании классического API Xamarin. Mac приложение может использовать только **System Mono**, однако это не поддерживается в Unified API. Системный моно означает Mono, установленный в операционной системе. При запуске приложения приложение Xamarin. Mac будет использовать его.

## <a name="selectors"></a>Селекторы

С помощью Xamarin у нас есть две отдельные экосистемы — .NET и Apple, которые нам нужно объединить, чтобы обеспечить плавность работы пользователей. Мы видели в разделе выше, как взаимодействуют две среды выполнения, и вы можете очень хорошо слышать термин "привязки", который позволяет использовать собственные API Mac в Xamarin. Привязки описаны подробно в [документации по привязке "цель-C](~/mac/platform/binding.md)". Теперь давайте посмотрим, как работает Xamarin. Mac.

Во-первых, должен существовать способ предоставления цели-C в C#, что делается с помощью селекторов. Селектор — это сообщение, которое отправляется объекту или классу. С помощью цели-C это осуществляется с помощью функций [objc_msgSend](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/index.html) . Дополнительные сведения об использовании селекторов см. в руководстве по средствам выбора для iOS [цели-C](~/ios/internals/objective-c-selectors.md) . Также должен быть способ предоставления управляемому коду цели-C, что усложняется из-за того, что цель-C не знает ничего о управляемом коде. Чтобы обойти это, мы используем [регистратор](~/mac/internals/registrar.md). Это подробно описано в следующем разделе.

## <a name="registrar"></a>регистратор

Как упоминалось выше, регистратор — это код, который предоставляет управляемому коду цель-C. Для этого создается список всех управляемых классов, производных от Нсобжект:

- Для всех классов, которые не упаковывают существующий класс цели-C, он создает новый класс цели-C с элементами цели-C, которые отражают все управляемые члены, имеющие атрибут `[Export]`.
- В реализациях для каждого члена цели – C код добавляется автоматически для вызова зеркального управляемого члена.

В псевдокоде ниже приведен пример того, как это делается.

**C#(управляемый код):**

```csharp
class MyViewController : UIViewController{
    [Export ("myFunc")]
    public void MyFunc ()
    {
    }
 }
 ```

**Цель-C (машинный код):**

```objc
@interface MyViewController : UIViewController
 - (void)myFunc;
@end 

@implementation MyViewController
- (void)myFunc {
    // Code to call the managed C# MyFunc method in MyViewController
}
@end
```

Управляемый код может содержать атрибуты, `[Register]` и `[Export]`, которые регистратор использует для того, чтобы убедиться, что объект должен быть предоставлен для цели-C. Атрибут [Register] используется для указания имени созданного класса цели-C в случае, если созданное по умолчанию имя не подходит. Все классы, производные от Нсобжект, автоматически регистрируются с помощью цели-C. Обязательный атрибут [Export] содержит строку, которая является селектором, используемым в созданном классе цели-C.

Существует два типа регистраторов, используемых в Xamarin. Mac — Dynamic и static:

- Динамические регистраторы — это регистратор по умолчанию для всех сборок Xamarin. Mac. Динамический регистратор выполняет регистрацию всех типов в сборке во время выполнения. Для этого используются функции, предоставляемые API среды выполнения цели-C. Таким образом, динамический регистратор имеет медленный запуск, но более быстрое время сборки. Собственные функции (обычно в C), называемые трамполинес, используются в качестве реализаций методов при использовании динамических регистраторов. Они отличаются в разных архитектурах.
- Статические регистраторы — статический регистратор создает код цели-C во время сборки, который затем компилируется в статическую библиотеку и связывается с исполняемым файлом. Это обеспечивает более быстрый запуск, но во время сборки занимает больше времени.

## <a name="application-launch"></a>Запуск приложения

Логика запуска Xamarin. Mac будет отличаться в зависимости от того, используется ли в качестве встроенного или системного Mono. Чтобы просмотреть код и шаги для запуска приложения Xamarin. Mac, см. файл [заголовка запуска](https://github.com/xamarin/xamarin-macios/blob/master/runtime/xamarin/launch.h) в общедоступном репозитории Xamarin-маЦиос.

## <a name="generator"></a>Generator

Xamarin. Mac содержит определения для каждого API Mac. Их можно просмотреть в [репозитории GitHub маЦиос](https://github.com/xamarin/xamarin-macios/tree/master/src). Эти определения содержат интерфейсы с атрибутами, а также все необходимые методы и свойства. Например, следующий код используется для определения NSBox в [пространстве имен AppKit](https://github.com/xamarin/xamarin-macios/blob/master/src/appkit.cs#L1465-L1526). Обратите внимание, что это интерфейс с несколькими методами и свойствами:

```csharp
[BaseType (typeof (NSView))]
public interface NSBox {

    …

    [Export ("borderRect")]
    CGRect BorderRect { get; }

    [Export ("titleRect")]
    CGRect TitleRect { get; }

    [Export ("titleCell")]
    NSObject TitleCell { get; }

    [Export ("sizeToFit")]
    void SizeToFit ();

    [Export ("contentViewMargins")]
    CGSize ContentViewMargins { get; set; }

    [Export ("setFrameFromContentFrame:")]
    void SetFrameFromContentFrame (CGRect contentFrame);

    …

}
```

Генератор, именуемый `bmac` в Xamarin. Mac, принимает эти файлы определения и использует средства .NET для их компиляции во временную сборку. Однако эта временная сборка непригодна для вызова кода цели-C. Затем генератор считывает временную сборку и создает C# код, который можно использовать во время выполнения. Вот почему, например, при добавлении случайного атрибута в файл Definition. CS он не будет отображаться в выводимом коде. Генератор не знает о нем, и поэтому `bmac` не знает, как искать его во временной сборке для вывода.

После создания Xamarin. Mac. dll упаковщик, `mmp`, будет объединять все компоненты вместе.

На высоком уровне он достигает этого, выполняя следующие задачи:

- Создайте структуру пакета приложений.
- Скопируйте в управляемые сборки.
- Если связывание включено, запустите управляемый компоновщик, чтобы оптимизировать сборки, удалив неиспользуемые части.
- Создание приложения запуска, связывание в коде средства запуска, о котором говорилось вместе с кодом регистратора, в статическом режиме.

Затем этот процесс выполняется как часть пользовательского процесса сборки, который компилирует пользовательский код в сборку, которая ссылается на Xamarin. Mac. dll и выполняется `mmp`, чтобы сделать ее пакетом.

Более подробные сведения о компоновщике и способах его использования см. в руководстве по [компоновщику](~/ios/deploy-test/linker.md) iOS.

## <a name="summary"></a>Сводка

В этом разделе мы рассмотрели компиляцию приложений Xamarin. Mac и изучить Xamarin. Mac и его связь с целью-C.
