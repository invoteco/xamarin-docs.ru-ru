---
title: Интеграция Azure Active Directory B2C с мобильными приложениями Azure
description: Azure Active Directory B2C является это облаке решение управления удостоверениями для мобильных приложений и веб-потребитель. В этой статье показано, как использовать Azure Active Directory B2C для предоставления проверки подлинности и авторизации к экземпляру мобильных приложений Azure с помощью Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 53F52036-A997-4D0F-86B4-4302C6913136
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2019
ms.openlocfilehash: 2f9cfceee4765dea946dfdac974ac2d56595ef94
ms.sourcegitcommit: 9aaae4f0af096cd136b3dcfbb9af591ba307dc25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/26/2019
ms.locfileid: "67398696"
---
# <a name="integrating-azure-active-directory-b2c-with-azure-mobile-apps"></a>Интеграция Azure Active Directory B2C с мобильными приложениями Azure

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)

_Azure Active Directory B2C обеспечивает управление идентификаторами облака для клиентских приложений. В этой статье объясняется, как использовать Azure Active Directory B2C для интеграции управления удостоверениями в экземпляр мобильных приложений Azure с помощью Xamarin.Forms._

## <a name="overview"></a>Обзор

Мобильные приложения Azure позволяют разрабатывать приложения с помощью масштабируемой серверной части, размещенных в службе приложений Azure. Он поддерживает мобильная проверка подлинности, автономной синхронизации и принудительной уведомления. Дополнительные сведения о мобильных приложений Azure, см. в разделе [использование мобильное приложение Azure](~/xamarin-forms/data-cloud/consuming/azure.md), и [проверки подлинности пользователей с помощью мобильных приложений Azure](~/xamarin-forms/data-cloud/authentication/azure.md).

Azure Active Directory B2C — это служба управления удостоверениями для клиентских приложений, которые позволяет пользователю:

- Войдите, используя их существующих учетных записей социальных сетей (Microsoft, Google, Facebook, Amazon, LinkedIn).
- Создайте новые учетные данные (адрес электронной почты и пароль, или имя пользователя и пароль). Эти учетные данные, называются *локального* учетных записей.

Дополнительные сведения о Azure Active Directory B2C, см. в разделе [проверки подлинности пользователей с помощью Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Azure Active Directory B2C можно использовать для управления процессом проверки подлинности для мобильного приложения Azure. Этот подход настраивает Управление удостоверениями в облаке и позволяет вносить изменения без изменения кода приложения.


Мобильные приложения Azure с Azure Active Directory B2C обеспечивает два метода проверки подлинности:

- [Управляемая клиентом](#client-managed-authentication) — мобильное приложение Xamarin.Forms запускает процесс проверки подлинности в клиенте Azure Active Directory B2C и передает токен проверки подлинности полученных экземпляру мобильных приложений Azure.
- [Управляемая сервером](#server-managed-authentication) — экземпляр мобильных приложений Azure использует клиент Azure Active Directory B2C, чтобы начать процесс проверки подлинности через рабочий процесс веб-.

В обоих случаях процесс проверки подлинности является предоставленное клиентом Azure Active Directory B2C. В приложении-примере экрана входа в систему должен выглядеть снимках экрана ниже:

![](azure-ad-b2c-mobile-app-images/screenshots.png "Страница входа")

В этом примере показано войдите с помощью локальной учетной записи, но можно также включить Microsoft, Google, Facebook и других поставщиков удостоверений.

## <a name="setup"></a>Установка

В обоих рабочих процессов процесс начальной для интеграции с клиентом Azure Active Directory B2C с мобильными приложениями Azure выглядит следующим образом:

1. Создайте экземпляр мобильных приложений Azure на портале Azure. Это создает начальный проект аналогичную примера проекта. Дополнительные сведения см. в разделе [использование мобильное приложение Azure](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Создание клиента Azure Active Directory B2C. Дополнительные сведения см. в разделе [проверки подлинности пользователей с помощью Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).
1. Включите проверку подлинности в экземпляра мобильных приложений Azure и приложения Xamarin.Forms. Дополнительные сведения см. в разделе [проверки подлинности пользователей с помощью мобильных приложений Azure](~/xamarin-forms/data-cloud/authentication/azure.md).

> [!NOTE]
> Библиотеки аутентификации Майкрософт (MSAL) является обязательным при использовании рабочего процесса управляемая клиентом проверка подлинности. MSAL использует веб-браузере устройства для проверки подлинности. После завершения проверки подлинности в браузере пользователь перенаправляется на схему URL-адрес. Приложение регистрирует пользовательскую схему URL-адрес, что позволяет восстановить контроль над взаимодействие с пользователем и ответ на проверку подлинности. Дополнительные сведения об использовании MSAL для взаимодействия с клиентом Azure Active Directory B2C, см. в разделе [проверки подлинности пользователей с помощью Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

## <a name="client-managed-authentication"></a>Управляемая клиентом проверка подлинности

В управляемая клиентом проверка подлинности приложения Xamarin.Forms связывается с клиентом Azure Active Directory B2C, чтобы начать процесс проверки подлинности. После входа в клиент Azure Active Directory B2C возвращает токен удостоверения, которые задаются для экземпляра мобильных приложений Azure. Это позволяет приложению Xamarin.Forms для выполнения действий, которым требуются разрешения пользователя, прошедшего аутентификацию.

### <a name="azure-active-directory-b2c-client-managed-tenant-configuration"></a>Конфигурация клиента, которым управляет клиент Azure Active Directory B2C

Для рабочего процесса, управляемая клиентом проверка подлинности клиента Azure Active Directory B2C следует настроить следующим образом:

- Задайте **включить собственный клиент** значение «Да».
- Задайте пользовательский URI перенаправления. В документации MSAL рекомендуется с помощью «msal» в сочетании с идентификатор приложения и за которым следует «:/ / auth /». Дополнительные сведения см. в разделе [MSAL клиентские приложения URI перенаправления](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Client-Applications#redirect-uri).

Следующем снимке экрана показана эта конфигурация:

[![«Конфигурация azure Active Directory B2C»](azure-ad-b2c-mobile-app-images/azure-redirect-uri-cropped.png)](azure-ad-b2c-mobile-app-images/azure-redirect-uri.png#lightbox "конфигурации Azure Active Directory B2C")

Политики входа в систему, используемой в клиенте Azure Active Directory B2C также должны быть настроены так, что URL-адрес ответа ту же схему пользовательский URL-адрес. Это можно сделать, выбрав **выполнения потока пользователя** на портале Azure для доступа к параметрам политики. Сохраните URL-адрес метаданных, найденные на этом экране, он вам понадобится для настройки мобильного приложения. Следующий снимок экрана демонстрирует эту конфигурацию и URL-адрес, который следует скопировать:

[![«Azure Active Directory B2C политики»](azure-ad-b2c-mobile-app-images/client-flow-policies-cropped.png)](azure-ad-b2c-mobile-app-images/client-flow-policies.png#lightbox "конфигурация политики Azure Active Directory B2C")

### <a name="azure-mobile-app-configuration"></a>Настройка мобильного приложения Azure

Для рабочего процесса управляемая клиентом проверка подлинности настройте экземпляра мобильных приложений Azure следующим образом:

- Проверка подлинности службы приложений должно быть включено.
- Действие, выполняемое, когда запрос не прошел проверку подлинности должно быть присвоено **войдите с помощью Azure Active Directory**.

Следующем снимке экрана показана эта конфигурация:

[![«Конфигурация проверки подлинности мобильных приложений azure»](azure-ad-b2c-mobile-app-images/ama-config-cropped.png)](azure-ad-b2c-mobile-app-images/ama-config.png#lightbox "конфигурацию проверки подлинности мобильных приложений Azure")

Настройте экземпляр мобильных приложений Azure для взаимодействия с клиентом Azure Active Directory B2C.

- Щелкните конфигурацию Azure Active Directory и включить **Дополнительно** режим для поставщика проверки подлинности Azure Active Directory.
- Задайте **идентификатор клиента** для **идентификатор приложения** клиента Azure Active Directory B2C.
- Задайте **URL-адрес издателя** URL-адрес политики метаданных, скопированный ранее во время настройки конфигурации клиента.

Следующем снимке экрана показана эта конфигурация:

![«Мобильные приложения azure Расширенная конфигурация»](azure-ad-b2c-mobile-app-images/ama-advanced-config.png)

### <a name="signing-in"></a>Вход

В следующем примере кода показаны вызовы основной метод запуска рабочего процесса управляемая клиентом проверка подлинности:

```csharp
public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...

    authenticationResult = await ADB2CClient.AcquireTokenAsync(
        Constants.Scopes,
        string.Empty,
        UIBehavior.SelectAccount,
        string.Empty,
        App.UiParent);

    ...

    var payload = new JObject();
    if (authenticationResult != null && !string.IsNullOrWhiteSpace(authenticationResult.AccessToken))
    {
        payload["access_token"] = authenticationResult.AccessToken;
    }

    User = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory,
        payload);
    success = true;

    ...
}
```

Библиотеки аутентификации Майкрософт (MSAL) используется для запуска рабочего процесса проверки подлинности с клиентом Azure Active Directory B2C. `AcquireTokenAsync` Метод запускает веб-браузере устройства и отображает параметры проверки подлинности, определенные в политике Azure Active Directory B2C, который задается параметром политики, ссылка на через `Constants.AuthoritySignin` константы. Эта политика определяет интерфейс для входа в систему и регистрации пользователя и утверждения, которые приложение получает при успешной проверке подлинности.

Результат `AcquireTokenAsync` является вызов метода `AuthenticationResult` экземпляра. Если проверка подлинности успешна, `AuthenticationResult` экземпляра будет содержать маркер доступа, который будет кэширован локально. Если проверка подлинности завершилась неудачно, `AuthenticationResult` экземпляра будет содержать данные, который указывает на причину сбоя проверки подлинности. Сведения о том, как использовать MSAL для обмена данными с клиентом Azure Active Directory B2C, см. в разделе [проверки подлинности пользователей с помощью Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Когда `CurrentClient.LoginAsync` вызова метода, экземпляра мобильных приложений Azure получает маркер доступа, в оболочку `JObject`. Наличие допустимых маркеров означает, что экземпляр мобильных приложений Azure не нужно запустить свой собственный поток проверки подлинности OAuth 2.0. Вместо этого `CurrentClient.LoginAsync` возвращает `MobileServiceUser` экземпляр, который будет храниться в `MobileServiceClient.CurrentUser` свойство. Это свойство предоставляет `UserId` и `MobileServiceAuthenticationToken` свойства. Они представляют собой прошедшего проверку подлинности пользователя и маркер, который можно использовать до истечения срока его действия. Маркер проверки подлинности будут включены во все запросы к экземпляру мобильных приложений Azure, выполните действия, требующие разрешения пользователя, прошедшего проверку подлинности приложения Xamarin.Forms.

### <a name="signing-out"></a>Выход

В следующем примере кода показан способ вызова управляемую клиентом процесс выхода:

```csharp
public async Task<bool> LogoutAsync()
{
    ...

    IEnumerable<IAccount> accounts = await ADB2CClient.GetAccountsAsync();
    while (accounts.Any())
    {
        await ADB2CClient.RemoveAsync(accounts.First());
        accounts = await ADB2CClient.GetAccountsAsync();
    }
    User = null;

    ...
}
```

`CurrentClient.LogoutAsync` Метод отмены проверяет подлинность пользователя с экземпляром мобильных приложений Azure, а затем все токены проверки подлинности будут удалены из локального кэша, созданные MSAL.

## <a name="server-managed-authentication"></a>Управляемая сервером проверка подлинности

В управляемая сервером проверка подлинности приложения Xamarin.Forms связывается с экземпляром мобильных приложений Azure, который использует клиент Azure Active Directory B2C для управления потоком проверки подлинности OAuth 2.0, отображая страницу входа в систему, как определено в политике B2C. После успешного входа экземпляра мобильных приложений Azure возвращает токен, который позволяет выполнять действия, которым требуются разрешения пользователя, прошедшего аутентификацию в приложении Xamarin.Forms.

### <a name="azure-active-directory-b2c-server-managed-tenant-configuration"></a>Настройка сервера под управлением Azure Active Directory B2C

Для рабочего процесса, управляемая сервером проверка подлинности клиента Azure Active Directory B2C следует настроить следующим образом:

- Включить web app/веб-API, а также разрешить неявный поток.
- Значение URL-адрес ответа адрес мобильного приложения Azure, за которым следует `/.auth/login/aad/callback`.

Следующем снимке экрана показана эта конфигурация:

[![«Конфигурация azure Active Directory B2C»](azure-ad-b2c-mobile-app-images/server-flow-config-cropped.png)](azure-ad-b2c-mobile-app-images/server-flow-config.png#lightbox "конфигурации Azure Active Directory B2C")

Политику, используемую в клиенте Azure Active Directory B2C необходимо настроить URL-адрес ответа. Это достигается путем установки URL-адрес ответа в адрес мобильного приложения Azure, за которым следует `/.auth/login/aad/callback`. Следует также сохранить URL-адрес метаданных, находится в верхней части этого экрана, как он потребуется вам для настройки мобильного приложения. Следующий снимок экрана демонстрирует эту конфигурацию и URL-адрес метаданных, следует сохранить:

![«Настройка политики azure Active Directory B2C»](azure-ad-b2c-mobile-app-images/server-flow-policies.png)

### <a name="azure-mobile-apps-instance-configuration"></a>Настройка экземпляра Azure мобильных приложений

Для рабочего процесса, управляемая сервером проверка подлинности экземпляра мобильных приложений Azure следует настроить следующим образом:

- Проверка подлинности службы приложений должно быть включено.
- Действие, выполняемое, когда запрос не прошел проверку подлинности должно быть присвоено **войдите с помощью Azure Active Directory**.

Следующем снимке экрана показана эта конфигурация:

[![«Конфигурация проверки подлинности мобильных приложений azure»](azure-ad-b2c-mobile-app-images/ama-config-cropped.png)](azure-ad-b2c-mobile-app-images/ama-config.png#lightbox "конфигурацию проверки подлинности мобильных приложений Azure")

Экземпляр мобильных приложений Azure, также должны быть настроены для взаимодействия с клиентом Azure Active Directory B2C:

- Щелкните конфигурацию Azure Active Directory и включить **Дополнительно** режим для поставщика проверки подлинности Azure Active Directory.
- Задайте **идентификатор клиента** для **идентификатор приложения** клиента Azure Active Directory B2C.
- **URL-адрес издателя** является URL-адрес политики метаданных, скопированный ранее во время настройки конфигурации клиента.

Следующем снимке экрана показана эта конфигурация:

![«Мобильные приложения azure Расширенная конфигурация»](azure-ad-b2c-mobile-app-images/ama-advanced-config.png)

### <a name="signing-in"></a>Вход

В следующем примере кода показано, как запустить рабочий процесс управляемая сервером проверка подлинности:

```csharp
public async Task<bool> AuthenticateAsync()
{
    ...

    MobileServiceUser user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory,
        Constants.URLScheme);

    ...
}
```

Когда `CurrentClient.LoginAsync` вызова метода, экземпляра мобильных приложений Azure выполняет политику связанный Azure Active Directory B2C, которая запускает процесс проверки подлинности OAuth 2.0. Каждый `AuthenticateAsync` метод зависит от платформы. Тем не менее каждый `AuthenticateAsync` вызовы методов `CurrentClient.LoginAsync` метода и указывает, что клиент Azure Active Directory будет использоваться в процессе проверки подлинности. Дополнительные сведения см. в разделе [входе пользователя](~/xamarin-forms/data-cloud/authentication/azure.md#logging-in).

`CurrentClient.LoginAsync` Возвращает `MobileServiceUser` экземпляр, который будет храниться в `CurrentClient.CurrentUser` свойство. Это свойство предоставляет `UserId` и `MobileServiceAuthenticationToken` свойства. Они представляют собой прошедшего проверку подлинности пользователя и маркер проверки подлинности для пользователя, который можно использовать до истечения срока его действия. Маркер проверки подлинности будут включены во все запросы к экземпляру мобильных приложений Azure, позволяя приложения Xamarin.Forms для выполнения действий на экземпляре мобильных приложений Azure, требуются разрешения пользователя, прошедшего аутентификацию.

### <a name="signing-out"></a>Выход

В следующем примере кода показан способ вызова управляемого сервером процесс выхода:

```csharp
public async Task<bool> LogoutAsync()
{
    ...

    await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();

    ...
}
```

`MobileServiceClient.LogoutAsync` Метод отмены проверяет подлинность пользователя с экземпляром мобильных приложений Azure. Дополнительные сведения см. в разделе [ведение журнала ожидания пользователей](~/xamarin-forms/data-cloud/authentication/azure.md#logging-out).


## <a name="related-links"></a>Связанные ссылки

- [TodoAzureAuth ClientFlow (пример)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)
- [TodoAzureAuth ServerFlow (пример)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CServerFlow/)
- [Использование мобильного приложения Azure](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Проверка подлинности пользователей с мобильными приложениями Azure](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Проверка подлинности пользователей с Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)
- [Пакет nuget библиотеки проверки подлинности Майкрософт](https://www.nuget.org/packages/Microsoft.Identity.Client)
- [Документация по библиотеке проверки подлинности Майкрософт](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)
