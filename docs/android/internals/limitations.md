---
title: Xamarin. Android и настольные компьютеры — различия в среде выполнения Mono
ms.prod: xamarin
ms.assetid: F953F9B4-3596-8B3A-A8E4-8219B5B9F7CA
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
ms.openlocfilehash: 8fe0e3a9adedb161c527ccdf6d6c3a7cd06a1d86
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027842"
---
# <a name="limitations"></a>Ограничения

Поскольку приложениям в Android требуется создавать типы прокси Java во время процесса сборки, невозможно создать весь код во время выполнения.

Это ограничения Xamarin. Android по сравнению с рабочим столом Mono:

## <a name="limited-dynamic-language-support"></a>Ограниченная поддержка динамических языков

 [Вызываемые оболочки Android](~/android/platform/java-integration/android-callable-wrappers.md) необходимы каждый раз, когда исполняющая среда Android должна вызывать управляемый код. Вызываемые оболочки Android создаются во время компиляции на основе статического анализа IL. Чистый результат этого: *нельзя* использовать динамические языки (IronPython, IronRuby и т. д.) в любом сценарии, где требуется подкласс типов Java (включая косвенные подклассы), так как не существует способа извлечения этих динамических типов во время компиляции. создание необходимых вызываемых оболочек Android.

## <a name="limited-java-generation-support"></a>Ограниченная поддержка создания Java

Чтобы код Java мог вызывать управляемый код, необходимо создать [вызываемые оболочки Android](~/android/platform/java-integration/android-callable-wrappers.md) . *По умолчанию*вызываемые оболочки Android будут содержать только (определенные) объявленные конструкторы и методы, переопределяющие виртуальный метод Java (т. е. он имеет [`RegisterAttribute`](xref:Android.Runtime.RegisterAttribute)) или реализуют метод интерфейса Java (также `Attribute`).
  
До выпуска 4,1 дополнительные методы не могут быть объявлены. В выпуске 4,1 [пользовательские атрибуты `Export` и `ExportField` можно использовать для объявления методов и полей Java в вызываемой оболочке Android](~/android/platform/java-integration/working-with-jni.md).

### <a name="missing-constructors"></a>Отсутствующие конструкторы

Конструкторы остаются сложными, если не используется [`ExportAttribute`](xref:Java.Interop.ExportAttribute) . Алгоритм создания конструкторов вызываемой оболочки Android заключается в том, что конструктор Java будет выдаваться в следующих случаях:

1. Существует сопоставление Java для всех типов параметров
2. Базовый класс объявляет тот же конструктор &ndash; это необходимо, поскольку вызываемая оболочка Android *должна* вызывать соответствующий конструктор базового класса; нельзя использовать аргументы по умолчанию (так как нет простого способа определить, какие значения следует использовать в Java).

Например, рассмотрим следующий класс.

```csharp
[Service]
class MyIntentService : IntentService {
    public MyIntentService (): base ("value")
    {
    }
}
```

Хотя это выглядит совершенно логично, полученная обертка Android *в сборках выпуска* не будет содержать конструктор по умолчанию. Следовательно, если вы попытаетесь запустить эту службу (например, [`Context.StartService`](xref:Android.Content.Context.StartService*), она завершится ошибкой:

```shell
E/AndroidRuntime(31766): FATAL EXCEPTION: main
E/AndroidRuntime(31766): java.lang.RuntimeException: Unable to instantiate service example.MyIntentService: java.lang.InstantiationException: can't instantiate class example.MyIntentService; no empty constructor
E/AndroidRuntime(31766):        at android.app.ActivityThread.handleCreateService(ActivityThread.java:2347)
E/AndroidRuntime(31766):        at android.app.ActivityThread.access$1600(ActivityThread.java:130)
E/AndroidRuntime(31766):        at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1277)
E/AndroidRuntime(31766):        at android.os.Handler.dispatchMessage(Handler.java:99)
E/AndroidRuntime(31766):        at android.os.Looper.loop(Looper.java:137)
E/AndroidRuntime(31766):        at android.app.ActivityThread.main(ActivityThread.java:4745)
E/AndroidRuntime(31766):        at java.lang.reflect.Method.invokeNative(Native Method)
E/AndroidRuntime(31766):        at java.lang.reflect.Method.invoke(Method.java:511)
E/AndroidRuntime(31766):        at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:786)
E/AndroidRuntime(31766):        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:553)
E/AndroidRuntime(31766):        at dalvik.system.NativeStart.main(Native Method)
E/AndroidRuntime(31766): Caused by: java.lang.InstantiationException: can't instantiate class example.MyIntentService; no empty constructor
E/AndroidRuntime(31766):        at java.lang.Class.newInstanceImpl(Native Method)
E/AndroidRuntime(31766):        at java.lang.Class.newInstance(Class.java:1319)
E/AndroidRuntime(31766):        at android.app.ActivityThread.handleCreateService(ActivityThread.java:2344)
E/AndroidRuntime(31766):        ... 10 more
```

Обходной путь заключается в объявлении конструктора по умолчанию, оформлении его с помощью `ExportAttribute`и задании [`ExportAttribute.SuperStringArgument`](xref:Java.Interop.ExportAttribute.SuperArgumentsString): 

```csharp
[Service]
class MyIntentService : IntentService {
    [Export (SuperArgumentsString = "\"value\"")]
    public MyIntentService (): base("value")
    {
    }

    // ...
}
```

### <a name="generic-c-classes"></a>Универсальные C# классы

Универсальные C# классы поддерживаются только частично. Существуют следующие ограничения.

- Универсальные типы не могут использовать `[Export]` или `[ExportField`]. Попытка сделать это приведет к возникновению ошибки `XA4207`.

    ```csharp
    public abstract class Parcelable<T> : Java.Lang.Object, IParcelable
    {
        // Invalid; generates XA4207
        [ExportField ("CREATOR")]
        public static IParcelableCreator CreateCreator ()
        {
            ...
    }
    ```

- Универсальные методы не могут использовать `[Export]` или `[ExportField]`:

    ```csharp
    public class Example : Java.Lang.Object
    {
        
        // Invalid; generates XA4207
        [Export]
        public static void Method<T>(T value)
        {
            ...
        }
    }
    ```

- `[ExportField]` не может использоваться в методах, которые возвращают `void`:

    ```csharp
    public class Example : Java.Lang.Object
    {
        // Invalid; generates XA4208
        [ExportField ("CREATOR")]
        public static void CreateSomething ()
        {
        }
    }
    ```

- Экземпляры универсальных типов _не должны_ создаваться из кода Java.
    Их можно только безопасно создать из управляемого кода:

    ```csharp
    [Activity (Label="Die!", MainLauncher=true)]
    public class BadGenericActivity<T> : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);
        }
    }
    ```

## <a name="partial-java-generics-support"></a>Частичная поддержка универсальных шаблонов Java

Поддержка привязки универсальных шаблонов Java ограничена. В частности, члены класса универсального экземпляра, производного от другого универсального класса (без экземпляра), остаются доступными как Java. lang. Object. Например, метод [Android. Content. намерения. жетпарцелабликстра](xref:Android.Content.Intent.GetParcelableExtra*) возвращает Java. lang. Object. Это обусловлено стиранием универсальных шаблонов Java.
У нас есть некоторые классы, которые не применяют это ограничение, но они корректируются вручную.

## <a name="related-links"></a>Связанные ссылки

- [Вызываемые программы-оболочки Android](~/android/platform/java-integration/android-callable-wrappers.md)
- [Работа с JNI](~/android/platform/java-integration/working-with-jni.md)
- [експортаттрибуте](xref:Java.Interop.ExportAttribute)
- [Строка](xref:Java.Interop.ExportAttribute.SuperArgumentsString)
- [регистераттрибуте](xref:Android.Runtime.RegisterAttribute)
