---
title: Проверка подлинности пользователей с Azure Active Directory B2C
description: Azure Active Directory B2C обеспечивает управление идентификаторами облака для мобильных приложений и веб-потребитель. В этой статье показано, как использовать Azure Active Directory B2C для интеграции управления удостоверениями в мобильное приложение с помощью библиотеки проверки подлинности Майкрософт.
ms.prod: xamarin
ms.assetid: B0A5DB65-0585-4A00-B908-22CCC286E6B6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/17/2019
ms.openlocfilehash: 06f5716c8decb21de39fd46abe734b5fdcd6bd43
ms.sourcegitcommit: 0f78ec17210b915b43ddab75937de8063e472c70
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/27/2019
ms.locfileid: "67417947"
---
# <a name="authenticate-users-with-azure-active-directory-b2c"></a>Проверка подлинности пользователей с Azure Active Directory B2C

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)

_Azure Active Directory B2C обеспечивает управление идентификаторами облака для мобильных приложений и веб-потребитель. В этой статье показано, как использовать Azure Active Directory B2C для интеграции управления удостоверениями в мобильное приложение с помощью библиотеки проверки подлинности Майкрософт._

## <a name="overview"></a>Обзор

Azure Active Directory B2C (ADB2C) — это служба управления удостоверениями для клиентских приложений. Она позволяет пользователям входить ваше приложение, с помощью существующих учетных записей социальных сетей или пользовательские учетные данные, такие как адрес электронной почты или имя пользователя и пароль. Пользовательские учетные данные учетных записей, называются _локального_ учетных записей.

Процесс для интеграции службы управления удостоверениям Azure Active Directory B2C в мобильные приложения выглядит следующим образом:

1. Создание клиента Azure Active Directory B2C
1. Регистрации вашего мобильного приложения в клиенте Azure Active Directory B2C
1. Создание политик для регистрации и входа в систему и забыли пароль маршруты пользователей
1. Используйте библиотеки аутентификации Майкрософт (MSAL) для запуска рабочего процесса проверки подлинности в клиенте Azure Active Directory B2C.

> [!NOTE]
> Azure Active Directory B2C поддерживает несколько поставщиков удостоверений, включая Microsoft, GitHub, Facebook, Twitter и многое другое. Дополнительные сведения о возможностях Azure Active Directory B2C, см. в разделе [документация Azure Active Directory B2C](/azure/active-directory-b2c/).
>
> Библиотека проверки подлинности Майкрософт поддерживает несколько архитектур приложений и платформ. Сведения о возможностях MSAL, см. в разделе [библиотека проверки подлинности Майкрософт](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki) на сайте GitHub.

## <a name="configure-an-azure-active-directory-b2c-tenant"></a>Настройка клиента Azure Active Directory B2C

Чтобы запустить пример проекта, необходимо создать клиент Azure Active Directory B2C. Дополнительные сведения см. в разделе [создание клиента Azure Active Directory B2C на портале Azure](/azure/active-directory-b2c/active-directory-b2c-get-started/).

После создания клиента, вам потребуется **имя_клиента** и **идентификатор клиента** Настройка мобильного приложения. Идентификатор клиента и имя определяются по доменам, сгенерированное при создании URL-адрес клиента. Если URL-адрес созданного клиента `https://contoso20190410tenant.onmicrosoft.com/` **идентификатор_клиента** является `contoso20190410tenant.onmicrosoft.com` и **имя_клиента** является `contoso20190410tenant`. Найти домен клиента на портале Azure, щелкнув **фильтр каталога и подписки** в верхнем меню. На следующем рисунке показан в каталоге Azure и кнопка "Фильтр" для подписки и домен клиента:

[![Имя клиента в Azure представление фильтра каталог и подписка](azure-ad-b2c-images/azure-tenant-name-cropped.png)](azure-ad-b2c-images/azure-tenant-name.png#lightbox)

В образце проекта, изменение **Constants.cs** в файле `tenantName` и `tenantId` поля. Ниже показано, как эти значения должны быть заданы при доменом клиента являются `https://contoso20190410tenant.onmicrosoft.com/`, замените эти значения значениями из вашего портала:

```csharp
public static class Constants
{
    static readonly string tenantName = "contoso20190410tenant";
    static readonly string tenantId = "contoso20190410tenant.onmicrosoft.com";
    ...
}
```

## <a name="register-your-mobile-application-with-azure-active-directory-b2c"></a>Регистрация мобильного приложения в Azure Active Directory B2C

Мобильное приложение должно быть зарегистрировано клиента перед их подключении и проверки подлинности пользователей. Процесс регистрации назначает уникальный **идентификатор приложения** приложению и **URL-адрес перенаправления** , направляет ответов к приложению после проверки подлинности. Дополнительные сведения см. в разделе [Azure Active Directory B2C: Регистрация приложения](/azure/active-directory-b2c/active-directory-b2c-app-registration/). Следует знать **идентификатор приложения** присваиваемый приложению, которое указано после имени приложения в области отображения свойств. На следующем снимке экрана показан где найти идентификатор приложения:

[![Идентификатор приложения в области отображения свойств приложения Azure](azure-ad-b2c-images/azure-application-id-cropped.png)](azure-ad-b2c-images/azure-application-id.png#lightbox)

Библиотека проверки подлинности Майкрософт ожидает **URL-адрес перенаправления** для приложения до вашего **идентификатор приложения** начинаются с текста «msal», после чего конечной точке с именем «auth». Если идентификатор приложения — «1234abcd», полный URL-адрес должен быть `msal1234abcd://auth`. Убедитесь, что приложение включил **Native client** параметр и создайте **пользовательский URI перенаправления** с помощью идентификатор приложения, как показано на следующем снимке экрана:

![Пользовательские URI перенаправления в области отображения свойств приложения Azure](azure-ad-b2c-images/azure-redirect-uri.png)

URL-адрес будет использоваться далее в обоих Android **ApplicationManifest.xml** и iOS **Info.plist**.

В образце проекта, изменение **Constants.cs** в файле `clientId` на вашей **идентификатор приложения**. В следующем коде показано как это значение должно быть установлено Если идентификатор приложения — `1234abcd`:

```csharp
public static class Constants
{
    static readonly string tenantName = "contoso20190410tenant";
    static readonly string tenantId = "contoso20190410tenant.onmicrosoft.com";
    static readonly string clientId = "1234abcd";
    ...
}
```

## <a name="create-sign-up-and-sign-in-policies-and-forgot-password-policies"></a>Создание политики регистрации и входа в систему и забыли политики паролей

Политика — это среда, в которые пользователи подключаются через для выполнения задачи, такие как создание учетной записи или сброса пароля. Политики также определяет содержимое токенов, которые приложение получает при выходе пользователя из интерфейса. Необходимо настроить политики для обоих учетной записи, регистрации и входа в систему и сбросить пароль. Azure имеет встроенные политики, которые упрощают создание распространенных политик. Дополнительные сведения см. в разделе [Azure Active Directory B2C: Встроенные политики](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).

После завершения установки политики, должны иметь две политики **маршруты пользователей (политики)** представления на портале Azure. Снимке экрана ниже показаны два настроенных политик на портале Azure:

![Просмотреть два настроенных политик в потоках пользователя Azure (политики)](azure-ad-b2c-images/azure-application-policies.png)

В образце проекта, изменение **Constants.cs** в файле `policySignin` и `policyPassword` поля, чтобы их имена соответствовали именам, выбранной на этапе настройки политики:

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

Общий проект .NET Standard и платформы проектов в решение Xamarin.Forms, необходимо добавить пакет NuGet библиотеки аутентификации Майкрософт (MSAL). Включает в себя MSAL `PublicClientApplicationBuilder` класс, который создает объект, соблюдая `IPublicClientApplication` интерфейс. MSAL использует `With` предложений для передачи дополнительных параметров методов конструктора и проверки подлинности.

В образце проекта, кода программной части для **App.xaml** определяет статические свойства с именем `AuthenticationClient` и `UIParent`и создает экземпляр `AuthenticationClient` объекта в конструкторе. `WithIosKeychainSecurityGroup` Предложение предоставляет имя группы безопасности для приложений iOS. `WithB2CAuthority` Предложение предоставляет значение по умолчанию **центра**, или политики, который будет использоваться для проверки подлинности пользователей. Следующий пример демонстрирует способы создания экземпляра `PublicClientApplication`:

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
            .Build();

        MainPage = new NavigationPage(new LoginPage());
    }

    ...
```

`OnAppearing` Обработчик событий в **LoginPage.xaml.cs** кода программной части вызовы `AcquireTokenSilentAsync` обновить токен проверки подлинности для пользователей, зарегистрированных в перед. Процесс проверки подлинности перенаправляет `LogoutPage` успеха и не выполняет никаких действий в случае сбоя. В следующем примере показано процесс автоматической повторной проверки подлинности в `OnAppearing`:

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

`OnLoginButtonClicked` Обработчик событий (вызываемых при нажатии кнопки входа) вызовов `AcquireTokenAsync`. Библиотеку MSAL автоматически открывает браузер мобильного устройства и переходит на страницу входа. URL-адрес входа, называется **центра**, представляет собой сочетание имени клиента и определить политики в **Constants.cs** файла. Если пользователь выбирает забыли параметр password, они будут возвращены в приложение с помощью исключение, которое запускает забыли пароль качества. В следующем примере показано процесс проверки подлинности:

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

`OnForgotPassword` Метод похож на процесс входа в систему, но реализует пользовательскую политику. `OnForgotPassword` использует другой перегрузке `AcquireTokenAsync`, который позволяет указать определенный **центр**. В следующем примере показано, как указан настраиваемый **центра** при получении маркера:

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

Заключительная часть проверки подлинности — это процесс выхода. `OnLogoutButtonClicked` Метод вызывается, когда пользователь нажимает кнопку выхода. Он перебирает все учетные записи и гарантирует, что токены стали недействительными. Следующий пример демонстрирует выхода реализации:

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

В iOS, настраиваемой схемой URL-адрес, который был зарегистрирован с помощью Azure Active Directory B2C необходимо зарегистрировать в **Info.plist**. MSAL ожидает, что схема URL-адреса, чтобы соответствовать определенному шаблону, описанные ранее в [регистрации мобильного приложения в Azure Active Directory B2C](/docs/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md#register-your-mobile-application-with-azure-active-directory-b2c). На следующем рисунке показан пользовательский схема URL-адреса в **Info.plist**.

![«Регистрация схему URL-адрес в iOS»](azure-ad-b2c-images/customurl-ios.png)

MSAL также требуется правам цепочки ключей в iOS, зарегистрированных в **Entitilements.plist**, как показано на следующем снимке экрана:

![«Установка прав приложения в iOS»](azure-ad-b2c-images/entitlements-ios.png)

По завершении запроса авторизации Azure Active Directory B2C перенаправляет URL-адрес зарегистрированного перенаправления. Пользовательские схемы URL-адрес приводит к iOS запуск мобильного приложения и передав URL-адрес в качестве параметра запуска, где он обрабатывается `OpenUrl` переопределить приложения `AppDelegate` класса и возвращает управление ее интерфейс в MSAL. `OpenUrl` Реализация показана в следующем примере кода:

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

В Android настраиваемой схемой URL-адрес, который был зарегистрирован с помощью Azure Active Directory B2C необходимо зарегистрировать в **AndroidManifest.xml**. MSAL ожидает, что схема URL-адреса, чтобы соответствовать определенному шаблону, описанные ранее в [регистрации мобильного приложения в Azure Active Directory B2C](/docs/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md#register-your-mobile-application-with-azure-active-directory-b2c). В следующем примере показан пользовательский схема URL-адреса в **AndroidManifest.xml**.

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
        <data android:scheme="INSERT_URI_SCHEME_HERE" android:host="auth" />"
      </intent-filter>
    </activity>"
  </application>
</manifest>
```

`MainActivity` Класса необходимо изменить, чтобы предоставить `UIParent` объект приложения в ходе `OnCreate` вызова. По завершении запроса авторизации Azure Active Directory B2C перенаправляет зарегистрированной схемы URL-адрес из **AndroidManifest.xml**. Зарегистрированные схемы URI приводит к Android вызова `OnActivityResult` метода с URL-адрес как параметр запуска, где он обрабатывается `SetAuthenticationContinuationEventArgs` метод.

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

Без дополнительной настройки необходим для использования MSAL на универсальной платформе Windows

## <a name="run-the-project"></a>Запуск проекта

Запустите приложение на устройстве виртуальным или физическим. При касании **входа** кнопку следует откройте браузер и перейдите на страницу, где можно войти или создать учетную запись. После выполнения входа в систему, вы будете перенаправлены на страницу выхода из приложения. На следующем рисунке показан вход пользователя на экране, работающие на Android и iOS:

![«Azure ADB2C экран входа в Android и iOS»](azure-ad-b2c-images/login.png)

## <a name="related-links"></a>Связанные ссылки

- [AzureADB2CAuth (пример)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)
- [Azure Active Directory B2C](/azure/active-directory-b2c/)
- [Библиотека аутентификации Майкрософт](https://www.nuget.org/packages/Microsoft.Identity.Client)
- [Документация по библиотеке проверки подлинности Майкрософт](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)
