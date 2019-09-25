---
title: Удаленные уведомления с Google Cloud Messaging
description: Это пошаговое руководство содержит пошаговое описание использования Google Cloud Messaging для реализации удаленных уведомлений (также называемых Push-уведомлениями) в приложении Xamarin. Android. В нем описываются различные классы, которые необходимо реализовать для взаимодействия с Google Cloud Messaging (GCM), а также объясняется, как задать разрешения в манифесте Android для доступа к GCM, и она демонстрирует сквозную обмен сообщениями с примером тестовой программы.
ms.prod: xamarin
ms.assetid: 4FC3C774-EF93-41B2-A81E-C6A08F32C09B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/02/2019
ms.openlocfilehash: a59f824e1d97966d8d0af92bc9bbcc8d80fcfa4d
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2019
ms.locfileid: "71249927"
---
# <a name="remote-notifications-with-google-cloud-messaging"></a>Удаленные уведомления с Google Cloud Messaging

> [!WARNING]
> Нерекомендуемый GCM в Google с 10 апреля 2018 г. Следующие документы и образцы проектов могут больше не поддерживаться. API сервера и клиента GCM Google будут удалены сразу после 29 мая 2019. Google рекомендует перенести приложения GCM в Firebase Cloud Messaging (FCM). Дополнительные сведения об устаревании и миграции GCM см. в разделе [Google Cloud Messaging — не рекомендуется](https://developers.google.com/cloud-messaging/).
>
> Чтобы приступить к удаленным уведомлениям с использованием Firebase Cloud Messaging с Xamarin, см. раздел [Удаленные уведомления с помощью FCM](remote-notifications-with-fcm.md).

_Это пошаговое руководство содержит пошаговое описание использования Google Cloud Messaging для реализации удаленных уведомлений (также называемых Push-уведомлениями) в приложении Xamarin. Android. В нем описываются различные классы, которые необходимо реализовать для взаимодействия с Google Cloud Messaging (GCM), а также объясняется, как задать разрешения в манифесте Android для доступа к GCM, и она демонстрирует сквозную обмен сообщениями с примером тестовой программы._

## <a name="gcm-notifications-overview"></a>Общие сведения об уведомлениях GCM

В этом пошаговом руководстве мы создадим приложение Xamarin. Android, которое использует Google Cloud Messaging (GCM) для реализации удаленных уведомлений (также известных как *Push-уведомления*). Мы реализуем различные службы намерения и прослушивателя, использующие GCM для удаленного обмена сообщениями, и протестируем нашу реализацию с помощью программы командной строки, которая имитирует сервер приложений. 

Прежде чем продолжить работу с этим пошаговым руководством, необходимо получить необходимые учетные данные для использования серверов GCM в Google. Этот процесс описан в [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md). В частности, вам потребуется *ключ API* и *идентификатор отправителя* для вставки в пример кода, представленного в этом пошаговом руководстве. 

Чтобы создать клиентское приложение Xamarin. Android с поддержкой GCM, мы будем использовать следующие шаги:

1. Установите дополнительные пакеты, необходимые для обмена данными с серверами GCM.
2. Настройка разрешений приложения для доступа к серверам GCM.
3. Реализуйте код для проверки наличия Сервисы Google Play. 
4. Реализуйте службу намерения регистрации, которая согласовывается с GCM для маркера регистрации.
5. Реализуйте службу прослушивателя ИДЕНТИФИКАТОРов экземпляров, которая прослушивает обновления токена регистрации от GCM.
6. Реализуйте службу прослушивателя GCM, которая получает удаленные сообщения от сервера приложений через GCM.

Это приложение будет использовать новую функцию GCM, известную как *раздел сообщений*. В разделе Обмен сообщениями сервер приложений отправляет сообщение в раздел, а не в список отдельных устройств. Устройства, подписанные на этот раздел, могут получать сообщения раздела в виде push-уведомлений. Дополнительные сведения об обмене сообщениями GCM см. в разделе [Реализация обмена сообщениями](https://developers.google.com/cloud-messaging/topic-messaging)в Google. 

Когда клиентское приложение будет готово, мы реализуем приложение командной строки C# , которое отправляет push-уведомление в наше клиентское приложение через gcm. 

## <a name="walkthrough"></a>Пошаговое руководство

Для начала создадим новое пустое решение с именем **RemoteNotifications**. Теперь добавим в это решение новый проект Android, основанный на шаблоне **приложения Android** . Давайте назовем **этот проект.** (Если вы не знакомы с созданием проектов Xamarin. Android, см. раздел [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).) Проект **ClientApp** будет содержать код для клиентского приложения Xamarin. Android, которое получает удаленные уведомления через gcm. 

### <a name="add-required-packages"></a>Добавить необходимые пакеты

Прежде чем мы можем реализовать код клиентского приложения, необходимо установить несколько пакетов, которые будут использоваться для взаимодействия с GCM. Кроме того, необходимо добавить приложение Google Play Маркет на наше устройство, если оно еще не установлено.

#### <a name="add-the-xamarin-google-play-services-gcm-package"></a>Добавление пакета Xamarin Сервисы Google Play GCM

Для получения сообщений от Google Cloud Messaging на устройстве должна быть установлена платформа [сервисы Google Play](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Gcm/) . Без этой платформы приложение Android не может принимать сообщения от серверов GCM. Сервисы Google Play выполняется в фоновом режиме, когда устройство Android включено, беззвучное прослушивание сообщений от GCM. Когда эти сообщения поступают, Сервисы Google Play преобразует сообщения в намерения, а затем передает их в зарегистрированные для них приложения. 

В Visual Studio щелкните правой кнопкой мыши **ссылки > Управление пакетами NuGet...** ; в Visual Studio для Mac щелкните правой кнопкой мыши **пакеты > добавить пакеты...** . Найдите **Xamarin сервисы Google Play-gcm** и установите этот пакет в проект **ClientApp** : 

[![Установка Сервисы Google Play](remote-notifications-with-gcm-images/1-google-play-services-sml.png)](remote-notifications-with-gcm-images/1-google-play-services.png#lightbox)

При установке **xamarin сервисы Google Play-gcm** **Xamarin сервисы Google Play-Base** устанавливается автоматически. Если появляется сообщение об ошибке, измените значение параметра *Минимальная версия Android на целевое* до значения, отличного от **Compile с использованием версии пакета SDK** , и повторите установку NuGet. 

Затем измените **MainActivity.CS** и добавьте следующие `using` инструкции:

```csharp
using Android.Gms.Common;
using Android.Util;
```

Это делает типы в пакете Сервисы Google Play ГМС доступными для нашего кода и добавляет функции ведения журнала, которые будут использоваться для отслеживания транзакций с помощью ГМС. 

#### <a name="google-play-store"></a>Google Play Маркет

Чтобы получать сообщения от GCM, на устройстве должно быть установлено Google Play Маркет приложение. (При установке Google Play приложения на устройство также устанавливается Google Play Маркет, поэтому скорее всего он уже установлен на тестовом устройстве.) Без Google Play приложение Android не может принимать сообщения от GCM. Если на устройстве еще не установлено приложение Google Play Маркет, посетите веб-сайт [Google Play](https://support.google.com/googleplay) , чтобы скачать и установить Google Play. 

Кроме того, можно использовать эмулятор Android под управлением Android 2,2 или более поздней версии вместо тестового устройства (вам не нужно устанавливать Google Play Маркет на эмуляторе Android). Однако при использовании эмулятора необходимо использовать Wi-Fi для подключения к GCM, и необходимо открыть несколько портов в брандмауэре Wi-Fi, как описано далее в этом пошаговом руководстве. 

### <a name="set-the-package-name"></a>Задание имени пакета

В [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)мы указали имя пакета для приложения с поддержкой GCM (это имя пакета также служит *идентификатором приложения* , связанным с нашим ключом API и идентификатором отправителя). Давайте откроем свойства проекта **ClientApp** и присвойте имя пакету этой строке. В этом примере мы устанавливаем имя `com.xamarin.gcmexample`пакета:

[![Задание имени пакета](remote-notifications-with-gcm-images/2-package-name-sml.png)](remote-notifications-with-gcm-images/2-package-name.png#lightbox)

Обратите внимание, что клиентское приложение не сможет получить маркер регистрации от GCM, если имя пакета *не совпадает с* именем пакета, которое мы указали в консоли Google Developer. 

### <a name="add-permissions-to-the-android-manifest"></a>Добавление разрешений в манифест Android

Для приложения Android должны быть настроены следующие разрешения, прежде чем они смогут получать уведомления от Google Cloud Messaging: 

- `com.google.android.c2dm.permission.RECEIVE`&ndash; Предоставляет приложению разрешение на регистрацию и получение сообщений от Google Cloud Messaging. (Что `c2dm` означает? Это означает _Обмен сообщениями между облаком и устройствами_, который теперь является устаревшим предшественником gcm. 
    GCM все еще `c2dm` используется во многих строках разрешений.) 

- `android.permission.WAKE_LOCK`&ndash; (Необязательно). предотвращает переход ЦП устройства в спящий режим во время прослушивания сообщения. 

- `android.permission.INTERNET`&ndash; Предоставляет доступ к Интернету, поэтому клиентское приложение может обмениваться данными с gcm. 

- *PACKAGE_NAME регистрирует приложение в* `.permission.C2D_MESSAGE` Android и запрашивает разрешение на монопольное получение всех сообщений C2D (от облака к устройству). &ndash; Префикс *PACKAGE_NAME* совпадает с идентификатором приложения. 

Эти разрешения будут заданы в манифесте Android. Давайте изменим файл **AndroidManifest. XML** и заменяем его содержимым следующим XML-кодом: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" 
    package="YOUR_PACKAGE_NAME" 
    android:versionCode="1" 
    android:versionName="1.0" 
    android:installLocation="auto">
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="YOUR_PACKAGE_NAME.permission.C2D_MESSAGE" />
    <permission android:name="YOUR_PACKAGE_NAME.permission.C2D_MESSAGE" 
                android:protectionLevel="signature" />
    <application android:label="ClientApp" android:icon="@drawable/Icon">
    </application>
</manifest>
```

В приведенном выше XML-коде измените *YOUR_PACKAGE_NAME* на имя пакета для проекта клиентского приложения. Например, `com.xamarin.gcmexample`. 

### <a name="check-for-google-play-services"></a>Проверка на наличие Сервисы Google Play

В этом пошаговом руководстве мы создаем приложение исходного костей с одним `TextView` в пользовательском интерфейсе. Это приложение не указывает взаимодействие с GCM напрямую. Вместо этого мы продемонстрируем окно вывода, чтобы увидеть, как наши подтверждения приложений с помощью GCM, а также проверит лоток уведомлений на наличие новых уведомлений по мере их прибытия. 

Сначала создадим макет для области сообщений. Измените **Resources. Layout. Main. axml** и замените содержимое следующим XML-кодом: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="10dp">
    <TextView
        android:text=" "
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/msgText"
        android:textAppearance="?android:attr/textAppearanceMedium"
        android:padding="10dp" />
</LinearLayout>
```

Сохраните **Main. axml** и закройте его.

При запуске клиентского приложения необходимо убедиться, что Сервисы Google Play доступен, прежде чем пытаться связаться с GCM. Измените **MainActivity.CS** и замените ``count`` объявление переменной экземпляра следующим объявлением переменной экземпляра: 

```csharp
TextView msgText;
```

Затем добавьте следующий метод в класс **MainActivity** : 

```csharp
public bool IsPlayServicesAvailable ()
{
    int resultCode = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable (this);
    if (resultCode != ConnectionResult.Success)
    {
        if (GoogleApiAvailability.Instance.IsUserResolvableError (resultCode))
            msgText.Text = GoogleApiAvailability.Instance.GetErrorString (resultCode);
        else
        {
            msgText.Text = "Sorry, this device is not supported";
            Finish ();
        }
        return false;
    }
    else
    {
        msgText.Text = "Google Play Services is available.";
        return true;
    }
}
```

Этот код проверяет устройство на наличие установленного Сервисы Google Play APK. Если он не установлен, в области сообщений отобразится сообщение с указанием пользователю загрузить APK из Google Play Маркет (или включить его в параметрах системы устройства). Так как мы хотим выполнить эту проверку при запуске клиентского приложения, мы добавим вызов этого метода в конце `OnCreate`. 

Затем замените `OnCreate` метод следующим кодом:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    IsPlayServicesAvailable ();
}
```

Этот код проверяет наличие Сервисы Google Play APK и записывает результат в область сообщений. 

Давайте полностью перестройте и запустите приложение. Вы увидите экран, который выглядит как на следующем снимке экрана: 

[![Сервисы Google Play доступен](remote-notifications-with-gcm-images/3-first-screen-sml.png)](remote-notifications-with-gcm-images/3-first-screen.png#lightbox)

Если вы не получаете этот результат, убедитесь, что на устройстве установлен Сервисы Google Play APK и что пакет **Xamarin сервисы Google Play-gcm** добавлен в проект **ClientApp** , как описано выше. Если возникает ошибка сборки, попробуйте очистить решение и снова построить проект. 

Далее мы напишем код для обращения к GCM и получите маркер регистрации.

### <a name="register-with-gcm"></a>Регистрация в GCM

Прежде чем приложение сможет получать удаленные уведомления от сервера приложений, оно должно быть зарегистрировано в GCM и возвращено маркер регистрации. Работа по регистрации нашего приложения с помощью gcm обрабатывается `IntentService` созданным нами. Наша `IntentService` процедура выполняет следующие действия: 

1. Использует API [InstanceId](https://developers.google.com/instance-id/) для создания маркеров безопасности, которые разрешают клиентскому приложению доступ к серверу приложений. В возврате мы получаем токен регистрации от GCM.

2. Перенаправляет маркер регистрации на сервер приложений (если он требуется серверу приложений).

3. Подписывается на один или несколько каналов уведомлений.

После реализации этого `IntentService`мы протестируем его, чтобы узнать, возвращает ли мы токен регистрации от gcm.

Добавьте новый файл с именем **RegistrationIntentService.CS** и замените код шаблона следующим кодом:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Util;
using Android.Gms.Gcm;
using Android.Gms.Gcm.Iid;

namespace ClientApp
{
    [Service(Exported = false)]
    class RegistrationIntentService : IntentService
    {
        static object locker = new object();

        public RegistrationIntentService() : base("RegistrationIntentService") { }

        protected override void OnHandleIntent (Intent intent)
        {
            try
            {
                Log.Info ("RegistrationIntentService", "Calling InstanceID.GetToken");
                lock (locker)
                {
                    var instanceID = InstanceID.GetInstance (this);
                    var token = instanceID.GetToken (
                        "YOUR_SENDER_ID", GoogleCloudMessaging.InstanceIdScope, null);

                    Log.Info ("RegistrationIntentService", "GCM Registration Token: " + token);
                    SendRegistrationToAppServer (token);
                    Subscribe (token);
                }
            }
            catch (Exception e)
            {
                Log.Debug("RegistrationIntentService", "Failed to get a registration token");
                return;
            }
        }

        void SendRegistrationToAppServer (string token)
        {
            // Add custom implementation here as needed.
        }

        void Subscribe (string token)
        {
            var pubSub = GcmPubSub.GetInstance(this);
            pubSub.Subscribe(token, "/topics/global", null);
        }
    }
}
```

В приведенном выше примере кода измените значение *YOUR_SENDER_ID* на идентификатор отправителя для проекта клиентского приложения. Чтобы получить идентификатор отправителя для проекта, сделайте следующее: 

1. Войдите в [консоль Google Cloud](https://console.cloud.google.com/) и выберите свое имя проекта в раскрывающемся меню. В области **сведений о проекте** , отображаемом для проекта, щелкните **Переход к параметрам проекта**:

    [![Выбор проекта Ксамарингкм](remote-notifications-with-gcm-images/7-choose-project-sml.png)](remote-notifications-with-gcm-images/7-choose-project.png#lightbox)

2. На странице **Параметры** выберите **номер** &ndash; проекта, который является идентификатором отправителя для вашего проекта:

    [![Отображаемый номер проекта](remote-notifications-with-gcm-images/9-project-number-sml.png)](remote-notifications-with-gcm-images/9-project-number.png#lightbox)

Мы хотим начать `RegistrationIntentService` работу, когда наше приложение запустится. Измените **MainActivity.CS** и измените `OnCreate` `RegistrationIntentService` метод, чтобы он начинался после проверки наличия сервисы Google Play: 

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView(Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    if (IsPlayServicesAvailable ())
    {
        var intent = new Intent (this, typeof (RegistrationIntentService));
        StartService (intent);
    }
}
```

Теперь рассмотрим каждый раздел `RegistrationIntentService` , чтобы понять, как это работает. 

Во-первых, мы `RegistrationIntentService` зададим нам следующий атрибут, указывающий, что в системе не создается экземпляр службы: 

```csharp
[Service (Exported = false)]
```

Конструктор именует рабочий поток регистратионинтентсервице, чтобы упростить отладку. `RegistrationIntentService` 

```csharp
public RegistrationIntentService() : base ("RegistrationIntentService") { }
```

Основные функциональные возможности `RegistrationIntentService` находятся `OnHandleIntent` в методе. Давайте рассмотрим этот код, чтобы увидеть, как он регистрирует приложение с помощью GCM.

#### <a name="request-a-registration-token"></a>Запрос маркера регистрации

`OnHandleIntent`сначала вызывает метод [InstanceId. токена](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID.html#getToken&#40;java.lang.String,%20java.lang.String&#41;) Google, чтобы запросить токен регистрации от gcm. Мы переносите этот код `lock` в, чтобы защититься от возможности одновременного &ndash; выполнения нескольких целей регистрации. это `lock` гарантирует, что эти способы обработки обрабатываются последовательно. Если не удалось получить маркер регистрации, выдается исключение и регистрируется ошибка. Если регистрация будет выполнена, для `token` параметра задается маркер регистрации, полученный от gcm: 

```csharp
static object locker = new object ();
...
try
{
    lock (locker)
    {
        var instanceID = InstanceID.GetInstance (this);
        var token = instanceID.GetToken (
            "YOUR_SENDER_ID", GoogleCloudMessaging.InstanceIdScope, null);
        ...
    }
}
catch (Exception e)
{
    Log.Debug ...
```

#### <a name="forward-the-registration-token-to-the-app-server"></a>Переадресация маркера регистрации на сервер приложений

Если получен маркер регистрации (т. е. исключение не было создано), мы вызываем `SendRegistrationToAppServer` , чтобы связать маркер регистрации пользователя с учетной записью на стороне сервера (если таковая имеется), которая поддерживается нашим приложением. Так как эта реализация зависит от проекта сервера приложений, здесь приводится пустой метод: 

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

В некоторых случаях серверу приложений не требуется маркер регистрации пользователя. в этом случае этот метод можно опустить. При отправке маркера регистрации на сервер `SendRegistrationToAppServer` приложений должен поддерживать логическое значение, указывающее, был ли маркер отправлен на сервер. Если это логическое значение false, `SendRegistrationToAppServer` отправляет маркер на сервер &ndash; приложений, в противном случае маркер уже был отправлен на сервер приложений в предыдущем вызове. 

#### <a name="subscribe-to-the-notification-topic"></a>Подпишитесь на раздел уведомлений

Затем мы вызываем наш `Subscribe` метод, чтобы указать GCM, что мы хотим оформить подписываться на раздел уведомлений. В `Subscribe`мы вызываем API [гкмпубсуб. Subscribe](https://developers.google.com/android/reference/com/google/android/gms/gcm/GcmPubSub.html#subscribe&#40;java.lang.String,%20java.lang.String,%20android.os.Bundle&#41;) для подписки клиентского приложения на все сообщения в `/topics/global`:

```csharp
void Subscribe (string token)
{
    var pubSub = GcmPubSub.GetInstance(this);
    pubSub.Subscribe(token, "/topics/global", null);
}
```

Сервер приложений должен отправлять уведомления в `/topics/global` случае их получения. Обратите внимание, что в `/topics` качестве имени раздела может потребоваться любое значение, если сервер приложений и клиентское приложение согласны с этими именами. (Здесь мы выбрали имя `global` , чтобы указать, что мы хотим получить сообщения обо всех разделах, поддерживаемых сервером приложений.) 

Сведения о сообщениях с темой GCM на стороне сервера см. в разделе [Отправка сообщений](https://developers.google.com/cloud-messaging/topic-messaging)Google в разделы. 

#### <a name="implement-an-instance-id-listener-service"></a>Реализация службы прослушивателя ИДЕНТИФИКАТОРов экземпляров

Маркеры регистрации являются уникальными и безопасными. Однако клиентскому приложению (или GCM) может потребоваться обновить маркер регистрации в случае повторной установки приложения или проблемы безопасности. По этой причине необходимо реализовать интерфейс `InstanceIdListenerService` , отвечающий на запросы обновления маркера от gcm. 

Добавьте новый файл с именем **InstanceIdListenerService.CS** и замените код шаблона следующим кодом: 

```csharp
using Android.App;
using Android.Content;
using Android.Gms.Gcm.Iid;

namespace ClientApp
{
    [Service(Exported = false), IntentFilter(new[] { "com.google.android.gms.iid.InstanceID" })]
    class MyInstanceIDListenerService : InstanceIDListenerService
    {
        public override void OnTokenRefresh()
        {
            var intent = new Intent (this, typeof (RegistrationIntentService));
            StartService (intent);
        }
    }
}
```

Добавьте `InstanceIdListenerService` к заметкам следующий атрибут, указывающий, что служба не должна создавать экземпляр службы и может получать запросы на обновление маркера регистрации GCM (также называемого *идентификатором экземпляра*): 

```csharp
[Service(Exported = false), IntentFilter(new[] { "com.google.android.gms.iid.InstanceID" })]
```

Метод в нашей службе `RegistrationIntentService` запускает, чтобы он мог перехватить новый маркер регистрации. `OnTokenRefresh`

#### <a name="test-registration-with-gcm"></a>Проверка регистрации с помощью GCM

Давайте полностью перестройте и запустите приложение. Если вы успешно получили маркер регистрации от GCM, маркер регистрации должен отображаться в окне вывода. Например: 

```shell
D/Mono    ( 1934): Assembly Ref addref ClientApp[0xb4ac2400] -> Xamarin.GooglePlayServices.Gcm[0xb4ac2640]: 2
I/RegistrationIntentService( 1934): Calling InstanceID.GetToken
I/RegistrationIntentService( 1934): GCM Registration Token: f8LdveCvXig:APA91bFIsjUAbP-V8TPQdLR89qQbEJh1SYG38AcCbBUf34z5gSdUc5OsXrgs93YFiGcRSRafPfzkz23lf3-LvYV1CwrFheMjHgwPeFSh12MywnRIhz

```

### <a name="handle-downstream-messages"></a>Обработайте подчиненные сообщения 

Код, который мы реализовали до сих пор, является только «настройкой» кодом; Он проверяет, установлен ли Сервисы Google Play и согласуется с GCM и сервером приложений, чтобы подготовить клиентское приложение для получения удаленных уведомлений. Однако мы еще не можем реализовать код, который фактически получает и обрабатывает подчиненные сообщения уведомления. Для этого необходимо реализовать *службу прослушивателя gcm*. Эта служба получает сообщения раздела от сервера приложений и локально передает их в виде уведомлений. После реализации этой службы мы создадим тестовую программу для отправки сообщений в GCM, чтобы мы могли увидеть, правильно ли работает наша реализация. 

#### <a name="add-a-notification-icon"></a>Добавить значок уведомления

Давайте сначала добавим маленький значок, который будет отображаться в области уведомлений при запуске уведомления. [Этот значок](remote-notifications-with-gcm-images/ic-stat-ic-notification.png) можно скопировать в свой проект или создать собственный пользовательский значок. Мы назовем файл значка **ic_stat_button_click. png** и копируем его в папку **Resources/Draw** . Не забудьте использовать **добавление > существующий элемент...** , чтобы включить этот файл значка в проект.

#### <a name="implement-a-gcm-listener-service"></a>Реализация службы прослушивателя GCM

Добавьте новый файл с именем **GcmListenerService.CS** и замените код шаблона следующим кодом:

```csharp
using Android.App;
using Android.Content;
using Android.OS;
using Android.Gms.Gcm;
using Android.Util;

namespace ClientApp
{
    [Service (Exported = false), IntentFilter (new [] { "com.google.android.c2dm.intent.RECEIVE" })]
    public class MyGcmListenerService : GcmListenerService
    {
        public override void OnMessageReceived (string from, Bundle data)
        {
            var message = data.GetString ("message");
            Log.Debug ("MyGcmListenerService", "From:    " + from);
            Log.Debug ("MyGcmListenerService", "Message: " + message);
            SendNotification (message);
        }

        void SendNotification (string message)
        {
            var intent = new Intent (this, typeof(MainActivity));
            intent.AddFlags (ActivityFlags.ClearTop);
            var pendingIntent = PendingIntent.GetActivity (this, 0, intent, PendingIntentFlags.OneShot);

            var notificationBuilder = new Notification.Builder(this)
                .SetSmallIcon (Resource.Drawable.ic_stat_ic_notification)
                .SetContentTitle ("GCM Message")
                .SetContentText (message)
                .SetAutoCancel (true)
                .SetContentIntent (pendingIntent);

            var notificationManager = (NotificationManager)GetSystemService(Context.NotificationService);
            notificationManager.Notify (0, notificationBuilder.Build());
        }
    }
}
```

Давайте рассмотрим каждый раздел нашего `GcmListenerService` , чтобы понять, как это работает. 

Во-первых, `GcmListenerService` мы запустим атрибут, чтобы указать, что эта служба не должна быть создана системой, и мы включили фильтр намерения, указывающий, что он получает сообщения gcm: 

```csharp
[Service (Exported = false), IntentFilter (new [] { "com.google.android.c2dm.intent.RECEIVE" })]
```

При `GcmListenerService` получении сообщения от gcm `OnMessageReceived` вызывается метод. Этот метод извлекает содержимое сообщения из переданного `Bundle`объекта, записывает в журнал содержимое сообщения (поэтому мы можем просмотреть его в окне вывода) и вызовы `SendNotification` , чтобы запустить локальное уведомление с полученным содержимым сообщения: 

```csharp
var message = data.GetString ("message");
Log.Debug ("MyGcmListenerService", "From:    " + from);
Log.Debug ("MyGcmListenerService", "Message: " + message);
SendNotification (message);
```

Метод использует `Notification.Builder` для создания уведомления, `NotificationManager` а затем использует для запуска уведомления. `SendNotification` Фактически это позволяет преобразовать удаленное сообщение уведомления в локальное уведомление, которое будет представлено пользователю.
Дополнительные сведения об использовании `Notification.Builder` и `NotificationManager`см. в разделе [локальные уведомления](~/android/app-fundamentals/notifications/local-notifications.md).

#### <a name="declare-the-receiver-in-the-manifest"></a>Объявите получателя в манифесте

Прежде чем мы можем получить сообщения от GCM, необходимо объявить прослушиватель GCM в манифесте Android. Давайте изменим файл **AndroidManifest. XML** и заменяем `<application>` раздел следующим кодом XML: 

```xml
<application android:label="RemoteNotifications" android:icon="@drawable/Icon">
    <receiver android:name="com.google.android.gms.gcm.GcmReceiver" 
              android:exported="true" 
              android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
            <category android:name="YOUR_PACKAGE_NAME" />
        </intent-filter>
    </receiver>
</application>
```

В приведенном выше XML-коде измените *YOUR_PACKAGE_NAME* на имя пакета для проекта клиентского приложения. В нашем примере пошагового руководства имя пакета `com.xamarin.gcmexample`—. 

Давайте рассмотрим, что делает каждый параметр в этом XML:

|Параметр|Описание|
|---|---|
|`com.google.android.gms.gcm.GcmReceiver`|Объявляет, что наше приложение реализует приемник GCM, который фиксирует и обрабатывает входящие push-уведомления.|
|`com.google.android.c2dm.permission.SEND`|Объявляет, что только серверы GCM могут напрямую передавать сообщения в приложение.|
|`com.google.android.c2dm.intent.RECEIVE`|Объявление о намерении фильтрации, которое наше приложение обрабатывает широковещательные сообщения от GCM.|
|`com.google.android.c2dm.intent.REGISTRATION`|Объявление о намерении фильтрации, которое наше приложение обрабатывает новые способы регистрации (то есть мы реализовали службу прослушивателя ИДЕНТИФИКАТОРов экземпляров).|

Кроме того, можно `GcmListenerService` использовать эти атрибуты вместо указания их в XML. здесь мы указываем их в **AndroidManifest. XML** , чтобы упростить дальнейшие действия с примерами кода. 

### <a name="create-a-message-sender-to-test-the-app"></a>Создание отправителя сообщений для тестирования приложения

Добавим в решение C# проект консольного приложения для настольных систем и назовем его **MessageSender**. Мы будем использовать это консольное приложение для имитации сервера &ndash; приложений, который будет отсылать сообщения с уведомлениями в **ClientApp** через gcm. 

#### <a name="add-the-jsonnet-package"></a>Добавление пакета Json.NET

В этом консольном приложении мы создаем полезные данные JSON, содержащие сообщение уведомления, которое необходимо отправить клиентскому приложению. Мы будем использовать пакет **JSON.NET** в **MessageSender** , чтобы упростить создание объекта JSON, необходимого для gcm. В Visual Studio щелкните правой кнопкой мыши **ссылки > Управление пакетами NuGet...** ; в Visual Studio для Mac щелкните правой кнопкой мыши **пакеты > добавить пакеты...** . 

Давайте выполним поиск пакета **JSON.NET** и устанавливаем его в проект: 

[![Установка пакета Json.NET](remote-notifications-with-gcm-images/4-add-json.net-sml.png)](remote-notifications-with-gcm-images/4-add-json.net.png#lightbox)

#### <a name="add-a-reference-to-systemnethttp"></a>Добавление ссылки на System .NET. http

Также необходимо добавить ссылку на `System.Net.Http` , чтобы мы могли создать экземпляр `HttpClient` для отправки тестового сообщения в gcm. В проекте **MessageSender** щелкните правой кнопкой мыши **ссылки > Добавить ссылку** и прокрутите вниз, пока не появится **System .NET. http**. Установите флажок рядом с **System .NET. http** и нажмите кнопку **ОК**. 

#### <a name="implement-code-that-sends-a-test-message"></a>Реализация кода, который отправляет тестовое сообщение

В **MessageSender**измените **Program.CS** и замените содержимое следующим кодом:

```csharp
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json.Linq;

namespace MessageSender
{
    class MessageSender
    {
        public const string API_KEY = "YOUR_API_KEY";
        public const string MESSAGE = "Hello, Xamarin!";

        static void Main (string[] args)
        {
            var jGcmData = new JObject();
            var jData = new JObject();

            jData.Add ("message", MESSAGE);
            jGcmData.Add ("to", "/topics/global");
            jGcmData.Add ("data", jData);

            var url = new Uri ("https://gcm-http.googleapis.com/gcm/send");
            try
            {
                using (var client = new HttpClient())
                {
                    client.DefaultRequestHeaders.Accept.Add(
                        new MediaTypeWithQualityHeaderValue("application/json"));

                    client.DefaultRequestHeaders.TryAddWithoutValidation (
                        "Authorization", "key=" + API_KEY);

                    Task.WaitAll(client.PostAsync (url,
                        new StringContent(jGcmData.ToString(), Encoding.Default, "application/json"))
                            .ContinueWith(response =>
                            {
                                Console.WriteLine(response);
                                Console.WriteLine("Message sent: check the client device notification tray.");
                            }));
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Unable to send GCM message:");
                Console.Error.WriteLine(e.StackTrace);
            }
        }
    }
}
```

В приведенном выше коде измените *YOUR_API_KEY* на ключ API для проекта клиентского приложения. 

Этот тестовый сервер приложений отправляет в GCM следующее сообщение в формате JSON:

```csharp
{
  "to": "/topics/global",
  "data": {
    "message": "Hello, Xamarin!"
  }
}
```

GCM, в свою очередь, пересылает это сообщение клиентскому приложению. Давайте создадим **MessageSender** и откройте окно консоли, где можно запустить его из командной строки.

### <a name="try-it"></a>Попробуйте!

Теперь мы готовы протестировать клиентское приложение. Если вы используете эмулятор или ваше устройство взаимодействует с GCM через Wi-Fi, необходимо открыть следующие порты TCP в брандмауэре для получения сообщений GCM: 5228, 5229 и 5230.

Запустите клиентское приложение и просмотрите окно вывода. `RegistrationIntentService` После успешного получения маркера регистрации от gcm в окне вывода должен отобразиться маркер с выходом журнала, похожим на следующий:

```shell
I/RegistrationIntentService(16103): GCM Registration Token: eX9ggabZV1Q:APA91bHjBnQXMUeBOT6JDiLpRt8m2YWtY ...
```

На этом этапе клиентское приложение готово к получению удаленного сообщения уведомления. В командной строке запустите программу **MessageSender. exe** , чтобы отправить в клиентское приложение сообщение уведомления "Hello, Xamarin".
Если вы еще не создали проект **MessageSender** , сделайте это сейчас.

Чтобы запустить **MessageSender. exe** в Visual Studio, откройте командную строку, перейдите в каталог **MessageSender/bin/debug** и выполните команду напрямую:

```cmd
MessageSender.exe
```

Чтобы запустить **MessageSender. exe** в Visual Studio для Mac, откройте сеанс терминала, измените значение на **MessageSender/bin/Debug и выполните отладку** каталога, а затем используйте Mono для запуска **MessageSender. exe.** 

```bash
mono MessageSender.exe
```

Распространение сообщения через GCM и обратно в клиентское приложение может занять до минуты. Если сообщение получено успешно, в окне вывода должны отобразиться выходные данные следующего вида: 

```shell
D/MyGcmListenerService(16103): From:    /topics/global
D/MyGcmListenerService(16103): Message: Hello, Xamarin!
```

Кроме того, обратите внимание, что в области уведомлений появился новый значок уведомления: 

[![Значок уведомления на устройстве](remote-notifications-with-gcm-images/5-icon-appears-sml.png)](remote-notifications-with-gcm-images/5-icon-appears.png#lightbox)

Когда вы открываете панель уведомлений для просмотра уведомлений, вы должны увидеть удаленное уведомление:

[![Отображается сообщение уведомления](remote-notifications-with-gcm-images/6-notification-in-tray-sml.png)](remote-notifications-with-gcm-images/6-notification-in-tray.png#lightbox)

Поздравляем, ваше приложение получило свое первое удаленное уведомление!

Обратите внимание, что сообщения GCM больше не будут приниматься, если приложение принудительно остановлено. Чтобы возобновить уведомления после принудительного завершения, приложение необходимо перезапустить вручную. Дополнительные сведения об этой политике Android см. [в разделе Запуск элементов управления в остановленных приложениях](https://developer.android.com/about/versions/android-3.1.html#launchcontrols) и в этой [записи переполнения стека](https://stackoverflow.com/questions/5051687/broadcastreceiver-not-receiving-boot-completed/19856267#19856267). 

## <a name="summary"></a>Сводка

В этом пошаговом руководстве подробно описаны шаги по реализации удаленных уведомлений в приложении Xamarin. Android. В нем было описано, как установить дополнительные пакеты, необходимые для связи GCM, и было объяснено, как настроить разрешения приложения для доступа к серверам GCM. В нем приведен пример кода, демонстрирующий, как проверить наличие Сервисы Google Play, как реализовать службу намерения регистрации и службу прослушивателя ИДЕНТИФИКАТОРов экземпляра, которая согласовывается с GCM для маркера регистрации и как реализовать прослушиватель GCM. Служба, которая получает и обрабатывает удаленные сообщения уведомления. Наконец, мы реализовали программу тестирования из командной строки для отправки тестовых уведомлений в наше клиентское приложение через GCM. 

## <a name="related-links"></a>Связанные ссылки

- [GCM RemoteNotifications (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/remotenotifications)
- [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)
