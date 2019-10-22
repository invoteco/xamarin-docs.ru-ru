---
title: Служба Azure SignalR с Xamarin. Forms
description: Приступая к работе со службой Azure SignalR и функциями Azure с помощью Xamarin. Forms
ms.prod: xamarin
ms.assetid: 1B9A69EF-C200-41BF-B098-D978D7F9CD8F
author: profexorgeek
ms.author: jusjohns
ms.date: 06/07/2019
ms.openlocfilehash: a4d0f5c5ceefcfe9a36a5fcf10c6fb4937c1db90
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "68739216"
---
# <a name="azure-signalr-service-with-xamarinforms"></a>Служба Azure SignalR с Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azuresignalr/)

ASP.NET Core SignalR — это модель приложения, которая упрощает процесс добавления взаимодействия с приложениями в режиме реального времени. Служба Azure SignalR обеспечивает быструю разработку и развертывание масштабируемых приложений SignalR. В функциях Azure используются кратковременные и независящие от сервера методы, которые можно комбинировать для формирования масштабируемых приложений, управляемых событиями.

В этой статье и примере показано, как объединить службу Azure SignalR и функции Azure с Xamarin. Forms для доставки сообщений в режиме реального времени подключенным клиентам.

## <a name="create-an-azure-signalr-service-and-azure-functions-app"></a>Создание службы Azure SignalR и приложения "функции Azure"

Пример приложения состоит из трех ключевых компонентов: концентратора службы SignalR Azure, экземпляра функций Azure с двумя функциями и мобильного приложения, которое может отправлять и получать сообщения. Эти компоненты взаимодействуют следующим образом:

1. Мобильное приложение вызывает функцию **согласования** Azure для получения сведений о концентраторе SignalR.
1. Мобильное приложение использует сведения о согласовании для регистрации в концентраторе SignalR и формирует соединение.
1. После регистрации мобильное приложение отправляет сообщения в функцию **разговора** Azure.
1. Функция **разговора** передает входящее сообщение в концентратор SignalR.
1. Концентратор SignalR передает сообщение всем подключенным экземплярам мобильных приложений, включая исходного отправителя.

> [!NOTE]
> Функции **Negotiate** и **разговора** в примере приложения можно запускать локально с помощью Visual Studio 2019 и средств среды выполнения Azure. Тем не менее службу Azure SignalR нельзя эмулировать локально, и с ней трудно предоставлять локально размещенные функции Azure физическим или виртуальным устройствам для тестирования. Рекомендуется развернуть функции Azure в экземпляре приложения "функции Azure", так как это позволяет выполнять межплатформенное тестирование. Сведения о развертывании см. [в статье развертывание функций Azure с помощью Visual Studio 2019](#deploy-azure-functions-with-visual-studio-2019).

### <a name="create-an-azure-signalr-service"></a>Создание службы Azure SignalR

Службу Azure SignalR можно создать, выбрав **создать ресурс** в верхнем левом углу портал Azure и выполнив поиск **SignalR**. Службу Azure SignalR можно создать на уровне Free. Служба Azure SignalR должна находиться в режиме **безсерверной** службы. Если вы случайно выбрали режим службы по умолчанию или классический, вы можете изменить его позже в свойствах службы Azure SignalR.

На следующем снимке экрана показано, как создать новую службу Azure SignalR.

![Снимок экрана создания службы Azure SignalR в портал Azure](azure-signalr-images/azure-signalr-create.png "Создание службы Azure SignalR")

После создания раздел **Keys** службы Azure SignalR содержит **строку подключения**, которая используется для подключения приложения функций Azure к концентратору SignalR. На следующем снимке экрана показано, где найти строку подключения в службе Azure SignalR:

![Снимок экрана со строкой подключения Azure SignalR в портал Azure](azure-signalr-images/azure-signalr-connection-string.png "Строка подключения Azure SignalR")

Эта строка подключения используется для [развертывания функций Azure с помощью Visual Studio 2019](#deploy-azure-functions-with-visual-studio-2019).

### <a name="create-an-azure-functions-app"></a>Создание приложения "функции Azure"

Чтобы протестировать пример приложения, необходимо создать новое приложение "функции Azure" в портал Azure. Запишите **имя приложения** , так как этот URL-адрес используется в образце файла приложения **Constants.CS** . На следующем снимке экрана показано создание нового приложения "функции Azure" с именем "ксдоксфунктионс":

[![Screenshot создания приложения для функций Azure](azure-signalr-images/azure-functions-app-cropped.png)](azure-signalr-images/azure-functions-app-full.png#lightbox)

Функции Azure можно развернуть в экземпляре приложения функций Azure из Visual Studio 2019. В следующих разделах описывается развертывание двух функций в примере приложения в экземпляре приложения функций Azure.

### <a name="build-azure-functions-in-visual-studio-2019"></a>Создание функций Azure в Visual Studio 2019

Пример приложения содержит библиотеку классов с именем **чатсервер**, которая включает две бессерверные функции Azure в файлах с именами **Negotiate.CS** и **Talk.CS**.

Функция `Negotiate` реагирует на веб-запросы с помощью объекта `SignalRConnectionInfo`, который содержит свойство `AccessToken` и свойство `Url`. Мобильное приложение использует эти значения для регистрации в концентраторе SignalR. В следующем коде показана функция `Negotiate`:

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

Функция `Talk` отвечает на запросы HTTP POST, которые предоставляют объект сообщения в теле POST. Текст POST преобразуется в `SignalRMessage` и пересылается в концентратор SignalR. В следующем коде показана функция `Talk`:

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

Дополнительные сведения о функциях Azure и приложениях функций Azure см. в [документации по функциям](/azure/azure-functions/)Azure.

### <a name="deploy-azure-functions-with-visual-studio-2019"></a>Развертывание функций Azure с помощью Visual Studio 2019

Visual Studio 2019 позволяет развертывать функции в приложении "функции Azure". Функции, размещаемые в Azure, упрощают межплатформенное тестирование, предоставляя доступную конечную точку тестирования для всех устройств.

Щелкнув правой кнопкой мыши пример приложения "функции" и выбрав пункт " **опубликовать** ", вы запускаете диалоговое окно для публикации функций в приложении "функции Azure". Если вы продали предыдущие шаги для настройки приложение-функция Azure, можно выбрать **вариант выбрать существующий** , чтобы опубликовать примеры приложений в приложении "функции Azure". На следующем снимке экрана показаны параметры диалогового окна публикации в Visual Studio 2019:

![Диалоговое окно вариантов публикации в Visual Studio 2019](azure-signalr-images/vs-publish-target.png "Параметры публикации в Visual Studio 2019")

После входа в учетная запись Майкрософт можно выбрать приложение функций Azure в качестве целевого объекта публикации. На следующем снимке экрана показан пример приложения "функции Azure" в диалоговом окне публикации Visual Studio 2019:

![Приложение "функции Azure" в диалоговом окне публикации Visual Studio 2019](azure-signalr-images/vs-app-selection.png "Приложение функций Azure в диалоговом окне публикации Visual Studio 2019")

После выбора экземпляра приложения функций Azure отображаются URL-адрес сайта, конфигурация и другие сведения о целевом приложении "функции Azure". Выберите **изменить параметры службы приложений Azure** и введите строку подключения в поле **remote (удаленное** ). Строка подключения используется функциями **Negotiate** и **разговора** для подключения к службе Azure SignalR и доступна в разделе **ключи** службы Azure SignalR в портал Azure. Дополнительные сведения о строке подключения см. в статье [Создание службы Azure SignalR](#create-an-azure-signalr-service).

После введения строки подключения можно нажать кнопку **опубликовать** , чтобы развернуть функции в приложении "функции Azure". По завершении функции будут перечислены в приложении "функции Azure" в портал Azure. На следующем снимке экрана показаны опубликованные функции в портал Azure:

![Функции, опубликованные в приложении "функции Azure"](azure-signalr-images/azure-functions-deployed.png "Функции, опубликованные в приложении "функции Azure"")

## <a name="integrate-azure-signalr-service-with-xamarinforms"></a>Интеграция службы Azure SignalR с Xamarin. Forms

Интеграция между службой Azure SignalR и приложением Xamarin. Forms — это класс службы SignalR, который создается в классе `MainPage` с обработчиками событий, назначенными трем событиям. Дополнительные сведения об этих обработчиках событий см. [в разделе Использование класса службы SignalR в Xamarin. Forms](#use-the-signalr-service-class-in-xamarinforms).

Пример приложения включает класс **Constants.CS** , который необходимо настроить с помощью конечной точки URL-адреса приложения функций Azure. В качестве значения свойства `HostName` задайте адрес приложения функций Azure. В следующем коде показаны свойства **Constants.CS** с примером `HostName` значения:

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
> Свойство `Username` в файле примера приложения **Constants.CS** использует значение `RuntimePlatform` устройства в качестве имени пользователя. Это упрощает тестирование устройств на разных платформах и определяет, какое устройство отправляет сообщение. В реальных приложениях это значение, скорее всего, будет уникальным именем пользователя, собранным во время регистрации или входа в систему.

### <a name="the-signalr-service-class"></a>Класс службы SignalR

Класс `SignalRService` в проекте **чатклиент** в примере приложения показывает реализацию, которая вызывает функции в приложении "функции Azure" для подключения к службе Azure SignalR.

Метод `SendMessageAsync` в классе `SignalRService` используется для отправки сообщений клиентам, подключенным к службе SignalR Azure. Этот метод выполняет запрос HTTP POST к функции **разговора** , размещенной в приложении "функции Azure", включая сериализованный в формате JSON объект `Message` в качестве полезных данных POST. Функция **разговора** передает сообщение в службу Azure SignalR для вещания всем подключенным клиентам. В следующем примере кода показан метод `SendMessageAsync`:

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

Метод `ConnectAsync` в классе `SignalRService` выполняет HTTP-запрос GET к функции **Negotiate** , размещенной в приложении "функции Azure". Функция **Negotiate** возвращает JSON, который десериализуется в экземпляр класса `NegotiateInfo`. После получения объекта `NegotiateInfo` он используется для непосредственной регистрации в службе Azure SignalR с помощью экземпляра класса `HubConnection`.

ASP.NET Core SignalR преобразует входящие данные из открытого соединения в сообщения и позволяет разработчикам определять типы сообщений и привязывать обработчики событий к входящим сообщениям по типу. Метод `ConnectAsync` регистрирует обработчик событий для имени сообщения, определенного в примере файла **Constants.CS** приложения, по умолчанию — "невмессаже".

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

Метод `AddNewMessage` привязан как обработчик событий в `ConnectAsync` сообщении, как показано в предыдущем коде. При получении сообщения метод `AddNewMessage` вызывается с данными сообщения, предоставленными в качестве `JObject`. Метод `AddNewMessage` преобразует `JObject` в экземпляр класса `Message`, а затем вызывает обработчик для `NewMessageReceived`, если он был привязан. В следующем примере кода показан метод `AddNewMessage`:

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

### <a name="use-the-signalr-service-class-in-xamarinforms"></a>Использование класса службы SignalR в Xamarin. Forms

Использование класса службы SignalR в Xamarin. Forms осуществляется путем привязки событий `SignalRService` класса в `MainPage` классе кода программной части.

Событие `Connected` в классе `SignalRService` срабатывает при успешном завершении подключения SignalR. Событие `ConnectionFailed` в классе `SignalRService` срабатывает при сбое подключения SignalR. Метод обработчика событий `SignalR_ConnectionChanged` привязан к обоим событиям в конструкторе `MainPage`. Этот обработчик событий обновляет состояния кнопок Connect и send на основе аргумента `success` соединения и добавляет сообщение, предоставленное событием, в очередь разговора с помощью метода `AddMessage`. В следующем коде показан метод обработчика событий `SignalR_ConnectionChanged`:

```csharp
void SignalR_ConnectionChanged(object sender, bool success, string message)
{
    connectButton.Text = "Connect";
    connectButton.IsEnabled = !success;
    sendButton.IsEnabled = success;

    AddMessage($"Server connection changed: {message}");
}
```

Событие `NewMessageReceived` в классе `SignalRService` срабатывает при получении нового сообщения от службы Azure SignalR. Метод обработчика событий `SignalR_NewMessageReceived` привязан к событию `NewMessageReceived` в конструкторе `MainPage`. Этот обработчик событий преобразует входящий объект `Message` в строку и добавляет его в очередь разговора с помощью метода `AddMessage`. В следующем коде показан метод обработчика событий `SignalR_NewMessageReceived`:

```csharp
void SignalR_NewMessageReceived(object sender, Model.Message message)
{
    string msg = $"{message.Name} ({message.TimeReceived}) - {message.Text}";
    AddMessage(msg);
}
```

Метод `AddMessage` добавляет новое сообщение в качестве объекта `Label` в очередь разговора. Метод `AddMessage` часто вызывается обработчиками событий извне потока пользовательского интерфейса, поэтому он заставляет обновления пользовательского интерфейса выполняться в основном потоке, чтобы предотвратить исключения. В следующем примере кода показан метод `AddMessage`:

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

Приложение для разговора с SignalR может быть протестировано на устройствах iOS, Android и UWP, указанных ниже.

1. Служба Azure SignalR создана.
1. Создано приложение функций Azure.
1. Настроили файл **Constants.CS** с помощью конечной точки приложения функций Azure.

После выполнения этих действий и запуска приложения нажмите кнопку Подключить, чтобы **Подключиться** к службе Azure SignalR. При вводе сообщения и нажатии кнопки **Отправить** появляется сообщение, показываемое в очереди разговора в любом подключенном мобильном приложении.

## <a name="related-links"></a>Связанные ссылки

* [Создание мобильных приложений в режиме реального времени с помощью Xamarin и SignalR](https://mybuild.techcommunity.microsoft.com/sessions/77333/)
* [Введение в SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr)
* [Введение в функции Azure](/azure/azure-functions/functions-overview)
* [Документация по функциям Azure](/azure/azure-functions/)
* [Пример разговора MVVM SignalR](https://github.com/lbugnion/sample-xamarin-signalr)
