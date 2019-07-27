---
title: Написание реагирующих приложений
ms.prod: xamarin
ms.assetid: 452DF940-6331-55F0-D130-002822BBED55
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: e3f7d788e71718f4ca1336b7906cf3d63bf07f32
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68509199"
---
# <a name="writing-responsive-applications"></a>Написание реагирующих приложений

Одним из ключей для поддержки пользовательского графического интерфейса пользователя является выполнение длительных задач в фоновом потоке, поэтому графический интерфейс не блокируется. Предположим, нам нужно вычислить значение, отображаемое пользователю, но для вычисления этого значения требуется 5 секунд:

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        SlowMethod ();
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        textview.Text = "Method Complete";
    }
}
```

Это будет работать, но приложение будет зависнуть в течение 5 секунд, пока вычисляется значение. В течение этого времени приложение не будет отвечать на любые взаимодействия с пользователем. Чтобы обойти это, мы хотим выполнить вычисления в фоновом потоке:

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        ThreadPool.QueueUserWorkItem (o => SlowMethod ());
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        textview.Text = "Method Complete";
    }
}
```

Теперь мы вычисляем значение в фоновом потоке, чтобы наш графический интерфейс по-прежнему отвечал во время вычисления. Тем не менее, когда вычисление завершено, происходит сбой приложения, и в журнале остается следующее:

```shell
E/mono    (11207): EXCEPTION handling: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):
E/mono    (11207): Unhandled Exception: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):   at Android.Runtime.JNIEnv.CallVoidMethod (IntPtr jobject, IntPtr jmethod, Android.Runtime.JValue[] parms)
E/mono    (11207):   at Android.Widget.TextView.set_Text (IEnumerable`1 value)
E/mono    (11207):   at MonoDroidDebugging.Activity1.SlowMethod ()
```

Это связано с тем, что графический интерфейс пользователя необходимо обновить из потока графического пользовательского интерфейса. Наш код обновляет графический интерфейс пользователя из потока ThreadPool, что приводит к сбою приложения. Нам нужно вычислить значение в фоновом потоке, а затем выполнить наше обновление в потоке графического пользовательского интерфейса, который обрабатывается с помощью [действия. рунонуисреад](xref:Android.App.Activity.RunOnUiThread*):

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        ThreadPool.QueueUserWorkItem (o => SlowMethod ());
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        RunOnUiThread (() => textview.Text = "Method Complete");
    }
}
```

Этот код работает так, как ожидалось. Этот графический интерфейс продолжает реагировать и правильно обновляется после сло вычисления.

Обратите внимание, что этот метод не используется только для вычисления дорогостоящего значения. Его можно использовать для выполнения любой длительной задачи, которая может быть выполнена в фоновом режиме, например вызов веб-службы или загрузка данных из Интернета.
