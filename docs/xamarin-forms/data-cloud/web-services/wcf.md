---
title: Использование веб-службы Windows Communication Foundation (WCF)
description: В этой статье показано, как использовать службу WCF Simple Object Access Protocol (SOAP) из приложения Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 5696FF04-EF21-4B7A-8C8B-26DE28B5C0AD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/28/2019
ms.openlocfilehash: d170e37b8bf4ce880f9d8f48d30defb42ee6bba2
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648011"
---
# <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>Использование веб-службы Windows Communication Foundation (WCF)

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todowcf)

_WCF — это унифицированная платформа корпорации Microsoft для построения сервисноориентированных приложений. Он позволяет разработчикам создавать безопасные, надежные, с поддержкой транзакций с возможностью взаимодействия распределенных приложений. В этой статье показано, как использовать службу WCF Simple Object Access Protocol (SOAP) из приложения Xamarin.Forms._

WCF описывает службу с различными контрактами, в том числе:

- **Контракты данных** — определение структуры данных, которые формируют основу для содержимого сообщения.
- **Контракты сообщений** — создания сообщений из контракты данных.
- **Контракты сбоя** — разрешить пользовательских ошибок SOAP указать.
- **Контракты службы** — определить операции, которые поддерживают службы и сообщения, необходимые для взаимодействия с каждой операцией. Они также определяют любое поведение пользовательской ошибки, которое может быть связан с операциями с каждой службы.

Существуют различия между ASP.NET Web Services (ASMX) и WCF, но WCF поддерживает те же возможности, которые предоставляет ASMX — сообщения SOAP по протоколу HTTP. Дополнительные сведения об использовании службы ASMX см. в разделе [Использование веб-служб ASP.NET (ASMX)](~/xamarin-forms/data-cloud/web-services/asmx.md).

> [!IMPORTANT]
> Поддержка платформы Xamarin для WCF ограничена текстовыми сообщениями SOAP по протоколу HTTP/HTTPS с помощью `BasicHttpBinding` класса.
>
> Поддержка WCF требует использования средств, доступных в среде Windows, для создания прокси-сервера и размещения Тодовкфсервице. Для сборки и тестирования приложения iOS потребуется развернуть Тодовкфсервице на компьютере Windows или в качестве веб-службы Azure.
>
> Собственные приложения Xamarin Forms обычно используют код совместно с библиотекой классов .NET Standard. Однако .NET Core в настоящее время не поддерживает WCF, поэтому общий проект должен быть унаследованной переносимой библиотекой классов. Сведения о поддержке WCF в .NET Core см. в разделе [Выбор между .NET Core и .NET Framework для серверных приложений](/dotnet/standard/choosing-core-framework-server).

Решение примера приложения включает службу WCF, которая может быть запущена локально и показана на следующем снимке экрана:

![](wcf-images/portal.png "Пример приложения")

> [!NOTE]
> В iOS 9 и более поздней версии приложение Transport Security (ATS) обеспечивает безопасных соединений между Интернет-ресурсов (например, server серверной части приложения) и приложения, тем самым предотвращая случайное раскрытие конфиденциальной информации. Поскольку ATS включена по умолчанию в приложениях, созданных для iOS 9, все подключения будут применяться требования к безопасности ATS. Если соединения не удовлетворяют этим требованиям, они вызовут сбой с исключением.
>
> Если невозможно использовать из могут быть присоединены ATS `HTTPS` протокола и безопасный обмен данными для Интернет-ресурсов. Это достигается путем обновления приложения **Info.plist** файла. Дополнительные сведения см. в разделе [безопасность транспорта приложения](~/ios/app-fundamentals/ats.md).

## <a name="consume-the-web-service"></a>Использование веб-службы

Служба WCF предоставляет следующие операции:

|Операция|Описание|Параметры|
|--- |--- |--- |
|GetTodoItems|Получение списка элементов задач|
|CreateTodoItem|Создать новый элемент списка дел|XML-сериализации TodoItem|
|EditTodoItem|Обновление элемента задачи|XML-сериализации TodoItem|
|DeleteTodoItem|Удаление элемента задачи|XML-сериализации TodoItem|

Дополнительные сведения о модели данных, используемых в приложении, см. в разделе [моделирования данных](~/xamarin-forms/data-cloud/web-services/introduction.md).

Объект *прокси* должен быть создан для использования службы WCF, что позволяет приложению для подключения к службе. Прокси создается с помощью много метаданных службы, определяющий методы и связанные службы конфигурации. Эти метаданные предоставляются в виде документа языка описания служб (WSDL), который создается веб-службой. Прокси-сервер может быть построен с помощью Microsoft WCF Web Service Reference Provider в Visual Studio 2017 для добавления ссылки на службу веб-службы в библиотеку .NET Standard. Вместо создания прокси с помощью Microsoft WCF Web Service Reference Provider в Visual Studio 2017 является использование ServiceModel Metadata Utility Tool (svcutil.exe). Дополнительные сведения см. в разделе [ServiceModel Metadata Utility Tool (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/).

Созданный прокси-классы предоставляют методы для использования веб-служб, использующих шаблон проектирования модели асинхронного программирования (APM). В этом случае асинхронная операция реализуется как два метода с именем *BeginOperationName* и *EndOperationName*, который начинают и завершают асинхронную операцию.

*BeginOperationName* метод начинает асинхронную операцию и возвращает объект, реализующий `IAsyncResult` интерфейс. После вызова метода *BeginOperationName*, приложение может продолжить выполнение инструкций в вызывающем потоке, пока асинхронная операция выполняется в потоке пула потоков.

Для каждого вызова *BeginOperationName*, приложение также должно вызвать *EndOperationName* для получения результатов операции. Возвращаемое значение *EndOperationName* имеет тот же тип, возвращаемый методом синхронной веб-службы. Например `EndGetTodoItems` метод возвращает коллекцию `TodoItem` экземпляров. *EndOperationName* также включает `IAsyncResult` параметр, который задается экземпляр, возвращаемый соответствующим вызовом метода *BeginOperationName* метод.

Библиотека параллельных задач (TPL) можно упростить процесс потребляет пару методов begin и end APM инкапсуляция асинхронных операций в том же `Task` объекта. Такая инкапсуляция предоставляется несколько перегрузок `TaskFactory.FromAsync` метод.

Дополнительные сведения о APM см. в разделе [модели асинхронного программирования](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) и [TPL и традиционное .NET Framework асинхронное программирование](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) на сайте MSDN.

### <a name="create-the-todoserviceclient-object"></a>Создание объекта Тодосервицеклиент

Созданный класс прокси предоставляет `TodoServiceClient` класс, который используется для взаимодействия со службой WCF по протоколу HTTP. Он предоставляет функциональные возможности для вызова методов веб-службы, асинхронных операций из URI определения экземпляра службы. Дополнительные сведения об асинхронных операциях см. в разделе [обзор поддержки асинхронного](~/cross-platform/platform/async.md).

`TodoServiceClient` Экземпляр объявлен на уровне класса, что объект существует до тех пор, пока приложение должно использовать службу WCF, как показано в следующем примере кода:

```csharp
public class SoapService : ISoapService
{
  ITodoService todoService;
  ...

  public SoapService ()
  {
    todoService = new TodoServiceClient (
      new BasicHttpBinding (),
      new EndpointAddress (Constants.SoapUrl));
  }
  ...
}
```

`TodoServiceClient` Экземпляр настроен с привязки сведения и адрес конечной точки. Привязка используется для указания транспорта, кодировки и протокола, требуемых для приложений и служб для взаимодействия друг с другом. `BasicHttpBinding` Указывает, что по транспортному протоколу HTTP будет отправляться кодировке текста сообщений SOAP. Задание адреса конечной точки позволяет приложению подключаться к различным экземплярам службы WCF, при условии, что несколько экземпляров, опубликованных.

Дополнительные сведения о настройке ссылки на службу, см. в разделе [Настройка ссылки на службу](~/cross-platform/data-cloud/web-services/index.md#wcf).

### <a name="create-data-transfer-objects"></a>Создание объектов для обмена данными

В примере приложения используется `TodoItem` для модели данных. Для хранения `TodoItem` элемента в веб-службы, оно сначала должно быть преобразовано в прокси, созданный `TodoItem` типа. Это достигается путем `ToWCFServiceTodoItem` метод, как показано в следующем примере кода:

```csharp
TodoWCFService.TodoItem ToWCFServiceTodoItem (TodoItem item)
{
  return new TodoWCFService.TodoItem
  {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

Этот метод просто создает новый `TodoWCFService.TodoItem` экземпляра и задает каждое свойство в свойстве идентичные из `TodoItem` экземпляра.

Аналогичным образом, при получении данных из веб-службы, оно должно быть преобразовано из прокси, созданный `TodoItem` тип `TodoItem` экземпляра. Это осуществляется с помощью `FromWCFServiceTodoItem` метод, как показано в следующем примере кода:

```csharp
static TodoItem FromWCFServiceTodoItem (TodoWCFService.TodoItem item)
{
  return new TodoItem
  {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}

```

Этот метод просто получает данные из прокси, созданный `TodoItem` введите и задает его в только что созданный `TodoItem` экземпляра.

### <a name="retrieve-data"></a>Получение данных

`TodoServiceClient.BeginGetTodoItems` И `TodoServiceClient.EndGetTodoItems` методы используются для вызова `GetTodoItems` операция, предоставляемая веб-службой. Эти асинхронные методы, инкапсулируются в `Task` объекта, как показано в следующем примере кода:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync <ObservableCollection<TodoWCFService.TodoItem>> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);

  foreach (var item in todoItems)
  {
    Items.Add (FromWCFServiceTodoItem (item));
  }
  ...
}
```

`Task.Factory.FromAsync` Метод создает `Task` , выполняющего `TodoServiceClient.EndGetTodoItems` метод один раз `TodoServiceClient.BeginGetTodoItems` завершения метода с `null` параметр, указывающий, что данные не передается в `BeginGetTodoItems` делегировать. Наконец, значение `TaskCreationOptions` перечисление указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

`TodoServiceClient.EndGetTodoItems` Возвращает `ObservableCollection` из `TodoWCFService.TodoItem` экземпляров, которые затем преобразуются `List` из `TodoItem` экземпляров для отображения.

### <a name="create-data"></a>Создание данных

`TodoServiceClient.BeginCreateTodoItem` И `TodoServiceClient.EndCreateTodoItem` методы используются для вызова `CreateTodoItem` операция, предоставляемая веб-службой. Эти асинхронные методы, инкапсулируются в `Task` объекта, как показано в следующем примере кода:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginCreateTodoItem,
    todoService.EndCreateTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` Метод создает `Task` , выполняющего `TodoServiceClient.EndCreateTodoItem` метод один раз `TodoServiceClient.BeginCreateTodoItem` завершения метода с `todoItem` данным, которое было передано в параметр `BeginCreateTodoItem` делегат для указания `TodoItem` создаваемого веб-службой. Наконец, значение `TaskCreationOptions` перечисление указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

Служба вызывает исключение web `FaultException` при невозможности создать `TodoItem`, который обрабатывается приложением.

### <a name="update-data"></a>Обновление данных

`TodoServiceClient.BeginEditTodoItem` И `TodoServiceClient.EndEditTodoItem` методы используются для вызова `EditTodoItem` операция, предоставляемая веб-службой. Эти асинхронные методы, инкапсулируются в `Task` объекта, как показано в следующем примере кода:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginEditTodoItem,
    todoService.EndEditTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` Метод создает `Task` , выполняющего `TodoServiceClient.EndEditTodoItem` метод один раз `TodoServiceClient.BeginCreateTodoItem` завершения метода с `todoItem` данным, которое было передано в параметр `BeginEditTodoItem` делегат для указания `TodoItem` обновления веб-службой. Наконец, значение `TaskCreationOptions` перечисление указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

Служба вызывает исключение web `FaultException` при невозможности найти или обновить `TodoItem`, который обрабатывается приложением.

### <a name="delete-data"></a>Удаление данных

`TodoServiceClient.BeginDeleteTodoItem` И `TodoServiceClient.EndDeleteTodoItem` методы используются для вызова `DeleteTodoItem` операция, предоставляемая веб-службой. Эти асинхронные методы, инкапсулируются в `Task` объекта, как показано в следующем примере кода:

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  ...
  await Task.Factory.FromAsync (
    todoService.BeginDeleteTodoItem,
    todoService.EndDeleteTodoItem,
    id,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` Метод создает `Task` , выполняющего `TodoServiceClient.EndDeleteTodoItem` метод один раз `TodoServiceClient.BeginDeleteTodoItem` завершения метода с `id` данным, которое было передано в параметр `BeginDeleteTodoItem` делегат для указания `TodoItem` должны быть удалены с веб-службы. Наконец, значение `TaskCreationOptions` перечисление указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

Служба вызывает исключение web `FaultException` если его не удается найти или удалить `TodoItem`, который обрабатывается приложением.

## <a name="configure-remote-access-to-iis-express"></a>Настройка удаленного доступа к IIS Express
В Visual Studio 2017 или Visual Studio 2019 вы сможете протестировать приложение UWP на ПК без дополнительной настройки. Для тестирования клиентов Android и iOS могут потребоваться дополнительные действия в этом разделе. Дополнительные сведения см. [в статье подключение к локальным веб-службам из симуляторов iOS и эмуляторов Android](~/cross-platform/deploy-test/connect-to-local-web-services.md) .

По умолчанию IIS Express будет отвечать только на запросы к `localhost`. Удаленные устройства (например, устройства Android, iPhone или даже симулятор) не будут иметь доступа к локальной службе WCF. Вам нужно будет знать IP-адрес рабочей станции Windows 10 в локальной сети. В этом примере предположим, что у рабочей станции есть IP-адрес `192.168.1.143`. Ниже описаны действия по настройке Windows 10 и IIS Express для приема удаленных подключений и подключения к службе из физического или виртуального устройства.

1. **Добавьте исключение в брандмауэр Windows**. Необходимо открыть порт через брандмауэр Windows, который приложения в подсети могут использовать для взаимодействия со службой WCF. Создайте правило входящего трафика, открыв порт 49393 в брандмауэре. В командной строке администратора выполните следующую команду:
    ```
    netsh advfirewall firewall add rule name="TodoWCFService" dir=in protocol=tcp localport=49393 profile=private remoteip=localsubnet action=allow
    ```

1. **Настройте IIS Express для приема удаленных подключений**. IIS Express можно настроить, изменив файл конфигурации для IIS Express в **папке [каталог решения]\.вс\конфиг\аппликатионхост.конфиг**. Найдите элемент с именем `TodoWCFService`. `site` Он должен выглядеть, как в следующем коде XML:

    ```xml
    <site name="TodoWCFService" id="2">
        <application path="/" applicationPool="Clr4IntegratedAppPool">
            <virtualDirectory path="/" physicalPath="C:\Users\tom\TodoWCF\TodoWCFService\TodoWCFService" />
        </application>
        <bindings>
            <binding protocol="http" bindingInformation="*:49393:localhost" />
        </bindings>
    </site>
    ```

    Вам потребуется добавить два `binding` элемента, чтобы открыть порт 49393 для внешнего трафика и эмулятора Android. Привязка использует `[IP address]:[port]:[hostname]` формат, указывающий, как IIS Express будет отвечать на запросы. Внешние запросы будут иметь имена узлов, которые должны быть указаны в `binding`виде. Добавьте следующий XML-код в `bindings` элемент, заменив IP-адрес своим IP-адресом:

    ```xml
    <binding protocol="http" bindingInformation="*:49393:192.168.1.143" />
    <binding protocol="http" bindingInformation="*:49393:127.0.0.1" />
    ```

    После внесения изменений `bindings` элемент должен выглядеть следующим образом:

    ```xml
    <site name="TodoWCFService" id="2">
        <application path="/" applicationPool="Clr4IntegratedAppPool">
            <virtualDirectory path="/" physicalPath="C:\Users\tom\TodoWCF\TodoWCFService\TodoWCFService" />
        </application>
        <bindings>
            <binding protocol="http" bindingInformation="*:49393:localhost" />
            <binding protocol="http" bindingInformation="*:49393:192.168.1.143" />
            <binding protocol="http" bindingInformation="*:49393:127.0.0.1" />
        </bindings>
    </site>
    ```

    >[!IMPORTANT]
    >По умолчанию IIS Express не будет принимать подключения из внешних источников по соображениям безопасности. Чтобы разрешить подключения с удаленных устройств, необходимо запустить IIS Express с разрешениями администратора. Самый простой способ сделать это — запустить Visual Studio 2017 с правами администратора. Это приведет к запуску IIS Express с правами администратора при запуске Тодовкфсервице.

    После выполнения этих действий вы сможете запустить Тодовкфсервице и подключиться с других устройств в подсети. Это можно проверить, запустив приложение и посетив `http://localhost:49393/TodoService.svc`страницу. Если при посещении этого URL-адреса появляется сообщение об ошибке `bindings` неправильного запроса, возможно, в IIS Express конфигурации возникла ошибка (запрос приближается IIS Express но отклоняется). Если возникает другая ошибка, возможно, приложение не работает или брандмауэр настроен неправильно.

    Чтобы разрешить IIS Express продолжать работу и обслуживать службу, отключите параметр " **изменить и продолжить** " в **свойствах проекта > отладчики веб->** .

1. **Настройте конечные устройства для доступа к службе**. Этот шаг включает настройку клиентского приложения, работающего на физическом или эмулированного устройстве, для доступа к службе WCF.

    Эмулятор Android использует внутренний прокси-сервер, который не защищает эмулятор от прямого доступа к `localhost` адресу главного компьютера. Вместо этого адрес `10.0.2.2` эмулятора направляется `localhost` на главный компьютер через внутренний прокси-сервер. Эти прокси-запросы будут иметь `127.0.0.1` в качестве имени узла в заголовке запроса, поэтому вы создали привязку IIS Express для этого имени узла в описанных выше шагах.

    Симулятор iOS выполняется на узле сборки Mac, даже если вы используете [Удаленный симулятор iOS для Windows](~/tools/ios-simulator/index.md). Сетевые запросы от симулятора будут иметь IP-адрес рабочей станции в локальной сети в качестве имени узла (в данном примере `192.168.1.143`это, но фактический IP-адрес будет отличаться). Именно поэтому вы создали привязку IIS Express для этого имени узла, как описано выше.

    Убедитесь, что свойствовфайлеConstants.csвпроектетодовкф(Portable)имеетправильныезначениядлявашей`SoapUrl` сети:

    ```csharp
    public static string SoapUrl
    {
        get
        {
            var defaultUrl = "http://localhost:49393/TodoService.svc";

            if (Device.RuntimePlatform == Device.Android)
            {
                defaultUrl = "http://10.0.2.2:49393/TodoService.svc";
            }
            else if (Device.RuntimePlatform == Device.iOS)
            {
                defaultUrl = "http://192.168.1.143:49393/TodoService.svc";
            }

            return defaultUrl;
        }
    }
    ```

    После настройки **Constants.CS** с соответствующими конечными точками вы сможете подключиться к тодовкфсервице, работающему на рабочей станции Windows 10, с физических или виртуальных устройств.

## <a name="related-links"></a>Связанные ссылки

- [TodoWCF (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todowcf)
- [Практическое руководство. Создание клиента Windows Communication Foundation](https://docs.microsoft.com/dotnet/framework/wcf/how-to-create-a-wcf-client)
- [Средство служебной программы метаданных ServiceModel (Svcutil. exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
