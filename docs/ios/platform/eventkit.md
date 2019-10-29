---
title: EventKit в Xamarin. iOS
description: В этом документе описывается EventKit и его использование в Xamarin. iOS. В нем обсуждаются календари, события календаря и напоминания, рассматриваются классы, часто используемые при программировании с помощью EventKit, и многое другое.
ms.prod: xamarin
ms.assetid: 00E88629-357D-1FCD-4FCE-1330D5D9D32C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 1be6da2bbaf4aeffe00d90945bd06867f929c334
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032552"
---
# <a name="eventkit-in-xamarinios"></a>EventKit в Xamarin. iOS

в iOS есть два встроенных в календарь приложения: приложение календаря и приложение напоминаний. Достаточно просто понять, как приложение календаря управляет данными календаря, но приложение напоминаний менее очевидно. Даты, связанные с напоминаниями, могут быть связаны с ними в тех случаях, когда они заполняются, по завершении и т. д. Таким образом, iOS хранит все данные календаря, будь то события календаря или напоминания в одном месте, которое называется *базой данных календаря*.

Платформа EventKit предоставляет способ доступа к *календарям*, *событиям календаря*и данным *напоминаний* , которые хранятся в базе данных календаря. Доступ к календарям и событиям календаря был доступен с iOS 4, но доступ к памяткам — это новая возможность в iOS 6.

В этом руководство мы будем охватывать следующие задачи:

- **Основы EventKit** . Это приводит к образованию фундаментальных частей EventKit через основные классы и обеспечивает понимание их использования. Этот раздел является обязательным для чтения перед началом следующей части документа. 
- **Общие задачи** . раздел Common Tasks (общие задачи) предназначен для краткого справочника о том, как выполнять такие общие операции, как; Перечисление календарей, создание, сохранение и получение событий календаря и напоминаний, а также использование встроенных контроллеров для создания и изменения событий календаря. Этот раздел не должен быть прочитан спереди на задний план, так как он предназначен для определенных задач. 

Все задачи в этом руководство доступны в прилагаемом образце приложения:

 [![](eventkit-images/01.png "The companion sample application screens")](eventkit-images/01.png#lightbox)

## <a name="requirements"></a>Требования

EventKit был введен в iOS 4,0, но доступ к данным напоминаний был представлен в iOS 6,0. Таким образом, чтобы выполнить общую EventKit разработку, необходимо выбрать по меньшей мере версии 4,0 и 6,0 для напоминаний.

Кроме того, приложение напоминаний недоступно в симуляторе, то есть данные напоминаний также будут недоступны, если не добавить их первыми. Кроме того, запросы на доступ отображаются только для пользователя на фактическом устройстве. Таким образом, EventKit разработку лучше тестировать на устройстве.

## <a name="event-kit-basics"></a>Основные сведения о пакете событий

При работе с EventKit важно понять общие классы и их использование. Все эти классы можно найти в `EventKit` и `EventKitUI` (для `EKEventEditController`).

### <a name="eventstore"></a>евентсторе

Класс *евентсторе* является наиболее важным классом в EventKit, так как он необходим для выполнения любых операций в EventKit. Его можно рассматривать как постоянное хранилище или ядро СУБД для всех данных EventKit. С `EventStore` у вас есть доступ к календарям и событиям календаря в приложении календаря, а также к напоминаниям в приложении "напоминания".

Поскольку `EventStore` похоже на ядро СУБД, он должен быть длительным, что означает, что его следует создавать и удалять как можно меньше в течение времени существования экземпляра приложения. На самом деле, если вы создадите один экземпляр `EventStore` в приложении, вы всегда можете обеспечить эту ссылку для всего жизненного цикла приложения, если вы не уверены, что это не потребуется. Кроме того, все вызовы должны поступать в один экземпляр `EventStore`. По этой причине для поддержания единственного экземпляра рекомендуется использовать одноэлементный шаблон.

#### <a name="creating-an-event-store"></a>Создание хранилища событий

В следующем коде показан эффективный способ создания отдельного экземпляра класса `EventStore` и его статического доступа из приложения:

```csharp
public class App
{
    public static App Current {
            get { return current; }
    }
    private static App current;

    public EKEventStore EventStore {
            get { return eventStore; }
    }
    protected EKEventStore eventStore;

    static App ()
    {
            current = new App();
    }
    protected App () 
    {
            eventStore = new EKEventStore ( );
    }
}
```

В приведенном выше коде используется одноэлементный шаблон для создания экземпляра `EventStore` при загрузке приложения. Доступ к `EventStore` можно получить глобально из приложения следующим образом:

```csharp
App.Current.EventStore;
```

Обратите внимание, что все примеры в этой статье используют этот шаблон, поэтому они ссылаются на `EventStore` с помощью `App.Current.EventStore`.

#### <a name="requesting-access-to-calendar-and-reminder-data"></a>Запрос доступа к данным календаря и напоминаний

Прежде чем получать доступ к любым данным через Евентсторе, приложение должно сначала запросить доступ к данным или напоминаниям о событиях календаря в зависимости от того, какая из них вам нужна. Для этого `EventStore` предоставляет метод с именем `RequestAccess`, который (при вызове) показывает пользователю представление предупреждения о том, что приложение запрашивает доступ к данным календаря или к данным напоминаний в зависимости от того, какой `EKEntityType` передается им. Поскольку он создает представление предупреждений, вызов является асинхронным и вызывает обработчик завершения, переданный в качестве `NSAction` (или лямбда-выражения), который будет получать два параметра. Логическое значение, указывающее, был ли предоставлен доступ, и `NSError`, который, если значение не равно null, будет содержать какие-либо сведения об ошибках в запросе. Например, приведенная ниже кодировка запрашивает доступ к данным события календаря и отображает представление предупреждения, если запрос не был предоставлен.

```csharp
App.Current.EventStore.RequestAccess (EKEntityType.Event, 
    (bool granted, NSError e) => {
            if (granted)
                    //do something here
            else
                    new UIAlertView ( "Access Denied", 
"User Denied Access to Calendar Data", null,
"ok", null).Show ();
            } );
```

После предоставления запроса он будет сохранен при условии, что приложение установлено на устройстве и не будет выводить оповещение пользователю.
Однако доступ предоставляется только к типу ресурса, предоставленным события календаря или напоминаний. Если приложению необходим доступ к обоим приложениям, оно должно запрашивать оба.

Так как разрешение запоминается, довольно недорого делать запрос каждый раз, поэтому рекомендуется всегда запрашивать доступ перед выполнением операции.

Кроме того, поскольку обработчик завершения вызывается в отдельном потоке (без пользовательского интерфейса), любые обновления пользовательского интерфейса в обработчике завершения должны вызываться через `InvokeOnMainThread`, в противном случае будет вызвано исключение, и если оно не будет перехвачено, произойдет сбой приложения.

### <a name="ekentitytype"></a>екентититипе

`EKEntityType` — это перечисление, описывающее тип элемента `EventKit` или данных. Он имеет два значения: `Event` и напоминания. Он используется в ряде методов, в том числе `EventStore.RequestAccess`, чтобы указать `EventKit`, какие данные получить доступ или получить.

### <a name="ekcalendar"></a>еккалендар

 *Еккалендар* представляет календарь, который содержит группу событий календаря. Календари могут храниться в разных местах, например локально, в *iCloud*, в расположении сторонних поставщиков, например в *Exchange Server* или *Google*, и т. д. Во многих случаях `EKCalendar` используется для указания `EventKit` где искать события или где их можно сохранить.

### <a name="ekeventeditcontroller"></a>екевентедитконтроллер

 *Екевентедитконтроллер* можно найти в пространстве имен `EventKitUI` и является встроенным контроллером, который можно использовать для изменения или создания событий календаря. Подобно встроенным контроллерам камеры, `EKEventEditController` выполняет тяжелую работу при отображении пользовательского интерфейса и обработке сохранения.

### <a name="ekevent"></a>екевент

 *Екевент* представляет событие календаря. И `EKEvent`, и `EKReminder` наследуются от `EKCalendarItem` и имеют такие поля, как `Title`, `Notes`и т. д.

### <a name="ekreminder"></a>екреминдер

 *Екреминдер* представляет элемент напоминания.

### <a name="ekspan"></a>експан

*Експан* — это перечисление, которое описывает диапазон событий при изменении событий, которые могут повторяться, и имеет два значения: *данный* и *футуривентс*. `ThisEvent` означает, что любые изменения будут происходить только с определенным событием в ряде, на который указывает ссылка, тогда как `FutureEvents` повлияет на это событие и все будущие повторения.

## <a name="tasks"></a>Задачи

Для простоты использования EventKit было разбито на распространенные задачи, описанные в следующих разделах.

### <a name="enumerate-calendars"></a>Перечисление календарей

Чтобы перечислить календари, настроенные пользователем на устройстве, вызовите `GetCalendars` на `EventStore` и передайте тип календарей (напоминаний или событий), которые вы хотите получить:

```csharp
EKCalendar[] calendars = 
App.Current.EventStore.GetCalendars ( EKEntityType.Event );
```

### <a name="add-or-modify-an-event-using-the-built-in-controller"></a>Добавление или изменение события с помощью встроенного контроллера

*Екевентедитвиевконтроллер* выполняет большую часть тяжелой работы, если вы хотите создать или изменить событие с тем же интерфейсом, который представлен пользователю при использовании приложения календаря:

 [![](eventkit-images/02.png "The UI that is presented to the user when using the Calendar Application")](eventkit-images/02.png#lightbox)

Чтобы использовать его, необходимо объявить его как переменную уровня класса, чтобы она не собралась сборщиком мусора, если она объявлена в методе:

```csharp
public class HomeController : DialogViewController
{
        protected CreateEventEditViewDelegate eventControllerDelegate;
        ...
}
```

Затем, чтобы запустить его, создайте его экземпляр, присвойте ему ссылку на `EventStore`, установите для него делегат *екевентедитвиевделегате* , а затем отобразите его с помощью `PresentViewController`:

```csharp
EventKitUI.EKEventEditViewController eventController = 
        new EventKitUI.EKEventEditViewController ();

// set the controller's event store - it needs to know where/how to save the event
eventController.EventStore = App.Current.EventStore;

// wire up a delegate to handle events from the controller
eventControllerDelegate = new CreateEventEditViewDelegate ( eventController );
eventController.EditViewDelegate = eventControllerDelegate;

// show the event controller
PresentViewController ( eventController, true, null );
```

Если вы хотите предварительно заполнить событие, можно либо создать новое событие (как показано ниже), либо получить сохраненное событие:

```csharp
EKEvent newEvent = EKEvent.FromStore ( App.Current.EventStore );
// set the alarm for 10 minutes from now
newEvent.AddAlarm ( EKAlarm.FromDate ( DateTime.Now.AddMinutes ( 10 ) ) );
// make the event start 20 minutes from now and last 30 minutes
newEvent.StartDate = DateTime.Now.AddMinutes ( 20 );
newEvent.EndDate = DateTime.Now.AddMinutes ( 50 );
newEvent.Title = "Get outside and exercise!";
newEvent.Notes = "This is your reminder to go and exercise for 30 minutes.”;
```

Если вы хотите предварительно заполнить пользовательский интерфейс, обязательно установите свойство события на контроллере:

```csharp
eventController.Event = newEvent;
```

Чтобы использовать существующее событие, см. раздел *Получение события по идентификатору* далее в разделе.

Делегат должен переопределять метод `Completed`, который вызывается контроллером, когда пользователь завершает работу с диалоговым окном.

```csharp
protected class CreateEventEditViewDelegate : EventKitUI.EKEventEditViewDelegate
{
        // we need to keep a reference to the controller so we can dismiss it
        protected EventKitUI.EKEventEditViewController eventController;

        public CreateEventEditViewDelegate (EventKitUI.EKEventEditViewController eventController)
        {
                // save our controller reference
                this.eventController = eventController;
        }

        // completed is called when a user eith
        public override void Completed (EventKitUI.EKEventEditViewController controller, EKEventEditViewAction action)
        {
                eventController.DismissViewController (true, null);
                }
        }
}
```

При необходимости в делегате можно проверить *действие* в методе `Completed`, чтобы изменить событие и повторно сохранить его, или выполнить другие действия, если оно будет отменено, д:

```csharp
public override void Completed (EventKitUI.EKEventEditViewController controller, EKEventEditViewAction action)
{
        eventController.DismissViewController (true, null);

        switch ( action ) {

        case EKEventEditViewAction.Canceled:
                break;
        case EKEventEditViewAction.Deleted:
                break;
        case EKEventEditViewAction.Saved:
                // if you wanted to modify the event you could do so here,
// and then save:
                //App.Current.EventStore.SaveEvent ( controller.Event, )
                break;
        }
}
```

### <a name="creating-an-event-programmatically"></a>Создание события программным способом

Чтобы создать событие в коде, используйте метод фабрики *фромсторе* для класса `EKEvent` и установите на нем любые данные:

```csharp
EKEvent newEvent = EKEvent.FromStore ( App.Current.EventStore );
// set the alarm for 10 minutes from now
newEvent.AddAlarm ( EKAlarm.FromDate ( DateTime.Now.AddMinutes ( 10 ) ) );
// make the event start 20 minutes from now and last 30 minutes
newEvent.StartDate = DateTime.Now.AddMinutes ( 20 );
newEvent.EndDate = DateTime.Now.AddMinutes ( 50 );
newEvent.Title = "Get outside and do some exercise!";
newEvent.Notes = "This is your motivational event to go and do 30 minutes of exercise. Super important. Do this.";
```

Необходимо задать календарь, в котором должно сохраняться событие, но если предпочтение отсутствует, можно использовать значение по умолчанию:

```csharp
newEvent.Calendar = App.Current.EventStore.DefaultCalendarForNewEvents;
```

Чтобы сохранить событие, вызовите метод *савивент* на `EventStore`:

```csharp
NSError e;
App.Current.EventStore.SaveEvent ( newEvent, EKSpan.ThisEvent, out e );
```

После сохранения свойство *EventIdentifier* будет обновлено уникальным идентификатором, который можно будет использовать позже для получения события:

```csharp
Console.WriteLine ("Event Saved, ID: " + newEvent.CalendarItemIdentifier);
```

 `EventIdentifier` — это идентификатор GUID в формате строки.

### <a name="create-a-reminder-programmatically"></a>Создание напоминания программными средствами

Создание напоминания в коде во многом аналогично созданию события календаря:

```csharp
EKReminder reminder = EKReminder.Create ( App.Current.EventStore );
reminder.Title = "Do something awesome!";
reminder.Calendar = App.Current.EventStore.DefaultCalendarForNewReminders;
```

Чтобы сохранить его, вызовите метод *савереминдер* на `EventStore`:

```csharp
NSError e;
App.Current.EventStore.SaveReminder ( reminder, true, out e );
```

### <a name="retrieving-an-event-by-id"></a>Получение события по ИДЕНТИФИКАТОРу

Чтобы получить событие по ИДЕНТИФИКАТОРу, используйте метод *евентфромидентифиер* на `EventStore` и передайте ему `EventIdentifier`, извлеченный из события:

```csharp
EKEvent mySavedEvent = App.Current.EventStore.EventFromIdentifier ( newEvent.EventIdentifier );
```

Для событий существуют два других свойства идентификатора, но для этого используется только `EventIdentifier`.

### <a name="retrieving-a-reminder-by-id"></a>Получение напоминания по ИДЕНТИФИКАТОРу

Чтобы получить напоминание, используйте метод *жеткалендаритем* на `EventStore` и передайте ему *календаритемидентифиер*:

```csharp
EKCalendarItem myReminder = App.Current.EventStore.GetCalendarItem ( reminder.CalendarItemIdentifier );
```

Поскольку `GetCalendarItem` возвращает `EKCalendarItem`, его необходимо привести к `EKReminder`, если требуется получить доступ к данным напоминаний или использовать экземпляр как `EKReminder` позже.

Не используйте `GetCalendarItem` для событий календаря, как на момент написания статьи, это не работает.

### <a name="deleting-an-event"></a>Удаление события

Чтобы удалить событие календаря, вызовите *ремовивент* на `EventStore` и передайте ссылку на событие и соответствующую `EKSpan`:

```csharp
NSError e;
App.Current.EventStore.RemoveEvent ( mySavedEvent, EKSpan.ThisEvent, true, out e);
```

Обратите внимание, что после удаления события ссылка на событие будет `null`.

### <a name="deleting-a-reminder"></a>Удаление напоминания

Чтобы удалить напоминание, вызовите *ремовереминдер* на `EventStore` и передайте ссылку на напоминание:

```csharp
NSError e;
App.Current.EventStore.RemoveReminder ( myReminder as EKReminder, true, out e);
```

Обратите внимание, что в приведенном выше коде имеется приведение к `EKReminder`, поскольку `GetCalendarItem` использовалось для получения

### <a name="searching-for-events"></a>Поиск событий

Для поиска событий календаря необходимо создать объект *нспредикате* с помощью метода *предикатефоревентс* на `EventStore`. `NSPredicate` — это объект данных запроса, который iOS использует для поиска совпадений:

```csharp
DateTime startDate = DateTime.Now.AddDays ( -7 );
DateTime endDate = DateTime.Now;
// the third parameter is calendars we want to look in, to use all calendars, we pass null
NSPredicate query = App.Current.EventStore.PredicateForEvents ( startDate, endDate, null );
```

После создания `NSPredicate`используйте метод *евентсматчинг* на `EventStore`:

```csharp
// execute the query
EKCalendarItem[] events = App.Current.EventStore.EventsMatching ( query );
```

Обратите внимание, что запросы являются синхронными (блокируются) и могут занимать много времени в зависимости от запроса, поэтому для этого может потребоваться запустить новый поток или задачу.

### <a name="searching-for-reminders"></a>Поиск напоминаний

Поиск напоминаний аналогичен событиям. Он требует наличия предиката, но вызов уже является асинхронным, поэтому вы не беспокоиться о блокировке потока:

```csharp
// create our NSPredicate which we'll use for the query
NSPredicate query = App.Current.EventStore.PredicateForReminders ( null );

// execute the query
App.Current.EventStore.FetchReminders (
        query, ( EKReminder[] items ) => {
                // do someting with the items
        } );
```

## <a name="summary"></a>Сводка

В этом документе представлен обзор как важных частей EventKit Framework, так и ряда наиболее распространенных задач. Однако платформа EventKit является очень большой и мощной и включает в себя функции, которые не появились здесь, такие как пакетные обновления, Настройка оповещений, настройка периодичности событий, регистрация и прослушивание изменений в базе данных календаря. Настройка геозаборов и др.  Дополнительные сведения см. в статье о [программировании календаря и напоминаний](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html)Apple.

## <a name="related-links"></a>Связанные ссылки

- [Календари (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/calendars)
- [Введение в iOS 6](~/ios/platform/introduction-to-ios6/index.md)
- [Общие сведения о календарях и напоминаниях](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html)
