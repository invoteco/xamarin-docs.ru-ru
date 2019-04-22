---
title: Использование веб-службы Windows Communication Foundation (WCF)
description: В этой статье показано, как использовать службу WCF Simple Object Access Protocol (SOAP) из приложения Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 5696FF04-EF21-4B7A-8C8B-26DE28B5C0AD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/28/2019
ms.openlocfilehash: 7106c0aed03800d3479471caab0974be3c09c1f8
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2019
ms.locfileid: "59239918"
---
# <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>Использование веб-службы Windows Communication Foundation (WCF)

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)

_WCF — это унифицированная платформа корпорации Microsoft для построения сервисноориентированных приложений. Он позволяет разработчикам создавать безопасные, надежные, с поддержкой транзакций с возможностью взаимодействия распределенных приложений. В этой статье показано, как использовать службу WCF Simple Object Access Protocol (SOAP) из приложения Xamarin.Forms._

WCF описывает службу, с различными разные контракты, в том числе:

- **Контракты данных** — определение структуры данных, которые формируют основу для содержимого сообщения.
- **Контракты сообщений** — создания сообщений из контракты данных.
- **Контракты сбоя** — разрешить пользовательских ошибок SOAP указать.
- **Контракты службы** — определить операции, которые поддерживают службы и сообщения, необходимые для взаимодействия с каждой операцией. Они также определяют любое поведение пользовательской ошибки, которое может быть связан с операциями с каждой службы.

Существуют различия между ASP.NET Web Services (ASMX) и WCF, но также WCF поддерживает те же возможности, которые предоставляет ASMX-сообщений SOAP по протоколу HTTP. Дополнительные сведения об использовании службы ASMX, см. в разделе [использование ASP.NET Web Services (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md).

> [!IMPORTANT]
> Поддержка платформы Xamarin WCF ограничена кодировкой текста сообщений SOAP по сравнению с использованием HTTP/HTTPS `BasicHttpBinding` класса.
>
> Поддержка WCF требует применения средств доступно только в среде Windows, чтобы создать прокси и размещения TodoWCFService. Построение и тестирование приложения iOS будет необходимо развертывать TodoWCFService на компьютере Windows или Azure веб-службой.
>
> Собственные приложения Xamarin Forms обычно совместное использование кода с библиотеку классов .NET Standard. Тем не менее .NET Core не поддерживает WCF, общий проект должен быть старую переносимую библиотеку классов. Сведения о поддержке WCF в .NET Core, см. в разделе [Выбор между .NET Core и .NET Framework для серверных приложений](/dotnet/standard/choosing-core-framework-server).

В примере приложения решения включает в себя службе WCF, которая может выполняться локально и показано на следующем снимке экрана:

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

Дополнительные сведения о модели данных, используемых в приложении, см. в разделе [моделирования данных](~/xamarin-forms/data-cloud/walkthrough.md).

Объект *прокси* должен быть создан для использования службы WCF, что позволяет приложению для подключения к службе. Прокси создается с помощью много метаданных службы, определяющий методы и связанные службы конфигурации. Эти метаданные предоставляются в виде документа языка описания служб (WSDL), который создается веб-службой. Прокси-сервер может быть построен с помощью Microsoft WCF Web Service Reference Provider в Visual Studio 2017 для добавления ссылки на службу веб-службы в библиотеку .NET Standard. Вместо создания прокси с помощью Microsoft WCF Web Service Reference Provider в Visual Studio 2017 является использование ServiceModel Metadata Utility Tool (svcutil.exe). Дополнительные сведения см. в разделе [ServiceModel Metadata Utility Tool (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/).

Созданный прокси-классы предоставляют методы для использования веб-служб, использующих шаблон проектирования модели асинхронного программирования (APM). В этом случае асинхронная операция реализуется как два метода с именем *BeginOperationName* и *EndOperationName*, который начинают и завершают асинхронную операцию.

*BeginOperationName* метод начинает асинхронную операцию и возвращает объект, реализующий `IAsyncResult` интерфейс. После вызова метода *BeginOperationName*, приложение может продолжить выполнение инструкций в вызывающем потоке, пока асинхронная операция выполняется в потоке пула потоков.

Для каждого вызова *BeginOperationName*, приложение также должно вызвать *EndOperationName* для получения результатов операции. Возвращаемое значение *EndOperationName* имеет тот же тип, возвращаемый методом синхронной веб-службы. Например `EndGetTodoItems` метод возвращает коллекцию `TodoItem` экземпляров. *EndOperationName* также включает `IAsyncResult` параметр, который задается экземпляр, возвращаемый соответствующим вызовом метода *BeginOperationName* метод.

Библиотека параллельных задач (TPL) можно упростить процесс потребляет пару методов begin и end APM инкапсуляция асинхронных операций в том же `Task` объекта. Такая инкапсуляция предоставляется несколько перегрузок `TaskFactory.FromAsync` метод.

Дополнительные сведения о APM см. в разделе [модели асинхронного программирования](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) и [TPL и традиционное .NET Framework асинхронное программирование](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) на сайте MSDN.

### <a name="create-the-todoserviceclient-object"></a>Создайте объект TodoServiceClient

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

### <a name="create-data-transfer-objects"></a>Создание объектов передачи данных

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

### <a name="retrieve-data"></a>Получить данные

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

### <a name="update-data"></a>обновление данных

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
В Visual Studio 2017 или Visual Studio 2019 г. можно протестировать приложение универсальной платформы Windows на ПК без дополнительной настройки. Тестирование клиентов iOS и Android может потребоваться дополнительные шаги в этом разделе. См. в разделе [подключение к локальные веб-службы из iOS симуляторов и эмуляторы Android](~/cross-platform/deploy-test/connect-to-local-web-services.md) Дополнительные сведения.

По умолчанию IIS Express будет отвечать на запросы только для `localhost`. Удаленного устройства (например, устройства Android, iPhone или даже симулятор) не будет доступа к локальной службе WCF. Необходимо знать IP-адрес рабочей станции Windows 10 в локальной сети. В данном примере предположим, что рабочей станции должно IP-адрес `192.168.1.143`. Ниже объясняется, как настроить Windows 10 и IIS Express для удаленных соединений и подключиться к службе из физического или виртуального устройства:

1. **Добавить исключение брандмауэра Windows**. Необходимо открыть порт, через брандмауэр Windows, приложений в подсети можно использовать для взаимодействия со службой WCF. Создайте правило для входящего трафика Открытие 49393 порт в брандмауэре. Из административной командной строки выполните следующую команду:
    ```
    netsh advfirewall firewall add rule name="TodoWCFService" dir=in protocol=tcp localport=49393 profile=private remoteip=localsubnet action=allow
    ```

1. **Настройка IIS Express, чтобы принимать удаленные подключения**. Можно настроить IIS Express путем изменения файла конфигурации для IIS Express в **[каталог решения]\.vs\config\applicationhost.config**. Найти `site` элемент с именем `TodoWCFService`. Он должен выглядеть как в следующем коде XML:

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

    Необходимо добавить два `binding` элементы для открытия порта 49393 внешнего трафика и эмулятора Android. Привязки `[IP address]:[port]:[hostname]` формате, которое указывает, как IIS Express будут отвечать на запросы. Внешние запросы будут иметь имена узлов, должен быть указан как `binding`. Добавьте следующий код XML для `bindings` элемента, заменив собственный IP-адрес IP-адрес:

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
    >По умолчанию IIS Express не будет добавлять подключения из внешних источников по соображениям безопасности. Чтобы разрешить соединения с удаленными устройствами необходимо запустить IIS Express с правами администратора. Для этого проще всего запустить Visual Studio 2017 с правами администратора. Это приведет к запуску IIS Express с правами администратора при выполнении TodoWCFService.

    Таким образом полный можно запустить TodoWCFService и подключиться с других устройств в подсети. Это можно проверить, запуск приложения и посещение `http://localhost:49393/TodoService.svc`. Если вы получаете **неправильный запрос** ошибки при посещении этот URL-адрес вашей `bindings` могут быть неверны в конфигурации IIS Express (запрос достигает IIS Express, но отклоняется). Если вы получаете другую ошибку возможно, что приложение не выполняется, или ваш брандмауэр настроен неправильно.

    Чтобы сервер IIS Express обеспечивает выполнение и обслуживание службы, выключите **изменить и продолжить** в диалоговом окне **свойства проекта > Web > отладчики**.

1. **Настройка конечной точки устройства используют для доступа к службе**. Этот шаг включает настройку клиентское приложение, выполняемое на устройстве физическое или эмулируемое, для доступа к службе WCF.

    Эмулятор Android использует внутренний прокси-сервера, который предотвращает эмулятор прямого доступа к хост-компьютера `localhost` адрес. Вместо этого адрес `10.0.2.2` в эмуляторе направляется `localhost` на хост-компьютере через внутренний прокси-класса. Эти запросы через прокси будет иметь `127.0.0.1` как имя узла в заголовке запроса, именно вы создали привязки IIS Express для этого имени узла, описанные ранее.

    IOS, симулятор работает на компьютере Mac узла, сборки, даже если вы используете [удаленный iOS Simulator для Windows](~/tools/ios-simulator/index.md). Сетевые запросы, в симуляторе будет иметь IP-адрес рабочей станции в локальной сети в качестве имени узла (в этом примере он имеет `192.168.1.143`, но фактический IP-адрес, скорее всего будет другой). Вот почему ранее вы создали привязки IIS Express для этого имени узла.

    Убедитесь, `SoapUrl` свойство в **Constants.cs** файл в проекте TodoWCF (переносимая) имеют значения, которые являются правильными для вашей сети:

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

    После того как вы настроите **Constants.cs** с соответствующим конечным точкам, можно будет соединяться с TodoWCFService, запущенной на рабочей станции Windows 10 с физическими или виртуальными устройствами.

## <a name="related-links"></a>Связанные ссылки

- [TodoWCF (пример)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)
- [Практическое руководство. Создание клиента Windows Communication Foundation](https://docs.microsoft.com/dotnet/framework/wcf/how-to-create-a-wcf-client)
- [Средство ServiceModel Metadata Utility Tool (svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
