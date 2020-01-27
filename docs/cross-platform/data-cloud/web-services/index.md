---
title: Введение в веб-службы
description: В этом руководстве показано, как использовать различные технологии веб-служб. Рассматриваемые темы включают взаимодействие со службами RESTFUL, службами SOAP и службами Windows Communication Foundation.
ms.prod: xamarin
ms.assetid: 72627B90-586A-02B6-E231-F7CE015A1B97
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: ebd7cad9ef33a44dbc7aa469bb4e866bdfea2e61
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724785"
---
# <a name="introduction-to-web-services"></a>Введение в веб-службы

_В этом руководстве показано, как использовать различные технологии веб-служб. Рассматриваемые темы включают взаимодействие со службами RESTFUL, службами SOAP и службами Windows Communication Foundation._

Для правильной работы многие мобильные приложения зависят от облака, и поэтому интеграция веб-служб в мобильные приложения является распространенным сценарием. Платформа Xamarin поддерживает использование различных технологий веб-служб и включает в себя встроенную и сторонние службы для использования служб RESTFUL, ASMX и Windows Communication Foundation (WCF).

Для клиентов, использующих Xamarin. Forms, существуют полные примеры использования каждой из этих технологий в документации по [веб-службам Xamarin. Forms](~/xamarin-forms/data-cloud/index.yml) .

> [!IMPORTANT]
> В iOS 9 Защита транспорта приложений (ATS) обеспечивает безопасное подключение между Интернет-ресурсами (например, серверным сервером приложения) и приложением, тем самым предотвращая случайное раскрытие конфиденциальной информации.
> Поскольку ATS включена по умолчанию в приложениях, созданных для iOS 9, все подключения будут применяться требования к безопасности ATS. Если соединения не удовлетворяют этим требованиям, они вызовут сбой с исключением.

Вы можете отказаться от ATS, если невозможно использовать протокол `HTTPS` и безопасное взаимодействие с Интернет-ресурсами. Это достигается путем обновления приложения **Info.plist** файла. Дополнительные сведения см. в разделе [безопасность транспорта приложения](~/ios/app-fundamentals/ats.md).

## <a name="rest"></a>REST

Representational State Transfer (REST) представляет собой архитектурный стиль для построения веб-служб. REST запросы выполняются по протоколу HTTP, используя те же глаголы HTTP, использующих веб-браузеры для получения веб-страниц и для отправки данных на серверы. Ниже приведены команды.

- **ПОЛУЧИТЬ** — эта операция используется для получения данных из веб-службы.
- **POST** — эта операция используется для создания нового элемента данных на веб-службы.
- **ПОМЕСТИТЕ** — эта операция используется для обновления элемента данных на веб-службы.
- **ИСПРАВЛЕНИЕ** — эта операция используется для обновления элемента данных на веб-службы, описывающий набор инструкций о том, как изменить элемент. Эта команда не используется в примере приложения.
- **Удалить** — эта операция используется для удаления элемента данных на веб-службы.

Веб-служба, вызываются API-интерфейсов RESTful API-интерфейсы, которые соответствуют REST и определяются с помощью:

- Базовый URI.
- Методы HTTP, такие как GET, POST, PUT, PATCH или DELETE.
- Тип носителя для данных, таких как JavaScript Object Notation (JSON).

Простота REST сделала его основной метод для доступа к веб-служб в мобильных приложениях.

## <a name="consuming-rest-services"></a>Использование служб RESTFUL

Существует несколько библиотек и классов, которые можно использовать для использования служб RESTFUL, а следующие подразделы обсуждают их. Дополнительные сведения об использовании службы RESTFUL см. [в статье Использование веб-службы RESTful](~/xamarin-forms/data-cloud/web-services/rest.md).

### <a name="httpclient"></a>HttpClient

[Клиентские библиотеки Microsoft HTTP](https://www.nuget.org/packages/Microsoft.Net.Http) предоставляют класс `HttpClient`, который используется для отправки и получения запросов по протоколу HTTP. Он предоставляет функциональные возможности для отправки HTTP-запросов и получения HTTP-ответов из ресурса, идентифицированного с помощью URI. Каждый запрос отправляется в качестве асинхронной операции. Дополнительные сведения об асинхронных операциях см. в разделе [обзор поддержки асинхронного](~/cross-platform/platform/async.md).

`HttpResponseMessage` Класс представляет сообщение ответа HTTP, полученные из веб-службы, после HTTP-запроса. Он содержит сведения о ответе, включая код состояния, заголовки и текст. `HttpContent` Класс представляет тело HTTP и заголовки содержимого, таких как `Content-Type` и `Content-Encoding`. Можно прочитать содержимое с помощью любого из `ReadAs` методы, такие как `ReadAsStringAsync` и `ReadAsByteArrayAsync`, в зависимости от формата данных.

Дополнительные сведения о классе `HttpClient` см. в разделе [Создание объекта HttpClient](~/xamarin-forms/data-cloud/web-services/rest.md).

<a name="Using_HTTPWebRequest" />

### <a name="httpwebrequest"></a>HTTPWebRequest

Вызов веб-служб с `HTTPWebRequest` включает в себя следующее:

- Создание экземпляра запроса для конкретного URI.
- Задание различных свойств HTTP для экземпляра запроса.
- Получение `HttpWebResponse` из запроса.
- Считывание данных из ответа.

Например, следующий код извлекает данные из национальной библиотеки США веб-службы медицина:

```csharp
var rxcui = "198440";
var request = HttpWebRequest.Create(string.Format(@"https://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui));
request.ContentType = "application/json";
request.Method = "GET";

using (HttpWebResponse response = request.GetResponse() as HttpWebResponse)
{
  if (response.StatusCode != HttpStatusCode.OK)
     Console.Out.WriteLine("Error fetching data. Server returned status code: {0}", response.StatusCode);
  using (StreamReader reader = new StreamReader(response.GetResponseStream()))
  {
               var content = reader.ReadToEnd();
               if(string.IsNullOrWhiteSpace(content)) {
                       Console.Out.WriteLine("Response contained empty body...");
               }
               else {
                       Console.Out.WriteLine("Response Body: \r\n {0}", content);
               }

               Assert.NotNull(content);
  }
}
```

В приведенном выше примере создается `HttpWebRequest`, который будет возвращать данные в формате JSON. Данные возвращаются в `HttpWebResponse`, из которого можно получить `StreamReader` для чтения данных.

<a name="Using_RESTSHARP" />

### <a name="restsharp"></a>RestSharp

Другой подход к использованию служб RESTFUL — использование библиотеки [рестшарп](http://restsharp.org/) . Рестшарп инкапсулирует HTTP-запросы, включая поддержку извлечения результатов в виде необработанной строки или в виде десериализованного C# объекта. Например, следующий код выполняет запрос к международной библиотеке медицина веб-службы США и получает результаты в виде строки в формате JSON:

```csharp
var request = new RestRequest(string.Format("{0}/allinfo", rxcui));
request.RequestFormat = DataFormat.Json;
var response = Client.Execute(request);
if(string.IsNullOrWhiteSpace(response.Content) || response.StatusCode != System.Net.HttpStatusCode.OK) {
       return null;
}
rxTerm = DeserializeRxTerm(response.Content);
```

`DeserializeRxTerm` — это метод, который принимает необработанную строку JSON из свойства `RestSharp.RestResponse.Content` и преобразует ее в C# объект. Десериализация данных, возвращаемых веб-службами, обсуждается далее в этой статье.

<a name="Using_NSUrlconnection" />

### <a name="nsurlconnection"></a>нсурлконнектион

Помимо классов, доступных в библиотеке базовых классов Mono (BCL), например `HttpWebRequest`и сторонних C# библиотек, таких как рестшарп, для использования веб-служб также доступны классы для конкретных платформ. Например, в iOS можно использовать классы `NSUrlConnection` и `NSMutableUrlRequest`.

В следующем примере кода показано, как вызвать стандартную библиотеку США для веб-службы медицина с помощью классов iOS:

```csharp
var rxcui = "198440";
var request = new NSMutableUrlRequest(new NSUrl(string.Format("https://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui)),
       NSUrlRequestCachePolicy.ReloadRevalidatingCacheData, 20);
request["Accept"] = "application/json";

var connectionDelegate = new RxTermNSURLConnectionDelegate();
var connection = new NSUrlConnection(request, connectionDelegate);
connection.Start();

public class RxTermNSURLConnectionDelegate : NSUrlConnectionDelegate
{
       StringBuilder _ResponseBuilder;
       public bool IsFinishedLoading { get; set; }
       public string ResponseContent { get; set; }

       public RxTermNSURLConnectionDelegate()
               : base()
       {
               _ResponseBuilder = new StringBuilder();
       }

       public override void ReceivedData(NSUrlConnection connection, NSData data)
       {
               if(data != null) {
                       _ResponseBuilder.Append(data.ToString());
               }
       }
       public override void FinishedLoading(NSUrlConnection connection)
       {
               IsFinishedLoading = true;
               ResponseContent = _ResponseBuilder.ToString();
       }
}
```

Как правило, классы платформы для использования веб-служб должны быть ограничены сценариями, в C#которых выполняется перенос машинного кода. Там, где это возможно, код доступа к веб-службе должен быть переносимым, чтобы его можно было совместно использовать на разных платформах.

<a name="Using_ServiceStack_Client" />

### <a name="servicestack"></a>ServiceStack

Другим вариантом вызова веб-служб является библиотека [стека обслуживания](https://servicestack.net) . Например, в следующем коде показано, как использовать метод `IServiceClient.GetAsync` в стеке службы для выдаче запроса на обслуживание:

```csharp
client.GetAsync<CustomersResponse>("",
          (response) => {
               foreach(var c in response.Customers) {
                       Console.WriteLine(c.CompanyName);
               }
       },
       (response, ex) => {
               Console.WriteLine(ex.Message);
       });
```

> [!IMPORTANT]
> Хотя такие средства, как ServiceStack и Рестшарп, упрощают вызов и использование служб RESTFUL, иногда нетривиальным является использование XML или JSON, который не соответствует стандартным соглашениям о сериализации _DataContract_ . При необходимости вызовите запрос и выполните соответствующую сериализацию явным образом с помощью библиотеки ServiceStack. Text, описанной ниже.

<a name="Options_for_consuming_RESTful_data" />

## <a name="consuming-restful-data"></a>Использование данных RESTFUL

Сообщения JSON обычно использовать веб-служб rESTful для возврата данных клиенту. JSON — это текстовый формат обмена данными, создающий компактные полезные данные, что приводит к снижению требований к пропускной способности при отправке данных. В этом разделе будут проверяться механизмы использования ответов RESTFUL в JSON и обычных-Old-XML (POX).

<a name="Using_System.JSON" />

### <a name="systemjson"></a>System. JSON

Платформа Xamarin поставляется без поддержки JSON. С помощью `JsonObject`можно извлечь результаты, как показано в следующем примере кода:

```csharp
var obj = JsonObject.Parse(json);
var properties = obj["rxtermsProperties"];
term.BrandName = properties["brandName"];
term.DisplayName = properties["displayName"];
term.Synonym = properties["synonym"];
term.FullName = properties["fullName"];
term.FullGenericName = properties["fullGenericName"];
term.Strength = properties["strength"];
```

Однако важно помнить, что средства `System.Json` загружают все данные в память.

<a name="Using_JSON.NET" />

### <a name="jsonnet"></a>JSON.NET

[Библиотека NewtonSoft JSON.NET](https://www.newtonsoft.com/json) — широко используемая библиотека для сериализации и десериализации сообщений JSON. В следующем примере кода показано, как использовать JSON.NET для десериализации сообщения JSON в C# объект:

```csharp
var term = new RxTerm();
var properties = JObject.Parse(json)["rxtermsProperties"];
term.BrandName = properties["brandName"].Value<string>();
term.DisplayName = properties["displayName"].Value<string>();
term.Synonym = properties["synonym"].Value<string>();;
term.FullName = properties["fullName"].Value<string>();;
term.FullGenericName = properties["fullGenericName"].Value<string>();;
term.Strength = properties["strength"].Value<string>();
term.RxCUI = properties["rxcui"].Value<string>();
```

<a name="Using_ServiceStack.Text" />

### <a name="servicestacktext"></a>ServiceStack. Text

ServiceStack. Text — это библиотека сериализации JSON, предназначенная для работы с библиотекой ServiceStack. В следующем примере кода показано, как выполнить синтаксический анализ JSON с помощью `ServiceStack.Text.JsonObject`.

```csharp
var result = JsonObject.Parse(json).Object("rxtermsProperties")
       .ConvertTo(x => new RxTerm {
               BrandName = x.Get("brandName"),
               DisplayName = x.Get("displayName"),
               Synonym = x.Get("synonym"),
               FullName = x.Get("fullName"),
               FullGenericName = x.Get("fullGenericName"),
               Strength = x.Get("strength"),
               RxTermDoseForm = x.Get("rxtermsDoseForm"),
               Route = x.Get("route"),
               RxCUI = x.Get("rxcui"),
               RxNormDoseForm = x.Get("rxnormDoseForm"),
       });
```

<a name="Using_System.Xml.Linq" />

### <a name="systemxmllinq"></a>System.Xml.Linq

В случае использования веб-службы RESTFUL на основе XML LINQ to XML можно использовать для анализа XML и заполнения C# объекта встроенным, как показано в следующем примере кода:

```csharp
var doc = XDocument.Parse(xml);
var result = doc.Root.Descendants("rxtermsProperties")
.Select(x=> new RxTerm()
       {
               BrandName = x.Element("brandName").Value,
               DisplayName = x.Element("displayName").Value,
               Synonym = x.Element("synonym").Value,
               FullName = x.Element("fullName").Value,
               FullGenericName = x.Element("fullGenericName").Value,
               //bind more here...
               RxCUI = x.Element("rxcui").Value,
       });
```

<a name="asmx" />

## <a name="aspnet-web-service-asmx"></a>Веб-служба ASP.NET (ASMX)

ASMX предоставляет возможность создавать веб-службы, которые отправляют сообщения с помощью протокола SOAP. SOAP — это протокол независимо от платформы и независимый от языка, для создания и вызова веб-служб. Потребителям службы ASMX не нужно ничего знать об используемой платформе, объектной модели или язык программирования, используемый для реализации службы. Они должны понять, как отправлять и получать сообщения SOAP.

Сообщение SOAP — это документ XML, содержащий следующие элементы:

- Корневой элемент с именем *конверт* , идентифицирующий XML-документа, как сообщение SOAP.
- Необязательный *заголовок* элемент, содержащий сведения о приложении, такие как данные проверки подлинности. Если *заголовок* присутствует элемент он должен быть первый дочерний элемент элемента *конверт* элемент.
- Обязательный *текст* элемент, содержащий сообщение SOAP, предназначенное для получателя.
- Необязательный *сбоя* элемент, который используется для указания сообщения об ошибках. Если *сбоя* элемент присутствует, он должен быть дочерним элементом элемента *текст* элемент.

SOAP может работать много транспортных протоколов, включая HTTP, SMTP, TCP и UDP. Тем не менее службы ASMX могут работать только по протоколу HTTP. Платформа Xamarin поддерживает стандартные реализации SOAP 1.1 по протоколу HTTP, и он также поддерживает многие из стандартных конфигураций службы ASMX.

### <a name="generating-a-proxy"></a>Создание учетной записи-посредника

Для использования службы ASMX необходимо создать *прокси-сервер* , который позволяет приложению подключаться к службе. Прокси создается с помощью много метаданных службы, определяющий методы и связанные службы конфигурации. Эти метаданные предоставляются в виде документа языка описания веб-служб (WSDL), созданного веб-службой. Прокси-сервер строится с помощью Visual Studio для Mac или Visual Studio, чтобы добавить веб-ссылку на веб – службу для проектов конкретной платформы.

URL веб-службы может быть размещенным удаленным источником или локальным ресурсом файловой системы, доступным по префиксу пути `file:///`, например:

```csharp
file:///Users/myUserName/projects/MyProjectName/service.wsdl
```

[![](images/add-webreference-dialog.png "The web service URL can either be a hosted remote source or local file system resource accessible via the file path prefix")](images/add-webreference-dialog.png#lightbox)

При этом создается прокси-сервер в папке Web или Service References проекта. Так как прокси-сервер создан, он не должен изменяться.

<a name="Manually_adding_a_proxy_to_a_project" />

#### <a name="manually-adding-a-proxy-to-a-project"></a>Добавление прокси-сервера в проект вручную

При наличии существующего прокси-сервера, созданного с помощью совместимых средств, эти выходные данные можно использовать при включении в состав проекта. В Visual Studio для Mac используйте **Добавление файлов...** пункт меню, чтобы добавить прокси-сервер. Кроме того, для этого требуется явная ссылка на *System. Web. Services. dll* с помощью команды **Добавить ссылки...** .

### <a name="consuming-the-proxy"></a>Использование прокси-сервера

Созданный прокси-классы предоставляют методы для использования веб-службы, использующих шаблон проектирования модели асинхронного программирования (APM). В этом шаблоне асинхронную операцию реализуется в виде двух методов с именем *BeginOperationName* и *EndOperationName*, который начинают и завершают асинхронную операцию.

*BeginOperationName* метод начинает асинхронную операцию и возвращает объект, реализующий `IAsyncResult` интерфейс. После вызова метода *BeginOperationName*, приложение может продолжить выполнение инструкций в вызывающем потоке, пока асинхронная операция выполняется в потоке пула потоков.

Для каждого вызова *BeginOperationName*, приложение также должно вызвать *EndOperationName* для получения результатов операции. Возвращаемое значение *EndOperationName* имеет тот же тип, возвращаемый методом синхронной веб-службы. В следующем коде показан пример такого действия:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync<ASMXService.TodoItem[]> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);
  ...
}
```

Библиотека параллельных задач (TPL) можно упростить процесс потребляет пару методов begin и end APM инкапсуляция асинхронных операций в том же `Task` объекта. Такая инкапсуляция предоставляется несколько перегрузок `Task.Factory.FromAsync` метод. Этот метод создает `Task`, который выполняет метод `TodoService.EndGetTodoItems` после завершения метода `TodoService.BeginGetTodoItems` с параметром `null`, указывающим, что данные не передаются в делегат `BeginGetTodoItems`. Наконец, значение `TaskCreationOptions` перечисление указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

Дополнительные сведения о APM см. в статьях [асинхронная модель программирования](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) и [TPL и традиционная .NET Framework асинхронное программирование](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) на сайте MSDN.

Дополнительные сведения об использовании службы ASMX см. [в статье Использование веб-службы ASP.NET (ASMX)](~/xamarin-forms/data-cloud/web-services/asmx.md).

<a name="wcf" />

## <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

WCF — это единая платформа Майкрософт для создания приложений, ориентированных на службы. Он позволяет разработчикам создавать безопасные, надежные, с поддержкой транзакций с возможностью взаимодействия распределенных приложений.

WCF описывает службу, с различными разные контракты, в том числе следующие:

- **Контракты данных** — определение структуры данных, которые формируют основу для содержимого сообщения.
- **Контракты сообщений** — создания сообщений из контракты данных.
- **Контракты сбоя** — разрешить пользовательских ошибок SOAP указать.
- **Контракты службы** — определить операции, которые поддерживают службы и сообщения, необходимые для взаимодействия с каждой операцией. Они также определяют любое поведение пользовательской ошибки, которое может быть связан с операциями с каждой службы.

Существуют различия между ASP.NET Web Services (ASMX) и WCF, но важно понимать, что WCF поддерживает те же возможности, которые предоставляет ASMX-сообщений SOAP по протоколу HTTP.

> [!IMPORTANT]
> Поддержка платформы Xamarin для WCF ограничена текстовыми сообщениями SOAP по протоколу HTTP/HTTPS с помощью класса `BasicHttpBinding`. Кроме того поддержка WCF требует применения средств доступно только в среде Windows для создания прокси-сервера.

### <a name="generating-a-proxy"></a>Создание учетной записи-посредника

Объект *прокси* должен быть создан для использования службы WCF, что позволяет приложению для подключения к службе. Прокси создается с помощью много метаданных службы, определяющий методы и связанные службы конфигурации. Эти метаданные предоставляются в виде документа языка описания служб (WSDL), который создается веб-службой. Прокси-сервер можно построить с помощью Microsoft WCF Web Service Reference Provider в Visual Studio 2017, чтобы добавить ссылку на службу для веб-службы в библиотеку .NET Standard.

Вместо создания прокси с помощью Microsoft WCF Web Service Reference Provider в Visual Studio 2017 является использование ServiceModel Metadata Utility Tool (svcutil.exe). Дополнительные сведения см. в разделе [ServiceModel Metadata Utility Tool (Svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe).

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

### <a name="configuring-the-proxy"></a>Настройка прокси-сервера

Настройка созданного прокси-сервера обычно принимает два аргумента конфигурации (в зависимости от SOAP 1.1/ASMX или WCF) во время инициализации: `EndpointAddress` и (или) связанные сведения о привязке, как показано в следующем примере:

```csharp
var binding = new BasicHttpBinding () {
       Name= "basicHttpBinding",
       MaxReceivedMessageSize = 67108864,
};

binding.ReaderQuotas = new System.Xml.XmlDictionaryReaderQuotas() {
       MaxArrayLength = 2147483646,
       MaxStringContentLength = 5242880,
};

var timeout = new TimeSpan(0,1,0);
binding.SendTimeout= timeout;
binding.OpenTimeout = timeout;
binding.ReceiveTimeout = timeout;

client = new Service1Client (binding, new EndpointAddress ("http://192.168.1.100/Service1.svc"));
```

Привязка используется для указания транспорта, кодировки и протокола, требуемых для приложений и служб для взаимодействия друг с другом. `BasicHttpBinding` Указывает, что по транспортному протоколу HTTP будет отправляться кодировке текста сообщений SOAP. Задание адреса конечной точки позволяет приложению подключаться к различным экземплярам службы WCF, при условии, что несколько экземпляров, опубликованных.

### <a name="consuming-the-proxy"></a>Использование прокси-сервера

Созданный прокси-классы предоставляют методы для использования веб-служб, использующих шаблон проектирования модели асинхронного программирования (APM). В этом случае асинхронная операция реализуется как два метода с именем *BeginOperationName* и *EndOperationName*, который начинают и завершают асинхронную операцию.

*BeginOperationName* метод начинает асинхронную операцию и возвращает объект, реализующий `IAsyncResult` интерфейс. После вызова метода *BeginOperationName*, приложение может продолжить выполнение инструкций в вызывающем потоке, пока асинхронная операция выполняется в потоке пула потоков.

Для каждого вызова *BeginOperationName*, приложение также должно вызвать *EndOperationName* для получения результатов операции. Возвращаемое значение *EndOperationName* имеет тот же тип, возвращаемый методом синхронной веб-службы. В следующем коде показан пример такого действия:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync <ObservableCollection<TodoWCFService.TodoItem>> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);
  ...
}
```

Библиотека параллельных задач (TPL) можно упростить процесс потребляет пару методов begin и end APM инкапсуляция асинхронных операций в том же `Task` объекта. Такая инкапсуляция предоставляется несколько перегрузок `Task.Factory.FromAsync` метод. Этот метод создает `Task`, который выполняет метод `TodoServiceClient.EndGetTodoItems` после завершения метода `TodoServiceClient.BeginGetTodoItems` с параметром `null`, указывающим, что данные не передаются в делегат `BeginGetTodoItems`. Наконец, значение `TaskCreationOptions` перечисление указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

Дополнительные сведения о APM см. в статьях [асинхронная модель программирования](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) и [TPL и традиционная .NET Framework асинхронное программирование](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) на сайте MSDN.

Дополнительные сведения об использовании службы WCF см. [в разделе Использование веб-службы Windows Communication Foundation (WCF)](~/xamarin-forms/data-cloud/web-services/wcf.md).

<a name="Calling_a_WCF_Service_with_Transport_Security" />

#### <a name="using-transport-security"></a>Использование безопасности транспорта

Службы WCF могут применять безопасность транспортного уровня для защиты от перехвата сообщений. Платформа Xamarin поддерживает привязки, которые используют безопасность на транспортном уровне с помощью SSL. Однако могут возникнуть ситуации, в которых стеку может потребоваться проверить сертификат, что приводит к непредвиденному поведению. Проверку можно переопределить, зарегистрировав делегат `ServerCertificateValidationCallback` перед вызовом службы, как показано в следующем примере кода:

```csharp
System.Net.ServicePointManager.ServerCertificateValidationCallback +=
(se, cert, chain, sslerror) => { return true; };
```

Это обеспечивает шифрование транспорта при игнорировании проверки сертификата на стороне сервера. Однако этот подход эффективно игнорирует проблемы доверия, связанные с сертификатом, и может быть неуместной. Дополнительные сведения см. в разделе [using Trusted корни респектфулли](https://www.mono-project.com/UsingTrustedRootsRespectfully) On [Mono-Project.com](https://www.mono-project.com).

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

#### <a name="using-client-credential-security"></a>Использование безопасности учетных данных клиента

Службы WCF также могут требовать, чтобы клиенты службы прошли проверку подлинности с помощью учетных данных. Платформа Xamarin не поддерживает протокол WS-Security, который позволяет клиентам передавать учетные данные внутри конверта сообщения SOAP. Однако платформа Xamarin поддерживает возможность отправки учетных данных обычной проверки подлинности HTTP на сервер, указав соответствующие `ClientCredentialType`.

```csharp
basicHttpBinding.Security.Transport.ClientCredentialType = HttpClientCredentialType.Basic;
```

Затем можно указать учетные данные обычной проверки подлинности:

```csharp
client.ClientCredentials.UserName.UserName = @"foo";
client.ClientCredentials.UserName.Password = @"mrsnuggles";
```

В приведенном выше примере, если вы получаете сообщение "закончилось из трамполинес типа 0", можно увеличить число трамполинес типа 0, добавив аргумент `–aot “trampolines={number of trampolines}”` в сборку. Дополнительные сведения см. в [руководстве по устранению неполадок](~/ios/troubleshooting/troubleshooting.md#trampolines).

Дополнительные сведения о обычной проверке подлинности HTTP, хотя в контексте веб-службы RESTFUL, см. в разделе [Проверка подлинности веб-службы RESTful](~/xamarin-forms/data-cloud/authentication/rest.md).

## <a name="related-links"></a>Связанные ссылки

- [Веб-службы в Xamarin. Forms](~/xamarin-forms/data-cloud/index.yml)
- [Средство служебной программы метаданных ServiceModel (Svcutil. exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
- [BasicHttpBinding](https://msdn.microsoft.com/library/system.servicemodel.basichttpbinding.aspx)
