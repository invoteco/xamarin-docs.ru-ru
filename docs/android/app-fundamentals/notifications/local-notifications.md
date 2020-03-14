---
title: Локальные уведомления на Android
description: В этом разделе показано, как реализовать локальные уведомления в Xamarin. Android. В нем объясняются различные элементы пользовательского интерфейса уведомления Android и обсуждаются функции API, связанные с созданием и отображением уведомлений.
ms.prod: xamarin
ms.assetid: 03E19D14-7C81-4D5C-88FC-C3A3A927DB46
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/16/2018
ms.openlocfilehash: 617c04e2f40af535fb381362a389524d693fad0b
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305609"
---
# <a name="local-notifications-on-android"></a>Локальные уведомления в Android

_В этом разделе показано, как реализовать локальные уведомления в Xamarin. Android. В нем объясняются различные элементы пользовательского интерфейса уведомления Android и обсуждаются функции API, связанные с созданием и отображением уведомлений._

## <a name="local-notifications-overview"></a>Общие сведения о локальных уведомлениях

Android предоставляет два управляемых системой раздела для отображения значков уведомлений и сведений о уведомлениях для пользователя. При первой публикации уведомления его значок отображается в *области уведомлений*, как показано на следующем снимке экрана:

![Пример области уведомлений на устройстве](local-notifications-images/01-notification-shade.png)

Чтобы получить сведения об уведомлении, пользователь может открыть ящик уведомлений (который разворачивает каждый значок уведомления для отображения содержимого уведомления) и выполнить все действия, связанные с уведомлениями. На следующем снимке экрана показан *ящик уведомлений* , соответствующий области уведомлений, показанной выше.

[![пример ящика уведомлений, отображающий три уведомления](local-notifications-images/02-notification-drawer-sml.png)](local-notifications-images/02-notification-drawer.png#lightbox)

В уведомлениях Android используются два типа макетов:

- ***Базовый макет*** &ndash; компактный, фиксированный формат представления.

- ***Развернутый макет*** &ndash; формат представления, который можно расширить до большего размера, чтобы получить дополнительные сведения.

Каждый из этих типов макетов (и их создание) объясняется в следующих разделах.

> [!NOTE]
> В этом руководством основное внимание уделяется [интерфейсам API нотификатионкомпат](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.html) из [библиотеки поддержки Android](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). Эти API обеспечивают максимальную обратную совместимость с Android 4,0 (API уровня 14).

### <a name="base-layout"></a>Базовый макет

Все уведомления Android основаны на базовом формате макета, который, как минимум, включает в себя следующие элементы:

1. *Значок уведомления*, представляющий исходное приложение, или тип уведомления, если приложение поддерживает различные типы уведомлений.

2. *Заголовок*уведомления или имя отправителя, если уведомление является личным сообщением.

3. Сообщение уведомления.

4. *Отметка времени*.

Эти элементы отображаются, как показано на следующей схеме:

[![расположение элементов уведомления](local-notifications-images/03-notification-callouts-sml.png)](local-notifications-images/03-notification-callouts.png#lightbox)

Базовые макеты ограничены 64 пикселями плотности (DP) в высоту. По умолчанию Android создает этот базовый стиль уведомлений.

При необходимости уведомления могут отображать большой значок, представляющий приложение или фотографию отправителя. При использовании крупного значка в уведомлении в Android 5,0 и более поздних версиях значок небольшого уведомления отображается в виде значка над большим значком:

![Фотография простого уведомления](local-notifications-images/04-simple-notification-photo.png)

Начиная с Android 5,0, уведомления также могут отображаться на экране блокировки.

[![пример уведомления на экране блокировки](local-notifications-images/05-lockscreen-notification-sml.png)](local-notifications-images/05-lockscreen-notification.png#lightbox)

Пользователь может дважды коснуться уведомления на экране блокировки, чтобы разблокировать устройство и перейти к приложению, которое поступило на это уведомление, или прокрутить, чтобы закрыть уведомление. Приложения могут устанавливать уровень видимости уведомления для управления тем, что отображается на экране блокировки, и пользователи могут выбрать, следует ли разрешить отображение конфиденциального содержимого в уведомлениях на экране блокировки.

В Android 5,0 появился высокоприоритетный формат представления уведомлений с названием « *голова»* . В верхней части экрана в течение нескольких секунд появится уведомление о заголовке, а затем ретреат резервное копирование в область уведомлений:

[![пример уведомления о заголовке](local-notifications-images/06-heads-up-notification-sml.png)](local-notifications-images/06-heads-up-notification.png#lightbox)

Уведомления в заголовке позволяют пользовательскому интерфейсу системы размещать важную информацию перед пользователем, не нарушая состояние текущего выполняемого действия.

В Android включена поддержка метаданных уведомлений, чтобы уведомления можно было сортировать и отображать в разумном виде. Метаданные уведомлений также определяют, как уведомления отображаются на экране блокировки и в формате заголовка. Приложения могут устанавливать следующие типы метаданных уведомлений:

- **Priority** &ndash; уровень приоритета определяет, как и когда отображаются уведомления. Например, в Android 5,0 уведомления с высоким приоритетом отображаются в виде головных уведомлений.

- &ndash; **видимости** указывает, сколько содержимого уведомлений должно отображаться при появлении уведомления на экране блокировки.

- **Категория** &ndash; информирует систему об обработке уведомлений в различных обстоятельствах, например, когда устройство находится в режиме "не *беспокоить* ".

> [!NOTE]
> **Видимость** и **Категория** появились в Android 5,0 и недоступны в более ранних версиях Android. Начиная с Android 8,0 [каналы уведомления](#notif-chan) используются для управления способом представления уведомлений пользователю.

### <a name="expanded-layouts"></a>Развернутые макеты

Начиная с Android 4,1, для уведомлений можно настроить развернутые стили макета, позволяющие пользователю увеличить высоту уведомления, чтобы просмотреть дополнительное содержимое. Например, в следующем примере показано развернутое уведомление макета в режиме контракта:

![Уведомление по контракту](local-notifications-images/07-contracted-notification.png)

Когда это уведомление будет развернуто, оно выводит сообщение целиком:

![Расширенное уведомление](local-notifications-images/08-expanded-notification.png)

Android поддерживает три развернутых стиля макета для уведомлений с одним событием:

- ***Большой текст*** &ndash; в режиме контракта, выводит выдержку из первой строки сообщения, за которой следуют две точки. В развернутом режиме отображает все сообщение (как показано в приведенном выше примере).

- ***Папка "Входящие"*** &ndash; в режиме контракта. отображает количество новых сообщений. В развернутом режиме отображает первое сообщение электронной почты или список сообщений в папке "Входящие".

- &ndash; ***изображений*** в режиме контракта отображает только текст сообщения. В развернутом режиме отображает текст и изображение.

[Помимо базового уведомления](#beyond-the-basic-notification) (далее в этой статье) объясняется, как создавать *большие текстовые*сообщения, *папки входящих сообщений*и уведомления с *изображениями* .

<a name="notif-chan"></a>
<a name="notification-channels"></a>
## <a name="notification-channels"></a>Каналы уведомления

Начиная с Android 8,0 (Oreo), можно использовать функцию *каналов уведомлений* для создания настраиваемого пользователем канала для каждого типа уведомлений, которые необходимо отобразить. Каналы уведомлений позволяют группировать уведомления таким образом, чтобы все уведомления, отправляемые в канал, проносили одинаковое поведение. Например, у вас может быть канал уведомления, предназначенный для уведомлений, требующих немедленного вмешательства, и отдельный канал "скрытого", используемый для информационных сообщений.

Приложение **YouTube** , устанавливаемое с Android Oreo, содержит две категории уведомлений: **скачивание уведомлений** и **Общие уведомления**:

[![экранов уведомлений для YouTube в Android Oreo](local-notifications-images/27-youtube-sml.png)](local-notifications-images/27-youtube.png#lightbox)

Каждая из этих категорий соответствует каналу уведомления. Приложение YouTube реализует канал **уведомлений о скачивании** и общий канал **уведомлений** . Пользователь может коснуться пункта **скачать уведомления**, в котором отображается экран параметров для канала уведомлений о скачивании приложения:

[окно "уведомления о загрузке ![" для приложения YouTube](local-notifications-images/28-yt-download-sml.png)](local-notifications-images/28-yt-download.png#lightbox)

На этом экране пользователь может изменить поведение канала уведомлений о **загрузке** , выполнив следующие действия.

- Установите уровень важности " **срочный**", " **высокий**", " **средний**" или " **низкий**", который настраивает уровень звука и визуального прерывания.

- Включите или выключите точку уведомления.

- Включите или Выключите мигающий источник.

- Отображение или скрытие уведомлений на экране блокировки.

- Переопределение параметра "не **беспокоить** ".

Канал **общих уведомлений** имеет похожие параметры:

[![общего экрана с уведомлениями для приложения YouTube](local-notifications-images/29-yt-general-sml.png)](local-notifications-images/29-yt-general.png#lightbox)

Обратите внимание, что вы не имеете абсолютного контроля над тем, как каналы уведомления взаимодействуют с пользователем &ndash; пользователь может изменять параметры для любого канала уведомления на устройстве, как показано на снимках экрана выше. Однако можно настроить значения по умолчанию (как будет описано ниже). Как показано в этих примерах, Новая функция каналов уведомлений дает возможность пользователям точно контролировать различные виды уведомлений.

## <a name="notification-creation"></a>Создание уведомления

Чтобы создать уведомление в Android, используйте класс [нотификатионкомпат. Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder) из пакета NuGet [Xamarin. Android. support. v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) . Этот класс позволяет создавать и публиковать уведомления в более старых версиях Android.
также обсуждается `NotificationCompat.Builder`.

`NotificationCompat.Builder` предоставляет методы для настройки различных параметров в уведомлении, например:

- Содержимое, включая заголовок, текст сообщения и значок уведомления.

- Стиль уведомления, например *большой текст*, *Папка "Входящие"* или стиль *изображения* .

- Приоритет уведомления: минимум, низкий, по умолчанию, высокий или максимум. В Android 8,0 и более поздних приоритетов задается через [_канал уведомления_](#notification-channels).

- Видимость уведомления на экране блокировки: открытый, частный или секретный.

- Метаданные категории, которые позволяют Android классифицировать и фильтровать уведомление.

- Необязательная цель, которая указывает действие, запускаемое при касании уведомления.

- Идентификатор канала уведомления, на который будет опубликовано уведомление (Android 8,0 и более поздние версии).

После настройки этих параметров в построителе создается объект уведомления, содержащий параметры. Чтобы опубликовать уведомление, необходимо передать этот объект уведомления в *Диспетчер уведомлений*. Android предоставляет класс [нотификатионманажер](xref:Android.App.NotificationManager) , который отвечает за публикацию уведомлений и их отображение пользователю. Ссылку на этот класс можно получить из любого контекста, например действия или службы.

### <a name="creating-a-notification-channel"></a>Создание канала уведомления

Приложения, работающие на Android 8,0, должны создавать канал уведомления для своих уведомлений. Для канала уведомления требуются следующие три части информации:

- Строка идентификатора, которая является уникальной для пакета, который будет обозначать канал.
- Имя канала, который будет отображаться для пользователя.  Имя должно содержать от одного до 40 символов.
- Важность канала.

Приложения должны проверить установленную версию Android.
Устройства под управлением версий, предшествующих Android 8,0, не должны создавать канал уведомления. Ниже приведен один из примеров создания канала уведомления в действии.

```csharp
void CreateNotificationChannel()
{
    if (Build.VERSION.SdkInt < BuildVersionCodes.O)
    {
        // Notification channels are new in API 26 (and not a part of the
        // support library). There is no need to create a notification
        // channel on older versions of Android.
        return;
    }

    var channelName = Resources.GetString(Resource.String.channel_name);
    var channelDescription = GetString(Resource.String.channel_description);
    var channel = new NotificationChannel(CHANNEL_ID, channelName, NotificationImportance.Default)
                  {
                      Description = channelDescription
                  };

    var notificationManager = (NotificationManager) GetSystemService(NotificationService);
    notificationManager.CreateNotificationChannel(channel);
}
```

Канал уведомления должен создаваться каждый раз при создании действия. Для метода `CreateNotificationChannel` его следует вызывать в методе `OnCreate` действия.

### <a name="creating-and-publishing-a-notification"></a>Создание и публикация уведомления

Чтобы создать уведомление в Android, выполните следующие действия.

1. Создадите экземпляр объекта `NotificationCompat.Builder`.

2. Вызывайте различные методы для объекта `NotificationCompat.Builder`, чтобы настроить параметры уведомления.

3. Вызовите метод [сборки](xref:Android.App.Notification.Builder.Build) объекта `NotificationCompat.Builder`, чтобы создать экземпляр объекта уведомления.

4. Вызовите метод [Notify](xref:Android.App.NotificationManager.Notify*) диспетчера уведомлений, чтобы опубликовать уведомление.

Для каждого уведомления необходимо указать по крайней мере следующие сведения:

- Маленький значок (24x24 DP в размере)

- Краткий заголовок

- Текст уведомления

В следующем примере кода показано, как использовать `NotificationCompat.Builder` для создания базового уведомления. Обратите внимание, что `NotificationCompat.Builder` методы поддерживают [цепочку методов](https://en.wikipedia.org/wiki/Method_chaining). то есть каждый метод возвращает объект построителя, поэтому вы можете использовать результат последнего вызова метода, чтобы вызвать следующий вызов метода:

```csharp
// Instantiate the builder and set notification elements:
NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

В этом примере создается новый объект `NotificationCompat.Builder` с именем `builder`, а также идентификатор используемого канала уведомления. Задаются заголовок и текст уведомления, а значок уведомления загружается из ресурсов, которые могут быть **нарисованы или ic_notification. png**. Вызов метода `Build` построителя уведомлений создает объект уведомления с этими параметрами. Следующим шагом является вызов метода `Notify` диспетчера уведомлений. Чтобы открыть Диспетчер уведомлений, вызовите `GetSystemService`, как показано выше.

Метод `Notify` принимает два параметра: идентификатор уведомления и объект уведомления. Идентификатор уведомления — это уникальное целое число, идентифицирующее уведомление для приложения. В этом примере идентификатор уведомления установлен в ноль (0); Тем не менее в рабочем приложении необходимо предоставить каждому уведомлению уникальный идентификатор. Повторное использование предыдущего значения идентификатора в вызове `Notify` вызывает перезапись последнего уведомления.

Когда этот код выполняется на устройстве Android 5,0, создается уведомление, которое выглядит как в следующем примере:

![Результат уведомления для примера кода](local-notifications-images/09-hello-world.png)

Значок уведомления отображается в левой части уведомления &ndash; это изображение в кружке &ldquo;я&rdquo; альфа-канал, чтобы Android мог нарисовать серый круг фона. Можно также указать значок без альфа-канала. Чтобы отобразить изображение в виде значка, см. раздел [Формат крупного значка](#large-icon-format) далее в этом разделе.

Метка времени устанавливается автоматически, но этот параметр можно переопределить, вызвав метод [сетвхен](xref:Android.App.Notification.Builder.SetWhen*) построителя уведомлений. Например, в следующем примере кода отметка времени присваивается текущему времени:

```csharp
builder.SetWhen (Java.Lang.JavaSystem.CurrentTimeMillis());
```

### <a name="enabling-sound-and-vibration"></a>Включение звука и вибрации

Если вы хотите, чтобы уведомление также воспроизводило звук, можно вызвать метод [SetDefaults](xref:Android.App.Notification.Builder.SetDefaults*) построителя уведомлений и передать флаг `NotificationDefaults.Sound`:

```csharp
// Instantiate the notification builder and enable sound:
NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetDefaults (NotificationDefaults.Sound)
    .SetSmallIcon (Resource.Drawable.ic_notification);
```

Этот вызов `SetDefaults` приведет к тому, что устройство будет воспроизводить звук при публикации уведомления. Если вы хотите, чтобы устройство вибрировало, а не воспроизводило звук, вы можете передать `NotificationDefaults.Vibrate` в `SetDefaults.` если вы хотите, чтобы устройство воспроизводило звук и вибрировало устройство, вы можете передать оба флага `SetDefaults`:

```csharp
builder.SetDefaults (NotificationDefaults.Sound | NotificationDefaults.Vibrate);
```

Если включить звук без указания звука для воспроизведения, Android будет использовать системное оповещение по умолчанию. Однако можно изменить звук, который будет воспроизводиться, вызвав метод [сетсаунд](xref:Android.App.Notification.Builder.SetSound*) построителя уведомлений. Например, чтобы воспроизвести звуковой сигнал с уведомлением (вместо стандартного звукового сигнала уведомления), можно получить универсальный код ресурса (URI) для звукового сигнала из [рингтонеманажер](xref:Android.Media.RingtoneManager) и передать его в `SetSound`:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Alarm));
```

Кроме того, для уведомления можно использовать системный звук мелодии по умолчанию.

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Ringtone));
```

После создания объекта уведомления можно задать свойства уведомлений для объекта уведомления (вместо того, чтобы настраивать их в соответствии с методами `NotificationCompat.Builder`). Например, вместо вызова метода `SetDefaults` для включения вибрации на уведомлении можно напрямую изменить битовый флаг свойства [по умолчанию](xref:Android.App.Notification.Defaults) уведомления:

```csharp
// Build the notification:
Notification notification = builder.Build();

// Turn on vibrate:
notification.Defaults |= NotificationDefaults.Vibrate;
```

Этот пример приводит к вибрировало устройства при публикации уведомления.

### <a name="updating-a-notification"></a>Обновление уведомления

Если вы хотите обновить содержимое уведомления после его публикации, можно повторно использовать существующий объект `NotificationCompat.Builder`, чтобы создать новый объект уведомления и опубликовать это уведомление с идентификатором последнего уведомления. Пример:

```csharp
// Update the existing notification builder content:
builder.SetContentTitle ("Updated Notification");
builder.SetContentText ("Changed to this message.");

// Build a notification object with updated content:
notification = builder.Build();

// Publish the new notification with the existing ID:
notificationManager.Notify (notificationId, notification);
```

В этом примере существующий объект `NotificationCompat.Builder` используется для создания нового объекта уведомления с другим названием и сообщением.
Новый объект уведомления публикуется с использованием идентификатора предыдущего уведомления, при этом обновляется содержимое ранее опубликованного уведомления:

![Обновленное уведомление](local-notifications-images/12-updated-notification.png)

Текст предыдущего уведомления повторно используется &ndash; только заголовок и текст уведомления изменяются во время отображения уведомления в хранилище уведомлений. Текст заголовка изменится с "Пример уведомления" на "обновленное уведомление", а текст сообщения изменится с "Hello World! Это мое первое уведомление!». в «изменено на это сообщение».

Уведомление остается видимым, пока не произойдет одно из трех действий:

- Пользователь отменяет уведомление (или касания " *Очистить все*").

- Приложение выполняет вызов `NotificationManager.Cancel`, передавая уникальный идентификатор уведомления, назначенный при публикации уведомления.

- Приложение вызывает `NotificationManager.CancelAll`.

Дополнительные сведения об обновлении уведомлений Android см. [в разделе изменение уведомления](https://developer.android.com/training/notify-user/managing.html#Updating).

### <a name="starting-an-activity-from-a-notification"></a>Запуск действия из уведомления

В Android уведомление может быть связано с *действием* &ndash; действия, которое запускается, когда пользователь касается уведомления. Это действие может находиться в другом приложении или даже в другой задаче. Чтобы добавить действие к уведомлению, создайте объект [pendingintents](xref:Android.App.PendingIntent) и свяжите `PendingIntent` с уведомлением. `PendingIntent` — это особый тип намерения, который позволяет приложению-получателю выполнять предопределенную часть кода с разрешениями отправляющего приложения. Когда пользователь касается уведомления, Android запускает действие, заданное `PendingIntent`.

В следующем фрагменте кода показано, как создать уведомление с `PendingIntent`, запускающей действие исходного приложения `MainActivity`:

```csharp
// Set up an intent so that tapping the notifications returns to this app:
Intent intent = new Intent (this, typeof(MainActivity));

// Create a PendingIntent; we're only using one PendingIntent (ID = 0):
const int pendingIntentId = 0;
PendingIntent pendingIntent =
    PendingIntent.GetActivity (this, pendingIntentId, intent, PendingIntentFlags.OneShot);

// Instantiate the builder and set notification elements, including pending intent:
NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
    .SetContentIntent (pendingIntent)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first action notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

Этот код очень похож на код уведомления в предыдущем разделе за исключением того, что в объект уведомления добавляется `PendingIntent`. В этом примере `PendingIntent` связывается с действием исходного приложения перед его передачей методу [сетконтентинтент](xref:Android.App.Notification.Builder.SetContentIntent*) в построителе уведомлений. Флаг `PendingIntentFlags.OneShot` передается методу `PendingIntent.GetActivity`, чтобы `PendingIntent` использовался только один раз. При выполнении этого кода отображается следующее уведомление:

![Уведомление о первом действии](local-notifications-images/10-first-action-notification.png)

Если коснуться этого уведомления, пользователь вернется к исходному действию.

В рабочем приложении Приложение должно *выполнить обработку стека назад* , когда пользователь нажмет кнопку " **назад** " в рамках действия уведомления (если вы не знакомы с задачами Android и стеком "назад", см. раздел [задачи и стек назад](https://developer.android.com/guide/components/tasks-and-back-stack.html)).
В большинстве случаев переход от действия уведомления к предыдущему результату должен вернуть пользователя из приложения и вернуться на начальный экран. Для управления стеком назад приложение использует класс [таскстаккбуилдер](xref:Android.App.TaskStackBuilder) для создания `PendingIntent` со стеком "назад".

Еще один реальный вопрос заключается в том, что исходному действию может потребоваться отправить данные в действие уведомления. Например, уведомление может означать, что получено текстовое сообщение, а для действия уведомления (экран просмотра сообщений) требуется идентификатор сообщения для отображения сообщения пользователю. Действие, создающее `PendingIntent`, может использовать метод [намерения. путекстра](xref:Android.Content.Intent.PutExtra*) для добавления данных (например, строки) к намерениям, чтобы эти данные передавались в действие уведомления.

В следующем образце кода показано, как использовать `TaskStackBuilder` для управления стеком назад, и он включает пример отправки одной строки сообщения в действие уведомления с именем `SecondActivity`:

```csharp
// Setup an intent for SecondActivity:
Intent secondIntent = new Intent (this, typeof(SecondActivity));

// Pass some information to SecondActivity:
secondIntent.PutExtra ("message", "Greetings from MainActivity!");

// Create a task stack builder to manage the back stack:
TaskStackBuilder stackBuilder = TaskStackBuilder.Create(this);

// Add all parents of SecondActivity to the stack:
stackBuilder.AddParentStack (Java.Lang.Class.FromType (typeof (SecondActivity)));

// Push the intent that starts SecondActivity onto the stack:
stackBuilder.AddNextIntent (secondIntent);

// Obtain the PendingIntent for launching the task constructed by
// stackbuilder. The pending intent can be used only once (one shot):
const int pendingIntentId = 0;
PendingIntent pendingIntent =
    stackBuilder.GetPendingIntent (pendingIntentId, PendingIntentFlags.OneShot);

// Instantiate the builder and set notification elements, including
// the pending intent:
NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
    .SetContentIntent (pendingIntent)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my second action notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

В этом примере кода приложение состоит из двух действий: `MainActivity` (который содержит приведенный выше код уведомления) и `SecondActivity`, экран, который пользователь видит после касания уведомления. При выполнении этого кода отображается простое уведомление (аналогично предыдущему примеру). При нажатии на уведомление пользователь переходит на экран `SecondActivity`:

![Снимок экрана второго действия](local-notifications-images/11-second-activity.png)

Строковое сообщение (переданное в метод `PutExtra` намерения) извлекается в `SecondActivity` с помощью следующей строки кода:

```csharp
// Get the message from the intent:
string message = Intent.Extras.GetString ("message", "");
```

Полученное сообщение «поздравления от MainActivity!» отображается на экране `SecondActivity`, как показано на снимке экрана выше. Когда пользователь нажимает кнопку " **назад** " в `SecondActivity`, Навигация переведется из приложения и возвращается на экран, предшествующий запуску приложения.

Дополнительные сведения о создании ожидающих целей см. в разделе [pendingintents](xref:Android.App.PendingIntent).

<a name="beyond-the-basic-notification" />

## <a name="beyond-the-basic-notification"></a>За пределами базового уведомления

Уведомления по умолчанию имеют простой базовый формат макета в Android, но этот базовый формат можно расширить, добавив дополнительные `NotificationCompat.Builder` методов. В этом разделе вы узнаете, как добавить большой значок фотографии в уведомление, и вы увидите примеры создания развернутых уведомлений о макете.

<a name="large-icon-format" />

### <a name="large-icon-format"></a>Формат крупных значков

В уведомлениях Android обычно отображается значок исходного приложения (в левой части уведомления). Однако уведомления могут отображать изображение или фотографию ( *крупный значок*) вместо стандартного маленького значка. Например, приложение для обмена сообщениями может отображать фотографию отправителя, а не значок приложения.

Ниже приведен пример базового уведомления Android 5,0 &ndash; отображается только маленький значок приложения:

![Пример обычного уведомления](local-notifications-images/13-sample-notification.png)

А ниже приведен снимок экрана уведомления после изменения, чтобы отобразить крупный значок &ndash; он использует значок, созданный на основе изображения обезьяны кода Xamarin:

![Пример уведомления о большом значке](local-notifications-images/14-large-icon-sample.png)

Обратите внимание, что если уведомление представлено в формате крупного значка, значок маленького приложения отображается в виде значка в правом нижнем углу крупного значка.

Чтобы использовать изображение в качестве большого значка в уведомлении, вызовите метод [сетларжеикон](xref:Android.App.Notification.Builder.SetLargeIcon*) построителя уведомлений и передайте точечный рисунок изображения. В отличие от `SetSmallIcon``SetLargeIcon` принимает только точечный рисунок. Чтобы преобразовать файл изображения в растровое изображение, используйте класс [битмапфактори](xref:Android.Graphics.BitmapFactory) . Пример:

```csharp
builder.SetLargeIcon (BitmapFactory.DecodeResource (Resources, Resource.Drawable.monkey_icon));
```

В этом примере кода открывается файл изображения в **Resources/Draw/monkey_icon. png**, преобразуется в точечный рисунок и передает полученный точечный рисунок в `NotificationCompat.Builder`. Как правило, разрешение исходного изображения больше, чем маленький значок &ndash; но не намного больше. Слишком большое изображение может вызвать ненужные операции изменения размера, которые могут задержать публикацию уведомления.

### <a name="big-text-style"></a>Стиль больших текста

*Текстовый* стиль — это развернутый шаблон макета, который используется для отображения длинных сообщений в уведомлениях. Как и в случае с развернутыми уведомлениями о разметке, сообщение с большим текстом первоначально отображается в формате Compact Presentation:

![Пример уведомления о большом тексте](local-notifications-images/15-big-text-notification.png)

В этом формате отображается только выдержка сообщения, заканчивающаяся двумя точками. Когда пользователь перетаскивает на уведомление, он разворачивается для отображения всего сообщения уведомления:

![Расширенное уведомление о большом тексте](local-notifications-images/16-big-text-expanded.png)

Этот расширенный формат макета также включает текст сводки в нижней части уведомления. Максимальная высота *текстового* уведомления — 256 точка распространения.

Чтобы создать текстовое уведомление с *большим текстом* , создайте экземпляр объекта `NotificationCompat.Builder`, как и раньше, а затем создайте экземпляр и добавьте объект [бигтекстстиле](xref:Android.App.Notification.BigTextStyle) в объект `NotificationCompat.Builder`. Например:

```csharp
// Instantiate the Big Text style:
Notification.BigTextStyle textStyle = new Notification.BigTextStyle();

// Fill it with text:
string longTextMessage = "I went up one pair of stairs.";
longTextMessage += " / Just like me. ";
//...
textStyle.BigText (longTextMessage);

// Set the summary text:
textStyle.SetSummaryText ("The summary text goes here.");

// Plug this style into the builder:
builder.SetStyle (textStyle);

// Create the notification and publish it ...
```

В этом примере текст сообщения и текст сводки хранятся в объекте `BigTextStyle` (`textStyle`) перед передачей в `NotificationCompat.Builder.`

### <a name="image-style"></a>Стиль изображения

Стиль *изображения* (также называемый *крупным* стилем изображения) — это расширенный формат уведомлений, который можно использовать для вывода изображения в тексте уведомления. Например, приложение для снимка экрана или приложение с фотографиями может использовать стиль уведомления *изображения* , чтобы предоставить пользователю уведомление о последнем захваченном изображении. Обратите внимание, что максимальная высота уведомления *изображения* — 256 DP &ndash; Android изменит размер изображения в соответствии с ограничениями доступной памяти.

Как и все развернутые уведомления макета, уведомления о *изображениях* сначала отображаются в компактном формате, который отображает выдержку сопроводительного текста сообщения:

![Для уведомлений компактного образа не отображается изображение](local-notifications-images/17-image-compact.png)

Когда пользователь перетаскивает *изображение* на уведомление, он разворачивается для отображения изображения. Например, ниже приведена расширенная версия предыдущего уведомления:

![Изображение раскрытого изображения](local-notifications-images/18-image-expanded.png)

Обратите внимание, что когда уведомление отображается в компактном формате, оно отображает текст уведомления (текст, который передается в метод `SetContentText` построителя уведомлений, как показано выше). Однако при раскрытии уведомления для отображения изображения отображается сводный текст над изображением.

Чтобы создать уведомление с *изображением* , создайте экземпляр объекта `NotificationCompat.Builder`, как и раньше, а затем создайте и вставьте объект [бигпиктурестиле](xref:Android.App.Notification.BigPictureStyle) в объект `NotificationCompat.Builder`. Пример:

```csharp
// Instantiate the Image (Big Picture) style:
Notification.BigPictureStyle picStyle = new Notification.BigPictureStyle();

// Convert the image to a bitmap before passing it into the style:
picStyle.BigPicture (BitmapFactory.DecodeResource (Resources, Resource.Drawable.x_bldg));

// Set the summary text that will appear with the image:
picStyle.SetSummaryText ("The summary text goes here.");

// Plug this style into the builder:
builder.SetStyle (picStyle);

// Create the notification and publish it ...
```

Как и метод `SetLargeIcon``NotificationCompat.Builder`, методу [BigPicture](xref:Android.App.Notification.BigPictureStyle.BigPicture*)`BigPictureStyle` требуется точечный рисунок изображения, которое должно отображаться в тексте уведомления. В этом примере метод [декодересаурце](xref:Android.Graphics.BitmapFactory.DecodeResource*) `BitmapFactory` считывает файл изображения, расположенный в разделе **Resources/Draw/x_bldg. png** , и преобразует его в растровое изображение.

Также можно отображать изображения, которые не упакованы в виде ресурса. Например, следующий пример кода загружает изображение с локальной карты SD и отображает его в уведомлении об *образах* :

```csharp
// Using the Image (Big Picture) style:
Notification.BigPictureStyle picStyle = new Notification.BigPictureStyle();

// Read an image from the SD card, subsample to half size:
BitmapFactory.Options options = new BitmapFactory.Options();
options.InSampleSize = 2;
string imagePath = "/sdcard/Pictures/my-tshirt.jpg";
picStyle.BigPicture (BitmapFactory.DecodeFile (imagePath, options));

// Set the summary text that will appear with the image:
picStyle.SetSummaryText ("Check out my new T-Shirt!");

// Plug this style into the builder:
builder.SetStyle (picStyle);

// Create notification and publish it ...
```

В этом примере файл изображения, расположенный по адресу **/сдкард/пиктурес/ми-тширт.ЖПГ** , загружается, изменяется на половину его исходного размера, а затем преобразуется в растровое изображение для использования в уведомлении.

![Пример изображения на футболки в уведомлении](local-notifications-images/19-tshirt-notification.png)

Если вы не знаете размер файла изображения заранее, рекомендуется обернуть вызов [битмапфактори. декодефиле](xref:Android.Graphics.BitmapFactory.DecodeFile*) в обработчик исключений, &ndash; `OutOfMemoryError` может возникнуть исключение, если изображение слишком велико для изменения размера в Android.

Дополнительные сведения о загрузке и декодировании больших растровых изображений см. в статье [эффективное использование больших точечных](https://github.com/xamarin/recipes/tree/master/Recipes/android/resources/general/load_large_bitmaps_efficiently)рисунков.

### <a name="inbox-style"></a>Стиль папки "Входящие"

Формат *«Входящие»* — это развернутый шаблон макета, предназначенный для отображения отдельных строк текста (например, «сводка по электронной почте») в тексте уведомления. Уведомление формата *"Входящие"* сначала отображается в компактном формате:

![Пример уведомления о сжатии папки "Входящие"](local-notifications-images/20-inbox-compact.png)

Когда пользователь перетаскивает на уведомление, он разворачивается для отображения сводки по электронной почте, как показано на снимке экрана ниже:

![Пример развернутого уведомления папки "Входящие"](local-notifications-images/21-inbox-expanded.png)

Чтобы создать уведомление *папки «Входящие»* , создайте экземпляр `NotificationCompat.Builder` объекта, как и раньше, и добавьте [InboxStyle](xref:Android.App.Notification.InboxStyle) в `NotificationCompat.Builder`. Например:

```csharp
// Instantiate the Inbox style:
Notification.InboxStyle inboxStyle = new Notification.InboxStyle();

// Set the title and text of the notification:
builder.SetContentTitle ("5 new messages");
builder.SetContentText ("chimchim@xamarin.com");

// Generate a message summary for the body of the notification:
inboxStyle.AddLine ("Cheeta: Bananas on sale");
inboxStyle.AddLine ("George: Curious about your blog post");
inboxStyle.AddLine ("Nikko: Need a ride to Evolve?");
inboxStyle.SetSummaryText ("+2 more");

// Plug this style into the builder:
builder.SetStyle (inboxStyle);
```

Чтобы добавить новые строки текста в текст уведомления, вызовите метод [аддлине](xref:Android.App.Notification.InboxStyle.AddLine*) объекта `InboxStyle` (максимальная высота уведомления *"входящие"* — 256 точка распространения). Обратите внимание, что, в отличие от стиля *больших текста* , стиль *папки «Входящие* » поддерживает отдельные строки текста в тексте уведомления.

Можно также использовать стиль *папки Входящие* для любого уведомления, которое должно отображать отдельные строки текста в развернутом формате. Например, стиль уведомления *"Входящие"* можно использовать для объединения нескольких ожидающих уведомлений в сводное уведомление &ndash; можно обновить одно уведомление в стиле *папки "Входящие"* на новые строки содержимого уведомления (см. раздел [Обновление уведомлений](#updating-a-notification) выше), а не создавать непрерывный поток новых, в основном аналогичные уведомления.

## <a name="configuring-metadata"></a>Настройка метаданных

`NotificationCompat.Builder` содержит методы, которые можно вызывать для установки метаданных уведомления, таких как приоритет, видимость и Категория. Android использует эти сведения &mdash; вместе с параметрами настройки пользователя &mdash;, чтобы определить, как и когда следует отображать уведомления.

### <a name="priority-settings"></a>Параметры приоритета

Приложения, работающие на Android 7,1 и более низкие, должны устанавливать приоритет непосредственно на самом уведомлении. Настройка приоритета уведомления определяет два результата при публикации уведомления:

- Где уведомление отображается по отношению к другим уведомлениям.
    Например, уведомления с высоким приоритетом представлены над уведомлениями с более низким приоритетом в области уведомлений, независимо от времени публикации каждого уведомления.

- Отображается ли уведомление в формате уведомления Head (Android 5,0 и более поздние версии). В качестве головных уведомлений отображаются только уведомления с *высоким* и *максимальным* приоритетом.

Xamarin. Android определяет следующие перечисления для настройки приоритета уведомлений:

- `NotificationPriority.Max` &ndash; предупреждает пользователя о срочном или критическом состоянии (например, о входящем вызове, последующим направлении или в экстренном оповещении). На устройствах Android 5,0 и более поздних версий уведомления о максимальном приоритете отображаются в формате "заголовки".

- `NotificationPriority.High` &ndash; информирует пользователя о важных событиях (например, о важных сообщениях электронной почты или о поступлении сообщений разговора в режиме реального времени). На устройствах Android 5,0 и более поздних версий уведомления с высоким приоритетом отображаются в виде заголовков.

- `NotificationPriority.Default` &ndash; уведомляет пользователя об условиях со средним уровнем важности.

- `NotificationPriority.Low` &ndash; для несрочной информации, которую пользователь должен знать (например, напоминания об обновлении программного обеспечения или обновления социальных сетей).

- `NotificationPriority.Min` &ndash;, чтобы получить общие сведения, которые пользователь видит только при просмотре уведомлений (например, о расположении или погоде).

Чтобы задать приоритет уведомления, вызовите метод [SetPriority](xref:Android.App.Notification.Builder.SetPriority*) объекта `NotificationCompat.Builder`, передав ему уровень приоритета. Пример:

```csharp
builder.SetPriority (NotificationPriority.High);
```

В следующем примере уведомление с высоким приоритетом — «важное сообщение!». отображается в верхней части ящика уведомлений:

![Пример уведомления с высоким приоритетом](local-notifications-images/22-hi-priority-drawer.png)

Так как это уведомление с высоким приоритетом, оно также отображается в виде головного уведомления над экраном текущего действия пользователя в Android 5,0:

![Пример уведомления о головах](local-notifications-images/23-heads-up-example.png)

В следующем примере уведомление о низком уровне важности помещается в уведомление о повышении уровня заряда батарей.

![Пример уведомления с низким приоритетом](local-notifications-images/24-lo-priority-drawer.png)

Так как уведомление "план за день" является уведомлением с низким приоритетом, Android не будет отображать его в виде головного раздела.

> [!NOTE]
> В Android 8,0 и более поздних версиях приоритет уведомления будет определяться с помощью параметров канала уведомления и настроек пользователя.

### <a name="visibility-settings"></a>Параметры видимости

Начиная с Android 5,0, доступен параметр *видимость* , позволяющий управлять тем, сколько содержимого уведомлений отображается на экране защищенной блокировки.
Xamarin. Android определяет следующие перечисления для настройки видимости уведомлений.

- `NotificationVisibility.Public` &ndash; на экране безопасной блокировки отображается полное содержимое уведомления.

- `NotificationVisibility.Private` &ndash; на экране защищенной блокировки отображается только основная информация (например, значок уведомления и имя приложения, которое его опубликовало), но остальные сведения об уведомлении скрыты. По умолчанию для всех уведомлений используется `NotificationVisibility.Private`.

- `NotificationVisibility.Secret` &ndash; на экране защищенной блокировки ничего не отображается, а даже значок уведомления. Содержимое уведомления доступно только после того, как пользователь разблокирует устройство.

Чтобы задать видимость уведомления, приложения вызывают метод `SetVisibility` объекта `NotificationCompat.Builder`, передавая параметр видимости. Например, этот вызов `SetVisibility` делает `Private`уведомлений:

```csharp
builder.SetVisibility (NotificationVisibility.Private);
```

При публикации `Private` уведомления на экране защищенной блокировки отображается только имя и значок приложения. Вместо сообщения уведомления пользователь видит "разблокировать устройство, чтобы увидеть это уведомление":

![Разблокируйте сообщение уведомления устройства](local-notifications-images/25-lockscreen-private.png)

В этом примере **нотификатионслаб** — это имя исходного приложения. Эта показана исправленная версия версия уведомления отображается только в том случае, если экран блокировки защищен (т. е. защищен с помощью ПИН-кода, шаблона или пароля) &ndash; если экран блокировки небезопасен, полное содержимое уведомления доступно на экране блокировки.

### <a name="category-settings"></a>Параметры категории

Начиная с Android 5,0, для ранжирования и фильтрации уведомлений доступны стандартные категории. Xamarin. Android предоставляет следующие перечисления для этих категорий:

- `Notification.CategoryCall` &ndash; входящего звонка.

- `Notification.CategoryMessage` &ndash; входящего текстового сообщения.

- `Notification.CategoryAlarm` &ndash; условие оповещения или истечение срока таймера.

- `Notification.CategoryEmail` &ndash; входящего сообщения электронной почты.

- `Notification.CategoryEvent` &ndash; события календаря.

- `Notification.CategoryPromo` &ndash; рекламное сообщение или рекламное объявление.

- `Notification.CategoryProgress` &ndash; ход выполнения фоновой операции.

- `Notification.CategorySocial` &ndash; обновления социальных сетей.

- `Notification.CategoryError` &ndash; сбоя фоновой операции или процесса проверки подлинности.

- `Notification.CategoryTransport` &ndash; обновления воспроизведения мультимедиа.

- `Notification.CategorySystem` &ndash; зарезервировано для системного использования (состояние системы или устройства).

- `Notification.CategoryService` &ndash; указывает, что запущена фоновая служба.

- `Notification.CategoryRecommendation` &ndash; сообщение рекомендации, относящееся к выполняющемуся в данный момент приложению.

- `Notification.CategoryStatus` &ndash; сведения об устройстве.

Когда уведомления сортируются, приоритет уведомления устанавливается в приоритет над его параметром категории. Например, уведомление с высоким приоритетом будет отображаться как головное, даже если оно принадлежит к категории `Promo`. Чтобы задать категорию уведомления, вызовите метод `SetCategory` объекта `NotificationCompat.Builder`, передав параметр Category. Пример:

```csharp
builder.SetCategory (Notification.CategoryCall);
```

Функция " *не беспокоить* " (Новая в Android 5,0) фильтрует уведомления на основе категорий. Например, экран " *не беспокоить* " в **параметрах** позволяет пользователю исключить уведомления для телефонных звонков и сообщений:

![Не беспокоить параметры экрана](local-notifications-images/26-do-not-disturb.png)

Когда пользователь настраивает параметр "не *беспокоить* " для блокировки всех прерываний, за исключением телефонных звонков (как показано на снимке экрана выше), Android позволяет получать уведомления с параметрами категории `Notification.CategoryCall`, которые отображаются, пока устройство находится в режиме "не *беспокоить* ". Обратите внимание, что `Notification.CategoryAlarm` уведомления никогда не блокируются в режиме не *беспокоить* .

В примере [локалнотификатионс](https://docs.microsoft.com/samples/xamarin/monodroid-samples/localnotifications) показано, как использовать `NotificationCompat.Builder` для запуска второго действия из уведомления. Этот пример кода объясняется в пошаговом руководстве по [использованию локальных уведомлений в Xamarin. Android](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md) .

### <a name="notification-styles"></a>Стили уведомлений

Чтобы создать уведомления в стиле *текста*, *изображения*или *папки "Входящие"* с `NotificationCompat.Builder`, приложение должно использовать версии совместимости этих стилей. Например, чтобы использовать стиль *текста с большим текстом* , создайте `NotificationCompat.BigTextstyle`:

```csharp
NotificationCompat.BigTextStyle textStyle = new NotificationCompat.BigTextStyle();

// Plug this style into the builder:
builder.SetStyle (textStyle);
```

Аналогичным образом приложение может использовать `NotificationCompat.InboxStyle` и `NotificationCompat.BigPictureStyle` для типов *входящих* и *изображений* соответственно.

### <a name="notification-priority-and-category"></a>Приоритет и Категория уведомления

`NotificationCompat.Builder` поддерживает метод `SetPriority` (доступен, начиная с Android 4,1). Однако метод `SetCategory` *не* поддерживается `NotificationCompat.Builder`, поскольку категории являются частью новой системы метаданных уведомлений, представленной в Android 5,0.

Для поддержки более ранних версий Android, где `SetCategory` недоступен, код может проверить уровень API во время выполнения, чтобы условно вызвать метод `SetCategory`, если уровень API равен или больше, чем Android 5,0 (уровень API 21):

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop) {
    builder.SetCategory (Notification.CategoryEmail);
}
```

В этом примере для **целевой платформы** приложения задано значение Android 5,0, а для **минимальной версии android** — **Android 4,1 (уровень API 16)** . Поскольку `SetCategory` доступен на уровне API 21 и более поздних версий, этот пример кода будет вызывать `SetCategory` только тогда, когда он доступен &ndash; он не будет вызывать `SetCategory`, если уровень API меньше 21.

### <a name="lock-screen-visibility"></a>Видимость экрана блокировки

Поскольку Android не поддерживал уведомления на экране блокировки до Android 5,0 (уровень API 21), `NotificationCompat.Builder` не поддерживает метод `SetVisibility`. Как описано выше для `SetCategory`, код может проверять уровень API во время выполнения и вызывать `SetVisiblity` только в том случае, если он доступен:

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop) {
    builder.SetVisibility (Notification.Public);
}
```

## <a name="summary"></a>Сводка

В этой статье объясняется, как создавать локальные уведомления в Android. В нем было описано Анатомия уведомления, в нем объясняется, как использовать `NotificationCompat.Builder` для создания уведомлений, как задать стиль уведомлений в форматах крупных значков, *больших текстовых*файлов, *изображений* и *входящих сообщений* , как настроить такие параметры метаданных уведомлений, как приоритет, видимость и категория, а также как запустить действие из уведомления. В этой статье также описано, как эти параметры уведомлений работают с новыми заголовками, экраном блокировки и *не беспокоить* функций, появившихся в Android 5,0. Наконец, вы узнали, как использовать `NotificationCompat.Builder` для поддержания совместимости уведомлений с более ранними версиями Android.

Рекомендации по проектированию уведомлений для Android см. в разделе [уведомления](https://developer.android.com/guide/topics/ui/notifiers/notifications.html).

## <a name="related-links"></a>Связанные ссылки

- [Нотификатионслаб (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-notificationslab)
- [Локалнотификатионс (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/localnotifications)
- [Пошаговое руководство по локальным уведомлениям в Android](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md)
- [Уведомление пользователя](https://developer.android.com/training/notify-user/index.html)
- [Уведомлений](xref:Android.App.Notification)
- [нотификатионманажер](xref:Android.App.NotificationManager)
- [Нотификатионкомпат. Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [Pendingintents](xref:Android.App.PendingIntent)
