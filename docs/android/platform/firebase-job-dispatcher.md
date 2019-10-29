---
title: Диспетчер заданий Firebase
description: В этом руководство описано, как запланировать фоновую работу с помощью библиотеки диспетчера заданий Firebase из Google.
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/05/2018
ms.openlocfilehash: 280fe11f935db0a364f3342b22bb9544cdda1e6d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020237"
---
# <a name="firebase-job-dispatcher"></a>Диспетчер заданий Firebase

_В этом руководство описано, как запланировать фоновую работу с помощью библиотеки диспетчера заданий Firebase из Google._

## <a name="overview"></a>Обзор

Одним из лучших способов обеспечения реагирования на запросы пользователя к приложению Android является обеспечение выполнения сложной или длительной работы в фоновом режиме. Однако важно, чтобы фоновая работа не негативно влияла на взаимодействие пользователя с устройством. 

Например, фоновое задание может опрашивать веб-сайт каждые три или четыре минуты для запроса изменений в определенном наборе данных. Это кажется немягким, однако это повлияет на время работы от аккумулятора. Приложение будет многократно выводить устройство из спящего режима, повышать производительность процессора до более высокого состояния электропитания, включать Радио, выполнять сетевые запросы и обрабатывать результаты. Это хуже, так как устройство не будет немедленно выключено и вернуться в состояние низкого энергопотребления. Плохо запланированная фоновая работа может привести к непреднамеренному состоянию устройства с ненужными и чрезмерными требованиями к питанию. Это похожее действие безобидным (опрос веб-сайта) сделает устройство непригодным для использования в течение относительно короткого периода времени.

Android предоставляет следующие API-интерфейсы для выполнения работы в фоновом режиме, но они недостаточно для интеллектуального планирования заданий. 

- Службы **[намеренных служб](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** &ndash; намеренно подходят для выполнения работы, однако они не дают возможности планировать работу.
- **[Алармманажер](https://developer.android.com/reference/android/app/AlarmManager.html)** &ndash; эти API-интерфейсы позволяют планировать работу только по расписанию, но не дают возможности реально выполнять работу. Кроме того, Алармманажер разрешает только ограничения на основе времени, что означает создание оповещения в определенное время или по истечении определенного периода времени. 
- **[Жобсчедулер](https://developer.android.com/reference/android/app/job/JobScheduler.html)** &ndash; JobSchedule — это отличный API, который работает с операционной системой для планирования заданий. Однако он доступен только для тех приложений Android, которые предназначены для API уровня 21 или более поздней версии. 
- **[Широковещательные получатели](~/android/app-fundamentals/broadcast-receivers.md)** , &ndash; приложение Android, могут настроить широковещательные приемники для выполнения работы в ответ на события на уровне системы или намерения. Однако широковещательные приемники не предоставляют контроль над временем выполнения задания. Кроме того, изменения в операционной системе Android ограничивают, когда будут работать широковещательные получатели, или на виды работы, на которую они могут отвечать. 

Существует два ключевых компонента для эффективного выполнения фоновой работы (иногда называемой _фоновым заданием_ или _заданием_):

1. **Интеллектуальное планирование работы** &ndash; важно, что когда приложение выполняет работу в фоновом режиме, которое оно делает в качестве хорошего. В идеале приложение не должно требовать выполнения задания. Вместо этого приложение должно указать условия, которые должны быть выполнены для выполнения задания, а затем запланировать выполнение работы при соблюдении условий. Это позволяет выполнять интеллектуальную работу с Android. Например, сетевые запросы могут быть пакетными для одновременного выполнения, чтобы обеспечить максимальное использование нагрузки, связанной с сетью.
2. **Инкапсуляция работы** &ndash; код для выполнения фоновой работы должен быть инкапсулирован в дискретный компонент, который может быть запущен независимо от пользовательского интерфейса и будет сравнительно легко перепланироваться, если не удается завершить работу для некоторых оправдан.

Диспетчер заданий Firebase — это библиотека из Google, которая предоставляет API Fluent для упрощения планирования фоновой работы. Она предназначена для замены Google Cloud Manager. Диспетчер заданий Firebase состоит из следующих API-интерфейсов:

- `Firebase.JobDispatcher.JobService` — это абстрактный класс, который должен быть расширен с помощью логики, которая будет выполняться в фоновом задании.
- `Firebase.JobDispatcher.JobTrigger` объявляет, когда должно запускаться задание. Обычно это выражается как окно времени, например, подождите не менее 30 секунд, прежде чем запускать задание, но запустите задание в течение 5 минут.
- `Firebase.JobDispatcher.RetryStrategy` содержит сведения о том, что следует делать, если задание не выполняется должным образом. Стратегия повтора указывает время ожидания перед повторной попыткой выполнения задания. 
- `Firebase.JobDispatcher.Constraint` — это необязательное значение, описывающее условие, которое должно быть выполнено перед запуском задания, например, если устройство находится в неконтролируемой сети или заряжается.
- `Firebase.JobDispatcher.Job` — это API-интерфейс, который объединяет предыдущие API-интерфейсы в с единицей работы, которая может быть запланирована `JobDispatcher`. Класс `Job.Builder` используется для создания экземпляра `Job`.
- `Firebase.JobDispatcher.JobDispatcher` использует три предыдущих API-интерфейса для планирования работы с операционной системой и предоставления способа отмены заданий при необходимости.

Чтобы запланировать работу с диспетчером заданий Firebase, приложение Xamarin. Android должно инкапсулировать код в типе, который расширяет класс `JobService`. в `JobService` есть три метода жизненного цикла, которые могут быть вызваны в течение времени существования задания:

- **`bool OnStartJob(IJobParameters parameters)`** &ndash; этот метод позволяет выполнять работу и всегда должен быть реализован. Он выполняется в основном потоке. Этот метод возвращает `true` при наличии оставшейся работы или `false` по завершении работы. 
- **`bool OnStopJob(IJobParameters parameters)`** &ndash; это вызывается, когда задание останавливается по какой-либо причине. Он должен возвращать `true`, если задание следует перепланировать для последующего использования.
- **`JobFinished(IJobParameters parameters, bool needsReschedule)`** &ndash; этот метод вызывается, когда `JobService` завершила какие-либо асинхронные операции. 

Чтобы запланировать задание, приложение создаст экземпляр объекта `JobDispatcher`. Затем `Job.Builder` используется для создания объекта `Job`, который предоставляется `JobDispatcher`, который попытается запланировать выполнение задания.

В этом руководство описано, как добавить диспетчер заданий Firebase в приложение Xamarin. Android и использовать его для планирования фоновой работы.

## <a name="requirements"></a>Требования

Диспетчеру заданий Firebase требуется интерфейс Android API уровня 9 или более поздней версии. Библиотека диспетчера заданий Firebase использует некоторые компоненты, предоставляемые Сервисы Google Play; на устройстве должно быть установлено Сервисы Google Play.

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>Использование библиотеки диспетчера заданий Firebase в Xamarin. Android

Чтобы начать работу с диспетчером заданий Firebase, сначала добавьте [пакет NuGet Xamarin. Firebase. жобдиспатчер](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher) в проект Xamarin. Android. Найдите в диспетчере пакетов NuGet пакет **Xamarin. Firebase. жобдиспатчер** (который по-прежнему находится в предварительной версии).

После добавления библиотеки диспетчера заданий Firebase создайте класс `JobService` и запланируйте его запуск с помощью экземпляра `FirebaseJobDispatcher`.

### <a name="creating-a-jobservice"></a>Создание Жобсервице

Вся работа, выполняемая библиотекой диспетчера заданий Firebase, должна быть выполнена в типе, расширяющем `Firebase.JobDispatcher.JobService` абстрактный класс. Создание `JobService` очень похоже на создание `Service` с помощью платформы Android: 

1. Расширение класса `JobService`
2. Украшение подкласса с помощью `ServiceAttribute`. Хотя это не является обязательным требованием, рекомендуется явно задать параметр `Name`, чтобы упростить отладку `JobService`. 
3. Добавьте `IntentFilter`, чтобы объявить `JobService` в **файле AndroidManifest. XML**. Это также позволит библиотеке диспетчера заданий Firebase определить и вызвать `JobService`.

Следующий код является примером простейшего `JobService` для приложения с помощью TPL для асинхронного выполнения некоторой работы:

```csharp
[Service(Name = "com.xamarin.fjdtestapp.DemoJob")]
[IntentFilter(new[] {FirebaseJobServiceIntent.Action})]
public class DemoJob : JobService
{
    static readonly string TAG = "X:DemoService";

    public override bool OnStartJob(IJobParameters jobParameters)
    {
        Task.Run(() =>
        {
            // Work is happening asynchronously (code omitted)
                       
        });

        // Return true because of the asynchronous work
        return true;  
    }

    public override bool OnStopJob(IJobParameters jobParameters)
    {
        Log.Debug(TAG, "DemoJob::OnStartJob");
        // nothing to do.
        return false;
    }
}
```

### <a name="creating-a-firebasejobdispatcher"></a>Создание Фиребасежобдиспатчер

Прежде чем можно будет запланировать работу, необходимо создать объект `Firebase.JobDispatcher.FirebaseJobDispatcher`. `FirebaseJobDispatcher` отвечает за планирование `JobService`. Следующий фрагмент кода является одним из способов создания экземпляра `FirebaseJobDispatcher`: 

 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

В предыдущем фрагменте кода `GooglePlayDriver` — это класс, помогающий `FirebaseJobDispatcher` взаимодействовать с некоторыми API-интерфейсами планирования в Сервисы Google Play на устройстве. Параметр `context` — любое `Context`Android, например действие. В настоящее время `GooglePlayDriver` является единственной реализацией `IDriver` в библиотеке диспетчера заданий Firebase. 

Привязка Xamarin. Android для диспетчера заданий Firebase предоставляет метод расширения для создания `FirebaseJobDispatcher` из `Context`: 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

После создания экземпляра `FirebaseJobDispatcher` можно создать `Job` и запустить код в классе `JobService`. `Job` создается объектом `Job.Builder` и будет рассмотрен в следующем разделе.

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>Создание Firebase. Жобдиспатчер. Job с помощью задания. Builder

Класс `Firebase.JobDispatcher.Job` отвечает за инкапсуляцию метаданных, необходимых для запуска `JobService`. @ No__t_0_ содержит такие сведения, как любое ограничение, которое должно быть выполнено перед выполнением задания, если `Job` повторяется или какие-либо триггеры, которые приведут к выполнению задания.  Как минимум, `Job` должен иметь _тег_ (уникальную строку, определяющую задание для `FirebaseJobDispatcher`) и тип `JobService`, который должен быть выполнен. Диспетчер заданий Firebase создает экземпляр `JobService`, когда найдет время запуска задания.  `Job` создается с помощью экземпляра класса `Firebase.JobDispatcher.Job.JobBuilder`. 

Следующий фрагмент кода является простым примером создания `Job` с помощью привязки Xamarin. Android:

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

`Job.Builder` выполнит некоторые основные проверки входных значений для задания. Если `Job.Builder` создать `Job`, будет создано исключение.  `Job.Builder` создаст `Job` со следующими по умолчанию:

- _Время существования_ `Job`(как долго оно будет запускаться) — только до перезагрузки устройства &ndash; после того, как устройство перезагрузится `Job` будет утеряно.
- `Job` не повторяется &ndash; он будет выполняться только один раз.
- `Job` будет запланировать запуск как можно скорее.
- Стратегия повтора по умолчанию для `Job` — использование _экспоненциальной_ задержки (см. Дополнительные сведения ниже в разделе [Настройка ретристратеги](#Setting_a_RetryStrategy)).

### <a name="scheduling-a-job"></a>Планирование задания

После создания `Job`необходимо запланировать его `FirebaseJobDispatcher` перед запуском. Существует два метода планирования `Job`.

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

Значение, возвращаемое `FirebaseJobDispatcher.Schedule`, будет одним из следующих целочисленных значений:

- `FirebaseJobDispatcher.ScheduleResultSuccess` &ndash; `Job` был успешно запланирован.
- `FirebaseJobDispatcher.ScheduleResultUnknownError` &ndash; возникла неизвестная проблема, препятствующая запланированию `Job`.
- `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable` &ndash; использовался недопустимый `IDriver`, или `IDriver` был каким либо образом недоступен. 
- `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger` &ndash; `Trigger` не поддерживалась.
- `FirebaseJobDispatcher.ScheduleResultBadService` &ndash; служба настроена неправильно или недоступна.

### <a name="configuring-a-job"></a>Настройка задания

Можно настроить задание. Ниже приведены примеры того, как задание может быть настроено.

- [Передача параметров в задание](#Passing_Parameters_to_a_Job) &ndash; `Job` может потребовать дополнительных значений для выполнения работы, например загрузки файла.
- [Ограничения Set](#Setting_Constraints) &ndash; могут потребоваться для запуска задания только при соблюдении определенных условий. Например, запускать `Job` только при зарядки устройства. 
- [Укажите, когда `Job` должен выполняться](#Setting_Job_Triggers) &ndash; диспетчер заданий Firebase позволяет приложениям указывать время выполнения задания.  
- [Объявите стратегию повторных попыток для заданий со сбоями](#Setting_a_RetryStrategy) &ndash; _стратегия повторных попыток_ предоставляет инструкции по `FirebaseJobDispatcher` действиям с `Jobs`, которые не удалось завершить. 

В следующих разделах будут рассмотрены дополнительные сведения о каждом из этих разделов.

<a name="Passing_Parameters_to_a_Job" />

#### <a name="passing-parameters-to-a-job"></a>Передача параметров в задание

Параметры передаются в задание путем создания `Bundle`, который передается вместе с методом `Job.Builder.SetExtras`:

```csharp
Bundle jobParameters = new Bundle();
jobParameters.PutInt(FibonacciCalculatorJob.FibonacciPositionKey, 25);

Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetExtras(jobParameters)
                      .Build();

```

Доступ к `Bundle` осуществляется из свойства `IJobParameters.Extras` метода `OnStartJob`:

```csharp
public override bool OnStartJob(IJobParameters jobParameters)
{
    int position = jobParameters.Extras.GetInt(FibonacciPositionKey, DEFAULT_VALUE);
    
    // rest of code omitted
} 
```

<a name="Setting_Constraints" />

#### <a name="setting-constraints"></a>Задание ограничений

Ограничения могут снизить затраты или истощение аккумулятора на устройстве. Класс `Firebase.JobDispatcher.Constraint` определяет эти ограничения как целочисленные значения.

- `Constraint.OnUnmeteredNetwork` &ndash; запускать задание только при подключении устройства к неподдерживаемой сети. Это полезно, чтобы предотвратить оплату данных у пользователя.
- `Constraint.OnAnyNetwork` &ndash; запустить задание в любой сети, к которой подключено устройство. Если указано вместе с `Constraint.OnUnmeteredNetwork`, это значение будет иметь приоритет.
- `Constraint.DeviceCharging` &ndash; запускать задание только при зарядки устройства.

Ограничения задаются с помощью метода `Job.Builder.SetConstraint`: 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers" />

`JobTrigger` предоставляет инструкции операционной системе о том, когда должно начаться задание. `JobTrigger` имеет _окно выполнения_ , которое определяет запланированное время выполнения `Job`. Окно выполнения имеет значение _начального окна_ и значение _конечного окна_ . Начальное окно — это количество секунд, в течение которых устройство должно ожидать перед выполнением задания, а значение конечного окна — максимальное число секунд ожидания перед запуском `Job`. 

`JobTrigger` можно создать с помощью метода `Firebase.Jobdispatcher.Trigger.ExecutionWindow`.  Например `Trigger.ExecutionWindow(15,60)` означает, что задание должно выполняться от 15 до 60 секунд после запланированного времени. Метод `Job.Builder.SetTrigger` используется для 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

`JobTrigger` по умолчанию для задания представлено значением `Trigger.Now`, которое указывает, что задание должно запускаться как можно быстрее после выполнения запланированного задания.

<a name="Setting_a_RetryStrategy" />

#### <a name="setting-a-retrystrategy"></a>Настройка Ретристратеги

`Firebase.JobDispatcher.RetryStrategy` используется для указания того, сколько времени задержки должно использовать устройство, прежде чем пытаться повторно запустить невыполненное задание. `RetryStrategy` имеет _политику_, которая определяет, какой базовый алгоритм будет использоваться для повторного планирования невыполненного задания, и окно выполнения, указывающее окно, в котором должно быть запланировано задание. Это _окно перепланирования_ определяется двумя значениями. Первое значение — это время ожидания (в секундах) перед повторным планированием задания ( _начальное значение отхода_ ), а второе число — это максимальное число секунд, по истечении которых задание должно выполняться ( _Максимальное значение отхода_ ). 

Два типа политик повтора идентифицируются следующими значениями типа int:

- `RetryStrategy.RetryPolicyExponential`, &ndash; политика _экспоненциальной_ задержки увеличит начальное значение отхода экспоненциально после каждого сбоя. При первом сбое задания библиотека будет ожидать интервал _initial, указанный перед повторным планированием задания &ndash; примере 30 секунд. При втором сбое задания библиотека будет ожидать не менее 60 секунд перед попыткой запуска задания. После третьей неудачной попытки библиотека будет ждать 120 секунд и т. д. `RetryStrategy` по умолчанию для библиотеки диспетчера заданий Firebase представляется объектом `RetryStrategy.DefaultExponential`. Он имеет первоначальный отход на 30 секунд и максимум в 3600 секунд.
- `RetryStrategy.RetryPolicyLinear` &ndash; эта стратегия является _линейной_ отработкой, что задание должно быть запланировано на выполнение через заданные интервалы (до тех пор, пока оно не будет выполнено). Линейная откладывание лучше всего подходит для работы, которая должна быть выполнена как можно скорее, или для проблем, которые будут быстро устранять себя. Библиотека диспетчера заданий Firebase определяет `RetryStrategy.DefaultLinear` с периодом перепланирования в интервале не менее 30 секунд и до 3600 секунд.

Можно определить пользовательский `RetryStrategy` с помощью метода `FirebaseJobDispatcher.NewRetryStrategy`. Он принимает три параметра:

1. `int policy` &ndash; _Политика_ является одним из предыдущих `RetryStrategy` значений, `RetryStrategy.RetryPolicyLinear`или `RetryStrategy.RetryPolicyExponential`.
2. `int initialBackoffSeconds` &ndash; _Начальное_ отставание — это задержка (в секундах), необходимая перед повторной попыткой выполнения задания. Значение по умолчанию — 30 секунд. 
3. `int maximumBackoffSeconds` &ndash; _Максимальное значение отхода_ объявляет максимальное время задержки (в секундах) перед повторной попыткой выполнения задания. Значение по умолчанию — 3600 секунд. 

```csharp
RetryStrategy retry = dispatcher.NewRetryStrategy(RetryStrategy.RetryPolicyLinear, initialBackoffSeconds, maximumBackoffSet);

// Create a Job and set the RetryStrategy via the Job.Builder
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetRetryStrategy(retry)
                      .Build();
```

### <a name="cancelling-a-job"></a>Отмена задания

Можно отменить все запланированные задания или только одно задание, используя метод `FirebaseJobDispatcher.CancelAll()` или метод `FirebaseJobDispatcher.Cancel(string)`:

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

Любой из методов возвратит целочисленное значение:

- `FirebaseJobDispatcher.CancelResultSuccess` &ndash; задание успешно отменено.
- `FirebaseJobDispatcher.CancelResultUnknownError` &ndash; ошибка не позволила отменить задание.
- `FirebaseJobDispatcher.CancelResult.NoDriverAvailable` &ndash; `FirebaseJobDispatcher` не может отменить задание, так как нет допустимых `IDriver`.

## <a name="summary"></a>Сводка

В этом руководство описано, как использовать диспетчер заданий Firebase для интеллектуального выполнения работы в фоновом режиме. В нем обсуждалось, как инкапсулировать работу, которую необходимо выполнить в качестве `JobService`, и как использовать `FirebaseJobDispatcher` для планирования этой работы, указания критериев с `JobTrigger` и способа обработки ошибок с `RetryStrategy`.

## <a name="related-links"></a>Связанные ссылки

- [Xamarin. Firebase. Жобдиспатчер на NuGet](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)
- [Firebase-Job-Dispatcher на GitHub](https://github.com/firebase/firebase-jobdispatcher-android)
- [Привязка Xamarin. Firebase. Жобдиспатчер](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [Интеллектуальное задание — планирование](https://developer.android.com/topic/performance/scheduling.html)
- [Батарея Android и оптимизация памяти — Google I/O 2016 (видео)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
