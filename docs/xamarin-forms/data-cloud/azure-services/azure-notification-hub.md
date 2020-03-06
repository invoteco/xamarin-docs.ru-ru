---
title: Отправка и получение push-уведомлений в центрах уведомлений Azure и Xamarin. Forms
description: В этой статье объясняется, как использовать центры уведомлений Azure для отправки push-уведомлений между платформами в приложения Xamarin. Forms.
ms.prod: xamarin
ms.assetid: 07D13195-3A0D-4C95-ACF0-143A9084973C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 11/27/2019
ms.openlocfilehash: 778f56ec844e2802c1e1bc783824d55218678761
ms.sourcegitcommit: e9d88587aafc912124b87732d81c3910247ad811
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2020
ms.locfileid: "78337292"
---
# <a name="send-and-receive-push-notifications-with-azure-notification-hubs-and-xamarinforms"></a>Отправка и получение push-уведомлений в центрах уведомлений Azure и Xamarin. Forms

[Пример загрузки ![](~/media/shared/download.png)скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azurenotificationhub/)

Push-уведомления доставляют данные из серверной системы в мобильное приложение. У Apple, Google и других платформ есть собственная служба push-уведомлений (PNS). Центры уведомлений Azure позволяют централизовать уведомления на разных платформах, чтобы ваше серверное приложение могли взаимодействовать с одним концентратором, что занимается распространением уведомлений для каждой конкретной платформы PNS.

Интегрируйте центры уведомлений Azure в мобильные приложения, выполнив следующие действия.

1. [Настройка Push-Notification Services и центра уведомлений Azure](#set-up-push-notification-services-and-azure-notification-hub).
1. [Узнайте, как использовать шаблоны и теги](#register-templates-and-tags-with-the-azure-notification-hub).
1. [Создание кросс-платформенного приложения Xamarin. Forms](#xamarinforms-application-functionality).
1. [Настройте собственный проект Android для push-уведомлений](#configure-the-android-application-for-notifications).
1. [Настройка собственного проекта iOS для push-уведомлений](#configure-ios-for-notifications).
1. [Тестовые уведомления с помощью центра уведомлений Azure](#test-notifications-in-the-azure-portal).
1. [Создайте серверное приложение для отправки уведомлений](#create-a-notification-dispatcher).

> [!NOTE]
> Если у вас еще нет [подписки Azure](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing), создайте [бесплатную учетную запись Azure](https://aka.ms/azfree-docs-mobileapps), прежде чем начать работу.

## <a name="set-up-push-notification-services-and-azure-notification-hub"></a>Настройка Push-Notification Services и центра уведомлений Azure

Интеграция концентраторов уведомлений Azure с мобильным приложением Xamarin. Forms аналогична интеграции центров уведомлений Azure с приложением Xamarin Native. Настройте **приложение FCM** , следуя инструкциям консоли Firebase в разделе Push- [уведомления в Xamarin. Android с помощью центров уведомлений Azure](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm#create-a-firebase-project-and-enable-firebase-cloud-messaging). Выполните следующие действия с помощью учебника по Xamarin. Android:

1. Определите имя пакета Android, например `com.xamarin.notifysample`, которое используется в примере.
1. Скачайте файл **Google-Services. JSON** из консоли Firebase. Этот файл будет добавлен в приложение Android на следующем шаге.
1. Создайте экземпляр центра уведомлений Azure и присвойте ему имя. В этой статье и образце используется `xdocsnotificationhub` в качестве имени центра.
1. Скопируйте **ключ сервера** FCM и сохраните его в качестве **ключа API** в **Google (gcm/FCM)** в центре уведомлений Azure.

На следующем снимке экрана показана конфигурация платформы Google в центре уведомлений Azure:

![Снимок экрана с конфигурацией Google центра уведомлений Azure](azure-notification-hub-images/fcm-notification-hub-config.png "Конфигурация Google центра уведомлений Azure")

Для завершения установки устройств iOS потребуется macOS компьютер. Настройте APNS, следуя начальным шагам [push-уведомлений в Xamarin. iOS, используя центры уведомлений Azure](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started#generate-the-certificate-signing-request-file). Выполните следующие действия с помощью учебника по Xamarin. iOS:

1. Определите идентификатор пакета iOS. В этой статье и образце используется `com.xamarin.notifysample` в качестве идентификатора пакета.
1. Создайте файл запроса подписи сертификата (CSR) и используйте его для создания сертификата push-уведомлений.
1. Отправьте сертификат push-уведомлений в **Apple (APNs)** в центре уведомлений Azure.

На следующем снимке экрана показана конфигурация платформы Apple в центре уведомлений Azure:

![Снимок экрана с конфигурацией Apple центра уведомлений Azure](azure-notification-hub-images/apns-notification-hub-config.png "Настройка Apple в центре уведомлений Azure")

## <a name="register-templates-and-tags-with-the-azure-notification-hub"></a>Регистрация шаблонов и тегов в центре уведомлений Azure

Центр уведомлений Azure требует, чтобы мобильные приложения регистрировались в центре, определили шаблоны и подписались на теги. Регистрация связывает с идентификатором в центре уведомлений Azure специфический для платформы PNS маркер. Дополнительные сведения о регистрациях см. в разделе [Управление регистрацией](/azure/notification-hubs/notification-hubs-push-notification-registration-management).

Шаблоны позволяют устройствам задавать параметризованные шаблоны сообщений. Входящие сообщения могут быть настроены для каждого устройства на каждый тег. Дополнительные сведения о шаблонах см. в разделе [шаблоны](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages).

Теги можно использовать для подписки на такие категории сообщений, как новости, Спорт и погода. Для простоты в примере приложения определяется шаблон по умолчанию с одним параметром, который называется `messageParam`, и один тег с именем `default`. В более сложных системах пользовательские теги можно использовать для обмена сообщениями между устройствами для персонализированных уведомлений. Дополнительные сведения о тегах см. в разделе [Маршрутизация и выражения тегов](/azure/notification-hubs/notification-hubs-tags-segment-push-message).

Для успешного получения сообщений каждое собственное приложение должно выполнить следующие действия:

1. Получите маркер PNS или токен от PNS платформы.
1. Зарегистрируйте маркер PNS в центре уведомлений Azure.
1. Укажите шаблон, который содержит те же параметры, что и исходящие сообщения.
1. Подпишитесь на тег, предназначенный для исходящих сообщений.

Эти действия описаны более подробно для каждой платформы в разделе [Настройка приложения Android для уведомлений](#configure-the-android-application-for-notifications) и [Настройка iOS для уведомлений](#configure-ios-for-notifications) .

## <a name="xamarinforms-application-functionality"></a>Функциональные возможности приложения Xamarin. Forms

Пример приложения Xamarin. Forms отображает список сообщений push-уведомлений. Это достигается с помощью метода `AddMessage`, который добавляет указанное сообщение push-уведомления в пользовательский интерфейс. Этот метод также предотвращает добавление повторяющихся сообщений в пользовательский интерфейс и выполняется в основном потоке, чтобы его можно было вызывать из любого потока. В следующем примере кода показан метод `AddMessage`:

```csharp
public void AddMessage(string message)
{
    Device.BeginInvokeOnMainThread(() =>
    {
        if (messageDisplay.Children.OfType<Label>().Where(c => c.Text == message).Any())
        {
            // Do nothing, an identical message already exists
        }
        else
        {
            Label label = new Label()
            {
                Text = message,
                HorizontalOptions = LayoutOptions.CenterAndExpand,
                VerticalOptions = LayoutOptions.Start
            };
            messageDisplay.Children.Add(label);
        }
    });
}
```

Пример приложения содержит файл **AppConstants.CS** , который определяет свойства, используемые проектами платформы. Этот файл необходимо настроить со значениями из центра уведомлений Azure. В следующем коде показан файл **AppConstants.CS** :

```csharp
public static class AppConstants
{
    public static string NotificationChannelName { get; set; } = "XamarinNotifyChannel";
    public static string NotificationHubName { get; set; } = "< Insert your Azure Notification Hub name >";
    public static string ListenConnectionString { get; set; } = "< Insert your DefaultListenSharedAccessSignature >";
    public static string DebugTag { get; set; } = "XamarinNotify";
    public static string[] SubscriptionTags { get; set; } = { "default" };
    public static string FCMTemplateBody { get; set; } = "{\"data\":{\"message\":\"$(messageParam)\"}}";
    public static string APNTemplateBody { get; set; } = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";
}
```

Настройте следующие значения в `AppConstants` для подключения примера приложения к концентратору уведомлений Azure:

* `NotificationHubName`. Используйте имя центра уведомлений Azure, созданного в портал Azure.
* `ListenConnectionString`. это значение находится в центре уведомлений Azure в разделе **политики доступа**.

На следующем снимке экрана показано, где находятся эти значения в портал Azure:

![Снимок экрана политики доступа в центре уведомлений Azure](azure-notification-hub-images/notification-hub-access-policy.png "Политика доступа к концентратору уведомлений Azure")

## <a name="configure-the-android-application-for-notifications"></a>Настройка приложения Android для получения уведомлений

Выполните следующие действия, чтобы настроить приложение Android для получения и обработки уведомлений:

1. Настройте **имя пакета** Android в соответствии с именем пакета в консоли Firebase.
1. Установите следующие пакеты NuGet для взаимодействия с центрами уведомлений Google Play, Firebase и Azure:
    1. Xamarin. Гуглеплайсервицес. base.
    1. Xamarin. Firebase. Messaging.
    1. Xamarin. Azure. NotificationHubs. Android.
1. Скопируйте `google-services.json` файл, скачанный во время установки FCM, в проект и задайте для действия сборки значение `GoogleServicesJson`.
1. [Настройте AndroidManifest. XML для взаимодействия с Firebase](#configure-android-manifest).
1. [Переопределите фиребасемессагингсервице, чтобы обрабатывались сообщения](#override-firebasemessagingservice-to-handle-messages).
1. [Добавление входящих уведомлений в пользовательский интерфейс Xamarin. Forms](#add-incoming-notifications-to-the-xamarinforms-ui).

> [!NOTE]
> Действие сборки **гуглесервицесжсон** является частью пакета NuGet **Xamarin. гуглеплайсервицес. base** . Visual Studio 2019 задает доступные действия сборки во время запуска. Если вы не видите **гуглесервицесжсон** в качестве действия сборки, перезапустите Visual Studio 2019 после установки пакетов NuGet.

### <a name="configure-android-manifest"></a>Настройка манифеста Android

Элементы `receiver` в элементе `application` позволяют приложению взаимодействовать с Firebase. Элементы `uses-permission` позволяют приложению работать с сообщениями и регистрироваться в центре уведомлений Azure. Полный **файл AndroidManifest. XML** должен выглядеть, как в примере ниже:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionCode="1" android:versionName="1.0" package="YOUR_PACKAGE_NAME" android:installLocation="auto">
  <uses-sdk android:minSdkVersion="21" />
  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
  <uses-permission android:name="android.permission.WAKE_LOCK" />
  <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
  <application android:label="Notification Hub Sample">
    <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
    <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
      <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
        <category android:name="${applicationId}" />
      </intent-filter>
    </receiver>
  </application>
</manifest>
```

### <a name="override-firebasemessagingservice-to-handle-messages"></a>Переопределение Фиребасемессагингсервице для работы с сообщениями

Для регистрации в Firebase и обработке сообщений подклассировать класс `FirebaseMessagingService`. Пример приложения определяет класс `FirebaseService`, который подклассировать `FirebaseMessagingService`. Этот класс помечен атрибутом `IntentFilter`, который включает фильтр `com.google.firebase.MESSAGING_EVENT`. Этот фильтр позволяет Android передавать входящие сообщения этому классу для обработки:

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
public class FirebaseService : FirebaseMessagingService
{
    // ...
}

```

При запуске приложения пакет SDK Firebase будет автоматически запрашивать уникальный идентификатор маркера с сервера Firebase. После успешного запроса метод `OnNewToken` будет вызываться для класса `FirebaseService`. Пример проекта переопределяет этот метод и регистрирует маркер в центрах уведомлений Azure:

```csharp
public override void OnNewToken(string token)
{
    // NOTE: save token instance locally, or log if desired

    SendRegistrationToServer(token);
}

void SendRegistrationToServer(string token)
{
    try
    {
        NotificationHub hub = new NotificationHub(AppConstants.NotificationHubName, AppConstants.ListenConnectionString, this);

        // register device with Azure Notification Hub using the token from FCM
        Registration registration = hub.Register(token, AppConstants.SubscriptionTags);

        // subscribe to the SubscriptionTags list with a simple template.
        string pnsHandle = registration.PNSHandle;
        TemplateRegistration templateReg = hub.RegisterTemplate(pnsHandle, "defaultTemplate", AppConstants.FCMTemplateBody, AppConstants.SubscriptionTags);
    }
    catch (Exception e)
    {
        Log.Error(AppConstants.DebugTag, $"Error registering device: {e.Message}");
    }
}
```

Метод `SendRegistrationToServer` регистрирует устройство в центре уведомлений Azure и подписывается на теги с помощью шаблона. Пример приложения определяет один тег с именем `default` и шаблон с одним параметром, который называется `messageParam` в файле **AppConstants.CS** . Дополнительные сведения о регистрации, тегах и шаблонах см. [в статье регистрация шаблонов и тегов в центре уведомлений Azure](#register-templates-and-tags-with-the-azure-notification-hub).

При получении сообщения метод `OnMessageReceived` будет вызываться для класса `FirebaseService`:

```csharp
public override void OnMessageReceived(RemoteMessage message)
{
    base.OnMessageReceived(message);
    string messageBody = string.Empty;

    if (message.GetNotification() != null)
    {
        messageBody = message.GetNotification().Body;
    }

    // NOTE: test messages sent via the Azure portal will be received here
    else
    {
        messageBody = message.Data.Values.First();
    }

    // convert the incoming message to a local notification
    SendLocalNotification(messageBody);

    // send the incoming message directly to the MainPage
    SendMessageToMainPage(messageBody);
}

void SendLocalNotification(string body)
{
    var intent = new Intent(this, typeof(MainActivity));
    intent.AddFlags(ActivityFlags.ClearTop);
    intent.PutExtra("message", body);
    
    //Unique request code to avoid PendingIntent collision.
    var requestCode = new Random().Next();
    var pendingIntent = PendingIntent.GetActivity(this, requestCode, intent, PendingIntentFlags.OneShot);

    var notificationBuilder = new NotificationCompat.Builder(this)
        .SetContentTitle("XamarinNotify Message")
        .SetSmallIcon(Resource.Drawable.ic_launcher)
        .SetContentText(body)
        .SetAutoCancel(true)
        .SetShowWhen(false)
        .SetContentIntent(pendingIntent);

    if (Build.VERSION.SdkInt >= BuildVersionCodes.O)
    {
        notificationBuilder.SetChannelId(AppConstants.NotificationChannelName);
    }

    var notificationManager = NotificationManager.FromContext(this);
    notificationManager.Notify(0, notificationBuilder.Build());
}

void SendMessageToMainPage(string body)
{
    (App.Current.MainPage as MainPage)?.AddMessage(body);
}
```

Входящие сообщения преобразуются в локальное уведомление с помощью метода `SendLocalNotification`. Этот метод создает новый `Intent` и помещает содержимое сообщения в `Intent` как `string` `Extra`. Когда пользователь откасается локального уведомления, находится ли приложение на переднем плане или в фоновом режиме, запускается `MainActivity` и получает доступ к содержимому сообщения через объект `Intent`.

В локальных уведомлениях и `Intent` примере пользователь должен предпринять действия по отприкосновению к уведомлению. Это желательно, когда пользователь должен выполнить действие до изменения состояния приложения. Однако в некоторых случаях может потребоваться получить доступ к данным сообщения, не требуя вмешательства пользователя. Предыдущий пример также отправляет сообщение непосредственно в текущий экземпляр `MainPage` с помощью метода `SendMessageToMainPage`. В рабочей среде при реализации обоих методов для одного типа сообщений объект `MainPage` будет получать дублирующиеся сообщения, если пользователь касается уведомления.

> [!NOTE]
> Приложение Android будет получать push-уведомления только в том случае, если оно выполняется в фоновом или на переднем плане. Чтобы получать push-уведомления, если основной `Activity` не выполняется, необходимо реализовать службу, которая выходит за рамки этого примера. Дополнительные сведения см. в разделе [Создание служб Android](/xamarin/android/app-fundamentals/services/) .

### <a name="add-incoming-notifications-to-the-xamarinforms-ui"></a>Добавление входящих уведомлений в пользовательский интерфейс Xamarin. Forms

Классу `MainActivity` необходимо получить разрешение для обработки уведомлений и управления данными входящих сообщений. В следующем коде показана полная реализация `MainActivity`:

```csharp
[Activity(Label = "NotificationHubSample", Icon = "@mipmap/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation, LaunchMode = LaunchMode.SingleTop)]
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        TabLayoutResource = Resource.Layout.Tabbar;
        ToolbarResource = Resource.Layout.Toolbar;

        base.OnCreate(savedInstanceState);

        global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
        LoadApplication(new App());

        if (!IsPlayServiceAvailable())
        {
            throw new Exception("This device does not have Google Play Services and cannot receive push notifications.");
        }

        CreateNotificationChannel();
    }

    protected override void OnNewIntent(Intent intent)
    {
        if (intent.Extras != null)
        {
            var message = intent.GetStringExtra("message");
            (App.Current.MainPage as MainPage)?.AddMessage(message);
        }

        base.OnNewIntent(intent);
    }

    bool IsPlayServiceAvailable()
    {
        int resultCode = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable(this);
        if (resultCode != ConnectionResult.Success)
        {
            if (GoogleApiAvailability.Instance.IsUserResolvableError(resultCode))
                Log.Debug(AppConstants.DebugTag, GoogleApiAvailability.Instance.GetErrorString(resultCode));
            else
            {
                Log.Debug(AppConstants.DebugTag, "This device is not supported");
            }
            return false;
        }
        return true;
    }

    void CreateNotificationChannel()
    {
        // Notification channels are new as of "Oreo".
        // There is no need to create a notification channel on older versions of Android.
        if (Build.VERSION.SdkInt >= BuildVersionCodes.O)
        {
            var channelName = AppConstants.NotificationChannelName;
            var channelDescription = String.Empty;
            var channel = new NotificationChannel(channelName, channelName, NotificationImportance.Default)
            {
                Description = channelDescription
            };

            var notificationManager = (NotificationManager)GetSystemService(NotificationService);
            notificationManager.CreateNotificationChannel(channel);
        }
    }
}
```

Атрибут `Activity` задает `LaunchMode` приложения `SingleTop`. Этот режим запуска сообщает ОС Android, что разрешается только один экземпляр этого действия. В этом режиме запуска входящие `Intent` данные направляются в метод `OnNewIntent`, который извлекает данные сообщений и отправляет их в экземпляр `MainPage` через метод `AddMessage`. Если приложение использует другой режим запуска, оно должно по-разному выполнять обработку `Intent` данных.

Метод `OnCreate` использует вспомогательный метод, именуемый `IsPlayServiceAvailable`, чтобы убедиться, что устройство поддерживает Google Playную службу. Эмуляторы или устройства, не поддерживающие службу Google Play, не могут получать push-уведомления от Firebase.

## <a name="configure-ios-for-notifications"></a>Настройка iOS для уведомлений

Процесс настройки приложения iOS для получения уведомлений:

1. Настройте **идентификатор пакета** в файле **info. plist** , чтобы он соответствовал значению, используемому в профиле подготовки.
1. Добавьте параметр **включить push-уведомления** в файл прав **. plist** .
1. Добавьте в проект пакет NuGet **Xamarin. Azure. NotificationHubs. iOS** .
1. [Регистрация для получения уведомлений с помощью APNs](#register-for-notifications-with-apns).
1. [Зарегистрируйте приложение в центре уведомлений Azure и подпишитесь на теги](#register-with-azure-notification-hub-and-subscribe-to-tags).
1. [Добавление уведомлений APNs в пользовательский интерфейс Xamarin. Forms](#add-apns-notifications-to-xamarinforms-ui).

На следующем снимке экрана показан параметр **включить push-уведомления** , выбранный в файле прав **. plist** в Visual Studio:

![Снимок экрана с назначением push-уведомлений](azure-notification-hub-images/push-notification-entitlement.png "Права на отправку push-уведомлений")

### <a name="register-for-notifications-with-apns"></a>Регистрация для получения уведомлений с помощью APNS

Для регистрации удаленных уведомлений метод `FinishedLaunching` в файле **AppDelegate.CS** должен быть переопределен. Регистрация зависит от версии iOS, используемой на устройстве. Проект iOS в примере приложения переопределяет метод `FinishedLaunching` для вызова `RegisterForRemoteNotifications`, как показано в следующем примере:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    global::Xamarin.Forms.Forms.Init();
    LoadApplication(new App());

    base.FinishedLaunching(app, options);

    RegisterForRemoteNotifications();

    return true;
}

void RegisterForRemoteNotifications()
{
    // register for remote notifications based on system version
    if (UIDevice.CurrentDevice.CheckSystemVersion(10, 0))
    {
        UNUserNotificationCenter.Current.RequestAuthorization(UNAuthorizationOptions.Alert |
            UNAuthorizationOptions.Sound |
            UNAuthorizationOptions.Sound,
            (granted, error) =>
            {
                if (granted)
                    InvokeOnMainThread(UIApplication.SharedApplication.RegisterForRemoteNotifications);
            });
    }
    else if (UIDevice.CurrentDevice.CheckSystemVersion(8, 0))
    {
        var pushSettings = UIUserNotificationSettings.GetSettingsForTypes(
        UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
        new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(pushSettings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();
    }
    else
    {
        UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
        UIApplication.SharedApplication.RegisterForRemoteNotificationTypes(notificationTypes);
    }
}
```

### <a name="register-with-azure-notification-hub-and-subscribe-to-tags"></a>Регистрация в центре уведомлений Azure и подписка на Теги

Когда устройство успешно зарегистрировано для удаленных уведомлений во время `FinishedLaunching` метода, iOS будет вызывать метод `RegisteredForRemoteNotifications`. Этот метод следует переопределить для выполнения следующих действий.

1. Создайте экземпляр `SBNotificationHub`.
1. Отмените регистрацию существующих регистраций.
1. Зарегистрируйте устройство в центре уведомлений.
1. Подпишитесь на определенные теги с помощью шаблона.

Дополнительные сведения о регистрации устройств, шаблонов и тегов см. [в статье регистрация шаблонов и тегов в центре уведомлений Azure](#register-templates-and-tags-with-the-azure-notification-hub). В следующем примере кода демонстрируется регистрация устройства и шаблонов:

```csharp
public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
{
    Hub = new SBNotificationHub(AppConstants.ListenConnectionString, AppConstants.NotificationHubName);

    // update registration with Azure Notification Hub
    Hub.UnregisterAll(deviceToken, (error) =>
    {
        if (error != null)
        {
            Debug.WriteLine($"Unable to call unregister {error}");
            return;
        }

        var tags = new NSSet(AppConstants.SubscriptionTags.ToArray());
        Hub.RegisterNative(deviceToken, tags, (errorCallback) =>
        {
            if (errorCallback != null)
            {
                Debug.WriteLine($"RegisterNativeAsync error: {errorCallback}");
            }
        });

        var templateExpiration = DateTime.Now.AddDays(120).ToString(System.Globalization.CultureInfo.CreateSpecificCulture("en-US"));
        Hub.RegisterTemplate(deviceToken, "defaultTemplate", AppConstants.APNTemplateBody, templateExpiration, tags, (errorCallback) =>
        {
            if (errorCallback != null)
            {
                if (errorCallback != null)
                {
                    Debug.WriteLine($"RegisterTemplateAsync error: {errorCallback}");
                }
            }
        });
    });
}
```

> [!NOTE]
> Регистрация для удаленных уведомлений может завершаться сбоем в таких ситуациях, как отсутствие подключения к сети. Можно переопределить метод `FailedToRegisterForRemoveNotifications` для управления сбоем регистрации.

### <a name="add-apns-notifications-to-xamarinforms-ui"></a>Добавление уведомлений APNS в пользовательский интерфейс Xamarin. Forms

Когда устройство получает удаленное уведомление, iOS вызывает метод `ReceivedRemoteNotification`. Формат JSON входящего сообщения преобразуется в объект `NSDictionary`, а метод `ProcessNotification` извлекает значения из словаря и отправляет их в экземпляр Xamarin. Forms `MainPage`. Метод `ReceivedRemoteNotifications` переопределен для вызова `ProcessNotification`, как показано в следующем коде:

```csharp
public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
{
    ProcessNotification(userInfo, false);
}

void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
{
    // make sure we have a payload
    if (options != null && options.ContainsKey(new NSString("aps")))
    {
        // get the APS dictionary and extract message payload. Message JSON will be converted
        // into a NSDictionary so more complex payloads may require more processing
        NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;
        string payload = string.Empty;
        NSString payloadKey = new NSString("alert");
        if (aps.ContainsKey(payloadKey))
        {
            payload = aps[payloadKey].ToString();
        }

        if (!string.IsNullOrWhiteSpace(payload))
        {
            (App.Current.MainPage as MainPage)?.AddMessage(payload);
        }

    }
    else
    {
        Debug.WriteLine($"Received request to process notification but there was no payload.");
    }
}
```

## <a name="test-notifications-in-the-azure-portal"></a>Тестовые уведомления в портал Azure

Центры уведомлений Azure позволяют проверить, может ли приложение принимать тестовые сообщения. Раздел **Тестовая Отправка** в концентраторе уведомлений позволяет выбрать целевую платформу и отправить сообщение. Если задать для **выражения отправки в тег** `default`, сообщения будут отсылаться в приложения, которые зарегистрировали шаблон для тега `default`. При нажатии кнопки **Отправить** создается отчет, содержащий количество устройств, достигнутых в сообщении. На следующем снимке экрана показана проверка уведомлений Android в портал Azure:

![Снимок экрана с тестовым сообщением центра уведомлений Azure](azure-notification-hub-images/azure-notification-hub-test-send.png "Тестовое сообщение центра уведомлений Azure")

### <a name="testing-tips"></a>Советы по тестированию

1. При проверке того, что приложение может получать push-уведомления, необходимо использовать физическое устройство. Возможно, виртуальные устройства Android и iOS настроены неправильно для получения push-уведомлений.
1. Пример приложения Android регистрирует свой токен и шаблоны один раз при выдаче маркера Firebase. Во время тестирования может потребоваться запросить новый маркер и повторно зарегистрировать его в центре уведомлений Azure. Для этого лучше всего очистить проект, удалить `bin` и `obj` папки и удалить приложение с устройства, прежде чем выполнять перестроение и развертывание.
1. Многие части потока push-уведомлений выполняются асинхронно. Это может привести к невозможности попадания точек останова или попадания в непредвиденный порядок. Используйте ведение журнала устройства или отладки для трассировки выполнения без прерывания потока приложений. Отфильтруйте журнал устройств Android, используя `DebugTag`, указанные в `Constants`.
1. При остановке отладки в Visual Studio приложение принудительно закрыто. Все получатели сообщений или другие службы, запущенные как часть процесса отладки, будут закрыты и не будут отвечать на события сообщений.

## <a name="create-a-notification-dispatcher"></a>Создание диспетчера уведомлений

Центры уведомлений Azure позволяют серверному приложению отправлять уведомления на устройства на разных платформах. В примере демонстрируется отправка уведомления с помощью консольного приложения **нотификатиондиспатчер** . Приложение включает файл **DispatcherConstants.CS** , который определяет следующие свойства:

```csharp
public static class DispatcherConstants
{
    public static string[] SubscriptionTags { get; set; } = { "default" };
    public static string NotificationHubName { get; set; } = "< Insert your Azure Notification Hub name >";
    public static string FullAccessConnectionString { get; set; } = "< Insert your DefaultFullSharedAccessSignature >";
}
```

Необходимо настроить **DispatcherConstants.CS** в соответствии с конфигурацией центра уведомлений Azure. Значение свойства `SubscriptionTags` должно соответствовать значениям, используемым в клиентских приложениях. Свойство `NotificationHubName` — это имя экземпляра концентратора уведомлений Azure. Свойство `FullAccessConnectionString` — это ключ доступа, который находится в **политиках доступа к**концентратору уведомлений. На следующем снимке экрана показано расположение свойств `NotificationHubName` и `FullAccessConnectionString` в портал Azure:

![Снимок экрана: имя центра уведомлений Azure и Фуллакцессконнектионстринг](azure-notification-hub-images/notification-hub-full-access-policy.png "Имя центра уведомлений Azure и Фуллакцессконнектионстринг")

Консольное приложение циклически обрабатывает каждое значение `SubscriptionTags` и отправляет уведомления подписчикам с помощью экземпляра класса `NotificationHubClient`. В следующем коде показано консольное приложение `Program` класс:

``` csharp
class Program
{
    static int messageCount = 0;

    static void Main(string[] args)
    {
        Console.WriteLine($"Press the spacebar to send a message to each tag in {string.Join(", ", DispatcherConstants.SubscriptionTags)}");
        WriteSeparator();
        while (Console.ReadKey().Key == ConsoleKey.Spacebar)
        {
            SendTemplateNotificationsAsync().GetAwaiter().GetResult();
        }
    }

    private static async Task SendTemplateNotificationsAsync()
    {
        NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(DispatcherConstants.FullAccessConnectionString, DispatcherConstants.NotificationHubName);
        Dictionary<string, string> templateParameters = new Dictionary<string, string>();

        messageCount++;

        // Send a template notification to each tag. This will go to any devices that
        // have subscribed to this tag with a template that includes "messageParam"
        // as a parameter
        foreach (var tag in DispatcherConstants.SubscriptionTags)
        {
            templateParameters["messageParam"] = $"Notification #{messageCount} to {tag} category subscribers!";
            try
            {
                await hub.SendTemplateNotificationAsync(templateParameters, tag);
                Console.WriteLine($"Sent message to {tag} subscribers.");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Failed to send template notification: {ex.Message}");
            }
        }

        Console.WriteLine($"Sent messages to {DispatcherConstants.SubscriptionTags.Length} tags.");
        WriteSeparator();
    }

    private static void WriteSeparator()
    {
        Console.WriteLine("==========================================================================");
    }
}
```

При запуске примера консольного приложения клавишу пробел можно нажать для отправки сообщений. Устройства, на которых выполняются клиентские приложения, должны получать пронумерованные уведомления при условии, что они настроены правильно.

## <a name="related-links"></a>Связанные ссылки

* [Шаблоны push-уведомлений](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages).
* [Управление регистрацией устройств](/azure/notification-hubs/notification-hubs-push-notification-registration-management).
* [Маршрутизация и выражения тегов](/azure/notification-hubs/notification-hubs-tags-segment-push-message).
* [Учебник по концентраторам уведомлений Azure Xamarin. Android](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm).
* [Учебник по центрам уведомлений Azure Xamarin. iOS](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started).
