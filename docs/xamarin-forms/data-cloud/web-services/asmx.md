---
title: Использование веб-службы ASP.NET (ASMX)
description: В этой статье показано, как использовать службу ASMX SOAP из приложения Xamarin.Forms.
ms.prod: xamarin
ms.assetid: D5533964-5528-4D35-9C2B-FAFB632472AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2019
ms.openlocfilehash: 124cffaf827ebeb8109f3cd4ac4dfd2701e43f9c
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656660"
---
# <a name="consume-an-aspnet-web-service-asmx"></a>Использование веб-службы ASP.NET (ASMX)

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todoasmx)

_ASMX предоставляет возможность создавать веб-служб, которые отправляют сообщения, используя Simple Object Access Protocol (SOAP). SOAP — это протокол независимо от платформы и независимый от языка, для создания и вызова веб-служб. Потребителям службы ASMX не нужно ничего знать об используемой платформе, объектной модели или язык программирования, используемый для реализации службы. Они должны понять, как отправлять и получать сообщения SOAP. В этой статье показано, как использовать службу ASMX SOAP из приложения Xamarin.Forms._

Сообщение SOAP — это документ XML, содержащий следующие элементы:

- Корневой элемент с именем *конверт* , идентифицирующий XML-документа, как сообщение SOAP.
- Необязательный *заголовок* элемент, содержащий сведения о приложении, такие как данные проверки подлинности. Если *заголовок* присутствует элемент он должен быть первый дочерний элемент элемента *конверт* элемент.
- Обязательный *текст* элемент, содержащий сообщение SOAP, предназначенное для получателя.
- Необязательный *сбоя* элемент, который используется для указания сообщения об ошибках. Если *сбоя* элемент присутствует, он должен быть дочерним элементом элемента *текст* элемент.

SOAP может работать много транспортных протоколов, включая HTTP, SMTP, TCP и UDP. Тем не менее службы ASMX могут работать только по протоколу HTTP. Платформа Xamarin поддерживает стандартные реализации SOAP 1.1 по протоколу HTTP, и он также поддерживает многие из стандартных конфигураций службы ASMX.

Этот пример включает мобильные приложения, работающие на физических или эмулированных устройствах, и службу ASMX, которая предоставляет методы для получения, добавления, изменения и удаления данных. При запуске мобильных приложений они подключаются к размещенной локально службе ASMX, как показано на следующем снимке экрана:

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

Дополнительные сведения о модели данных, используемых в приложении, см. в разделе [моделирования данных](~/xamarin-forms/data-cloud/web-services/introduction.md).

## <a name="create-the-todoservice-proxy"></a>Создание прокси-сервера файле todoservice

Класс прокси, вызываемый `TodoService`, расширяет `SoapHttpClientProtocol` и предоставляет методы для взаимодействия со службой ASMX по протоколу HTTP. Прокси-сервер создается путем добавления веб-ссылки на каждый проект конкретной платформы в Visual Studio 2019 или Visual Studio 2017. Веб-ссылка создает методы и события для каждого действия, определенного в документе языка описания веб-служб (WSDL) службы.

Например, `GetTodoItems` действие службы приводит к `GetTodoItemsAsync` методу и `GetTodoItemsCompleted` событию в прокси-сервере. Созданный метод имеет тип возвращаемого значения void и вызывает `GetTodoItems` действие для родительского `SoapHttpClientProtocol` класса. Когда вызванный метод получает ответ от службы, он запускает `GetTodoItemsCompleted` событие и предоставляет данные ответа в `Result` свойстве события.

## <a name="create-the-isoapservice-implementation"></a>Создание реализации Исоапсервице

Чтобы обеспечить работу многоплатформенного проекта для работы со службой, в примере определяется `ISoapService` интерфейс, который следует за [моделью асинхронного программирования задач в C# ](/dotnet/csharp/programming-guide/concepts/async/). Каждая платформа реализует `ISoapService` для предоставления прокси-сервера, зависящего от платформы. В примере используются `TaskCompletionSource` объекты для предоставления прокси в качестве асинхронного интерфейса задачи. Сведения об использовании `TaskCompletionSource` приведены в реализациях каждого типа действия в следующих разделах.

Пример `SoapService`:

1. Создает экземпляр в `TodoService` качестве экземпляра уровня класса
1. Создает коллекцию, `Items` вызываемую `TodoItem` для хранения объектов
1. Указывает пользовательскую конечную точку для `Url` необязательного свойства в`TodoService`

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

### <a name="create-data-transfer-objects"></a>Создание объектов для обмена данными

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

Этот метод создает новый `ASMService.TodoItem` экземпляр и присваивает каждому свойству идентичное свойство `TodoItem` из экземпляра.

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

Этот метод извлекает данные из типа, созданного `TodoItem` прокси-сервером, и задает его в `TodoItem` созданном экземпляре.

### <a name="retrieve-data"></a>Получение данных

Интерфейсу требуется, чтобы `Task` метод возвращал объект со коллекцией элементов. `RefreshDataAsync` `ISoapService` `TodoService.GetTodoItemsAsync` Однако метод возвращает значение void. Чтобы удовлетворить шаблон интерфейса, необходимо вызвать `GetTodoItemsAsync`, дождаться `GetTodoItemsCompleted` срабатывания события и заполнить коллекцию. Это позволяет вернуть в пользовательский интерфейс допустимую коллекцию.

Приведенный ниже пример создает новый `TaskCompletionSource`, начинает асинхронный вызов `RefreshDataAsync` в методе и ожидает объект `Task` , предоставленный `TaskCompletionSource`. При вызове обработчика `Items` `TaskCompletionSource`события он заполняет коллекцию и обновляет: `TodoService_GetTodoItemsCompleted`

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

Дополнительные сведения см. в статьях [асинхронная модель программирования](/dotnet/standard/asynchronous-programming-patterns/asynchronous-programming-model-apm) и [TPL и традиционная .NET Framework асинхронное программирование](/dotnet/standard/parallel-programming/tpl-and-traditional-async-programming).

### <a name="create-or-edit-data"></a>Создание или изменение данных

При создании или изменении данных необходимо реализовать `ISoapService.SaveTodoItemAsync` метод. Этот метод определяет, `TodoItem` является ли элемент новым или обновленным и вызывает соответствующий метод `todoService` для объекта. Обработчики `EditTodoItemCompleted` событий `todoService` и также должны быть реализованы, чтобы вы узнали, когда получил ответ от службы ASMX (они могут быть объединены в один обработчик, так как они выполняют одну и ту же операцию). `CreateTodoItemCompleted` В следующем примере демонстрируются реализации интерфейса и обработчика событий, а также `TaskCompletionSource` объект, используемый для асинхронной обработки:

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

Для удаления данных требуется аналогичная реализация. Определите, реализуйте обработчик событий `ISoapService.DeleteTodoItemAsync` и метод: `TaskCompletionSource`

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

Для тестирования физических или эмулированных устройств с помощью локально размещенной службы требуются пользовательские конфигурации IIS, адреса конечных точек и правила брандмауэра. Дополнительные сведения о настройке среды для тестирования см. в разделе [Настройка удаленного доступа к IIS Express](wcf.md#configure-remote-access-to-iis-express). Единственное различие между тестированием WCF и ASMX — номер порта файле todoservice.

## <a name="related-links"></a>Связанные ссылки

- [TodoASMX (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todoasmx)
- [IAsyncResult](https://docs.microsoft.com/dotnet/api/system.iasyncresult)
