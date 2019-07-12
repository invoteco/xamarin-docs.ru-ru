---
title: Аутентификация и авторизация
description: В данной главе объясняется, как мобильное приложение eShopOnContainers выполняет проверку подлинности и авторизацию для контейнерных микрослужб.
ms.prod: xamarin
ms.assetid: e3f27b4c-f7f5-4839-a48c-30bcb919c59e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2017
ms.openlocfilehash: efaea24e559aa2f3bdfd87c1c083ce1d777dbb3f
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832165"
---
# <a name="authentication-and-authorization"></a>Аутентификация и авторизация

Проверка подлинности — это процесс получения учетных данных идентификации, такие как имя и пароль пользователя и проверки этих учетных данных на наличие полномочий. Если учетные данные действительны, сущность, которая отправлена учетные данные, считается прошедшим проверку подлинности. После проверки подлинности удостоверения процесс авторизации определяет, имеет ли это удостоверение доступ к конкретному ресурсу.

Существует много подходов к интеграции проверки подлинности и авторизации в приложение Xamarin.Forms, которое взаимодействует с веб-приложения ASP.NET MVC, включая поставщиков внешней проверки подлинности, например Microsoft, Google, с помощью ASP.NET Core Identity Facebook или Twitter и проверки подлинности по промежуточного слоя. В мобильном приложении eShopOnContainers выполняет проверку подлинности и авторизации с помощью микрослужбы контейнерных удостоверений, которая использует IdentityServer 4. Мобильное приложение запрашивает у IdentityServer, маркеры безопасности, для проверки подлинности пользователя или для доступа к ресурсу. Для IdentityServer для выдачи маркеров от имени пользователя пользователь должен войти в к IdentityServer. Тем не менее сервер не предоставляет пользовательский интерфейс или базы данных для проверки подлинности. Таким образом в образце приложения eShopOnContainers, для этой цели используется удостоверение ASP.NET Core.

## <a name="authentication"></a>Проверка подлинности

Проверка подлинности необходима, если приложению должно быть известно удостоверение текущего пользователя. ASP.NET Core основным механизмом для идентификации пользователей является система членства ASP.NET Core Identity, которая хранит сведения о пользователе в хранилище данных, настроенном разработчиком. Как правило это хранилище данных будет хранилище EntityFramework, хотя пользовательские хранилища или пакеты сторонних может использоваться для хранения данных удостоверений в хранилище Azure, Azure Cosmos DB или других местах.

Сценарии проверки подлинности, которые делают использование хранилища данных локального пользователя, и которые сохраняются сведения об удостоверениях между запросами с помощью файлов cookie (как обычно бывает в веб-приложений ASP.NET MVC), удостоверение ASP.NET Core — это подходящее решение. Тем не менее файлы cookie не всегда нецелесообразно хранить и передавать данные. Например веб-приложение ASP.NET Core, которая предоставляет RESTful конечных точек, доступных из мобильного приложения обычно требуется для использования проверки подлинности токена носителя, так как файлы cookie не может использоваться в этом сценарии. Тем не менее токены носителя можно легко получать и содержится в заголовке авторизации веб-запросов, сделанных в мобильном приложении.

### <a name="issuing-bearer-tokens-using-identityserver-4"></a>Выдает маркеры носителя, с помощью IdentityServer 4

[IdentityServer 4](https://github.com/IdentityServer/IdentityServer4) — фреймворк с открытым кодом OpenID Connect и OAuth 2.0 для ASP.NET Core, который может использоваться для многих сценариев проверки подлинности и авторизации, включая выдает маркеры безопасности для локальных пользователей удостоверения ASP.NET Core.

> [!NOTE]
> OpenID Connect и OAuth 2.0 очень похожи, имея различные обязанности.

OpenID Connect представляет собой уровень проверки подлинности на основе протокола OAuth 2.0. OAuth 2 — это протокол, который позволяет приложениям запрашивать маркеры доступа от службы маркеров безопасности и использовать их для взаимодействия с интерфейсами API. Это делегирование снижает сложность в клиентские приложения и интерфейсы API, так как можно быть централизованной проверки подлинности и авторизации.

Сочетание OpenID Connect и OAuth 2.0 объединить две проблемы основополагающих принципов обеспечения безопасности проверки подлинности и доступа к API и IdentityServer 4 представляет собой реализацию этих протоколов.

В приложениях, использующих прямое взаимодействие клиента с микрослужбой, таких как приложения eShopOnContainers выделенная микрослужба аутентификации выступает в качестве токенов безопасности службы (STS) можно использовать для проверки подлинности пользователей, как показано на рис. 9-1. Дополнительные сведения о прямое взаимодействие клиента с микрослужбой, см. в разделе [обмен данными между клиентом и Микрослужб](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md#communication_between_client_and_microservices).

![](authentication-and-authorization-images/authentication.png "Проверка подлинности с выделенная микрослужба аутентификации")

**Рис. 9-1.** Проверка подлинности с выделенная микрослужба аутентификации

В мобильном приложении eShopOnContainers взаимодействует с микрослужбы по идентификации, который использует 4 identityserver должно выполнять проверку подлинности и контроль доступа для интерфейсов API. Таким образом мобильное приложение запрашивает маркеры у IdentityServer, для проверки подлинности пользователя или для доступа к ресурсу:

-   Проверка подлинности пользователей с identityserver должно достигается запросив мобильное приложение *удостоверений* маркер, который представляет результат процесса проверки подлинности. Минимум он содержит идентификатор пользователя и сведения о том, как и когда пользователь прошел проверку подлинности. Он также может содержать дополнительные идентификационных данных.
-   Доступ к ресурсу с identityserver должно достигается запросив мобильное приложение *доступа* маркер, который разрешает доступ к ресурсу API. Клиенты, запроса маркеров доступа и пересылать их в API. Маркеры доступа содержат сведения о клиенте и пользователь (при его наличии). API-интерфейсы, затем использовать эту информацию для авторизации доступа к их данным.

> [!NOTE]
> Клиента должен быть зарегистрирован с identityserver должно, прежде чем он может запрашивать маркеры.

### <a name="adding-identityserver-to-a-web-application"></a>Добавление IdentityServer веб-приложение

Чтобы веб-приложение ASP.NET Core для использования IdentityServer 4 его необходимо добавить в решение Visual Studio веб-приложения. Дополнительные сведения см. в разделе [Обзор](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) в документации по IdentityServer.

После IdentityServer включен в решение Visual Studio веб-приложения, его необходимо добавить в конвейер обработки запросов HTTP для веб-приложения, чтобы она может выполнять запросы к конечным точкам OpenID Connect и OAuth 2.0. Это можно сделать в `Configure` метод веб-приложения `Startup` класса, как показано в следующем примере кода:

```csharp
public void Configure(  
    IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)  
{  
    ...  
    app.UseIdentity();  
    ...  
}
```

Порядок имеет значение в конвейер обработки запросов HTTP для веб-приложения. Таким образом IdentityServer должны добавляться в конвейер, прежде чем платформа пользовательского интерфейса, который реализует экран входа.

### <a name="configuring-identityserver"></a>Настройка IdentityServer

IdentityServer должно быть настроено в `ConfigureServices` метод веб-приложения `Startup` класс путем вызова `services.AddIdentityServer` метод, как показано в следующем примере кода из эталонного приложения eShopOnContainers:

```csharp
public void ConfigureServices(IServiceCollection services)  
{  
    ...  
    services.AddIdentityServer(x => x.IssuerUri = "null")  
        .AddSigningCredential(Certificate.Get())                 
        .AddAspNetIdentity<ApplicationUser>()  
        .AddConfigurationStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .AddOperationalStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .Services.AddTransient<IProfileService, ProfileService>();  
}
```

После вызова метода `services.AddIdentityServer` метод, настройте следующие компоненты называются дополнительные текучего API:

-   Учетные данные, используемые для подписывания.
-   Ресурсы API и удостоверений, которые пользователи могут запрашивать доступ к.
-   Клиенты, которые будут подключаться для запроса маркеров.
-   Удостоверение ASP.NET Core.

> [!TIP]
> Динамически Загрузите конфигурацию IdentityServer 4. IdentityServer 4 API-интерфейсы позволяют настроить сервер из списка в памяти объектов конфигурации. В образце приложения eShopOnContainers этих коллекций в памяти жестко запрограммированы в приложение. Тем не менее в рабочих сценариях их можно загрузить динамически из файла конфигурации или из базы данных.

Сведения о настройке identityserver должно использовать удостоверения ASP.NET Core, см. в разделе [с помощью ASP.NET Core Identity](https://identityserver4.readthedocs.io/en/latest/quickstarts/8_aspnet_identity.html) в документации по IdentityServer.

#### <a name="configuring-api-resources"></a>Настройка ресурсов API

При настройке ресурсов API `AddInMemoryApiResources` ожидает, что метод `IEnumerable<ApiResource>` коллекции. В следующем коде показано в примере `GetApis` метод, который содержит эту коллекцию на eShopOnContainers обращение к приложению:

```csharp
public static IEnumerable<ApiResource> GetApis()  
{  
    return new List<ApiResource>  
    {  
        new ApiResource("orders", "Orders Service"),  
        new ApiResource("basket", "Basket Service")  
    };  
}
```

Этот метод указывает, что сервер следует защитить, заказов и корзины API-интерфейсы. Таким образом, IdentityServer управляемый доступ маркеры будут необходимы при выполнении вызовов к этим API. Дополнительные сведения о `ApiResource` введите, см. в разделе [ресурса API](https://identityserver4.readthedocs.io/en/latest/reference/api_resource.html) в документации по IdentityServer 4.

#### <a name="configuring-identity-resources"></a>Настройка идентификаторов ресурсов

При настройке ресурсов удостоверений `AddInMemoryIdentityResources` ожидает, что метод `IEnumerable<IdentityResource>` коллекции. Ресурсы удостоверений — это данные, например идентификатор пользователя, имя или адрес электронной почты. Каждый ресурс удостоверений имеет уникальное имя и произвольных утверждений можно назначить типы, которые будут включены в маркер идентификации для пользователя. В следующем коде показано в примере `GetResources` метод, который содержит эту коллекцию на eShopOnContainers обращение к приложению:

```csharp
public static IEnumerable<IdentityResource> GetResources()  
{  
    return new List<IdentityResource>  
    {  
        new IdentityResources.OpenId(),  
        new IdentityResources.Profile()  
    };  
}
```

Спецификации OpenID Connect описывает некоторые [ресурсы стандартные удостоверения](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims). Минимальным требованием является то, что поддержка предоставляется для выдачи уникальный идентификатор для пользователей. Это достигается путем предоставления `IdentityResources.OpenId` ресурс идентификаторов.

> [!NOTE]
> `IdentityResources` Класс поддерживает все области, определенные в спецификации OpenID Connect (openid, электронной почты, профиль, телефона и адрес).

IdentityServer также поддерживает определение пользовательских удостоверений ресурсов. Дополнительные сведения см. в разделе [определение пользовательских удостоверений ресурсов](http://docs.identityserver.io/en/latest/topics/resources.html#defining-custom-identity-resources) в документации по IdentityServer. Дополнительные сведения о `IdentityResource` введите, см. в разделе [ресурс идентификаторов](https://identityserver4.readthedocs.io/en/latest/reference/identity_resource.html) в документации по IdentityServer 4.

#### <a name="configuring-clients"></a>Настройка клиентов

Клиенты являются приложениями, которые может запрашивать маркеры из IdentityServer. Как правило следующие параметры должны быть определены для каждого клиента как минимум:

-   Идентификатор уникального клиента.
-   Разрешенные взаимодействия со службой маркеров (известный как тип предоставления).
-   Расположение, где отправляются токены идентификации и доступа (известный как URI перенаправления).
-   Список ресурсов, которые клиент может получить доступ к (известный как области).

При настройке клиентов, `AddInMemoryClients` ожидает, что метод `IEnumerable<Client>` коллекции. В следующем примере кода показана конфигурация для мобильного приложения eShopOnContainers в `GetClients` метод, который содержит эту коллекцию на eShopOnContainers обращение к приложению:

```csharp
public static IEnumerable<Client> GetClients(Dictionary<string,string> clientsUrl)
{
    return new List<Client>
    {
        ...
        new Client
        {
            ClientId = "xamarin",
            ClientName = "eShop Xamarin OpenId Client",
            AllowedGrantTypes = GrantTypes.Hybrid,
            ClientSecrets =
            {
                new Secret("secret".Sha256())
            },
            RedirectUris = { clientsUrl["Xamarin"] },
            RequireConsent = false,
            RequirePkce = true,
            PostLogoutRedirectUris = { $"{clientsUrl["Xamarin"]}/Account/Redirecting" },
            AllowedCorsOrigins = { "http://eshopxamarin" },
            AllowedScopes = new List<string>
            {
                IdentityServerConstants.StandardScopes.OpenId,
                IdentityServerConstants.StandardScopes.Profile,
                IdentityServerConstants.StandardScopes.OfflineAccess,
                "orders",
                "basket"
            },
            AllowOfflineAccess = true,
            AllowAccessTokensViaBrowser = true
        },
        ...
    };
}
```

Эта конфигурация указывает данные для следующих свойств:

-   `ClientId`: Уникальный идентификатор для клиента.
-   `ClientName`: Отображаемое имя клиента, который используется для ведения журнала и на экране согласия.
-   `AllowedGrantTypes`: Указывает, как клиент хочет взаимодействие с identityserver должно. Дополнительные сведения см. в разделе [Настройка поток проверки подлинности](#configuring_the_authentication_flow).
-   `ClientSecrets`: Задает учетные данные секрета клиента, которые используются при запросе маркеров от конечной точки маркеров.
-   `RedirectUris`: Указывает допустимый URI, к которым для возврата маркеров или коды авторизации.
-   `RequireConsent`: Указывает, требуется ли экран согласия.
-   `RequirePkce`: Признак того, клиенты, использующие код авторизации отправки ключа проверки.
-   `PostLogoutRedirectUris`: Указывает URI, чтобы выполнить перенаправление после выхода.
-   `AllowedCorsOrigins`: Указывает источник, клиента, таким образом, сервер может разрешить вызовы независимо от источника от начала координат.
-   `AllowedScopes`: Указывает ресурсы, к которым клиент имеет доступ. По умолчанию клиент не имеет доступа к любым ресурсам.
-   `AllowOfflineAccess`: Указывает, может ли клиент запрашивать маркеры обновления.

<a name="configuring_the_authentication_flow" />

#### <a name="configuring-the-authentication-flow"></a>Настройка потока проверки подлинности

Поток проверки подлинности между клиентом и IdentityServer можно настроить, указав типов предоставления разрешения в `Client.AllowedGrantTypes` свойство. Спецификации OpenID Connect и OAuth 2.0 определить количество потоков аутентификации, включая:

-   Неявные. Этот поток оптимизирован для приложений на основе браузера и должен использоваться для пользователя только для проверки подлинности, или запросы маркеров проверки подлинности и доступа. Все маркеры при передаче через браузер и поэтому дополнительные возможности, например маркеры обновления не разрешены.
-   Код авторизации. Этот поток обеспечивает возможность получать токены от через обратный канал, в отличие от внешнего канала браузера, то же время также поддерживает проверку подлинности клиента.
-   Гибридный. Этот поток представляет собой сочетание неявный и типы предоставления кода авторизации. Маркер идентификации передается по каналу браузера и содержит ответ со знаком протокола, а также другие артефакты, такие как код авторизации. После успешной проверки ответа обратного канала должен использоваться для получения доступа и токена обновления.

> [!TIP]
> Используйте гибридный поток проверки подлинности. Поток проверки подлинности гибридного снижает количество атак, применимые к каналу браузера и является рекомендуемый рабочий процесс для собственных приложений, которые нужно получить маркеры доступа (и возможно для маркеров обновления).

Дополнительные сведения о потоках проверки подлинности см. в разделе [типы предоставления](https://identityserver4.readthedocs.io/en/latest/topics/grant_types.html) в документации по IdentityServer 4.

### <a name="performing-authentication"></a>Выполнение проверки подлинности

Для IdentityServer для выдачи маркеров от имени пользователя пользователь должен войти в к IdentityServer. Тем не менее сервер не предоставляет пользовательский интерфейс или базы данных для проверки подлинности. Таким образом в образце приложения eShopOnContainers, для этой цели используется удостоверение ASP.NET Core.

Мобильное приложение eShopOnContainers использует для проверки подлинности с identityserver должно гибридный поток проверки подлинности, как показано на рисунке 9-2.

![](authentication-and-authorization-images/sign-in.png "Общее представление о процессе входа в систему")

**Рис. 9-2.** Общее представление о процессе входа в систему

Сделан запрос входа в систему на `<base endpoint>:5105/connect/authorize`. После успешной проверки подлинности сервер возвращает ответ проверки подлинности, содержащий код авторизации и токен удостоверения. Код авторизации затем отправляется `<base endpoint>:5105/connect/token`, который отвечает с доступом, удостоверения и маркеры обновления.

EShopOnContainers мобильное приложение знаки готовой IdentityServer, отправив запрос на `<base endpoint>:5105/connect/endsession`, с дополнительными параметрами. После выхода, сервер отвечает, отправляя URI перенаправления после выхода мобильное приложение. Этот процесс показан на рис. 9-3.

![](authentication-and-authorization-images/sign-out.png "Общее представление о процессе выхода")

**Рис. 9-3.** Общее представление о процессе выхода

В мобильном приложении eShopOnContainers, осуществляется взаимодействие с identityserver должно `IdentityService` класса, который реализует `IIdentityService` интерфейс. Этот интерфейс определяет, что реализующий класс должен предоставить `CreateAuthorizationRequest`, `CreateLogoutRequest`, и `GetTokenAsync` методы.

#### <a name="signing-in"></a>Вход в систему

Когда пользователь касается **входа** кнопку `LoginView`, `SignInCommand` в `LoginViewModel` выполняется класс, который в свою очередь выполняет `SignInAsync` метод. Этот метод показан в следующем примере кода:

```csharp
private async Task SignInAsync()  
{  
    ...  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    IsLogin = true;  
    ...  
}
```

Этот метод вызывает `CreateAuthorizationRequest` метод в `IdentityService` класс, который показан в следующем примере кода:

```csharp
public string CreateAuthorizationRequest()
{
    // Create URI to authorization endpoint
    var authorizeRequest = new AuthorizeRequest(GlobalSetting.Instance.IdentityEndpoint);

    // Dictionary with values for the authorize request
    var dic = new Dictionary<string, string>();
    dic.Add("client_id", GlobalSetting.Instance.ClientId);
    dic.Add("client_secret", GlobalSetting.Instance.ClientSecret); 
    dic.Add("response_type", "code id_token");
    dic.Add("scope", "openid profile basket orders locations marketing offline_access");
    dic.Add("redirect_uri", GlobalSetting.Instance.IdentityCallback);
    dic.Add("nonce", Guid.NewGuid().ToString("N"));
    dic.Add("code_challenge", CreateCodeChallenge());
    dic.Add("code_challenge_method", "S256");

    // Add CSRF token to protect against cross-site request forgery attacks.
    var currentCSRFToken = Guid.NewGuid().ToString("N");
    dic.Add("state", currentCSRFToken);

    var authorizeUri = authorizeRequest.Create(dic); 
    return authorizeUri;
}

```

Этот метод создает URI для элемента IdentityServer [конечная точка авторизации](https://identityserver4.readthedocs.io/en/latest/endpoints/authorize.html), с необходимыми аргументами. Конечная точка авторизации находится в `/connect/authorize` на порте 5105 базовый конечной точки в виде параметра пользователя. Дополнительные сведения о пользовательских параметрах см. в разделе [управления конфигурацией](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Уязвимость в мобильном приложении eShopOnContainers уменьшается путем реализации ключа проверки для обмена кодом (PKCE) расширения OAuth. PKCE защищает код авторизации используется, если оно перехватывается. Это достигается путем создания секрета verifier, хэш которого передается в запросе на авторизацию, клиент и представленное нехешированной при активации код авторизации. Дополнительные сведения о PKCE, см. в разделе [ключ подтверждения для кода Exchange с общедоступных клиентов OAuth](https://tools.ietf.org/html/rfc7636) веб-сайте Internet Engineering Task Force.

Полученный URI хранится в `LoginUrl` свойство `LoginViewModel` класса. Когда `IsLogin` свойство становится `true`, [ `WebView` ](xref:Xamarin.Forms.WebView) в `LoginView` становится видимым. `WebView` Осуществляет привязку данных его [ `Source` ](xref:Xamarin.Forms.WebView.Source) свойства `LoginUrl` свойство `LoginViewModel` класса и поэтому IdentityServer отправляет запрос входа в систему при `LoginUrl` свойство имеет значение В IdentityServer конечной точки авторизации. Когда сервер получает этот запрос, и пользователь не прошел проверку подлинности, `WebView` будете перенаправлены на страницу настроенного входа, как показано на рисунке 9-4.

![](authentication-and-authorization-images/login.png "Страница входа, отображаемого элементом веб-представления")

**Рис. 9-4.** Страница входа, отображаемого элементом веб-представления

После завершения входа [ `WebView` ](xref:Xamarin.Forms.WebView) будут перенаправляться в возвращаемое URI. Это `WebView` навигации приведет к `NavigateAsync` метод в `LoginViewModel` класса для выполнения, как показано в следующем примере кода:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    var authResponse = new AuthorizeResponse(url);  
    if (!string.IsNullOrWhiteSpace(authResponse.Code))  
    {  
        var userToken = await _identityService.GetTokenAsync(authResponse.Code);  
        string accessToken = userToken.AccessToken;  

        if (!string.IsNullOrWhiteSpace(accessToken))  
        {  
            Settings.AuthAccessToken = accessToken;  
            Settings.AuthIdToken = authResponse.IdentityToken;  

            await NavigationService.NavigateToAsync<MainViewModel>();  
            await NavigationService.RemoveLastFromBackStackAsync();  
        }  
    }  
    ...  
}
```

Этот метод выполняет синтаксический анализ, содержащийся в URI, возвращаемый ответ проверки подлинности, и при условии, что присутствует код авторизации, оно отправляет запрос к IdentityServer [конечная точка маркера](https://identityserver4.readthedocs.io/en/latest/endpoints/token.html), передавая код авторизации, Средство проверки PKCE для секрета и другие необходимые параметры. Конечная точка маркера находится в `/connect/token` на порте 5105 базовый конечной точки в виде параметра пользователя. Дополнительные сведения о пользовательских параметрах см. в разделе [управления конфигурацией](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!TIP]
> Проверка возвращаемого значения идентификаторы URI. Несмотря на то, что в мобильном приложении eShopOnContainers не проверяет возвращаемое URI, рекомендуется проверить, что возвращаемое URI ссылается на известном расположении, для предотвращения атак открытого перенаправления.

Если конечная точка маркера получает код авторизации и проверки секретный PKCE, он отвечает маркер доступа, маркер идентификации и маркер обновления. Маркер доступа (который предоставляет доступ к ресурсам API) и маркер затем сохраняются как параметры приложения, и выполняется переход по страницам. Таким образом, общий эффект в мобильном приложении eShopOnContainers это: при условии, что пользователи могут успешно пройти проверку подлинности с identityserver должно, переходе к `MainView` страница, которая является [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) отображающий `CatalogView` как его выбранной вкладки.

Сведения о навигации по страницам, см. в разделе [навигации](~/xamarin-forms/enterprise-application-patterns/navigation.md). Сведения о том, как [ `WebView` ](xref:Xamarin.Forms.WebView) навигации вызывает метод модели представления для выполнения, см. в разделе [вызова навигации с помощью поведений](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Сведения о параметрах приложения см. в разделе [управления конфигурацией](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> EShopOnContainers также позволяет макетов вход, если приложение настроено для использования макеты служб в `SettingsView`. В этом режиме приложения не взаимодействуют с IdentityServer, вместо этого, позволяя пользователю вход с использованием учетных данных.

#### <a name="signing-out"></a>Подписывание развертывания

Когда пользователь касается **Выход** кнопку `ProfileView`, `LogoutCommand` в `ProfileViewModel` выполняется класс, который в свою очередь выполняет `LogoutAsync` метод. Этот метод выполняет навигацию по страницам для `LoginView` страницы, передав `LogoutParameter` экземпляр значение `true` как параметр. Дополнительные сведения о передаче параметров во время навигации по страницам см. в разделе [передачи параметров во время навигации](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

При создании представления и осуществлен переход, `InitializeAsync` связанное представление модели представления метода, который затем выполняет `Logout` метод `LoginViewModel` класс, который показан в следующем примере кода:

```csharp
private void Logout()  
{  
    var authIdToken = Settings.AuthIdToken;  
    var logoutRequest = _identityService.CreateLogoutRequest(authIdToken);  

    if (!string.IsNullOrEmpty(logoutRequest))  
    {  
        // Logout  
        LoginUrl = logoutRequest;  
    }  
    ...  
}
```

Этот метод вызывает `CreateLogoutRequest` метод в `IdentityService` класса, передавая маркер идентификации, полученные из параметров приложения, как параметр. Дополнительные сведения о параметрах приложения см. в разделе [управления конфигурацией](~/xamarin-forms/enterprise-application-patterns/configuration-management.md). Метод `CreateLogoutRequest` показан в следующем примере кода:

```csharp
public string CreateLogoutRequest(string token)  
{  
    ...  
    return string.Format("{0}?id_token_hint={1}&post_logout_redirect_uri={2}",   
        GlobalSetting.Instance.LogoutEndpoint,  
        token,  
        GlobalSetting.Instance.LogoutCallback);  
}
```

Этот метод создает URI для элемента IdentityServer [завершить сеанс конечной точки](https://identityserver4.readthedocs.io/en/latest/endpoints/endsession.html#refendsession), с необходимыми аргументами. Конечная точка окончания сеанса находится в `/connect/endsession` на порте 5105 базовый конечной точки в виде параметра пользователя. Дополнительные сведения о пользовательских параметрах см. в разделе [управления конфигурацией](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

Полученный URI хранится в `LoginUrl` свойство `LoginViewModel` класса. Хотя `IsLogin` свойство `true`, [ `WebView` ](xref:Xamarin.Forms.WebView) в `LoginView` является видимым. `WebView` Осуществляет привязку данных его [ `Source` ](xref:Xamarin.Forms.WebView.Source) свойства `LoginUrl` свойство `LoginViewModel` класса и поэтому запрос выхода для IdentityServer при `LoginUrl` свойство имеет значение В IdentityServer конечной сеанса. Когда сервер получает этот запрос, при условии, что пользователь, выполнивший вход, происходит выход. Проверка подлинности отслеживается с файлом cookie, управляемых по промежуточного слоя проверки подлинности файла cookie из ASP.NET Core. Таким образом выйдя из IdentityServer удаляет файл cookie проверки подлинности и отправляет перенаправления после выхода, URI обратно клиенту.

В мобильном приложении [ `WebView` ](xref:Xamarin.Forms.WebView) будут перенаправляться в URI перенаправления после выхода. Это `WebView` навигации приведет к `NavigateAsync` метод в `LoginViewModel` класса для выполнения, как показано в следующем примере кода:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    Settings.AuthAccessToken = string.Empty;  
    Settings.AuthIdToken = string.Empty;  
    IsLogin = false;  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    ...  
}
```

Этот метод удаляет маркер идентификации и маркер доступа из параметров приложения и задает `IsLogin` свойства `false`, чего [ `WebView` ](xref:Xamarin.Forms.WebView) на `LoginView` страница станет невидимой . Наконец `LoginUrl` свойству IdentityServer URI из [конечная точка авторизации](https://identityserver4.readthedocs.io/en/latest/endpoints/authorize.html), с необходимыми аргументами, в процессе подготовки в следующий раз, когда пользователь инициирует вход в систему.

Сведения о навигации по страницам, см. в разделе [навигации](~/xamarin-forms/enterprise-application-patterns/navigation.md). Сведения о том, как [ `WebView` ](xref:Xamarin.Forms.WebView) навигации вызывает метод модели представления для выполнения, см. в разделе [вызова навигации с помощью поведений](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Сведения о параметрах приложения см. в разделе [управления конфигурацией](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> EShopOnContainers также позволяет макетирование выхода при настройке приложения для использования в SettingsView макеты служб. В этом режиме приложение не взаимодействует с identityserver должно и вместо очищает все хранимые токены из параметров приложения.

<a name="authorization" />

## <a name="authorization"></a>Авторизация

После проверки подлинности веб-ASP.NET Core, часто требуется API-интерфейсы для разрешения доступа, что позволяет службе предоставить интерфейсы API доступны некоторые пользователям, прошедшим проверку, но не всем.

Ограничение доступа к маршруту ASP.NET Core MVC может осуществляться путем применения атрибута Authorize к контроллеру или действие, которое ограничивает доступ к контроллеру или действие, прошедшим проверку подлинности, как показано в следующем примере кода:

```csharp
[Authorize]  
public class BasketController : Controller  
{  
    ...  
}
```

Если неавторизованный пользователь пытается получить доступ к контроллеру или действию, отмеченный атрибутом `Authorize` атрибут, платформа MVC возвращает код состояния 401 (неавторизованный) HTTP.

> [!NOTE]
> Параметры могут быть заданы на `Authorize` атрибута для ограничения API для определенных пользователей. Дополнительные сведения см. в разделе [авторизации](/aspnet/core/security/authorization/introduction/).

IdentityServer можно интегрировать в рабочий процесс авторизации, чтобы маркеры доступа, он предоставляет контроль авторизации. Этот подход показан на рисунке 9-5.

![](authentication-and-authorization-images/authorization.png "Авторизация с использованием маркера доступа")

**Рис. 9-5.** Авторизация с использованием маркера доступа

В мобильном приложении eShopOnContainers взаимодействует с микрослужбы по идентификации и запрашивает маркер доступа как часть процесса проверки подлинности. Маркер доступа, перенаправляется API, предоставляемых микрослужб упорядочения и корзины как часть запросов на доступ. Маркеры доступа содержат информацию о клиенте и пользователь. API-интерфейсы, затем использовать эту информацию для авторизации доступа к их данным. Сведения о том, как настроить сервер для защиты API-интерфейсов, см. в разделе [Настройка ресурсов API](#configuring-api-resources).

### <a name="configuring-identityserver-to-perform-authorization"></a>Настройка IdentityServer выполнение авторизации

Выполнение авторизации с identityserver должно, необходимо добавить его авторизации по промежуточного слоя конвейер запросов HTTP веб-приложения. По промежуточного слоя добавляется в `ConfigureAuth` метод веб-приложения `Startup` класс, который вызывается из `Configure` метод и демонстрируется в следующем примере кода из эталонного приложения eShopOnContainers:

```csharp
protected virtual void ConfigureAuth(IApplicationBuilder app)  
{  
    var identityUrl = Configuration.GetValue<string>("IdentityUrl");  
    app.UseIdentityServerAuthentication(new IdentityServerAuthenticationOptions  
    {  
        Authority = identityUrl.ToString(),  
        ScopeName = "basket",  
        RequireHttpsMetadata = false  
    });  
} 
```

Этот метод гарантирует, что API может осуществляться только с действительный маркер доступа. По промежуточного слоя проверяет входящий токен, чтобы убедиться, что оно отправлено с надежным издателем и проверяет, что маркер является допустимым для использования с API, которая его получает. Таким образом перейдя к контроллеру упорядочение или корзины вернет (несанкционированных) код состояния HTTP 401, указывающее, что маркер доступа является обязательным.

> [!NOTE]
> По промежуточного слоя сервер авторизации необходимо добавить в конвейер запросов HTTP веб-приложения перед добавлением MVC с `app.UseMvc()` или `app.UseMvcWithDefaultRoute()`.

### <a name="making-access-requests-to-apis"></a>Создание запросов на доступ к интерфейсам API

При составлении запросов к микрослужб упорядочения и корзины, доступ маркер, полученный из IdentityServer во время процесса проверки подлинности, должны быть включены в запрос, как показано в следующем примере кода:

```csharp
var authToken = Settings.AuthAccessToken;  
Order = await _ordersService.GetOrderAsync(Convert.ToInt32(order.OrderNumber), authToken);
```

Маркер доступа хранится в виде параметра приложения и получить из хранилища с платформой и включены в вызове `GetOrderAsync` метод в `OrderService` класса.

Аналогичным образом маркер доступа должен быть включен при отправке данных IdentityServer защите API, как показано в следующем примере кода:

```csharp
var authToken = Settings.AuthAccessToken;  
await _basketService.UpdateBasketAsync(new CustomerBasket  
{  
    BuyerId = userInfo.UserId,   
    Items = BasketItems.ToList()  
}, authToken);
```

Маркер доступа извлекается из хранилища, зависящее от платформы и включены в вызове `UpdateBasketAsync` метод в `BasketService` класса.

`RequestProvider` Класс, в мобильном приложении eShopOnContainers, использует `HttpClient` класса для выполнения запросов к API-интерфейсов RESTful, предоставляемые в образце приложения eShopOnContainers. При отправке запросов в заказ и корзины API-интерфейсы, которые требуют наличия авторизации, действительный маркер доступа должен быть включен в запрос. Это достигается путем добавления маркера доступа к заголовкам `HttpClient` экземпляра, как показано в следующем примере кода:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
```

`DefaultRequestHeaders` Свойство `HttpClient` класс предоставляет заголовки, которые отправляются с каждым запросом, и маркер доступа добавляется `Authorization` префиксом со строкой заголовка `Bearer`. Отправки запроса к API-интерфейсов RESTful, значение `Authorization` извлекается и проверять на соответствие отправляются от доверенного издателя, что позволяет определить, имеет ли пользователь разрешение на вызов API, получает его заголовок.

Дополнительные сведения о том, как мобильное приложение eShopOnContainers выполняет веб-запросы, см. в разделе [доступ к удаленным данным](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md).

## <a name="summary"></a>Сводка

Существует множество подходов к интеграции проверки подлинности и авторизации в приложение Xamarin.Forms, которое взаимодействует с веб-приложение ASP.NET MVC. В мобильном приложении eShopOnContainers выполняет проверку подлинности и авторизации с помощью микрослужбы контейнерных удостоверений, которая использует IdentityServer 4. IdentityServer — это платформа с открытым исходным OpenID Connect и OAuth 2.0 для ASP.NET Core, которое интегрируется с удостоверением ASP.NET Core для выполнения проверки подлинности токена носителя.

Мобильное приложение запрашивает у IdentityServer, маркеры безопасности, для проверки подлинности пользователя или для доступа к ресурсу. При доступе к ресурсу, маркер доступа должен быть включен в запросе к API, требующие авторизации. По промежуточного слоя сервер проверяет входящие маркеры доступа, чтобы убедиться, что они передаются от доверенного издателя, и что они являются допустимыми для использования с API, который их получает.


## <a name="related-links"></a>Связанные ссылки

- [Скачайте электронную книгу (2 МБ в формате PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (пример)](https://github.com/dotnet-architecture/eShopOnContainers)
