---
title: Связанные службы в Xamarin. Android
description: Привязанные службы — это службы Android, предоставляющие интерфейс клиентского сервера, с которым может взаимодействовать клиент (например, действие Android). В этом руководство обсуждаются ключевые компоненты, связанные с созданием привязанной службы, а также способы ее использования в приложении Xamarin. Android.
ms.prod: xamarin
ms.assetid: 809ECE88-EF08-4E9A-B389-A2DC08C51A6E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/04/2018
ms.openlocfilehash: 584f523446584192cfa882697c0f76865ce78a10
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "70754929"
---
# <a name="bound-services-in-xamarinandroid"></a>Связанные службы в Xamarin. Android

_Привязанные службы — это службы Android, предоставляющие интерфейс клиентского сервера, с которым может взаимодействовать клиент (например, действие Android). В этом руководство обсуждаются ключевые компоненты, связанные с созданием привязанной службы, а также способы ее использования в приложении Xamarin. Android._

## <a name="bound-services-overview"></a>Общие сведения о связанных службах

Службы, предоставляющие интерфейс клиентского сервера для взаимодействия клиентов с этой службой напрямую, называются _привязанными службами_.  Одновременно может быть несколько клиентов, одновременно подключенных к одному экземпляру службы. Привязанная служба и клиент изолированы друг от друга. Вместо этого Android предоставляет ряд промежуточных объектов, которые управляют состоянием соединения между ними. Это состояние сохраняется объектом, реализующим интерфейс [`Android.Content.IServiceConnection`](xref:Android.Content.IServiceConnection) .  Этот объект создается клиентом и передается в качестве параметра методу [`BindService`](xref:Android.Content.Context.BindService*) . @No__t_0 доступен для любого объекта [`Android.Content.Context`](xref:Android.Content.Context) (например, действия). Это запрос к операционной системе Android для запуска службы и привязки к ней клиента. Существует три способа привязки клиента к службе с помощью метода `BindService`:

- **Связыватель службы** &ndash; связывателя службы — это класс, реализующий интерфейс [`Android.OS.IBinder`](xref:Android.OS.IBinder) . Большинство приложений не будут реализовывать этот интерфейс напрямую, вместо этого они расширяют класс [`Android.OS.Binder`](xref:Android.OS.Binder) . Это наиболее распространенный подход, который подходит для случаев, когда служба и клиент находятся в одном процессе.
- **Использование Messenger** &ndash; этот прием подходит для тех случаях, когда служба может существовать в отдельном процессе. Вместо этого запросы на обслуживание передаются между клиентом и службой с помощью [`Android.OS.Messenger`](xref:Android.OS.Messenger). В службе будет создан [`Android.OS.Handler`](xref:Android.OS.Handler) , обрабатывающий запросы `Messenger`. Это будет рассмотрено в другом пошаговом руководству.
- **Использование языка определения интерфейсов Android (AIDL)** &ndash; [AIDL](https://developer.android.com/guide/components/aidl) является расширенной методикой, которая не рассматривается в этом руководством.

После привязки клиента к службе связь между ними происходит через объект `Android.OS.IBinder`.  Этот объект отвечает за интерфейс, который позволит клиенту взаимодействовать со службой. Для каждого приложения Xamarin. Android не требуется реализовывать этот интерфейс с нуля, пакет SDK для Android предоставляет класс [`Android.OS.Binder`](xref:Android.OS.Binder) , который занимает большую часть кода, необходимого для маршалинга объекта между клиентом и службой.

Когда клиент работает со службой, ему необходимо отменить привязку, вызвав метод `UnbindService`. После того, как последний клиент отменяет привязку к службе, Android останавливается и удаляет связанную службу.

На этой схеме показано, как действие, подключение службы, связыватель и служба связаны друг с другом:

![Схема, показывающая, как компоненты службы связаны друг с другом](bound-services-images/bound-services-02.png "Схема, показывающая, как компоненты службы связаны друг с другом.")

В этом руководство описано, как расширить класс `Service` для реализации привязанной службы. Кроме того, в нем рассматривается реализация `IServiceConnection` и расширение `Binder`, позволяющее клиенту взаимодействовать со службой. Пример приложения прилагается к этому руководству, которое содержит решение с одним проектом Xamarin. Android с именем **[баундсервицедемо](https://github.com/xamarin/monodroid-samples/tree/master/ApplicationFundamentals/ServiceSamples/BoundServiceDemo)** . Это очень простое приложение, которое демонстрирует, как реализовать службу и как привязать к ней действие. У привязанной службы есть очень простой API с одним методом, `GetFormattedTimestamp`, который возвращает строку, сообщающую пользователю о запуске службы и времени ее выполнения. Приложение также позволяет пользователю вручную отменить привязку и привязать к службе.

[![Screenshot приложения, работающего на телефоне с Android](bound-services-images/bound-services-03-sml.png)](bound-services-images/bound-services-03.png#lightbox)

## <a name="implementing-and-consuming-a-bound-service"></a>Реализация и использование привязанной службы

Существует три компонента, которые необходимо реализовать, чтобы приложение Android использовало связанную службу:

1. **Расширьте класс `Service` и реализуйте методы обратного вызова жизненного цикла** &ndash; этот класс будет содержать код, который будет выполнять работу, которая будет запрошена службой. Более подробно эти сведения будут рассмотрены ниже.
2. **Создание класса, реализующего `IServiceConnection`** &ndash; этот интерфейс предоставляет методы обратного вызова, которые будут вызываться Android для уведомления клиента о том, что подключение к службе изменилось, т. е. клиент подключился к службе или отключится к ней. Соединение со службой также предоставит ссылку на объект, который клиент может использовать для непосредственного взаимодействия со службой. Эта ссылка называется _связывателем_.
3. **Создание класса, реализующего `IBinder`** &ndash; реализация _связывателя_ предоставляет API, используемый клиентом для взаимодействия со службой. Связыватель может предоставить ссылку на связанную службу, допуская вызов методов напрямую, или связыватель может предоставить клиентский API, который инкапсулирует и скрывает связанную службу из приложения. @No__t_0 должен предоставлять необходимый код для удаленных вызовов процедур. Необязательно (или рекомендуется) для непосредственной реализации интерфейса `IBinder`. Вместо этого приложения должны расширять тип `Binder`, который предоставляет большую часть базовой функциональности, необходимой для `IBinder`.
4. **Запуск и привязка к службе** &ndash; после создания подключения службы, связывателя и службы приложение Android отвечает за запуск службы и привязку к ней.

Каждое из этих действий будет рассмотрено в следующих разделах более подробно.

### <a name="extend-the-service-class"></a>Расширение класса `Service`

Чтобы создать службу с помощью Xamarin. Android, необходимо использовать подкласс `Service` и для украшения класса с [`ServiceAttribute`](xref:Android.App.ServiceAttribute). Атрибут используется средствами сборки Xamarin. Android для правильной регистрации службы в файле **AndroidManifest. XML** приложения, подобной действию, связанная служба имеет собственный жизненный цикл и методы обратного вызова, связанные с важными событиями в Это жизненный цикл. Ниже приведен пример некоторых наиболее распространенных методов обратного вызова, которые будет реализовывать служба.

- `OnCreate` &ndash; этот метод вызывается Android при создании экземпляра службы. Он используется для инициализации любых переменных или объектов, необходимых службе в течение всего времени существования. Этот метод является необязательным.
- `OnBind` &ndash; этот метод должен быть реализован всеми связанными службами. Он вызывается, когда первый клиент пытается подключиться к службе. Он вернет экземпляр `IBinder`, чтобы клиент мог взаимодействовать со службой. Пока служба выполняется, объект `IBinder` будет использоваться для выполнения всех будущих клиентских запросов для привязки к службе.
- `OnUnbind` &ndash; этот метод вызывается, когда у всех привязанных клиентов отсутствует привязка. После возврата `true` из этого метода служба будет вызывать `OnRebind` с намерением, переданным в `OnUnbind` при привязке к ней новых клиентов. Это можно сделать, когда служба продолжит работу после ее непривязки. Это могло произойти, если недавно непривязанная служба была запущена и `StopService` или `StopSelf` не вызывался. В таком случае `OnRebind` позволяет получить цель. Значение по умолчанию возвращает `false`, что не выполняет никаких действий. Необязательный.
- `OnDestroy` &ndash; этот метод вызывается при уничтожении службы Android. В этом методе должны быть выполнены все необходимые операции очистки, такие как освобождение ресурсов. Необязательный.

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

В примере метод `OnCreate` Инициализирует объект, содержащий логику для извлечения и форматирования метки времени, запрашиваемой клиентом. Когда первый клиент пытается выполнить привязку к службе, Android будет вызывать метод `OnBind`. Эта служба создает экземпляр объекта `TimestampBinder`, который позволит клиентам получать доступ к этому экземпляру выполняющейся службы. Класс `TimestampBinder` обсуждается в следующем разделе.

### <a name="implementing-ibinder"></a>Реализация IBinder

Как уже упоминалось, объект `IBinder` предоставляет коммуникационный канал между клиентом и службой. Приложения Android не должны реализовывать интерфейс `IBinder`, [`Android.OS.Binder`](xref:Android.OS.Binder) должны быть расширены. Класс `Binder` предоставляет большую часть необходимой инфраструктуры, необходимой для маршалирования объекта связывателя из службы (которая может выполняться в отдельном процессе) к клиенту. В большинстве случаев `Binder` подкласс состоит всего лишь из нескольких строк кода и заключает ссылку на службу. В этом примере `TimestampBinder` имеет свойство, которое предоставляет клиенту `TimestampService`:

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

Этот `Binder` позволяет вызывать открытые методы в службе; Например:

```csharp
string currentTimestamp = serviceConnection.Binder.Service.GetFormattedTimestamp()
```

После расширения `Binder` необходимо реализовать `IServiceConnection`, чтобы соединить все вместе.

### <a name="creating-the-service-connection"></a>Создание подключения к службе

@No__t_0 будет представлено | появление | предоставить | подключить объект `Binder` к клиенту. Помимо реализации интерфейса `IServiceConnection`, класс должен расширять `Java.Lang.Object`. Подключение службы также должно предоставлять некоторый способ, которым клиент может получить доступ к `Binder` (и, следовательно, взаимодействовать со связанной службой).

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

В рамках процесса привязки Android вызывает метод `OnServiceConnected`, предоставляя `name` привязанной службы и `binder`, содержащую ссылку на саму службу. В этом примере соединение службы имеет два свойства: одно, содержащее ссылку на связыватель, и логический флаг для, если клиент подключен к службе.

Метод `OnServiceDisconnected` вызывается только в том случае, если соединение между клиентом и службой неожиданно потеряно или разорвано. Этот метод позволяет клиенту ответить на прерывание в службе.  

## <a name="starting-and-binding-to-a-service-with-an-explicit-intent"></a>Запуск и привязка к службе с явной намерением

Чтобы использовать связанную службу, клиент (например, действие) должен создать экземпляр объекта, который реализует `Android.Content.IServiceConnection` и вызвать метод `BindService`. `BindService` вернет `true`, если служба привязана к, `false` в противном случае. Метод `BindService` принимает три следующих параметра.

- **@No__t_1** &ndash; цель должна явно указать, к какой службе нужно подключиться.
- **Объект `IServiceConnection`** &ndash; этот объект является посредником, предоставляющим методы обратного вызова для уведомления клиента о запуске и остановке связанной службы.
- **[`Android.Content.Bind`](xref:Android.Content.Bind) enum** &ndash; этот параметр является набором флагов, используемых системой при привязке объекта. Чаще всего используется значение [`Bind.AutoCreate`](xref:Android.Content.Bind.AutoCreate), которое автоматически запускает службу, если она еще не запущена.

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

Некоторые ООП приверженцы могут отменять предыдущую реализацию класса `TimestampBinder`, так как это нарушение [закона Деметры](https://en.wikipedia.org/wiki/Law_of_Demeter) , которое в самом простом состоянии состоит в том, чтобы не поговорить с незнакомцам; Общайтесь только с друзьями. Эта конкретная реализация предоставляет конкретный класс `TimestampService` всем клиентам.

Строго говоря, клиенту не требуется знание о `TimestampService` и предоставление клиентам доступа к конкретному классу, чтобы сделать приложение более нестабильнымным и трудным для поддержания в течение всего времени существования. Альтернативным подходом является использование интерфейса, предоставляющего метод `GetFormattedTimestamp()`, и прокси-вызовы к службе через `Binder` (или, возможно, класс подключения службы):  

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
- [Android. Content. Bind](xref:Android.Content.Bind)
- [Android. Content. Context](xref:Android.Content.Context)
- [Android. Content. Исервицеконнектион](xref:Android.Content.IServiceConnection)
- [Android. OS. BINDER](xref:Android.OS.Binder)
- [Android. OS. IBinder](xref:Android.OS.IBinder)
- [Баундсервицедемо (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-servicesamples-boundservicedemo)
