---
title: Отправлять и получать Push-уведомлений с помощью центров уведомлений Azure и Xamarin.Forms
description: В этой статье объясняется, как использовать центры уведомлений Azure для отправки кроссплатформенных Push-уведомлений в приложения Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 07D13195-3A0D-4C95-ACF0-143A9084973C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 05/23/2019
ms.openlocfilehash: 474398922bf00e3a430166d8b2e073d200e6ed6e
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2019
ms.locfileid: "67659331"
---
# <a name="send-and-receive-push-notifications-with-azure-notification-hubs-and-xamarinforms"></a>Отправлять и получать Push-уведомлений с помощью центров уведомлений Azure и Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png)загрузить пример](https://github.com/xamarin/xamarin-forms-samples/tree/master/WebServices/AzureNotificationHub)

Отправьте сведения доставки уведомления из серверной системы мобильных приложений. Apple, Google и других платформ имеют свои собственные службы Push-уведомлений (PNS). Центры уведомлений позволяют централизовать уведомления на платформах, поэтому серверной части приложения могут взаимодействовать с единый концентратор, который отвечает за распространение уведомлений в PNS каждой платформы.

Интеграция центров уведомлений в мобильные приложения, выполнив следующие действия:

1. [Настройка служб Push-уведомлений и центр уведомлений Azure](#set-up-push-notification-services-and-azure-notification-hub).
1. [Общие сведения об использовании шаблонов и теги](#register-templates-and-tags-with-the-azure-notification-hub).
1. [Создание приложения Xamarin.Forms кросс платформенные](#xamarinforms-application-functionality).
1. [Настройка собственного проекта Android для Push-уведомления](#configure-the-android-application-for-notifications).
1. [Настройка проекта native iOS Push-уведомлений](#configure-ios-for-notifications).
1. [Протестировать уведомления, используя Центр уведомлений Azure](#test-notifications-in-the-azure-portal).
1. [Создание серверной части приложения для отправки уведомлений](#create-a-notification-dispatcher).

## <a name="set-up-push-notification-services-and-azure-notification-hub"></a>Настройка служб Push-уведомлений и центр уведомлений Azure

Интеграция центров уведомлений в мобильное приложение Xamarin.Forms аналогична интеграция центров уведомлений Azure собственное приложение Xamarin. Настройка **приложения FCM** , выполнив шаги в консоли Firebase [Push-уведомления для Xamarin.Android с помощью центров уведомлений Azure](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm#create-a-firebase-project-and-enable-firebase-cloud-messaging). Выполните следующие действия, работе с учебником Xamarin.Android:

1. Определить имя пакета Android, например `com.xamarin.notifysample`, который используется в образце.
1. Скачайте **google-services.json** в консоли Firebase. Вы добавите этот файл в приложение Android, на следующем шаге.
1. Создайте экземпляр центра уведомлений Azure и присвойте ему имя. Такое использование статья и образец `xdocsnotificationhub` качестве имени концентратора.
1. Скопируйте FCM **ключ сервера** и сохраните его как **ключ API** под **Google (GCM или FCM)** в центре уведомлений Azure.

На следующем рисунке показаны конфигурации платформы Google в концентраторе уведомлений Azure:

![Снимок экрана: Настройка Google центра уведомлений Azure](azure-notification-hub-images/fcm-notification-hub-config.png "Настройка Google центра уведомлений Azure")

Вам потребуется компьютера macOS, чтобы завершить установку для устройств iOS. Настройка APNS с начального действия в следующих [Push-уведомления для Xamarin.iOS с помощью центров уведомлений Azure](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started#generate-the-certificate-signing-request-file). Выполните следующие действия, работе с учебником Xamarin.iOS:

1. Определите идентификатор пакета iOS. Такое использование статья и образец `com.xamarin.notifysample` как идентификатор пакета.
1. Создайте файл запроса подписи сертификата (CSR) и использовать его для создания сертификата Push-уведомлений.
1. Отправить сертификат Push-уведомлений в разделе **Apple (APNS)** в центре уведомлений Azure.

На следующем рисунке показаны конфигурации платформы Apple в концентраторе уведомлений Azure:

![Снимок экрана конфигурации Apple центра уведомлений Azure](azure-notification-hub-images/apns-notification-hub-config.png "конфигурации Apple центра уведомлений Azure")

## <a name="register-templates-and-tags-with-the-azure-notification-hub"></a>Регистрация шаблонов и теги в центре уведомлений Azure

Центр уведомлений Azure требует мобильных приложений для регистрации в концентраторе, определять шаблоны и подписываться на теги. Регистрация связывает дескриптор PNS платформы идентификатор в центре уведомлений Azure. Дополнительные сведения о регистрации, см. в разделе [управления регистрацией](/azure/notification-hubs/notification-hubs-push-notification-registration-management).

Шаблоны позволяют устройств для указания шаблонов параметризованное сообщение. Входящие сообщения можно настроить на каждом устройстве, на один тег. Дополнительные сведения о шаблонах см. в разделе [шаблоны](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages).

Теги можно использовать для подписки на категории сообщений, например новости, Спорт и погоды. Для простоты пример приложения определяет шаблон по умолчанию с одним параметром вызывается `messageParam` и один тег с именем `default`. В более сложных системах пользовательские теги можно использовать для сообщения пользователя на устройствах для персонализированных уведомлений. Дополнительные сведения о тегах см. в разделе [Маршрутизация и выражения тегов](/azure/notification-hubs/notification-hubs-tags-segment-push-message).

Для успешного получения сообщений, эти действия необходимо выполнить каждый собственное приложение:

1. Получите маркер PNS или маркер от PNS платформы.
1. Зарегистрируйте маркер PNS концентратора уведомлений Azure.
1. Укажите шаблон, который содержит те же параметры, что исходящие сообщения.
1. Подпишитесь на теге мишенью для исходящих сообщений.

Эти действия описаны более подробно для каждой платформы в [настройте приложение Android для уведомлений](#configure-the-android-application-for-notifications) и [настроить уведомления на устройствах iOS](#configure-ios-for-notifications) разделы.

## <a name="xamarinforms-application-functionality"></a>Функциональные возможности приложения Xamarin.Forms

Пример приложения Xamarin.Forms отображает список сообщений. Это достигается за счет `AddMessage` метод, который добавляет указанный Push-уведомлением в пользовательский Интерфейс. Этот метод также запрещает повторяющихся сообщений в пользовательский интерфейс и выполняется в основном потоке, поэтому его можно вызывать из любого потока. В следующем примере кода показан метод `AddMessage`:

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

Пример приложения содержит **AppConstants.cs** файл, который определяет свойства, используемые проекты платформы. Этот файл должен настраиваться с помощью значений из центра уведомлений Azure. В следующем коде показан **AppConstants.cs** файла:

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

Настроить указанные ниже значения в `AppConstants` подключение примера приложения для центра уведомлений Azure:

* `NotificationHubName`: Используйте имя центра уведомлений Azure, вы создали на портале Azure.
* `ListenConnectionString`: Это значение можно найти в центре уведомлений Azure в разделе **политики доступа**.

На следующем снимке экрана показано, где находятся эти значения на портале Azure:

![Снимок экрана политики доступа центра уведомлений Azure](azure-notification-hub-images/notification-hub-access-policy.png "политики доступа центра уведомлений Azure")

## <a name="configure-the-android-application-for-notifications"></a>Настройте приложение Android для уведомлений

Выполните следующие действия, чтобы настроить приложение Android для получения и обработки уведомлений.

1. Настройка Android **имя пакета** в соответствии с именем пакета в консоли Firebase.
1. Установите следующие пакеты NuGet для взаимодействия с Google Play, Firebase и центров уведомлений:
    1. Xamarin.GooglePlayServices.Base.
    1. Xamarin.Firebase.Messaging.
    1. Xamarin.Azure.NotificationHubs.Android.
1. Копировать `google-services.json` файл, который вы скачали во время установки FCM в проект и задайте действие сборки для `GoogleServicesJson`.
1. [Настройка AndroidManifest.xml для взаимодействия с Firebase](#configure-android-manifest).
1. [Зарегистрировать приложение с помощью Firebase и центр уведомлений Azure с помощью `FirebaseInstanceIdService` ](#register-using-a-custom-firebaseinstanceidservice).
1. [Обрабатывать сообщения с `FirebaseMessagingService` ](#process-messages-with-a-firebasemessagingservice).
1. [Добавление входящих уведомлений в пользовательском Интерфейсе Xamarin.Forms](#add-incoming-notifications-to-the-xamarinforms-ui).

> [!NOTE]
> **GoogleServicesJson** действие сборки является частью **Xamarin.GooglePlayServices.Base** пакет NuGet. Visual Studio 2019 задает доступные действия сборки во время запуска. Если вы не видите **GoogleServicesJson** Build Action, перезапустите Visual Studio 2019 г. После установки пакетов NuGet.

### <a name="configure-android-manifest"></a>Настройка манифеста Android

`receiver` Элементы внутри `application` элемент разрешить приложению взаимодействовать с помощью Firebase. `uses-permission` Элементы позволяют приложению обрабатывать сообщения и регистрация в центре уведомлений Azure. Полный **AndroidManifest.xml** должен выглядеть аналогично приведенному ниже:

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

### <a name="register-using-a-custom-firebaseinstanceidservice"></a>Регистрация с помощью пользовательских FirebaseInstanceIdService

Firebase выдает токены, которые однозначно идентифицировать устройство в PNS. Маркеры имеют более длинный временем существования, но периодически обновляются. Если маркер является выдачи или обновления, приложение должно зарегистрировать свой новый маркер в центре уведомлений Azure. Регистрации, обрабатывается экземпляром класса, производного от `FirebaseInstanceIdService`.

В приложении-примере `FirebaseRegistrationService` класс наследует от `FirebaseInstanceIdService`. Этот класс имеет `IntentFilter` , включающий `com.google.firebase.INSTANCE_ID_EVENT`, позволяя ОС Android автоматически вызывать `OnTokenRefresh` при Firebase выдает маркер.

В следующем коде показан пользовательский `FirebaseInstanceIdService` из примера приложения:

```csharp
[Service]
[IntentFilter(new [] { "com.google.firebase.INSTANCE_ID_EVENT"})]
public class FirebaseRegistrationService : FirebaseInstanceIdService
{
    public override void OnTokenRefresh()
    {
        string token = FirebaseInstanceId.Instance.Token;

        // NOTE: logging the token is not recommended in production but during
        // development it is useful to test messages directly from Firebase
        Log.Info(AppConstants.DebugTag, $"Token received: {token}");

        SendRegistrationToServer(token);
    }

    void SendRegistrationToServer(string token)
    {
        try
        {
            NotificationHub hub = new NotificationHub(AppConstants.NotificationHubName, AppConstants.ListenConnectionString, this);

            // register device with Azure Notification Hub using the token from FCM
            Registration reg = hub.Register(token, AppConstants.SubscriptionTags);

            // subscribe to the SubscriptionTags list with a simple template.
            string pnsHandle = reg.PNSHandle;
            var cats = string.Join(", ", reg.Tags);
            var temp = hub.RegisterTemplate(pnsHandle, "defaultTemplate", AppConstants.FCMTemplateBody, AppConstants.SubscriptionTags);
        }
        catch (Exception e)
        {
            Log.Error(AppConstants.DebugTag, $"Error registering device: {e.Message}");
        }
    }
}
```

`SendRegistrationToServer` Метод в `FirebaseRegistrationClass` регистрирует устройство в центре уведомлений Azure и подписывается на теги с помощью шаблона. Пример приложения определяет один тег `default` и шаблон с одним параметром с именем `messageParam` в **AppConstants.cs** файла. Дополнительные сведения о регистрации, теги и шаблоны, см. в разделе [зарегистрировать шаблоны и теги в центре уведомлений Azure](#register-templates-and-tags-with-the-azure-notification-hub)

### <a name="process-messages-with-a-firebasemessagingservice"></a>Обрабатывает сообщения с помощью FirebaseMessagingService

Входящие сообщения направляются в `FirebaseMessagingService` экземпляра, где их можно преобразовать в локальное уведомление. Проект Android в приложении-примере содержит класс с именем `FirebaseService` , наследуемый от `FirebaseMessagingService`. Этот класс имеет `IntentFilter` , включающий `com.google.firebase.MESSAGING_EVENT`, позволяя ОС Android автоматически вызывать `OnMessageReceived` при получении Push-уведомлением.

В следующем примере показан `FirebaseService` из примера приложения:

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
public class FirebaseService : FirebaseMessagingService
{
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
        var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

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
}
```

Входящие сообщения преобразуются в локальное уведомление с `SendLocalNotification` метод. Этот метод создает новую `Intent` и помещает его содержимого в `Intent` как `string` `Extra`. Когда пользователь касается уведомления, находится ли приложение в обычном или фоновом режиме, `MainActivity` запускается, и имеет доступ к содержимому сообщения через `Intent` объекта.

Уведомления и `Intent` пример требует от пользователя выполнить действие касания в уведомлении. Это желательно, когда пользователь должен выполнить действия, прежде чем изменения состояния приложения. Тем не менее вы можете получить доступ к данным сообщения без вмешательства пользователя в некоторых случаях. Предыдущий пример также отправляет сообщение непосредственно к текущему `MainPage` с экземпляром `SendMessageToMainPage` метод. В рабочей среде, если вы реализуете только один тип сообщений, оба метода `MainPage` объекта, при касании уведомления будут получать повторяющихся сообщений.

> [!NOTE]
> Приложение Android будет получать Push-уведомления только в том случае, если оно выполняется в фоновом режиме или в переднего плана. Для получения Push-уведомлений при основной `Activity` является не запущена, необходимо реализовать службу, которая выходит за рамки данного образца. Дополнительные сведения см. в разделе [Создание службы Android](/xamarin/android/app-fundamentals/services/)

### <a name="add-incoming-notifications-to-the-xamarinforms-ui"></a>Добавление входящих уведомлений в пользовательском Интерфейсе Xamarin.Forms

`MainActivity` Классу необходимо получить разрешение для обработки уведомлений и управления ими данные входящего сообщения. В следующем коде показан полный `MainActivity` реализации:

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

        if (IsPlayServiceAvailable() == false)
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

`Activity` Атрибут задает приложение `LaunchMode` для `SingleTop`. Этот режим запуска сообщает ОС Android, чтобы разрешить только один экземпляр этого действия. В этом режиме запуска входящих `Intent` обмен данными для `OnNewIntent` метод, который извлекает данные сообщения и отправляет ее в `MainPage` экземпляра через `AddMessage` метод. Если приложение использует режим запуска другой, он должен обрабатывать `Intent` данных по-разному.

`OnCreate` Метод использует вспомогательный метод, именуемый `IsPlayServiceAvailable` для обеспечения устройство поддерживает службу Google Play. Эмуляторах или устройствах, которые не поддерживают службу Google Play не может получать Push-уведомления из Firebase.

## <a name="configure-ios-for-notifications"></a>Настроить уведомления на устройствах iOS

Находится в процессе настройки приложение iOS может получать уведомления:

1. Настройка **идентификатор пакета** в **Info.plist** файл в соответствии с значение, используемое в профиле подготовки.
1. Добавить **включить Push-уведомления** равным **Entitlements.plist** файла.
1. Добавить **Xamarin.Azure.NotificationHubs.iOS** свой проект пакет NuGet.
1. [Зарегистрироваться для получения уведомлений APNS](#register-for-notifications-with-apns).
1. [Регистрация приложения в центре уведомлений Azure и подписываться на теги](#register-with-azure-notification-hub-and-subscribe-to-tags).
1. [Добавление уведомлений APNS в пользовательский Интерфейс Xamarin.Forms](#add-apns-notifications-to-xamarinforms-ui).

На следующем снимке экрана показан **включить Push-уведомления** параметра, выбранного в **Entitlements.plist** файла в Visual Studio:

![Снимок экрана с назначением Push-уведомления](azure-notification-hub-images/push-notification-entitlement.png "Push-уведомления прав")

### <a name="register-for-notifications-with-apns"></a>Регистрация для получения уведомлений с помощью APNS

`FinishedLaunching` Метод в **AppDelegate.cs** файл должен быть переопределен для регистрации удаленных уведомлений. Регистрация будет зависеть от версии iOS, используемых на устройстве. Проект iOS в приложении-примере переопределяет `FinishedLaunching` метод для вызова `RegisterForRemoteNotifications` как показано в следующем примере:

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

### <a name="register-with-azure-notification-hub-and-subscribe-to-tags"></a>Регистрация в центре уведомлений Azure и подписываться на теги

Когда устройство успешно зарегистрировала для удаленных уведомлений во время `FinishedLaunching` метод, iOS будет вызывать `RegisteredForRemoteNotifications` метод. Этот метод следует переопределить для выполнения следующих действий:

1. Создать экземпляр `SBNotificationHub`.
1. Отмена регистрации все существующие регистрации.
1. Зарегистрируйте устройство в центре уведомлений.
1. Подписаться на определенные теги с помощью шаблона.

Дополнительные сведения о регистрации устройства, шаблоны и теги, см. в разделе [зарегистрировать шаблоны и теги в центре уведомлений Azure](#register-templates-and-tags-with-the-azure-notification-hub). Следующий код демонстрирует регистрацию устройства и шаблоны:

```csharp
public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
{
    Hub = new SBNotificationHub(AppConstants.ListenConnectionString, AppConstants.NotificationHubName);

    // update registration with Azure Notification Hub
    Hub.UnregisterAllAsync(deviceToken, (error) =>
    {
        if (error != null)
        {
            Debug.WriteLine($"Unable to call unregister {error}");
            return;
        }

        var tags = new NSSet(AppConstants.SubscriptionTags.ToArray());
        Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) =>
        {
            if (errorCallback != null)
            {
                Debug.WriteLine($"RegisterNativeAsync error: {errorCallback}");
            }
        });

        var templateExpiration = DateTime.Now.AddDays(120).ToString(System.Globalization.CultureInfo.CreateSpecificCulture("en-US"));
        Hub.RegisterTemplateAsync(deviceToken, "defaultTemplate", AppConstants.APNTemplateBody, templateExpiration, tags, (errorCallback) =>
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
> Регистрация для получения удаленных уведомлений может завершиться ошибкой в ситуациях, например, отсутствует сетевое подключение. Вы можете переопределить `FailedToRegisterForRemoveNotifications` метод, чтобы обработать сбой регистрации.

### <a name="add-apns-notifications-to-xamarinforms-ui"></a>Добавление пользовательского интерфейса Xamarin.Forms уведомления APNS

Когда устройство получает удаленного уведомления iOS вызывает метод `ReceivedRemoteNotification` метод. Входящее сообщение JSON преобразуется в `NSDictionary` объекта и `ProcessNotification` метод извлекает значения из словаря и отправляет их в Xamarin.Forms `MainPage` экземпляра. `ReceivedRemoteNotifications` Метод переопределяется, чтобы вызвать `ProcessNotification` как показано в следующем коде:

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

## <a name="test-notifications-in-the-azure-portal"></a>Тестовые уведомления на портале Azure

Центры уведомлений позволяют проверить, что приложение может получать тестовые сообщения. **Тестовая отправка** раздел в концентраторе уведомлений позволяет выбрать целевую платформу и отправить сообщение. Установка **Отправка к выражению тега** для `default` будет отправлять сообщения приложениям, которые зарегистрировали шаблон для `default` тега. Щелкнув **отправки** формируется отчет, содержащий количество устройств, связаться с сообщением. На следующем рисунке показан проверка Android уведомления на портале Azure:

![Снимок экрана центра уведомлений Azure тестовое сообщение](azure-notification-hub-images/azure-notification-hub-test-send.png "тестовое сообщение центра уведомлений Azure")

### <a name="testing-tips"></a>Советы по тестированию

1. При тестировании, что приложение может получать Push-уведомлений, необходимо использовать физическое устройство. Виртуальные устройства iOS и Android может быть неверно настроен для получения Push-уведомлений.
1. Пример приложения Android регистрирует его маркер и шаблонов, после выпуска маркера Firebase. Во время тестирования может потребоваться запросить новый маркер и повторно зарегистрировать с помощью центра уведомлений Azure. — Лучший способ сделать это принудительно очистить проект, удалите `bin` и `obj` папок и удалите приложение с устройства до повторного построения и развертывания.
1. Многие компоненты потока уведомлений push выполняются асинхронно. Это может привести точки останова не заданы базовые попаданий или попадании в непредвиденном порядке. Используйте устройства или журнала отладки для трассировки выполнения, не прерывая поток приложения. Фильтрация журнала устройства Android с помощью `DebugTag` указано в `Constants`.

## <a name="create-a-notification-dispatcher"></a>Создание диспетчера уведомлений

Центры уведомлений позволяют серверной части приложения для отправки уведомления на устройства на платформах. В нем демонстрируется диспетчеризации уведомлений с **NotificationDispatcher** консольное приложение. Приложение включает в себя **DispatcherConstants.cs** файл, который определяет следующие свойства:

```csharp
public static class DispatcherConstants
{
    public static string[] SubscriptionTags { get; set; } = { "default" };
    public static string NotificationHubName { get; set; } = "< Insert your Azure Notification Hub name >";
    public static string FullAccessConnectionString { get; set; } = "< Insert your DefaultFullSharedAccessSignature >";
}
```

Необходимо настроить **DispatcherConstants.cs** в соответствии с конфигурацией вашего центра уведомлений Azure. Значение `SubscriptionTags` свойства должны совпадать со значениями, используемых в клиентских приложениях. `NotificationHubName` Свойство — имя экземпляра концентратора уведомлений Azure. `FullAccessConnectionString` Свойство является ключом доступа, найти в центре уведомлений **политики доступа**. На следующем снимке экрана показан расположение `NotificationHubName` и `FullAccessConnectionString` свойства на портале Azure:

![Снимок экрана имя центра уведомлений Azure и FullAccessConnectionString](azure-notification-hub-images/notification-hub-full-access-policy.png "имя центра уведомлений Azure и FullAccessConnectionString")

Консольное приложение обрабатывает каждую `SubscriptionTags` значение и отправляет уведомления для подписчиков, использующих экземпляр `NotificationHubClient` класса. В следующем коде показано консольное приложение `Program` класса:

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

При запуске консольного приложения, нужно нажать пробел для отправки сообщений. Устройства под управлением клиента, который должен получать приложения нумерованных уведомления, при условии, что они настроены правильно.

## <a name="related-links"></a>Связанные ссылки

* [Push-уведомлений шаблонов](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages).
* [Управление регистрацией устройств](/azure/notification-hubs/notification-hubs-push-notification-registration-management).
* [Маршрутизация и выражения тегов](/azure/notification-hubs/notification-hubs-tags-segment-push-message).
* [Руководства по концентраторам уведомлений Azure для Xamarin.Android](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm).
* [Руководства по концентраторам уведомлений Azure для Xamarin.iOS](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started).
