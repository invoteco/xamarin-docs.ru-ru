---
title: Вход с помощью Apple в Xamarin. iOS
description: При использовании сторонних служб аутентификации Войдите в систему с помощью Apple.
ms.prod: xamarin
ms.assetid: CDA59BBF-AAE1-4D4F-B4AE-DB37220D41EB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/10/2019
ms.openlocfilehash: ae4cccc900396c7ebd6e737160e38c5e9dcdc74e
ms.sourcegitcommit: cf56d2bae34dc0f8e94c2d3d28d5f460d59807bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2019
ms.locfileid: "70986168"
---
# <a name="sign-in-with-apple-in-xamarinios"></a>Вход с помощью Apple в Xamarin. iOS

![Сейчас этот API находится в предварительной версии](~/media/shared/preview.png)

Вход с помощью Apple — это новая служба, которая обеспечивает защиту идентификации для пользователей сторонних служб проверки подлинности. Начиная с iOS 13, Apple требует, чтобы любое новое приложение, использующее сторонние службы проверки подлинности, также предпредоставило вход с помощью Apple. При обновлении существующих приложений не требуется добавлять вход с помощью Apple до апреля 2020.

В этом документе рассказывается, как можно добавить вход с помощью приложений Apple в iOS 13.

## <a name="requirements"></a>Требования

Для этого компонента требуется:
* iOS 13
* Xamarin. iOS 12,99
* Visual Studio 2019 или Visual Studio 2019 для Mac с поддержкой Xcode 11. 
 
Дополнительные сведения см. в разделе [Приступая к работе](get-started.md) .

## <a name="apple-developer-setup"></a>Установка для разработчиков Apple

Перед сборкой и запуском приложения с помощью функции входа с Apple необходимо выполнить следующие действия. В [сертификатах разработчика Apple на портале & профили идентификаторов][5] :

1. Создайте идентификатор нового **приложения** .
2. Задайте описание в поле **Описание** .
3. Выберите **явный** идентификатор пакета и задайте `com.xamarin.AddingTheSignInWithAppleFlowToYourApp` его в поле.
4. Включите **Вход с помощью функции Apple** и зарегистрируйте новое удостоверение.
5. Создайте новый профиль подготовки с новым удостоверением.
6. Скачайте и установите его на устройстве.
7. В Visual Studio включите **Вход с помощью возможности Apple** в файле прав **. plist** .

## <a name="check-sign-in-status"></a>Проверка состояния входа

Когда приложение запустится или сначала необходимо проверить состояние проверки подлинности пользователя, создайте экземпляр `ASAuthorizationAppleIdProvider` и проверьте текущее состояние:

```csharp
var appleIdProvider = new ASAuthorizationAppleIdProvider ();
appleIdProvider.GetCredentialState (KeychainItem.CurrentUserIdentifier, (credentialState, error) => {
    switch (credentialState) {
    case ASAuthorizationAppleIdProviderCredentialState.Authorized:
        // The Apple ID credential is valid.
        break;
    case ASAuthorizationAppleIdProviderCredentialState.Revoked:
        // The Apple ID credential is revoked.
        break;
    case ASAuthorizationAppleIdProviderCredentialState.NotFound:
        // No credential was found, so show the sign-in UI.
        InvokeOnMainThread (() => {
            var storyboard = UIStoryboard.FromName ("Main", null);

            if (!(storyboard.InstantiateViewController (nameof (LoginViewController)) is LoginViewController viewController))
                return;

            viewController.ModalPresentationStyle = UIModalPresentationStyle.FormSheet;
            viewController.ModalInPresentation = true;
            Window?.RootViewController?.PresentViewController (viewController, true, null);
        });
        break;
    }
});
```

В этом коде, вызванном `FinishedLaunching` во время `AppDelegate.cs`в, приложение будет `NotFound` работать, когда `LoginViewController` состояние и представляется пользователю. Если состояние возвращает `Authorized` значение или `Revoked`, пользователю может быть предоставлено другое действие.

## <a name="a-loginviewcontroller-for-sign-in-with-apple"></a>Логинвиевконтроллер для входа с помощью Apple

Класс, реализующий логику входа в систему и предлагающий вход с `IASAuthorizationControllerDelegate` помощью Apple, должен `LoginViewController` реализовать и `IASAuthorizationControllerPresentationContextProviding` , как в примере ниже. `UIViewController`

```csharp
public partial class LoginViewController : UIViewController, IASAuthorizationControllerDelegate, IASAuthorizationControllerPresentationContextProviding {
    public LoginViewController (IntPtr handle) : base (handle)
    {
    }

    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
        // Perform any additional setup after loading the view, typically from a nib.

        SetupProviderLoginView ();
    }

    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);

        PerformExistingAccountSetupFlows ();
    }

    void SetupProviderLoginView ()
    {
        var authorizationButton = new ASAuthorizationAppleIdButton (ASAuthorizationAppleIdButtonType.Default, ASAuthorizationAppleIdButtonStyle.White);
        authorizationButton.TouchUpInside += HandleAuthorizationAppleIDButtonPress;
        loginProviderStackView.AddArrangedSubview (authorizationButton);
    }

    // Prompts the user if an existing iCloud Keychain credential or Apple ID credential is found.
    void PerformExistingAccountSetupFlows ()
    {
        // Prepare requests for both Apple ID and password providers.
        ASAuthorizationRequest [] requests = {
            new ASAuthorizationAppleIdProvider ().CreateRequest (),
            new ASAuthorizationPasswordProvider ().CreateRequest ()
        };

        // Create an authorization controller with the given requests.
        var authorizationController = new ASAuthorizationController (requests);
        authorizationController.Delegate = this;
        authorizationController.PresentationContextProvider = this;
        authorizationController.PerformRequests ();
    }

    private void HandleAuthorizationAppleIDButtonPress (object sender, EventArgs e)
    {
        var appleIdProvider = new ASAuthorizationAppleIdProvider ();
        var request = appleIdProvider.CreateRequest ();
        request.RequestedScopes = new [] { ASAuthorizationScope.Email, ASAuthorizationScope.FullName };

        var authorizationController = new ASAuthorizationController (new [] { request });
        authorizationController.Delegate = this;
        authorizationController.PresentationContextProvider = this;
        authorizationController.PerformRequests ();
    }
}
```

![Анимация примера приложения с помощью функции входа с Apple](sign-in-images/sign-in-flow.png)

Этот пример кода проверяет текущее состояние входа в `PerformExistingAccountSetupFlows` систему и подключается к текущему представлению в качестве делегата. При обнаружении существующих учетных данных для цепочки ключей iCloud или Apple ID пользователю будет предложено использовать его.

Компания Apple `ASAuthorizationAppleIdButton`предоставляет кнопку специально для этой цели. При затронутой кнопке запускается рабочий процесс, обрабатываемый в `HandleAuthorizationAppleIDButtonPress`методе.

## <a name="handling-authorization"></a>Обработка авторизации

`IASAuthorizationController` В реализуется любая пользовательская логика для хранения учетной записи пользователя. В примере ниже хранится учетная запись пользователя в связке ключей, собственная служба хранилища Apple.

```csharp
#region IASAuthorizationController Delegate

[Export ("authorizationController:didCompleteWithAuthorization:")]
public void DidComplete (ASAuthorizationController controller, ASAuthorization authorization)
{
    if (authorization.GetCredential<ASAuthorizationAppleIdCredential> () is ASAuthorizationAppleIdCredential appleIdCredential) {
        var userIdentifier = appleIdCredential.User;
        var fullName = appleIdCredential.FullName;
        var email = appleIdCredential.Email;

        // Create an account in your system.
        // For the purpose of this demo app, store the userIdentifier in the keychain.
        try {
            new KeychainItem ("com.example.apple-samplecode.juice", "userIdentifier").SaveItem (userIdentifier);
        } catch (Exception) {
            Console.WriteLine ("Unable to save userIdentifier to keychain.");
        }

        // For the purpose of this demo app, show the Apple ID credential information in the ResultViewController.
        if (!(PresentingViewController is ResultViewController viewController))
            return;

        InvokeOnMainThread (() => {
            viewController.UserIdentifierText = userIdentifier;
            viewController.GivenNameText = fullName?.GivenName ?? "";
            viewController.FamilyNameText = fullName?.FamilyName ?? "";
            viewController.EmailText = email ?? "";

            DismissViewController (true, null);
        });
    } else if (authorization.GetCredential<ASPasswordCredential> () is ASPasswordCredential passwordCredential) {
        // Sign in using an existing iCloud Keychain credential.
        var username = passwordCredential.User;
        var password = passwordCredential.Password;

        // For the purpose of this demo app, show the password credential as an alert.
        InvokeOnMainThread (() => {
            var message = $"The app has received your selected credential from the keychain. \n\n Username: {username}\n Password: {password}";
            var alertController = UIAlertController.Create ("Keychain Credential Received", message, UIAlertControllerStyle.Alert);
            alertController.AddAction (UIAlertAction.Create ("Dismiss", UIAlertActionStyle.Cancel, null));

            PresentViewController (alertController, true, null);
        });
    }
}

[Export ("authorizationController:didCompleteWithError:")]
public void DidComplete (ASAuthorizationController controller, NSError error)
{
    Console.WriteLine (error);
}

#endregion
```

## <a name="authorization-controller"></a>Контроллер авторизации

Последняя часть этой реализации — это компонент, `ASAuthorizationController` который управляет запросами авторизации для поставщика.

```csharp
#region IASAuthorizationControllerPresentation Context Providing

public UIWindow GetPresentationAnchor (ASAuthorizationController controller) => View.Window;

#endregion
```

## <a name="summary"></a>Сводка

В этой статье впервые появился вход с Apple для iOS. 

## <a name="related-links"></a>Связанные ссылки

* [Вход с использованием рекомендаций Apple](https://developer.apple.com/design/human-interface-guidelines/sign-in-with-apple/overview/)
* [Войдите с помощью прав Apple.][2]
* [ВВДК 2019 сеанс 706: Введение в вход с помощью Apple.][3]
* [Настройка входа с помощью Apple для Xamarin. Forms][4]

[1]: https://developer.apple.com/documentation/authenticationservices/adding_the_sign_in_with_apple_flow_to_your_app
[2]: https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_applesignin
[3]: https://developer.apple.com/videos/play/wwdc19/706/
[4]: ~/xamarin-forms/platform/sign-in-with-apple/setup.md
[5]: https://developer.apple.com/account/resources/identifiers/list