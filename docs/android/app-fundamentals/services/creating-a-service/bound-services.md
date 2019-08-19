---
title: Связанные службы в Xamarin. Android
description: Привязанные службы — это службы Android, предоставляющие интерфейс клиентского сервера, с которым может взаимодействовать клиент (например, действие Android). В этом руководство обсуждаются ключевые компоненты, связанные с созданием привязанной службы, а также способы ее использования в приложении Xamarin. Android.
ms.prod: xamarin
ms.assetid: 809ECE88-EF08-4E9A-B389-A2DC08C51A6E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/04/2018
ms.openlocfilehash: 6b585783f21cc18112ef766819c9851baac96ef1
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68644191"
---
# <a name="bound-services-in-xamarinandroid"></a>Связанные службы в Xamarin. Android

_Привязанные службы — это службы Android, предоставляющие интерфейс клиентского сервера, с которым может взаимодействовать клиент (например, действие Android). В этом руководство обсуждаются ключевые компоненты, связанные с созданием привязанной службы, а также способы ее использования в приложении Xamarin. Android._

## <a name="bound-services-overview"></a>Общие сведения о связанных службах

Службы, предоставляющие интерфейс клиентского сервера для взаимодействия клиентов с этой службой напрямую, называются _привязанными службами_.  Одновременно может быть несколько клиентов, одновременно подключенных к одному экземпляру службы. Привязанная служба и клиент изолированы друг от друга. Вместо этого Android предоставляет ряд промежуточных объектов, которые управляют состоянием соединения между ними. Это состояние сохраняется объектом, реализующим [`Android.Content.IServiceConnection`](xref:Android.Content.IServiceConnection) интерфейс.  Этот объект создается клиентом и передается в качестве параметра [`BindService`](xref:Android.Content.Context.BindService*) в метод. Объект доступен для любого [`Android.Content.Context`](xref:Android.Content.Context) объекта (например, действия). `BindService` Это запрос к операционной системе Android для запуска службы и привязки к ней клиента. Существует три способа привязки клиента к службе с помощью `BindService` метода:

* **Связыватель службы** Связыватель службы — это класс, [`Android.OS.IBinder`](xref:Android.OS.IBinder) реализующий интерфейс. &ndash; Большинство приложений не будут реализовывать этот интерфейс напрямую, вместо этого они будут расширять [`Android.OS.Binder`](xref:Android.OS.Binder) класс. Это наиболее распространенный подход, который подходит для случаев, когда служба и клиент находятся в одном процессе.
* **Использование программы Messenger** &ndash; Этот метод подходит для тех случаях, когда служба может существовать в отдельном процессе. Вместо этого запросы на обслуживание маршалируется между клиентом и службой через [`Android.OS.Messenger`](xref:Android.OS.Messenger). Создается в службе, которая будет `Messenger` выполнять запросы. [`Android.OS.Handler`](xref:Android.OS.Handler) Это будет рассмотрено в другом пошаговом руководству.
* **Использование языка определения интерфейса Android (AIDL)** &ndash; [AIDL](https://developer.android.com/guide/components/aidl) — это сложная методика, которая не рассматривается в этом руководством.

После привязки клиента к службе связь между ними осуществляется через `Android.OS.IBinder` объект.  Этот объект отвечает за интерфейс, который позволит клиенту взаимодействовать со службой. Для каждого приложения Xamarin. Android не требуется реализовывать этот интерфейс с нуля, пакет SDK для Android предоставляет [`Android.OS.Binder`](xref:Android.OS.Binder) класс, который занимает большую часть кода, необходимого для маршалинга объекта между клиентом и службой.

Когда клиент выполняется со службой, ему необходимо отменить привязку, вызвав `UnbindService` метод. После того, как последний клиент отменяет привязку к службе, Android останавливается и удаляет связанную службу.

На этой схеме показано, как действие, подключение службы, связыватель и служба связаны друг с другом:

![Схема, показывающая, как компоненты службы связаны друг с другом](bound-services-images/bound-services-02.png "Схема, показывающая, как компоненты службы связаны друг с другом.")

В этом руководство рассматривается, как расширить `Service` класс для реализации привязанной службы. Кроме того, он охватывает `IServiceConnection` реализацию и `Binder` расширение, позволяя клиенту взаимодействовать со службой. Пример приложения прилагается к этому руководству, которое содержит решение с одним проектом Xamarin. Android с именем **[баундсервицедемо](https://github.com/xamarin/monodroid-samples/tree/master/ApplicationFundamentals/ServiceSamples/BoundServiceDemo)** . Это очень простое приложение, которое демонстрирует, как реализовать службу и как привязать к ней действие. У привязанной службы есть очень простой API с одним методом, `GetFormattedTimestamp`который возвращает строку, сообщающую пользователю о запуске службы и времени ее выполнения. Приложение также позволяет пользователю вручную отменить привязку и привязать к службе.

[![Снимок экрана приложения, работающего на телефоне с Android](bound-services-images/bound-services-03-sml.png)](bound-services-images/bound-services-03.png#lightbox)

## <a name="implementing-and-consuming-a-bound-service"></a>Реализация и использование привязанной службы

Существует три компонента, которые необходимо реализовать, чтобы приложение Android использовало связанную службу:

1. **Расширьте`Service` класс и реализуйте методы** &ndash; обратного вызова жизненного цикла. Этот класс будет содержать код, который будет выполнять работу, которая будет запрошена службой. Более подробно эти сведения будут рассмотрены ниже.
2. **Создание класса, реализующего `IServiceConnection`**  &ndash; этот интерфейс, предоставляет методы обратного вызова, которые будут вызываться Android для уведомления клиента о том, что подключение к службе изменилось, т. е. клиент подключен или отключен к служеб. Соединение со службой также предоставит ссылку на объект, который клиент может использовать для непосредственного взаимодействия со службой. Эта ссылка называется связывателем.
3. **Создание класса, реализующего `IBinder`**  &ndash; реализацию _связывателя_ , предоставляет API, который клиент использует для взаимодействия со службой. Связыватель может предоставить ссылку на связанную службу, допуская вызов методов напрямую, или связыватель может предоставить клиентский API, который инкапсулирует и скрывает связанную службу из приложения. `IBinder` Должен предоставляться код, необходимый для удаленных вызовов процедур. Для непосредственной реализации `IBinder` интерфейса необязательно (или не рекомендуется). Вместо этого приложения должны расширять `Binder` тип, который предоставляет большую часть базовой функциональности, необходимой `IBinder`для.
4. **Запуск и привязка к службе** &ndash; После создания подключения к службе, связывателя и службы приложение Android несет ответственность за запуск службы и привязку к ней.

Каждое из этих действий будет рассмотрено в следующих разделах более подробно.

### <a name="extend-the-service-class"></a>`Service` Расширение класса

Для создания службы с помощью Xamarin. Android необходимо создать подкласс `Service` и декоративный класс [`ServiceAttribute`](xref:Android.App.ServiceAttribute)с помощью. Атрибут используется средствами сборки Xamarin. Android для правильной регистрации службы в файле **AndroidManifest. XML** приложения, подобной действию, связанная служба имеет собственный жизненный цикл и методы обратного вызова, связанные с важными событиями в Это жизненный цикл. Ниже приведен пример некоторых наиболее распространенных методов обратного вызова, которые будет реализовывать служба.

* `OnCreate`&ndash; Этот метод вызывается Android при создании экземпляра службы. Он используется для инициализации любых переменных или объектов, необходимых службе в течение всего времени существования. Этот метод является необязательным.
* `OnBind`&ndash; Этот метод должен быть реализован всеми связанными службами. Он вызывается, когда первый клиент пытается подключиться к службе. Он вернет экземпляр `IBinder` , чтобы клиент мог взаимодействовать со службой. Пока служба выполняется, `IBinder` объект будет использоваться для выполнения всех последующих клиентских запросов на привязку к службе.
* `OnUnbind`&ndash; Этот метод вызывается, когда у всех привязанных клиентов отсутствует привязка. При возврате `true` из этого метода служба будет вызываться `OnRebind` с намерением, передаваемым при привязке новых клиентов к нему. `OnUnbind` Это можно сделать, когда служба продолжит работу после ее непривязки. Это может произойти, если недавно непривязанная служба также была запущена и `StopService` `StopSelf` не была вызвана. В таком случае `OnRebind` позволяет получить намерение. По умолчанию `false` возвращает, что ничего не делает. Необязательный параметр.
* `OnDestroy`&ndash; Этот метод вызывается при уничтожении службы Android. В этом методе должны быть выполнены все необходимые операции очистки, такие как освобождение ресурсов. Необязательный параметр.

Основные события жизненного цикла привязанной службы показаны на этой схеме:

![Схема, показывающая порядок вызова методов жизненного цикла](bound-services-images/bound-services-01.png "Схема, показывающая порядок вызова методов жизненного цикла.")

В следующем фрагменте кода из сопутствующего приложения, прилагаемого к этому руководству, показано, как реализовать связанную службу в Xamarin. Android:

```csharp
using Android.App;
using Android.Util;
using Android.Content;
using Android.OS;

namespace BoundServiceDemo
{
    [Service(Name="com.xamarin.ServicesDemo1")]
    public class TimestampService : Service, IGetTimestamp
    {
        static readonly string TAG = typeof(TimestampService).FullName;
        IGetTimestamp timestamper;

        public IBinder Binder { get; private set; }

        public override void OnCreate()
        {
            // This method is optional to implement
            base.OnCreate();
            Log.Debug(TAG, "OnCreate");
            timestamper = new UtcTimestamper();
        }

        public override IBinder OnBind(Intent intent)
        {
            // This method must always be implemented
            Log.Debug(TAG, "OnBind");
            this.Binder = new TimestampBinder(this);
            return this.Binder;
        }

        public override bool OnUnbind(Intent intent)
        {
            // This method is optional to implement
            Log.Debug(TAG, "OnUnbind");
            return base.OnUnbind(intent);
        }

        public override void OnDestroy()
        {
            // This method is optional to implement
            Log.Debug(TAG, "OnDestroy");
            Binder = null;
            timestamper = null;
            base.OnDestroy();
        }

        /// <summary>
        /// This method will return a formatted timestamp to the client.
        /// </summary>
        /// <returns>A string that details what time the service started and how long it has been running.</returns>
        public string GetFormattedTimestamp()
        {
            return timestamper?.GetFormattedTimestamp();
        }
    }
}
```

В примере `OnCreate` метод инициализирует объект, содержащий логику для извлечения и форматирования метки времени, запрашиваемой клиентом. Когда первый клиент пытается выполнить привязку к службе, Android будет вызывать `OnBind` метод. Эта служба создает экземпляр `TimestampBinder` объекта, который позволит клиентам получить доступ к этому экземпляру выполняющейся службы. `TimestampBinder` Класс рассматривается в следующем разделе.

### <a name="implementing-ibinder"></a>Реализация IBinder

Как уже упоминалось `IBinder` , объект предоставляет коммуникационный канал между клиентом и службой. Приложения Android не должны реализовывать `IBinder` интерфейс [`Android.OS.Binder`](xref:Android.OS.Binder) , должен быть расширен. `Binder` Класс предоставляет большую часть необходимой инфраструктуры, необходимой для маршалирования объекта связывателя из службы (которая может выполняться в отдельном процессе) к клиенту. В большинстве случаев `Binder` подкласс представляет собой всего несколько строк кода и заключает ссылку на службу. В этом примере `TimestampBinder` имеет свойство, которое `TimestampService` предоставляет клиенту:

```csharp
public class TimestampBinder : Binder
{
    public TimestampBinder(TimestampService service)
    {
        this.Service = service;
    }

    public TimestampService Service { get; private set; }
}
```

Это `Binder` дает возможность вызывать открытые методы для службы, например:

```csharp
string currentTimestamp = serviceConnection.Binder.Service.GetFormattedTimestamp()
```

После `Binder` расширения необходимо реализовать `IServiceConnection` , чтобы соединить все вместе.

### <a name="creating-the-service-connection"></a>Создание подключения к службе

Будет представлен | представляем | открыть | `Binder` подключить объект к клиенту. `IServiceConnection` Помимо реализации `IServiceConnection` интерфейса, класс должен расширять `Java.Lang.Object`. Подключение службы также должно предоставлять некоторый способ, которым клиент может получить доступ `Binder` к (и, следовательно, к привязанной службе).

Этот код из сопровождающего примера проекта является одним из возможных способов реализации `IServiceConnection`:

```csharp
using Android.Util;
using Android.OS;
using Android.Content;

namespace BoundServiceDemo
{
    public class TimestampServiceConnection : Java.Lang.Object, IServiceConnection, IGetTimestamp
    {
        static readonly string TAG = typeof(TimestampServiceConnection).FullName;

        MainActivity mainActivity;
        public TimestampServiceConnection(MainActivity activity)
        {
            IsConnected = false;
            Binder = null;
            mainActivity = activity;
        }

        public bool IsConnected { get; private set; }
        public TimestampBinder Binder { get; private set; }

        public void OnServiceConnected(ComponentName name, IBinder service)
        {
            Binder = service as TimestampBinder;
            IsConnected = this.Binder != null;

            string message = "onServiceConnected - ";
            Log.Debug(TAG, $"OnServiceConnected {name.ClassName}");

            if (IsConnected)
            {
                message = message + " bound to service " + name.ClassName;
                mainActivity.UpdateUiForBoundService();
            }
            else
            {
                message = message + " not bound to service " + name.ClassName;
                mainActivity.UpdateUiForUnboundService();
            }

            Log.Info(TAG, message);
            mainActivity.timestampMessageTextView.Text = message;

        }

        public void OnServiceDisconnected(ComponentName name)
        {
            Log.Debug(TAG, $"OnServiceDisconnected {name.ClassName}");
            IsConnected = false;
            Binder = null;
            mainActivity.UpdateUiForUnboundService();
        }

        public string GetFormattedTimestamp()
        {
            if (!IsConnected)
            {
                return null;
            }

            return Binder?.GetFormattedTimestamp();
        }
    }
}

```

В рамках процесса привязки Android вызывает `OnServiceConnected` метод, `name` предоставляя связанную службу и объект `binder` , содержащий ссылку на саму службу. В этом примере соединение службы имеет два свойства: одно, содержащее ссылку на связыватель, и логический флаг для, если клиент подключен к службе.

`OnServiceDisconnected` Метод вызывается только в том случае, если соединение между клиентом и службой неожиданно утеряно или разорвано. Этот метод позволяет клиенту ответить на прерывание в службе.  

## <a name="starting-and-binding-to-a-service-with-an-explicit-intent"></a>Запуск и привязка к службе с явной намерением

Чтобы использовать привязанную службу, клиент (например, действие) должен создать экземпляр объекта, который реализует `Android.Content.IServiceConnection` метод и вызывает его. `BindService` `BindService`будет возвращать `true` значение, если служба привязана `false` к, если нет. Метод `BindService` принимает три следующих параметра.

* Цель должна явно указывать, к какой службе подключаться. **`Intent`** &ndash;
* **Объект`IServiceConnection`**  ,которыйпредставляетсобойпосредник,предоставляющийметодыобратноговызовадляуведомленияклиентаозапускеи&ndash; остановке привязанной службы.
* Enum этот параметр является набором флагов, используемых системой при привязке объекта. **[`Android.Content.Bind`](xref:Android.Content.Bind)** &ndash; Чаще всего используется значение [`Bind.AutoCreate`](xref:Android.Content.Bind.AutoCreate), которое автоматически запускает службу, если она еще не запущена.

В следующем фрагменте кода приведен пример того, как запустить связанную службу в действии, используя явное намерение:

```csharp
protected override void OnStart ()
{
    base.OnStart ();

    if (serviceConnection == null)
    {
        this.serviceConnection = new TimestampServiceConnection(this);
    }

    Intent serviceToStart = new Intent(this, typeof(TimestampService));
    BindService(serviceToStart, this.serviceConnection, Bind.AutoCreate);

}
```

> [!IMPORTANT]
> Начиная с Android 5,0 (уровень API 21) можно только выполнить привязку к службе с явной намерением.

## <a name="architectural-notes-about-the-service-connection-and-the-binder"></a>Заметки об архитектуре подключения службы и связывателя.

Некоторые ООП приверженцы могут отменять предыдущую реализацию `TimestampBinder` класса, так как это нарушение [закона Деметры](https://en.wikipedia.org/wiki/Law_of_Demeter) , которое в самом простом состоянии состоит в том, чтобы не поговорить с незнакомцам; Общайтесь только с друзьями. Эта конкретная реализация предоставляет конкретный `TimestampService` класс всем клиентам.

Строго говоря, клиенту не требуется знание о том `TimestampService` , предоставляет ли этот конкретный класс клиентам более нестабильным и труднее поддерживать его в течение всего времени существования. Альтернативный подход заключается в использовании интерфейса, который предоставляет `GetFormattedTimestamp()` метод, и вызовов прокси-сервера к службе `Binder` через (или возможный класс подключения службы):  

```csharp
public class TimestampBinder : Binder, IGetTimestamp
{
    TimestampService service;
    public TimestampBinder(TimestampService service)
    {
        this.service = service;
    }

    public string GetFormattedTimestamp()
    {
        return service?.GetFormattedTimestamp();
    }
}
```

Этот конкретный пример позволяет действию вызывать методы в самой службе:

```csharp
// In this example the Activity is only talking to a friend, i.e. the IGetTimestamp interface provided by the Binder.
string currentTimestamp = serviceConnection.Binder.GetFormattedTimestamp()
```


## <a name="related-links"></a>Связанные ссылки

- [Android. app. Service](xref:Android.App.Service)
- [Android.Content.Bind](xref:Android.Content.Bind)
- [Android.Content.Context](xref:Android.Content.Context)
- [Android. Content. Исервицеконнектион](xref:Android.Content.IServiceConnection)
- [Android. OS. BINDER](xref:Android.OS.Binder)
- [Android. OS. IBinder](xref:Android.OS.IBinder)
- [Баундсервицедемо (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-servicesamples-boundservicedemo)
