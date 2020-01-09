---
title: Проверка подлинности пользователей с помощью Azure Active Directory B2C
description: Azure Active Directory B2C обеспечивает управление удостоверениями в облаке для мобильных и веб-приложений. В этой статье показано, как использовать Azure Active Directory B2C для интеграции управления удостоверениями в мобильное приложение с помощью библиотеки проверки подлинности Майкрософт.
ms.prod: xamarin
ms.assetid: B0A5DB65-0585-4A00-B908-22CCC286E6B6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/04/2019
ms.openlocfilehash: 946cf65f7d83722fd388bed555b9d3f35c487708
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75489821"
---
# <a name="authenticate-users-with-azure-active-directory-b2c"></a>Проверка подлинности пользователей с помощью Azure Active Directory B2C

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azureadb2cauth)

_Azure Active Directory B2C обеспечивает управление удостоверениями в облаке для мобильных и веб-приложений. В этой статье показано, как использовать Azure Active Directory B2C для интеграции управления удостоверениями в мобильное приложение с помощью библиотеки проверки подлинности Майкрософт._

## <a name="overview"></a>Обзор

Azure Active Directory B2C (ADB2C) — это служба управления удостоверениями для приложений, ориентированных на потребителей. Она позволяет пользователям входить в приложение с помощью существующих учетных записей социальных сетей или пользовательских учетных данных, таких как электронная почта, имя пользователя и пароль. Учетные записи настраиваемых учетных данных называются _локальными_ учетными записями.

Процесс для интеграции службы управления удостоверениям Azure Active Directory B2C в мобильные приложения выглядит следующим образом:

1. Создание клиента Azure Active Directory B2C.
1. Регистрации вашего мобильного приложения в клиенте Azure Active Directory B2C.
1. Создание политик для регистрации и входа и забытых пользовательских потоков паролей.
1. Используйте библиотеку проверки подлинности Майкрософт (MSAL), чтобы запустить рабочий процесс проверки подлинности с помощью клиента Azure Active Directory B2C.

> [!NOTE]
> Если у вас еще нет [подписки Azure](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing), создайте [бесплатную учетную запись Azure](https://aka.ms/azfree-docs-mobileapps), прежде чем начать работу.

Azure Active Directory B2C поддерживает несколько поставщиков удостоверений, включая Microsoft, GitHub, Facebook, Twitter и т. д. Дополнительные сведения о Azure Active Directory B2C возможностях см. в [документации по Azure Active Directory B2C](/azure/active-directory-b2c/).

Библиотека проверки подлинности Майкрософт поддерживает несколько архитектур приложений и платформ. Дополнительные сведения о возможностях MSAL см. в разделе [Библиотека проверки подлинности Майкрософт](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki) на GitHub.

## <a name="configure-an-azure-active-directory-b2c-tenant"></a>Настройка клиента Azure Active Directory B2C

Чтобы запустить пример проекта, необходимо создать клиент Azure Active Directory B2C. Дополнительные сведения см. в разделе [создание клиента Azure Active Directory B2C на портале Azure](/azure/active-directory-b2c/active-directory-b2c-get-started/).

После создания клиента для настройки мобильного приложения потребуется **имя клиента** и **идентификатор клиента** . ИДЕНТИФИКАТОР и имя клиента определяются доменом, созданным при создании URL-адреса клиента. Если созданный URL-адрес клиента имеет `https://contoso20190410tenant.onmicrosoft.com/` **идентификатор клиента** `contoso20190410tenant.onmicrosoft.com`, а **имя клиента** — `contoso20190410tenant`. Найдите домен клиента в портал Azure, щелкнув **Фильтр каталогов и подписок** в верхнем меню. На следующем снимке экрана показана кнопка "Каталог Azure" и "Фильтр подписки" и домен клиента:

[![имя клиента в представлении фильтра каталога Azure и подписки](azure-ad-b2c-images/azure-tenant-name-cropped.png)](azure-ad-b2c-images/azure-tenant-name.png#lightbox)

В образце проекта измените файл **Constants.CS** , чтобы задать поля `tenantName` и `tenantId`. В следующем коде показано, как эти значения должны быть заданы, если домен клиента `https://contoso20190410tenant.onmicrosoft.com/`, замените эти значения значениями с портала.

```csharp
public static class Constants
{
    static readonly string tenantName = "contoso20190410tenant";
    static readonly string tenantId = "contoso20190410tenant.onmicrosoft.com";
    ...
}
```

## <a name="register-your-mobile-application-with-azure-active-directory-b2c"></a>Регистрация мобильного приложения с помощью Azure Active Directory B2C

Мобильное приложение должно быть зарегистрировано в клиенте, прежде чем оно сможет подключиться и проверить подлинность пользователей. Процесс регистрации назначает приложению уникальный **идентификатор приложения** и **URL-адрес перенаправления** , который направляет ответы обратно в приложение после проверки подлинности. Дополнительные сведения см. в разделе [Azure Active Directory B2C: регистрация приложения](/azure/active-directory-b2c/active-directory-b2c-app-registration/). Необходимо знать **идентификатор приложения** , назначенный приложению, который указан после имени приложения в представлении свойств. На следующем снимке экрана показано, где найти идентификатор приложения:

[Идентификатор приложения ![в представлении свойств приложения Azure](azure-ad-b2c-images/azure-application-id-cropped.png)](azure-ad-b2c-images/azure-application-id.png#lightbox)

Библиотека проверки подлинности Майкрософт ожидает **URL-адрес перенаправления** приложения в качестве **идентификатора приложения** с текстом "msal", а затем конечную точку с именем "auth". Если идентификатор приложения — "1234abcd", полный URL-адрес должен быть `msal1234abcd://auth`. Убедитесь, что в приложении включен собственный параметр **клиента** , и создайте **пользовательский URI перенаправления** с помощью идентификатора приложения, как показано на следующем снимке экрана:

![Пользовательский URI перенаправления в представлении свойств приложения Azure](azure-ad-b2c-images/azure-redirect-uri.png)

URL-адрес будет использоваться позже в Android **ApplicationManifest. XML** и iOS **info. plist**.

В образце проекта измените файл **Constants.CS** , чтобы в поле `clientId` задать **идентификатор приложения**. В следующем коде показано, как это значение должно быть задано, если идентификатор приложения `1234abcd`:

```csharp
public static class Constants
{
    static readonly string tenantName = "contoso20190410tenant";
    static readonly string tenantId = "contoso20190410tenant.onmicrosoft.com";
    static readonly string clientId = "1234abcd";
    ...
}
```

## <a name="create-sign-up-and-sign-in-policies-and-forgot-password-policies"></a>Создание политик регистрации и входа и забытые политики паролей

Политика — это опыт, с помощью которого пользователи могут выполнять такие задачи, как создание учетной записи или сброс пароля. Политика также определяет содержимое токенов, получаемых приложением при возвращении пользователя из процесса. Необходимо настроить политики для регистрации учетной записи и входа в систему, а также для сброса пароля. В Azure есть встроенные политики, упрощающие создание общих политик. Дополнительные сведения см. в разделе [Azure Active Directory B2C: встроенные политики](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).

После завершения установки политики в портал Azure будут существовать две политики в представлении " **потоки пользователей (политики)** ". На следующем снимке экрана показаны две настроенные политики в портал Azure.

![Две настроенные политики в представлении "потоки пользователей Azure" (политики)](azure-ad-b2c-images/azure-application-policies.png)

В образце проекта измените файл **Constants.CS** , чтобы задать `policySignin` и `policyPassword` поля для отображения имен, выбранных во время установки политики.

```csharp
public static class Constants
{
    static readonly string tenantName = "contoso20190410tenant";
    static readonly string tenantId = "contoso20190410tenant.onmicrosoft.com";
    static readonly string clientId = "1234abcd";
    static readonly string policySignin = "B2C_1_signupsignin1";
    static readonly string policyPassword = "B2C_1_passwordreset";
    ...
}
```

## <a name="use-the-microsoft-authentication-library-msal-for-authentication"></a>Использование библиотеки проверки подлинности Майкрософт (MSAL) для проверки подлинности

Пакет NuGet библиотеки проверки подлинности (Майкрософт) (MSAL) должен быть добавлен в общий, .NET Standard проект и проекты платформы в решении Xamarin. Forms. MSAL включает класс `PublicClientApplicationBuilder`, который конструирует объект, который придерживается интерфейса `IPublicClientApplication`. MSAL использует предложения `With` для предоставления дополнительных параметров конструктору и методам проверки подлинности.

В примере проекта код программной части для **app. XAML** определяет статические свойства с именами `AuthenticationClient` и `UIParent`, а также создает экземпляр объекта `AuthenticationClient` в конструкторе. Предложение `WithIosKeychainSecurityGroup` предоставляет имя группы безопасности для приложений iOS. Предложение `WithB2CAuthority` предоставляет **центр**по умолчанию или политику, которые будут использоваться для проверки подлинности пользователей. Предложение `WithRedirectUri` сообщает экземпляру концентраторов уведомлений Azure, какой URI перенаправления следует использовать, если задано несколько URI. В следующем примере показано, как создать экземпляр `PublicClientApplication`.

```csharp
public partial class App : Application
{
    public static IPublicClientApplication AuthenticationClient { get; private set; }

    public static object UIParent { get; set; } = null;

    public App()
    {
        InitializeComponent();

        AuthenticationClient = PublicClientApplicationBuilder.Create(Constants.ClientId)
            .WithIosKeychainSecurityGroup(Constants.IosKeychainSecurityGroups)
            .WithB2CAuthority(Constants.AuthoritySignin)
            .WithRedirectUri($"msal{Constants.ClientId}://auth")
            .Build();

        MainPage = new NavigationPage(new LoginPage());
    }

    ...
```

> [!NOTE]
> Если в экземпляре концентраторов уведомлений Azure определен только один URI перенаправления, то экземпляр `AuthenticationClient` может работать без указания URI перенаправления с помощью предложения `WithRedirectUri`. Однако следует всегда указывать это значение в случае, если конфигурация Azure расширяется для поддержки других клиентов или методов проверки подлинности.

Обработчик событий `OnAppearing` в коде **LoginPage.XAML.CS** вызывает `AcquireTokenSilentAsync`, чтобы обновить маркер проверки подлинности для пользователей, выполнивших вход. Процесс проверки подлинности перенаправляется на `LogoutPage` в случае успеха и не выполняет никаких действий в случае сбоя. В следующем примере показан процесс повторной проверки подлинности без участия пользователя в `OnAppearing`:

```csharp
public partial class LoginPage : ContentPage
{
    ...

    protected override async void OnAppearing()
    {
        try
        {
            // Look for existing account
            IEnumerable<IAccount> accounts = await App.AuthenticationClient.GetAccountsAsync();

            AuthenticationResult result = await App.AuthenticationClient
                .AcquireTokenSilent(Constants.Scopes, accounts.FirstOrDefault())
                .ExecuteAsync();

            await Navigation.PushAsync(new LogoutPage(result));
        }
        catch
        {
            // Do nothing - the user isn't logged in
        }
        base.OnAppearing();
    }

    ...
}
```

Обработчик событий `OnLoginButtonClicked` (срабатывает при нажатии кнопки входа) вызывает `AcquireTokenAsync`. Библиотека MSAL автоматически открывает браузер мобильных устройств и переходит на страницу входа. URL-адрес входа, называемый **центром**, представляет собой сочетание имени клиента и политик, определенных в файле **Constants.CS** . Если пользователь выбирает параметр забыл пароль, он возвращается в приложение с исключением, которое запускает процедуру забытого пароля. В следующем примере показан процесс проверки подлинности.

```csharp
public partial class LoginPage : ContentPage
{
    ...

    async void OnLoginButtonClicked(object sender, EventArgs e)
    {
        AuthenticationResult result;
        try
        {
            result = await App.AuthenticationClient
                .AcquireTokenInteractive(Constants.Scopes)
                .WithPrompt(Prompt.SelectAccount)
                .WithParentActivityOrWindow(App.UIParent)
                .ExecuteAsync();

            await Navigation.PushAsync(new LogoutPage(result));
        }
        catch (MsalException ex)
        {
            if (ex.Message != null && ex.Message.Contains("AADB2C90118"))
            {
                result = await OnForgotPassword();
                await Navigation.PushAsync(new LogoutPage(result));
            }
            else if (ex.ErrorCode != "authentication_canceled")
            {
                await DisplayAlert("An error has occurred", "Exception message: " + ex.Message, "Dismiss");
            }
        }
    }

    ...
}
```

Метод `OnForgotPassword` аналогичен процессу входа, но реализует пользовательскую политику. `OnForgotPassword` использует другую перегрузку `AcquireTokenAsync`, которая позволяет предоставить определенный **центр**. В следующем примере показано, как предоставить пользовательский **центр** при получении маркера.

```csharp
public partial class LoginPage : ContentPage
{
    ...
    async Task<AuthenticationResult> OnForgotPassword()
    {
        try
        {
            return await App.AuthenticationClient
                .AcquireTokenInteractive(Constants.Scopes)
                .WithPrompt(Prompt.SelectAccount)
                .WithParentActivityOrWindow(App.UIParent)
                .WithB2CAuthority(Constants.AuthorityPasswordReset)
                .ExecuteAsync();
        }
        catch (MsalException)
        {
            // Do nothing - ErrorCode will be displayed in OnLoginButtonClicked
            return null;
        }
    }
}
```

Последняя часть проверки подлинности — это процесс выхода. Метод `OnLogoutButtonClicked` вызывается, когда пользователь нажимает кнопку Выход. Он циклически проходит все учетные записи и гарантирует, что их маркеры стали недействительными. В примере ниже демонстрируется реализация выхода:

```csharp
public partial class LogoutPage : ContentPage
{
    ...
    async void OnLogoutButtonClicked(object sender, EventArgs e)
    {
        IEnumerable<IAccount> accounts = await App.AuthenticationClient.GetAccountsAsync();

        while (accounts.Any())
        {
            await App.AuthenticationClient.RemoveAsync(accounts.First());
            accounts = await App.AuthenticationClient.GetAccountsAsync();
        }

        await Navigation.PopAsync();
    }
}
```

### <a name="ios"></a>iOS

В iOS настраиваемая схема URL-адресов, зарегистрированная в Azure Active Directory B2C, должна быть зарегистрирована в **info. plist**. MSAL ждет, чтобы схема URL-адресов соответствовала определенному шаблону, описанному ранее в разделе [Регистрация мобильного приложения с помощью Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md#register-your-mobile-application-with-azure-active-directory-b2c). На следующем снимке экрана показана настраиваемая схема URL-адресов в **info. plist**.

!["Регистрация настраиваемой схемы URL-адресов в iOS"](azure-ad-b2c-images/customurl-ios.png)

Для MSAL также требуются права на цепочку ключей в iOS, зарегистрированные в **ентитилементс. plist**, как показано на следующем снимке экрана:

!["Настройка прав приложения в iOS"](azure-ad-b2c-images/entitlements-ios.png)

По завершении запроса авторизации Azure Active Directory B2C перенаправляет URL-адрес зарегистрированного перенаправления. Пользовательская схема URL-адресов приводит к запуску мобильного приложения iOS и передаче URL-адреса в качестве параметра запуска, где он обрабатывается `OpenUrl` переопределением класса приложения `AppDelegate` и возвращает управление интерфейсом MSAL. Реализация `OpenUrl` показана в следующем примере кода:

```csharp
using Microsoft.Identity.Client;

namespace TodoAzure.iOS
{
    [Register("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        ...
        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url);
            return base.OpenUrl(app, url, options);
        }
    }
}
```

### <a name="android"></a>Android

В Android пользовательская схема URL-адресов, зарегистрированная в Azure Active Directory B2C, должна быть зарегистрирована в **AndroidManifest. XML**. MSAL ждет, чтобы схема URL-адресов соответствовала определенному шаблону, описанному ранее в разделе [Регистрация мобильного приложения с помощью Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md#register-your-mobile-application-with-azure-active-directory-b2c). В следующем примере показана пользовательская схема URL-адресов в **файле AndroidManifest. XML**.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionCode="1" android:versionName="1.0" package="com.xamarin.adb2cauthorization">
  <uses-sdk android:minSdkVersion="15" />
  <application android:label="ADB2CAuthorization">
    <activity android:name="microsoft.identity.client.BrowserTabActivity">
      <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <!-- example -->
        <!-- <data android:scheme="msalaaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" android:host="auth" /> -->
        <data android:scheme="INSERT_URI_SCHEME_HERE" android:host="auth" />
      </intent-filter>
    </activity>"
  </application>
</manifest>
```

Класс `MainActivity` необходимо изменить, чтобы предоставить приложению `UIParent` объект во время вызова `OnCreate`. Когда Azure Active Directory B2C завершает запрос авторизации, он перенаправляется на зарегистрированную схему URL-адресов из **AndroidManifest. XML**. Зарегистрированная схема URI вызывает в Android метод `OnActivityResult` с URL-адресом в качестве параметра запуска, где он обрабатывается методом `SetAuthenticationContinuationEventArgs`.

```csharp
public class MainActivity : FormsAppCompatActivity
{
    protected override void OnCreate(Bundle bundle)
    {
        TabLayoutResource = Resource.Layout.Tabbar;
        ToolbarResource = Resource.Layout.Toolbar;

        base.OnCreate(bundle);

        Forms.Init(this, bundle);
        LoadApplication(new App());
        App.UIParent = this;
    }

    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {
        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode, resultCode, data);
    }
}
```

### <a name="universal-windows-platform"></a>Универсальная платформа Windows

Для использования MSAL на универсальная платформа Windows не требуется дополнительная настройка

## <a name="run-the-project"></a>Запуск проекта

Запустите приложение на виртуальном или физическом устройстве. При нажатии кнопки **входа** необходимо открыть браузер и перейти к странице, где можно выполнить вход или создать учетную запись. После завершения процесса входа вы должны вернуться на страницу выхода приложения. На следующем снимке экрана показан экран входа пользователя, выполняющийся в Android и iOS:

![Экран входа Azure ADB2C в Android и iOS](azure-ad-b2c-images/login.png)

## <a name="related-links"></a>Связанные ссылки

- [AzureADB2CAuth (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azureadb2cauth)
- [Azure Active Directory B2C](/azure/active-directory-b2c/)
- [Библиотека аутентификации Майкрософт](https://www.nuget.org/packages/Microsoft.Identity.Client)
- [Документация по библиотеке проверки подлинности Майкрософт](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)
