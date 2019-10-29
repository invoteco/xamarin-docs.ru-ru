---
title: Вызываемые оболочки Android для Xamarin. Android
ms.prod: xamarin
ms.assetid: C33E15FA-1E2B-819A-C656-CA588D611492
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: 7278fd624bb3147c2e1a1a1a79adde68813a9888
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020150"
---
# <a name="android-callable-wrappers-for-xamarinandroid"></a>Вызываемые оболочки Android для Xamarin. Android

Вызываемые оболочки Android (АКВС) требуются всякий раз, когда среда выполнения Android вызывает управляемый код. Эти оболочки являются обязательными, так как в среде выполнения не существует способа регистрации классов с помощью графики (среда выполнения Android). (В частности, [функция JNI дефинекласс ()](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp15986) не поддерживается средой выполнения Android.} Таким же вызываемые оболочки Android составляют отсутствие поддержки регистрации типов во время выполнения. 

*Каждый раз* Код Android должен выполнить `virtual` или метод интерфейса, который `overridden` или реализован в управляемом коде, Xamarin. Android должен предоставить прокси-сервер Java, чтобы этот метод был отправлен в соответствующий управляемый тип. Эти типы прокси Java — это код Java, имеющий тот же базовый класс и список интерфейсов Java, что и управляемый тип, реализующий те же конструкторы и объявляющие переопределенный базовый класс и методы интерфейса. 

Вызываемые оболочки Android создаются программой **monodroid. exe** во время [процесса сборки](~/android/deploy-test/building-apps/build-process.md): они формируются для всех типов, которые (прямо или косвенно) наследуют [Java. lang. Object](xref:Java.Lang.Object). 

## <a name="android-callable-wrapper-naming"></a>Имя вызываемой оболочки Android

Имена пакетов для вызываемых оболочек Android основаны на MD5SUM имени экспортируемого типа с указанием сборки. Такая методика именования позволяет сделать то же полное имя типа доступным для разных сборок, не вводя ошибку упаковки. 

Из-за этой схемы именования MD5SUM вы не можете напрямую обращаться к типам по имени. Например, следующая `adb`ная команда не будет работать, так как имя типа `my.ActivityType` не создается по умолчанию: 

```shell
adb shell am start -n My.Package.Name/my.ActivityType
```

Кроме того, при попытке ссылки на тип по имени могут отобразиться следующие ошибки:

```shell
java.lang.ClassNotFoundException: Didn't find class "com.company.app.MainActivity"
on path: DexPathList[[zip file "/data/app/com.company.App-1.apk"] ...
```

Если требуется *доступ* к типам по имени, можно объявить имя для этого типа в объявлении атрибута. Например, ниже приведен код, объявляющий действие с полным именем `My.ActivityType`:

```csharp
namespace My {
    [Activity]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

Для свойства `ActivityAttribute.Name` можно задать явное объявление имени этого действия: 

```csharp
namespace My {
    [Activity(Name="my.ActivityType")]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

После добавления этого параметра свойства `my.ActivityType` к нему можно получить доступ по имени из внешнего кода и из сценариев `adb`. Атрибут `Name` можно задать для множества различных типов, включая `Activity`, `Application`, `Service`, `BroadcastReceiver`и `ContentProvider`. 

- [ActivityAttribute.Name](xref:Android.App.ActivityAttribute.Name)
- [ApplicationAttribute.Name](xref:Android.App.ApplicationAttribute.Name)
- [ServiceAttribute.Name](xref:Android.App.ServiceAttribute.Name)
- [BroadcastReceiverAttribute.Name](xref:Android.Content.BroadcastReceiverAttribute.Name)
- [ContentProviderAttribute.Name](xref:Android.Content.ContentProviderAttribute.Name)

Именование АКВ на основе MD5SUM было введено в Xamarin. Android 5,0. Дополнительные сведения об именовании атрибутов см. в разделе [регистераттрибуте](xref:Android.Runtime.RegisterAttribute). 

## <a name="implementing-interfaces"></a>Реализация интерфейсов

Иногда может потребоваться реализовать интерфейс Android, например [Android. Content. икомпоненткаллбаккс](xref:Android.Content.IComponentCallbacks). Поскольку все классы и интерфейс Android расширяют интерфейс [Android. Runtime. ижаваобжект](xref:Android.Runtime.IJavaObject) , возникает вопрос: как реализовать `IJavaObject`? 

Был получен ответ на вопрос. Причина, по которой все типы Android должны реализовать `IJavaObject` так, чтобы Xamarin. Android имел вызываемую оболочку Android для предоставления в Android, т. е. прокси-сервер Java для данного типа. Поскольку **monodroid. exe** ищет только подклассы `Java.Lang.Object`, а `Java.Lang.Object` реализует `IJavaObject,` ответ очевидна: подкласс `Java.Lang.Object`: 

```csharp
class MyComponentCallbacks : Java.Lang.Object, Android.Content.IComponentCallbacks {

    public void OnConfigurationChanged (Android.Content.Res.Configuration newConfig)
    {
        // implementation goes here...
    } 

    public void OnLowMemory ()
    {
        // implementation goes here...
    }
}
```

## <a name="implementation-details"></a>Сведения о реализации

*Оставшаяся часть этой страницы предоставляет сведения о реализации, которые могут быть изменены без предварительного уведомления* (и представлены здесь только потому, что разработчикам интересно, что происходит). 

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

public class HelloAndroid
    extends android.app.Activity
{
    static final String __md_methods;
    static {
        __md_methods = "n_onCreate:(Landroid/os/Bundle;)V:GetOnCreate_Landroid_os_Bundle_Handler\n" + "";
        mono.android.Runtime.register (
            "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, 
            Culture=neutral, PublicKeyToken=null", HelloAndroid.class, __md_methods);
    }

    public HelloAndroid ()
    {
        super ();
        if (getClass () == HelloAndroid.class)
            mono.android.TypeManager.Activate (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, 
                Culture=neutral, PublicKeyToken=null", "", this, new java.lang.Object[] {  });
    }

    @Override
    public void onCreate (android.os.Bundle p0)
    {
        n_onCreate (p0);
    }

    private native void n_onCreate (android.os.Bundle p0);
}
```

Обратите внимание, что базовый класс сохраняется, и для каждого метода, переопределяемого в управляемом коде, предоставляются `native` объявления методов. 
