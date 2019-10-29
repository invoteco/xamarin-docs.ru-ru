---
title: Пошаговое руководство. Сохранение состояния действия
description: Мы рассмотрели теории о сохранении состояния в пошаговом руководству по жизненному циклу действий. Теперь давайте рассмотрим пример.
ms.prod: xamarin
ms.assetid: A6090101-67C6-4BDD-9416-F2FB74805A87
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: e449e6a62d0c8ca283f20c689477c1f1482611c5
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73017007"
---
# <a name="walkthrough---saving-the-activity-state"></a>Пошаговое руководство. Сохранение состояния действия

_Мы рассмотрели теории о сохранении состояния в пошаговом руководству по жизненному циклу действий. Теперь давайте рассмотрим пример._

## <a name="activity-state-walkthrough"></a>Пошаговое руководство по состоянию действий

Откройте проект **ActivityLifecycle_Start** (в примере [активитилифецикле](https://docs.microsoft.com/samples/xamarin/monodroid-samples/activitylifecycle) ), выполните сборку и запустите его. Это очень простой проект, который содержит два действия для демонстрации жизненного цикла действия и способов вызова различных методов жизненного цикла. При запуске приложения отображается экран `MainActivity`.

[![действия экрана](saving-state-images/01-activity-a-sml.png)](saving-state-images/01-activity-a.png#lightbox)

### <a name="viewing-state-transitions"></a>Просмотр переходов состояния

Каждый метод в этом примере записывает в окно вывода приложения IDE, чтобы указать состояние действия. (Чтобы открыть окно вывода в Visual Studio, нажмите **клавиши CTRL + ALT + O**;, чтобы открыть окно вывода в Visual Studio для Mac, щелкните **просмотр > Pad > выходные данные приложения**.)

При первом запуске приложения в окне Вывод отображаются изменения состояния *действия A*: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

После нажатия кнопки **Запуск действия b** мы видим, что *действие* приостанавливается и останавливается, пока *действие B* проходит изменения состояния: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.SecondActivity] Activity B - OnCreate
[ActivityLifecycle.SecondActivity] Activity B - OnStart
[ActivityLifecycle.SecondActivity] Activity B - OnResume
[ActivityLifecycle.MainActivity] Activity A - OnStop
```

В результате *действие б* запускается и отображается вместо *действия a*: 

[экран![действия б](saving-state-images/02-activity-b-sml.png)](saving-state-images/02-activity-b.png#lightbox)

Когда мы нажимайте кнопку " **назад** ", *действие б* уничтожается и возобновляется *действие A* : 

```shell
[ActivityLifecycle.SecondActivity] Activity B - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnRestart
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
[ActivityLifecycle.SecondActivity] Activity B - OnStop
[ActivityLifecycle.SecondActivity] Activity B - OnDestroy
```

### <a name="adding-a-click-counter"></a>Добавление счетчика щелчков

Теперь мы изменим приложение, чтобы у нас была кнопка, которая подсчитывает и отображает количество щелчков мышью. Сначала добавим переменную экземпляра `_counter` в `MainActivity`:

```csharp
int _counter = 0;
```

Теперь измените файл макета **Resource/Layout/Main. axml** и добавьте новый `clickButton`, отображающий количество раз, когда пользователь нащелкнул кнопку. Результирующий **Main. axml** должен выглядеть следующим образом: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <Button
        android:id="@+id/myButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/mybutton_text" />
    <Button
        android:id="@+id/clickButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/counterbutton_text" />
</LinearLayout>
```

Добавим следующий код в конец метода [OnCreate](xref:Android.App.Activity.OnCreate*) в `MainActivity` &ndash; этот код обрабатывает события click из `clickButton`:

```csharp
var clickbutton = FindViewById<Button> (Resource.Id.clickButton);
clickbutton.Text = Resources.GetString (
    Resource.String.counterbutton_text, _counter);
clickbutton.Click += (object sender, System.EventArgs e) =>
{
    _counter++;
    clickbutton.Text = Resources.GetString (
        Resource.String.counterbutton_text, _counter);
} ;
```

При повторной сборке и запуске приложения появляется новая кнопка, которая увеличивает и отображает значение `_counter` для каждого щелчка:

[![добавить число касаний](saving-state-images/03-touched-sml.png)](saving-state-images/03-touched.png#lightbox)

Но при вращении устройства в альбомный режим этот счетчик будет потерян:

[![с поворотом на альбомную устанавливает счетчик обратно в ноль](saving-state-images/05-rotate-nosave-sml.png)](saving-state-images/05-rotate-nosave.png#lightbox)

Изучая выходные данные приложения, мы видим, что *действие A* было приостановлено, остановлено, уничтожено, повторно создано, перезапущено, затем возобновлено во время смены с книжной на альбомную. 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnStop
[ActivityLifecycle.MainActivity] Activity A - On Destroy

[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

Так как *действие A* уничтожается и повторно создается при вращении устройства, его состояние будет потеряно. Далее мы добавим код для сохранения и восстановления состояния экземпляра.

### <a name="adding-code-to-preserve-instance-state"></a>Добавление кода для сохранения состояния экземпляра

Добавим метод в `MainActivity`, чтобы сохранить состояние экземпляра. До уничтожения *действия A* Android автоматически вызывает [онсавеинстанцестате](xref:Android.App.Activity.OnSaveInstanceState*) и передает в [пакет](xref:Android.OS.Bundle) , который мы можем использовать для хранения состояния экземпляра. Давайте воспользуемся им, чтобы сохранить число щелчков в виде целого числа:

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
    outState.PutInt ("click_count", _counter);
    Log.Debug(GetType().FullName, "Activity A - Saving instance state");

    // always call the base implementation!
    base.OnSaveInstanceState (outState);    
}
```

Когда *действие* а воссоздается и возобновляется, Android передает этот `Bundle` обратно в метод `OnCreate`. Добавим код в `OnCreate`, чтобы восстановить значение `_counter` из переданного `Bundle`. Добавьте следующий код непосредственно перед строкой, где определено `clickbutton`: 

```csharp
if (bundle != null)
{
    _counter = bundle.GetInt ("click_count", 0);
    Log.Debug(GetType().FullName, "Activity A - Recovered instance state");
}
```

Выполните сборку и запустите приложение еще раз, а затем нажмите вторую кнопку несколько раз. При вращении устройства в альбомный режим счетчик сохраняется.

[![при повороте экрана отображается число четырех сохраненных](saving-state-images/06-rotate-save-sml.png)](saving-state-images/06-rotate-save.png#lightbox)

Давайте взглянем на окно вывод, чтобы увидеть, что произошло:

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.MainActivity] Activity A - Saving instance state
[ActivityLifecycle.MainActivity] Activity A - OnStop
[ActivityLifecycle.MainActivity] Activity A - On Destroy

[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - Recovered instance state
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

Перед вызовом метода [OnStop](xref:Android.App.Activity.OnStop) был вызван наш новый метод `OnSaveInstanceState` для сохранения `_counter` значения в `Bundle`. При вызове метода `OnCreate` Android передал этот `Bundle` назад, и мы смогли использовать его для восстановления значения `_counter` в то место, где мы остановились.

## <a name="summary"></a>Сводка

В этом валксаугх мы использовали наши знания о жизненном цикле действий для сохранения данных о состоянии.

## <a name="related-links"></a>Связанные ссылки

- [Активитилифецикле (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/activitylifecycle)
- [Жизненный цикл действия](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Действие Android](xref:Android.App.Activity)
