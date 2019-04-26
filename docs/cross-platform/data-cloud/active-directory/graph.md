---
title: Доступ к API Graph
description: В этом документе описывается добавление проверки подлинности Azure Active Directory для мобильных приложений, созданных с помощью Xamarin.
ms.prod: xamarin
ms.assetid: F94A9FF4-068E-4B71-81FE-46920745380D
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: c43dfa79831f22e55490b27c3c360602ae717627
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61190024"
---
# <a name="accessing-the-graph-api"></a>Доступ к API Graph

Выполните следующие действия, чтобы использовать API Graph из приложения Xamarin.

1. [Регистрация в Azure Active Directory](~/cross-platform/data-cloud/active-directory/get-started/register.md) на *windowsazure.com* портала, затем
2. [Настройка служб](~/cross-platform/data-cloud/active-directory/get-started/configure.md).

## <a name="step-3-adding-active-directory-authentication-to-an-app"></a>Шаг 3. Добавление проверки подлинности Active Directory в приложение

В приложении, добавьте ссылку на **Azure библиотеку аутентификации Active Directory (ADAL в Azure)** с помощью диспетчера пакетов NuGet в Visual Studio или Visual Studio для Mac.
Обязательно выберите **Показать пакеты предварительного выпуска** для включения этого пакета, так как это на стадии предварительной версии.

> [!IMPORTANT]
> Примечание. Azure ADAL 3.0 в настоящее время доступна в предварительной версии и может присутствовать критические изменения перед выпуском окончательной версии. 


![](graph-images/06.-adal-nuget-package.jpg "Добавьте ссылку на библиотеку Azure Active Directory проверки подлинности (ADAL в Azure)")

В приложении теперь необходимо добавить следующие переменные уровня класса, которые требуются для потока проверки подлинности.

```csharp
//Client ID
public static string clientId = "25927d3c-.....-63f2304b90de";
public static string commonAuthority = "https://login.windows.net/common"
//Redirect URI
public static Uri returnUri = new Uri("http://xam-demo-redirect");
//Graph URI if you've given permission to Azure Active Directory
const string graphResourceUri = "https://graph.windows.net";
public static string graphApiVersion = "2013-11-08";
//AuthenticationResult will hold the result after authentication completes
AuthenticationResult authResult = null;
```

Следует заметить, вот `commonAuthority`. При конечную точку проверки подлинности `common`, ваше приложение станет **мультитенантной**, который означает, что любой пользователь, можно использовать имя входа с помощью учетных данных Active Directory. После проверки подлинности этот пользователь будет работать в контексте собственной службы Active Directory — т. е. они будут видеть сведения, связанные с его Active Directory.

### <a name="write-method-to-acquire-access-token"></a>Создать метод для получения токена доступа

Следующий код (для Android) будет запуск проверки подлинности и после завершения назначения результата в `authResult`. IOS и Windows Phone реализаций немного отличаться: второй параметр (`Activity`) отличается в iOS и отсутствует на Windows Phone.

```csharp
public static async Task<AuthenticationResult> GetAccessToken
            (string serviceResourceId, Activity activity)
{
    authContext = new AuthenticationContext(Authority);
    if (authContext.TokenCache.ReadItems().Count() > 0)
        authContext = new AuthenticationContext(authContext.TokenCache.ReadItems().First().Authority);
    var authResult = await authContext.AcquireTokenAsync(serviceResourceId, clientId, returnUri, new AuthorizationParameters(activity));
    return authResult;
}  
```

В приведенном выше коде `AuthenticationContext` отвечает за проверку подлинности с помощью commonAuthority. Он имеет `AcquireTokenAsync` метод, который принимают параметры, как ресурс, который должен быть доступен, в этом случае `graphResourceUri`, `clientId`, и `returnUri`. Приложение будет отправлять `returnUri` после завершения проверки подлинности. Этот код будет оставаться одинаковым для всех платформ, однако последний параметр `AuthorizationParameters`, будет отличаться на разных платформах и отвечает за регулирующих потока проверки подлинности.

В случае с Android или iOS, мы передаем `this` параметр `AuthorizationParameters(this)` для совместного использования контекста, тогда как в Windows он передается без какой-либо параметр как новый `AuthorizationParameters()`.

### <a name="handle-continuation-for-android"></a>Маркер продолжения для Android

После завершения проверки подлинности поток должен вернуться в приложение. В случае с Android, он обрабатывается в следующем примере кода, который следует добавить **MainActivity.cs**:


```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
  base.OnActivityResult(requestCode, resultCode, data);
  AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);

    
}
```

### <a name="handle-continuation-for-windows-phone"></a>Маркер продолжения для Windows Phone

Для Windows Phone измените `OnActivated` метод в **App.xaml.cs** файл с помощью кода:

```csharp
protected override void OnActivated(IActivatedEventArgs args)
{
#if WINDOWS_PHONE_APP
  if (args is IWebAuthenticationBrokerContinuationEventArgs)
  {
     WebAuthenticationBrokerContinuationHelper.SetWebAuthenticationBrokerContinuationEventArgs(args as IWebAuthenticationBrokerContinuationEventArgs);
  }
#endif
  base.OnActivated(args);
}
```

Теперь при запуске приложения, вы увидите диалоговое окно проверки подлинности.
После успешной проверки подлинности вам будет предложено ваши разрешения на доступ к ресурсам (в нашем случае Graph API):

![](graph-images/08.-authentication-flow.jpg "После успешной проверки подлинности вам будет предложено ваши разрешения на доступ к ресурсам в нашем случае Graph API")

Если проверка подлинности завершена успешно и вы предоставили приложению доступ к ресурсам, вы должны получить `AccessToken` и `RefreshToken` поле со списком в `authResult`. Для дальнейшей вызовов API и авторизации с помощью Azure Active Directory в фоновом необходимы эти маркеры.

![](graph-images/07.-access-token-for-authentication.jpg "Эти маркеры необходимы для дальнейшего вызовов API, а также для авторизации с помощью Azure Active Directory в фоновом")

Например приведенный ниже код позволяет получить список пользователей из Active Directory. Можно заменить URL-адрес API веб-API, защищенного Azure AD.

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get,
    "https://graph.windows.net/tendulkar.onmicrosoft.com/users?api-version=2013-04-05");
request.Headers.Authorization =
  new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
var response = await client.SendAsync(request);
var content = await response.Content.ReadAsStringAsync();
```

