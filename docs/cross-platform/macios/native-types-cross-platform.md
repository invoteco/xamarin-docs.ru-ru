---
title: Работа с собственными типами в кроссплатформенных приложениях
description: В этой статье рассматривается использование новых типов iOS Unified API Native Type (НИНТ, нуинт, нфлоат) в кросс-платформенном приложении, где код используется для устройств, не относящихся к iOS, таких как Android или Windows Phoneные ОС.
ms.prod: xamarin
ms.assetid: B9C56C3B-E196-4ADA-A1DE-AC10D1001C2A
author: davidortinau
ms.author: daortin
ms.date: 04/07/2016
ms.openlocfilehash: c86a00f325f9799b16f6244d3d1cb68de31be005
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73015541"
---
# <a name="working-with-native-types-in-cross-platform-apps"></a>Работа с собственными типами в кроссплатформенных приложениях

_В этой статье рассматривается использование новых типов iOS Unified API Native Type (НИНТ, нуинт, нфлоат) в кросс-платформенном приложении, где код используется для устройств, не относящихся к iOS, таких как Android или Windows Phoneные ОС._

Типы 64-Types работают с API iOS и Mac. При написании общего кода, который выполняется в Android или Windows, необходимо управлять преобразованием Объединенных типов в обычные типы .NET, к которым можно предоставить общий доступ.

В этом документе обсуждаются различные способы взаимодействия с Unified API из общего или общего кода.

## <a name="when-to-use-the-native-types"></a>Когда следует использовать собственные типы

В единые API-интерфейсы Xamarin. iOS и Xamarin. Mac все еще входят `int`, `uint` и `float` типы данных, а также типы `RectangleF`, `SizeF` и `PointF`. Эти существующие типы данных должны продолжать использоваться в любом совместно используемом, кросс-платформенном коде. Новые собственные типы данных следует использовать только при вызове интерфейса API Mac или iOS, где требуется поддержка типов, поддерживающих архитектуру.

В зависимости от природы кода, к которому предоставляется общий доступ, могут возникнуть ситуации, когда для межплатформенного кода может потребоваться работа с типами данных `nint`, `nuint` и `nfloat`. Например, Библиотека, которая обрабатывает преобразования прямоугольных данных, которые ранее использовали `System.Drawing.RectangleF` для совместного использования версий приложения Xamarin. iOS и Xamarin. Android, необходимо обновить для обработки собственных типов в iOS.

Обработка этих изменений зависит от размера и сложности приложения, а также от того, какой формат использовался для совместного использования кода, как будет показано в следующих разделах.

## <a name="code-sharing-considerations"></a>Рекомендации по совместному использованию кода

Как указано в документе о [параметрах совместного использования кода](~/cross-platform/app-fundamentals/code-sharing.md) , существует два основных способа совместного использования кода между межплатформенными проектами: общие проекты и переносимые библиотеки классов. Какой из двух типов был использован, ограничит параметры, которые имеются при обработке собственных типов данных в кросс-платформенном коде.

### <a name="portable-class-library-projects"></a>Проекты переносимой библиотеки классов

Переносимая библиотека классов (PCL) позволяет ориентироваться на платформы, которые требуется поддерживать, и использовать интерфейсы для предоставления функциональных возможностей для конкретных платформ.

Так как тип проекта PCL компилируется в `.DLL` и не имеет смысла Unified API, вы вынуждены использовать существующие типы данных (`int`, `uint`, `float`) в исходном коде PCL, а тип привести вызовы к классам PCL. методы и в интерфейсных приложениях. Пример:

```csharp
using NativePCL;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

### <a name="shared-projects"></a>Общие проекты

Тип проекта общий ресурс позволяет организовать исходный код в отдельном проекте, который затем включается и компилируется в отдельные клиентские приложения для конкретных платформ, а также использует директивы компилятора `#if`, необходимые для управления конкретными платформами. необходимость.

При выборе метода поддержки для собственных типов данных в кросс-платформенном режиме необходимо учитывать размер и сложность мобильных приложений, использующих общий код, а также размер и сложность общего кода. Проект общего ресурса.

На основе этих факторов можно реализовать следующие типы решений, используя директивы компилятора `if __UNIFIED__ ... #endif`, чтобы обрабатывать Unified API конкретные изменения в коде.

#### <a name="using-duplicate-methods"></a>Использование повторяющихся методов

Возьмем пример библиотеки, выполняющей преобразование для прямоугольных данных, указанных выше. Если библиотека содержит только один или два очень простых метода, вы можете создать дублирующиеся версии этих методов для Xamarin. iOS и Xamarin. Android. Пример:

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

В приведенном выше коде, так как подпрограмма `CalculateArea` очень проста, мы использовали условную компиляцию и создали отдельную Unified API версию метода. С другой стороны, если в библиотеке содержалось много подпрограмм или несколько сложных подпрограмм, это решение было бы нецелесообразным, так как в этом случае возникает проблема с поддержанием всех методов в синхронизации для внесения изменений или исправления ошибок.

#### <a name="using-method-overloads"></a>Использование перегрузок методов

В этом случае решением может быть создание перегруженной версии методов с использованием 32-разрядных типов данных, чтобы они стали `CGRect` в качестве параметра и (или) возвращаемого значения, преобразуйте это значение в `RectangleF` (зная, что преобразование из `nfloat` в `float` — Это преобразование с потерей данных) и вызовет исходную версию подпрограммы для выполнения фактической работы. Пример:

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

Опять же, это хорошее решение, если потери точности не влияют на результаты конкретных потребностей вашего приложения.

#### <a name="using-alias-directives"></a>Использование директив псевдонима

Для областей, где вероятность потери точности является проблемой, другое возможное решение заключается в использовании директив `using` для создания псевдонима для типов данных native и Кореграфикс, включая следующий код в начало файлов с общим исходным кодом и преобразуя все необходимые @no_ _t_1_, `uint` или `float` значения для `nint`, `nuint` или `nfloat`:

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

Итак, наш пример кода выглядит следующим образом:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
    // Map Unified CoreGraphic classes to MonoTouch classes
    using RectangleF = global::CoreGraphics.CGRect;
    using SizeF = global::CoreGraphics.CGSize;
    using PointF = global::CoreGraphics.CGPoint;
#else
    // Map Unified types to MonoTouch types
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

Обратите внимание, что мы изменили метод `CalculateArea` для возврата `nfloat` вместо стандартного `float`. Это было сделано для того, чтобы не было получено сообщение об ошибке компиляции при попытке _неявного_ преобразования `nfloat` результата вычисления (поскольку оба значения, умноженные на, имеют тип `nfloat`) в `float` возвращаемое значение.

Если код компилируется и запускается на неUnified APIном устройстве, `using nfloat = global::System.Single;` сопоставляет `nfloat` с `Single`, который неявно преобразуется в `float`, что позволяет использовать клиентское приложение для вызова метода `CalculateArea` без изменения.

#### <a name="using-type-conversions-in-the-front-end-app"></a>Использование преобразований типов в приложении внешнего интерфейса

В случае, если клиентские приложения делают всего несколько вызовов библиотеки общего кода, другое решение может быть неизменным и выполнять приведение типов в приложении Xamarin. iOS или Xamarin. Mac при вызове существующей подпрограммы. Пример:

```csharp
using NativeShared;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

Если используемое приложение делает сотни вызовов к общей библиотеке кода, это, возможно, не будет хорошим решением.

Основываясь на архитектуре нашего приложения, мы могли бы использовать одно или несколько из перечисленных выше решений для поддержки собственных типов данных (где это необходимо) в коде кросс-платформенного кода.

## <a name="xamarinforms-applications"></a>Приложения Xamarin. Forms

Для использования Xamarin. Forms для межплатформенного пользовательского интерфейса, который также будет использоваться совместно с Unified API приложением, необходимо следующее:

- Все решение должно использовать пакет NuGet для Xamarin. Forms версии 1.3.1 (или более поздней).
- Для любого пользовательского рендеринга Xamarin. iOS используйте те же типы решений, которые были представлены выше, в зависимости от того, как был предоставлен общий доступ к коду пользовательского интерфейса (общий проект или PCL).

Как и в стандартном кросс-платформенном приложении, существующие 32-разрядные типы данных должны использоваться в любом многоплатформенном коде в большинстве случаев. Новые собственные типы данных следует использовать только при вызове интерфейса API Mac или iOS, где требуется поддержка типов, поддерживающих архитектуру.

Дополнительные сведения см. в документации по [обновлению существующих приложений Xamarin. Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md) .

## <a name="summary"></a>Сводка

В этой статье мы рассмотрели, когда использовать собственные типы данных в Unified API приложении и их последствия для разных платформ. Мы предпредставил несколько решений, которые можно использовать в ситуациях, когда новые собственные типы данных должны использоваться в межплатформенных библиотеках. Кроме того, мы рассмотрели краткое руководство по поддержке унифицированных интерфейсов API в приложениях на разных платформах Xamarin. Forms.

## <a name="related-links"></a>Связанные ссылки

- [Unified API](~/cross-platform/macios/unified/index.md)
- [Собственные типы](~/cross-platform/macios/nativetypes.md)
- [Параметры общего доступа к коду](~/cross-platform/app-fundamentals/code-sharing.md)
- [Пример общего доступа к коду](https://docs.microsoft.com/samples/xamarin/mobile-samples/sharingcode/)
