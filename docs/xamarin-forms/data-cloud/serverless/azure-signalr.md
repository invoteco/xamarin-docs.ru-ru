---
title: Служба Azure SignalR с помощью Xamarin.Forms
description: Начало работы с SignalR службы Azure и функций Azure с помощью Xamarin.Forms
ms.prod: xamarin
ms.assetid: 1B9A69EF-C200-41BF-B098-D978D7F9CD8F
author: profexorgeek
ms.author: jusjohns
ms.date: 06/07/2019
ms.openlocfilehash: d79ffb844a65c145fd6cb22a5a3e1dfc7a6bfe41
ms.sourcegitcommit: 0f78ec17210b915b43ddab75937de8063e472c70
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/27/2019
ms.locfileid: "67418127"
---
# <a name="azure-signalr-service-with-xamarinforms"></a>Служба Azure SignalR с помощью Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-samples/tree/master/WebServices/AzureSignalR)

ASP.NET Core SignalR представляет собой модель приложения, которая упрощает процесс добавления связи в режиме реального времени к приложениям. Служба Azure SignalR позволяет быстрой разработки и внедрения масштабируемых приложений SignalR. Функции Azure являются короткоживущими, бессерверного кода методы, которые могут быть объединены для формы событиями и масштабируемых приложений.

Это статья и Образец показано, как объединить служба Azure SignalR и функций Azure с помощью Xamarin.Forms для доставки сообщений в режиме реального времени для подключенных клиентов.

## <a name="create-an-azure-signalr-service-and-azure-functions-app"></a>Создание приложения-функции Azure и служба Azure SignalR

Пример приложения состоит из трех основных компонентов: концентратор служба Azure SignalR, экземпляр "функции Azure" с двумя функциями и мобильного приложения, можно отправлять и получать сообщения. Эти компоненты взаимодействуют следующим образом:

1. Мобильное приложение вызывает **Negotiate** функцию Azure для получения сведений о концентратора SignalR.
1. Мобильное приложение использует сведения согласования, чтобы зарегистрироваться в центре SignalR и формирует соединения.
1. После регистрации, мобильное приложение отправляет сообщения **поговорим** функции Azure.
1. **Поговорим** функция передает входящее сообщение в концентратор SignalR.
1. Концентратор SignalR осуществляет широковещательную передачу сообщений для всех экземпляров подключенным мобильным приложением, включая отправителю.

> [!NOTE]
> **Negotiate** и **поговорим** функции в примере приложения могут выполняться локально с помощью Visual Studio 2019 и средства среды выполнения Azure. Тем не менее служба Azure SignalR невозможно эмулировать локально и сложен для предоставления локально размещенные функций Azure на физические или виртуальные устройства для тестирования. Рекомендуется развертывание функций Azure в экземпляр приложения функций Azure, так как он разрешает платформ тестирования. Дополнительные сведения о развертывании, см. в разделе [развертывание функций Azure с помощью Visual Studio 2019](#deploy-azure-functions-with-visual-studio-2019).

### <a name="create-an-azure-signalr-service"></a>Создание службы Azure SignalR

Это служба Azure SignalR можно создать, выбрав **создать ресурс** в верхнем левом углу на портале Azure и поиске **SignalR**. Служба Azure SignalR могут создаваться на уровне "бесплатный". Служба Azure SignalR должна быть в **бессерверных** режим службы. Если вы случайно выбрать, по умолчанию или режим классической службы, его можно изменить позже в свойствах служба Azure SignalR.

На следующем рисунке показаны создание новой службы SignalR Azure:

![Создание экрана SignalR службы Azure на портале Azure](azure-signalr-images/azure-signalr-create.png "создается служба Azure SignalR")

После создания **ключи** раздел службы SignalR Azure содержит **строку подключения**, который используется для подключения приложения функций Azure в концентратор SignalR. На следующем рисунке показаны, где можно найти строку подключения в службе Azure SignalR:

![Снимок экрана Azure SignalR строку подключения на портале Azure](azure-signalr-images/azure-signalr-connection-string.png "строки подключения Azure SignalR")

Эта строка подключения используется [развертывание функций Azure с помощью Visual Studio 2019](#deploy-azure-functions-with-visual-studio-2019).

### <a name="create-an-azure-functions-app"></a>Создание приложения функций Azure

Чтобы протестировать пример приложения, следует создать новое приложение функций Azure на портале Azure. Запомните или запишите **имя_приложения** так как этот URL-адрес используется в приложении-примере **Constants.cs** файла. На следующем рисунке показаны создание нового приложения функций Azure, называется «xdocsfunctions»:

[![Создание экрана из Azure функции приложения](azure-signalr-images/azure-functions-app-cropped.png)](azure-signalr-images/azure-functions-app-full.png#lightbox)

Функции Azure можно развернуть на экземпляр приложения функций Azure из Visual Studio 2019. В следующих разделах развертывания из двух функций в примере приложения к экземпляру приложения функций Azure.

### <a name="build-azure-functions-in-visual-studio-2019"></a>Создание функций Azure в Visual Studio 2019 г.

Пример приложения содержит библиотеку классов с именем **ChatServer**, который включает два бессерверных функций Azure в файлы, называемые **Negotiate.cs** и **Talk.cs**.

`Negotiate` — Функция отвечает на веб-запросов с `SignalRConnectionInfo` , содержащий `AccessToken` свойство и `Url` свойство. Мобильное приложение использует эти значения для регистрации в концентратор SignalR. В следующем коде показан `Negotiate` функции:

```csharp
[FunctionName("Negotiate")]
public static SignalRConnectionInfo GetSignalRInfo(
    [HttpTrigger(AuthorizationLevel.Anonymous,"get",Route = "negotiate")]
    HttpRequest req,
    [SignalRConnectionInfo(HubName = "simplechat")]
    SignalRConnectionInfo connectionInfo)
{
    return connectionInfo;
}
```

`Talk` — Функция отвечает на запросы HTTP POST, которые предоставляют объект сообщения в тело POST. Тело запроса POST преобразуется в `SignalRMessage` и перенаправляются к концентратору SignalR. В следующем коде показан `Talk` функции:

```csharp
[FunctionName("Talk")]
public static async Task<IActionResult> Run(
    [HttpTrigger(
        AuthorizationLevel.Anonymous,
        "post",
        Route = "talk")]
    HttpRequest req,
    [SignalR(HubName = "simplechat")]
    IAsyncCollector<SignalRMessage> questionR,
    ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    try
    {
        string json = await new StreamReader(req.Body).ReadToEndAsync();
        dynamic obj = JsonConvert.DeserializeObject(json);

        var name = obj.name.ToString();
        var text = obj.text.ToString();

        var jObject = new JObject(obj);

        await questionR.AddAsync(
            new SignalRMessage
            {
                Target = "newMessage",
                Arguments = new[] { jObject }
            });

        return new OkObjectResult($"Hello {name}, your message was '{text}'");
    }
    catch (Exception ex)
    {
        return new BadRequestObjectResult("There was an error: " + ex.Message);
    }
}
```

Дополнительные сведения о функции Azure и приложений в функциях Azure, см. в разделе [документации по функциям Azure](/azure/azure-functions/).

### <a name="deploy-azure-functions-with-visual-studio-2019"></a>Развертывание функции Azure с помощью Visual Studio 2019 г.

2019 г. Visual Studio позволяет развертывать функции для приложения функций Azure. Размещенный в Azure функции облегчают, между различными платформами тестирования, предоставляя доступная конечная точка тестирования для всех устройств.

Щелкните правой кнопкой мыши пример приложения функции и выбор **публикации** запускает диалоговое окно Публикация функций для приложения функций Azure. Если вы выполнили предыдущие шаги, чтобы настроить приложение-функцию Azure, вы можете **выбрать существующее** Публикация примеров приложений для приложения функций Azure. На следующем рисунке показаны параметры в диалоговом окне публикации в Visual Studio 2019 г.:

![Диалоговое окно выбора публикации в Visual Studio 2019](azure-signalr-images/vs-publish-target.png "параметры публикации в Visual Studio 2019 г.")

После входа учетную запись Майкрософт, можно найти и выбрать приложения функций Azure в качестве цели публикации. На следующем рисунке показан пример приложения функций Azure в диалоговом окне публикации 2019 г. Visual Studio:

![Приложения функций Azure в диалоговом окне публикации Visual Studio 2019](azure-signalr-images/vs-app-selection.png "приложения функций Azure в диалоговое окно публикации Visual Studio 2019 г.")

После выбора приложения функций Azure отображается экземпляр, URL-адрес сайта, конфигурации и другие сведения о цели приложения функций Azure. Выберите **изменить параметры службы приложений Azure** и введите строку подключения в **удаленного** поля. Строка подключения используется **Negotiate** и **поговорим** функции для подключения к службе Azure SignalR и доступен в **ключи** раздел Azure SignalR Службы на портале Azure. Дополнительные сведения о строке подключения, см. в разделе [Создание службы Azure SignalR](#create-an-azure-signalr-service).

Когда вы введете строку подключения, можно щелкнуть **публикации** для развертывания своих функций в приложение функции Azure. После завершения функции будут перечислены в приложения функций Azure на портале Azure. На следующем рисунке показаны функции, опубликованной на портале Azure:

![Функции, опубликованных в приложения функций Azure](azure-signalr-images/azure-functions-deployed.png "функции публикации в приложения функций Azure")

## <a name="integrate-azure-signalr-service-with-xamarinforms"></a>Интеграция служба Azure SignalR с помощью Xamarin.Forms

Интеграция между служба Azure SignalR и в приложении Xamarin.Forms — класс службы SignalR, экземпляр которого создается в `MainPage` класса с обработчиками событий, назначенных три события. Дополнительные сведения об этих обработчиках событий см. в разделе [используйте класс службы SignalR в Xamarin.Forms](#use-the-signalr-service-class-in-xamarinforms).

Образец приложения включает **Constants.cs** класс, который следует настроить, используя URL-адрес конечной точки приложения функций Azure. Установите для параметра `HostName` свойство на ваш адрес для приложения функций Azure. В следующем коде показан **Constants.cs** свойства с примером `HostName` значение:

```csharp
public static class Constants
{
    public static string HostName { get; set; } = "https://example-functions-app.azurewebsites.net/";

    // Used to differentiate message types sent via SignalR. This
    // sample only uses a single message type.
    public static string MessageName { get; set; } = "newMessage";

    public static string Username
    {
        get
        {
            return $"{Device.RuntimePlatform} User";
        }
    }
}
```

> [!NOTE]
> `Username` Свойства в приложении-примере **Constants.cs** файл использует устройства `RuntimePlatform` значение в качестве имени пользователя. Это упрощает тестирование платформ устройств и определить, какое устройство отправляет сообщение. В реальном приложении это значение скорее всего, будет уникальное имя пользователя, собранные во время входа вверх или войдите в процесс.

### <a name="the-signalr-service-class"></a>Класс службы SignalR

`SignalRService` В класс **ChatClient** проекта в примере приложения показывает реализацию, который вызывает функции в приложения функций Azure для подключения к службе Azure SignalR.

`SendMessageAsync` Метод в `SignalRService` класс используется для отправки сообщений для клиентов, подключенных к службе Azure SignalR. Этот метод выполняет запрос HTTP POST для **поговорим** функции, размещенного в приложении функции Azure, включая JSON-сериализованный `Message` объект в качестве полезной нагрузки POST. **Поговорим** функция передает сообщения служба Azure SignalR для широкое вещание на всех подключенных клиентов. В следующем примере кода показан метод `SendMessageAsync`:

```csharp
public async Task SendMessageAsync(string username, string message)
{
    IsBusy = true;

    var newMessage = new Message
    {
        Name = username,
        Text = message
    };

    var json = JsonConvert.SerializeObject(newMessage);
    var content = new StringContent(json, Encoding.UTF8, "application/json");
    var result = await client.PostAsync($"{Constants.HostName}/api/talk", content);

    IsBusy = false;
}
```

`ConnectAsync` Метод в `SignalRService` класс выполняет запрос HTTP GET к **Negotiate** функции, размещенного в приложении функций Azure. **Negotiate** функция возвращает JSON, который десериализуется в экземпляр `NegotiateInfo` класса. Один раз `NegotiateInfo` извлечь объект, он используется для регистрации непосредственно в службе Azure SignalR с помощью экземпляра `HubConnection` класса.

ASP.NET Core SignalR преобразует входящие данные из открытого подключения в сообщения и позволяет разработчикам определять типы сообщений и привязать обработчики событий для обработки сообщений по типу. `ConnectAsync` Метод регистрирует обработчик для сообщения имени, определенному в приложении-примере **Constants.cs** файл, который является «newMessage» по умолчанию.

В следующем примере кода показан метод `ConnectAsync`:

```csharp
public async Task ConnectAsync()
{
    try
    {
        IsBusy = true;

        string negotiateJson = await client.GetStringAsync($"{Constants.HostName}/api/negotiate");
        NegotiateInfo negotiate = JsonConvert.DeserializeObject<NegotiateInfo>(negotiateJson);
        HubConnection connection = new HubConnectionBuilder()
            .WithUrl(negotiate.Url, options =>
            {
                options.AccessTokenProvider = async () => negotiate.AccessToken;
            })
            .Build();

        connection.On<JObject>(Constants.MessageName, AddNewMessage);
        await connection.StartAsync();

        IsConnected = true;
        IsBusy = false;

        Connected?.Invoke(this, true, "Connection successful.");
    }
    catch (Exception ex)
    {
        ConnectionFailed?.Invoke(this, false, ex.Message);
    }
}
```

`AddNewMessage` Метод привязан как обработчик событий в `ConnectAsync` сообщения, как показано в предыдущем коде. При получении сообщения `AddNewMessage` с данными сообщения, как вызывается метод `JObject`. `AddNewMessage` Метод преобразует `JObject` к экземпляру `Message` класса и затем вызывает обработчик для `NewMessageReceived` Если одна из них привязана. В следующем примере кода показан метод `AddNewMessage`:

```csharp
public void AddNewMessage(JObject message)
{
    Message messageModel = new Message
    {
        Name = message.GetValue("name").ToString(),
        Text = message.GetValue("text").ToString(),
        TimeReceived = DateTime.Now
    };

    NewMessageReceived?.Invoke(this, messageModel);
}
```

### <a name="use-the-signalr-service-class-in-xamarinforms"></a>Используйте класс службы SignalR в Xamarin.Forms

Используя класс службы SignalR в Xamarin.Forms достигается путем привязки `SignalRService` класса событий в `MainPage` класса вспомогательного кода.

`Connected` Событие в `SignalRService` класс возникает при успешном подключении SignalR. `ConnectionFailed` Событие в `SignalRService` класс возникает при сбое подключения SignalR. `SignalR_ConnectionChanged` Метод обработчика событий привязан к обоих событий в `MainPage` конструктор. Этот обработчик обновляет состояния кнопки connect и отправки, на основе подключения `success` аргумента и добавляет сообщение, передаваемое событие в очередь чата с помощью `AddMessage` метод. В следующем коде показан `SignalR_ConnectionChanged` метод обработчика событий:

```csharp
void SignalR_ConnectionChanged(object sender, bool success, string message)
{
    connectButton.Text = "Connect";
    connectButton.IsEnabled = !success;
    sendButton.IsEnabled = success;

    AddMessage($"Server connection changed: {message}");
}
```

`NewMessageReceived` Событие в `SignalRService` класс возникает при получении нового сообщения из SignalR службы Azure. `SignalR_NewMessageReceived` Метод обработчика событий привязан к `NewMessageReceived` событие в `MainPage` конструктор. Этот обработчик событий преобразует входящий `Message` в строку и добавляет его в очередь чата с помощью `AddMessage` метод. В следующем коде показан `SignalR_NewMessageReceived` метод обработчика событий:

```csharp
void SignalR_NewMessageReceived(object sender, Model.Message message)
{
    string msg = $"{message.Name} ({message.TimeReceived}) - {message.Text}";
    AddMessage(msg);
}
```

`AddMessage` Метод добавляет новое сообщение, как `Label` объект в очередь чата. `AddMessage` Метод часто вызывается обработчики событий из-за пределами основной поток пользовательского интерфейса, поэтому он заставляет выполнения обновлений пользовательского интерфейса в основном потоке для предотвращения возникновения исключений. В следующем примере кода показан метод `AddMessage`:

```csharp
void AddMessage(string message)
{
    Device.BeginInvokeOnMainThread(() =>
    {
        Label label = new Label
        {
            Text = message,
            HorizontalOptions = LayoutOptions.Start,
            VerticalOptions = LayoutOptions.Start
        };

        messageList.Children.Add(label);
    });
}
```

## <a name="test-the-application"></a>Тестирование приложения

Можно тестировать приложения чата SignalR на iOS, Android и универсальной платформы Windows, при условии, что у вас есть:

1. Создать службу Azure SignalR.
1. Создали приложение функции Azure.
1. Настроить **Constants.cs** файл с конечной точкой приложения функций Azure.

После выполнения этих действий и запустить приложение, нажав кнопку **Connect** кнопка создает подключение к службе Azure SignalR. Введите сообщение и щелкните **отправки** результаты кнопки в сообщениях в очереди чата на любой подключенные мобильными приложениями.

## <a name="related-links"></a>Связанные ссылки

* [Берет на себя создание в режиме реального времени мобильных приложений с помощью Xamarin и SignalR](https://mybuild.techcommunity.microsoft.com/sessions/77333/)
* [Введение в SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr)
* [Введение в функции Azure](/azure/azure-functions/functions-overview)
* [Документация по функциям Azure](/azure/azure-functions/)
* [Пример чата MVVM SignalR](https://github.com/lbugnion/sample-xamarin-signalr)
