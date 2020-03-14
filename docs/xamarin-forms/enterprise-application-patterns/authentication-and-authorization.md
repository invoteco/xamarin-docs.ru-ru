---
title: Проверка подлинности и авторизация
description: В этой главе объясняется, как мобильное приложение eShopOnContainers выполняет проверку подлинности и авторизацию для контейнерных микрослужб.
ms.prod: xamarin
ms.assetid: e3f27b4c-f7f5-4839-a48c-30bcb919c59e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2017
ms.openlocfilehash: 528ccd66cc013f83752d93251cb9714115b29819
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306329"
---
# <a name="authentication-and-authorization"></a>Проверка подлинности и авторизация

Проверка подлинности — это процесс получения учетных данных для идентификации, таких как имя и пароль пользователя, и проверки этих учетных данных в центре сертификации. Если учетные данные действительны, сущность, которая отправила учетные данные, считается аутентифицированным удостоверением. После проверки подлинности удостоверения процесс авторизации определяет, имеет ли это удостоверение доступ к данному ресурсу.

Существует множество подходов к интеграции проверки подлинности и авторизации в приложение Xamarin. Forms, которое взаимодействует с веб-приложением ASP.NET MVC, включая использование удостоверения ASP.NET Core, внешних поставщиков проверки подлинности, таких как Microsoft, Google, Facebook, Twitter и по промежуточного слоя для проверки подлинности. Мобильное приложение eShopOnContainers выполняет проверку подлинности и авторизацию с помощью микрослужбы контейнерных удостоверений, которая использует IdentityServer 4. Мобильное приложение запрашивает маркеры безопасности от IdentityServer для проверки подлинности пользователя или доступа к ресурсу. Чтобы IdentityServer выдавал маркеры от имени пользователя, пользователь должен войти в IdentityServer. Однако IdentityServer не предоставляет пользовательский интерфейс или базу данных для проверки подлинности. Поэтому в приложении-образце eShopOnContainers для этой цели используется удостоверение ASP.NET Core.

## <a name="authentication"></a>Аутентификация

Проверка подлинности необходима, когда приложению необходимо известно удостоверение текущего пользователя. Основным механизмом для идентификации пользователей ASP.NET Core является система членства в ASP.NET Core, которая хранит сведения о пользователе в хранилище данных, настроенном разработчиком. Как правило, это хранилище данных будет EntityFramework хранилищем, хотя пользовательские хранилища и пакеты сторонних производителей можно использовать для хранения сведений об удостоверениях в службе хранилища Azure, Azure Cosmos DB или в других расположениях.

В сценариях проверки подлинности, использующих локальное хранилище данных пользователя и сохраняющей сведения об удостоверениях между запросами через файлы cookie (как обычно в веб-приложениях ASP.NET MVC), ASP.NET Core удостоверение является подходящим решением. Однако файлы cookie не всегда являются естественным способом сохранения и передачи данных. Например, веб-приложение ASP.NET Core, которое предоставляет конечные точки RESTFUL, доступ к которым осуществляется из мобильного приложения, обычно должно использовать проверку подлинности маркера носителя, так как файлы cookie не могут использоваться в этом сценарии. Однако токены носителя можно легко получить и добавить в заголовок авторизации веб-запросов, сделанных из мобильного приложения.

### <a name="issuing-bearer-tokens-using-identityserver-4"></a>Выдача токенов носителя с помощью IdentityServer 4

[IdentityServer 4](https://github.com/IdentityServer/IdentityServer4) — это инфраструктура с открытым исходным кодом OpenID Connect Connect и OAuth 2,0 для ASP.NET Core, которую можно использовать для многих сценариев проверки подлинности и авторизации, включая выдачу маркеров безопасности для локальных ASP.NET Core пользователей удостоверений.

> [!NOTE]
> OpenID Connect Connect и OAuth 2,0 очень похожи, и у них есть разные обязанности.

OpenID Connect Connect — это уровень проверки подлинности на основе протокола OAuth 2,0. OAuth 2 — это протокол, позволяющий приложениям запрашивать маркеры доступа из службы маркеров безопасности и использовать их для взаимодействия с API. Такое делегирование снижает сложность как клиентских приложений, так и интерфейсов API, так как проверка подлинности и авторизация могут быть централизованными

Сочетание OpenID Connect Connect и OAuth 2,0 сочетает две фундаментальные проблемы безопасности, связанные с проверкой подлинности и доступом к API, а IdentityServer 4 — реализацию этих протоколов.

В приложениях, использующих прямое взаимодействие клиента с микрослужбой, например Справочное приложение eShopOnContainers, выделенная микрослужба проверки подлинности, действующая как служба маркеров безопасности (STS), может использоваться для проверки подлинности пользователей, как показано на рисунке. 9-1. Дополнительные сведения о прямой связи клиента с микрослужбой см. в разделе [взаимодействие между клиентом и микрослужбами](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md#communication_between_client_and_microservices).

![](authentication-and-authorization-images/authentication.png "Authentication by a dedicated authentication microservice")

**Рис. 9-1.** Проверка подлинности с помощью выделенной микрослужбы проверки подлинности

Мобильное приложение eShopOnContainers взаимодействует с микрослужбой Identity, которая использует IdentityServer 4 для выполнения проверки подлинности и управления доступом для интерфейсов API. Таким образом, мобильное приложение запрашивает маркеры от IdentityServer для проверки подлинности пользователя или доступа к ресурсу:

- Проверка подлинности пользователей с помощью IdentityServer достигается мобильным приложением, запрашивающим маркер *идентификации* , который представляет результат процесса проверки подлинности. Как минимум, он содержит идентификатор пользователя, а также сведения о том, как и когда пользователь прошел проверку подлинности. Он также может содержать дополнительные данные удостоверений.
- Доступ к ресурсу с IdentityServer достигается мобильным приложением, запрашивающим маркер *доступа* , который обеспечивает доступ к ресурсу API. Клиенты запрашивают маркеры доступа и пересылают их в API. Маркеры доступа содержат сведения о клиенте и пользователе (если он есть). Затем интерфейсы API используют эти сведения для авторизации доступа к своим данным.

> [!NOTE]
> Клиент должен быть зарегистрирован в IdentityServer, прежде чем он сможет запрашивать токены.

### <a name="adding-identityserver-to-a-web-application"></a>Добавление IdentityServer в веб-приложение

Чтобы ASP.NET Core веб-приложение использовало IdentityServer 4, его необходимо добавить в решение Visual Studio веб-приложения. Дополнительные сведения см. в разделе [Обзор](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) в документации по IdentityServer.

После включения IdentityServer в решение Visual Studio веб-приложения оно должно быть добавлено к конвейеру обработки HTTP-запросов в Интернете, чтобы он мог обслуживать запросы к конечным точкам OpenID Connect Connect и OAuth 2,0. Это достигается методом `Configure` в классе `Startup` веб-приложения, как показано в следующем примере кода:

```csharp
public void Configure(  
    IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)  
{  
    ...  
    app.UseIdentity();  
    ...  
}
```

Важность порядка в конвейере обработки HTTP-запросов. Поэтому IdentityServer должен быть добавлен в конвейер до того, как платформа пользовательского интерфейса, реализующая экран входа в систему.

### <a name="configuring-identityserver"></a>Настройка IdentityServer

IdentityServer должен быть настроен в методе `ConfigureServices` в классе `Startup` веб-приложения путем вызова метода `services.AddIdentityServer`, как показано в следующем примере кода из приложения-образца eShopOnContainers:

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

После вызова метода `services.AddIdentityServer` вызываются дополнительные интерфейсы API Fluent для настройки следующих параметров:

- Учетные данные, используемые для подписывания.
- Ресурсы API и удостоверений, к которым пользователи могут запрашивать доступ.
- Клиенты, которые будут подключаться к маркерам запросов.
- Удостоверение ASP.NET Core.

> [!TIP]
> Динамически загружать конфигурацию IdentityServer 4. Интерфейсы API IdentityServer 4 позволяют настраивать IdentityServer из списка объектов конфигурации в памяти. В приложении-образце eShopOnContainers эти коллекции в памяти жестко запрограммированы в приложении. Однако в рабочих сценариях их можно загрузить динамически из файла конфигурации или из базы данных.

Сведения о настройке IdentityServer для использования удостоверения ASP.NET Core см. в разделе [использование ASP.NET Core Identity](https://identityserver4.readthedocs.io/en/latest/quickstarts/6_aspnet_identity.html) в документации по IdentityServer.

#### <a name="configuring-api-resources"></a>Настройка ресурсов API

При настройке ресурсов API метод `AddInMemoryApiResources` принимает `IEnumerable<ApiResource>` коллекцию. В следующем примере кода показан метод `GetApis`, который предоставляет эту коллекцию в эталонном приложении eShopOnContainers:

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

Этот метод указывает, что IdentityServer должен защищать API заказов и корзины. Поэтому при вызове этих API-интерфейсов будут требоваться IdentityServer управляемые маркеры доступа. Дополнительные сведения о типе `ApiResource` см. в разделе [ресурс API](https://identityserver4.readthedocs.io/en/latest/reference/api_resource.html) в документации по IdentityServer 4.

#### <a name="configuring-identity-resources"></a>Настройка ресурсов удостоверений

При настройке ресурсов удостоверений метод `AddInMemoryIdentityResources` принимает `IEnumerable<IdentityResource>` коллекцию. Ресурсы удостоверений — это такие данные, как идентификатор пользователя, имя или адрес электронной почты. Каждый ресурс удостоверения имеет уникальное имя, и к нему могут быть назначены произвольные типы утверждений, которые затем будут включаться в маркер идентификации для пользователя. В следующем примере кода показан метод `GetResources`, который предоставляет эту коллекцию в эталонном приложении eShopOnContainers:

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

В спецификации OpenID Connect Connect указаны некоторые [стандартные ресурсы удостоверений](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims). Минимальное требование заключается в том, что предоставляется поддержка для создания уникального идентификатора для пользователей. Это достигается за счет предоставления ресурса удостоверения `IdentityResources.OpenId`.

> [!NOTE]
> Класс `IdentityResources` поддерживает все области, определенные в спецификации подключения OpenID Connect (OpenID Connect, адрес электронной почты, профиль, Телефон и адрес).

IdentityServer также поддерживает определение пользовательских ресурсов удостоверений. Дополнительные сведения см. в разделе [Определение пользовательских ресурсов удостоверений](http://docs.identityserver.io/en/latest/topics/resources.html#defining-custom-identity-resources) в документации по IdentityServer. Дополнительные сведения о типе `IdentityResource` см. в разделе [ресурс удостоверений](https://identityserver4.readthedocs.io/en/latest/reference/identity_resource.html) в документации по IdentityServer 4.

#### <a name="configuring-clients"></a>Настройка клиентов

Клиенты — это приложения, которые могут запрашивать токены от IdentityServer. Как правило, следующие параметры должны быть определены для каждого клиента как минимум:

- Уникальный идентификатор клиента.
- Разрешенные взаимодействия со службой маркеров (называемые типом предоставления).
- Расположение, куда отправляются маркеры удостоверений и доступа (называемые URI перенаправления).
- Список ресурсов, к которым разрешен доступ клиенту (называемые областями).

При настройке клиентов метод `AddInMemoryClients` принимает `IEnumerable<Client>` коллекцию. В следующем примере кода показана конфигурация мобильного приложения eShopOnContainers в методе `GetClients`, который предоставляет эту коллекцию в эталонном приложении eShopOnContainers:

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

Эта конфигурация задает данные для следующих свойств:

- `ClientId`— уникальный идентификатор клиента.
- `ClientName`: отображаемое имя клиента, используемое для ведения журнала, и экран согласия.
- `AllowedGrantTypes`: указывает, как клиент хочет взаимодействовать с IdentityServer. Дополнительные сведения см. [в разделе Настройка потока проверки подлинности](#configuring_the_authentication_flow).
- `ClientSecrets`: указывает секретные учетные данные клиента, которые используются при запросе токенов из конечной точки токена.
- `RedirectUris`: Указывает допустимые URI, к которым возвращаются маркеры или коды авторизации.
- `RequireConsent`: указывает, требуется ли экран согласия.
- `RequirePkce`: указывает, должны ли клиенты, использующие код авторизации, отправить ключ подтверждения.
- `PostLogoutRedirectUris`: Указывает разрешенные URI для перенаправления после выхода.
- `AllowedCorsOrigins`: указывает источник клиента, чтобы IdentityServer мог разрешить вызовы между источниками из источника.
- `AllowedScopes`: Указывает ресурсы, к которым у клиента есть доступ. По умолчанию клиент не имеет доступа к ресурсам.
- `AllowOfflineAccess`: указывает, может ли клиент запрашивать маркеры обновления.

<a name="configuring_the_authentication_flow" />

#### <a name="configuring-the-authentication-flow"></a>Настройка потока проверки подлинности

Поток проверки подлинности между клиентом и IdentityServer можно настроить, указав типы предоставления в свойстве `Client.AllowedGrantTypes`. Спецификации OpenID Connect Connect и OAuth 2,0 определяют ряд потоков проверки подлинности, в том числе:

- Полностью. Этот поток оптимизирован для приложений на основе браузера и должен использоваться только для проверки подлинности пользователя или для запросов маркеров доступа и проверки подлинности. Все маркеры передаются через браузер, поэтому такие дополнительные функции, как маркеры обновления, не разрешены.
- Код авторизации. Этот поток предоставляет возможность извлечения токенов в обратном канале, в отличие от интерфейсного канала браузера, а также поддерживает проверку подлинности клиента.
- Гибридный. Этот поток представляет собой сочетание типов предоставления неявных и разрешенных кодов авторизации. Маркер удостоверения передается через канал браузера и содержит ответ на подписанный протокол вместе с другими артефактами, такими как код авторизации. После успешной проверки ответа необходимо использовать обратный канал для получения маркера доступа и обновления.

> [!TIP]
> Используйте гибридный поток проверки подлинности. Гибридный поток проверки подлинности уменьшает количество атак, применяемых к каналу браузера, и является рекомендуемым потоком для собственных приложений, которые хотят получить маркеры доступа (и, возможно, обновить маркеры).

Дополнительные сведения о потоках проверки подлинности см. в разделе [Grant Types](https://identityserver4.readthedocs.io/en/latest/topics/grant_types.html) в документации по IdentityServer 4.

### <a name="performing-authentication"></a>Выполнение проверки подлинности

Чтобы IdentityServer выдавал маркеры от имени пользователя, пользователь должен войти в IdentityServer. Однако IdentityServer не предоставляет пользовательский интерфейс или базу данных для проверки подлинности. Поэтому в приложении-образце eShopOnContainers для этой цели используется удостоверение ASP.NET Core.

Мобильное приложение eShopOnContainers проходит проверку подлинности с помощью IdentityServer с гибридным потоком проверки подлинности, как показано на рис. 9-2.

![](authentication-and-authorization-images/sign-in.png "High-level overview of the sign-in process")

**Рис. 9-2.** Высокоуровневый обзор процесса входа

Запрос на вход выполняется в `<base endpoint>:5105/connect/authorize`. После успешной проверки подлинности IdentityServer возвращает ответ на проверку подлинности, содержащий код авторизации и маркер удостоверения. Затем код авторизации отправляется в `<base endpoint>:5105/connect/token`, который реагирует на маркеры доступа, удостоверений и обновления.

Мобильное приложение eShopOnContainers выходит из IdentityServer, отправляя запрос `<base endpoint>:5105/connect/endsession`с дополнительными параметрами. После выхода из системы IdentityServer отвечает, отправляя в мобильное приложение URI перенаправления обратного выхода. Этот процесс показан на рис. 9-3.

![](authentication-and-authorization-images/sign-out.png "High-level overview of the sign-out process")

**Рис. 9-3.** Высокоуровневый обзор процесса выхода

В мобильном приложении eShopOnContainers взаимодействие с IdentityServer выполняется классом `IdentityService`, который реализует интерфейс `IIdentityService`. Этот интерфейс указывает, что реализующий класс должен предоставлять методы `CreateAuthorizationRequest`, `CreateLogoutRequest`и `GetTokenAsync`.

#### <a name="signing-in"></a>Вход в систему

Когда пользователь нажмет кнопку **входа** на `LoginView`, выполняется `SignInCommand` в классе `LoginViewModel`, который, в свою очередь, выполняет метод `SignInAsync`. Этот метод показан в следующем примере кода:

```csharp
private async Task SignInAsync()  
{  
    ...  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    IsLogin = true;  
    ...  
}
```

Этот метод вызывает метод `CreateAuthorizationRequest` в классе `IdentityService`, который показан в следующем примере кода:

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
    dic.Add("redirect_uri", GlobalSetting.Instance.Callback);
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

Этот метод создает универсальный код ресурса (URI) для [конечной точки авторизации](https://identityserver4.readthedocs.io/en/latest/endpoints/authorize.html)IdentityServer с необходимыми параметрами. Конечная точка авторизации находится на `/connect/authorize` порта 5105 базовой конечной точки, предоставляемой в качестве параметра пользователя. Дополнительные сведения о параметрах пользователей см. в разделе [Управление конфигурацией](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Поверхность атаки мобильного приложения eShopOnContainers уменьшается путем внедрения расширения ключа подтверждения для обмена кодом (PKCE) в OAuth. PKCE защищает код авторизации от использования в случае его перехвата. Это достигается тем, что клиент создает средство проверки секрета, хэш, который передается в запросе авторизации и представляется нехэшированным при активации кода авторизации. Дополнительные сведения о PKCE см. в статье [ключ подтверждения для обмена кодом на общедоступных клиентах OAuth](https://tools.ietf.org/html/rfc7636) на веб-сайте Task Engineering Web site.

Возвращенный URI хранится в свойстве `LoginUrl` класса `LoginViewModel`. Когда свойство `IsLogin` преобразуется в `true`, [`WebView`](xref:Xamarin.Forms.WebView) в `LoginView` становятся видимыми. `WebView` данные привязывают свойство [`Source`](xref:Xamarin.Forms.WebView.Source) к свойству `LoginUrl` класса `LoginViewModel`, и поэтому выполняет запрос на вход в IdentityServer, когда свойство `LoginUrl` имеет значение конечной точки авторизации IdentityServer. Когда IdentityServer получает этот запрос и пользователь не прошел проверку подлинности, `WebView` будет перенаправлен на настроенную страницу входа, которая показана на рис. 9-4.

![](authentication-and-authorization-images/login.png "Login page displayed by the WebView")

**Рис. 9-4.** Страница входа, отображаемая WebView

После завершения входа [`WebView`](xref:Xamarin.Forms.WebView) будет перенаправлен на универсальный код ресурса (URI) возврата. Эта `WebView` навигации приведет к выполнению метода `NavigateAsync` в классе `LoginViewModel`, который показан в следующем примере кода:

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

Этот метод анализирует ответ проверки подлинности, содержащийся в URI возврата, и при условии, что присутствует допустимый код авторизации, он выполняет запрос к [конечной точке маркера](https://identityserver4.readthedocs.io/en/latest/endpoints/token.html)IdentityServer, передает код авторизации, средство проверки секрета PKCE и другие необходимые параметры. Конечная точка маркера находится на `/connect/token` порта 5105 базовой конечной точки, предоставляемой в качестве параметра пользователя. Дополнительные сведения о параметрах пользователей см. в разделе [Управление конфигурацией](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!TIP]
> Проверка возвращенных URI. Несмотря на то, что мобильное приложение eShopOnContainers не проверяет URI возврата, рекомендуется проверить, что возвращаемый URI ссылается на известное расположение, чтобы предотвратить атаки с открытым перенаправлением.

Если конечная точка маркера получает допустимый код авторизации и PKCE Secret Verifier, она реагирует на маркер доступа, маркер идентификации и маркер обновления. Маркер доступа (который разрешает доступ к ресурсам API) и маркер удостоверения сохраняются как параметры приложения, а также выполняется Навигация по страницам. Таким образом, общий результат в мобильном приложении eShopOnContainers: при условии, что пользователи могут успешно пройти проверку подлинности с помощью IdentityServer, они переходят на страницу `MainView`, которая представляет собой [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) , отображающую `CatalogView` в качестве выбранной вкладки.

Дополнительные сведения о навигации по страницам см. в разделе [Навигация](~/xamarin-forms/enterprise-application-patterns/navigation.md). Сведения о том, как [`WebView`ная](xref:Xamarin.Forms.WebView) Навигация вызывает выполнение метода модели представления, см. в разделе [вызов навигации с использованием поведений](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Сведения о параметрах приложения см. в разделе [Управление конфигурацией](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> EShopOnContainers также позволяет использовать фиктивный вход, когда приложение настроено на использование служб макетирования в `SettingsView`. В этом режиме приложение не взаимодействует с IdentityServer, а позволяет пользователю войти в систему, используя любые учетные данные.

#### <a name="signing-out"></a>Выход

Когда пользователь нажмет кнопку **выхода** в `ProfileView`, выполняется `LogoutCommand` в классе `ProfileViewModel`, который, в свою очередь, выполняет метод `LogoutAsync`. Этот метод выполняет навигацию по страницам на `LoginView` странице, передавая `LogoutParameter` экземпляру значение `true` в качестве параметра. Дополнительные сведения о передаче параметров при навигации по страницам см. в разделе [Передача параметров во время навигации](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

Когда представление создается и переходит к, выполняется метод `InitializeAsync` связанной модели представления представления, который затем выполняет метод `Logout` класса `LoginViewModel`, который показан в следующем примере кода:

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

Этот метод вызывает метод `CreateLogoutRequest` в классе `IdentityService`, передавая маркер удостоверения, полученный из параметров приложения, в качестве параметра. Дополнительные сведения о параметрах приложений см. в разделе [Управление конфигурацией](~/xamarin-forms/enterprise-application-patterns/configuration-management.md). Метод `CreateLogoutRequest` показан в следующем примере кода:

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

Этот метод создает универсальный код ресурса (URI) [конечной точки конечного сеанса](https://identityserver4.readthedocs.io/en/latest/endpoints/endsession.html#refendsession)IdentityServer с необходимыми параметрами. Конечная точка конечного сеанса находится в `/connect/endsession` порта 5105 базовой конечной точки, предоставляемой в качестве параметра пользователя. Дополнительные сведения о параметрах пользователей см. в разделе [Управление конфигурацией](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

Возвращенный URI хранится в свойстве `LoginUrl` класса `LoginViewModel`. В то время как свойство `IsLogin` `true`, отображается [`WebView`](xref:Xamarin.Forms.WebView) в `LoginView`. `WebView` данные привязывают свойство [`Source`](xref:Xamarin.Forms.WebView.Source) к свойству `LoginUrl` класса `LoginViewModel`, и поэтому создает запрос на выход в IdentityServer, если свойство `LoginUrl` имеет значение конечной точки конечного сеанса IdentityServer. Когда IdentityServer получает этот запрос при условии, что пользователь вошел в систему, выполняется выход. Проверка подлинности осуществляется с помощью файла cookie, управляемого по промежуточного слоя проверки подлинности файлов cookie, от ASP.NET Core. Таким образом, выход из IdentityServer удаляет файл cookie для проверки подлинности и отправляет клиенту URI перенаправления обратного выхода.

В мобильном приложении [`WebView`](xref:Xamarin.Forms.WebView) будет перенаправляться в URI перенаправления выхода из системы. Эта `WebView` навигации приведет к выполнению метода `NavigateAsync` в классе `LoginViewModel`, который показан в следующем примере кода:

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

Этот метод очищает маркер удостоверения и маркер доступа из параметров приложения и устанавливает для свойства `IsLogin` значение `false`, что приводит к тому, что [`WebView`](xref:Xamarin.Forms.WebView) на странице `LoginView` становится невидимой. Наконец, для свойства `LoginUrl` задается универсальный код ресурса (URI) [конечной точки авторизации](https://identityserver4.readthedocs.io/en/latest/endpoints/authorize.html)IdentityServer с необходимыми параметрами в процессе подготовки к следующему моменту, когда пользователь инициирует вход.

Дополнительные сведения о навигации по страницам см. в разделе [Навигация](~/xamarin-forms/enterprise-application-patterns/navigation.md). Сведения о том, как [`WebView`ная](xref:Xamarin.Forms.WebView) Навигация вызывает выполнение метода модели представления, см. в разделе [вызов навигации с использованием поведений](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Сведения о параметрах приложения см. в разделе [Управление конфигурацией](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> EShopOnContainers также позволяет макетировать выход, когда приложение настроено для использования служб макетирования в Сеттингсвиев. В этом режиме приложение не взаимодействует с IdentityServer, а удаляет все сохраненные токены из параметров приложения.

<a name="authorization" />

## <a name="authorization"></a>Авторизация

После проверки подлинности ASP.NET Core веб-API часто требуется авторизовать доступ, что позволяет службе сделать интерфейсы API доступными для некоторых пользователей, прошедших проверку подлинности, но не для всех.

Ограничение доступа к ASP.NET Coreному маршруту MVC можно достичь, применив атрибут авторизации к контроллеру или действию, который ограничивает доступ к контроллеру или действию для пользователей, прошедших проверку подлинности, как показано в следующем примере кода:

```csharp
[Authorize]  
public class BasketController : Controller  
{  
    ...  
}
```

Если неавторизованный пользователь пытается получить доступ к контроллеру или действию, помеченному атрибутом `Authorize`, платформа MVC возвращает код состояния HTTP 401 (несанкционированный).

> [!NOTE]
> Параметры можно указать в атрибуте `Authorize`, чтобы ограничить API определенными пользователями. Дополнительные сведения см. в статье об [авторизации](/aspnet/core/security/authorization/introduction/).

IdentityServer можно интегрировать в рабочий процесс авторизации, чтобы получить маркеры доступа, обеспечивающие управление авторизацией. Этот подход показан на рис. 9-5.

![](authentication-and-authorization-images/authorization.png "Authorization by access token")

**Рис. 9-5.** Авторизация по маркеру доступа

Мобильное приложение eShopOnContainers взаимодействует с микрослужбой Identity и запрашивает маркер доступа как часть процесса проверки подлинности. Затем маркер доступа перенаправляется в интерфейсы API, предоставляемые микрослужбами упорядочивания и корзины в рамках запросов на доступ. Маркеры доступа содержат сведения о клиенте и пользователе. Затем интерфейсы API используют эти сведения для авторизации доступа к своим данным. Сведения о настройке IdentityServer для защиты API-интерфейсов см. в разделе [Настройка ресурсов API](#configuring-api-resources).

### <a name="configuring-identityserver-to-perform-authorization"></a>Настройка IdentityServer для выполнения авторизации

Для выполнения авторизации с помощью IdentityServer по промежуточного слоя авторизации должно быть добавлено в конвейер запросов HTTP веб-приложения. По промежуточного слоя добавляется в метод `ConfigureAuth` в классе `Startup` веб-приложения, который вызывается из метода `Configure` и демонстрируется в следующем примере кода из приложения-образца eShopOnContainers:

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

Этот метод гарантирует, что доступ к API может осуществляться только с помощью действительного маркера доступа. По промежуточного слоя проверяет входящий токен, чтобы убедиться, что он отправляется от доверенного издателя, и проверяет, что токен является допустимым для использования с API, который его получает. Поэтому при переходе к контроллеру упорядочивания или Подборка будет возвращен код состояния HTTP 401 (несанкционированный), указывающий на необходимость использования маркера доступа.

> [!NOTE]
> Перед добавлением MVC с `app.UseMvc()` или `app.UseMvcWithDefaultRoute()`необходимо добавить по промежуточного слоя авторизации IdentityServer в конвейер запросов HTTP веб-приложения.

### <a name="making-access-requests-to-apis"></a>Предоставление запросов на доступ к API-интерфейсам

При выполнении запросов к микрослужбам упорядочивания и подбора маркер доступа, полученный от IdentityServer во время процесса проверки подлинности, должен быть включен в запрос, как показано в следующем примере кода:

```csharp
var authToken = Settings.AuthAccessToken;  
Order = await _ordersService.GetOrderAsync(Convert.ToInt32(order.OrderNumber), authToken);
```

Маркер доступа хранится как параметр приложения и извлекается из хранилища, зависящего от платформы, и включается в вызов метода `GetOrderAsync` в классе `OrderService`.

Аналогичным образом маркер доступа должен быть включен при отправке данных в защищенный API IdentityServer, как показано в следующем примере кода:

```csharp
var authToken = Settings.AuthAccessToken;  
await _basketService.UpdateBasketAsync(new CustomerBasket  
{  
    BuyerId = userInfo.UserId,   
    Items = BasketItems.ToList()  
}, authToken);
```

Маркер доступа извлекается из хранилища, зависящего от платформы, и включается в вызов метода `UpdateBasketAsync` в классе `BasketService`.

Класс `RequestProvider` в мобильном приложении eShopOnContainers использует класс `HttpClient` для выполнения запросов к интерфейсам API RESTFUL, предоставляемым эталонным приложением eShopOnContainers. При выполнении запросов к интерфейсам API упорядочивания и корзины, для которых требуется авторизация, в запрос должен быть включен допустимый маркер доступа. Это достигается путем добавления маркера доступа к заголовкам экземпляра `HttpClient`, как показано в следующем примере кода:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
```

Свойство `DefaultRequestHeaders` класса `HttpClient` предоставляет заголовки, которые отправляются с каждым запросом, а маркер доступа добавляется в заголовок `Authorization` с префиксом строки `Bearer`. Когда запрос отправляется в API RESTFUL, значение заголовка `Authorization` извлекается и проверяется, чтобы убедиться, что он отправлен от доверенного издателя, и используется для определения того, имеет ли пользователь разрешение на вызов API, который его получает.

Дополнительные сведения о том, как мобильное приложение eShopOnContainers выполняет веб-запросы, см. в разделе [доступ к удаленным данным](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md).

## <a name="summary"></a>Сводка

Существует множество подходов к интеграции проверки подлинности и авторизации в приложение Xamarin. Forms, которое взаимодействует с веб-приложением ASP.NET MVC. Мобильное приложение eShopOnContainers выполняет проверку подлинности и авторизацию с помощью микрослужбы контейнерных удостоверений, которая использует IdentityServer 4. IdentityServer — это инфраструктура OpenID Connect Connect с открытым исходным кодом и OAuth 2,0 для ASP.NET Core, которая интегрируется с ASP.NET Core удостоверением для выполнения проверки подлинности маркера носителя.

Мобильное приложение запрашивает маркеры безопасности от IdentityServer для проверки подлинности пользователя или доступа к ресурсу. При доступе к ресурсу маркер доступа должен быть добавлен в запрос к интерфейсам API, требующим авторизации. По промежуточного слоя IdentityServer проверяет входящие маркеры доступа, чтобы убедиться, что они отправляются от доверенного издателя и что они действительны для использования с API, который их получает.

## <a name="related-links"></a>Связанные ссылки

- [Скачать электронную книгу (2 МБ в формате PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (пример)](https://github.com/dotnet-architecture/eShopOnContainers)
