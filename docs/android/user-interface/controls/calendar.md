---
title: Календарь Xamarin. Android
ms.prod: xamarin
ms.assetid: 78666541-CA14-4CD8-A94A-A9621C57813E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 0172a602f3e85f0de66b39613b4a28e49344ba08
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510341"
---
# <a name="xamarinandroid-calendar"></a>Календарь Xamarin. Android


## <a name="calendar-api"></a>API календаря

Новый набор API-интерфейсов календаря, появившихся в Android 4, поддерживает приложения, предназначенные для чтения и записи данных в поставщик календаря. Эти API-интерфейсы поддерживают множество вариантов взаимодействия с данными календаря, включая возможность чтения и записи событий, участников и напоминаний. Используя поставщик календаря в приложении, данные, добавляемые через API, будут отображаться в встроенном приложении календаря, поставляемом с Android 4.


## <a name="adding-permissions"></a>Добавление разрешений

При работе с новыми API-интерфейсами календаря в приложении сначала нужно добавить соответствующие разрешения в манифест Android. Разрешения, которые необходимо добавить, — `android.permisson.READ_CALENDAR` и `android.permission.WRITE_CALENDAR`, в зависимости от того, выполняется ли чтение или запись данных календаря.


## <a name="using-the-calendar-contract"></a>Использование контракта календаря

После настройки разрешений можно взаимодействовать с данными календаря с помощью `CalendarContract` класса. Этот класс предоставляет модель данных, которую могут использовать приложения при взаимодействии с поставщиком календаря. `CalendarContract` Позволяет приложениям разрешать универсальные коды ресурса (URI) в календарные сущности, такие как календари и события. Он также предоставляет способ взаимодействия с различными полями в каждой сущности, например имя и идентификатор календаря, а также дату начала и окончания события.

Рассмотрим пример, в котором используется API календаря. В этом примере мы рассмотрим, как перечислять календари и их события, а также как добавить новое событие в календарь.


## <a name="listing-calendars"></a>Создание списка календарей

Сначала давайте рассмотрим, как перечислить календари, зарегистрированные в приложении календаря. Для этого можно создать экземпляр `CursorLoader`. В Android 3,0 (API 11) `CursorLoader` предпочтительным способом `ContentProvider`использования. Как минимум необходимо указать универсальный код ресурса (URI) содержимого для календарей и столбцов, которые нужно вернуть. Эта спецификация столбца называется проекцией.

`CursorLoader.LoadInBackground` Вызов метода позволяет запросить поставщик содержимого для данных, например поставщика календаря.
`LoadInBackground`выполняет фактическую операцию загрузки и возвращает объект `Cursor` с результатами запроса.

Помогает нам указать как содержимое `Uri` , так и проекцию. `CalendarContract` Чтобы получить содержимое `Uri` для запросов к календарям, можно просто `CalendarContract.Calendars.ContentUri` использовать свойство следующим образом:

```csharp
var calendarsUri = CalendarContract.Calendars.ContentUri;
```

`CalendarContract` Использование для указания столбцов календаря, которые нам нужны, равно простому. Мы просто добавляем поля в `CalendarContract.Calendars.InterfaceConsts` класс в массив. Например, следующий код включает идентификатор календаря, отображаемое имя и имя учетной записи:

```csharp
string[] calendarsProjection = {
    CalendarContract.Calendars.InterfaceConsts.Id,
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName
};
```

Важно включить, если вы `SimpleCursorAdapter` используете для привязки данных к пользовательскому интерфейсу, как будет показано ниже. `Id` Используя URI содержимого и проекцию, мы создадим экземпляр `CursorLoader` и `CursorLoader.LoadInBackground` вызываю метод, чтобы вернуть курсор с данными календаря, как показано ниже:

```csharp
var loader = new CursorLoader(this, calendarsUri, calendarsProjection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();

```

Пользовательский интерфейс для этого примера содержит, `ListView`а каждый элемент в списке представляет один календарь. В следующем коде XML показана разметка, `ListView`включающая:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="fill_parent">
  <ListView
    android:id="@android:id/android:list"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content" />
</LinearLayout>
```

Кроме того, необходимо указать пользовательский интерфейс для каждого элемента списка, который мы помещаем в отдельный XML-файл следующим образом:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="wrap_content">
  <TextView android:id="@+id/calDisplayName"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="16dip" />
  <TextView android:id="@+id/calAccountName"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="12dip" />
</LinearLayout>
```

С этого момента он просто является обычным кодом Android для привязки данных из курсора к пользовательскому интерфейсу. Мы будем использовать, `SimpleCursorAdapter` как показано ниже.

```csharp
string[] sourceColumns = {
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName };

int[] targetResources = {
    Resource.Id.calDisplayName, Resource.Id.calAccountName };      

SimpleCursorAdapter adapter = new SimpleCursorAdapter (this,
    Resource.Layout.CalListItem, cursor, sourceColumns, targetResources);

ListAdapter = adapter;
```

В приведенном выше коде адаптер принимает столбцы, указанные в `sourceColumns` массиве, и записывает их в элементы пользовательского интерфейса `targetResources` в массиве для каждой записи календаря в курсоре. Используемое здесь действие является подклассом `ListActivity`. оно `ListAdapter` включает свойство, для которого задается адаптер.

Ниже приведен снимок экрана с конечным результатом, в котором отображаются сведения о календаре `ListView`в:

[![Календардемо работает в эмуляторе, отображая две записи календаря](calendar-images/11-calendar.png)](calendar-images/11-calendar.png#lightbox)



## <a name="listing-calendar-events"></a>Составление списка событий календаря

Теперь рассмотрим, как перечислить события для данного календаря.
Основываясь на приведенном выше примере, мы представим список событий, когда пользователь выбирает один из календарей. Поэтому нам нужно будет выполнить обработку выбора элементов в предыдущем коде:

```csharp
ListView.ItemClick += (sender, e) => {
    int i = (e as ItemEventArgs).Position;

    cursor.MoveToPosition(i);
    int calId =
        cursor.GetInt (cursor.GetColumnIndex (calendarsProjection [0]));

    var showEvents = new Intent(this, typeof(EventListActivity));
    showEvents.PutExtra("calId", calId);
    StartActivity(showEvents);
};
```

В этом коде мы создаем намерение открыть действие типа `EventListActivity`, передав идентификатор календаря в цель. Нам потребуется идентификатор, чтобы знать, в каком календаре нужно запросить события. В методе можно получить идентификатор из, `Intent` как показано ниже: `OnCreate` `EventListActivity`

```csharp
_calId = Intent.GetIntExtra ("calId", -1);
```

Теперь давайте запрашивать события для этого идентификатора календаря. Процесс запроса событий аналогичен тому, как мы запрашивали список календарей ранее, только на `CalendarContract.Events` этот раз мы будем работать с классом. Следующий код создает запрос для получения событий:

```csharp
var eventsUri = CalendarContract.Events.ContentUri;

string[] eventsProjection = {
    CalendarContract.Events.InterfaceConsts.Id,
    CalendarContract.Events.InterfaceConsts.Title,
    CalendarContract.Events.InterfaceConsts.Dtstart
};

var loader = new CursorLoader(this, eventsUri, eventsProjection,
                   String.Format ("calendar_id={0}", _calId), null, "dtstart ASC");
var cursor = (ICursor)loader.LoadInBackground();
```

В этом коде мы сначала получаем содержимое `Uri` для событий `CalendarContract.Events.ContentUri` из свойства. Затем мы указываем столбцы событий, которые нужно получить в массиве Евентспрожектион.
Наконец, мы создаем экземпляр `CursorLoader` с этими сведениями и вызываем `LoadInBackground` метод загрузчика для возврата `Cursor` с данными события.

Чтобы отобразить данные события в пользовательском интерфейсе, мы можем использовать разметку и код так же, как и раньше, чтобы отобразить список календарей. Опять же, мы `SimpleCursorAdapter` используем для привязки данных к, `ListView` как показано в следующем коде:

```csharp
string[] sourceColumns = {
    CalendarContract.Events.InterfaceConsts.Title,
    CalendarContract.Events.InterfaceConsts.Dtstart };

int[] targetResources = {
    Resource.Id.eventTitle,
    Resource.Id.eventStartDate };

var adapter = new SimpleCursorAdapter (this, Resource.Layout.EventListItem,
    cursor, sourceColumns, targetResources);

adapter.ViewBinder = new ViewBinder ();       
ListAdapter = adapter;
```

Основное различие между этим кодом и кодом, который мы использовали ранее для отображения списка календаря `ViewBinder`, — это использование, которое задается в строке:

```csharp
adapter.ViewBinder = new ViewBinder ();
```

`ViewBinder` Класс позволяет нам контролировать способ привязки значений к представлениям. В этом случае мы используем его для преобразования времени начала события из миллисекунд в строку даты, как показано в следующей реализации:

```csharp
class ViewBinder : Java.Lang.Object, SimpleCursorAdapter.IViewBinder
{    
    public bool SetViewValue (View view, Android.Database.ICursor cursor,
        int columnIndex)
    {
        if (columnIndex == 2) {
            long ms = cursor.GetLong (columnIndex);

            DateTime date = new DateTime (1970, 1, 1, 0, 0, 0,
                DateTimeKind.Utc).AddMilliseconds (ms).ToLocalTime ();

            TextView textView = (TextView)view;
            textView.Text = date.ToLongDateString ();

            return true;
        }
        return false;
    }    
}
```

Отобразится список событий, как показано ниже.

[![Снимок экрана примера приложения, отображающего три события календаря](calendar-images/12-events.png)](calendar-images/12-events.png#lightbox)



## <a name="adding-a-calendar-event"></a>Добавление события календаря

Мы рассмотрели, как считывать данные календаря. Теперь давайте посмотрим, как добавить событие в календарь. Чтобы это работало, обязательно включите в него `android.permission.WRITE_CALENDAR` разрешение, упомянутое ранее. Чтобы добавить событие в календарь, мы будем делать следующее:

1.  `ContentValues` Создайте экземпляр.
1.  Используйте ключи из `CalendarContract.Events.InterfaceConsts` класса для заполнения `ContentValues` экземпляра.
1.  Задайте Часовые пояса для времени начала и окончания события.
1.  `ContentResolver` Используйте для вставки данных события в календарь.


Эти шаги показаны в приведенном ниже коде.

```csharp
ContentValues eventValues = new ContentValues ();

eventValues.Put (CalendarContract.Events.InterfaceConsts.CalendarId,
    _calId);
eventValues.Put (CalendarContract.Events.InterfaceConsts.Title,
    "Test Event from M4A");
eventValues.Put (CalendarContract.Events.InterfaceConsts.Description,
    "This is an event created from Xamarin.Android");
eventValues.Put (CalendarContract.Events.InterfaceConsts.Dtstart,
    GetDateTimeMS (2011, 12, 15, 10, 0));
eventValues.Put (CalendarContract.Events.InterfaceConsts.Dtend,
    GetDateTimeMS (2011, 12, 15, 11, 0));

eventValues.Put(CalendarContract.Events.InterfaceConsts.EventTimezone,
    "UTC");
eventValues.Put(CalendarContract.Events.InterfaceConsts.EventEndTimezone,
    "UTC");

var uri = ContentResolver.Insert (CalendarContract.Events.ContentUri,
    eventValues);
```

Обратите внимание, что если не задать часовой пояс, будет выдано исключение `Java.Lang.IllegalArgumentException` типа. Поскольку значения времени события должны быть выражены в миллисекундах с момента создания эпохи `GetDateTimeMS` , мы создаем метод (в `EventListActivity`) для преобразования спецификаций даты в формат миллисекунд:

```csharp
long GetDateTimeMS (int yr, int month, int day, int hr, int min)
{
    Calendar c = Calendar.GetInstance (Java.Util.TimeZone.Default);

    c.Set (Java.Util.CalendarField.DayOfMonth, 15);
    c.Set (Java.Util.CalendarField.HourOfDay, hr);
    c.Set (Java.Util.CalendarField.Minute, min);
    c.Set (Java.Util.CalendarField.Month, Calendar.December);
    c.Set (Java.Util.CalendarField.Year, 2011);

    return c.TimeInMillis;
}
```

Если добавить кнопку в пользовательский интерфейс списка событий и выполнить приведенный выше код в обработчике события нажатия кнопки, событие добавляется в календарь и обновляется в нашем списке, как показано ниже:

[![Снимок экрана примера приложения с событиями календаря с последующим нажатием кнопки "добавить пример события"](calendar-images/13.png)](calendar-images/13.png#lightbox)

Если открыть приложение календаря, мы увидим, что это событие также написано здесь:

[![Снимок экрана: приложение календаря, отображающее выбранное событие календаря](calendar-images/14.png)](calendar-images/14.png#lightbox)

Как видите, Android предоставляет мощный и простой доступ для получения и сохранения данных календаря, позволяя приложениям легко интегрировать возможности календаря.


## <a name="related-links"></a>Связанные ссылки

- [Демонстрация календаря (пример)](https://developer.xamarin.com/samples/monodroid/CalendarDemo/)
- [Знакомство со Южным Сандвичевым](http://www.android.com/about/ice-cream-sandwich/)
- [Платформа Android 4,0](https://developer.android.com/sdk/android-4.0.html)
