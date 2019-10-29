---
title: Push-уведомления в iOS
description: В этом документе описывается работа с Push-уведомлениями в iOS 9 и более ранних версиях. Здесь обсуждаются сертификаты, регистрация в службе шлюза push-уведомлений Apple (APNS) и многое другое.
ms.prod: xamarin
ms.assetid: 64B3BE6A-A3E2-4B1B-95ED-02D27A8FDAAC
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 468d0e16a3bd5745a243b2d7c09e642e3aeffd1d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031371"
---
# <a name="push-notifications-in-ios"></a>Push-уведомления в iOS

> [!IMPORTANT]
> Сведения в этом разделе относятся к iOS 9 и более ранним версиям, поэтому она оставлена для поддержки более старых версий iOS. Для iOS 10 и более поздних версий ознакомьтесь с [руководством по платформе пользовательских уведомлений](~/ios/platform/user-notifications/index.md) для поддержки локальных и удаленных уведомлений на устройстве iOS.

Push-уведомления должны храниться в кратком виде и содержать достаточно данных, чтобы уведомить мобильное приложение о том, что оно должно связаться с серверным приложением для обновления. Например, когда приходит новое сообщение электронной почты, серверное приложение уведомляет только мобильное приложение о поступлении нового сообщения. Уведомление не будет содержать новое электронное сообщение. Затем мобильное приложение будет получать новые сообщения с сервера, когда это было уместно

В центре push-уведомлений в iOS используется *Служба шлюза push-уведомлений Apple (APNs)* . Это служба, предоставляемая компанией Apple, которая отвечает за маршрутизацию уведомлений с сервера приложений на устройства iOS.
На следующем рисунке показана топология push-уведомлений для iOS.![](remote-notifications-in-ios-images/image4.png "На этом рисунке показана топология push-уведомлений для iOS")

Удаленные уведомления — это строки в формате JSON, которые соответствуют формату и протоколам, указанным в разделе [полезные данные уведомления](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CreatingtheNotificationPayload.html#//apple_ref/doc/uid/TP40008194-CH10-SW1) [руководства по программированию локальных и push-уведомлений](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) в [документации разработчика iOS. ](https://developer.apple.com/devcenter/ios/index.action).

Apple поддерживает две среды APNS: *"песочницу" и "* *Рабочая* среда". Среда "песочницы" предназначена для тестирования на этапе разработки, и ее можно найти по адресу `gateway.sandbox.push.apple.com` через TCP-порт 2195. Рабочая среда предназначена для использования в развернутых приложениях, и ее можно найти по адресу `gateway.push.apple.com` через TCP-порт 2195.

## <a name="requirements"></a>Требования

Push-уведомление должно соблюдать следующие правила, определяемые архитектурой APNS:

- **256 байт сообщений** — размер всего сообщения в уведомлении не должен превышать 256 байт.
- **Без подтверждения поступления** — APNs не предоставляет отправителю уведомление о том, что сообщение было отправлено предполагаемому получателю. Если устройство недоступно и отправлено несколько последовательных уведомлений, все уведомления, кроме самого последнего, будут потеряны. На устройство будет доставлено только самое последнее уведомление.
- Для **каждого приложения требуется защищенный сертификат** . связь с APNs должна выполняться по протоколу SSL.

## <a name="creating-and-using-certificates"></a>Создание и использование сертификатов

Каждая из сред, упомянутых в предыдущем разделе, требует собственного сертификата. В этом разделе рассматривается создание сертификата, связывание его с профилем подготовки, а затем получение сертификата обмена личной информацией для использования с Пушшарп.

1. Чтобы создать сертификаты, перейдите на портал подготовки iOS на веб-сайте Apple, как показано на следующем снимке экрана (Обратите внимание на пункт меню идентификаторы приложений слева):

    [![](remote-notifications-in-ios-images/image5new.png "The iOS Provisioning Portal on Apples website")](remote-notifications-in-ios-images/image5new.png#lightbox)

2. Затем перейдите к разделу идентификатор приложения и создайте новый идентификатор приложения, как показано на следующем снимке экрана:

    [![](remote-notifications-in-ios-images/image6new.png "Navigate to the App IDs section and create a new app ID")](remote-notifications-in-ios-images/image6new.png#lightbox)

3. При нажатии кнопки **+** вы сможете ввести описание и идентификатор пакета для идентификатора приложения, как показано на следующем снимке экрана:

    [![](remote-notifications-in-ios-images/image7new.png "Enter the description and a Bundle Identifier for the app ID")](remote-notifications-in-ios-images/image7new.png#lightbox)

4. Обязательно выберите **ЯВНЫЙ идентификатор приложения** и убедитесь, что идентификатор пакета не заканчивается `*`. При этом будет создан идентификатор, который подходит для нескольких приложений, а для одного приложения должны быть созданы сертификаты push-уведомлений.

5. В разделе службы приложений выберите **Push-уведомления**.

    [![](remote-notifications-in-ios-images/image8new.png "Select Push Notifications")](remote-notifications-in-ios-images/image8new.png#lightbox)

6. И нажмите кнопку **Отправить** , чтобы подтвердить регистрацию нового идентификатора приложения:

    [![](remote-notifications-in-ios-images/image9new.png "Confirm registration of the new App ID")](remote-notifications-in-ios-images/image9new.png#lightbox)

7. Затем необходимо создать сертификат для идентификатора приложения. В левой области навигации перейдите к пункту **сертификаты > все** и нажмите кнопку `+`, как показано на следующем снимке экрана:

    [![](remote-notifications-in-ios-images/image10new.png "Create the certificate for the app ID")](remote-notifications-in-ios-images/image8.png#lightbox)

8. Укажите, хотите ли вы использовать сертификат разработки или рабочей среды:

    [![](remote-notifications-in-ios-images/image11new.png "Select a Development or Production certificate")](remote-notifications-in-ios-images/image11new.png#lightbox)

9. Затем выберите новый идентификатор приложения, который мы только что создали:

    [![](remote-notifications-in-ios-images/image12new.png "Select the new App ID just created")](remote-notifications-in-ios-images/image12new.png#lightbox)

10. При этом будут отображены инструкции по созданию *запроса подписи сертификата* с помощью приложения для **доступа к цепочке ключей** на компьютере Mac.

11. Теперь, когда сертификат создан, он должен быть использован в рамках процесса сборки для подписи приложения, чтобы его можно было зарегистрировать с помощью APNs. Для этого требуется создать и установить профиль подготовки, использующий сертификат.

12. Чтобы создать профиль подготовки для разработки, перейдите к разделу **профили подготовки** и выполните действия по его созданию, используя только что созданный идентификатор приложения.

13. Создав профиль подготовки, откройте **Организатор Xcode** и обновите его. Если созданный профиль подготовки не отображается, может потребоваться Скачать профиль на портале подготовки iOS и импортировать его вручную. На следующем снимке экрана показан пример организатора с добавленным профилем инициализации:  
    [![](remote-notifications-in-ios-images/image13new.png "This screen shot shows an example of the Organizer with the provision profile added")](remote-notifications-in-ios-images/image13new.png#lightbox)

14. На этом этапе необходимо настроить проект Xamarin. iOS для использования только что созданного профиля подготовки. Это делается в диалоговом окне " **Параметры проекта** " на вкладке " **Подписывание пакета iOS** ", как показано на следующем снимке экрана:  
    [![](remote-notifications-in-ios-images/image11.png "Configure the Xamarin.iOS project to use this newly created provisioning profile")](remote-notifications-in-ios-images/image11.png#lightbox)

На этом этапе приложение настроено для работы с Push-уведомлениями. Однако с сертификатом по-прежнему требуется еще несколько шагов. Этот сертификат имеет формат DER, несовместимый с Пушшарп, для которого требуется сертификат обмена личной информацией (PKCS12). Чтобы преобразовать сертификат, чтобы он можно было использовать с помощью Пушшарп, выполните следующие заключительные действия.

1. **Скачайте файл сертификата** — вход на портал подготовки iOS, перейдите на вкладку Сертификаты, выберите сертификат, связанный с правильным профилем подготовки, и щелкните **скачать** .
1. **Открыть доступ к цепочке ключей** — это приложение является интерфейсом GUI для системы управления паролями в OS X.
1. **Импортируйте сертификат** — если сертификат еще не установлен, **файл... Импорт элементов** из меню доступа к цепочке ключей. Перейдите к сертификату, экспортированному ранее, и выберите его.
1. **Экспорт сертификата** . Разверните сертификат, чтобы был виден связанный закрытый ключ, щелкните его правой кнопкой мыши и выберите пункт Экспорт. Вам будет предложено ввести имя файла и пароль для экспортированного файла.

На этом этапе мы закончили работу с сертификатами. Мы создали сертификат, который будет использоваться для подписывания приложений iOS и преобразования этого сертификата в формат, который можно использовать с Пушшарп в серверном приложении. Теперь давайте посмотрим, как приложения iOS взаимодействуют с APNS.

## <a name="registering-with-apns"></a>Регистрация с помощью APNS

Прежде чем приложение iOS сможет получить удаленное уведомление, оно должно быть зарегистрировано в APNS. APNS создаст уникальный маркер устройства и вернет его в приложение iOS. Затем приложение iOS выберет маркер устройства, а затем зарегистрирует его на сервере приложений. После того как все это случится, регистрация завершается, и сервер приложений может отправлять уведомления на мобильное устройство.

Теоретически маркер устройства может изменяться каждый раз, когда приложение iOS регистрируется в APNS, однако на практике это не случается часто. В качестве оптимизации приложение может кэшировать самый последний маркер устройства и обновлять сервер приложений только при его изменении. На следующей схеме показан процесс регистрации и получения маркера устройства.

 ![](remote-notifications-in-ios-images/image12.png "This diagram illustrates the process of registration and obtaining a device token")

Регистрация с помощью APNS обрабатывается в методе `FinishedLaunching` класса делегата приложения путем вызова `RegisterForRemoteNotificationTypes` для текущего объекта `UIApplication`. Когда приложение iOS регистрируется с помощью APNS, оно также должно указывать типы удаленных уведомлений, которые требуется получать. Эти удаленные типы уведомлений объявляются в `UIRemoteNotificationType`перечисления. В следующем фрагменте кода приведен пример того, как приложение iOS может зарегистрироваться для получения уведомлений об удаленных оповещениях и индикаторах.

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
    var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());

    UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
    UIApplication.SharedApplication.RegisterForRemoteNotifications ();
} else {
    UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
    UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
}
```

Запрос регистрации APNS выполняется в фоновом режиме. когда получен ответ, iOS вызывает метод `RegisteredForRemoteNotifications` в классе `AppDelegate` и передает зарегистрированный маркер устройства. Маркер будет содержаться в объекте `NSData`. В следующем фрагменте кода показано, как получить маркер устройства, предоставленный APNS:

```csharp
public override void RegisteredForRemoteNotifications (
UIApplication application, NSData deviceToken)
{
    // Get current device token
    var DeviceToken = deviceToken.Description;
    if (!string.IsNullOrWhiteSpace(DeviceToken)) {
        DeviceToken = DeviceToken.Trim('<').Trim('>');
    }

    // Get previous device token
    var oldDeviceToken = NSUserDefaults.StandardUserDefaults.StringForKey("PushDeviceToken");

    // Has the token changed?
    if (string.IsNullOrEmpty(oldDeviceToken) || !oldDeviceToken.Equals(DeviceToken))
    {
        //TODO: Put your own logic here to notify your server that the device token has changed/been created!
    }

    // Save new device token
    NSUserDefaults.StandardUserDefaults.SetString(DeviceToken, "PushDeviceToken");
}
```

Если по какой-либо причине не удается выполнить регистрацию (например, устройство не подключено к Интернету), то iOS вызывает `FailedToRegisterForRemoteNotifications` для класса делегата приложения. В следующем фрагменте кода показано, как отобразить предупреждение, информирующее пользователя о том, что произошел сбой регистрации:

```csharp
public override void FailedToRegisterForRemoteNotifications (UIApplication application , NSError error)
{
    new UIAlertView("Error registering push notifications", error.LocalizedDescription, null, "OK", null).Show();
}
```

### <a name="device-token-housekeeping"></a>Служебная заметка устройства

Срок действия маркеров устройства истекает или изменяется с течением времени. По этой причине серверные приложения должны выполнить некоторую очистку и очистить срок действия или изменения маркеров. Когда приложение отправляет в виде push-уведомления на устройство с просроченным токеном, APNS записывает и сохраняет этот токен с истекшим сроком действия. Затем серверы могут запросить APNS, чтобы узнать, какие токены устарели.

APNS используется для предоставления *службы обратной связи* — конечная точка HTTPS, которая проходит проверку подлинности через сертификат, созданный для отправки push-уведомлений, и отправляет обратно данные о сроках действия маркеров. Она устарела Apple и удалена.

Вместо этого существует новый код состояния HTTP для варианта, который ранее был передан службой обратной связи:

> 410-маркер устройства больше не активен для раздела.

Кроме того, новый ключ данных JSON `timestamp` будет находиться в тексте ответа:

> Если значение в заголовке: Status равно 410, значение этого параметра является последним моментом, когда APNs подтвердил, что маркер устройства стал недействительным для раздела.
>
> Отключите отправку уведомлений, пока устройство не зарегистрирует маркер с последующей меткой времени в поставщике.

## <a name="summary"></a>Сводка

В этом разделе представлены основные понятия, связанные с Push-уведомлениями в iOS. В нем объяснена роль службы шлюза push-уведомлений Apple (APNS). Затем мы рассмотрели создание и использование сертификатов безопасности, необходимых для APNS. Наконец, этот документ завершился с обсуждением того, как серверы приложений могут использовать *службы отзывов* для прекращения отслеживания токенов устройств с истекшим сроком действия.

## <a name="related-links"></a>Связанные ссылки

- [Уведомления. Демонстрация локальных и удаленных уведомлений (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/notifications)
- [Локальные и Push-уведомления для разработчиков](https://developer.apple.com/notifications/)
- [UIApplication](https://docs.microsoft.com/dotnet/api/uikit.uiapplication)
- [уиремотенотификатионтипе](https://docs.microsoft.com/dotnet/api/uikit.UIRemoteNotificationType)
