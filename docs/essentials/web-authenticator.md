---
title: 'Xamarin.Essentials: Веб-средство проверки подлинности'
description: В этом документе описывается класс WebAuthenticator в Xamarin.Essentials. Он позволяет запускать потоки проверки подлинности на основе браузера, ожидающие обратный вызов приложения.
ms.assetid: 3D95371E-5D59-440E-8D31-F3C04E493DC1
author: redth
ms.author: jodick
ms.date: 03/26/2020
ms.openlocfilehash: 82c136e1de7d2aa7f2d7f132b8ee1639ee44aba4
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "80638157"
---
# <a name="xamarinessentials-web-authenticator"></a>Xamarin.Essentials: Веб-средство проверки подлинности

Класс **WebAuthenticator** позволяет запускать потоки на основе браузера, которые ожидают обратного вызова по определенному URL-адресу, зарегистрированному за приложением.

## <a name="overview"></a>Обзор

Во многих приложениях требуется функция проверки подлинности пользователей. Часто это означает, что пользователи смогут выполнять вход в существующие учетные записи Майкрософт, Facebook, Google, а теперь и Apple.

[Библиотека проверки подлинности Майкрософт (MSAL)](https://docs.microsoft.com/azure/active-directory/develop/msal-overview) содержит удобное готовое решение для добавления функции проверки подлинности в приложение. Реализована даже поддержка приложений Xamarin в клиентском пакете NuGet.

Если вы хотите использовать собственную веб-службу для проверки подлинности, вы можете использовать **WebAuthenticator** для реализации такой возможности на стороне клиента.

## <a name="why-use-a-server-back-end"></a>Зачем использовать серверный компонент?

Многие поставщики проверки подлинности теперь предлагают только потоки явной или двусторонней проверки подлинности для обеспечения безопасности. Это означает, что для выполнения потока проверки подлинности вам потребуется _секрет клиента_ от поставщика. К сожалению, мобильные приложения — не лучшее место для хранения секретов. И хранение любой информации в коде, двоичных файлах или любом другом месте в мобильном приложении считается небезопасными.

Рекомендуется использовать серверный веб-компонент в качестве промежуточного уровня между мобильным приложением и поставщиком проверки подлинности.

> [!IMPORTANT]
> Из-за недостаточной защиты хранимых секретов клиента настоятельно не рекомендуется использовать старые библиотеки и шаблоны проверки подлинности, предназначенные только для мобильных устройств и не использующие серверные веб-компоненты в потоке проверки подлинности.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

Для получения доступа к функции **WebAuthenticator** нужно создать описанную ниже конфигурацию для конкретной платформы.

# <a name="android"></a>[Android](#tab/android)

Для обработки URI обратного вызова на устройствах Android требуется настроить фильтр намерения. Это легко сделать, создав подкласс класса `WebAuthenticatorCallbackActivity`.

> [!NOTE]
> Рекомендуем реализовать [ссылки на приложения Android](https://developer.android.com/training/app-links/) для обработки URI обратного вызова. Убедитесь, что только ваше приложение может зарегистрироваться для обработки URI обратного вызова.

```csharp
const string CALLBACK_SCHEME = "myapp";

[Activity(NoHistory = true, LaunchMode = LaunchMode.SingleTop)]
[IntentFilter(new[] { Intent.ActionView },
    Categories = new[] { Intent.CategoryDefault, Intent.CategoryBrowsable },
    DataScheme = CALLBACK_SCHEME)]
public class WebAuthenticationCallbackActivity : Xamarin.Essentials.WebAuthenticatorCallbackActivity
{
}
```

Кроме того, вам нужно выполнить обратный вызов Essentials из переопределения `OnResume` в `MainActivity`:

```csharp
protected override void OnResume()
{
    base.OnResume();

    Xamarin.Essentials.Platform.OnResume();
}
```

# <a name="ios"></a>[iOS](#tab/ios)

В iOS необходимо добавить шаблон URI обратного вызова приложения в файл Info.plist.

> [!NOTE]
> Для регистрации URI обратного вызова приложения рекомендуется использовать [универсальные ссылки приложений](https://developer.apple.com/documentation/uikit/inter-process_communication/allowing_apps_and_websites_to_link_to_your_content).

Кроме того, потребуется переопределить метод `OpenUrl` `AppDelegate` для обращений к Essentials:

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
{
    if (Xamarin.Essentials.Platform.OpenUrl(app, url, options))
        return true;

    return base.OpenUrl(app, url, options);
}
```

# <a name="uwp"></a>[UWP](#tab/uwp)

Для UWP необходимо объявить URI обратного вызова в файле `Package.appxmanifest`.

```xml
    <Extensions>
        <uap:Extension Category="windows.protocol">
            <uap:Protocol Name="myapp">
                <uap:DisplayName>My App</uap:DisplayName>
            </uap:Protocol>
        </uap:Extension>
    </Extensions>
```

-----

## <a name="using-webauthenticator"></a>Использование WebAuthenticator

Добавьте в свой класс ссылку на Xamarin.Essentials:

```csharp
using Xamarin.Essentials;
```

API состоит в основном из одного метода `AuthenticateAsync`, который принимает два параметра: URL-адрес, используемый для выполнения потока в веб-браузере, и URI, используемый для обратного вызова в потоке и зарегистрированный за приложением для обработки.

Результатом является объект `WebAuthenticatorResult`, который содержит все параметры запроса, проанализированные из URI обратного вызова:

```csharp
var authResult = await WebAuthenticator.AuthenticateAsync(
        new Uri("https://mysite.com/mobileauth/Microsoft"),
        new Uri("myapp://"));

var accessToken = authResult?.AccessToken;
```

API `WebAuthenticator` обращается по URL-адресу в браузере и ожидает получения обратного вызова:

![Стандартный поток WebAuthenticator](images/web-authenticator.png)

Если пользователь отменяет поток в любой точке, возвращается результат `null`.

## <a name="platform-differences"></a>Различия между платформами

# <a name="android"></a>[Android](#tab/android)

По возможности используются пользовательские вкладки, в противном случае для URL-адреса запускается намерение.

# <a name="ios"></a>[iOS](#tab/ios)

В iOS 12 или более поздней версии используется `ASWebAuthenticationSession`.  В iOS 11 используется `SFAuthenticationSession`.  В более ранних версиях iOS по возможности используется `SFSafariViewController`, в противном случае используется Safari.

# <a name="uwp"></a>[UWP](#tab/uwp)

В UWP по возможности используется `WebAuthenticationBroker`, в противном случае используется браузер системы.

-----

## <a name="apple-sign-in"></a>Вход с помощью Apple ID

В соответствии с [правилами оценки приложений для App Store](https://developer.apple.com/app-store/review/guidelines/#sign-in-with-apple), если для проверки подлинности в приложении используются службы с поддержкой входа с учетной записью социальной сети, в приложении также должен предлагаться вход с помощью Apple ID.

Чтобы добавить такой вариант входа, нужно сначала [настроить приложение соответствующим образом](https://docs.microsoft.com/xamarin/ios/platform/ios13/sign-in).

Для iOS 13 и более поздних версий необходимо вызвать метод `AppleSignInAuthenticator.AuthenticateAsync()`. При этом будет использоваться нативный API для входа с помощью Apple ID, чтобы обеспечить оптимальную работу пользователей на таких устройствах. Вы можете написать общий код для использования нужного API в среде выполнения следующим образом:

```csharp
var scheme = "..."; // Apple, Microsoft, Google, Facebook, etc.
WebAuthenticatorResult r = null;

if (scheme.Equals("Apple")
    && DeviceInfo.Platform == DevicePlatform.iOS
    && DeviceInfo.Version.Major >= 13)
{
    // Use Native Apple Sign In API's
    r = await AppleSignInAuthenticator AuthenticateAsync();
}
else
{
    // Web Authentication flow
    var authUrl = new Uri(authenticationUrl + scheme);
    var callbackUrl = new Uri("xamarinessentials://");

    r = await WebAuthenticator.AuthenticateAsync(authUrl, callbackUrl);
}

var accessToken = r?.AccessToken;
```

> [!TIP]
> На устройствах, отличающихся от iOS 13, выполнение этого кода приведет к выполнению потока проверки подлинности в браузере. Этот код также можно использовать для включение входа с помощью Apple ID на устройствах Android и UWP.

-----

## <a name="aspnet-core-server-back-end"></a>Серверный компонент ASP.NET Core

API `WebAuthenticator` можно использовать с любой серверной веб-службой.  Для использования с приложением ASP.NET Core сначала нужно настроить веб-приложение, выполнив такие действия:

1. В веб-приложении ASP.NET Core настройте необходимые [поставщики проверки подлинности на основе учетной записи социальной сети](https://docs.microsoft.com/aspnet/core/security/authentication/social/?view=aspnetcore-3.1&tabs=visual-studio).
2. Задайте для схемы проверки подлинности по умолчанию значение `CookieAuthenticationDefaults.AuthenticationScheme` в вызове `.AddAuthentication()`.
3. Используйте `.AddCookies()` в вызове `.AddAuthentication()` Startup.cs.
4. Для настройки всех поставщиков обязательно используйте `.SaveTokens = true;`.

> [!TIP]
> Чтобы включить вход с помощью Apple ID, можно использовать пакет NuGet `AspNet.Security.OAuth.Apple`.  Полный пример [Startup.cs](https://github.com/xamarin/Essentials/blob/develop/Samples/Sample.Server.WebAuthenticator/Startup.cs#L32-L60) можно просмотреть в репозитории GitHub для Essentials.

### <a name="add-a-custom-mobile-auth-controller"></a>Добавление пользовательского контроллера для проверки подлинности на мобильных устройствах

Поток проверки подлинности на мобильных устройствах желательно инициировать непосредственно для выбранного пользователем поставщика (например, путем нажатия кнопки "Майкрософт" на экране входа в приложение).  Важно также реализовать возврат в приложение соответствующих сведений по конкретному URI обратного вызова для завершения потока проверки подлинности.

Для таких задач используйте контроллер API:

```csharp
[Route("mobileauth")]
[ApiController]
public class AuthController : ControllerBase
{
    const string callbackScheme = "myapp";

    [HttpGet("{scheme}")] // eg: Microsoft, Facebook, Apple, etc
    public async Task Get([FromRoute]string scheme)
    {
        // 1. Initiate authentication flow with the scheme (provider)
        // 2. When the provider calls back to this URL
        //    a. Parse out the result
        //    b. Build the app callback URL
        //    c. Redirect back to the app
    }
}
```

Задача этого контроллера — определить запрашиваемую приложением схему (поставщика) и инициировать поток проверки подлинности с помощью поставщика социальных сетей. Когда поставщик отправляет обратный вызов во внутреннюю веб-службу, контроллер анализирует результат и перенаправляет его в URI обратного вызова приложения с параметрами.

Иногда может потребоваться вернуть данные, например `access_token` поставщика, в приложение, что можно сделать с помощью параметров запроса URI обратного вызова. Вы также можете создать свое удостоверение на сервере и передать собственный маркер в приложение. Этот процесс вы можете настроить на свое усмотрение.

Ознакомьтесь с [полным примером контроллера](https://github.com/xamarin/Essentials/blob/develop/Samples/Sample.Server.WebAuthenticator/Controllers/MobileAuthController.cs) в репозитории Essentials.

-----
## <a name="api"></a>API

- [Исходный код WebAuthenticator](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/WebAuthenticator)
- [Документация по API WebAuthenticator](xref:Xamarin.Essentials.WebAuthenticator)
- [Пример для сервера ASP.NET Core](https://github.com/xamarin/Essentials/blob/develop/Samples/Sample.Server.WebAuthenticator/)
