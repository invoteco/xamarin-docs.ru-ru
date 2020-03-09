---
title: Использование веб-службы Windows Communication Foundation (WCF)
description: В этой статье показано, как использовать службу WCF Simple Object Access Protocol (SOAP) из приложения Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 5696FF04-EF21-4B7A-8C8B-26DE28B5C0AD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/28/2019
ms.openlocfilehash: 28cb1573262b63cc2b0ccad9f468fe36c682718d
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/07/2020
ms.locfileid: "78915358"
---
# <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>Использование веб-службы Windows Communication Foundation (WCF)

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todowcf)

_WCF — это единая платформа Майкрософт для создания приложений, ориентированных на службы. Она позволяет разработчикам создавать безопасные, надежные, транзакционные и совместимые распределенные приложения. В этой статье показано, как использовать службу протокола SOAP WCF из приложения Xamarin. Forms._

WCF описывает службу с различными контрактами, в том числе:

- **Контракты данных** — определяют структуры данных, которые формируют базу содержимого в сообщении.
- **Контракты сообщений** — Составление сообщений из существующих контрактов данных.
- **Контракты ошибок** — разрешить указывать пользовательские ошибки SOAP.
- **Контракты служб** — укажите операции, которые поддерживаются службами, и сообщения, необходимые для взаимодействия с каждой операцией. Они также определяют любое поведение пользовательской ошибки, которое может быть связан с операциями с каждой службы.

Существуют различия между ASP.NET Web Services (ASMX) и WCF, но WCF поддерживает те же возможности, которые предоставляет ASMX — сообщения SOAP по протоколу HTTP. Дополнительные сведения об использовании службы ASMX см. в разделе [Использование веб-служб ASP.NET (ASMX)](~/xamarin-forms/data-cloud/web-services/asmx.md).

> [!IMPORTANT]
> Поддержка платформы Xamarin для WCF ограничена текстовыми сообщениями SOAP по протоколу HTTP/HTTPS с помощью класса `BasicHttpBinding`.
>
> Поддержка WCF требует использования средств, доступных в среде Windows, для создания прокси-сервера и размещения Тодовкфсервице. Для сборки и тестирования приложения iOS потребуется развернуть Тодовкфсервице на компьютере Windows или в качестве веб-службы Azure.
>
> Собственные приложения Xamarin Forms обычно используют код совместно с библиотекой классов .NET Standard. Однако .NET Core в настоящее время не поддерживает WCF, поэтому общий проект должен быть унаследованной переносимой библиотекой классов. Сведения о поддержке WCF в .NET Core см. в разделе [Выбор между .NET Core и .NET Framework для серверных приложений](/dotnet/standard/choosing-core-framework-server).

Решение примера приложения включает службу WCF, которая может быть запущена локально и показана на следующем снимке экрана:

![](wcf-images/portal.png "Sample Application")

> [!NOTE]
> В iOS 9 и более поздней версии приложение Transport Security (ATS) обеспечивает безопасных соединений между Интернет-ресурсов (например, server серверной части приложения) и приложения, тем самым предотвращая случайное раскрытие конфиденциальной информации. Поскольку ATS включена по умолчанию в приложениях, созданных для iOS 9, все подключения будут применяться требования к безопасности ATS. Если соединения не удовлетворяют этим требованиям, они вызовут сбой с исключением.
>
> ATS можно отключать, если невозможно использовать протокол `HTTPS` и безопасное взаимодействие с Интернет-ресурсами. Это можно сделать, обновив файл **info. plist** приложения. Дополнительные сведения см. в статье [Безопасность транспорта приложений](~/ios/app-fundamentals/ats.md).

## <a name="consume-the-web-service"></a>Использование веб-службы

Служба WCF предоставляет следующие операции:

|Операция|Описание|Параметры|
|--- |--- |--- |
|GetTodoItems|Получение списка элементов задач|
|CreateTodoItem|Создать новый элемент списка дел|XML-сериализации TodoItem|
|EditTodoItem|Обновление элемента задачи|XML-сериализации TodoItem|
|DeleteTodoItem|Удаление элемента задачи|XML-сериализации TodoItem|

Дополнительные сведения о модели данных, используемой в приложении, см. в разделе [моделирование данных](~/xamarin-forms/data-cloud/web-services/introduction.md).

Для использования службы WCF необходимо создать *прокси-сервер* , который позволяет приложению подключаться к службе. Прокси создается с помощью много метаданных службы, определяющий методы и связанные службы конфигурации. Эти метаданные предоставляются в виде документа языка описания служб (WSDL), который создается веб-службой. Прокси-сервер может быть построен с помощью Microsoft WCF Web Service Reference Provider в Visual Studio 2017 для добавления ссылки на службу веб-службы в библиотеку .NET Standard. Вместо создания прокси с помощью Microsoft WCF Web Service Reference Provider в Visual Studio 2017 является использование ServiceModel Metadata Utility Tool (svcutil.exe). Дополнительные сведения см. в разделе [средство служебной программы метаданных ServiceModel (Svcutil. exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/).

Созданный прокси-классы предоставляют методы для использования веб-служб, использующих шаблон проектирования модели асинхронного программирования (APM). В этом шаблоне асинхронная операция реализуется в виде двух методов с именами *бегиноператионнаме* и *ендоператионнаме*, которые начинают и завершают асинхронную операцию.

Метод *бегиноператионнаме* начинает асинхронную операцию и возвращает объект, реализующий интерфейс `IAsyncResult`. После вызова *бегиноператионнаме*приложение может продолжить выполнение инструкций в вызывающем потоке, в то время как асинхронная операция выполняется в потоке пула потоков.

Для каждого вызова *бегиноператионнаме*приложение должно также вызывать *ендоператионнаме* для получения результатов операции. Возвращаемое значение *ендоператионнаме* имеет тот же тип, что и синхронный метод веб-службы. Например, метод `EndGetTodoItems` Возвращает коллекцию экземпляров `TodoItem`. Метод *ендоператионнаме* также включает параметр `IAsyncResult`, для которого необходимо задать экземпляр, возвращаемый соответствующим вызовом метода *бегиноператионнаме* .

Библиотека параллельных задач (TPL) может упростить процесс использования пары методов Begin и End APM, инкапсулирующие асинхронные операции в одном объекте `Task`. Это инкапсуляция обеспечивается несколькими перегрузками метода `TaskFactory.FromAsync`.

Дополнительные сведения о APM см. в разделе [асинхронная модель программирования](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) и [TPL и традиционная .NET Framework асинхронное программирование](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) на MSDN.

### <a name="create-the-todoserviceclient-object"></a>Создание объекта Тодосервицеклиент

Созданный прокси-класс предоставляет класс `TodoServiceClient`, который используется для взаимодействия со службой WCF по протоколу HTTP. Он предоставляет функциональные возможности для вызова методов веб-службы, асинхронных операций из URI определения экземпляра службы. Дополнительные сведения об асинхронных операциях см. в разделе [Общие сведения о поддержке асинхронных](~/cross-platform/platform/async.md)операций.

`TodoServiceClient` экземпляр объявляется на уровне класса, чтобы объект находился в течение всего времени, когда приложению требуется использовать службу WCF, как показано в следующем примере кода:

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

Для экземпляра `TodoServiceClient` настроены сведения о привязке и адрес конечной точки. Привязка используется для указания транспорта, кодировки и протокола, требуемых для приложений и служб для взаимодействия друг с другом. `BasicHttpBinding` указывает, что сообщения SOAP, закодированные в виде текста, будут отправляться через транспортный протокол HTTP. Задание адреса конечной точки позволяет приложению подключаться к различным экземплярам службы WCF, при условии, что несколько экземпляров, опубликованных.

Дополнительные сведения о настройке ссылки на службу см. [в разделе Настройка ссылки на службу](~/cross-platform/data-cloud/web-services/index.md#wcf).

### <a name="create-data-transfer-objects"></a>Создание объектов для обмена данными

Пример приложения использует класс `TodoItem` для моделирования данных. Чтобы сохранить элемент `TodoItem` в веб-службе, его необходимо сначала преобразовать в тип, созданный прокси-сервером `TodoItem`. Это достигается методом `ToWCFServiceTodoItem`, как показано в следующем примере кода:

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

Этот метод просто создает новый экземпляр `TodoWCFService.TodoItem` и присваивает каждому свойству идентичное свойство из экземпляра `TodoItem`.

Аналогично, когда данные извлекаются из веб-службы, они должны быть преобразованы из типа, созданного прокси-сервером `TodoItem`, в экземпляр `TodoItem`. Это достигается с помощью метода `FromWCFServiceTodoItem`, как показано в следующем примере кода:

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

Этот метод просто извлекает данные из типа, созданного прокси-сервером `TodoItem`, и задает его в созданном экземпляре `TodoItem`.

### <a name="retrieve-data"></a>Получение данных

Методы `TodoServiceClient.BeginGetTodoItems` и `TodoServiceClient.EndGetTodoItems` используются для вызова операции `GetTodoItems`, предоставляемой веб-службой. Эти асинхронные методы инкапсулируются в объект `Task`, как показано в следующем примере кода:

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

Метод `Task.Factory.FromAsync` создает `Task`, который выполняет метод `TodoServiceClient.EndGetTodoItems` после завершения метода `TodoServiceClient.BeginGetTodoItems` с параметром `null`, указывающим, что в делегат `BeginGetTodoItems` не передаются никакие данные. Наконец, значение перечисления `TaskCreationOptions` указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

Метод `TodoServiceClient.EndGetTodoItems` возвращает `ObservableCollection` экземпляров `TodoWCFService.TodoItem`, которые затем преобразуются в `List` `TodoItem` экземпляров для вывода.

### <a name="create-data"></a>Создание данных

Методы `TodoServiceClient.BeginCreateTodoItem` и `TodoServiceClient.EndCreateTodoItem` используются для вызова операции `CreateTodoItem`, предоставляемой веб-службой. Эти асинхронные методы инкапсулируются в объект `Task`, как показано в следующем примере кода:

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

Метод `Task.Factory.FromAsync` создает `Task`, который выполняет метод `TodoServiceClient.EndCreateTodoItem` после завершения метода `TodoServiceClient.BeginCreateTodoItem`, а параметр `todoItem` — данные, передаваемые в делегат `BeginCreateTodoItem` для указания `TodoItem`, создаваемого веб-службой. Наконец, значение перечисления `TaskCreationOptions` указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

Веб-служба создает `FaultException`, если не удается создать `TodoItem`, который обрабатывается приложением.

### <a name="update-data"></a>Обновление данных

Методы `TodoServiceClient.BeginEditTodoItem` и `TodoServiceClient.EndEditTodoItem` используются для вызова операции `EditTodoItem`, предоставляемой веб-службой. Эти асинхронные методы инкапсулируются в объект `Task`, как показано в следующем примере кода:

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

Метод `Task.Factory.FromAsync` создает `Task`, который выполняет метод `TodoServiceClient.EndEditTodoItem` после завершения метода `TodoServiceClient.BeginCreateTodoItem`, а параметр `todoItem` — данные, передаваемые в делегат `BeginEditTodoItem` для указания `TodoItem`, обновляемого веб-службой. Наконец, значение перечисления `TaskCreationOptions` указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

Веб-служба создает `FaultException`, если не удается выполнить обнаружение или обновление `TodoItem`, который обрабатывается приложением.

### <a name="delete-data"></a>Удаление данных

Методы `TodoServiceClient.BeginDeleteTodoItem` и `TodoServiceClient.EndDeleteTodoItem` используются для вызова операции `DeleteTodoItem`, предоставляемой веб-службой. Эти асинхронные методы инкапсулируются в объект `Task`, как показано в следующем примере кода:

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

Метод `Task.Factory.FromAsync` создает `Task`, который выполняет метод `TodoServiceClient.EndDeleteTodoItem` после завершения метода `TodoServiceClient.BeginDeleteTodoItem`, а параметр `id` — данные, передаваемые в делегат `BeginDeleteTodoItem` для указания `TodoItem`, удаляемого веб-службой. Наконец, значение перечисления `TaskCreationOptions` указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

Веб-служба создает `FaultException`, если не удается выполнить обнаружение или удаление `TodoItem`, которая обрабатывается приложением.

## <a name="configure-remote-access-to-iis-express"></a>Настройка удаленного доступа к IIS Express
В Visual Studio 2017 или Visual Studio 2019 вы сможете протестировать приложение UWP на ПК без дополнительной настройки. Для тестирования клиентов Android и iOS могут потребоваться дополнительные действия в этом разделе. Дополнительные сведения см. [в статье подключение к локальным веб-службам из симуляторов iOS и эмуляторов Android](~/cross-platform/deploy-test/connect-to-local-web-services.md) .

По умолчанию IIS Express будет отвечать только на запросы `localhost`. Удаленные устройства (например, устройства Android, iPhone или даже симулятор) не будут иметь доступа к локальной службе WCF. Вам нужно будет знать IP-адрес рабочей станции Windows 10 в локальной сети. В этом примере предположим, что на рабочей станции есть IP-адрес `192.168.1.143`. Ниже описаны действия по настройке Windows 10 и IIS Express для приема удаленных подключений и подключения к службе из физического или виртуального устройства.

1. **Добавьте исключение в брандмауэр Windows**. Необходимо открыть порт через брандмауэр Windows, который приложения в подсети могут использовать для взаимодействия со службой WCF. Создайте правило входящего трафика, открыв порт 49393 в брандмауэре. В командной строке администратора выполните следующую команду:

    ```
    netsh advfirewall firewall add rule name="TodoWCFService" dir=in protocol=tcp localport=49393 profile=private remoteip=localsubnet action=allow
    ```

1. **Настройте IIS Express для приема удаленных подключений**. IIS Express можно настроить, изменив файл конфигурации для IIS Express в **папке [каталог решения]\.вс\конфиг\аппликатионхост.конфиг**. Найдите элемент `site` с именем `TodoWCFService`. Он должен выглядеть, как в следующем коде XML:

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

    Необходимо добавить два элемента `binding`, чтобы открыть порт 49393 для внешнего трафика и эмулятора Android. Привязка использует формат `[IP address]:[port]:[hostname]`, который указывает, как IIS Express будет отвечать на запросы. Внешние запросы будут содержать имена узлов, которые должны быть указаны в качестве `binding`. Добавьте следующий XML-код в элемент `bindings`, заменив IP-адрес своим IP-адресом:

    ```xml
    <binding protocol="http" bindingInformation="*:49393:192.168.1.143" />
    <binding protocol="http" bindingInformation="*:49393:127.0.0.1" />
    ```

    После внесения изменений элемент `bindings` должен выглядеть следующим образом:

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

    После выполнения этих действий вы сможете запустить Тодовкфсервице и подключиться с других устройств в подсети. Это можно проверить, запустив приложение и посетив `http://localhost:49393/TodoService.svc`. Если при посещении этого URL-адреса возникает ошибка **неправильного запроса** , `bindings` может быть неправильной в конфигурации IIS Express (запрос приближается IIS Express но отклоняется). Если возникает другая ошибка, возможно, приложение не работает или брандмауэр настроен неправильно.

    Чтобы разрешить IIS Express продолжать работу и обслуживать службу, отключите параметр " **изменить и продолжить** " в **свойствах проекта > отладчики веб->** .

1. **Настройте конечные устройства для доступа к службе**. Этот шаг включает настройку клиентского приложения, работающего на физическом или эмулированного устройстве, для доступа к службе WCF.

    Эмулятор Android использует внутренний прокси-сервер, который не защищает эмулятор от прямого доступа к адресу `localhost` главного компьютера. Вместо этого адрес, `10.0.2.2` в эмуляторе, направляется на `localhost` на размещающем компьютере через внутренний прокси-сервер. Эти запросы с прокси-сервером будут `127.0.0.1` в качестве имени узла в заголовке запроса, поэтому вы создали привязку IIS Express для этого имени узла в описанных выше действиях.

    Симулятор iOS выполняется на узле сборки Mac, даже если вы используете [Удаленный симулятор iOS для Windows](~/tools/ios-simulator/index.md). Сетевые запросы от симулятора будут иметь IP-адрес рабочей станции в локальной сети в качестве имени узла (в этом примере это `192.168.1.143`, но фактический IP-адрес, скорее всего, будет отличаться). Именно поэтому вы создали привязку IIS Express для этого имени узла, как описано выше.

    Убедитесь, что свойство `SoapUrl` в файле **Constants.CS** в проекте Тодовкф (Portable) имеет правильные значения для вашей сети:

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

- [Тодовкф (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todowcf)
- [Руководство. Создание клиента Windows Communication Foundation](https://docs.microsoft.com/dotnet/framework/wcf/how-to-create-a-wcf-client)
- [Средство служебной программы метаданных ServiceModel (Svcutil. exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
