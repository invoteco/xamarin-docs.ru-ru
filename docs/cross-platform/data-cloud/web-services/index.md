---
title: Введение в веб-служб
description: В этом руководстве показано, как использовать технологии разных веб-служб. Рассматриваются связи со службами REST, SOAP служб и служб Windows Communication Foundation.
ms.prod: xamarin
ms.assetid: 72627B90-586A-02B6-E231-F7CE015A1B97
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: afebe7f491855844e18bf054d665cf8d54e8f353
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2019
ms.locfileid: "57672395"
---
# <a name="introduction-to-web-services"></a>Введение в веб-служб

_В этом руководстве показано, как использовать технологии разных веб-служб. Рассматриваются связи со службами REST, SOAP служб и служб Windows Communication Foundation._

Для правильной работы многих мобильных приложениях, зависящих от облака, и поэтому интеграция веб-служб в мобильные приложения — распространенный сценарий. Платформы Xamarin поддерживает использование различных веб-службы технологии и включает поддержку встроенных и сторонних поставщиков для работы со службами RESTful, ASMX и Windows Communication Foundation (WCF).

Для клиентов, использующих Xamarin.Forms, есть полные примеры использования каждого из этих технологий в [Xamarin.Forms веб-служб](~/xamarin-forms/data-cloud/index.md) документации.

> [!IMPORTANT]
> В iOS 9 приложение Transport Security (ATS) обеспечивает безопасных соединений между Интернет-ресурсов (например, server серверной части приложения) и приложения, тем самым предотвращая случайное раскрытие конфиденциальной информации.
> Поскольку ATS включена по умолчанию в приложениях, созданных для iOS 9, все подключения будут применяться требования к безопасности ATS. Если соединения не удовлетворяют этим требованиям, они вызовут сбой с исключением.

Вы можете отказаться от ATS Если невозможно использовать `HTTPS` протокола и безопасный обмен данными для Интернет-ресурсов. Это достигается путем обновления приложения **Info.plist** файла. Дополнительные сведения см. в разделе [безопасность транспорта приложения](~/ios/app-fundamentals/ats.md).

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

## <a name="consuming-rest-services"></a>Использование служб REST

Существует несколько библиотек и классы, которые могут использоваться для использования служб REST, а в следующих подразделах описываются их. Дополнительные сведения об использовании службы REST, см. в разделе [RESTful веб-служба](~/xamarin-forms/data-cloud/consuming/rest.md).

### <a name="httpclient"></a>HttpClient

[Клиентских библиотек HTTP Майкрософт](https://www.nuget.org/packages/Microsoft.Net.Http) предоставляет `HttpClient` класс, который используется для отправки и получения запросов по протоколу HTTP. Он предоставляет функциональные возможности для отправки запросов HTTP и получения ответов HTTP с определенным URI-адресом ресурса. Каждый запрос отправляется в качестве асинхронной операции. Дополнительные сведения об асинхронных операциях см. в разделе [обзор поддержки асинхронного](~/cross-platform/platform/async.md).

`HttpResponseMessage` Класс представляет сообщение ответа HTTP, полученные из веб-службы, после HTTP-запроса. Он содержит сведения об ответе, включая код состояния, заголовки и текст. `HttpContent` Класс представляет тело HTTP и заголовки содержимого, таких как `Content-Type` и `Content-Encoding`. Можно прочитать содержимое с помощью любого из `ReadAs` методы, такие как `ReadAsStringAsync` и `ReadAsByteArrayAsync`, в зависимости от формата данных.

Дополнительные сведения о `HttpClient` , представлена в разделе [создания объекта HTTPClient](~/xamarin-forms/data-cloud/consuming/rest.md).

<a name="Using_HTTPWebRequest" />

### <a name="httpwebrequest"></a>HTTPWebRequest

Вызов веб-служб с `HTTPWebRequest` включает в себя:

-  Создание экземпляра запроса для определенного URI.
-  Настройка различных свойств HTTP для экземпляра запроса.
-  Извлечение `HttpWebResponse` из запроса.
-  Чтение данных из ответа.

Например следующий код извлекает данные из США. Национальный библиотеки веб-службы:

```csharp
var rxcui = "198440";
var request = HttpWebRequest.Create(string.Format(@"http://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui));
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

В приведенном выше примере создается `HttpWebRequest` , возвращает данные в формате JSON. Данные возвращаются в `HttpWebResponse`, из которого `StreamReader` можно получить для чтения данных.

<a name="Using_RESTSHARP" />

### <a name="restsharp"></a>RestSharp

Использует другой подход для использования служб REST [RestSharp](http://restsharp.org/) библиотеки. RestSharp инкапсулирует HTTP-запросов, включая поддержку для получения результатов, как содержимое необработанную строку или как десериализованные C# объекта. Например следующий код выполняет запрос в США Строка с отформатированной национальных библиотеки веб-службы, а затем извлекает результаты как JSON-ФАЙЛ:

```csharp
var request = new RestRequest(string.Format("{0}/allinfo", rxcui));
request.RequestFormat = DataFormat.Json;
var response = Client.Execute(request);
if(string.IsNullOrWhiteSpace(response.Content) || response.StatusCode != System.Net.HttpStatusCode.OK) {
       return null;
}
rxTerm = DeserializeRxTerm(response.Content);
```

`DeserializeRxTerm` — Это метод, который займет необработанную строку JSON из `RestSharp.RestResponse.Content` свойство и преобразовать его в C# объекта. Далее в этой статье рассматривается десериализации данные, возвращенные из веб-служб.

<a name="Using_NSUrlconnection" />

### <a name="nsurlconnection"></a>NSUrlConnection

Помимо классов, доступных в базе Mono библиотека классов (BCL), такие как `HttpWebRequest`и указанной третьей стороной C# библиотек, таких как RestSharp, классы, зависящие от платформы также доступны для использования веб-служб. Например, в iOS `NSUrlConnection` и `NSMutableUrlRequest` классы могут использоваться.

В следующем примере кода показано, как вызвать США Национальный веб-служба библиотеки с помощью классов iOS:

```csharp
var rxcui = "198440";
var request = new NSMutableUrlRequest(new NSUrl(string.Format("http://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui)),
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

Как правило, классы, зависящие от платформы для использования веб-служб должна выполняться только для сценариев, где машинного кода переносится в C#. Где это возможно, таким образом, он может быть общей кросс платформенного кода доступа к веб-службы должны быть переносимыми.

<a name="Using_ServiceStack_Client" />

### <a name="servicestack"></a>ServiceStack

Другой вариант для вызова веб-служб — [стека службы](http://www.servicestack.net/) библиотеки. Например, приведенный ниже показано использование стека службы `IServiceClient.GetAsync` метод для выдачи запроса на обслуживание:

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
> Хотя средства, такие как ServiceStack и RestSharp позволяют легко вызывать и использовать REST служб, иногда бывает не так просто использовать XML или JSON, который не соответствует стандарту _DataContract_ соглашения о сериализации. При необходимости вызвать запрос и обрабатывать соответствующий сериализацию явным образом с помощью библиотеки ServiceStack.Text, о котором рассказывается ниже.


<a name="Options_for_consuming_RESTful_data" />

## <a name="consuming-restful-data"></a>Использование данных RESTful

Сообщения JSON обычно использовать веб-служб rESTful для возврата данных клиенту. JSON — это основанные на тексте формат обмена данными, создает compact полезных данных, что приводит к ограниченной пропускной способности при отправке данных. В этом разделе будут проверены механизмы для использования RESTful ответы в JSON и Plain Old XML (POX).

<a name="Using_System.JSON" />

### <a name="systemjson"></a>System.JSON

Платформа Xamarin поставляется с поддержкой JSON без дополнительной настройки. С помощью `JsonObject`, результаты могут быть получены, как показано в следующем примере кода:

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

Тем не менее важно Имейте в виду, что `System.Json` средства загрузки полностью данные в память.

<a name="Using_JSON.NET" />

### <a name="jsonnet"></a>JSON.NET

[Библиотеки NewtonSoft JSON.NET](http://www.newtonsoft.com/json) — это широко используемый библиотека для сериализации и десериализации сообщений JSON. В следующем примере кода показано, как использовать JSON.NET для десериализации сообщения JSON в C# объекта:

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

### <a name="servicestacktext"></a>ServiceStack.Text

ServiceStack.Text — это библиотека сериализации JSON, разработанные для работы с библиотекой ServiceStack. В следующем примере кода показано, как выполнить синтаксический анализ JSON с помощью `ServiceStack.Text.JsonObject`:

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

В случае использования в веб-службе REST на основе XML, LINQ to XML можно использовать для анализа кода XML и заполнения C# объекта линейно, как показано в следующем примере кода:

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

## <a name="aspnet-web-service-asmx"></a>Веб-службы ASP.NET (ASMX)

ASMX предоставляет возможность создавать веб-служб, которые отправляют сообщения, используя Simple Object Access Protocol (SOAP). SOAP — это протокол независимо от платформы и независимый от языка, для создания и вызова веб-служб. Потребителям службы ASMX не нужно ничего знать об используемой платформе, объектной модели или язык программирования, используемый для реализации службы. Они должны понять, как отправлять и получать сообщения SOAP.

Сообщение SOAP — это документ XML, содержащий следующие элементы:

- Корневой элемент с именем *конверт* , идентифицирующий XML-документа, как сообщение SOAP.
- Необязательный *заголовок* элемент, содержащий сведения о приложении, такие как данные проверки подлинности. Если *заголовок* присутствует элемент он должен быть первый дочерний элемент элемента *конверт* элемент.
- Обязательный *текст* элемент, содержащий сообщение SOAP, предназначенное для получателя.
- Необязательный *сбоя* элемент, который используется для указания сообщения об ошибках. Если *сбоя* элемент присутствует, он должен быть дочерним элементом элемента *текст* элемент.

SOAP может работать много транспортных протоколов, включая HTTP, SMTP, TCP и UDP. Тем не менее службы ASMX могут работать только по протоколу HTTP. Платформа Xamarin поддерживает стандартные реализации SOAP 1.1 по протоколу HTTP, и он также поддерживает многие из стандартных конфигураций службы ASMX.

### <a name="generating-a-proxy"></a>Создание учетной записи-посредника

Объект *прокси* должен быть создан использовать службу ASMX, что позволяет приложению для подключения к службе. Прокси создается с помощью много метаданных службы, определяющий методы и связанные службы конфигурации. Эти метаданные предоставляются как документ языка описания служб (WSDL), который создается веб-службой. Прокси создается с помощью Visual Studio для Mac или Visual Studio для добавления в проекты под конкретные платформы веб-ссылки для веб-службы.

URL веб-службы может быть размещенной удаленного источника или ресурса локальной файловой системы, доступных с помощью `file:///` префикс пути, например:

```csharp
file:///Users/myUserName/projects/MyProjectName/service.wsdl
```

[![](images/add-webreference-dialog.png "URL веб-службы может быть размещенной удаленного источника или ресурса локальной файловой системы, доступных с помощью префикса пути файла")](images/add-webreference-dialog.png#lightbox)

Это приводит к возникновению ошибки прокси-сервера в папке ссылок на службы или веб-проекта. Так как прокси-сервер создается код, не должен быть изменен.

<a name="Manually_adding_a_proxy_to_a_project" />

#### <a name="manually-adding-a-proxy-to-a-project"></a>Добавление учетной записи-посредника в проект вручную

При наличии существующего прокси-сервера, созданный с использованием совместимых инструментов, эти выходные данные могут потребляться включены как часть проекта. В Visual Studio для Mac использовать **добавить файлы...** пункт меню, чтобы добавить прокси-сервер. Кроме того, для этого требуется *System.Web.Services.dll* ссылаться явным образом с помощью **Добавление ссылок...** диалоговое окно.

### <a name="consuming-the-proxy"></a>Использование прокси-сервера

Созданный прокси-классы предоставляют методы для использования веб-службы, использующих шаблон проектирования модели асинхронного программирования (APM). В этом шаблоне асинхронную операцию реализуется в виде двух методов с именем *BeginOperationName* и *EndOperationName*, который начинают и завершают асинхронную операцию.

*BeginOperationName* метод начинает асинхронную операцию и возвращает объект, реализующий `IAsyncResult` интерфейс. После вызова метода *BeginOperationName*, приложение может продолжить выполнение инструкций в вызывающем потоке, пока асинхронная операция выполняется в потоке пула потоков.

Для каждого вызова *BeginOperationName*, приложение также должно вызвать *EndOperationName* для получения результатов операции. Возвращаемое значение *EndOperationName* имеет тот же тип, возвращаемый методом синхронной веб-службы. В следующем примере кода показан пример этого:

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

Библиотека параллельных задач (TPL) можно упростить процесс потребляет пару методов begin и end APM инкапсуляция асинхронных операций в том же `Task` объекта. Такая инкапсуляция предоставляется несколько перегрузок `Task.Factory.FromAsync` метод. Этот метод создает `Task` , выполняющего `TodoService.EndGetTodoItems` метод один раз `TodoService.BeginGetTodoItems` завершения метода с `null` параметр, указывающий, что данные не передается в `BeginGetTodoItems` делегировать. Наконец, значение `TaskCreationOptions` перечисление указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

Дополнительные сведения о APM, см. в разделе [модели асинхронного программирования](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) и [TPL и традиционное .NET Framework асинхронное программирование](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) на сайте MSDN.

Дополнительные сведения об использовании службы ASMX, см. в разделе [использование службы ASP.NET Web (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md).

<a name="wcf" />

## <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

WCF — это унифицированная платформа корпорации Microsoft для построения сервисноориентированных приложений. Он позволяет разработчикам создавать безопасные, надежные, с поддержкой транзакций с возможностью взаимодействия распределенных приложений.

WCF описывает службу, с различными разные контракты, в том числе следующие:

- **Контракты данных** — определение структуры данных, которые формируют основу для содержимого сообщения.
- **Контракты сообщений** — создания сообщений из контракты данных.
- **Контракты сбоя** — разрешить пользовательских ошибок SOAP указать.
- **Контракты службы** — определить операции, которые поддерживают службы и сообщения, необходимые для взаимодействия с каждой операцией. Они также определяют любое поведение пользовательской ошибки, которое может быть связан с операциями с каждой службы.

Существуют различия между ASP.NET Web Services (ASMX) и WCF, но важно понимать, что WCF поддерживает те же возможности, которые предоставляет ASMX-сообщений SOAP по протоколу HTTP.

> [!IMPORTANT]
> Поддержка платформы Xamarin WCF ограничена кодировкой текста сообщений SOAP по сравнению с использованием HTTP/HTTPS `BasicHttpBinding` класса. Кроме того поддержка WCF требует применения средств доступно только в среде Windows для создания прокси-сервера.

### <a name="generating-a-proxy"></a>Создание учетной записи-посредника

Объект *прокси* должен быть создан для использования службы WCF, что позволяет приложению для подключения к службе. Прокси создается с помощью много метаданных службы, определяющий методы и связанные службы конфигурации. Эти метаданные предоставляются в виде документа языка описания служб (WSDL), который создается веб-службой. Прокси-сервер может быть построен с помощью Microsoft WCF Web Service Reference Provider в Visual Studio 2017 для добавления ссылки на службу веб-службы в библиотеку .NET Standard.

Вместо создания прокси с помощью Microsoft WCF Web Service Reference Provider в Visual Studio 2017 является использование ServiceModel Metadata Utility Tool (svcutil.exe). Дополнительные сведения см. в разделе [ServiceModel Metadata Utility Tool (Svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe).

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

### <a name="configuring-the-proxy"></a>Настройка прокси-сервера

Настройка созданного прокси обычно займет два аргумента конфигурации (в зависимости от SOAP 1.1/ASMX или WCF) во время инициализации: `EndpointAddress` и/или связанные сведения о привязке, как показано в следующем примере:

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

Для каждого вызова *BeginOperationName*, приложение также должно вызвать *EndOperationName* для получения результатов операции. Возвращаемое значение *EndOperationName* имеет тот же тип, возвращаемый методом синхронной веб-службы. В следующем примере кода показан пример этого:

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

Библиотека параллельных задач (TPL) можно упростить процесс потребляет пару методов begin и end APM инкапсуляция асинхронных операций в том же `Task` объекта. Такая инкапсуляция предоставляется несколько перегрузок `Task.Factory.FromAsync` метод. Этот метод создает `Task` , выполняющего `TodoServiceClient.EndGetTodoItems` метод один раз `TodoServiceClient.BeginGetTodoItems` завершения метода с `null` параметр, указывающий, что данные не передается в `BeginGetTodoItems` делегировать. Наконец, значение `TaskCreationOptions` перечисление указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

Дополнительные сведения о APM, см. в разделе [модели асинхронного программирования](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) и [TPL и традиционное .NET Framework асинхронное программирование](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) на сайте MSDN.

Дополнительные сведения об использовании службы WCF см. в разделе [этого веб-служба Windows Communication Foundation (WCF)](~/xamarin-forms/data-cloud/consuming/wcf.md).

<a name="Calling_a_WCF_Service_with_Transport_Security" />

#### <a name="using-transport-security"></a>Использование безопасности транспорта

Службы WCF могут использовать безопасность на транспортном уровне для защиты от перехвата сообщений. Платформа Xamarin поддерживает привязки, в которых используется безопасность на транспортном уровне с помощью протокола SSL. Однако возможны ситуации, в которых стека может потребоваться проверить сертификат, который приводит к непредвиденного поведения. Проверку можно переопределить, зарегистрировав `ServerCertificateValidationCallback` делегат перед вызовом службы, как показано в следующем примере кода:

```csharp
System.Net.ServicePointManager.ServerCertificateValidationCallback +=
(se, cert, chain, sslerror) => { return true; };
```

Это позволяет поддерживать шифрование транспорта, игнорируя проверку сертификата на сервере. Тем не менее этот подход фактически не учитывает вопросы доверия, связанный с сертификатом и может не подойти. Дополнительные сведения см. в разделе [с помощью доверенных корневых ЦС уважительно](https://www.mono-project.com/UsingTrustedRootsRespectfully) на [mono-project.com](https://www.mono-project.com).

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

#### <a name="using-client-credential-security"></a>С помощью учетных данных Client Security

Службы WCF может также потребоваться клиентов службы для проверки подлинности с использованием учетных данных. Платформа Xamarin не поддерживает протокол WS-Security, который позволяет клиентам отправлять учетные данные в конверте SOAP сообщения. Тем не менее, платформа Xamarin поддерживает возможность передавать учетные данные обычной проверки подлинности HTTP на сервер, указав соответствующие `ClientCredentialType`:

```csharp
basicHttpBinding.Security.Transport.ClientCredentialType = HttpClientCredentialType.Basic;
```

Затем можно указать учетные данные обычной проверки подлинности:

```csharp
client.ClientCredentials.UserName.UserName = @"foo";
client.ClientCredentials.UserName.Password = @"mrsnuggles";
```

В приведенном выше примере, если сообщение об ошибке «Не хватило trampolines типа 0» можно увеличить число trampolines типа 0, добавив `–aot “trampolines={number of trampolines}”` аргумент для сборки. Дополнительные сведения см. в [руководстве по устранению неполадок](~/ios/troubleshooting/troubleshooting.md#trampolines).

Дополнительные сведения о базовой аутентификации HTTP, несмотря на то что в контексте веб-службы REST, см. в разделе [проверки подлинности веб-службы RESTful](~/xamarin-forms/data-cloud/authentication/rest.md).

## <a name="related-links"></a>Связанные ссылки

- [Веб-служб в Xamarin.Forms](~/xamarin-forms/data-cloud/index.md)
- [Средство ServiceModel Metadata Utility Tool (svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
- [BasicHttpBinding](https://msdn.microsoft.com/library/system.servicemodel.basichttpbinding.aspx)
