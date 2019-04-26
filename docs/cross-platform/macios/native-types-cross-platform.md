---
title: Работа с собственными типами в кроссплатформенных приложениях
description: В этой статье рассматривается использование нового iOS Unified API, собственные типы (nint, nuint, nfloat) в приложении кросс платформенных, где код используется совместно с устройства без iOS, например, Android или ОС Windows Phone.
ms.prod: xamarin
ms.assetid: B9C56C3B-E196-4ADA-A1DE-AC10D1001C2A
author: asb3993
ms.author: amburns
ms.date: 04/07/2016
ms.openlocfilehash: 489d2a76e6eff661360b24d1872ed1343c74b85e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61261185"
---
# <a name="working-with-native-types-in-cross-platform-apps"></a>Работа с собственными типами в кроссплатформенных приложениях

_В этой статье рассматривается использование нового iOS Unified API, собственные типы (nint, nuint, nfloat) в приложении кросс платформенных, где код используется совместно с устройства без iOS, например, Android или ОС Windows Phone._


Собственные типы 64 типы работать с iOS и Mac API-интерфейсы. Если вы создаете общий код, который тоже работает на платформе Android или Windows, нужно будет управлять преобразованием типов объединенных в регулярные типы .NET, которые можно совместно использовать.

В этом документе описаны различные возможности для взаимодействия с на единый API из кода общего/общее.

## <a name="when-to-use-the-native-types"></a>Когда следует использовать собственные типы

Xamarin.iOS и Xamarin.Mac унифицированными API по-прежнему включать `int`, `uint` и `float` типы данных, а также `RectangleF`, `SizeF` и `PointF` типов. Эти существующие типы данных должны продолжать использовать в любой общей, кросс платформенного кода. Новые типы данных в собственном формате следует использовать только в том случае, при вызовах к API iOS или Mac где поддержка архитектуры с поддержкой типы являются обязательными.

В зависимости от кода, общего доступа, может возникнуть где кросс платформенного кода может понадобиться для работы с `nint`, `nuint` и `nfloat` типов данных. Например: это библиотека, которая обрабатывает преобразований данные прямоугольника, который ранее использовал `System.Drawing.RectangleF` для совместного использования функциональных возможностей между версиями Xamarin.iOS и Xamarin.Android приложения, потребуется обновить, чтобы обрабатывать собственные типы в iOS.

Способ обработки этих изменений зависит от размера и сложности приложения и форме, совместно использовать код используется, как мы увидим в инструкции в следующих разделах.

## <a name="code-sharing-considerations"></a>Совместное использование вопросы кода

Как уже говорилось в [варианты совместного использования кода](~/cross-platform/app-fundamentals/code-sharing.md) документа, существует два основных способа для совместного использования кода между проектами платформ: Общий доступ к проектам и переносимых библиотек классов. Из двух типов применялось, ограничит параметры, которые у нас есть при обработке собственных типов данных в кросс платформенного кода.

### <a name="portable-class-library-projects"></a>Переносимые библиотеки классов

Переносимые библиотеки классов (PCL) позволяет для различных платформ, которые вы хотите поддерживать и использовать интерфейсы для предоставления функциональных возможностей платформы.

Так как тип проекта переносимой библиотеки Классов компилируется вниз до `.DLL` и в нем нет смысла на единый API, вы будете вынуждены продолжать использовать существующие типы данных (`int`, `uint`, `float`) в PCL исходный код и тип приведения вызовы PCL классы и методы в клиентские приложения. Пример.

```csharp
using NativePCL;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

### <a name="shared-projects"></a>Общие проекты

Тип общего ресурса проекта позволяет организовать исходный код в отдельном проекте, затем получает включается и компилируется в отдельные платформы внешние приложения и использовать `#if` директивы компилятора, когда возникает необходимость управлять ими требования к конкретной платформы.

Размер и сложность передней завершить мобильных приложений, которые используют общий код, а также размер и сложность кода, общего доступа, необходимо принимать во внимание при выборе метода поддержки собственных данных типов кроссплатформенной с Тип общего ресурса проекта.

Исходя из этих факторов, следующие типы решений могут быть реализованы с помощью `if __UNIFIED__ ... #endif` директивы компилятора для обработки единый API конкретные изменения в код.

#### <a name="using-duplicate-methods"></a>С помощью повторяющихся методов

Рассмотрим в качестве примера библиотеки, осуществляющей преобразований данные прямоугольника, приведенном выше. Если библиотека содержит только один или два очень простые методы, можно выбрать Создание повторяющихся версии этих методов для Xamarin.iOS и Xamarin.Android. Пример:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
using CoreGraphics;
#endif

namespace NativeShared
{
    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        #if __UNIFIED__
            public static nfloat CalculateArea(CGRect rect) {

                // Calculate area...
                return (rect.Width * rect.Height);

            }
        #else
            public static float CalculateArea(RectangleF rect) {

                // Calculate area...
                return (rect.Width * rect.Height);

            }
        #endif
        #endregion
    }
}
```

В приведенном выше коде так как `CalculateArea` процедура очень проста, мы использовали условной компиляции и создать отдельные, Unified API версии метода. С другой стороны Если эта библиотека содержит многие подпрограммы или несколько сложных подпрограммы, это решение не будет возможно, так как он будет представлять проблемы синхронизации все методы для изменения или исправления ошибок.

#### <a name="using-method-overloads"></a>С помощью метода перегрузки

В этом случае решение может быть для создания версии перегрузка методов, с помощью типов данных 32-разрядная версия, благодаря чему они теперь использовать `CGRect` как параметр или возвращаемое значение, преобразовать это значение к `RectangleF` (зная, что преобразование из `nfloat` для `float` с потерей данных преобразования) и вызвать исходную версию подпрограммы для выполняют фактическую работу. Пример:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
using CoreGraphics;
#endif

namespace NativeShared
{
    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        #if __UNIFIED__
            public static nfloat CalculateArea(CGRect rect) {

            // Call original routine to calculate area
            return (nfloat)CalculateArea((RectangleF)rect);

            }
        #endif

        public static float CalculateArea(RectangleF rect) {

            // Calculate area...
            return (rect.Width * rect.Height);

        }

        #endregion
    }
}

```

Опять же это является хорошим решением до тех пор, пока потери точности не влияет на результаты для определенных потребностей вашего приложения.

#### <a name="using-alias-directives"></a>Директивы псевдонима using

Для областей, где проблема потери точности, другим возможным решением является использование `using` директивы, чтобы создать псевдоним для машинного кода и CoreGraphics типов данных, добавив следующий код в верхнюю часть файлов общего исходного кода и преобразование любой требуется `int`, `uint` или `float` значения `nint`, `nuint` или `nfloat`:

```csharp
#if __UNIFIED__
    // Mappings Unified CoreGraphic classes to MonoTouch classes
    using RectangleF = global::CoreGraphics.CGRect;
    using SizeF = global::CoreGraphics.CGSize;
    using PointF = global::CoreGraphics.CGPoint;
#else
    // Mappings Unified types to MonoTouch types
    using nfloat = global::System.Single;
    using nint = global::System.Int32;
    using nuint = global::System.UInt32;
#endif
```

Таким образом, чтобы наш пример код станет таким:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
    // Mappings Unified CoreGraphic classes to MonoTouch classes
    using RectangleF = global::CoreGraphics.CGRect;
    using SizeF = global::CoreGraphics.CGSize;
    using PointF = global::CoreGraphics.CGPoint;
#else
    // Mappings Unified types to MonoTouch types
    using nfloat = global::System.Single;
    using nint = global::System.Int32;
    using nuint = global::System.UInt32;
#endif

namespace NativeShared
{

    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        public static nfloat CalculateArea(RectangleF rect) {

            // Calculate area...
            return (rect.Width * rect.Height);

        }
        #endregion
    }
}
```

Обратите внимание, что мы изменили `CalculateArea` метод возвращает `nfloat` вместо стандартного `float`. Это было сделано, то не получили бы ошибку компиляции, попытка _неявно_ преобразовать `nfloat` результат наших вычислений (так как оба значения умножаемое `nfloat`) в `float` возвращаемое значение.

Если код скомпилирован и запущен на устройстве не единый API, `using nfloat = global::System.Single;` сопоставляет `nfloat` для `Single` которого выполняет неявное преобразование к `float` принимающего приложения переднего плана для вызова `CalculateArea` метод без изменение.


#### <a name="using-type-conversions-in-the-front-end-app"></a>С помощью преобразования типов в приложении переднего плана

В случае, если внешний интерфейс приложения только внести небольшое число вызовов в библиотеку общего кода, другим решением может быть оставить без изменений библиотеки и приведение типов в приложении Xamarin.iOS и Xamarin.Mac при вызове существующей процедуры. Пример.

```csharp
using NativeShared;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

Если потребляющее приложение предоставляет сотни вызовами к библиотеке общего кода, это опять же, может оказаться хорошим решением.

На основе нашего приложения архитектуры, мы может оказаться, используя один или несколько из приведенного выше решений для поддержки собственных данных типы в своем коде кросс платформенных (если требуются).


## <a name="xamarinforms-applications"></a>Приложения Xamarin.Forms

Для использования Xamarin.Forms для кросс платформенных пользовательских интерфейсов, которые также будут использоваться совместно с приложением единый API требуется следующее:

- Всего решения необходимо использовать версии 1.3.1 (или выше) пакета NuGet Xamarin.Forms.
- Для любого Xamarin.iOS пользовательские модули подготовки используйте те же типы решений, представленные выше, в зависимости от того, как код пользовательского интерфейса был общего (общий проект или переносимой библиотеки Классов).

Как в стандартное приложение между различными платформами существующих 32-разрядная версия типы данных должны использоваться в любом коде общего, кросс платформенных для большинства ситуаций все. Новые собственные типы данных следует использовать только в том случае, при вызовах к API iOS или Mac где поддержка архитектуры с поддержкой типы являются обязательными.

Дополнительные сведения см. наш [обновление имеющихся приложений Xamarin.Forms](https://developer.xamarin.com/guides/cross-platform/macios/updating-xamarin-forms-apps/) документации.

## <a name="summary"></a>Сводка

В этой статье у нас есть см. когда следует использовать собственные типы данных в приложении единый API и их последствия между различными платформами. Мы представили несколько решений, которые можно использовать в ситуациях, когда необходимо использовать новые собственные типы данных в кросс платформенные библиотеки. И мы рассмотрели краткое руководство по поддержке Unified API-интерфейсов в Xamarin.Forms кросс платформенных приложений.



## <a name="related-links"></a>Связанные ссылки

- [Unified API](~/cross-platform/macios/unified/index.md)
- [Собственные типы](~/cross-platform/macios/nativetypes.md)
- [Варианты совместного использования кода](~/cross-platform/app-fundamentals/code-sharing.md)
- [Пример совместного использования кода](https://developer.xamarin.com/samples/mobile/SharingCode/)
