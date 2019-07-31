---
title: Передачи в Xamarin. iOS
description: В этой статье рассматривается работа с передачей в приложение Xamarin. iOS для передачи действий пользователя между приложениями, работающими на других устройствах пользователя.
ms.prod: xamarin
ms.assetid: 405F966A-4085-4621-AA15-33D663AD15CD
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 28c5086833ceb1dc8550e513b120f7355aa9bebe
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656579"
---
# <a name="handoff-in-xamarinios"></a>Передачи в Xamarin. iOS

_В этой статье рассматривается работа с передачей в приложение Xamarin. iOS для передачи действий пользователя между приложениями, работающими на других устройствах пользователя._

Компания Apple представила передачу данных в iOS 8 и OS X Yosemite (10,10), чтобы предоставить пользователю общий механизм передачи действий, запущенных на одном из устройств, на другое устройство, которое поддерживает то же самое приложение, или другое приложение, поддерживающее то же действие.

[![](handoff-images/handoff02.png "Пример выполнения операции передачи")](handoff-images/handoff02.png#lightbox)

В этой статье рассматривается Включение совместного использования действий в приложении Xamarin. iOS и подробное рассмотрение платформы передачи:

## <a name="about-handoff"></a>О передаче

Прием (также известный как непрерывность) появился в Apple в iOS 8 и OS X Yosemite (10,10) как способ запуска действия на одном из устройств (iOS или Mac) и продолжать это действие на другом устройстве (как определено Иклауом пользователя). d учетная запись).

Переданная функция была расширена в iOS 9, а также поддерживает новые возможности расширенного поиска. Дополнительные сведения см. в документации по [усовершенствованиям поиска](~/ios/platform/search/index.md) .

Например, пользователь может запустить электронное письмо на устройстве iPhone и без проблем продолжить письмо на своем компьютере Mac, при этом вся информация о сообщениях заполняется и курсор находится в том же расположении, в котором они были в iOS.

Любое приложение, использующее один и тот же _идентификатор команды_ , может использовать передачу для продолжения действий пользователей в приложениях при условии, что эти приложения либо доставляются через магазин приложений iTunes, либо подписаны зарегистрированным разработчиком (для приложений Mac, Enterprise или ad hoc).

Все `NSDocument` приложения `UIDocument` , основанные на или, автоматически имеют встроенную поддержку, и для получения поддержки необходимо внести минимальные изменения.

### <a name="continuing-user-activities"></a>Продолжение действий пользователя

Класс (вместе с некоторыми незначительными изменениями `UIKit` в `AppKit`и) обеспечивает поддержку определения действия пользователя, которое потенциально может быть продолжено на другом устройстве пользователя. `NSUserActivity`

Чтобы действие передавалось на другое устройство пользователя, оно должно быть инкапсулировано в экземпляре `NSUserActivity`, помеченном как _текущее действие_, иметь набор полезных данных (данные, используемые для выполнения продолжения), а действие должно быть передано на это устройство.

Для определения действия, которое должно быть продолжено с помощью iCloud, передается некоторая информация об минимальном объеме данных.

На принимающем устройстве пользователь получит уведомление о том, что действие доступно для продолжения. Если пользователь решит продолжить действие на новом устройстве, запустится указанное приложение (если оно еще не запущено), а полезные данные из `NSUserActivity` будут использоваться для перезапуска действия.

[![](handoff-images/handoffinteractions.png "Обзор продолжающихся действий пользователя")](handoff-images/handoffinteractions.png#lightbox)

Только приложения с одинаковым ИДЕНТИФИКАТОРом группы разработчиков и отвечают на данный _тип действия_ , могут быть продолжены. Приложение определяет типы действий, которые он поддерживает, в рамках `NSUserActivityTypes` ключа файла **info. plist** . В этом случае продолжающееся устройство выбирает приложение для выполнения продолжения на основе идентификатора группы, типа действия и, при необходимости, _названия действия_.

Принимающее приложение использует сведения из `NSUserActivity` `UserInfo` словаря для настройки пользовательского интерфейса и восстановления состояния данного действия, чтобы переход был незаметным для конечного пользователя.

Если продолжение требует больше информации `NSUserActivity`, чем может быть эффективно отправлено через, возобновляемое приложение может отправить вызов исходному приложению и установить один или несколько потоков для передачи необходимых данных. Например, если действие редактировало большой текстовый документ с несколькими изображениями, для передачи сведений, необходимых для продолжения действия на принимающем устройстве, потребуется потоковая передача. Дополнительные сведения см. в разделе [Поддержка потоков продолжения](#supporting-continuation-streams) ниже.

Как уже говорилось `NSDocument` выше `UIDocument` , или приложения на основе автоматически имеют встроенную поддержку. Дополнительные сведения см. в разделе [Поддержка передачи в приложениях на основе документов](#supporting-handoff-in-document-based-apps) ниже.

### <a name="the-nsuseractivity-class"></a>Класс Нсусерактивити

`NSUserActivity` Класс является основным объектом в переданном обмене и используется для инкапсуляции состояния действия пользователя, доступного для продолжения. Приложение будет создавать экземпляр копии `NSUserActivity` для любой поддерживаемой им активности и продолжать работу на другом устройстве. Например, редактор документов создаст действие для каждого документа, открытого в данный момент. Однако только самый первый документ (отображается в самом самом простом окне или вкладке) является _текущим действием_ и таким образом доступным для продолжения.

Экземпляр `NSUserActivity` определяется `Title` как свойствами и. `ActivityType` Свойство `UserInfo` Dictionary используется для передачи сведений о состоянии действия. Задайте для `NeedsSave` `true` свойства значение, если требуется отложенная загрузка сведений о состоянии с помощью `NSUserActivity`делегата. Используйте метод, чтобы объединить новые данные из других клиентов `UserInfo` в словарь в соответствии с требованиями для сохранения состояния действия. `AddUserInfoEntries`

### <a name="the-nsuseractivitydelegate-class"></a>Класс Нсусерактивитиделегате

Используется для сохранения сведений `NSUserActivity`в `UserInfo` словаре и синхронизации с текущим состоянием действия. `NSUserActivityDelegate` Когда системе требуется обновить сведения в действии (например, перед продолжением на другом устройстве), он вызывает `UserActivityWillSave` метод делегата.

Необходимо реализовать `UserActivityWillSave` метод и внести изменения `NSUserActivity` в (например `UserInfo`, `Title`, и т. д.), чтобы убедиться, что он по-прежнему отражает состояние текущего действия. Когда система вызывает `UserActivityWillSave` метод `NeedsSave` , флаг будет сброшен. При изменении каких бы то ни было свойств данных действия необходимо `NeedsSave` `true` снова задать значение.

Вместо того чтобы использовать `UserActivityWillSave` приведенный выше метод, можно `UIKit` автоматически или `AppKit` управлять пользовательской деятельностью. Для этого задайте `UserActivity` свойство объекта ответчика и `UpdateUserActivityState` реализуйте метод. Дополнительные сведения см. в разделе [Поддержка передачи в ответах](#supporting-handoff-in-responders) ниже.

### <a name="app-framework-support"></a>Поддержка платформы приложений

Оба `UIKit` (IOS) и `AppKit` (OS X) предоставляют встроенную `NSDocument`поддержку для передачи в классы, ответчик (`NSResponder``UIResponder`/) и `AppDelegate` . Хотя каждая операционная система немного отличается, базовый механизм и API-интерфейсы одинаковы.

#### <a name="user-activities-in-document-based-apps"></a>Действия пользователей в приложениях на основе документов

Приложения iOS и OS X на основе документов автоматически имеют встроенную поддержку. Чтобы активировать эту поддержку, необходимо добавить `NSUbiquitousDocumentUserActivityType` ключ и значение для каждой `CFBundleDocumentTypes` записи в файле **info. plist** приложения.

Если этот ключ имеется, `NSDocument` и, и `UIDocument` автоматически создают `NSUserActivity` экземпляры для документов на основе iCloud с указанным типом. Необходимо указать тип действия для каждого типа документа, который поддерживает приложение, и несколько типов документов могут использовать один и тот же тип действия. `UserInfo` `NSUserActivity` `FileURL` И, `UIDocument` и автоматически заполняют свойство объекта значением их свойства. `NSDocument`

В OS X `AppKit` управляемый `NSUserActivity` объект и, связанный с ответчиками, автоматически становится текущим действием, когда окно документа становится главным окном. В iOS `NSUserActivity` для объектов, управляемых с `UIKit`помощью, необходимо либо вызвать `BecomeCurrent` метод явным образом, либо задать `UserActivity` свойство документа для, `UIViewController` когда приложение поступает на передний план.

`AppKit`автоматически восстановит все `UserActivity` свойства, созданные таким образом, в OS X. Это происходит, `ContinueUserActivity` если метод возвращает `false` или, если он не реализован. В этом случае документ открывается с помощью `OpenDocument` метода, `NSDocumentController` а затем `RestoreUserActivityState` он получает вызов метода.

Дополнительные сведения см. в разделе [Поддержка передачи в приложениях на основе документов](#supporting-handoff-in-document-based-apps) .

#### <a name="user-activities-and-responders"></a>Действия и ответчики пользователя

И, `AppKit` и могут автоматически управлять действием пользователя, если оно задано `UserActivity` в качестве свойства объекта "ответчик". `UIKit` Если состояние изменено, необходимо задать `NeedsSave` `UserActivity` для `true`свойства респондента значение. Система автоматически сохранит значение `UserActivity` при необходимости, после того как сторона отправит время на обновление состояния, вызвав его `UpdateUserActivityState` метод.

Если несколько ответчиков совместно используют один `NSUserActivity` экземпляр, они `UpdateUserActivityState` получают обратный вызов, когда система обновляет объект действия пользователя. Ответчику необходимо вызвать `AddUserInfoEntries` метод, чтобы обновить `UserInfo` словарь, `NSUserActivity`чтобы отразить текущее состояние действия в данный момент. Словарь удаляется перед каждым `UpdateUserActivityState` вызовом. `UserInfo`

Чтобы отсоединить себя от действия, респондент может задать `UserActivity` `null`свойству значение. Если управляемый `NSUserActivity` экземпляр платформы приложений не имеет больше связанных ответчиков или документов, он автоматически становится недействительным.

Дополнительные сведения см. в разделе [Поддержка передачи в ответах](#supporting-handoff-in-responders) ниже.

#### <a name="user-activities-and-the-appdelegate"></a>Действия пользователя и AppDelegate

Приложение `AppDelegate` является основной точкой входа при обработке продолжения передачи. Когда пользователь отвечает на уведомление о передаче, запускается соответствующее приложение (если оно еще не запущено) и `WillContinueUserActivityWithType` вызывается метод. `AppDelegate` На этом этапе приложение должно информировать пользователя о начале продолжения.

Экземпляр доставляется `AppDelegate`при `ContinueUserActivity` вызове метода. `NSUserActivity` На этом этапе необходимо настроить пользовательский интерфейс приложения и продолжить заданное действие.

Дополнительные сведения см. в разделе [Реализация](#implementing-handoff) передачи ниже.

## <a name="enabling-handoff-in-a-xamarin-app"></a>Включение передачи в приложении Xamarin

Из-за требований безопасности, накладываемых на передачу, приложение Xamarin. iOS, использующее среду передачи, должно быть правильно настроено как на портале разработчика Apple, так и в файле проекта Xamarin. iOS.

Выполните следующие действия:

1. Войдите на [портал разработчика Apple](https://developer.apple.com).
2. Щелкните **сертификаты, идентификаторы & профили**.
3. Если вы еще не сделали этого, щелкните **идентификаторы** и создайте идентификатор для своего приложения (например `com.company.appname`,), иначе измените существующий идентификатор.
4. Убедитесь, что служба **iCloud** проверена на наличие указанного идентификатора:

    [![](handoff-images/provision01.png "Включение службы iCloud для указанного идентификатора")](handoff-images/provision01.png#lightbox)
5. Сохраните изменения.
6. Щелкните **Профили** > подготовки**Разработка** и создайте новый профиль подготовки разработки для вашего приложения:

    [![](handoff-images/provision02.png "Создание нового профиля подготовки для разработки для приложения")](handoff-images/provision02.png#lightbox)
7. Скачайте и установите новый профиль подготовки или используйте Xcode для скачивания и установки профиля.
8. Измените параметры проекта Xamarin. iOS и убедитесь, что вы используете только что созданный профиль подготовки:

    [![](handoff-images/provision03.png "Выберите только что созданный профиль подготовки")](handoff-images/provision03.png#lightbox)
9. Затем измените файл **info. plist** и убедитесь, что вы используете идентификатор приложения, который использовался для создания профиля подготовки:

    [![](handoff-images/provision04.png "Задать идентификатор приложения")](handoff-images/provision04.png#lightbox)
10. Прокрутите страницу до раздела **фоновые режимы** и проверьте следующие элементы:

    [![](handoff-images/provision05.png "Включение требуемых фоновых режимов")](handoff-images/provision05.png#lightbox)
11. Сохраните изменения во всех файлах.

После этих параметров приложение теперь готово к доступу к API-интерфейсам инфраструктуры передачи. Подробные сведения о подготовке см. в руководствах по [подготовке](~/ios/get-started/installation/device-provisioning/index.md) и [подготовке устройств к](~/ios/get-started/installation/device-provisioning/index.md) использованию.

## <a name="implementing-handoff"></a>Реализация передачи

Действия пользователей могут быть продолжены между приложениями, подписанными одним и тем же ИДЕНТИФИКАТОРом группы разработчиков, и поддерживать одинаковый тип действия. Реализация приема в приложении Xamarin. IOS требует создания объекта действия пользователя (в `UIKit` или `AppKit`), обновления состояния объекта для отслеживания действия и продолжения действия на принимающем устройстве.

### <a name="identifying-user-activities"></a>Определение действий пользователя

Первым шагом в реализации передачи является обнаружение типов действий пользователей, поддерживаемых приложением, и просмотр того, какие из этих действий являются хорошими кандидатами на продолжение на другом устройстве. Например, приложение ToDo может поддерживать изменение элементов в качестве одного _типа действия пользователя_и поддерживать просмотр списка Доступные элементы в виде другого.

Приложение может создать столько типов действий пользователей, сколько требуется, по одному для любой функции, предоставляемой приложением. Для каждого типа действий пользователя приложению необходимо будет отвести наблюдение за началом и завершением действия типа, а также поддерживать актуальную информацию о состоянии, чтобы продолжить выполнение этой задачи на другом устройстве.

Действия пользователя можно продолжить в любом приложении, подписанном с помощью одного и того же идентификатора команды, без сопоставления "один к одному" между отправляющим и принимающим приложениями. Например, данное приложение может создавать четыре различных типа действий, которые используются разными отдельными приложениями на другом устройстве. Это распространенное событие между версией Mac приложения (которое может содержать множество функций и функций) и приложений iOS, в которых каждое приложение меньше и сосредоточено на конкретной задаче.

### <a name="creating-activity-type-identifiers"></a>Создание идентификаторов типов действий

_Идентификатор типа действия_ — это короткая строка, добавляемая `NSUserActivityTypes` в массив файла **info. plist** приложения, используемый для уникальной идентификации данного типа действия пользователя. Для каждого действия, которое поддерживает приложение, в массиве будет одна запись. Apple предлагает использовать нотацию в стиле DNS для идентификатора типа действия, чтобы избежать конфликтов. Например: `com.company-name.appname.activity` для конкретных действий на основе приложения или `com.company-name.activity` для действий, которые могут выполняться в нескольких приложениях.

Идентификатор типа действия используется при создании `NSUserActivity` экземпляра для идентификации типа действия. При продолжении действия на другом устройстве тип действия (вместе с ИДЕНТИФИКАТОРом группы приложения) определяет, какое приложение следует запустить для продолжения действия.

В качестве примера мы создадим пример приложения с именем **монкэйбровсер** ([скачать здесь](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-monkeybrowser)). В этом приложении будут представлены четыре вкладки, каждый из которых имеет свой URL-адрес, Открытый в представлении веб-браузера. Пользователь сможет продолжить любые вкладки на другом устройстве iOS, где выполняется приложение.

Чтобы создать необходимые идентификаторы типов действий для поддержки такого поведения, измените файл **info. plist** и переключитесь в представление **исходного кода** . `NSUserActivityTypes` Добавьте ключ и создайте следующие идентификаторы:

[![](handoff-images/type01.png "Ключ Нсусерактивититипес и необходимые идентификаторы в редакторе plist")](handoff-images/type01.png#lightbox)

Мы создали четыре новых идентификатора типа действия, по одному для каждой вкладки в примере приложения **монкэйбровсер** . При создании собственных приложений замените содержимое `NSUserActivityTypes` массива идентификаторами типов действий, характерными для действий, которые поддерживает приложение.

### <a name="tracking-user-activity-changes"></a>Отслеживание изменений действий пользователей

При создании нового экземпляра `NSUserActivity` класса мы будем `NSUserActivityDelegate` указывать экземпляр для отслеживания изменений в состоянии действия. Например, следующий код можно использовать для отслеживания изменений состояния:

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

`UserActivityReceivedData` Метод вызывается, когда поток продолжения получает данные с отправляющего устройства. Дополнительные сведения см. в разделе [Поддержка потоков продолжения](#supporting-continuation-streams) ниже.

Метод `UserActivityWasContinued` вызывается, когда другое устройство перетратило действие с текущего устройства. В зависимости от типа действия, например при добавлении нового элемента в список задач, приложению может потребоваться прервать действие на отправляющем устройстве.

`UserActivityWillSave` Метод вызывается перед сохранением и синхронизацией всех изменений действия на локально доступных устройствах. Этот метод можно использовать для внесения последних поминутных изменений `UserInfo` в свойство `NSUserActivity` экземпляра перед его отправкой.

### <a name="creating-a-nsuseractivity-instance"></a>Создание экземпляра Нсусерактивити

Каждое действие, которое приложение хочет предоставить возможность продолжить работу на другом устройстве, должно быть инкапсулировано в `NSUserActivity` экземпляре. Приложение может создать столько действий, сколько требуется, и характер этих действий зависит от функциональности и возможностей рассматриваемого приложения. Например, приложение электронной почты может создать одно действие для создания нового сообщения, а другое — для чтения сообщения.

Для нашего примера приложения создается новое `NSUserActivity` значение при каждом вводе нового URL-адреса в одном из представлений веб-браузера с вкладками. Следующий код сохраняет состояние данной вкладки:

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

Он создает новый `NSUserActivity` с помощью одного из созданных выше типов действий пользователя и предоставляет удобное для чтения название действия. Он подключается к созданному выше `NSUserActivityDelegate` экземпляру, чтобы отслеживать изменения состояния и информирует iOS о том, что это действие пользователя является текущим действием.

### <a name="populating-the-userinfo-dictionary"></a>Заполнение словаря UserInfo

Как мы видели выше, `UserInfo` свойство `NSUserActivity` класса представляет собой `NSDictionary` пары "ключ-значение", используемые для определения состояния данного действия. `UserInfo` Значения `NSString` `NSSet` `NSDate` `NSData` `NSArray` `NSDictionary` ,хранящиесяв`NSNull`, должны иметь один из следующих типов:,,,,, `NSURL`,, или. `NSNumber` `NSURL`значения данных, указывающие на документы iCloud, будут автоматически скорректированы таким образом, чтобы они указывали на те же документы на принимающем устройстве.

В приведенном выше примере мы создали `NSMutableDictionary` объект и заполнили его одним ключом, который предоставляет URL-адрес, который пользователь просматривал в данный момент на данной вкладке. `AddUserInfoEntries` Метод действия пользователя использовался для обновления действия данными, которые будут использоваться для восстановления действия на принимающем устройстве:

```csharp
// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);
```

Apple рекомендует хранить данные на самом простом, чтобы обеспечить своевременную отправку данных на принимающее устройство. Если требуется отправить более подробную информацию, например изображение, присоединенное к документу, необходимо отправлять, следует использовать потоки продолжения. Дополнительные сведения см. в разделе [Поддержка потоков продолжения](#supporting-continuation-streams) ниже.

### <a name="continuing-an-activity"></a>Продолжение действия

Переданное устройство автоматически уведомляет локальные устройства iOS и OS X, находящиеся в физической близости к исходному устройству, и подписалась на одну и ту же учетную запись iCloud, доступную для непрерывных действий пользователя. Если пользователь решит продолжить действие на новом устройстве, система запустит соответствующее приложение (на основе идентификатора команды и типа действия) и будет иметь информацию о `AppDelegate` том, что должно произойти это продолжение.

Во-первых `WillContinueUserActivityWithType` , вызывается метод, поэтому приложение может информировать пользователя о том, что продолжение должно начаться. Мы используем следующий код в файле **AppDelegate.CS** нашего примера приложения, чтобы начать обработку продолжения:

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

В приведенном выше примере каждый контроллер представления регистрируется `AppDelegate` в и имеет открытый `PreparingToHandoff` метод, который отображает индикатор действия и сообщение, информирующее пользователя о том, что действие будет передаваться на текущее устройство. Пример

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

Дляфактическогопродолженияданногодействиябудетвызванметод.`ContinueUserActivity` `AppDelegate` Опять же, из нашего примера приложения:

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

Открытый `PerformHandoff` метод каждого контроллера представления на самом деле предварительно формирует передачу и восстанавливает действие на текущем устройстве. В этом примере он отображает тот же URL-адрес на данной вкладке, которую пользователь просматривает на другом устройстве. Пример

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

Метод включает в себя `UIApplicationRestorationHandler` объект, который можно вызывать для возобновления действия на основе документа или респондента. `ContinueUserActivity` При вызове в обработчик восстановления `NSArray` необходимо передать объекты или restorable. Например:

```csharp
completionHandler (new NSObject[]{Tab4});
```

Для каждого переданного объекта будет `RestoreUserActivityState` вызван его метод. Затем каждый объект может использовать данные в `UserInfo` словаре для восстановления своего собственного состояния. Например:

```csharp
public override void RestoreUserActivityState (NSUserActivity activity)
{
    base.RestoreUserActivityState (activity);

    // Log activity
    Console.WriteLine ("Restoring Activity {0}", activity.Title);
}
```

Для приложений на основе документов, если не `ContinueUserActivity` реализуется метод или он `UIKit` возвращает `false`значение или `AppKit` может автоматически возобновить действие. Дополнительные сведения см. в разделе [Поддержка передачи в приложениях на основе документов](#supporting-handoff-in-document-based-apps) .

### <a name="failing-handoff-gracefully"></a>Неправильное переработка

Поскольку передаваемые данные полагаются на передачу данных между коллекцией неслабо подключенных устройств iOS и OS X, процесс передачи иногда может завершиться сбоем. Вы должны разрабатывать приложение для корректной обработке этих сбоев и информировать пользователей о любых возникающих ситуациях.

В случае сбоя `DidFailToContinueUserActivitiy` `AppDelegate` будет вызван метод из. Например:

```csharp
public override void DidFailToContinueUserActivitiy (UIApplication application, string userActivityType, NSError error)
{
    // Log information about the failure
    Console.WriteLine ("User Activity {0} failed to continue. Error: {1}", userActivityType, error.LocalizedDescription);
}
```

Для предоставления пользователю сведений о `NSError` сбое следует использовать предоставленный параметр.

## <a name="native-app-to-web-browser-handoff"></a>Перепередачи собственного приложения в веб-браузер

Пользователю может потребоваться продолжить действие, не устанавливая на нужное устройство соответствующее собственное приложение. В некоторых ситуациях интерфейс на основе веб-интерфейса может предоставлять необходимые функции, и действие можно продолжать. Например, учетная запись электронной почты пользователя может предоставлять пользовательский интерфейс веб-интерфейса для создания и чтения сообщений.

Если исходное, собственное приложение знает URL-адрес Web interface (и необходимый синтаксис для определения возобновляемого элемента), он может закодировать эту информацию в `WebpageURL` свойстве `NSUserActivity` экземпляра. Если на принимающем устройстве не установлено соответствующее собственное приложение для выполнения продолжения, можно вызвать предоставленный веб-интерфейс.

## <a name="web-browser-to-native-app-handoff"></a>Веб-браузер для передачи собственного приложения

Если пользователь использовал веб-интерфейс на исходном устройстве, а собственное приложение на принимающем устройстве заявляет доменную часть `WebpageURL` свойства, система будет использовать это приложение, чтобы выполнить обработку продолжения. Новое устройство получит `NSUserActivity` экземпляр, который помечает тип действия как `BrowsingWeb` , и `WebpageURL` будет содержать URL-адрес, который пользователь посетил, `UserInfo` словарь будет пустым.

Чтобы приложение принимало участие в таком типе передачи, оно должно запрашивать домен в определенном `com.apple.developer.associated-domains` формате `<service>:<fully qualified domain name>` ( `activity continuation:company.com`например,).

Если указанный домен соответствует `WebpageURL` значению свойства, то при передаче загружается список утвержденных идентификаторов приложений с веб-сайта в этом домене. Веб-сайт должен предоставить список утвержденных идентификаторов в подписанном файле JSON с именем **Apple-App-site-Association** (например `https://company.com/apple-app-site-association`,).

Этот JSON-файл содержит словарь, указывающий список идентификаторов приложений в форме `<team identifier>.<bundle identifier>`. Например:

```csharp
{
    "activitycontinuation": {
        "apps": [    "YWBN8XTPBJ.com.company.FirstApp",
            "YWBN8XTPBJ.com.company.SecondApp" ]
    }
}
```

Чтобы подписать JSON-файл (чтобы он был правильным `Content-Type` `application/pkcs7-mime`), используйте приложение **терминала** и `openssl` команду с сертификатом и ключом, выданным центром сертификации, которому доверяет iOS (см [https://support.apple.com/kb/ht5012](https://support.apple.com/kb/ht5012) . список). Например:

```csharp
echo '{"activitycontinuation":{"apps":["YWBN8XTPBJ.com.company.FirstApp",
"YWBN8XTPBJ.com.company.SecondApp"]}}' > json.txt

cat json.txt | openssl smime -sign -inkey company.com.key
-signer company.com.pem
-certfile intermediate.pem
-noattr -nodetach
-outform DER > apple-app-site-association
```

Команда выводит подписанный файл JSON, который вы поместите на веб-сайт, по URL-адресу **Apple-App-site-Association.** `openssl` Например:

```csharp
https://example.com/apple-app-site-association.
```

Приложение получит все действия, домен `WebpageURL` которых находится в его `com.apple.developer.associated-domains` правах. Только протоколы `https`иподдерживаются , любой другой протокол вызовет исключение. `http`

## <a name="supporting-handoff-in-document-based-apps"></a>Поддержка передачи в приложениях на основе документов

Как упоминалось выше, в iOS и OS X приложения на основе документов автоматически поддерживают прием документов на основе iCloud, если файл **info. plist** приложения содержит `CFBundleDocumentTypes` ключ `NSUbiquitousDocumentUserActivityType`. Например:

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

В этом примере строка представляет собой обозначение приложения в обратную DNS с именем добавленного действия. Если этот способ введен, записи типа действия не нужно повторять в `NSUserActivityTypes` массиве файла **info. plist** .

Автоматически созданный объект действия пользователя (доступный через `UserActivity` свойство документа) может ссылаться на другие объекты в приложении и использоваться для восстановления состояния при продолжении. Например, для трассировки выбора элементов и позиции документа. Необходимо задать для `true` этого свойства действия `NeedsSave` значение при изменении `UserInfo` состояния и `UpdateUserActivityState` обновлении словаря в методе.

`UserActivity` Свойство может использоваться из любого потока и соответствует протоколу "ключ-значение" (кво), поэтому его можно использовать для синхронизации документа по мере его перемещения в iCloud и из него. `UserActivity` Свойство станет недействительным при закрытии документа.

Дополнительные сведения см. в документации Apple о [поддержке действий пользователей в приложениях на основе документов](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/HandoffFundamentals/HandoffFundamentals.html#//apple_ref/doc/uid/TP40014338-CH3-SW5) .

## <a name="supporting-handoff-in-responders"></a>Поддержка передачи в Ответчиках

Вы можете связать с действиями ответчики ( `UIResponder` наследуемые `NSResponder` от iOS или в OS X), задав `UserActivity` их свойства. Система автоматически сохраняет `UserActivity` свойство в нужное время, вызывая `UpdateUserActivityState` метод респондента для добавления текущих данных в объект действия `AddUserInfoEntriesFromDictionary` пользователя с помощью метода.

## <a name="supporting-continuation-streams"></a>Поддержка потоков продолжения

Это может быть ситуация, когда объем сведений, необходимых для продолжения действия, не может быть эффективно передан исходными полезными данными. В таких ситуациях принимающее приложение может установить один или несколько потоков между самим собой и исходным приложением для передачи данных.

Исходное приложение задаст `SupportsContinuationStreams` свойству `NSUserActivity` экземпляра `true`значение. Например:

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

Принимающее приложение может затем вызвать `GetContinuationStreams` метод `NSUserActivity` класса в `AppDelegate` , чтобы установить поток. Например:

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

На исходном устройстве делегат действия пользователя получает потоки, вызывая его `DidReceiveInputStream` метод для предоставления запрошенных данных, чтобы продолжить действия пользователя на возобновляемом устройстве.

Вы будете использовать `NSInputStream` для предоставления доступа только для чтения к потоковой передаче данных `NSOutputStream` и предоставления доступа только для записи. Потоки должны использоваться в запросах и ответах, когда принимающее приложение запрашивает больше данных, а исходное приложение предоставляет его. Таким образом, данные, записанные в поток вывода на исходном устройстве, считываются из входного потока на продолжающемся устройстве и наоборот.

Даже в ситуациях, когда требуется поток продолжения, между двумя приложениями должно быть не меньше обратной и не более.

Дополнительные сведения см. в документации Apple об [использовании потоков продолжения](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/AdoptingHandoff/AdoptingHandoff.html#//apple_ref/doc/uid/TP40014338-CH2-SW13) .

## <a name="handoff-best-practices"></a>Рекомендации по передаче

Успешная реализация простого продолжения действия пользователя с помощью передачи требует тщательного проектирования из-за всех вовлеченных в него компонентов. Компания Apple предлагает принять следующие рекомендации для приложений с поддержкой передачи:

- Разработайте действия пользователя, чтобы они требовали наименьшей полезной нагрузки, чтобы связать состояние действия, которое должно быть продолжено. Чем больше полезная нагрузка, тем больше времени занимает продолжение.
- Если необходимо переносить большие объемы данных для успешного продолжения, примите во внимание затраты, связанные с настройкой и нагрузкой сети.
- Обычно для большого приложения Mac создаются действия пользователя, которые обрабатываются несколькими, более мелкими приложениями на устройствах iOS. Различные версии приложений и ОС должны быть спроектированы для совместной работы или неправильного сбоя.
- При указании типов действий используйте нотацию обратных DNS, чтобы избежать конфликтов. Если действие относится к конкретному приложению, его имя должно быть включено в определение типа (например `com.myCompany.myEditor.editing`,). Если действие может работать в нескольких приложениях, удалите имя приложения из определения (например `com.myCompany.editing`,).
- Если приложению необходимо обновить состояние действий пользователя (`NSUserActivity`), `NeedsSave` задайте для `true`свойства значение. В нужное время передача будет вызывать `UserActivityWillSave` метод делегата, чтобы при необходимости можно было `UserInfo` обновить словарь.
- Так как процесс передачи не может быть немедленно инициализирован на принимающем устройстве, следует реализовать объект `AppDelegate` `WillContinueUserActivity` и сообщить пользователю о том, что продолжение должно начаться.

## <a name="example-handoff-app"></a>Пример переданного приложения

В качестве примера использования передачи в приложение Xamarin. iOS мы включили пример приложения [**монкэйбровсер**](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-monkeybrowser) с этим руководством. Приложение имеет четыре вкладки, которые пользователь может использовать для просмотра веб-страниц, каждый из которых имеет определенный тип действия: Погода, избранное, чашка кофе и работа.

На любой вкладке, когда пользователь вводит новый URL-адрес и нажмет кнопку **Go** , для этой `NSUserActivity` вкладки создается новый, содержащий URL-адрес, просматриваемый пользователем в данный момент:

[![](handoff-images/handoff01.png "Пример переданного приложения")](handoff-images/handoff01.png#lightbox)

Если на другом устройстве пользователя установлено приложение **монкэйбровсер** , оно входит в iCloud с одной и той же учетной записью пользователя, находится в той же сети и в близком расположении к указанному выше устройству, действие передачи будет отображаться на начальном экране (в нижнем левый угол):

[![](handoff-images/handoff02.png "Действие, отображаемое на начальном экране в левом нижнем углу экрана")](handoff-images/handoff02.png#lightbox)

Если пользователь перетаскивает вверх по значку передачи, приложение будет запущено и действие пользователя, указанное в, `NSUserActivity` будет продолжено на новом устройстве:

[![](handoff-images/handoff03.png "Действие пользователя продолжается на новом устройстве")](handoff-images/handoff03.png#lightbox)

Когда активность пользователя успешно отправлена на другое устройство Apple, устройство `NSUserActivity` отправки получит вызов `UserActivityWasContinued` метода `NSUserActivityDelegate` , чтобы сообщить ему о том, что действия пользователя были успешно переданы в другое. модем.

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
