---
title: Идентификатор Touch и идентификатор лица с Xamarin. iOS
description: В этом документе представлено обобщенное описание биометрической проверки подлинности в iOS.
ms.prod: xamarin
ms.assetid: 4BC8EFD6-52FC-4793-BA69-D6BFF850FE5F
ms.technology: xamarin-ios
author: profexorgeek
ms.author: jusjohns
ms.date: 12/16/2019
ms.openlocfilehash: 744a07343b9da87f196c0664f57b7d950844ab04
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75490430"
---
# <a name="use-touch-id-and-face-id-with-xamarinios"></a>Использование Touch ID и идентификатора лица с Xamarin. iOS

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-faceidsample/)

iOS поддерживает две системы биометрической проверки подлинности:

1. **Touch ID** использует датчик отпечатков пальцев под кнопкой "Главная".
1. Для проверки подлинности пользователей с помощью проверки лица в **идентификаторе лица** используются датчики передней камеры.

Touch ID появился в iOS 7 и идентификатор лица в iOS 11.

Эти системы проверки подлинности полагаются на аппаратный процессор безопасности, именуемый _Secure анклава_. Безопасный анклава отвечает за шифрование математических представлений данных лиц и отпечатков пальцев, а также проверку подлинности пользователей с помощью этих сведений. В соответствии с Apple, лица и данные отпечатков пальцев не оставляют устройства и не архивируются в iCloud. Приложения взаимодействуют с безопасным анклава через API _локальной проверки подлинности_ и не могут получить данные о лицах или отпечатки пальца или получить прямой доступ к безопасному анклава.

Идентификатор касания и идентификатор лица могут использоваться приложениями для проверки подлинности пользователя перед предоставлением доступа к защищенному содержимому.

## <a name="local-authentication-context"></a>Контекст локальной проверки подлинности

Биометрическая проверка подлинности в iOS основана на объекте _контекста локальной проверки подлинности_ , который является экземпляром класса `LAContext`. Класс `LAContext` позволяет:

- Проверьте доступность биометрического оборудования.
- Оцените политики проверки подлинности.
- Оцените элементы управления доступом.
- Настройка и отображение запросов проверки подлинности.
- Повторное использование или непроверка состояния проверки подлинности.
- Управление учетными данными.

## <a name="detect-available-authentication-methods"></a>Определение доступных методов проверки подлинности

Пример проекта включает `AuthenticationView`, поддерживаемый `AuthenticationViewController`. Этот класс переопределяет метод `ViewWillAppear` для обнаружения доступных методов проверки подлинности:

```csharp
partial class AuthenticationViewController: UIViewController
{
    // ...
    string BiometryType = "";

    public override void ViewWillAppear(bool animated)
    {
        base.ViewWillAppear(animated);
        unAuthenticatedLabel.Text = "";
    
        var context = new LAContext();
        var buttonText = "";

        // Is login with biometrics possible?
        if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, out var authError1))
        {
            // has Touch ID or Face ID
            if (UIDevice.CurrentDevice.CheckSystemVersion(11, 0))
            {
                context.LocalizedReason = "Authorize for access to secrets"; // iOS 11
                BiometryType = context.BiometryType == LABiometryType.TouchId ? "Touch ID" : "Face ID";
                buttonText = $"Login with {BiometryType}";
            }
            // No FaceID before iOS 11
            else
            {
                buttonText = $"Login with Touch ID";
            }
        }

        // Is pin login possible?
        else if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthentication, out var authError2))
        {
            buttonText = $"Login"; // with device PIN
            BiometryType = "Device PIN";
        }

        // Local authentication not possible
        else
        {
            // Application might choose to implement a custom username/password
            buttonText = "Use unsecured";
            BiometryType = "none";
        }
        AuthenticateButton.SetTitle(buttonText, UIControlState.Normal);
    }
}
```

Метод `ViewWillAppear` вызывается, когда пользовательский интерфейс будет отображаться для пользователя. Этот метод определяет новый экземпляр `LAContext` и использует метод `CanEvaluatePolicy`, чтобы определить, включена ли биометрическая проверка подлинности. Если да, то он проверяет версию системы и `BiometryType` перечисление, чтобы определить, какие параметры биометрической метрики доступны.

Если биометрическая проверка подлинности не включена, приложение пытается вернуться к подлинности ПИН-кода. Если ни одна из биометрических или ПИН-кодов не доступна, владелец устройства не включил функции безопасности и содержимое не может быть защищено с помощью локальной проверки подлинности.

## <a name="authenticate-a-user"></a>Проверка подлинности пользователя

`AuthenticationViewController` в примере проекта включает метод `AuthenticateMe`, который отвечает за проверку подлинности пользователя:

```csharp
partial class AuthenticationViewController: UIViewController
{
    // ...
    string BiometryType = "";

    partial void AuthenticateMe(UIButton sender)
    {
        var context = new LAContext();
        NSError AuthError;
        var localizedReason = new NSString("To access secrets");
    
        // Because LocalAuthentication APIs have been extended over time,
        // you must check iOS version before setting some properties
        context.LocalizedFallbackTitle = "Fallback";
    
        if (UIDevice.CurrentDevice.CheckSystemVersion(10, 0))
        {
            context.LocalizedCancelTitle = "Cancel";
        }
        if (UIDevice.CurrentDevice.CheckSystemVersion(11, 0))
        {
            context.LocalizedReason = "Authorize for access to secrets";
            BiometryType = context.BiometryType == LABiometryType.TouchId ? "TouchID" : "FaceID";
        }
    
        // Check if biometric authentication is possible
        if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, out AuthError))
        {
            replyHandler = new LAContextReplyHandler((success, error) =>
            {
                // This affects UI and must be run on the main thread
                this.InvokeOnMainThread(() =>
                {
                    if (success)
                    {
                        PerformSegue("AuthenticationSegue", this);
                    }
                    else
                    {
                        unAuthenticatedLabel.Text = $"{BiometryType} Authentication Failed";
                    }
                });
    
            });
            context.EvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, localizedReason, replyHandler);
        }

        // Fall back to PIN authentication
        else if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthentication, out AuthError))
        {
            replyHandler = new LAContextReplyHandler((success, error) =>
            {
                // This affects UI and must be run on the main thread
                this.InvokeOnMainThread(() =>
                {
                    if (success)
                    {
                        PerformSegue("AuthenticationSegue", this);
                    }
                    else
                    {
                        unAuthenticatedLabel.Text = "Device PIN Authentication Failed";
                        AuthenticateButton.Hidden = true;
                    }
                });
    
            });
            context.EvaluatePolicy(LAPolicy.DeviceOwnerAuthentication, localizedReason, replyHandler);
        }

        // User hasn't configured any authentication: show dialog with options
        else
        {
            unAuthenticatedLabel.Text = "No device auth configured";
            var okCancelAlertController = UIAlertController.Create("No authentication", "This device does't have authentication configured.", UIAlertControllerStyle.Alert);
            okCancelAlertController.AddAction(UIAlertAction.Create("Use unsecured", UIAlertActionStyle.Default, alert => PerformSegue("AuthenticationSegue", this)));
            okCancelAlertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, alert => Console.WriteLine("Cancel was clicked")));
            PresentViewController(okCancelAlertController, true, null);
        }
    } 
}
```

Метод `AuthenticateMe` вызывается в ответ на нажатие пользователем кнопки **входа** . Создается новый объект `LAContext` и проверяется версия устройства, чтобы определить, какие свойства следует задать в контексте локальной проверки подлинности.

Метод `CanEvaluatePolicy` вызывается, чтобы проверить, включена ли биометрическая проверка подлинности, когда это возможно, вернуться к проверке подлинности, а затем предложить незащищенный режим, если проверка подлинности недоступна. Если доступен метод проверки подлинности, то для отображения пользовательского интерфейса и завершения процесса проверки подлинности используется метод `EvaluatePolicy`.

Пример проекта содержит макеты данных и представление для отображения данных, если проверка подлинности прошла успешно.

## <a name="related-links"></a>Связанные ссылки

- [Пример локальной аутентификации с использованием сенсорного идентификатора или идентификатора лица](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-faceidsample/)
- [Сведения об Touch ID](https://support.apple.com/en-us/HT204587) в support.Apple.com
- [О идентификаторе лица](https://support.apple.com/en-us/HT208108) в support.Apple.com
