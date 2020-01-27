---
title: Обратные вызовы на Android
ms.prod: xamarin
ms.assetid: F3A7A4E6-41FE-4F12-949C-96090815C5D6
author: davidortinau
ms.author: daortin
ms.date: 11/14/2017
ms.openlocfilehash: 2d1d5b8985d132e5a5839e3cd23aaec32fc3815a
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725117"
---
# <a name="callbacks-on-android"></a>Обратные вызовы на Android

Вызов Java из C# является довольно *рискованным бизнесом*. Это означает, что существует *шаблон* для обратных вызовов с C# на Java; Однако это сложнее, чем хотелось бы.

Мы расскажем о трех вариантах обратного вызова, которые лучше всего подходят для Java:

- Абстрактные классы
- интерфейсов,
- Виртуальные методы

## <a name="abstract-classes"></a>Абстрактные классы

Это самый простой маршрут для обратных вызовов, поэтому я рекомендую использовать `abstract`, если вы только пытаетесь получить обратный вызов в простейшей форме.

Начнем с класса, C# который мы хотим реализовать в Java:

```csharp
[Register("mono.embeddinator.android.AbstractClass")]
public abstract class AbstractClass : Java.Lang.Object
{
    public AbstractClass() { }

    public AbstractClass(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer) { }

    [Export("getText")]
    public abstract string GetText();
}
```

Ниже приведены подробные сведения о том, как выполнить эту работу.

- `[Register]` создает хорошее имя пакета в Java — вы получите автоматически созданное имя пакета без него.
- Подклассы `Java.Lang.Object` сигналы в внедрение .NET для запуска класса через генератор Java Xamarin. Android.
- Пустой конструктор: — это то, что нужно использовать из кода Java.
- Конструктор `(IntPtr, JniHandleOwnership)`: что Xamarin. Android будет использовать для создания C#эквивалентных объектов Java.
- `[Export]` сигнализирует Xamarin. Android предоставить метод Java. Мы также можем изменить имя метода, так как в мире Java нравится использование нижних методов Case.

Теперь создадим C# метод для тестирования сценария:

```csharp
[Register("mono.embeddinator.android.JavaCallbacks")]
public class JavaCallbacks : Java.Lang.Object
{
    [Export("abstractCallback")]
    public static string AbstractCallback(AbstractClass callback)
    {
        return callback.GetText();
    }
}
```

`JavaCallbacks` может быть любым классом для проверки, если это `Java.Lang.Object`.

Теперь выполните внедрение .NET для сборки .NET, чтобы создать AAR. Дополнительные сведения см. в разделе [Начало работы Guide](~/tools/dotnet-embedding/get-started/java/android.md) .

После импорта файла AAR в Android Studio напишем модульный тест:

```java
@Test
public void abstractCallback() throws Throwable {
    AbstractClass callback = new AbstractClass() {
        @Override
        public String getText() {
            return "Java";
        }
    };

    assertEquals("Java", callback.getText());
    assertEquals("Java", JavaCallbacks.abstractCallback(callback));
}
```

Итак, мы:

- Реализует `AbstractClass` в Java с анонимным типом
- Убедитесь, что наш экземпляр возвращает `"Java"` из Java.
- Убедитесь, что наш экземпляр возвращает `"Java"` изC#
- Добавлены `throws Throwable`, поскольку C# конструкторы в настоящий момент помечены `throws`

Если вы запустили этот модульный тест как есть, произойдет сбой с ошибкой, например:

```csharp
System.NotSupportedException: Unable to find Invoker for type 'Android.AbstractClass'. Was it linked away?
```

Здесь отсутствует тип `Invoker`. Это подкласс `AbstractClass`, который перенаправляет C# вызовы к Java. Если объект Java входит C# в мир, а эквивалентный C# тип является абстрактным, Xamarin. Android автоматически ищет C# тип с суффиксом `Invoker` для использования в C# коде.

Xamarin. Android использует этот шаблон `Invoker` для проектов привязки Java, помимо прочего.

Вот наша реализация `AbstractClassInvoker`:

```csharp
class AbstractClassInvoker : AbstractClass
{
    IntPtr class_ref, id_gettext;

    public AbstractClassInvoker(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass(Handle);
        class_ref = JNIEnv.NewGlobalRef(lref);
        JNIEnv.DeleteLocalRef(lref);
    }

    protected override Type ThresholdType
    {
        get { return typeof(AbstractClassInvoker); }
    }

    protected override IntPtr ThresholdClass
    {
        get { return class_ref; }
    }

    public override string GetText()
    {
        if (id_gettext == IntPtr.Zero)
            id_gettext = JNIEnv.GetMethodID(class_ref, "getText", "()Ljava/lang/String;");
        IntPtr lref = JNIEnv.CallObjectMethod(Handle, id_gettext);
        return GetObject<Java.Lang.String>(lref, JniHandleOwnership.TransferLocalRef)?.ToString();
    }

    protected override void Dispose(bool disposing)
    {
        if (class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef(class_ref);
        class_ref = IntPtr.Zero;

        base.Dispose(disposing);
    }
}
```

В этой статье происходит довольно много действий:

- Добавлен класс с суффиксом `Invoker` подклассах `AbstractClass`
- Добавлен `class_ref` для хранения ссылки JNI на класс Java, который подклассет наш C# класс
- Добавлен `id_gettext` для хранения ссылки JNI на метод `getText` Java
- Включает конструктор `(IntPtr, JniHandleOwnership)`
- Реализованы `ThresholdType` и `ThresholdClass` в качестве требования для Xamarin. Android, чтобы узнать сведения о `Invoker`
- `GetText`, необходимые для поиска метода Java `getText` с соответствующей сигнатурой JNI и ее вызова
- `Dispose` просто требуется очистить ссылку на `class_ref`

После добавления этого класса и создания нового AAR модульный тест проходит. Как видите, этот шаблон для обратных вызовов не является *идеальным*, но выполнимо.

Дополнительные сведения о Java Interop см. в [документации по Xamarin. Android](~/android/platform/java-integration/working-with-jni.md) для этой темы.

## <a name="interfaces"></a>интерфейсов,

Интерфейсы во многом идентичны абстрактным классам, за исключением одного подробного: Xamarin. Android не создает Java для них. Это обусловлено тем, что до внедрения .NET не существует многих сценариев, в C# которых Java будет реализовывать интерфейс.

Предположим, что у нас есть следующий C# интерфейс:

```csharp
[Register("mono.embeddinator.android.IJavaCallback")]
public interface IJavaCallback : IJavaObject
{
    [Export("send")]
    void Send(string text);
}
```

`IJavaObject` сигнализирует о внедрении .NET, что это интерфейс Xamarin. Android, но в противном случае он идентичен классу `abstract`.

Так как Xamarin. Android сейчас не создает код Java для этого интерфейса, добавьте в C# проект следующий Java:

```java
package mono.embeddinator.android;

public interface IJavaCallback {
    void send(String text);
}
```

Вы можете поместить файл в любое место, но не забудьте задать для его действия сборки значение `AndroidJavaSource`. Это подаст сигнал внедрению .NET, чтобы скопировать его в нужный каталог для компиляции в файл AAR.

Далее реализация `Invoker` будет совсем одинаковой:

```csharp
class IJavaCallbackInvoker : Java.Lang.Object, IJavaCallback
{
    IntPtr class_ref, id_send;

    public IJavaCallbackInvoker(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass(Handle);
        class_ref = JNIEnv.NewGlobalRef(lref);
        JNIEnv.DeleteLocalRef(lref);
    }

    protected override Type ThresholdType
    {
        get { return typeof(IJavaCallbackInvoker); }
    }

    protected override IntPtr ThresholdClass
    {
        get { return class_ref; }
    }

    public void Send(string text)
    {
        if (id_send == IntPtr.Zero)
            id_send = JNIEnv.GetMethodID(class_ref, "send", "(Ljava/lang/String;)V");
        JNIEnv.CallVoidMethod(Handle, id_send, new JValue(new Java.Lang.String(text)));
    }

    protected override void Dispose(bool disposing)
    {
        if (class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef(class_ref);
        class_ref = IntPtr.Zero;

        base.Dispose(disposing);
    }
}
```

После создания файла AAR в Android Studio можно написать следующий Проходный модульный тест:

```java
class ConcreteCallback implements IJavaCallback {
    public String text;
    @Override
    public void send(String text) {
        this.text = text;
    }
}

@Test
public void interfaceCallback() {
    ConcreteCallback callback = new ConcreteCallback();
    JavaCallbacks.interfaceCallback(callback, "Java");
    assertEquals("Java", callback.text);
}
```

## <a name="virtual-methods"></a>Виртуальные методы

Переопределение `virtual` в Java возможно, но не является большим опытом.

Предположим, что у вас есть следующий C# класс:

```csharp
[Register("mono.embeddinator.android.VirtualClass")]
public class VirtualClass : Java.Lang.Object
{
    public VirtualClass() { }

    public VirtualClass(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer) { }

    [Export("getText")]
    public virtual string GetText() { return "C#"; }
}
```

Если вы следовали приведенному выше примеру класса `abstract`, он будет работать, за исключением одного из сведений: _Xamarin. Android не будет искать `Invoker`_ .

Чтобы устранить эту проблему, измените C# класс так, чтобы он `abstract`:

```csharp
public abstract class VirtualClass : Java.Lang.Object
```

Это не идеальный вариант, но он работает. Xamarin. Android выберет `VirtualClassInvoker` и Java может использовать `@Override` в методе.

## <a name="callbacks-in-the-future"></a>Обратные вызовы в будущем

Существует несколько вещей, которые можно было бы улучшить в следующих сценариях:

1. `throws Throwable` в C# этих запросах для конструкторов [PR](https://github.com/xamarin/java.interop/pull/170)исправлена.
1. Сделайте генератор Java в интерфейсах поддержки Xamarin. Android.
    - Это устраняет необходимость в добавлении исходного файла Java с действием сборки `AndroidJavaSource`.
1. Сделайте так, чтобы Xamarin. Android загружал `Invoker` для виртуальных классов.
    - Это избавляет от необходимости пометить класс в нашем `virtual` примере `abstract`.
1. Автоматическое создание `Invoker` классов для внедрения .NET
    - Это будет сложно, но выполнимо. Xamarin. Android уже выполняет нечто вроде этого для проектов привязки Java.

Здесь можно выполнить множество действий, но эти улучшения внедрены в .NET.

## <a name="further-reading"></a>Дополнительные сведения

- [начало работы в Android](~/tools/dotnet-embedding/get-started/java/android.md)
- [Предварительные исследования Android](~/tools/dotnet-embedding/android/index.md)
- [Ограничения внедрения .NET](~/tools/dotnet-embedding/limitations.md)
- [Коды ошибок и описания](~/tools/dotnet-embedding/errors.md)
