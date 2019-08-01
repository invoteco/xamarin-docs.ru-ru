---
title: Работа с JNI и Xamarin. Android
description: Xamarin. Android позволяет писать приложения Android с C# помощью вместо Java. В Xamarin. Android предоставляется несколько сборок, которые предоставляют привязки для библиотек Java, включая Mono. Android. dll и Mono. Android. Гуглемапс. dll. Однако привязки не предоставляются для каждой возможной библиотеки Java, а предоставляемые привязки могут не привязывать все типы и члены Java. Для использования несвязанных типов и членов Java можно использовать собственный интерфейс Java (JNI). В этой статье показано, как использовать JNI для взаимодействия с типами и членами Java из приложений Xamarin. Android.
ms.prod: xamarin
ms.assetid: A417DEE9-7B7B-4E35-A79C-284739E3838E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 2acc57a65f9a9dfb49088c17b5b4cfac7e9d2ab6
ms.sourcegitcommit: f255aa286bd52e8a80ffa620c2e93c97f069f8ec
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/31/2019
ms.locfileid: "68680887"
---
# <a name="working-with-jni-and-xamarinandroid"></a>Работа с JNI и Xamarin. Android

_Xamarin. Android позволяет писать приложения Android с C# помощью вместо Java. В Xamarin. Android предоставляется несколько сборок, которые предоставляют привязки для библиотек Java, включая Mono. Android. dll и Mono. Android. Гуглемапс. dll. Однако привязки не предоставляются для каждой возможной библиотеки Java, а предоставляемые привязки могут не привязывать все типы и члены Java. Для использования несвязанных типов и членов Java можно использовать собственный интерфейс Java (JNI). В этой статье показано, как использовать JNI для взаимодействия с типами и членами Java из приложений Xamarin. Android._

## <a name="overview"></a>Обзор

Создать управляемую обертку (MCW) для вызова кода Java не всегда обязательно или возможно. Во многих случаях «Inline» JNI вполне приемлем и полезен для одноразового использования несвязанных членов Java. Часто проще использовать JNI для вызова одного метода в классе Java, чем для создания всей привязки JAR.

Xamarin. Android предоставляет `Mono.Android.dll` сборку, которая предоставляет привязку для `android.jar` библиотеки Android. Типы и члены, отсутствующие `Mono.Android.dll` в типах и отсутствующих `android.jar` в, могут использоваться для привязки вручную. Для привязки типов и членов Java используется **собственный интерфейс Java** (**JNI**) для поиска типов, полей чтения и записи и вызова методов.

API JNI в Xamarin. Android концептуально похож на `System.Reflection` API в .NET: он позволяет выполнять поиск типов и членов по имени, читать и записывать значения полей, вызывать методы и многое другое. JNI и `Android.Runtime.RegisterAttribute` настраиваемый атрибут можно использовать для объявления виртуальных методов, которые могут быть привязаны для переопределения. Можно привязывать интерфейсы, чтобы их можно было реализовать в C#.

В этом документе объясняется следующее:

- Как JNI ссылается на типы.
- Как искать, читать и записывать поля.
- Способы поиска и вызова методов.
- Как предоставить виртуальные методы, чтобы разрешить переопределение из управляемого кода.
- Как предоставлять интерфейсы.

## <a name="requirements"></a>Требования

JNI, представленный в [пространстве имен Android. Runtime. жниенв](xref:Android.Runtime.JNIEnv), доступен в каждой версии Xamarin. Android.
Для привязки типов и интерфейсов Java необходимо использовать Xamarin. Android 4,0 или более поздней версии.

## <a name="managed-callable-wrappers"></a>Управляемые вызываемые оболочки

Управляемая обертка (**MCW**) представляет собой *привязку* для класса или интерфейса Java, который создает оболочку для всех механизмов JNI, чтобы код C# клиента не должен беспокоиться о базовой сложности JNI. Большинство из них состоит из управляемых вызываемых оболочек. `Mono.Android.dll`

Управляемые вызываемые оболочки служат двум целям:

1.  Инкапсулировать использование JNI, чтобы код клиента не знал о базовой сложности.
1.  Обеспечьте возможность подклассировать Java-типы и реализовать интерфейсы Java.

Первая цель — исключительно для удобства и инкапсуляции сложности, чтобы у потребителей был простой, управляемый набор классов для использования. Для этого необходимо использовать различные члены [жниенв](xref:Android.Runtime.JNIEnv) , как описано далее в этой статье. Помните, что управляемые вызываемые оболочки не являются строго &ndash; необходимыми "встроенным" JNI, вполне приемлемым и полезно для одноразового использования несвязанных членов Java. Реализация подклассов и интерфейса требует использования управляемых вызываемых оболочек.

## <a name="android-callable-wrappers"></a>Вызываемые программы-оболочки Android

Вызываемые оболочки Android (АКВ) требуются, когда среде выполнения Android (ART) требуется вызывать управляемый код. Эти оболочки являются обязательными, поскольку не существует способа регистрации классов во время выполнения.
(В частности, функция [дефинекласс](http://docs.oracle.com/javase/6/docs/technotes/guides/jni/spec/functions.html#wp15986) JNI не поддерживается средой выполнения Android. Таким же вызываемые оболочки Android составляют отсутствие поддержки регистрации типов во время выполнения.)

Каждый раз, когда код Android должен выполнить виртуальный или интерфейсный метод, который переопределен или реализован в управляемом коде, Xamarin. Android должен предоставить прокси Java, чтобы этот метод был отправлен в соответствующий управляемый тип. Эти типы прокси Java — это код Java, который имеет тот же базовый класс и список интерфейсов Java, что и управляемый тип, реализующий те же конструкторы и объявляющие переопределенный базовый класс и методы интерфейса.

Вызываемые оболочки Android создаются программой **monodroid. exe** во время [процесса сборки](~/android/deploy-test/building-apps/build-process.md)и создаются для всех типов, которые (прямо или косвенно) наследуют [Java. lang. Object](xref:Java.Lang.Object).

### <a name="implementing-interfaces"></a>Реализация интерфейсов

Иногда может потребоваться реализовать интерфейс Android (например, [Android. Content. икомпоненткаллбаккс](xref:Android.Content.IComponentCallbacks)).

Все классы и интерфейсы Android расширяют интерфейс [Android. Runtime. ижаваобжект](xref:Android.Runtime.IJavaObject) ; Таким образом, все типы Android должны `IJavaObject`реализовывать.
Xamarin. Android использует &ndash; `IJavaObject` этот факт для предоставления Android с помощью прокси-сервера Java (вызываемой оболочки Android) для данного управляемого типа. Поскольку **monodroid. exe** ищет `Java.Lang.Object` подклассы (которые должны реализовывать `IJavaObject`), подклассы `Java.Lang.Object` предоставляют нам способ реализации интерфейсов в управляемом коде. Например:

```csharp
class MyComponentCallbacks : Java.Lang.Object, Android.Content.IComponentCallbacks {
    public void OnConfigurationChanged (Android.Content.Res.Configuration newConfig) {
        // implementation goes here...
    }
    public void OnLowMemory () {
        // implementation goes here...
    }
}
```


### <a name="implementation-details"></a>Сведения о реализации

*Оставшаяся часть этой статьи содержит сведения о реализации, которые могут быть изменены без предварительного уведомления* . (и предоставляется здесь только потому, что разработчики могут интересно узнать о том, что происходит внутри.

Например, при наличии следующего C# источника:

```csharp
using System;
using Android.App;
using Android.OS;

namespace Mono.Samples.HelloWorld
{
    public class HelloAndroid : Activity
    {
        protected override void OnCreate (Bundle savedInstanceState)
        {
            base.OnCreate (savedInstanceState);
            SetContentView (R.layout.main);
        }
    }
}
```

Программа **аргументы mandroid. exe** создаст следующую вызываемую оболочку Android:

```java
package mono.samples.helloWorld;

public class HelloAndroid extends android.app.Activity {
    static final String __md_methods;
    static {
        __md_methods =
            "n_onCreate:(Landroid/os/Bundle;)V:GetOnCreate_Landroid_os_Bundle_Handler\n" +
            "";
        mono.android.Runtime.register (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null",
                HelloAndroid.class,
                __md_methods);
    }

    public HelloAndroid ()
    {
        super ();
        if (getClass () == HelloAndroid.class)
            mono.android.TypeManager.Activate (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null",
                "", this, new java.lang.Object[] { });
    }

    @Override
    public void onCreate (android.os.Bundle p0)
    {
        n_onCreate (p0);
    }

    private native void n_onCreate (android.os.Bundle p0);
}
```

Обратите внимание, что базовый класс сохраняется, а объявления собственного метода предоставляются для каждого метода, переопределяемого в управляемом коде.



### <a name="exportattribute-and-exportfieldattribute"></a>Експортаттрибуте и Експортфиелдаттрибуте

Как правило, Xamarin. Android автоматически создает код Java, который состоит из АКВ; Это поколение основано на именах классов и методов, когда класс является производным от класса Java и переопределяет существующие методы Java. Однако в некоторых сценариях создание кода не является достаточным, как описано ниже:

-   Android поддерживает имена действий в XML-атрибутах макета, например атрибут [Android: OnClick](xref:Android.Views.View.IOnClickListener.OnClick*) XML. Если он указан, экземпляр неструктурированного представления пытается найти метод Java.

-   Для интерфейса [Java. IO. Serializable](https://developer.android.com/reference/java/io/Serializable.html) требуются `readObject` методы `writeObject` и. Так как они не являются членами этого интерфейса, наша соответствующая управляемая реализация не предоставляет эти методы коду Java.

-   Интерфейс [Android. OS. для загрузки](xref:Android.OS.Parcelable) требует, чтобы класс реализации имел статическое поле `CREATOR` типа. `Parcelable.Creator` Для созданного кода Java требуется явное поле. В нашем стандартном сценарии не существует способа вывода поля в коде Java из управляемого кода.

Поскольку создание кода не предоставляет решения для создания произвольных методов Java с произвольными именами, начиная с Xamarin. Android 4,2, были введены [експортаттрибуте](xref:Java.Interop.ExportAttribute) и [експортфиелдаттрибуте](xref:Java.Interop.ExportFieldAttribute) , предлагающие решение, приведенное выше. вариант. Оба атрибута находятся в `Java.Interop` пространстве имен:

-   `ExportAttribute`&ndash; указывает имя метода и ожидаемые типы исключений (для предоставления явного "Throw" в Java). При использовании в методе метод экспортируется в метод Java, который создает код диспетчеризации для соответствующего вызова JNI в управляемом методе. Его можно использовать с `android:onClick` и. `java.io.Serializable`

-   `ExportFieldAttribute`&ndash; указывает имя поля. Он находится в методе, который работает как инициализатор поля. Его можно использовать с `android.os.Parcelable`.

В примере проекта [експортаттрибуте](https://docs.microsoft.com/samples/xamarin/monodroid-samples/exportattribute) показано, как использовать эти атрибуты.


#### <a name="troubleshooting-exportattribute-and-exportfieldattribute"></a>Устранение неполадок Експортаттрибуте и Експортфиелдаттрибуте

-   Сбой упаковки из-за отсутствия **Mono. Android. Export. dll** &ndash; при `ExportAttribute` использовании `ExportFieldAttribute` или в некоторых методах кода или зависимых библиотек необходимо добавить **Mono. Android. Export. dll**. Эта сборка изолирована для поддержки кода обратного вызова из Java. Он отделен от **Mono. Android. dll** , так как добавляет дополнительный размер в приложение.

-   В выпуске `MissingMethodException` Build, происходит для &ndash; методов экспорта в сборке `MissingMethodException` выпуска, выполняется для методов экспорта. (Эта проблема исправлена в последней версии Xamarin. Android.)



### <a name="exportparameterattribute"></a>експортпараметераттрибуте

`ExportAttribute`и `ExportFieldAttribute` предоставляют функциональные возможности, которые может использовать код времени выполнения Java. Этот код времени выполнения обращается к управляемому коду через созданные методы JNI, управляемые этими атрибутами. В результате нет существующего метода Java, который привязывает управляемый метод. Таким образом, метод Java создается из сигнатуры управляемого метода.

Однако этот вариант не является полностью детерминированным. Особенно важно, что это справедливо в некоторых расширенных сопоставлениях между управляемыми типами и типами Java, такими как:

-  InputStream
-  OutputStream
-  ксмлпуллпарсер
-  ксмлресаурцепарсер

Если типы, такие как, необходимы для экспортированных методов, `ExportParameterAttribute` необходимо использовать для явного предоставления соответствующего параметра или возвращаемого значения типу.



### <a name="annotation-attribute"></a>Атрибут заметки

В Xamarin. Android 4,2 мы преобразовали `IAnnotation` типы реализации в атрибуты (System. Attribute) и добавили поддержку создания заметок в оболочках Java.

Это означает следующие изменения направления:

-   Генератор привязки создается `Java.Lang.DeprecatedAttribute` из `java.Lang.Deprecated` (в то время как он `[Obsolete]` должен находиться в управляемом коде).

-   Это не означает, что существующий `Java.Lang.Deprecated` класс будет «исчезновение». Эти объекты на основе Java можно по-прежнему использовать как обычные объекты Java (если такое использование существует). Будут существовать `Deprecated` классы и `DeprecatedAttribute` .

-   Класс помечен как `[Annotation]`. `Java.Lang.DeprecatedAttribute` При наличии настраиваемого атрибута, унаследованного от этого `[Annotation]` атрибута, задача MSBuild создаст аннотацию Java для этого настраиваемого атрибута (@Deprecated) в вызываемой оболочке Android (АКВ).

-   Заметки могут быть созданы на классах, методах и экспортируемых полях (которые являются методом в управляемом коде).

Если содержащийся класс (класс с заметками или класс, содержащий элементы с заметками) не зарегистрирован, весь источник класса Java не создается вообще, включая заметки. Для методов можно указать, `ExportAttribute` чтобы получить явно созданный метод и закомментировать его. Кроме того, невозможно создать определение класса заметки Java. Иными словами, при определении настраиваемого управляемого атрибута для определенной заметки необходимо добавить другую библиотеку JAR-файлов, содержащую соответствующий класс аннотации Java. Добавление исходного файла Java, определяющего тип заметки, не является достаточным. Компилятор Java не работает так же, как **APT**.

Кроме того, применяются следующие ограничения.

-   В этом процессе преобразования не учитывается `@Target` аннотация типа заметки.

-   Атрибуты на свойство не работают. Вместо этого используйте атрибуты для метода получения или задания свойства.



## <a name="class-binding"></a>Привязка класса

Привязка класса означает написание управляемой вызываемой оболочки для упрощения вызова базового типа Java.

Привязка виртуальных и абстрактных методов для разрешения переопределения из C# требует наличия Xamarin. Android 4,0. Однако любая версия Xamarin. Android может привязывать невиртуальные методы, статические методы или виртуальные методы без поддержки переопределений.

Привязка обычно содержит следующие элементы:

-  [Обработчик JNI для привязанного типа Java](#_Looking_up_Java_Types).

-  [JNI идентификаторы полей и свойства для каждого привязанного поля](#_Instance_Fields).

-  [JNI идентификаторы методов и методы для каждого привязанного метода](#_Instance_Methods).

-  Если требуется подкласс, тип должен иметь настраиваемый атрибут [регистераттрибуте](xref:Android.Runtime.RegisterAttribute) в объявлении типа с [регистераттрибуте. донотженератеакв](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw) установлен в `true`значение.



### <a name="declaring-type-handle"></a>Объявление маркера типа

Для методов поиска полей и методов требуется ссылка на объект, ссылающийся на их объявляющий тип. По соглашению это значение хранится в `class_ref` поле:

```csharp
static IntPtr class_ref = JNIEnv.FindClass(CLASS);
```

Дополнительные сведения о `CLASS` токене см. в разделе [JNI Type References](#_JNI_Type_References) .


### <a name="binding-fields"></a>Привязка полей

Поля Java предоставляются в виде C# свойств, например поле Java [Java.lang.System.in](https://developer.android.com/reference/java/lang/System.html#in) привязывается как C# свойство [Java.lang.JavaSystem.in](xref:Java.Lang.JavaSystem.In).
Более того, поскольку JNI различает статические поля и поля экземпляров, для реализации свойств используются различные методы.

Привязка поля включает три набора методов:

1.  Метод *получения идентификатора поля* . Метод *получения поля* с идентификатором отвечает за возврат маркера поля, который будет использоваться методами значения *поля Get* и *set поля* . Получение идентификатора поля требует знания объявляющего типа, имени поля и [сигнатуры типа JNI](#JNI_Type_Signatures) поля.

1.  Методы *получения значения поля* . Эти методы занимают маркер поля и отвечают за считывание значения поля из Java.
    Используемый метод зависит от типа поля.

1.  Методы *набора значений полей* . Эти методы занимают маркер поля и отвечают за запись значения поля в Java. Используемый метод зависит от типа поля.

[Статические поля](#_Static_Fields) используют методы [жниенв. жетстатикфиелдид](xref:Android.Runtime.JNIEnv.GetStaticMethodID*), `JNIEnv.GetStatic*Field`и [жниенв. сетстатикфиелд](xref:Android.Runtime.JNIEnv.SetStaticField*) .

 В [полях экземпляра](#_Instance_Fields) используются методы [жниенв. жетфиелдид](xref:Android.Runtime.JNIEnv.GetFieldID*), `JNIEnv.Get*Field`и [жниенв. SetField](xref:Android.Runtime.JNIEnv.SetField*) .

Например, статическое свойство `JavaSystem.In` может быть реализовано следующим образом:

```csharp
static IntPtr in_jfieldID;
public static System.IO.Stream In
{
    get {
        if (in_jfieldId == IntPtr.Zero)
            in_jfieldId = JNIEnv.GetStaticFieldID (class_ref, "in", "Ljava/io/InputStream;");
        IntPtr __ret = JNIEnv.GetStaticObjectField (class_ref, in_jfieldId);
        return InputStreamInvoker.FromJniHandle (__ret, JniHandleOwnership.TransferLocalRef);
    }
}
```

Примечание. Мы используем [инпутстреаминвокер. фромжнихандле](xref:Android.Runtime.InputStreamInvoker.FromJniHandle*) для преобразования ссылки JNI в `System.IO.Stream` экземпляр `JniHandleOwnership.TransferLocalRef` и используем, поскольку [жниенв. жетстатикобжектфиелд](xref:Android.Runtime.JNIEnv.GetStaticObjectField*) Возвращает локальную ссылку.

Многие типы [Android. Runtime](xref:Android.Runtime) имеют `FromJniHandle` методы, которые преобразуют ссылку JNI в нужный тип.

### <a name="method-binding"></a>Привязка метода

Методы Java предоставляются как C# методы и как C# свойства. Например, метод Java [Java. lang. Runtime. рунфинализерсонексит](https://developer.android.com/reference/java/lang/Runtime.html#runFinalizersOnExit(boolean)) привязан как метод [Java. lang. Runtime. рунфинализерсонексит](xref:Java.Lang.Runtime.RunFinalizersOnExit*) , а метод [Java. lang. Object. GetObject](https://developer.android.com/reference/java/lang/Object.html#getClass) привязан как [Java. lang. Object. class. ](xref:Java.Lang.Object.Class)свойство.

Вызов метода состоит из двух этапов:

1.  *Идентификатор метода Get* для вызываемого метода. Метод *Get метода ID* отвечает за возврат обработчика метода, который будет использоваться методами вызова метода. Получение идентификатора метода требует знания объявляющего типа, имени метода и [сигнатуры типа JNI](#JNI_Type_Signatures) метода.

1.  Вызовите метод.

Как и в случае с полями, методы, используемые для получения идентификатора метода и вызова метода, отличаются между статическими методами и методами экземпляра.

[Статические методы](#_Static_Methods_1) используют [жниенв. жетстатикмесодид ()](xref:Android.Runtime.JNIEnv.GetStaticMethodID*) для поиска идентификатора метода `JNIEnv.CallStatic*Method` и используют семейство методов для вызова.

[Методы экземпляра](#_Instance_Methods) используют [жниенв. жетмесодид](xref:Android.Runtime.JNIEnv.GetMethodID*) для поиска идентификатора метода `JNIEnv.Call*Method` и используют семейства методов и `JNIEnv.CallNonvirtual*Method` для вызова.

Привязка метода, возможно, является не просто вызовом метода. Привязка метода также включает возможность переопределения метода (для абстрактных и неконечных методов) или реализации (для методов интерфейса). В разделе [Поддержка наследования, интерфейсы](#_Supporting_Inheritance,_Interfaces_1) описываются сложности поддерживающих виртуальные методы и методы интерфейса.

<a name="_Static_Methods_1" />

#### <a name="static-methods"></a>Статические методы

Привязка статического метода предполагает использование `JNIEnv.GetStaticMethodID` для получения обработчика метода, а затем использует соответствующий `JNIEnv.CallStatic*Method` метод в зависимости от типа возвращаемого значения метода. Ниже приведен пример привязки для метода [Runtime.](https://developer.android.com/reference/java/lang/Runtime.html#getRuntime()) среды выполнения.

```csharp
static IntPtr id_getRuntime;

[Register ("getRuntime", "()Ljava/lang/Runtime;", "")]
public static Java.Lang.Runtime GetRuntime ()
{
    if (id_getRuntime == IntPtr.Zero)
        id_getRuntime = JNIEnv.GetStaticMethodID (class_ref,
                "getRuntime", "()Ljava/lang/Runtime;");

    return Java.Lang.Object.GetObject<Java.Lang.Runtime> (
            JNIEnv.CallStaticObjectMethod  (class_ref, id_getRuntime),
            JniHandleOwnership.TransferLocalRef);
}
```

Обратите внимание, что мы сохраняем обработчик метода в статическом поле `id_getRuntime`,. Это оптимизация производительности, поэтому при каждом вызове не требуется выполнять поиск обработчика метода. Не обязательно кэшировать обработчик метода таким образом. После получения маркера метода [жниенв. каллстатикобжектмесод](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) используется для вызова метода. `JNIEnv.CallStaticObjectMethod`Возвращает значение `IntPtr` типа, содержащее маркер возвращаемого экземпляра Java.
[Java. lang. Object.&lt;GetObject T&gt;(IntPtr, жнихандлеовнершип)](xref:Java.Lang.Object.GetObject*) используется для преобразования дескриптора Java в экземпляр объекта со строгой типизацией.



#### <a name="non-virtual-instance-method-binding"></a>Привязка метода не виртуального экземпляра

Привязка метода `JNIEnv.GetMethodID` `JNIEnv.Call*Method` экземпляра или метода экземпляра, который не требует переопределения, предполагает использование для получения обработчика метода, а затем использует соответствующий метод в зависимости от типа возвращаемого значения метода. `final` Ниже приведен пример привязки для `Object.Class` свойства.

```csharp
static IntPtr id_getClass;
public Java.Lang.Class Class {
    get {
        if (id_getClass == IntPtr.Zero)
            id_getClass = JNIEnv.GetMethodID (class_ref, "getClass", "()Ljava/lang/Class;");
        return Java.Lang.Object.GetObject<Java.Lang.Class> (
                JNIEnv.CallObjectMethod (Handle, id_getClass),
                JniHandleOwnership.TransferLocalRef);
    }
}
```

Обратите внимание, что мы сохраняем обработчик метода в статическом поле `id_getClass`,.
Это оптимизация производительности, поэтому при каждом вызове не требуется выполнять поиск обработчика метода. Не обязательно кэшировать обработчик метода таким образом. После получения маркера метода [жниенв. каллстатикобжектмесод](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) используется для вызова метода. `JNIEnv.CallStaticObjectMethod`Возвращает значение `IntPtr` типа, содержащее маркер возвращаемого экземпляра Java.
[Java. lang. Object.&lt;GetObject T&gt;(IntPtr, жнихандлеовнершип)](xref:Java.Lang.Object.GetObject*) используется для преобразования дескриптора Java в экземпляр объекта со строгой типизацией.


### <a name="binding-constructors"></a>Конструкторы привязки

Конструкторы — это методы Java с именем `"<init>"`. Как и в случае с методами `JNIEnv.GetMethodID` экземпляра Java, используется для поиска обработчика конструктора. В отличие от методов Java, методы [жниенв. NewObject](xref:Android.Runtime.JNIEnv.NewObject*) используются для вызова обработчика метода конструктора. Возвращаемое значение `JNIEnv.NewObject` является JNI локальной ссылкой:


```csharp
int value = 42;
IntPtr class_ref    = JNIEnv.FindClass ("java/lang/Integer");
IntPtr id_ctor_I    = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
IntPtr lrefInstance = JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value));
// Dispose of lrefInstance, class_ref…
```

Обычно привязка класса будет подклассом [Java. lang. Object](xref:Java.Lang.Object).
При создании подклассов `Java.Lang.Object`происходит дополнительная семантика `Java.Lang.Object` : экземпляр поддерживает глобальную ссылку на экземпляр Java через `Java.Lang.Object.Handle` свойство.

1.  Конструктор `Java.Lang.Object` по умолчанию будет распределять экземпляр Java.

1.  Если `RegisterAttribute` тип имеет значение, а `RegisterAttribute.DoNotGenerateAcw` — `true` , то экземпляр `RegisterAttribute.Name` типа создается с помощью конструктора по умолчанию.

1.  В противном случае вызываемая [оболочка Android](~/android/platform/java-integration/android-callable-wrappers.md) ( `this.GetType` АКВ), соответствующая, создается с помощью конструктора по умолчанию. Вызываемые оболочки Android создаются во время создания пакета для `Java.Lang.Object` каждого подкласса `RegisterAttribute.DoNotGenerateAcw` , для которого не `true`задано значение.

Для типов, которые не являются привязками класса, это ожидаемая семантика: `Mono.Samples.HelloWorld.HelloAndroid` экземпляр C# экземпляра должен создавать экземпляр Java `mono.samples.helloworld.HelloAndroid` , который представляет собой созданную вызываемую оболочку Android.

Для привязок классов это поведение может быть правильным, если тип Java содержит конструктор по умолчанию и не требуется вызывать другие конструкторы. В противном случае необходимо предоставить конструктор, который выполняет следующие действия:

1.  Вызов [Java. lang. Object (IntPtr, жнихандлеовнершип)](xref:Java.Lang.Object#ctor*) вместо конструктора по умолчанию `Java.Lang.Object` . Это необходимо, чтобы не создавать новый экземпляр Java.

1.  Проверьте значение [Java. lang. Object. Handle](xref:Java.Lang.Object.Handle) перед созданием экземпляров Java. Свойство будет иметь значение, отличное от `IntPtr.Zero` , если вызываемая оболочка Android была создана в коде Java, а привязка класса создается так, чтобы содержать созданный экземпляр оболочки Android. `Object.Handle` Например `mono.samples.helloworld.HelloAndroid` , когда Android создает экземпляр, сначала будет создана вызываемая оболочка Android, а конструктор Java `HelloAndroid` создаст экземпляр `Object.Handle` соответствующего `Mono.Samples.HelloWorld.HelloAndroid` типа со свойством, которое задайте экземпляру Java перед выполнением конструктора.

1.  Если текущий тип среды выполнения отличается от объявляющего типа, необходимо создать экземпляр соответствующей вызываемой оболочки Android и использовать [Object. сесандле](xref:Java.Lang.Object.SetHandle*) для хранения маркера, возвращаемого методом [жниенв. CreateInstance](xref:Android.Runtime.JNIEnv.CreateInstance*).

1.  Если текущий тип среды выполнения совпадает с объявляемым типом, вызовите конструктор Java и используйте [Object. сесандле](xref:Java.Lang.Object.SetHandle*) для хранения маркера, возвращаемого `JNIEnv.NewInstance` .

Например, рассмотрим конструктор [Java. lang. Integer (int)](https://developer.android.com/reference/java/lang/Integer.html#Integer(int)) . Он привязан как:

```csharp
// Cache the constructor's method handle for later use
static IntPtr id_ctor_I;

// Need [Register] for subclassing
// RegisterAttribute.Name is always ".ctor"
// RegisterAttribute.Signature is tye JNI type signature of constructor
// RegisterAttribute.Connector is ignored; use ""
[Register (".ctor", "(I)V", "")]
public Integer (int value)
    // 1. Prevent Object default constructor execution
    : base (IntPtr.Zero, JniHandleOwnership.DoNotTransfer)
{
    // 2. Don't allocate Java instance if already allocated
    if (Handle != IntPtr.Zero)
        return;

    // 3. Derived type? Create Android Callable Wrapper
    if (GetType () != typeof (Integer)) {
        SetHandle (
                Android.Runtime.JNIEnv.CreateInstance (GetType (), "(I)V", new JValue (value)),
                JniHandleOwnership.TransferLocalRef);
        return;
    }

    // 4. Declaring type: lookup &amp; cache method id...
    if (id_ctor_I == IntPtr.Zero)
        id_ctor_I = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
    // ...then create the Java instance and store
    SetHandle (
            JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value)),
            JniHandleOwnership.TransferLocalRef);
}
```

Методы [жниенв. CreateInstance](xref:Android.Runtime.JNIEnv.CreateInstance*) являются вспомогательными методами для выполнения `JNIEnv.FindClass`, `JNIEnv.GetMethodID`, `JNIEnv.NewObject`и `JNIEnv.DeleteGlobalReference` для значения, возвращаемого из `JNIEnv.FindClass`. См. подробные сведения в следующем подразделе.

<a name="_Supporting_Inheritance,_Interfaces_1" />

### <a name="supporting-inheritance-interfaces"></a>Поддержка наследования, интерфейсы

Для создания подкласса типа Java или реализации интерфейса Java требуется создание вызываемых [оболочек Android](~/android/platform/java-integration/android-callable-wrappers.md) (АКВС), создаваемых для каждого `Java.Lang.Object` подкласса в процессе упаковки. Создание АКВ контролируется с помощью настраиваемого атрибута [Android. Runtime. регистераттрибуте](xref:Android.Runtime.RegisterAttribute) .

Для C# типов `[Register]` конструктор настраиваемого атрибута требует один аргумент: [JNI упрощенный справочник по типам](#_Simplified_Type_References_1) для соответствующего типа Java. Это позволяет предоставлять разные имена между Java и C#.

До Xamarin. Android 4,0 `[Register]` пользовательский атрибут был недоступен для "псевдонимов" существующих типов Java. Это обусловлено тем, что процесс создания АКВ создаст АКВС для `Java.Lang.Object` каждого обнаруженного подкласса.

В Xamarin. Android 4,0 введено свойство [регистераттрибуте. донотженератеакв](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw) . Это свойство указывает процессу создания АКВ *пропустить* тип с аннотацией, что позволяет объявить новые управляемые вызываемые оболочки, которые не будут приводить к созданию АКВС при создании пакета. Это позволяет привязать существующие типы Java. Например, рассмотрим следующий простой класс Java, `Adder`который содержит один метод, `add`который добавляет к целочисленным значениям и возвращает результат:

```java
package mono.android.test;
public class Adder {
    public int add (int a, int b) {
        return a + b;
    }
}
```

`Adder` Тип можно привязать следующим образом:

```csharp
[Register ("mono/android/test/Adder", DoNotGenerateAcw=true)]
public partial class Adder : Java.Lang.Object {
    static IntPtr class_ref = JNIEnv.FindClass ( "mono/android/test/Adder");

    public Adder ()
    {
    }

    public Adder (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }
}
partial class ManagedAdder : Adder {
}
```

Здесь тип имеет `Adder` псевдоним для типа Java. `Adder` C# Атрибут используется для указания JNI имени `mono.android.test.Adder` типа `DoNotGenerateAcw` Java, а свойство используется для запрета создания АКВ. `[Register]` Это приведет к созданию АКВ для `ManagedAdder` типа, который правильно подклассет `mono.android.test.Adder` тип. Если свойство не использовалось, процесс сборки Xamarin. Android создавал бы новый `mono.android.test.Adder` тип Java. `RegisterAttribute.DoNotGenerateAcw` Это приведет к ошибкам компиляции, так как `mono.android.test.Adder` тип будет представлен дважды в двух отдельных файлах.



### <a name="binding-virtual-methods"></a>Привязка виртуальных методов

`ManagedAdder`подклассировать тип Java `Adder` , но это не особенно интересно C# `Adder` : тип не определяет виртуальные методы, поэтому `ManagedAdder` не может переопределять ничего.

Для `virtual` методов привязки, допускающих переопределение подклассов, необходимо выполнить несколько вещей, которые делятся на две следующие категории:

1.  **Привязка метода**

1.  **Регистрация метода**


#### <a name="method-binding"></a>Привязка метода

Привязка метода C# `Adder` требует добавления двух элементов поддержки в определение: `ThresholdType`и `ThresholdClass`.

##### <a name="thresholdtype"></a>срешолдтипе

`ThresholdType` Свойство возвращает текущий тип привязки:

```csharp
partial class Adder {
    protected override System.Type ThresholdType {
        get {
            return typeof (Adder);
        }
    }
}
```

`ThresholdType`используется в привязке метода, чтобы определить, когда он должен выполнять виртуальные и невиртуальные методы диспетчеризации. Он должен всегда возвращать `System.Type` экземпляр, соответствующий объявляемому C# типу.

##### <a name="thresholdclass"></a>срешолдкласс

`ThresholdClass` Свойство возвращает ссылку на класс JNI для связанного типа:

```csharp
partial class Adder {
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```

`ThresholdClass`используется в привязке метода при вызове методов, не являющихся виртуальными.

#### <a name="binding-implementation"></a>Реализация привязки

Реализация привязки метода отвечает за вызов метода Java в среде выполнения. Он также содержит `[Register]` объявление настраиваемого атрибута, которое является частью регистрации метода, и будет обсуждаться в разделе Регистрация метода:

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
    public virtual int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        if (GetType () == ThresholdType)
            return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
        return JNIEnv.CallNonvirtualIntMethod (Handle, ThresholdClass, id_add, new JValue (a), new JValue (b));
    }
}
```

`id_add` Поле содержит идентификатор метода для вызова метода Java. `JNIEnv.GetMethodID``class_ref``"add"`Значение берется из, для которого требуется объявляющий класс (), имя метода Java () и сигнатура JNI метода (`"(II)I"`). `id_add`

После получения идентификатора метода он `GetType` `ThresholdType` сравнивается с, чтобы определить, требуется ли виртуальная или Невиртуальная доставка. Виртуальная отправка необходима при `GetType` совпадении `ThresholdType`, `Handle` как это может указывать на подкласс, выделенный Java, который переопределяет метод.

Если `GetType` не совпадает `ThresholdType` `ManagedAdder` `base.Add`, `Adder` то классдолженбытьподклассами(например,),иреализациябудетвызыватьсятолькопри`Adder.Add` вызове подкласса. Это не виртуальный вариант диспетчеризации, который `ThresholdClass` входит в состав. `ThresholdClass`Указывает, какой класс Java предоставит реализацию вызываемого метода.



#### <a name="method-registration"></a>Регистрация метода

Предположим, что у нас `ManagedAdder` есть обновленное определение `Adder.Add` , переопределяющее метод:

```csharp
partial class ManagedAdder : Adder {
    public override int Add (int a, int b) {
        return (a*2) + (b*2);
    }
}
```

Помните, `Adder.Add` что `[Register]` имел настраиваемый атрибут:

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
```

Конструктор `[Register]` настраиваемого атрибута принимает три значения:

1.  Имя метода Java (в данном случае `"add"` ).

1.  Сигнатура типа JNI метода ( `"(II)I"` в данном случае).

1.  *Метод соединителя* , `GetAddHandler` в данном случае.
    Методы соединителя будут обсуждаться далее.


Первые два параметра позволяют процессу создания АКВ создать объявление метода для переопределения метода. Результирующий АКВ будет содержать следующий код:

```csharp
public class ManagedAdder extends mono.android.test.Adder {
    static final String __md_methods;
    static {
        __md_methods = "n_add:(II)I:GetAddHandler\n" +
            "";
        mono.android.Runtime.register (...);
    }
    @Override
    public int add (int p0, int p1) {
        return n_add (p0, p1);
    }
    private native int n_add (int p0, int p1);
    // ...
}
```

Обратите внимание `@Override` , что объявляется метод, который делегирует методу с префиксом, имеющему `n_`то же имя. Это гарантирует, что при вызове `ManagedAdder.add` `ManagedAdder.n_add` кода Java будет вызван, что позволит выполнить переопределяющий C# `ManagedAdder.Add` метод.

Таким образом, самый важный вопрос: как `ManagedAdder.n_add` подключен к? `ManagedAdder.Add`

Методы `native` Java регистрируются в среде выполнения Java (среда выполнения Android) с помощью [функции JNI регистернативес](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp17734).
`RegisterNatives`принимает массив структур, содержащих имя метода Java, сигнатуру типа JNI и указатель на функцию для вызова следующего [соглашения о вызове JNI](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/design.html#wp715).
Указатель функции должен быть функцией, принимающей два аргумента-указателя, за которыми следуют параметры метода. Метод Java `ManagedAdder.n_add` должен быть реализован с помощью функции, имеющей следующий прототип C:

```csharp
int FunctionName(JNIEnv *env, jobject this, int a, int b)
```

Xamarin. Android не предоставляет `RegisterNatives` метод. Вместо этого АКВ и MCW вместе предоставляют сведения, необходимые для вызова `RegisterNatives`: АКВ содержит имя метода и сигнатуру типа JNI, единственная отсутствующая вещь — это указатель функции для подключения.

Именно здесь поставляется *метод соединителя* . Третий `[Register]` параметр настраиваемого атрибута является именем метода, определенного в зарегистрированном типе, или базового класса зарегистрированного типа, который не принимает параметры и `System.Delegate`возвращает. Возвращаемый `System.Delegate` в свою очередь ссылка указывает на метод, имеющий правильную сигнатуру функции JNI. Наконец, делегат, возвращаемый методом соединителя, *должен* быть корневым, чтобы сборщик мусора не собирал его, так как делегат предоставляется Java.

```csharp
#pragma warning disable 0169
static Delegate cb_add;
// This method must match the third parameter of the [Register]
// custom attribute, must be static, must return System.Delegate,
// and must accept no parameters.
static Delegate GetAddHandler ()
{
    if (cb_add == null)
        cb_add = JNINativeWrapper.CreateDelegate ((Func<IntPtr, IntPtr, int, int, int>) n_Add);
    return cb_add;
}
// This method is registered with JNI.
static int n_Add (IntPtr jnienv, IntPtr lrefThis, int a, int b)
{
    Adder __this = Java.Lang.Object.GetObject<Adder>(lrefThis, JniHandleOwnership.DoNotTransfer);
    return __this.Add (a, b);
}
#pragma warning restore 0169
```

`GetAddHandler` Метод создает`n_Add` делегат, который ссылается на метод, а затем вызывает [жнинативевраппер. креатеделегате.](xref:Android.Runtime.JNINativeWrapper.CreateDelegate*) `Func<IntPtr, IntPtr, int, int,
int>`
`JNINativeWrapper.CreateDelegate`заключает предоставленный метод в блок try/catch, чтобы все необработанные исключения обрабатывались, и это приведет к вызову события [андроидевент. унхандледексцептионраисер](xref:Android.Runtime.AndroidEnvironment.UnhandledExceptionRaiser) . Результирующий делегат хранится в статической `cb_add` переменной, поэтому сборщик мусора не освобождает делегат.

Наконец, `n_Add` метод отвечает за упаковку параметров JNI в соответствующие управляемые типы, а затем делегируя вызов метода.

Примечание. Всегда используйте `JniHandleOwnership.DoNotTransfer` при получении MCW через экземпляр Java. Их обработка в качестве локальной ссылки (и, таким `JNIEnv.DeleteLocalRef`образом, вызов) приведет&gt; к нарушению управляемого Java&gt; перехода стека.

### <a name="complete-adder-binding"></a>Завершение привязки Adder

Полная управляемая привязка для `mono.android.tests.Adder` типа:

```csharp
[Register ("mono/android/test/Adder", DoNotGenerateAcw=true)]
public class Adder : Java.Lang.Object {

    static IntPtr class_ref = JNIEnv.FindClass ("mono/android/test/Adder");

    public Adder ()
    {
    }

    public Adder (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }

    protected override Type ThresholdType {
        get {return typeof (Adder);}
    }

    protected override IntPtr ThresholdClass {
        get {return class_ref;}
    }

#region Add
    static IntPtr id_add;

    [Register ("add", "(II)I", "GetAddHandler")]
    public virtual int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        if (GetType () == ThresholdType)
            return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
        return JNIEnv.CallNonvirtualIntMethod (Handle, ThresholdClass, id_add, new JValue (a), new JValue (b));
    }

#pragma warning disable 0169
    static Delegate cb_add;
    static Delegate GetAddHandler ()
    {
        if (cb_add == null)
            cb_add = JNINativeWrapper.CreateDelegate ((Func<IntPtr, IntPtr, int, int, int>) n_Add);
        return cb_add;
    }

    static int n_Add (IntPtr jnienv, IntPtr lrefThis, int a, int b)
    {
        Adder __this = Java.Lang.Object.GetObject<Adder>(lrefThis, JniHandleOwnership.DoNotTransfer);
        return __this.Add (a, b);
    }
#pragma warning restore 0169
#endregion
}
```



### <a name="restrictions"></a>Ограничения

При записи типа, который соответствует следующим критериям:

1.  Подклассы`Java.Lang.Object`

1.  `[Register]` Имеет настраиваемый атрибут

1.  `RegisterAttribute.DoNotGenerateAcw` равно `true`


Затем для взаимодействия GC тип *не должен* иметь полей, которые могут ссылаться `Java.Lang.Object` на подкласс или `Java.Lang.Object` в среде выполнения. Например, не допускаются поля `System.Object` типа и любого типа интерфейса. Разрешены типы, которые `Java.Lang.Object` не могут ссылаться на экземпляры, `System.String` такие `List<int>`как и. Это ограничение заключается в предотвращении преждевременной коллекции объектов сборщиком мусора.

Если тип должен содержать поле экземпляра, которое может ссылаться на `Java.Lang.Object` экземпляр, то тип поля должен быть `System.WeakReference` или `GCHandle`.



## <a name="binding-abstract-methods"></a>Привязка абстрактных методов

Методы `abstract` привязки во многом идентичны привязке виртуальных методов. Существует только два отличия:

1.  Абстрактный метод является абстрактным. Он по-прежнему хранит `[Register]` атрибут и связанную с ним регистрацию метода. Привязка метода просто перемещается `Invoker` в тип.

1.  Создается `Invoker` тип, `abstract` не являющийся типом, который подклассировать абстрактный тип. `Invoker` Тип должен переопределять все абстрактные методы, объявленные в базовом классе, а переопределенная реализация — реализацией метода привязки, хотя невиртуальный вариант диспетчеризации можно игнорировать.


Например, предположим, что приведенный `mono.android.test.Adder.add` выше метод `abstract`имел значение. Привязка будет изменена так, `Adder.Add` что была бы абстрактной, `AdderInvoker` и будет определен новый тип, который реализуется `Adder.Add`: C#

```csharp
partial class Adder {
    [Register ("add", "(II)I", "GetAddHandler")]
    public abstract int Add (int a, int b);

    // The Method Registration machinery is identical to the
    // virtual method case...
}

partial class AdderInvoker : Adder {
    public AdderInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }

    static IntPtr id_add;
    public override int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
    }
}
```

Этот `Invoker` тип необходим только при получении ссылок JNI на экземпляры, созданные на языке Java.


## <a name="binding-interfaces"></a>Интерфейсы привязки

Интерфейсы привязки концептуально похожи на классы привязки, содержащие виртуальные методы, но многие особенности отличаются неявными (и не настолько опасными) способами. Рассмотрим следующее [объявление интерфейса Java](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/Adder.java#L14):

```csharp
public interface Progress {
    void onAdd(int[] values, int currentIndex, int currentSum);
}
```

Привязки интерфейсов имеют две части: определение C# интерфейса и определение вызывающего объекта для интерфейса.



### <a name="interface-definition"></a>Определение интерфейса

Определение C# интерфейса должно удовлетворять следующим требованиям.

-   Определение интерфейса должно иметь `[Register]` настраиваемый атрибут.

-   Определение интерфейса должно расширять `IJavaObject interface`.
    Несоблюдение этого действия предотвратит наследование АКВС от интерфейса Java.

-   Каждый метод интерфейса должен содержать `[Register]` атрибут, указывающий соответствующее имя метода Java, сигнатуру JNI и метод соединителя.

-   В методе соединителя также должен быть указан тип, на котором может располагаться метод соединителя.

При `abstract` связывании `virtual` и использовании метода соединителя Поиск выполняется в иерархии наследования регистрируемого типа. Интерфейсы могут не иметь методов, содержащих тексты, поэтому это не работает, поэтому требование указания типа должно указывать, где расположен метод соединителя. Тип указывается в строке метода соединителя после двоеточия `':'`и должен быть полным именем типа сборки типа, содержащего вызывающее средство.

Объявления методов интерфейса — это перевод соответствующего метода Java с использованием *совместимых* типов. Для встроенных типов Java совместимые типы являются C# соответствующими типами, например Java `int` —. C# `int` Для ссылочных типов совместимый тип является типом, который может предоставить JNIный маркер соответствующего типа Java.

Члены интерфейса не будут вызываться напрямую вызовом Java &ndash; с помощью типа &ndash; вызывающего объекта, поэтому разрешена какая-либо гибкость.

Интерфейс выполнения Java можно объявить [в C# следующим образом](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L83):

```csharp
[Register ("mono/android/test/Adder$Progress", DoNotGenerateAcw=true)]
public interface IAdderProgress : IJavaObject {
    [Register ("onAdd", "([III)V",
            "GetOnAddHandler:Mono.Samples.SanityTests.IAdderProgressInvoker, SanityTests, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")]
    void OnAdd (JavaArray<int> values, int currentIndex, int currentSum);
}
```

Обратите внимание, что в приведенном выше `int[]` примере параметр Java сопоставляется с [жавааррай&lt;int&gt;](xref:Android.Runtime.JavaArray`1).
Это не обязательно: можно привязать его к C# `int[]`, или `IList<int>`, или что-то другое. Какой бы тип был выбран, `Invoker` он должен иметь возможность преобразовать его в тип Java `int[]` для вызова.

### <a name="invoker-definition"></a>Определение вызывающего модуля

Определение типа должно наследовать `Java.Lang.Object`, реализовывать соответствующий интерфейс и предоставлять все методы подключения, на которые имеются ссылки в определении интерфейса. `Invoker` Существует еще одно предложение, отличающееся от привязки класса: `class_ref` идентификаторы полей и методов должны быть членами экземпляра, а не статическими элементами.

Причина, по которой члены экземпляра должны выполнять `JNIEnv.GetMethodID` действия в среде выполнения Android. (Это может быть также поведением Java; оно не было протестировано.) `JNIEnv.GetMethodID` возвращает значение null при поиске метода, который поступает из реализованного интерфейса, а не объявленного интерфейса. Рассмотрим интерфейс Java [. util. сортедмап&lt;K, v&gt; ](https://developer.android.com/reference/java/util/SortedMap.html) , который реализует интерфейс [Java. util. Map&lt;k, v.&gt; ](https://developer.android.com/reference/java/util/Map.html) Map предоставляет метод [clear](https://developer.android.com/reference/java/util/Map.html#clear()) , поэтому вполне разумное `Invoker` определение для сортедмап будет выглядеть следующим образом:

```csharp
// Fails at runtime. DO NOT FOLLOW
partial class ISortedMapInvoker : Java.Lang.Object, ISortedMap {
    static IntPtr class_ref = JNIEnv.FindClass ("java/util/SortedMap");
    static IntPtr id_clear;
    public void Clear()
    {
        if (id_clear == IntPtr.Zero)
            id_clear = JNIEnv.GetMethodID(class_ref, "clear", "()V");
        JNIEnv.CallVoidMethod(Handle, id_clear);
    }
     // ...
}
```

Описанный выше `JNIEnv.GetMethodID` `Map.clear` метод завершится ошибкой `null` , так как будет возвращаться при поиске `SortedMap` метода через экземпляр класса.

Существует два решения: отслеживание интерфейса, из которого поступают все методы, и наличие `class_ref` интерфейса для каждого из них, а также выполнение поиска в методе для наиболее производного типа класса, а не типа интерфейса. Последнее выполняется в **Mono. Android. dll**.

Определение вызывающего модуля состоит из шести разделов: конструктора, `Dispose` метода `ThresholdType` , элементов и `ThresholdClass` , `GetObject` метода, реализации метода интерфейса и реализации метода соединителя.



#### <a name="constructor"></a>Конструктор

Конструктору необходимо найти класс среды выполнения вызываемого экземпляра и сохранить класс среды выполнения в поле экземпляра `class_ref` :

```csharp
partial class IAdderProgressInvoker {
    IntPtr class_ref;
    public IAdderProgressInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass (Handle);
        class_ref   = JNIEnv.NewGlobalRef (lref);
        JNIEnv.DeleteLocalRef (lref);
    }
}
```

Примечание. Свойство должно использоваться в теле конструктора, а не в `handle` параметре, как `handle` в Android версии 4.0 параметр может быть недопустимым после завершения выполнения базового конструктора. `Handle`


#### <a name="dispose-method"></a>Метод Dispose

`Dispose` Метод должен освободить глобальную ссылку, выделенную в конструкторе:

```csharp
partial class IAdderProgressInvoker {
    protected override void Dispose (bool disposing)
    {
        if (this.class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef (this.class_ref);
        this.class_ref = IntPtr.Zero;
        base.Dispose (disposing);
    }
}
```


#### <a name="thresholdtype-and-thresholdclass"></a>Срешолдтипе и Срешолдкласс

Элементы `ThresholdType` и`ThresholdClass` идентичны тому, что обнаружено в привязке класса:

```csharp
partial class IAdderProgressInvoker {
    protected override Type ThresholdType {
        get {
            return typeof (IAdderProgressInvoker);
        }
    }
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```


#### <a name="getobject-method"></a>Метод GetObject

Для поддержки `GetObject` [Extensions. жавакаст&lt;T&gt;()](xref:Android.Runtime.Extensions.JavaCast*)требуется статический метод:

```csharp
partial class IAdderProgressInvoker {
    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }
}
```


#### <a name="interface-methods"></a>Методы интерфейса

Каждый метод интерфейса должен иметь реализацию, которая вызывает соответствующий метод Java через JNI:

```csharp
partial class IAdderProgressInvoker {
    IntPtr id_onAdd;
    public void OnAdd (JavaArray<int> values, int currentIndex, int currentSum)
    {
        if (id_onAdd == IntPtr.Zero)
            id_onAdd = JNIEnv.GetMethodID (class_ref, "onAdd", "([III)V");
        JNIEnv.CallVoidMethod (Handle, id_onAdd, new JValue (JNIEnv.ToJniHandle (values)), new JValue (currentIndex), new JValue (currentSum));
    }
}
```



#### <a name="connector-methods"></a>Методы соединителя

Методы соединителя и вспомогательная инфраструктура отвечают за упаковку параметров JNI в соответствующие C# типы. Параметр Java `int[]` будет передан как JNI `jintArray`, который находится `IntPtr` в пределах. C# Для поддержки вызова `JavaArray<int>` C# интерфейса необходимо маршалировать в объект. `IntPtr`

```csharp
partial class IAdderProgressInvoker {
    static Delegate cb_onAdd;
    static Delegate GetOnAddHandler ()
    {
        if (cb_onAdd == null)
            cb_onAdd = JNINativeWrapper.CreateDelegate ((Action<IntPtr, IntPtr, IntPtr, int, int>) n_OnAdd);
        return cb_onAdd;
    }

    static void n_OnAdd (IntPtr jnienv, IntPtr lrefThis, IntPtr values, int currentIndex, int currentSum)
    {
        IAdderProgress __this = Java.Lang.Object.GetObject<IAdderProgress>(lrefThis, JniHandleOwnership.DoNotTransfer);
        using (var _values = new JavaArray<int>(values, JniHandleOwnership.DoNotTransfer)) {
            __this.OnAdd (_values, currentIndex, currentSum);
        }
    }
}
```

Если `int[]` бы было `JavaList<int>`предпочтительнее, вместо этого можно использовать [жниенв. IsArray ()](xref:Android.Runtime.JNIEnv.GetArray*) :

```csharp
int[] _values = (int[]) JNIEnv.GetArray(values, JniHandleOwnership.DoNotTransfer, typeof (int));
```

Однако обратите внимание, `JNIEnv.GetArray` что копирует весь массив между виртуальными машинами, поэтому для больших массивов это может привести к большому увеличению нагрузки на GC.


### <a name="complete-invoker-definition"></a>Полное определение вызывающего модуля

[Полное определение иаддерпрогрессинвокер](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L88):

```csharp
class IAdderProgressInvoker : Java.Lang.Object, IAdderProgress {

    IntPtr class_ref;

    public IAdderProgressInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass (Handle);
        class_ref = JNIEnv.NewGlobalRef (lref);
        JNIEnv.DeleteLocalRef (lref);
    }

    protected override void Dispose (bool disposing)
    {
        if (this.class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef (this.class_ref);
        this.class_ref = IntPtr.Zero;
        base.Dispose (disposing);
    }

    protected override Type ThresholdType {
        get {return typeof (IAdderProgressInvoker);}
    }

    protected override IntPtr ThresholdClass {
        get {return class_ref;}
    }

    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }

#region OnAdd
    IntPtr id_onAdd;
    public void OnAdd (JavaArray<int> values, int currentIndex, int currentSum)
    {
        if (id_onAdd == IntPtr.Zero)
            id_onAdd = JNIEnv.GetMethodID (class_ref, "onAdd",
                    "([III)V");
        JNIEnv.CallVoidMethod (Handle, id_onAdd,
                new JValue (JNIEnv.ToJniHandle (values)),
                new JValue (currentIndex),
new JValue (currentSum));
    }

#pragma warning disable 0169
    static Delegate cb_onAdd;
    static Delegate GetOnAddHandler ()
    {
        if (cb_onAdd == null)
            cb_onAdd = JNINativeWrapper.CreateDelegate ((Action<IntPtr, IntPtr, IntPtr, int, int>) n_OnAdd);
        return cb_onAdd;
    }

    static void n_OnAdd (IntPtr jnienv, IntPtr lrefThis, IntPtr values, int currentIndex, int currentSum)
    {
        IAdderProgress __this = Java.Lang.Object.GetObject<IAdderProgress>(lrefThis, JniHandleOwnership.DoNotTransfer);
        using (var _values = new JavaArray<int>(values, JniHandleOwnership.DoNotTransfer)) {
            __this.OnAdd (_values, currentIndex, currentSum);
        }
    }
#pragma warning restore 0169
#endregion
}
```

## <a name="jni-object-references"></a>Ссылки на объекты JNI

Многие методы жниенв возвращают *ссылки на объекты* *JNI* , которые похожи `GCHandle`на s. JNI предоставляет три различных типа ссылок на объекты: локальные ссылки, глобальные ссылки и слабые глобальные ссылки. Все три представляются как `System.IntPtr`, *но* (в соответствии с разделом типов функций JNI) `IntPtr`не `JNIEnv` все возвращенные методы являются ссылками. Например, [жниенв. жетмесодид](xref:Android.Runtime.JNIEnv.GetMethodID*) возвращает `IntPtr`, но не возвращает ссылку на объект `jmethodID`, он возвращает. Дополнительные сведения см. в [документации по функции JNI](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html) .

Локальные ссылки создаются большинством методов создания ссылок.
Android допускает существование ограниченного числа локальных ссылок в любое заданное время, обычно 512. Локальные ссылки можно удалить с помощью [жниенв. делетелокалреф](xref:Android.Runtime.JNIEnv.DeleteLocalRef*).
В отличие от JNI, не все методы Reference Жниенв, возвращающие ссылки на объекты, возвращают локальные ссылки. [Жниенв. FindClass](xref:Android.Runtime.JNIEnv.FindClass*) возвращает *глобальную* ссылку. Настоятельно рекомендуется удалять локальные ссылки как можно быстрее, возможно, путем создания [объекта Java. lang. Object](xref:Java.Lang.Object) вокруг объекта и указания `JniHandleOwnership.TransferLocalRef` в [Java. lang. Object (дескриптор IntPtr, перемещение жнихандлеовнершип). ](xref:Java.Lang.Object#ctor*)конструктор.

Глобальные ссылки создаются с помощью [жниенв. невглобалреф](xref:Android.Runtime.JNIEnv.NewGlobalRef*) и [жниенв. FindClass](xref:Android.Runtime.JNIEnv.FindClass*).
Их можно уничтожить с помощью [жниенв. делетеглобалреф](xref:Android.Runtime.JNIEnv.DeleteGlobalRef*).
Эмуляторы имеют ограничение в 2 000 необработанных глобальных ссылок, в то время как устройства имеют ограничение в около 52 000 глобальных ссылок.

Слабые глобальные ссылки доступны только в Android v 2.2 (Фройо) и более поздних версиях. Слабые глобальные ссылки можно удалить с помощью [жниенв. делетевеакглобалреф](xref:Android.Runtime.JNIEnv.DeleteWeakGlobalRef*).

### <a name="dealing-with-jni-local-references"></a>Работа с локальными ссылками JNI

Методы [жниенв. жетобжектфиелд](xref:Android.Runtime.JNIEnv.GetObjectField*), [жниенв. жетстатикобжектфиелд](xref:Android.Runtime.JNIEnv.GetStaticObjectField*), [жниенв. каллобжектмесод](xref:Android.Runtime.JNIEnv.CallObjectMethod*), [жниенв. каллнонвиртуалобжектмесод](xref:Android.Runtime.JNIEnv.CallNonvirtualObjectMethod*) `IntPtr` и [JNIEnv. CallStaticObjectMethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) возвращают, какой Содержит локальную ссылку JNI на объект Java или `IntPtr.Zero` значение, если был возвращен `null`язык Java. Из-за ограниченного количества локальных ссылок, которые можно исключать одновременно (512 записей), желательно обеспечить своевременное удаление ссылок. Существует три способа, с помощью которых локальные ссылки могут быть обработаны: явно удалить их `Java.Lang.Object` , создать экземпляр для их хранения и `Java.Lang.Object.GetObject<T>()` использовать для создания управляемой вызываемой обертки вокруг них.



### <a name="explicitly-deleting-local-references"></a>Явное удаление локальных ссылок

[Жниенв. делетелокалреф](xref:Android.Runtime.JNIEnv.DeleteLocalRef*) используется для удаления локальных ссылок. После удаления локальной ссылки она больше не может использоваться, поэтому необходимо соблюдать осторожность, чтобы убедиться, что `JNIEnv.DeleteLocalRef` это последняя задача, выполняемая с локальной ссылкой.

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
try {
    // Do something with `lref`
}
finally {
    JNIEnv.DeleteLocalRef (lref);
}
```

### <a name="wrapping-with-javalangobject"></a>Заключение в оболочку с помощью Java. lang. Object

`Java.Lang.Object`предоставляет конструктор [Java. lang. Object (дескриптор IntPtr, жнихандлеовнершип-перемещение)](xref:Java.Lang.Object#ctor*) , который можно использовать для создания оболочки для выхода из JNI ссылки. Параметр [жнихандлеовнершип](xref:Android.Runtime.JniHandleOwnership) определяет, `IntPtr` как должен обрабатываться параметр:

-   [Жнихандлеовнершип. доноттрансфер](xref:Android.Runtime.JniHandleOwnership.DoNotTransfer) &ndash; . созданный `Java.Lang.Object` экземпляр создаст новую глобальную ссылку из `handle` параметра и `handle` не изменится.
    Вызывающий объект отвечает за освобождение `handle` , если это необходимо.

-   [Жнихандлеовнершип. трансферлокалреф](xref:Android.Runtime.JniHandleOwnership.TransferLocalRef) &ndash; . созданный `Java.Lang.Object` экземпляр создаст новую глобальную ссылку из `handle` параметра и `handle` будет удален с помощью [жниенв. делетелокалреф](xref:Android.Runtime.JNIEnv.DeleteLocalRef*) . Вызывающий объект не должен `handle` быть освобожден и не должен `handle` использоваться после завершения выполнения конструктора.

-   [Жнихандлеовнершип. трансферглобалреф](xref:Android.Runtime.JniHandleOwnership.TransferLocalRef) &ndash; `Java.Lang.Object` созданный`handle` экземпляр принимает владение параметром. Вызывающий объект не должен `handle` быть бесплатным.


Так как методы вызова метода JNI возвращают локальные ссылки, `JniHandleOwnership.TransferLocalRef` обычно используется:

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef);
```

Созданная Глобальная ссылка не будет освобождена, `Java.Lang.Object` пока экземпляр не будет удален сборщиком мусора. Если у вас есть возможность, удаление экземпляра приведет к освобождению глобальной ссылки, что ускоряет сборку мусора:

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
using (var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef)) {
    // use value ...
}
```

### <a name="using-javalangobjectgetobjectlttgt"></a>Использование Java. lang. Object.&lt;GetObject T&gt;()

`Java.Lang.Object`предоставляет метод [Java&lt;. lang. Object. GetObject T&gt;(дескриптор IntPtr, перенаправление жнихандлеовнершип)](xref:Java.Lang.Object.GetObject*) , который можно использовать для создания управляемой вызываемой обертки указанного типа.

Тип `T` должен соответствовать следующим требованиям.

1.  `T`должен быть ссылочным типом.

1.  `T`должен реализовывать `IJavaObject` интерфейс.

1.  Если `T` не является абстрактным классом или интерфейсом `T` , необходимо предоставить конструктор с типами `(IntPtr,
    JniHandleOwnership)` параметров.

1.  Если `T` является абстрактным классом или интерфейсом, *должно* быть доступно средство *вызова* для `T` . Вызывающий объект — это не абстрактный тип, который `T` наследует `T` или реализует, и имеет то же `T` имя, что и суффикс вызывающего. Например, если T является интерфейсом `Java.Lang.IRunnable` , тип `Java.Lang.IRunnableInvoker` должен существовать и должен содержать требуемый `(IntPtr,
    JniHandleOwnership)` конструктор.


Так как методы вызова метода JNI возвращают локальные ссылки, `JniHandleOwnership.TransferLocalRef` обычно используется:

```csharp
IntPtr lrefString = JNIEnv.CallObjectMethod(instance, methodID);
Java.Lang.String value = Java.Lang.Object.GetObject<Java.Lang.String>( lrefString, JniHandleOwnership.TransferLocalRef);
```

<a name="_Looking_up_Java_Types" />

## <a name="looking-up-java-types"></a>Поиск типов Java

Чтобы найти поле или метод в JNI, необходимо сначала выполнить объявление типа для поля или метода. Метод [Android. Runtime. жниенв. FindClass (строка)](xref:Android.Runtime.JNIEnv.FindClass*)) используется для поиска типов Java. Параметр String представляет собой *упрощенную ссылку на тип* или *полную ссылку на тип* для типа Java. Дополнительные сведения о упрощенных и полных ссылках на типы см. в [разделе JNI Type References](#_JNI_Type_References) .

Примечание. В отличие от любого `JNIEnv` другого метода, который возвращает экземпляры `FindClass` объекта, возвращает глобальную ссылку, а не локальную ссылку.

<a name="_Instance_Fields" />

## <a name="instance-fields"></a>Поля экземпляра

Для управления полями используются *идентификаторы полей*. Идентификаторы полей получаются через [жниенв. жетфиелдид](xref:Android.Runtime.JNIEnv.GetFieldID*), для которого требуется класс, в котором определено поле, имя поля и [сигнатура типа JNI](#JNI_Type_Signatures) поля.

Идентификаторы полей не обязательно освобождаются и являются допустимыми, пока загружается соответствующий тип Java. (В настоящее время Android не поддерживает выгрузку класса.)

Существует два набора методов для управления полями экземпляров: один для чтения полей экземпляра и один для записи полей экземпляра. Всем наборам методов требуется идентификатор поля для чтения или записи значения поля.

### <a name="reading-instance-field-values"></a>Считывание значений полей экземпляра

Набор методов для чтения значений полей экземпляров соответствует шаблону именования:

```csharp
* JNIEnv.Get*Field(IntPtr instance, IntPtr fieldID);
```

где `*` — это тип поля:

-   [Жниенв. жетобжектфиелд](xref:Android.Runtime.JNIEnv.GetObjectField*) &ndash; считывает значение любого поля экземпляра, не встроенного типа, такого как `java.lang.Object` , массивы и типы интерфейса. Возвращаемое значение — это локальная ссылка JNI.

-   [Жниенв.](xref:Android.Runtime.JNIEnv.GetBooleanField*) &ndash; жетбулеанфиелд`bool` считывает значение полей экземпляра.

-   [Жниенв.](xref:Android.Runtime.JNIEnv.GetByteField*) &ndash; жетбитефиелд`sbyte` считывает значение полей экземпляра.

-   [Жниенв.](xref:Android.Runtime.JNIEnv.GetCharField*) &ndash; жетчарфиелд`char` считывает значение полей экземпляра.

-   [Жниенв.](xref:Android.Runtime.JNIEnv.GetShortField*) &ndash; жетшортфиелд`short` считывает значение полей экземпляра.

-   [Жниенв.](xref:Android.Runtime.JNIEnv.GetIntField*) &ndash; жетинтфиелд`int` считывает значение полей экземпляра.

-   [Жниенв.](xref:Android.Runtime.JNIEnv.GetLongField*) &ndash; жетлонгфиелд`long` считывает значение полей экземпляра.

-   [Жниенв.](xref:Android.Runtime.JNIEnv.GetFloatField*) &ndash; жетфлоатфиелд`float` считывает значение полей экземпляра.

-   [Жниенв.](xref:Android.Runtime.JNIEnv.GetDoubleField*) &ndash; жетдаублефиелд`double` считывает значение полей экземпляра.

### <a name="writing-instance-field-values"></a>Запись значений полей экземпляра

Набор методов для записи значений полей экземпляров соответствует шаблону именования:

```csharp
JNIEnv.SetField(IntPtr instance, IntPtr fieldID, Type value);
```

где *Type* — это тип поля:

-   [Жниенв. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; записывает значение любого поля, не встроенного типа, такого как `java.lang.Object` , массивы и типы интерфейсов. Значением может быть локальная ссылка на JNI, JNI Global Reference, JNI слабая Глобальная ссылка или `IntPtr.Zero` (для `null` ). `IntPtr`

-   [Жниенв. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `bool` записывает значения полей экземпляров.

-   [Жниенв. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `sbyte` записывает значения полей экземпляров.

-   [Жниенв. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `char` записывает значения полей экземпляров.

-   [Жниенв. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `short` записывает значения полей экземпляров.

-   [Жниенв. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `int` записывает значения полей экземпляров.

-   [Жниенв. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `long` записывает значения полей экземпляров.

-   [Жниенв. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `float` записывает значения полей экземпляров.

-   [Жниенв. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `double` записывает значения полей экземпляров.

<a name="_Static_Fields" />

## <a name="static-fields"></a>Статические поля

Статические поля обрабатываются с помощью *идентификаторов полей*. Идентификаторы полей получаются через [жниенв. жетстатикфиелдид](xref:Android.Runtime.JNIEnv.GetStaticFieldID*), для которого требуется класс, в котором определено поле, имя поля и [сигнатура типа JNI](#JNI_Type_Signatures) поля.

Идентификаторы полей не обязательно освобождаются и являются допустимыми, пока загружается соответствующий тип Java. (В настоящее время Android не поддерживает выгрузку класса.)

Существует два набора методов для управления статическими полями: один для чтения полей экземпляра и один для записи полей экземпляра. Всем наборам методов требуется идентификатор поля для чтения или записи значения поля.

### <a name="reading-static-field-values"></a>Считывание значений статических полей

Набор методов для чтения статических значений полей соответствует шаблону именования:

```csharp
* JNIEnv.GetStatic*Field(IntPtr class, IntPtr fieldID);
```

где `*` — это тип поля:

-   [Жниенв. жетстатикобжектфиелд](xref:Android.Runtime.JNIEnv.GetStaticObjectField*) &ndash; считывает значение любого статического поля, не встроенного типа, такого как `java.lang.Object` , массивы и типы интерфейса. Возвращаемое значение — это локальная ссылка JNI.

-   [Жниенв.](xref:Android.Runtime.JNIEnv.GetStaticBooleanField*) &ndash; жетстатикбулеанфиелд`bool` считывает значение статических полей.

-   [Жниенв.](xref:Android.Runtime.JNIEnv.GetStaticByteField*) &ndash; жетстатикбитефиелд`sbyte` считывает значение статических полей.

-   [Жниенв.](xref:Android.Runtime.JNIEnv.GetStaticCharField*) &ndash; жетстатикчарфиелд`char` считывает значение статических полей.

-   [Жниенв.](xref:Android.Runtime.JNIEnv.GetStaticShortField*) &ndash; жетстатикшортфиелд`short` считывает значение статических полей.

-   [Жниенв.](xref:Android.Runtime.JNIEnv.GetStaticLongField*) &ndash; жетстатиклонгфиелд`long` считывает значение статических полей.

-   [Жниенв.](xref:Android.Runtime.JNIEnv.GetStaticFloatField*) &ndash; жетстатикфлоатфиелд`float` считывает значение статических полей.

-   [Жниенв.](xref:Android.Runtime.JNIEnv.GetStaticDoubleField*) &ndash; жетстатикдаублефиелд`double` считывает значение статических полей.

### <a name="writing-static-field-values"></a>Запись статических значений полей

Набор методов для записи статических значений полей следует шаблону именования:

```csharp
JNIEnv.SetStaticField(IntPtr class, IntPtr fieldID, Type value);
```

где *Type* — это тип поля:

-   [Жниенв. сетстатикфиелд](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; записывает значение любого статического поля, не встроенного типа, такого как `java.lang.Object` , массивы и типы интерфейса. Значением может быть локальная ссылка на JNI, JNI Global Reference, JNI слабая Глобальная ссылка или `IntPtr.Zero` (для `null` ). `IntPtr`

-   [Жниенв. сетстатикфиелд](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `bool` записывает значения статических полей.

-   [Жниенв. сетстатикфиелд](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `sbyte` записывает значения статических полей.

-   [Жниенв. сетстатикфиелд](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `char` записывает значения статических полей.

-   [Жниенв. сетстатикфиелд](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `short` записывает значения статических полей.

-   [Жниенв. сетстатикфиелд](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `int` записывает значения статических полей.

-   [Жниенв. сетстатикфиелд](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `long` записывает значения статических полей.

-   [Жниенв. сетстатикфиелд](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `float` записывает значения статических полей.

-   [Жниенв. сетстатикфиелд](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `double` записывает значения статических полей.


<a name="_Instance_Methods" />

## <a name="instance-methods"></a>Методы экземпляра

Методы экземпляра вызываются через *идентификаторы методов*. Идентификаторы методов получаются через [жниенв. жетмесодид](xref:Android.Runtime.JNIEnv.GetMethodID*), для которого требуется тип, в котором определен метод, имя метода и [сигнатура типа JNI](#JNI_Type_Signatures) метода.

Идентификаторы методов не обязательно освобождаются и являются допустимыми, пока загружается соответствующий тип Java. (В настоящее время Android не поддерживает выгрузку класса.)

Существует два набора методов для вызова методов: один для вызова методов практически и один для вызова методов, которые не являются виртуальными. Оба набора методов требуют идентификатора метода для вызова метода, а Невиртуальный вызов также требует указания того, какая реализация класса должна быть вызвана.

Методы интерфейса можно искать только внутри объявляющего типа. методы, поступающие из расширенных или наследуемых интерфейсов, не могут быть просмотрены. Дополнительные сведения см. в разделе Дополнительные интерфейсы привязки и реализация вызывающего вызова.

Возможен поиск любого метода, объявленного в классе или любом базовом классе или реализованном интерфейсе.

### <a name="virtual-method-invocation"></a>Вызов виртуального метода

Набор методов для вызова методов практически соответствует шаблону именования:

```csharp
* JNIEnv.Call*Method( IntPtr instance, IntPtr methodID, params JValue[] args );
```

где `*` — возвращаемый тип метода.

-   [Жниенв. каллобжектмесод](xref:Android.Runtime.JNIEnv.CallObjectMethod*) &ndash; вызывает метод, который возвращает невстроенный `java.lang.Object` тип, например, массивы и интерфейсы. Возвращаемое значение — это локальная ссылка JNI.

-   [Жниенв. каллбулеанмесод](xref:Android.Runtime.JNIEnv.CallBooleanMethod*) &ndash; вызывает `bool` метод, возвращающий значение.

-   [Жниенв. каллбитемесод](xref:Android.Runtime.JNIEnv.CallByteMethod*) &ndash; вызывает `sbyte` метод, возвращающий значение.

-   [Жниенв. каллчармесод](xref:Android.Runtime.JNIEnv.CallCharMethod*) &ndash; вызывает `char` метод, возвращающий значение.

-   [Жниенв. каллшортмесод](xref:Android.Runtime.JNIEnv.CallShortMethod*) &ndash; вызывает `short` метод, возвращающий значение.

-   [Жниенв. калллонгмесод](xref:Android.Runtime.JNIEnv.CallLongMethod*) &ndash; вызывает `long` метод, возвращающий значение.

-   [Жниенв. каллфлоатмесод](xref:Android.Runtime.JNIEnv.CallFloatMethod*) &ndash; вызывает `float` метод, возвращающий значение.

-   [Жниенв. каллдаублемесод](xref:Android.Runtime.JNIEnv.CallDoubleMethod*) &ndash; вызывает `double` метод, возвращающий значение.

### <a name="non-virtual-method-invocation"></a>Вызов невиртуального метода

Набор методов для вызова методов, которые не являются виртуальными, следует шаблону именования:

```csharp
* JNIEnv.CallNonvirtual*Method( IntPtr instance, IntPtr class, IntPtr methodID, params JValue[] args );
```

где `*` — возвращаемый тип метода. Вызов метода, не являющегося виртуальным, обычно используется для вызова базового метода виртуального метода.

-   [Жниенв. каллнонвиртуалобжектмесод](xref:Android.Runtime.JNIEnv.CallNonvirtualObjectMethod*) &ndash; — Невиртуальный вызов метода, который возвращает невстроенный `java.lang.Object` тип, например, массивы и интерфейсы. Возвращаемое значение — это локальная ссылка JNI.

-   [Жниенв. каллнонвиртуалбулеанмесод](xref:Android.Runtime.JNIEnv.CallNonvirtualBooleanMethod*) &ndash; — Невиртуальный вызов `bool` метода, возвращающего значение.

-   [Жниенв. каллнонвиртуалбитемесод](xref:Android.Runtime.JNIEnv.CallNonvirtualByteMethod*) &ndash; — Невиртуальный вызов `sbyte` метода, возвращающего значение.

-   [Жниенв. каллнонвиртуалчармесод](xref:Android.Runtime.JNIEnv.CallNonvirtualCharMethod*) &ndash; — Невиртуальный вызов `char` метода, возвращающего значение.

-   [Жниенв. каллнонвиртуалшортмесод](xref:Android.Runtime.JNIEnv.CallNonvirtualShortMethod*) &ndash; — Невиртуальный вызов `short` метода, возвращающего значение.

-   [Жниенв. каллнонвиртуаллонгмесод](xref:Android.Runtime.JNIEnv.CallNonvirtualLongMethod*) &ndash; — Невиртуальный вызов `long` метода, возвращающего значение.

-   [Жниенв. каллнонвиртуалфлоатмесод](xref:Android.Runtime.JNIEnv.CallNonvirtualFloatMethod*) &ndash; — Невиртуальный вызов `float` метода, возвращающего значение.

-   [Жниенв. каллнонвиртуалдаублемесод](xref:Android.Runtime.JNIEnv.CallNonvirtualDoubleMethod*) &ndash; — Невиртуальный вызов `double` метода, возвращающего значение.

<a name="_Static_Methods" />

## <a name="static-methods"></a>Статические методы

Статические методы вызываются через *идентификаторы методов*. Идентификаторы методов получаются через [жниенв. жетстатикмесодид](xref:Android.Runtime.JNIEnv.GetStaticMethodID*), для которого требуется тип, в котором определен метод, имя метода и [сигнатура типа JNI](#JNI_Type_Signatures) метода.

Идентификаторы методов не обязательно освобождаются и являются допустимыми, пока загружается соответствующий тип Java. (В настоящее время Android не поддерживает выгрузку класса.)

### <a name="static-method-invocation"></a>Вызов статического метода

Набор методов для вызова методов практически соответствует шаблону именования:

```csharp
* JNIEnv.CallStatic*Method( IntPtr class, IntPtr methodID, params JValue[] args );
```

где `*` — возвращаемый тип метода.

-   [Жниенв. каллстатикобжектмесод](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) &ndash; вызывает статический метод, который возвращает невстроенный `java.lang.Object` тип, например, массивы и интерфейсы. Возвращаемое значение — это локальная ссылка JNI.

-   [Жниенв. каллстатикбулеанмесод](xref:Android.Runtime.JNIEnv.CallStaticBooleanMethod*) &ndash; вызывает статический `bool` метод, который возвращает значение.

-   [Жниенв. каллстатикбитемесод](xref:Android.Runtime.JNIEnv.CallStaticByteMethod*) &ndash; вызывает статический `sbyte` метод, который возвращает значение.

-   [Жниенв. каллстатикчармесод](xref:Android.Runtime.JNIEnv.CallStaticCharMethod*) &ndash; вызывает статический `char` метод, который возвращает значение.

-   [Жниенв. каллстатикшортмесод](xref:Android.Runtime.JNIEnv.CallStaticShortMethod*) &ndash; вызывает статический `short` метод, который возвращает значение.

-   [Жниенв. каллстатиклонгмесод](xref:Android.Runtime.JNIEnv.CallLongMethod*) &ndash; вызывает статический `long` метод, который возвращает значение.

-   [Жниенв. каллстатикфлоатмесод](xref:Android.Runtime.JNIEnv.CallStaticFloatMethod*) &ndash; вызывает статический `float` метод, который возвращает значение.

-   [Жниенв. каллстатикдаублемесод](xref:Android.Runtime.JNIEnv.CallStaticDoubleMethod*) &ndash; вызывает статический `double` метод, который возвращает значение.

<a name="JNI_Type_Signatures" />

## <a name="jni-type-signatures"></a>Сигнатуры типа JNI

[Сигнатуры типа JNI](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/types.html#wp16432) — это [ссылки на типы JNI](#_JNI_Type_References) (хотя и неупрощенные ссылки на типы), за исключением методов. В методах сигнатура типа JNI является открывающей круглой `'('`скобкой, за которой следуют ссылки на типы для всех типов параметров, Объединенных вместе (без разделителей запятый или что-либо еще), за которыми следует `')'`закрывающая круглая скобка, , за которым следует ссылка на тип возвращаемого значения метода JNI.

Например, при наличии метода Java:

```java
long f(int n, String s, int[] array);
```

Сигнатура типа JNI будет выглядеть следующим образом:

```csharp
(ILjava/lang/String;[I)J
```

Как правило, для определения подписей JNI *настоятельно* рекомендуется `javap` использовать команду. Например, сигнатура типа JNI для метода [Java. lang. Thread. штат. valueOf (String)](https://developer.android.com/reference/java/lang/Thread.State.html#valueOf(java.lang.String)) — это "(лжава/lang/String;) лжава/lang/Thread $ State;", а сигнатура типа JNI для метода [Java. lang. Thread. штат. Values](https://developer.android.com/reference/java/lang/Thread.State.html#values) — "() [лжава/ lang/поток $ State; ". Наблюдайте за точкой с запятой в конце; они *являются* частью сигнатуры типа JNI.

<a name="_JNI_Type_References" />

## <a name="jni-type-references"></a>Ссылки на тип JNI

Ссылки на типы JNI отличаются от ссылок на типы Java. Нельзя использовать полные имена типов Java, например `java.lang.String` with JNI, вместо них следует использовать варианты `"java/lang/String"` JNI или `"Ljava/lang/String;"`, в зависимости от контекста; дополнительные сведения см. ниже.
Существует четыре типа ссылок на типы JNI:

- **встроенные**
- **механизм**
- **type**
- **array**

### <a name="built-in-type-references"></a>Ссылки на встроенные типы

Ссылки на встроенный тип — это один символ, используемый для ссылки на встроенные типы значений. Сопоставление выглядит следующим образом:

- `"B"`для `sbyte` .
- `"S"`для `short` .
- `"I"`для `int` .
- `"J"`для `long` .
- `"F"`для `float` .
- `"D"`для `double` .
- `"C"`для `char` .
- `"Z"`для `bool` .
- `"V"`для `void` возвращаемых типов методов.

<a name="_Simplified_Type_References_1" />

### <a name="simplified-type-references"></a>Упрощенные ссылки на типы

Упрощенные ссылки на типы могут использоваться только в [жниенв. FindClass (String)](xref:Android.Runtime.JNIEnv.FindClass*)).
Существует два способа получить упрощенную ссылку на тип:

1.  Из полного `'.'` имени Java замените каждую из имен пакета и перед именем типа с помощью `'/'` , а каждый `'.'` в имени `'$'` типа на.

1.  Считывает выходные данные `'unzip -l android.jar | grep JavaName'` .

Любой из этих двух типов приведет к тому, что Java [. lang. Thread. State](https://developer.android.com/reference/java/lang/Thread.State.html) будет сопоставляться с упрощенной ссылкой `java/lang/Thread$State`на тип.

### <a name="type-references"></a>Ссылки на типы

Ссылка на тип — это встроенная ссылка на тип или упрощенная ссылка на тип с `'L'` префиксом `';'` и суффиксом. Для типа Java [Java. lang. String](https://developer.android.com/reference/java/lang/String.html)упрощена ссылка `"java/lang/String"`на тип, а ссылка на тип —. `"Ljava/lang/String;"`

Ссылки на типы используются с ссылками на типы массивов и с сигнатурами JNI.

Чтобы получить ссылку на тип, можно прочитать выходные данные `'javap -s -classpath android.jar fully.qualified.Java.Name'`.
В зависимости от применяемого типа можно использовать объявление конструктора или возвращаемый тип метода для определения имени JNI. Например:

```shell
$ javap -classpath android.jar -s java.lang.Thread.State
Compiled from "Thread.java"
```

```java
public final class java.lang.Thread$State extends java.lang.Enum{
public static final java.lang.Thread$State NEW;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State RUNNABLE;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State BLOCKED;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State WAITING;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State TIMED_WAITING;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State TERMINATED;
  Signature: Ljava/lang/Thread$State;
public static java.lang.Thread$State[] values();
  Signature: ()[Ljava/lang/Thread$State;
public static java.lang.Thread$State valueOf(java.lang.String);
  Signature: (Ljava/lang/String;)Ljava/lang/Thread$State;
static {};
  Signature: ()V
}
```

`Thread.State`является типом Enum Java, поэтому мы можем использовать сигнатуру `valueOf` метода, чтобы определить, что ссылка на тип — лжава/lang/Thread $ State;.

### <a name="array-type-references"></a>Ссылки на типы массивов

Ссылки на типы массивов добавляются `'['` в ссылку на тип JNI.
Упрощенные ссылки на типы не могут использоваться при указании массивов.

`int[]` Например ,`"[I"`имеет, имеет`"[[I"`, и`java.lang.Object[]` имеет .`"[Ljava/lang/Object;"` `int[][]`

## <a name="java-generics-and-type-erasure"></a>Универсальные шаблоны Java и тип очистки

В *большинстве случаев* , как ВИДНО через JNI, универсальные шаблоны Java *не существуют*.
Есть некоторые «вринклес», но эти вринклес в том, как Java взаимодействует с универсальными шаблонами, а не с тем, как JNI ищет и вызывает универсальные члены.

При взаимодействии с JNI не существует разницы между универсальным типом или членом и неуниверсальным типом или членом. Например, универсальный тип [Java. lang.&lt;Class T&gt; ](https://developer.android.com/reference/java/lang/Class.html) также является необработанным универсальным типом `java.lang.Class`, `"java/lang/Class"`оба из которых имеют одинаковую упрощенную ссылку на тип.

## <a name="java-native-interface-support"></a>Поддержка собственного интерфейса Java

[Android. Runtime. жниенв](xref:Android.Runtime.JNIEnv) — это управляемая оболочка для собственного интерфейса ЖАВЕ (JNI). Функции JNI объявляются в [спецификации собственного интерфейса Java](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html), хотя методы были изменены для удаления `JNIEnv*` явного параметра и `jclass` `IntPtr` используются вместо `jobject`,, `jmethodID`, 123. Например, рассмотрим [функцию JNI NewObject](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp4517):

```csharp
jobject NewObjectA(JNIEnv *env, jclass clazz, jmethodID methodID, jvalue *args);
```

Он предоставляется как метод [жниенв. NewObject](xref:Android.Runtime.JNIEnv.NewObject*) :

```csharp
public static IntPtr NewObject(IntPtr clazz, IntPtr jmethod, params JValue[] parms);
```

Преобразование между двумя вызовами достаточно просто. В C у вас будет:

```c
jobject CreateMapActivity(JNIEnv *env)
{
    jclass    Map_Class   = (*env)->FindClass(env, "mono/samples/googlemaps/MyMapActivity");
    jmethodID Map_defCtor = (*env)->GetMethodID (env, Map_Class, "<init>", "()V");
    jobject   instance    = (*env)->NewObject (env, Map_Class, Map_defCtor);

    return instance;
}
```

C# Эквивалент будет следующим:

```csharp
IntPtr CreateMapActivity()
{
    IntPtr Map_Class   = JNIEnv.FindClass ("mono/samples/googlemaps/MyMapActivity");
    IntPtr Map_defCtor = JNIEnv.GetMethodID (Map_Class, "<init>", "()V");
    IntPtr instance    = JNIEnv.NewObject (Map_Class, Map_defCtor);

    return instance;
}
```

При наличии экземпляра объекта Java, хранящегося в IntPtr, вы, вероятно, захотите сделать что-то с ним. Для этого можно использовать методы Жниенв, такие как [жниенв. каллвоидмесод ()](xref:Android.Runtime.JNIEnv.CallVoidMethod*) , но если уже есть аналоговая C# оболочка, необходимо создать обертку для ссылки JNI. Это можно сделать с помощью метода расширения [Extension.&lt;жавакаст T >](xref:Android.Runtime.Extensions.JavaCast*) :

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = new Java.Lang.Object(lrefActivity, JniHandleOwnership.TransferLocalRef)
    .JavaCast<Activity>();
```

Можно также использовать метод [Java. lang. Object.&lt;GetObject T >](xref:Java.Lang.Object.GetObject*) :

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = Java.Lang.Object.GetObject<Activity>(lrefActivity, JniHandleOwnership.TransferLocalRef);
```

Кроме того, все функции JNI были изменены путем удаления `JNIEnv*` параметра, присутствующего в каждой функции JNI.

## <a name="summary"></a>Сводка

Непосредственная работа с JNI — это плохойная работа, которую следует избегать при любых затратах. К сожалению, избежать этого не всегда. надеюсь, в этом руководством вы получите помощь, если вы достигли несвязанных вариантов Java с Mono для Android.

## <a name="related-links"></a>Связанные ссылки

- [Спецификация собственного интерфейса Java](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/jniTOC.html)
- [Собственные функции интерфейса Java](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)
