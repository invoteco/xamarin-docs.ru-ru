---
title: Использование службы веб-ASP.NET (ASMX)
description: В этой статье показано, как использовать службу ASMX SOAP из приложения Xamarin.Forms.
ms.prod: xamarin
ms.assetid: D5533964-5528-4D35-9C2B-FAFB632472AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2019
ms.openlocfilehash: 71fb2b4742a66a560541febc9dbe87aed503da4c
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/17/2019
ms.locfileid: "59690281"
---
# <a name="consuming-an-aspnet-web-service-asmx"></a>Использование службы веб-ASP.NET (ASMX)

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX/)

_ASMX предоставляет возможность создавать веб-служб, которые отправляют сообщения, используя Simple Object Access Protocol (SOAP). SOAP — это протокол независимо от платформы и независимый от языка, для создания и вызова веб-служб. Потребителям службы ASMX не нужно ничего знать об используемой платформе, объектной модели или язык программирования, используемый для реализации службы. Они должны понять, как отправлять и получать сообщения SOAP. В этой статье показано, как использовать службу ASMX SOAP из приложения Xamarin.Forms._

Сообщение SOAP — это документ XML, содержащий следующие элементы:

- Корневой элемент с именем *конверт* , идентифицирующий XML-документа, как сообщение SOAP.
- Необязательный *заголовок* элемент, содержащий сведения о приложении, такие как данные проверки подлинности. Если *заголовок* присутствует элемент он должен быть первый дочерний элемент элемента *конверт* элемент.
- Обязательный *текст* элемент, содержащий сообщение SOAP, предназначенное для получателя.
- Необязательный *сбоя* элемент, который используется для указания сообщения об ошибках. Если *сбоя* элемент присутствует, он должен быть дочерним элементом элемента *текст* элемент.

SOAP может работать много транспортных протоколов, включая HTTP, SMTP, TCP и UDP. Тем не менее службы ASMX могут работать только по протоколу HTTP. Платформа Xamarin поддерживает стандартные реализации SOAP 1.1 по протоколу HTTP, и он также поддерживает многие из стандартных конфигураций службы ASMX.

Этот пример включает мобильных приложений, работающих на устройствах физическое или эмулируемое и службы ASMX, предоставляющий методы для получения, добавления, изменения и удаления данных. При запуске приложений для мобильных устройств, они подключаются к службе локально размещенные ASMX, как показано на следующем снимке экрана:

![](asmx-images/portal.png "Пример приложения")

> [!NOTE]
> В iOS 9 и более поздней версии приложение Transport Security (ATS) обеспечивает безопасных соединений между Интернет-ресурсов (например, server серверной части приложения) и приложения, тем самым предотвращая случайное раскрытие конфиденциальной информации. Поскольку ATS включена по умолчанию в приложениях, созданных для iOS 9, все подключения будут применяться требования к безопасности ATS. Если соединения не удовлетворяют этим требованиям, они вызовут сбой с исключением.
> Если невозможно использовать из могут быть присоединены ATS `HTTPS` протокола и безопасный обмен данными для Интернет-ресурсов. Это достигается путем обновления приложения **Info.plist** файла. Дополнительные сведения см. в разделе [безопасность транспорта приложения](~/ios/app-fundamentals/ats.md).

## <a name="consume-the-web-service"></a>Использование веб-службы

Служба ASMX предоставляет следующие операции:

|Операция|Описание|Параметры|
|--- |--- |--- |
|GetTodoItems|Получение списка элементов задач|
|CreateTodoItem|Создать новый элемент списка дел|XML-сериализации TodoItem|
|EditTodoItem|Обновление элемента задачи|XML-сериализации TodoItem|
|DeleteTodoItem|Удаление элемента задачи|XML-сериализации TodoItem|

Дополнительные сведения о модели данных, используемых в приложении, см. в разделе [моделирования данных](~/xamarin-forms/data-cloud/walkthrough.md).

## <a name="create-the-todoservice-proxy"></a>Создание прокси-сервер TodoService

Класс прокси-сервера с именем `TodoService`, расширяет `SoapHttpClientProtocol` и предоставляет методы для взаимодействия со службой ASMX по протоколу HTTP. Прокси создается путем добавления веб-ссылки для каждого проекта под конкретную платформу в 2019 г. Visual Studio или Visual Studio 2017. Веб-ссылки создает методы и события для каждого действия, определенного в документе языка описания служб (WSDL) для службы.

Например `GetTodoItems` службы приводит к тому `GetTodoItemsAsync` метод и `GetTodoItemsCompleted` событие в прокси-сервер. Созданный метод имеет возвращаемый тип void и вызывает `GetTodoItems` действия на родительском `SoapHttpClientProtocol` класса. Когда вызванный метод получает ответ от службы, он срабатывает `GetTodoItemsCompleted` событий и предоставляет данные ответа в событии `Result` свойство.

## <a name="create-the-isoapservice-implementation"></a>Создать реализацию ISoapService

Чтобы включить общий, кросс платформенных проект для работы со службой, в образце определяется `ISoapService` интерфейса, которые соответствуют [задачи модели асинхронного программирования в C# ](/dotnet/csharp/programming-guide/concepts/async/). Каждая платформа реализует `ISoapService` для предоставления платформы прокси-сервер. В образце используется `TaskCompletionSource` объектов для предоставления прокси-сервер как интерфейс асинхронной задачи. Сведения об использовании `TaskCompletionSource` находятся в реализациях каждый тип действия в следующих разделах.

Образец `SoapService`:

1. Создает экземпляр `TodoService` как экземпляр уровня класса
1. Создает коллекцию с именем `Items` для хранения `TodoItem` объектов
1. Указывает настраиваемую конечную точку для необязательного `Url` свойство `TodoService`

```csharp
public class SoapService : ISoapService
{
    ASMXService.TodoService todoService;
    public List<TodoItem> Items { get; private set; } = new List<TodoItem>();

    public SoapService ()
    {
        todoService = new ASMXService.TodoService ();
        todoService.Url = Constants.SoapUrl;
        ...
    }
}
```

### <a name="create-data-transfer-objects"></a>Создание объектов передачи данных

В примере приложения используется `TodoItem` для модели данных. Для хранения `TodoItem` элемента в веб-службы, оно сначала должно быть преобразовано в прокси, созданный `TodoItem` типа. Это достигается путем `ToASMXServiceTodoItem` метод, как показано в следующем примере кода:

```csharp
ASMXService.TodoItem ToASMXServiceTodoItem (TodoItem item)
{
    return new ASMXService.TodoItem {
        ID = item.ID,
        Name = item.Name,
        Notes = item.Notes,
        Done = item.Done
    };
}
```

Этот метод создает новую `ASMService.TodoItem` экземпляра и задает каждое свойство в свойстве идентичные из `TodoItem` экземпляра.

Аналогичным образом, при получении данных из веб-службы, оно должно быть преобразовано из прокси, созданный `TodoItem` тип `TodoItem` экземпляра. Это осуществляется с помощью `FromASMXServiceTodoItem` метод, как показано в следующем примере кода:

```csharp
static TodoItem FromASMXServiceTodoItem (ASMXService.TodoItem item)
{
    return new TodoItem {
        ID = item.ID,
        Name = item.Name,
        Notes = item.Notes,
        Done = item.Done
    };
}
```

Этот метод получает данные из прокси, созданный `TodoItem` введите и задает его в только что созданный `TodoItem` экземпляра.

### <a name="retrieve-data"></a>Получить данные

`ISoapService` Ожидает, что интерфейс `RefreshDataAsync` метод для возврата `Task` с коллекцией элементов. Тем не менее `TodoService.GetTodoItemsAsync` метод возвращает значение void. Чтобы соответствует шаблону интерфейса, необходимо вызвать `GetTodoItemsAsync`, дождитесь `GetTodoItemsCompleted` событий включался и заполнения коллекции. Это позволяет возвращать допустимую коллекцию в пользовательский интерфейс.

В приведенном ниже примере создает новую `TaskCompletionSource`, начинает асинхронный вызов в `RefreshDataAsync` метод и ожидает `Task` предоставляемые `TaskCompletionSource`. Когда `TodoService_GetTodoItemsCompleted` обработчик событий вызывается он заполняет `Items` коллекции и обновления `TaskCompletionSource`:

```csharp
public class SoapService : ISoapService
{
    TaskCompletionSource<bool> getRequestComplete = null;
    ...

    public SoapService()
    {
        ...
        todoService.GetTodoItemsCompleted += TodoService_GetTodoItemsCompleted;
    }

    public async Task<List<TodoItem>> RefreshDataAsync()
    {
        getRequestComplete = new TaskCompletionSource<bool>();
        todoService.GetTodoItemsAsync();
        await getRequestComplete.Task;
        return Items;
    }

    private void TodoService_GetTodoItemsCompleted(object sender, ASMXService.GetTodoItemsCompletedEventArgs e)
    {
        try
        {
            getRequestComplete = getRequestComplete ?? new TaskCompletionSource<bool>();

            Items = new List<TodoItem>();
            foreach (var item in e.Result)
            {
                Items.Add(FromASMXServiceTodoItem(item));
            }
            getRequestComplete?.TrySetResult(true);
        }
        catch (Exception ex)
        {
            Debug.WriteLine(@"\t\tERROR {0}", ex.Message);
        }
    }

    ...
}
```

Дополнительные сведения см. в разделе [модели асинхронного программирования](/dotnet/standard/asynchronous-programming-patterns/asynchronous-programming-model-apm) и [TPL и традиционное .NET Framework асинхронное программирование](/dotnet/standard/parallel-programming/tpl-and-traditional-async-programming).

### <a name="create-or-edit-data"></a>Создание или изменение данных

При создании или изменении данных, необходимо реализовать `ISoapService.SaveTodoItemAsync` метод. Этот метод также обнаруживает ли `TodoItem` — это новый или обновленный элемент и вызывает соответствующий метод для `todoService` объекта. `CreateTodoItemCompleted` И `EditTodoItemCompleted` обработчики событий также должен быть реализован, чтобы знать, когда `todoService` получил ответ от службы ASMX (они могут быть объединены в один обработчик поскольку они выполняют ту же операцию). Ниже приведен пример реализации обработчика интерфейс и событий, а также `TaskCompletionSource` объект, используемый для работать асинхронно:

```csharp
public class SoapService : ISoapService
{
    TaskCompletionSource<bool> saveRequestComplete = null;
    ...

    public SoapService()
    {
        ...
        todoService.CreateTodoItemCompleted += TodoService_SaveTodoItemCompleted;
        todoService.EditTodoItemCompleted += TodoService_SaveTodoItemCompleted;
    }

    public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
    {
        try
        {
            var todoItem = ToASMXServiceTodoItem(item);
            saveRequestComplete = new TaskCompletionSource<bool>();
            if (isNewItem)
            {
                todoService.CreateTodoItemAsync(todoItem);
            }
            else
            {
                todoService.EditTodoItemAsync(todoItem);
            }
            await saveRequestComplete.Task;
        }
        catch (SoapException se)
        {
            Debug.WriteLine("\t\t{0}", se.Message);
        }
        catch (Exception ex)
        {
            Debug.WriteLine("\t\tERROR {0}", ex.Message);
        }
    }

    private void TodoService_SaveTodoItemCompleted(object sender, System.ComponentModel.AsyncCompletedEventArgs e)
    {
        saveRequestComplete?.TrySetResult(true);
    }

    ...
}
```

### <a name="delete-data"></a>Удаление данных

Для удаления данных требуется похожая реализация. Определение `TaskCompletionSource`, Реализуйте обработчик события и `ISoapService.DeleteTodoItemAsync` метод:

```csharp
public class SoapService : ISoapService
{
    TaskCompletionSource<bool> deleteRequestComplete = null;
    ...

    public SoapService()
    {
        ...
        todoService.DeleteTodoItemCompleted += TodoService_DeleteTodoItemCompleted;
    }

    public async Task DeleteTodoItemAsync (string id)
    {
        try
        {
            deleteRequestComplete = new TaskCompletionSource<bool>();
            todoService.DeleteTodoItemAsync(id);
            await deleteRequestComplete.Task;
        }
        catch (SoapException se)
        {
            Debug.WriteLine("\t\t{0}", se.Message);
        }
        catch (Exception ex)
        {
            Debug.WriteLine("\t\tERROR {0}", ex.Message);
        }
    }

    private void TodoService_DeleteTodoItemCompleted(object sender, System.ComponentModel.AsyncCompletedEventArgs e)
    {
        deleteRequestComplete?.TrySetResult(true);
    }

    ...
}
```

## <a name="test-the-web-service"></a>Тестирование веб-службы

Тестирование физическое или эмулируемое устройств с помощью локально размещенной службы требует настраиваемой конфигурации IIS, адреса конечных точек и правил брандмауэра на месте. Дополнительные сведения о том, как настроить среду для тестирования, см. в разделе [настроить удаленный доступ к IIS Express](wcf.md#configure-remote-access-to-iis-express). Единственное различие между WCF и ASMX тестирования — номер порта из TodoService.

## <a name="related-links"></a>Связанные ссылки

- [TodoASMX (пример)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX/)
- [IAsyncResult](https://docs.microsoft.com/dotnet/api/system.iasyncresult)
