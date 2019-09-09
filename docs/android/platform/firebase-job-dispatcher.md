---
title: Диспетчер заданий Firebase
description: В этом руководство описано, как запланировать фоновую работу с помощью библиотеки диспетчера заданий Firebase из Google.
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/05/2018
ms.openlocfilehash: cb7e8aaca13405aedd422288421d497653ddbfe8
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761205"
---
# <a name="firebase-job-dispatcher"></a>Диспетчер заданий Firebase

_В этом руководство описано, как запланировать фоновую работу с помощью библиотеки диспетчера заданий Firebase из Google._

## <a name="overview"></a>Обзор

Одним из лучших способов обеспечения реагирования на запросы пользователя к приложению Android является обеспечение выполнения сложной или длительной работы в фоновом режиме. Однако важно, чтобы фоновая работа не негативно влияла на взаимодействие пользователя с устройством. 

Например, фоновое задание может опрашивать веб-сайт каждые три или четыре минуты для запроса изменений в определенном наборе данных. Это кажется немягким, однако это повлияет на время работы от аккумулятора. Приложение будет многократно выводить устройство из спящего режима, повышать производительность процессора до более высокого состояния электропитания, включать Радио, выполнять сетевые запросы и обрабатывать результаты. Это хуже, так как устройство не будет немедленно выключено и вернуться в состояние низкого энергопотребления. Плохо запланированная фоновая работа может привести к непреднамеренному состоянию устройства с ненужными и чрезмерными требованиями к питанию. Это похожее действие безобидным (опрос веб-сайта) сделает устройство непригодным для использования в течение относительно короткого периода времени.

Android предоставляет следующие API-интерфейсы для выполнения работы в фоновом режиме, но они недостаточно для интеллектуального планирования заданий. 

- **[Службы намерения](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** &ndash; Службы с намерением прекрасно подходят для выполнения работы, однако они не дают возможности планировать работу.
- **[Алармманажер](https://developer.android.com/reference/android/app/AlarmManager.html)** &ndash; Эти API-интерфейсы позволяют планировать работу только по расписанию, но не дают возможности реально выполнять работу. Кроме того, Алармманажер разрешает только ограничения на основе времени, что означает создание оповещения в определенное время или по истечении определенного периода времени. 
- **[Жобсчедулер](https://developer.android.com/reference/android/app/job/JobScheduler.html)** &ndash; JobSchedule — это отличный API, который работает с операционной системой для планирования заданий. Однако он доступен только для тех приложений Android, которые предназначены для API уровня 21 или более поздней версии. 
- **[Широковещательные приемники](~/android/app-fundamentals/broadcast-receivers.md)** &ndash; Приложение Android может настроить широковещательные приемники для выполнения работы в ответ на события на уровне системы или намерения. Однако широковещательные приемники не предоставляют контроль над временем выполнения задания. Кроме того, изменения в операционной системе Android ограничивают, когда будут работать широковещательные получатели, или на виды работы, на которую они могут отвечать. 

Существует два ключевых компонента для эффективного выполнения фоновой работы (иногда называемой _фоновым заданием_ или _заданием_):

1. **Интеллектуальное планирование работы** &ndash; Важно, что когда приложение работает в фоновом режиме, которое оно делает в качестве хорошего. В идеале приложение не должно требовать выполнения задания. Вместо этого приложение должно указать условия, которые должны быть выполнены для выполнения задания, а затем запланировать выполнение работы при соблюдении условий. Это позволяет выполнять интеллектуальную работу с Android. Например, сетевые запросы могут быть пакетными для одновременного выполнения, чтобы обеспечить максимальное использование нагрузки, связанной с сетью.
2. **Инкапсуляция работы** &ndash; Код для выполнения фоновой работы должен быть инкапсулирован в дискретный компонент, который может быть запущен независимо от пользовательского интерфейса и будет сравнительно простым в планировании, если по какой-либо причине не удается завершить работу.

Диспетчер заданий Firebase — это библиотека из Google, которая предоставляет API Fluent для упрощения планирования фоновой работы. Она предназначена для замены Google Cloud Manager. Диспетчер заданий Firebase состоит из следующих API-интерфейсов:

- `Firebase.JobDispatcher.JobService` — Это абстрактный класс, который должен быть расширен с помощью логики, которая будет выполняться в фоновом задании.
- При запуске задания объявляется. `Firebase.JobDispatcher.JobTrigger` Обычно это выражается как окно времени, например, подождите не менее 30 секунд, прежде чем запускать задание, но запустите задание в течение 5 минут.
- Содержит `Firebase.JobDispatcher.RetryStrategy` сведения о том, что следует делать, если задание не выполняется должным образом. Стратегия повтора указывает время ожидания перед повторной попыткой выполнения задания. 
- `Firebase.JobDispatcher.Constraint` — Это необязательное значение, описывающее условие, которое должно быть выполнено перед запуском задания, например устройство находится в сети с нелимитным тарифом или заряжается.
- — Это API-интерфейс, который объединяет предыдущие API в с единицей работы, которую можно запланировать `JobDispatcher`. `Firebase.JobDispatcher.Job` Класс используется для создания экземпляра `Job`. `Job.Builder`
- Использует `Firebase.JobDispatcher.JobDispatcher` три предыдущих API-интерфейса для планирования работы с операционной системой и предоставления способа отмены заданий при необходимости.

Чтобы запланировать работу с диспетчером заданий Firebase, приложение Xamarin. Android должно инкапсулировать код в типе, который расширяет `JobService` класс. `JobService`содержит три метода жизненного цикла, которые могут быть вызваны в течение времени существования задания:

- **`bool OnStartJob(IJobParameters parameters)`** &ndash; Этот метод позволяет выполнять работу и всегда реализовываться. Он выполняется в основном потоке. Этот метод возвращает `true` значение, если есть оставшаяся работа или `false` если работа выполнена. 
- **`bool OnStopJob(IJobParameters parameters)`** &ndash; Это вызывается, когда задание останавливается по какой-либо причине. Он должен возвращать `true` значение, если задание следует перепланировать для последующего использования.
- **`JobFinished(IJobParameters parameters, bool needsReschedule)`** Этот метод вызывается, `JobService` когда завершается любая Асинхронная работа. &ndash; 

Чтобы запланировать задание, приложение создаст экземпляр `JobDispatcher` объекта. Затем используется для `Job` создания объекта, который предоставляется для, `JobDispatcher` который попытается запланировать выполнение задания. `Job.Builder`

В этом руководство описано, как добавить диспетчер заданий Firebase в приложение Xamarin. Android и использовать его для планирования фоновой работы.

## <a name="requirements"></a>Требования

Диспетчеру заданий Firebase требуется интерфейс Android API уровня 9 или более поздней версии. Библиотека диспетчера заданий Firebase использует некоторые компоненты, предоставляемые Сервисы Google Play; на устройстве должно быть установлено Сервисы Google Play.

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>Использование библиотеки диспетчера заданий Firebase в Xamarin. Android

Чтобы начать работу с диспетчером заданий Firebase, сначала добавьте [пакет NuGet Xamarin. Firebase. жобдиспатчер](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher) в проект Xamarin. Android. Найдите в диспетчере пакетов NuGet пакет **Xamarin. Firebase. жобдиспатчер** (который по-прежнему находится в предварительной версии).

После добавления библиотеки диспетчера заданий Firebase создайте `JobService` класс и запланируйте его запуск с помощью экземпляра. `FirebaseJobDispatcher`

### <a name="creating-a-jobservice"></a>Создание Жобсервице

Вся работа, выполняемая библиотекой диспетчера заданий Firebase, должна быть выполнена в типе, расширяющем `Firebase.JobDispatcher.JobService` абстрактный класс. `JobService` Создание `Service` с помощью платформы Android очень похоже на создание. 

1. `JobService` Расширение класса
2. Украшение подкласса с `ServiceAttribute`помощью. Хотя это не является обязательным требованием, рекомендуется явно задать `Name` параметр, чтобы упростить `JobService`отладку. 
3. Добавьте, `JobService` чтобы объявить в **файле AndroidManifest. XML.** `IntentFilter` Это также поможет библиотеке диспетчера заданий Firebase определить и вызвать `JobService`.

Следующий код является примером простейшего `JobService` для приложения, использующего TPL для асинхронного выполнения некоторой работы:

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

Прежде чем можно будет запланировать работу, необходимо создать `Firebase.JobDispatcher.FirebaseJobDispatcher` объект. Отвечает за планирование а `JobService`. `FirebaseJobDispatcher` Следующий фрагмент кода является одним из способов создания экземпляра класса `FirebaseJobDispatcher`. 

 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

В предыдущем фрагменте `GooglePlayDriver` кода это класс, который `FirebaseJobDispatcher` помогает взаимодействовать с некоторыми API-интерфейсами планирования в сервисы Google Play на устройстве. Параметр `context` — это любое Android `Context`, например действие. В настоящее время `IDriver`являетсяединственной реализацией в библиотеке диспетчера заданий Firebase. `GooglePlayDriver` 

Привязка Xamarin. Android для диспетчера заданий Firebase предоставляет метод расширения для создания `FirebaseJobDispatcher` `Context`из: 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

После создания экземпляра `JobService` класса можно `Job` создать и выполнить код в классе. `FirebaseJobDispatcher` Объектсоздается`Job.Builder` объектом и будет рассмотрен в следующем разделе. `Job`

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>Создание Firebase. Жобдиспатчер. Job с помощью задания. Builder

Класс отвечает за инкапсуляцию метаданных, необходимых для `JobService`выполнения. `Firebase.JobDispatcher.Job` Содержит такие сведения, как любое ограничение, которое должно быть выполнено, прежде чем задание может быть запущено, `Job` если повторяется, или какие-либо триггеры, которые приведут к запуску задания.`Job`  Как минимум, `Job` необходимо иметь _тег_ (уникальную строку, определяющую задание `FirebaseJobDispatcher`) и тип `JobService` выполняемого объекта. Диспетчер заданий Firebase создает экземпляр, `JobService` когда найдет время запуска задания.  Объект `Job` создается с помощью экземпляра `Firebase.JobDispatcher.Job.JobBuilder` класса. 

Следующий фрагмент кода является простым примером создания `Job` с помощью привязки Xamarin. Android:

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

`Job.Builder` Выполнит некоторые основные проверки входных значений для задания. Если `Job.Builder` невозможно`Job`создать, будет создано исключение.  `Job.Builder` Создаст объект`Job` со следующими значениями по умолчанию:

- Время существования (время запланированного запуска ) составляет только до тех пор, &ndash; пока устройство `Job` не будет перезагружено после того, как устройство будет перезагружено. `Job`
- `Job` Не&ndash; повторяется, он будет выполняться только один раз.
- Запуск `Job` будет запланирован как можно скорее.
- Стратегия повтора по умолчанию для `Job` — использование _экспоненциальной_ задержки (см. Дополнительные сведения ниже в разделе [Настройка ретристратеги](#Setting_a_RetryStrategy)).

### <a name="scheduling-a-job"></a>Планирование задания

После создания `Job`его необходимо запланировать `FirebaseJobDispatcher` с помощью перед запуском. Существует два метода планирования `Job`:

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

Значение, возвращаемое `FirebaseJobDispatcher.Schedule` , будет иметь одно из следующих целочисленных значений:

- `FirebaseJobDispatcher.ScheduleResultSuccess`&ndash; Расписание`Job` успешно запланировано.
- `FirebaseJobDispatcher.ScheduleResultUnknownError`Произошла неизвестная проблема, `Job` препятствующая запланированию. &ndash;
- `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable`Был использован `IDriver` недопустимый или `IDriver` недоступен. &ndash; 
- `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger`&ndash; Не поддерживается`Trigger` .
- `FirebaseJobDispatcher.ScheduleResultBadService`&ndash; Служба настроена неправильно или недоступна.

### <a name="configuring-a-job"></a>Настройка задания

Можно настроить задание. Ниже приведены примеры того, как задание может быть настроено.

- [Передача параметров в задание](#Passing_Parameters_to_a_Job) Для выполнения работы может потребоваться дополнительное значение, например скачивание файла. `Job` &ndash;
- [Задать ограничения](#Setting_Constraints) &ndash; Может потребоваться запустить задание, только если выполняются определенные условия. Например, `Job` если устройство заряжается, выполняется только. 
- [Укажите, когда `Job` должен запускаться](#Setting_Job_Triggers) &ndash; диспетчер заданий Firebase, позволяет приложениям указывать время выполнения задания.  
- [Объявление стратегии повтора для заданий со сбоями](#Setting_a_RetryStrategy) Стратегия повторных _попыток_ `FirebaseJobDispatcher` предоставляет инструкции `Jobs` по выполнению этой ошибки. &ndash; 

В следующих разделах будут рассмотрены дополнительные сведения о каждом из этих разделов.

<a name="Passing_Parameters_to_a_Job" />

#### <a name="passing-parameters-to-a-job"></a>Передача параметров в задание

Параметры передаются в задание путем создания `Bundle` , который передается вместе `Job.Builder.SetExtras` с методом.

```csharp
Bundle jobParameters = new Bundle();
jobParameters.PutInt(FibonacciCalculatorJob.FibonacciPositionKey, 25);

Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetExtras(jobParameters)
                      .Build();

```

Объект `Bundle` доступен `IJobParameters.Extras` из свойства`OnStartJob` метода:

```csharp
public override bool OnStartJob(IJobParameters jobParameters)
{
    int position = jobParameters.Extras.GetInt(FibonacciPositionKey, DEFAULT_VALUE);
    
    // rest of code omitted
} 
```

<a name="Setting_Constraints" />

#### <a name="setting-constraints"></a>Задание ограничений

Ограничения могут снизить затраты или истощение аккумулятора на устройстве. `Firebase.JobDispatcher.Constraint` Класс определяет эти ограничения как целочисленные значения.

- `Constraint.OnUnmeteredNetwork`&ndash; Запускайте задание только в том случае, если устройство подключено к неподдерживаемой сети. Это полезно, чтобы предотвратить оплату данных у пользователя.
- `Constraint.OnAnyNetwork`&ndash; Запустите задание в любой сети, к которой подключено устройство. Если указано вместе с `Constraint.OnUnmeteredNetwork`, это значение будет иметь приоритет.
- `Constraint.DeviceCharging`&ndash; Запускать задание только при зарядки устройства.

Ограничения задаются с `Job.Builder.SetConstraint` помощью метода: 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers" />

`JobTrigger` Предоставляет рекомендации операционной системе о том, когда должно начаться задание. Имеет окно _выполнения_ , которое определяет запланированное `Job` время выполнения. `JobTrigger` Окно выполнения имеет значение _начального окна_ и значение _конечного окна_ . Начальное окно — это количество секунд, в течение которых устройство должно ожидать перед выполнением задания, а значение конечного окна — максимальное число секунд ожидания перед запуском `Job`. 

Можно создать с помощью `Firebase.Jobdispatcher.Trigger.ExecutionWindow`метода. `JobTrigger`  Например `Trigger.ExecutionWindow(15,60)` , это означает, что задание должно выполняться от 15 до 60 секунд по расписанию. `Job.Builder.SetTrigger` Метод используется для 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

Значение по `JobTrigger` умолчанию для задания представлено значением `Trigger.Now`, которое указывает, что задание запускается как можно скорее после его планирования.

<a name="Setting_a_RetryStrategy" />

#### <a name="setting-a-retrystrategy"></a>Настройка Ретристратеги

`Firebase.JobDispatcher.RetryStrategy` Используется для указания того, сколько времени задержки должно использовать устройство, прежде чем пытаться повторно запустить невыполненное задание. A `RetryStrategy` имеет _политику_, которая определяет, какой базовый алгоритм будет использоваться для повторного планирования невыполненного задания, и окно выполнения, указывающее окно, в котором должно быть запланировано задание. Это _окно перепланирования_ определяется двумя значениями. Первое значение — это время ожидания (в секундах) перед повторным планированием задания ( _начальное значение отхода_ ), а второе число — это максимальное число секунд, по истечении которых задание должно выполняться ( _Максимальное значение отхода_ ). 

Два типа политик повтора идентифицируются следующими значениями типа int:

- `RetryStrategy.RetryPolicyExponential`Политика экспоненциальной задержки увеличит начальное значение отхода экспоненциально после каждого сбоя. &ndash; При первом сбое задания библиотека будет ожидать интервал _initial, указанный перед повторным планированием задания &ndash; , например 30 секунд. При втором сбое задания библиотека будет ожидать не менее 60 секунд перед попыткой запуска задания. После третьей неудачной попытки библиотека будет ждать 120 секунд и т. д. Значение по `RetryStrategy` умолчанию для библиотеки диспетчера заданий Firebase представлено `RetryStrategy.DefaultExponential` объектом. Он имеет первоначальный отход на 30 секунд и максимум в 3600 секунд.
- `RetryStrategy.RetryPolicyLinear`Эта стратегия является линейной отработкой, что задание должно быть запланировано на выполнение через заданные интервалы (до тех пор, пока оно не будет выполнено). &ndash; Линейная откладывание лучше всего подходит для работы, которая должна быть выполнена как можно скорее, или для проблем, которые будут быстро устранять себя. Библиотека диспетчера заданий Firebase определяет, `RetryStrategy.DefaultLinear` что содержит период перепланирования не менее 30 секунд и не более 3600 секунд.

Можно определить пользовательский `RetryStrategy` `FirebaseJobDispatcher.NewRetryStrategy` метод с помощью метода. Он принимает три параметра:

1. `int policy``RetryStrategy.RetryPolicyExponential` `RetryStrategy` `RetryStrategy.RetryPolicyLinear`Политика является одним из предыдущих значений, или. &ndash;
2. `int initialBackoffSeconds`Начальное отставание — это задержка (в секундах), необходимая перед повторной попыткой выполнения задания. &ndash; Значение по умолчанию — 30 секунд. 
3. `int maximumBackoffSeconds`Максимальное значение отхода объявляет максимальное время задержки в секундах перед повторной попыткой выполнения задания. &ndash; Значение по умолчанию — 3600 секунд. 

```csharp
RetryStrategy retry = dispatcher.NewRetryStrategy(RetryStrategy.RetryPolicyLinear, initialBackoffSeconds, maximumBackoffSet);

// Create a Job and set the RetryStrategy via the Job.Builder
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetRetryStrategy(retry)
                      .Build();
```

### <a name="cancelling-a-job"></a>Отмена задания

Можно отменить все запланированные задания или только одно задание с помощью `FirebaseJobDispatcher.CancelAll()` метода `FirebaseJobDispatcher.Cancel(string)` или метода:

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

Любой из методов возвратит целочисленное значение:

- `FirebaseJobDispatcher.CancelResultSuccess`&ndash; Задание успешно отменено.
- `FirebaseJobDispatcher.CancelResultUnknownError`&ndash; Не удалось отменить задание из – за ошибки.
- `FirebaseJobDispatcher.CancelResult.NoDriverAvailable`Не удается отменить задание, так как нет допустимых `IDriver` доступных. &ndash; `FirebaseJobDispatcher`

## <a name="summary"></a>Сводка

В этом руководство описано, как использовать диспетчер заданий Firebase для интеллектуального выполнения работы в фоновом режиме. В нем обсуждалось, как инкапсулировать работу, которая должна быть выполнена `JobService` в виде, и как `FirebaseJobDispatcher` использовать, чтобы запланировать работу, `JobTrigger` указав критерии и способ обработки ошибок с помощью `RetryStrategy`.

## <a name="related-links"></a>Связанные ссылки

- [Xamarin. Firebase. Жобдиспатчер на NuGet](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)
- [Firebase-Job-Dispatcher на GitHub](https://github.com/firebase/firebase-jobdispatcher-android)
- [Привязка Xamarin. Firebase. Жобдиспатчер](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [Интеллектуальное задание — планирование](https://developer.android.com/topic/performance/scheduling.html)
- [Батарея Android и оптимизация памяти — Google I/O 2016 (видео)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
