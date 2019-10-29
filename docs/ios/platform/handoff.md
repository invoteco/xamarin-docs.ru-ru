---
title: Передачи в Xamarin. iOS
description: В этой статье рассматривается работа с передачей в приложение Xamarin. iOS для передачи действий пользователя между приложениями, работающими на других устройствах пользователя.
ms.prod: xamarin
ms.assetid: 405F966A-4085-4621-AA15-33D663AD15CD
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 97a4a90b66e2c205ef8e9081e6989bf0f3650b16
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032410"
---
# <a name="handoff-in-xamarinios"></a>Передачи в Xamarin. iOS

_В этой статье рассматривается работа с передачей в приложение Xamarin. iOS для передачи действий пользователя между приложениями, работающими на других устройствах пользователя._

Компания Apple представила передачу данных в iOS 8 и OS X Yosemite (10,10), чтобы предоставить пользователю общий механизм передачи действий, запущенных на одном из устройств, на другое устройство, которое поддерживает то же самое приложение, или другое приложение, поддерживающее то же действие.

[![](handoff-images/handoff02.png "An example of performing a Handoff operation")](handoff-images/handoff02.png#lightbox)

В этой статье рассматривается Включение совместного использования действий в приложении Xamarin. iOS и подробное рассмотрение платформы передачи:

## <a name="about-handoff"></a>О передаче

Прием (также известный как непрерывность) появился в Apple в iOS 8 и OS X Yosemite (10,10) как способ запуска действия на одном из устройств (iOS или Mac) и продолжать это действие на другом устройстве (как определено Иклауом пользователя). d учетная запись).

Переданная функция была расширена в iOS 9, а также поддерживает новые возможности расширенного поиска. Дополнительные сведения см. в документации по [усовершенствованиям поиска](~/ios/platform/search/index.md) .

Например, пользователь может запустить электронное письмо на устройстве iPhone и без проблем продолжить письмо на своем компьютере Mac, при этом вся информация о сообщениях заполняется и курсор находится в том же расположении, в котором они были в iOS.

Любое приложение, использующее один и тот же _идентификатор команды_ , может использовать передачу для продолжения действий пользователей в приложениях при условии, что эти приложения либо доставляются через магазин приложений iTunes, либо подписаны зарегистрированным разработчиком (для приложений Mac, Enterprise или ad hoc).

Любой `NSDocument` или `UIDocument` приложения автоматически имеют встроенную поддержку, а также требует минимальных изменений для поддержки передачи.

### <a name="continuing-user-activities"></a>Продолжение действий пользователя

Класс `NSUserActivity` (вместе с некоторыми незначительными изменениями в `UIKit` и `AppKit`) обеспечивает поддержку определения действия пользователя, которое потенциально может быть продолжено на другом устройстве пользователя.

Чтобы действие передавалось на другое устройство пользователя, оно должно быть инкапсулировано в экземпляре `NSUserActivity`, помечено как _текущее действие_, иметь набор полезных данных (данные, используемые для выполнения продолжения), а действие затем должно быть передано в Это устройство.

Для определения действия, которое должно быть продолжено с помощью iCloud, передается некоторая информация об минимальном объеме данных.

На принимающем устройстве пользователь получит уведомление о том, что действие доступно для продолжения. Если пользователь решит продолжить действие на новом устройстве, запускается указанное приложение (если оно еще не запущено) и полезные данные из `NSUserActivity` используются для перезапуска действия.

[![](handoff-images/handoffinteractions.png "An overview of Continuing User Activities")](handoff-images/handoffinteractions.png#lightbox)

Только приложения с одинаковым ИДЕНТИФИКАТОРом группы разработчиков и отвечают на данный _тип действия_ , могут быть продолжены. Приложение определяет типы действий, которые он поддерживает, в `NSUserActivityTypes`ном ключе файла **info. plist** . В этом случае продолжающееся устройство выбирает приложение для выполнения продолжения на основе идентификатора группы, типа действия и, при необходимости, _названия действия_.

Принимающее приложение использует сведения из словаря `UserInfo` `NSUserActivity`для настройки его пользовательского интерфейса и восстановления состояния данного действия, чтобы переход был незаметным для конечного пользователя.

Если продолжение требует больше информации, чем может быть эффективно отправлено с помощью `NSUserActivity`, возобновляемое приложение может отправить вызов в исходное приложение и установить один или несколько потоков для передачи необходимых данных. Например, если действие редактировало большой текстовый документ с несколькими изображениями, для передачи сведений, необходимых для продолжения действия на принимающем устройстве, потребуется потоковая передача. Дополнительные сведения см. в разделе [Поддержка потоков продолжения](#supporting-continuation-streams) ниже.

Как уже говорилось выше, `NSDocument` или `UIDocument` приложения автоматически имеют встроенную поддержку. Дополнительные сведения см. в разделе [Поддержка передачи в приложениях на основе документов](#supporting-handoff-in-document-based-apps) ниже.

### <a name="the-nsuseractivity-class"></a>Класс Нсусерактивити

Класс `NSUserActivity` является основным объектом в переданном обмене и используется для инкапсуляции состояния действия пользователя, доступного для продолжения. Приложение будет создавать экземпляр копии `NSUserActivity` для любых действий, которые он поддерживает, и хочет продолжить работу на другом устройстве. Например, редактор документов создаст действие для каждого документа, открытого в данный момент. Однако только самый первый документ (отображается в самом самом простом окне или вкладке) является _текущим действием_ и таким образом доступным для продолжения.

Экземпляр `NSUserActivity` определяется как свойствами `ActivityType`, так и `Title`. Свойство Dictionary `UserInfo` используется для передачи сведений о состоянии действия. Задайте для свойства `NeedsSave` значение `true`, если требуется отложенная загрузка сведений о состоянии с помощью делегата `NSUserActivity`. Используйте метод `AddUserInfoEntries` для слияния новых данных из других клиентов с `UserInfo` словарем в соответствии с требованиями для сохранения состояния действия.

### <a name="the-nsuseractivitydelegate-class"></a>Класс Нсусерактивитиделегате

`NSUserActivityDelegate` используется для сохранения сведений в словаре `UserInfo` `NSUserActivity`и синхронизации с текущим состоянием действия. Когда системе требуется обновить сведения в действии (например, перед продолжением на другом устройстве), он вызывает метод `UserActivityWillSave` делегата.

Необходимо реализовать метод `UserActivityWillSave` и внести изменения в `NSUserActivity` (например, `UserInfo`, `Title`и т. д.), чтобы убедиться, что она по-прежнему отражает состояние текущего действия. Когда система вызывает метод `UserActivityWillSave`, флаг `NeedsSave` будет сброшен. При изменении каких бы то ни было свойств данных действия необходимо присвоить параметру `NeedsSave` значение `true`.

Вместо использования приведенного выше метода `UserActivityWillSave` можно автоматически управлять действиями пользователей `UIKit` или `AppKit`. Для этого задайте свойство `UserActivity` объекта ответчика и реализуйте метод `UpdateUserActivityState`. Дополнительные сведения см. в разделе [Поддержка передачи в ответах](#supporting-handoff-in-responders) ниже.

### <a name="app-framework-support"></a>Поддержка платформы приложений

Как `UIKit` (iOS), так и `AppKit` (OS X) предоставляют встроенную поддержку передачи в классы `NSDocument`, ответчика (`UIResponder`/`NSResponder`) и `AppDelegate`. Хотя каждая операционная система немного отличается, базовый механизм и API-интерфейсы одинаковы.

#### <a name="user-activities-in-document-based-apps"></a>Действия пользователей в приложениях на основе документов

Приложения iOS и OS X на основе документов автоматически имеют встроенную поддержку. Чтобы активировать эту поддержку, необходимо добавить ключ `NSUbiquitousDocumentUserActivityType` и значение для каждой записи `CFBundleDocumentTypes` в файле **info. plist** приложения.

Если этот ключ имеется, то как `NSDocument`, так и `UIDocument` автоматически создают экземпляры `NSUserActivity` для документов на основе iCloud с указанным типом. Необходимо указать тип действия для каждого типа документа, который поддерживает приложение, и несколько типов документов могут использовать один и тот же тип действия. Как `NSDocument`, так и `UIDocument` автоматически заполняют свойство `UserInfo` `NSUserActivity`, используя их значение свойства `FileURL`.

В OS X `NSUserActivity`, управляемые `AppKit` и связанные с ответчиками, автоматически становятся текущими действиями, когда окно документа становится главным окном. В iOS для `NSUserActivity` объектов, управляемых `UIKit`, необходимо либо явно вызвать метод `BecomeCurrent`, либо установить свойство `UserActivity` документа на `UIViewController`, когда приложение поступает на передний план.

`AppKit` автоматически восстановит все свойства `UserActivity`, созданные таким образом, в OS X. Это происходит, если метод `ContinueUserActivity` возвращает `false` или, если он не реализован. В этом случае документ открывается с помощью метода `OpenDocument` `NSDocumentController` и затем будет получен вызов метода `RestoreUserActivityState`.

Дополнительные сведения см. в разделе [Поддержка передачи в приложениях на основе документов](#supporting-handoff-in-document-based-apps) .

#### <a name="user-activities-and-responders"></a>Действия и ответчики пользователя

И `UIKit`, и `AppKit` могут автоматически управлять действием пользователя, если оно задано в качестве свойства `UserActivity` объекта "ответчик". Если состояние было изменено, необходимо задать для свойства `NeedsSave` `UserActivity` респондента значение `true`. Система автоматически сохраняет `UserActivity`, когда это необходимо, после предоставления времени респондента для обновления состояния путем вызова его метода `UpdateUserActivityState`.

Если несколько ответчиков совместно используют один экземпляр `NSUserActivity`, они получают обратный вызов `UpdateUserActivityState`, когда система обновляет объект действия пользователя. Ответчику необходимо вызвать метод `AddUserInfoEntries`, чтобы обновить словарь `UserInfo` `NSUserActivity`, чтобы отразить текущее состояние действия в данный момент. Словарь `UserInfo` удаляется перед каждым вызовом `UpdateUserActivityState`.

Чтобы отсоединить себя от действия, респондент может задать для свойства `UserActivity` значение `null`. Если управляемый экземпляр `NSUserActivity` Framework не имеет связанных с ним ответчиков или документов, он автоматически становится недействительным.

Дополнительные сведения см. в разделе [Поддержка передачи в ответах](#supporting-handoff-in-responders) ниже.

#### <a name="user-activities-and-the-appdelegate"></a>Действия пользователя и AppDelegate

`AppDelegate` приложения — это основная точка входа при обработке продолжения передачи. Когда пользователь отвечает на уведомление о передаче, запускается соответствующее приложение (если оно еще не запущено) и вызывается метод `WillContinueUserActivityWithType` `AppDelegate`. На этом этапе приложение должно информировать пользователя о начале продолжения.

Экземпляр `NSUserActivity` доставляется при вызове метода `ContinueUserActivity` `AppDelegate`. На этом этапе необходимо настроить пользовательский интерфейс приложения и продолжить заданное действие.

Дополнительные сведения см. в разделе [Реализация](#implementing-handoff) передачи ниже.

## <a name="enabling-handoff-in-a-xamarin-app"></a>Включение передачи в приложении Xamarin

Из-за требований безопасности, накладываемых на передачу, приложение Xamarin. iOS, использующее среду передачи, должно быть правильно настроено как на портале разработчика Apple, так и в файле проекта Xamarin. iOS.

Выполните следующие действия:

1. Войдите на [портал разработчика Apple](https://developer.apple.com).
2. Щелкните **сертификаты, идентификаторы & профили**.
3. Если вы еще не сделали этого, щелкните **идентификаторы** и создайте идентификатор приложения (например, `com.company.appname`), а затем измените существующий идентификатор.
4. Убедитесь, что служба **iCloud** проверена на наличие указанного идентификатора:

    [![](handoff-images/provision01.png "Enable the iCloud service for the given ID")](handoff-images/provision01.png#lightbox)
5. Сохраните изменения.
6. Щелкните **профили подготовки** > **разработки** и создайте новый профиль подготовки разработки для вашего приложения:

    [![](handoff-images/provision02.png "Create a new development provisioning profile for the app")](handoff-images/provision02.png#lightbox)
7. Скачайте и установите новый профиль подготовки или используйте Xcode для скачивания и установки профиля.
8. Измените параметры проекта Xamarin. iOS и убедитесь, что вы используете только что созданный профиль подготовки:

    [![](handoff-images/provision03.png "Select the provisioning profile just created")](handoff-images/provision03.png#lightbox)
9. Затем измените файл **info. plist** и убедитесь, что вы используете идентификатор приложения, который использовался для создания профиля подготовки:

    [![](handoff-images/provision04.png "Set App ID")](handoff-images/provision04.png#lightbox)
10. Прокрутите страницу до раздела **фоновые режимы** и проверьте следующие элементы:

    [![](handoff-images/provision05.png "Enable the required background modes")](handoff-images/provision05.png#lightbox)
11. Сохраните изменения во всех файлах.

После этих параметров приложение теперь готово к доступу к API-интерфейсам инфраструктуры передачи. Подробные сведения о подготовке см. в руководствах по [подготовке](~/ios/get-started/installation/device-provisioning/index.md) и [подготовке устройств к](~/ios/get-started/installation/device-provisioning/index.md) использованию.

## <a name="implementing-handoff"></a>Реализация передачи

Действия пользователей могут быть продолжены между приложениями, подписанными одним и тем же ИДЕНТИФИКАТОРом группы разработчиков, и поддерживать одинаковый тип действия. Реализация приема в приложении Xamarin. iOS требует создания объекта действия пользователя (в `UIKit` или `AppKit`), обновления состояния объекта для отслеживания действия и продолжения действия на принимающем устройстве.

### <a name="identifying-user-activities"></a>Определение действий пользователя

Первым шагом в реализации передачи является обнаружение типов действий пользователей, поддерживаемых приложением, и просмотр того, какие из этих действий являются хорошими кандидатами на продолжение на другом устройстве. Например, приложение ToDo может поддерживать изменение элементов в качестве одного _типа действия пользователя_и поддерживать просмотр списка Доступные элементы в виде другого.

Приложение может создать столько типов действий пользователей, сколько требуется, по одному для любой функции, предоставляемой приложением. Для каждого типа действий пользователя приложению необходимо будет отвести наблюдение за началом и завершением действия типа, а также поддерживать актуальную информацию о состоянии, чтобы продолжить выполнение этой задачи на другом устройстве.

Действия пользователя можно продолжить в любом приложении, подписанном с помощью одного и того же идентификатора команды, без сопоставления "один к одному" между отправляющим и принимающим приложениями. Например, данное приложение может создавать четыре различных типа действий, которые используются разными отдельными приложениями на другом устройстве. Это распространенное событие между версией Mac приложения (которое может содержать множество функций и функций) и приложений iOS, в которых каждое приложение меньше и сосредоточено на конкретной задаче.

### <a name="creating-activity-type-identifiers"></a>Создание идентификаторов типов действий

_Идентификатор типа действия_ — это короткая строка, добавляемая в `NSUserActivityTypes` массив файла **info. plist** приложения, используемый для уникальной идентификации данного типа действия пользователя. Для каждого действия, которое поддерживает приложение, в массиве будет одна запись. Apple предлагает использовать нотацию в стиле DNS для идентификатора типа действия, чтобы избежать конфликтов. Например: `com.company-name.appname.activity` для конкретных действий на основе приложения или `com.company-name.activity` для действий, которые могут выполняться в нескольких приложениях.

Идентификатор типа действия используется при создании экземпляра `NSUserActivity` для идентификации типа действия. При продолжении действия на другом устройстве тип действия (вместе с ИДЕНТИФИКАТОРом группы приложения) определяет, какое приложение следует запустить для продолжения действия.

В качестве примера мы создадим пример приложения с именем **монкэйбровсер** ([скачать здесь](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-monkeybrowser)). В этом приложении будут представлены четыре вкладки, каждый из которых имеет свой URL-адрес, Открытый в представлении веб-браузера. Пользователь сможет продолжить любые вкладки на другом устройстве iOS, где выполняется приложение.

Чтобы создать необходимые идентификаторы типов действий для поддержки такого поведения, измените файл **info. plist** и переключитесь в представление **исходного кода** . Добавьте ключ `NSUserActivityTypes` и создайте следующие идентификаторы:

[![](handoff-images/type01.png "The NSUserActivityTypes key and required identifiers in the plist editor")](handoff-images/type01.png#lightbox)

Мы создали четыре новых идентификатора типа действия, по одному для каждой вкладки в примере приложения **монкэйбровсер** . При создании собственных приложений замените содержимое массива `NSUserActivityTypes` идентификаторами типов действий, характерными для действий, которые поддерживает приложение.

### <a name="tracking-user-activity-changes"></a>Отслеживание изменений действий пользователей

При создании нового экземпляра класса `NSUserActivity` мы будем указывать экземпляр `NSUserActivityDelegate` для отслеживания изменений в состоянии действия. Например, следующий код можно использовать для отслеживания изменений состояния:

```csharp
using System;
using CoreGraphics;
using Foundation;
using UIKit;

namespace MonkeyBrowse
{
    public class UserActivityDelegate : NSUserActivityDelegate
    {
        #region Constructors
        public UserActivityDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void UserActivityReceivedData (NSUserActivity userActivity, NSInputStream inputStream, NSOutputStream outputStream)
        {
            // Log
            Console.WriteLine ("User Activity Received Data: {0}", userActivity.Title);
        }

        public override void UserActivityWasContinued (NSUserActivity userActivity)
        {
            Console.WriteLine ("User Activity Was Continued: {0}", userActivity.Title);
        }

        public override void UserActivityWillSave (NSUserActivity userActivity)
        {
            Console.WriteLine ("User Activity will be Saved: {0}", userActivity.Title);
        }
        #endregion
    }
}
```

Метод `UserActivityReceivedData` вызывается, когда поток продолжения получает данные от отправляющего устройства. Дополнительные сведения см. в разделе [Поддержка потоков продолжения](#supporting-continuation-streams) ниже.

Метод `UserActivityWasContinued` вызывается, когда другое устройство перетратило действие с текущего устройства. В зависимости от типа действия, например при добавлении нового элемента в список задач, приложению может потребоваться прервать действие на отправляющем устройстве.

Метод `UserActivityWillSave` вызывается перед сохранением и синхронизацией любых изменений в действии на локально доступных устройствах. Этот метод можно использовать для внесения последних изменений в свойство `UserInfo` экземпляра `NSUserActivity` перед его отправкой.

### <a name="creating-a-nsuseractivity-instance"></a>Создание экземпляра Нсусерактивити

Каждое действие, которое приложение хочет предоставить возможность продолжить работу на другом устройстве, должно быть инкапсулировано в `NSUserActivity` экземпляре. Приложение может создать столько действий, сколько требуется, и характер этих действий зависит от функциональности и возможностей рассматриваемого приложения. Например, приложение электронной почты может создать одно действие для создания нового сообщения, а другое — для чтения сообщения.

В нашем примере приложения создается новый `NSUserActivity` каждый раз, когда пользователь вводит новый URL-адрес в одно из представлений веб-браузера с вкладками. Следующий код сохраняет состояние данной вкладки:

```csharp
public NSString UserActivityTab1 = new NSString ("com.xamarin.monkeybrowser.tab1");
public NSUserActivity UserActivity { get; set; }
...

UserActivity = new NSUserActivity (UserActivityTab1);
UserActivity.Title = "Weather Tab";
UserActivity.Delegate = new UserActivityDelegate ();

// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);

// Inform Activity that it has been updated
UserActivity.BecomeCurrent ();
```

Он создает новый `NSUserActivity` с помощью одного из созданных выше типов действий пользователя и предоставляет удобное для чтения название действия. Он подключается к экземпляру `NSUserActivityDelegate`, созданному выше, чтобы отслеживать изменения состояния и информирует iOS о том, что это действие пользователя является текущим действием.

### <a name="populating-the-userinfo-dictionary"></a>Заполнение словаря UserInfo

Как было показано выше, свойство `UserInfo` класса `NSUserActivity` представляет собой `NSDictionary` пар "ключ-значение", используемых для определения состояния данного действия. Значения, хранящиеся в `UserInfo`, должны иметь один из следующих типов: `NSArray`, `NSData`, `NSDate`, `NSDictionary`, `NSNull`, `NSNumber`, `NSSet`, `NSString`или `NSURL`. `NSURL` значения данных, указывающие на документы iCloud, будут автоматически скорректированы, чтобы они указывали на те же документы на принимающем устройстве.

В приведенном выше примере мы создали объект `NSMutableDictionary` и заполнили его одним ключом, который предоставляет URL-адрес, который пользователь просматривал в данный момент на данной вкладке. Метод `AddUserInfoEntries` действия пользователя использовался для обновления действия данными, которые будут использоваться для восстановления действия на принимающем устройстве:

```csharp
// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);
```

Apple рекомендует хранить данные на самом простом, чтобы обеспечить своевременную отправку данных на принимающее устройство. Если требуется отправить более подробную информацию, например изображение, присоединенное к документу, необходимо отправлять, следует использовать потоки продолжения. Дополнительные сведения см. в разделе [Поддержка потоков продолжения](#supporting-continuation-streams) ниже.

### <a name="continuing-an-activity"></a>Продолжение действия

Переданное устройство автоматически уведомляет локальные устройства iOS и OS X, находящиеся в физической близости к исходному устройству, и подписалась на одну и ту же учетную запись iCloud, доступную для непрерывных действий пользователя. Если пользователь решит продолжить действие на новом устройстве, система запустит соответствующее приложение (на основе идентификатора команды и типа действия), а также сведения о `AppDelegate`, которые должны быть выполнены.

Во-первых, вызывается метод `WillContinueUserActivityWithType`, поэтому приложение может информировать пользователя о том, что продолжение должно начаться. Мы используем следующий код в файле **AppDelegate.CS** нашего примера приложения, чтобы начать обработку продолжения:

```csharp
public NSString UserActivityTab1 = new NSString ("com.xamarin.monkeybrowser.tab1");
public NSString UserActivityTab2 = new NSString ("com.xamarin.monkeybrowser.tab2");
public NSString UserActivityTab3 = new NSString ("com.xamarin.monkeybrowser.tab3");
public NSString UserActivityTab4 = new NSString ("com.xamarin.monkeybrowser.tab4");
...

public FirstViewController Tab1 { get; set; }
public SecondViewController Tab2 { get; set;}
public ThirdViewController Tab3 { get; set; }
public FourthViewController Tab4 { get; set; }
...

public override bool WillContinueUserActivity (UIApplication application, string userActivityType)
{
    // Report Activity
    Console.WriteLine ("Will Continue Activity: {0}", userActivityType);

    // Take action based on the user activity type
    switch (userActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Inform view that it's going to be modified
        Tab1.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Inform view that it's going to be modified
        Tab2.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Inform view that it's going to be modified
        Tab3.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Inform view that it's going to be modified
        Tab4.PreparingToHandoff ();
        break;
    }

    // Inform system we handled this
    return true;
}
```

В приведенном выше примере каждый контроллер представления регистрируется в `AppDelegate` и имеет открытый метод `PreparingToHandoff`, который отображает индикатор действия и сообщение, позволяющее пользователю узнать о том, что действие будет передаваться на текущее устройство. Пример

```csharp
private void ShowBusy(string reason) {

    // Display reason
    BusyText.Text = reason;

    //Define Animation
    UIView.BeginAnimations("Show");
    UIView.SetAnimationDuration(1.0f);

    Handoff.Alpha = 0.5f;

    //Execute Animation
    UIView.CommitAnimations();
}
...

public void PreparingToHandoff() {
    // Inform caller
    ShowBusy ("Continuing Activity...");
}
```

Для фактического продолжения данного действия будет вызван `ContinueUserActivity` `AppDelegate`. Опять же, из нашего примера приложения:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Report Activity
    Console.WriteLine ("Continuing User Activity: {0}", userActivity.ToString());

    // Get input and output streams from the Activity
    userActivity.GetContinuationStreams ((NSInputStream arg1, NSOutputStream arg2, NSError arg3) => {
        // Send required data via the streams
        // ...
    });

    // Take action based on the Activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Preform handoff
        Tab1.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab1});
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Preform handoff
        Tab2.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab2});
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Preform handoff
        Tab3.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab3});
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Preform handoff
        Tab4.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab4});
        break;
    }

    // Inform system we handled this
    return true;
}
```

Метод public `PerformHandoff` каждого контроллера представления фактически предварительно формирует передачу и восстанавливает действие на текущем устройстве. В этом примере он отображает тот же URL-адрес на данной вкладке, которую пользователь просматривает на другом устройстве. Пример

```csharp
private void HideBusy() {

    //Define Animation
    UIView.BeginAnimations("Hide");
    UIView.SetAnimationDuration(1.0f);

    Handoff.Alpha = 0f;

    //Execute Animation
    UIView.CommitAnimations();
}
...

public void PerformHandoff(NSUserActivity activity) {

    // Hide busy indicator
    HideBusy ();

    // Extract URL from dictionary
    var url = activity.UserInfo ["Url"].ToString ();

    // Display value
    URL.Text = url;

    // Display the give webpage
    WebView.LoadRequest(new NSUrlRequest(NSUrl.FromString(url)));

    // Save activity
    UserActivity = activity;
    UserActivity.BecomeCurrent ();

}
```

Метод `ContinueUserActivity` включает `UIApplicationRestorationHandler`, которые можно вызывать для возобновления действия на основе документа или респондента. При вызове в обработчик восстановления необходимо передать объекты `NSArray` или restorable. Пример:

```csharp
completionHandler (new NSObject[]{Tab4});
```

Для каждого переданного объекта будет вызван его метод `RestoreUserActivityState`. Затем каждый объект может использовать данные в словаре `UserInfo` для восстановления своего собственного состояния. Пример:

```csharp
public override void RestoreUserActivityState (NSUserActivity activity)
{
    base.RestoreUserActivityState (activity);

    // Log activity
    Console.WriteLine ("Restoring Activity {0}", activity.Title);
}
```

Если для приложений на основе документов не реализован метод `ContinueUserActivity` или он возвращает `false`, `UIKit` или `AppKit` может автоматически возобновить действие. Дополнительные сведения см. в разделе [Поддержка передачи в приложениях на основе документов](#supporting-handoff-in-document-based-apps) .

### <a name="failing-handoff-gracefully"></a>Неправильное переработка

Поскольку передаваемые данные полагаются на передачу данных между коллекцией неслабо подключенных устройств iOS и OS X, процесс передачи иногда может завершиться сбоем. Вы должны разрабатывать приложение для корректной обработке этих сбоев и информировать пользователей о любых возникающих ситуациях.

В случае сбоя будет вызван метод `DidFailToContinueUserActivitiy` `AppDelegate`. Пример:

```csharp
public override void DidFailToContinueUserActivitiy (UIApplication application, string userActivityType, NSError error)
{
    // Log information about the failure
    Console.WriteLine ("User Activity {0} failed to continue. Error: {1}", userActivityType, error.LocalizedDescription);
}
```

Для предоставления пользователю сведений об ошибке следует использовать предоставленный `NSError`.

## <a name="native-app-to-web-browser-handoff"></a>Перепередачи собственного приложения в веб-браузер

Пользователю может потребоваться продолжить действие, не устанавливая на нужное устройство соответствующее собственное приложение. В некоторых ситуациях интерфейс на основе веб-интерфейса может предоставлять необходимые функции, и действие можно продолжать. Например, учетная запись электронной почты пользователя может предоставлять пользовательский интерфейс веб-интерфейса для создания и чтения сообщений.

Если исходное, собственное приложение знает URL-адрес Web interface (и необходимый синтаксис для определения возобновляемого элемента), он может закодировать эту информацию в свойстве `WebpageURL` экземпляра `NSUserActivity`. Если на принимающем устройстве не установлено соответствующее собственное приложение для выполнения продолжения, можно вызвать предоставленный веб-интерфейс.

## <a name="web-browser-to-native-app-handoff"></a>Веб-браузер для передачи собственного приложения

Если пользователь использовал веб-интерфейс на исходном устройстве, а собственное приложение на принимающем устройстве заявляет часть домена свойства `WebpageURL`, система будет использовать это приложение для выполнения продолжения. Новое устройство получит экземпляр `NSUserActivity`, который помечает тип действия как `BrowsingWeb`, а `WebpageURL` будет содержать URL-адрес, который пользователь посетил, словарь `UserInfo` будет пустым.

Чтобы приложение принимало участие в этом типе передачи, оно должно запрашивать домен в `com.apple.developer.associated-domains`ом сопоставлении с форматом `<service>:<fully qualified domain name>` (например: `activity continuation:company.com`).

Если указанный домен соответствует значению `WebpageURL` свойства, то при передаче загружается список утвержденных идентификаторов приложений с веб-сайта в этом домене. Веб-сайт должен предоставить список утвержденных идентификаторов в подписанном файле JSON с именем **Apple-App-site-Association** (например, `https://company.com/apple-app-site-association`).

Этот JSON-файл содержит словарь, указывающий список идентификаторов приложений в форме `<team identifier>.<bundle identifier>`. Пример:

```csharp
{
    "activitycontinuation": {
        "apps": [    "YWBN8XTPBJ.com.company.FirstApp",
            "YWBN8XTPBJ.com.company.SecondApp" ]
    }
}
```

Чтобы подписать JSON-файл (чтобы он был правильно `Content-Type` `application/pkcs7-mime`), используйте приложение **терминала** и команду `openssl` с сертификатом и ключом, выданным центром сертификации, которому доверяет iOS (см. список [https://support.apple.com/kb/ht5012](https://support.apple.com/kb/ht5012) для получения списка). Пример:

```csharp
echo '{"activitycontinuation":{"apps":["YWBN8XTPBJ.com.company.FirstApp",
"YWBN8XTPBJ.com.company.SecondApp"]}}' > json.txt

cat json.txt | openssl smime -sign -inkey company.com.key
-signer company.com.pem
-certfile intermediate.pem
-noattr -nodetach
-outform DER > apple-app-site-association
```

Команда `openssl` выводит подписанный файл JSON, который вы поместите на веб-сайт, по URL-адресу **Apple-App-site-Association** . Пример:

```csharp
https://example.com/apple-app-site-association.
```

Приложение получит все действия, у которых домен `WebpageURL` находится в `com.apple.developer.associated-domains` прав. Поддерживаются только протоколы `http` и `https`. любой другой протокол вызовет исключение.

## <a name="supporting-handoff-in-document-based-apps"></a>Поддержка передачи в приложениях на основе документов

Как упоминалось выше, в iOS и OS X приложения на основе документов автоматически поддерживают передачу документов на основе iCloud, если файл **info. plist** приложения содержит `CFBundleDocumentTypes`ный ключ `NSUbiquitousDocumentUserActivityType`. Пример:

```xml
<key>CFBundleDocumentTypes</key>
<array>
    <dict>
        <key>CFBundleTypeName</key>
        <string>NSRTFDPboardType</string>
        . . .
        <key>LSItemContentTypes</key>
        <array>
        <string>com.myCompany.rtfd</string>
        </array>
        . . .
        <key>NSUbiquitousDocumentUserActivityType</key>
        <string>com.myCompany.myEditor.editing</string>
    </dict>
</array>
```

В этом примере строка представляет собой обозначение приложения в обратную DNS с именем добавленного действия. Если этот способ введен, записи типа действия не нужно повторять в массиве `NSUserActivityTypes` файла **info. plist** .

Автоматически созданный объект действия пользователя (доступный через свойство `UserActivity` документа) может ссылаться на другие объекты в приложении и использоваться для восстановления состояния при продолжении. Например, для трассировки выбора элементов и позиции документа. Необходимо задать эти действия `NeedsSave` свойства для `true` при изменении состояния и обновлении словаря `UserInfo` в методе `UpdateUserActivityState`.

Свойство `UserActivity` может использоваться из любого потока и соответствует протоколу "ключ-значение" (кво), поэтому его можно использовать для синхронизации документа по мере его перемещения в iCloud и из него. При закрытии документа свойство `UserActivity` будет недействительным.

Дополнительные сведения см. в документации Apple о [поддержке действий пользователей в приложениях на основе документов](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/HandoffFundamentals/HandoffFundamentals.html#//apple_ref/doc/uid/TP40014338-CH3-SW5) .

## <a name="supporting-handoff-in-responders"></a>Поддержка передачи в Ответчиках

Вы можете связать с действиями ответчики (наследуемые от `UIResponder` в iOS или `NSResponder` в OS X), задав их свойства `UserActivity`. Система автоматически сохраняет свойство `UserActivity` в нужное время, вызывая метод `UpdateUserActivityState` респондента для добавления текущих данных в объект действия пользователя с помощью метода `AddUserInfoEntriesFromDictionary`.

## <a name="supporting-continuation-streams"></a>Поддержка потоков продолжения

Это может быть ситуация, когда объем сведений, необходимых для продолжения действия, не может быть эффективно передан исходными полезными данными. В таких ситуациях принимающее приложение может установить один или несколько потоков между самим собой и исходным приложением для передачи данных.

Исходное приложение установит для свойства `SupportsContinuationStreams` экземпляра `NSUserActivity` значение `true`. Пример:

```csharp
// Create a new user Activity to support this tab
UserActivity = new NSUserActivity (ThisApp.UserActivityTab1){
    Title = "Weather Tab",
    SupportsContinuationStreams = true
};
UserActivity.Delegate = new UserActivityDelegate ();

// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);

// Inform Activity that it has been updated
UserActivity.BecomeCurrent ();
```

Принимающее приложение может затем вызвать метод `GetContinuationStreams` `NSUserActivity` в `AppDelegate` для создания потока. Пример:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Report Activity
    Console.WriteLine ("Continuing User Activity: {0}", userActivity.ToString());

    // Get input and output streams from the Activity
    userActivity.GetContinuationStreams ((NSInputStream arg1, NSOutputStream arg2, NSError arg3) => {
        // Send required data via the streams
        // ...
    });

    // Take action based on the Activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Preform handoff
        Tab1.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab1});
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Preform handoff
        Tab2.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab2});
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Preform handoff
        Tab3.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab3});
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Preform handoff
        Tab4.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab4});
        break;
    }

    // Inform system we handled this
    return true;
}
```

На исходном устройстве делегат действия пользователя получает потоки, вызывая метод `DidReceiveInputStream` для предоставления запрошенных данных, чтобы продолжить действия пользователя на возобновляемом устройстве.

Вы будете использовать `NSInputStream`, чтобы предоставить доступ только для чтения к потоковой информации, а `NSOutputStream` предоставить доступ только для записи. Потоки должны использоваться в запросах и ответах, когда принимающее приложение запрашивает больше данных, а исходное приложение предоставляет его. Таким образом, данные, записанные в поток вывода на исходном устройстве, считываются из входного потока на продолжающемся устройстве и наоборот.

Даже в ситуациях, когда требуется поток продолжения, между двумя приложениями должно быть не меньше обратной и не более.

Дополнительные сведения см. в документации Apple об [использовании потоков продолжения](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/AdoptingHandoff/AdoptingHandoff.html#//apple_ref/doc/uid/TP40014338-CH2-SW13) .

## <a name="handoff-best-practices"></a>Рекомендации по передаче

Успешная реализация простого продолжения действия пользователя с помощью передачи требует тщательного проектирования из-за всех вовлеченных в него компонентов. Компания Apple предлагает принять следующие рекомендации для приложений с поддержкой передачи:

- Разработайте действия пользователя, чтобы они требовали наименьшей полезной нагрузки, чтобы связать состояние действия, которое должно быть продолжено. Чем больше полезная нагрузка, тем больше времени занимает продолжение.
- Если необходимо переносить большие объемы данных для успешного продолжения, примите во внимание затраты, связанные с настройкой и нагрузкой сети.
- Обычно для большого приложения Mac создаются действия пользователя, которые обрабатываются несколькими, более мелкими приложениями на устройствах iOS. Различные версии приложений и ОС должны быть спроектированы для совместной работы или неправильного сбоя.
- При указании типов действий используйте нотацию обратных DNS, чтобы избежать конфликтов. Если действие относится к конкретному приложению, его имя должно быть включено в определение типа (например `com.myCompany.myEditor.editing`). Если действие может работать в нескольких приложениях, удалите имя приложения из определения (например `com.myCompany.editing`).
- Если приложению необходимо обновить состояние действий пользователя (`NSUserActivity`), задайте для свойства `NeedsSave` значение `true`. В нужное время передача будет вызывать метод `UserActivityWillSave` делегата, чтобы при необходимости можно было обновить словарь `UserInfo`.
- Так как процесс передачи не может быть немедленно инициализирован на принимающем устройстве, следует реализовать `WillContinueUserActivity` `AppDelegate`и сообщить пользователю о том, что продолжение должно начаться.

## <a name="example-handoff-app"></a>Пример переданного приложения

В качестве примера использования передачи в приложение Xamarin. iOS мы включили пример приложения [**монкэйбровсер**](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-monkeybrowser) с этим руководством. В приложении есть четыре вкладки, которые пользователь может использовать для просмотра веб-страниц, каждый с определенным типом действия: Погода, избранное, чашка кофе и работа.

На любой вкладке, когда пользователь вводит новый URL-адрес и касанием кнопки **Go (перейти** ), для этой вкладки создается новый `NSUserActivity`, содержащий URL-адрес, просматриваемый пользователем в данный момент:

[![](handoff-images/handoff01.png "Example Handoff App")](handoff-images/handoff01.png#lightbox)

Если на другом устройстве пользователя установлено приложение **монкэйбровсер** , оно входит в iCloud с одной и той же учетной записью пользователя, находится в той же сети и в близком расположении к указанному выше устройству, действие передачи будет отображаться на начальном экране (в нижнем левый угол):

[![](handoff-images/handoff02.png "The Handoff Activity displayed on the home screen in the lower left hand corner")](handoff-images/handoff02.png#lightbox)

Если пользователь перетаскивает вверх по значку передачи, приложение будет запущено и действие пользователя, указанное в `NSUserActivity`, будет продолжено на новом устройстве:

[![](handoff-images/handoff03.png "The User Activity continued on the new device")](handoff-images/handoff03.png#lightbox)

Когда активность пользователя успешно отправлена на другое устройство Apple, `NSUserActivity` отправляющего устройства получит вызов метода `UserActivityWasContinued` на своем `NSUserActivityDelegate`, чтобы сообщить ему о том, что действия пользователя успешно переданы на другое устройство.

## <a name="summary"></a>Сводка

В этой статье приводятся общие сведения о платформе передачи данных, используемой для продолжения действий пользователя между несколькими устройствами Apple пользователя. Далее показано, как включить и реализовать передачу в приложении Xamarin. iOS. И, наконец, обсуждаются различные типы доступных продолженностей и рекомендации по передаче.

## <a name="related-links"></a>Связанные ссылки

- [Примеры для iOS 9](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [Пример Монкэйбровсер](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-monkeybrowser)
- [iOS 9 для разработчиков](https://developer.apple.com/ios/pre-release/)
- [Новые возможности iOS 9,0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Хомекитдевелопер Guide](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [Рекомендации по пользовательскому интерфейсу HomeKit](https://developer.apple.com/homekit/ui-guidelines/)
- [Справочник по HomeKit Framework](https://developer.apple.com/library/ios/home_kit_framework_ref)
