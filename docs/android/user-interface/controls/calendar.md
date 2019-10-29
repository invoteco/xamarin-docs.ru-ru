---
title: Календарь Xamarin. Android
ms.prod: xamarin
ms.assetid: 78666541-CA14-4CD8-A94A-A9621C57813E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 3d74e2db541e1f30c7626cd1b08228c1e8f57a42
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029320"
---
# <a name="xamarinandroid-calendar"></a>Календарь Xamarin. Android

## <a name="calendar-api"></a>API календаря

Новый набор API-интерфейсов календаря, появившихся в Android 4, поддерживает приложения, предназначенные для чтения и записи данных в поставщик календаря. Эти API-интерфейсы поддерживают множество вариантов взаимодействия с данными календаря, включая возможность чтения и записи событий, участников и напоминаний. Используя поставщик календаря в приложении, данные, добавляемые через API, будут отображаться в встроенном приложении календаря, поставляемом с Android 4.

## <a name="adding-permissions"></a>Добавление разрешений

При работе с новыми API-интерфейсами календаря в приложении сначала нужно добавить соответствующие разрешения в манифест Android. Разрешения, которые необходимо добавить, `android.permisson.READ_CALENDAR` и `android.permission.WRITE_CALENDAR`в зависимости от того, производится ли чтение и/или запись данных календаря.

## <a name="using-the-calendar-contract"></a>Использование контракта календаря

После настройки разрешений можно взаимодействовать с данными календаря с помощью класса `CalendarContract`. Этот класс предоставляет модель данных, которую могут использовать приложения при взаимодействии с поставщиком календаря. `CalendarContract` позволяет приложениям разрешать универсальные коды ресурса (URI) в календарные сущности, такие как календари и события. Он также предоставляет способ взаимодействия с различными полями в каждой сущности, например имя и идентификатор календаря, а также дату начала и окончания события.

Рассмотрим пример, в котором используется API календаря. В этом примере мы рассмотрим, как перечислять календари и их события, а также как добавить новое событие в календарь.

## <a name="listing-calendars"></a>Создание списка календарей

Сначала давайте рассмотрим, как перечислить календари, зарегистрированные в приложении календаря. Для этого можно создать экземпляр `CursorLoader`. В Android 3,0 (API 11) `CursorLoader` является предпочтительным способом использования `ContentProvider`. Как минимум необходимо указать универсальный код ресурса (URI) содержимого для календарей и столбцов, которые нужно вернуть. Эта спецификация столбца называется _проекцией_.

Вызов метода `CursorLoader.LoadInBackground` позволяет запросить поставщик содержимого для данных, например поставщика календаря.
`LoadInBackground` выполняет фактическую операцию загрузки и возвращает `Cursor` результатов запроса.

`CalendarContract` помогает нам указать как `Uri` содержимого, так и проекцию. Чтобы получить `Uri` содержимого для запросов календарей, можно просто использовать свойство `CalendarContract.Calendars.ContentUri` следующим образом:

```csharp
var calendarsUri = CalendarContract.Calendars.ContentUri;
```

Использование `CalendarContract` для указания столбцов календаря, которые нам нужны, так же просто. Мы просто добавляем поля в класс `CalendarContract.Calendars.InterfaceConsts` в массив. Например, следующий код включает идентификатор календаря, отображаемое имя и имя учетной записи:

```csharp
string[] calendarsProjection = {
    CalendarContract.Calendars.InterfaceConsts.Id,
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName
};
```

`Id` важно включить, если для привязки данных к пользовательскому интерфейсу используется `SimpleCursorAdapter`, как скоро будет видно. Используя URI содержимого и проекцию, мы создадим экземпляр `CursorLoader` и вызываю метод `CursorLoader.LoadInBackground`, чтобы вернуть курсор с данными календаря, как показано ниже:

```csharp
var loader = new CursorLoader(this, calendarsUri, calendarsProjection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();

```

Пользовательский интерфейс для этого примера содержит `ListView`с каждым элементом списка, представляющим один календарь. В следующем коде XML показана разметка, которая включает в себя `ListView`:

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

С этого момента он просто является обычным кодом Android для привязки данных из курсора к пользовательскому интерфейсу. Мы будем использовать `SimpleCursorAdapter` следующим образом:

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

В приведенном выше коде адаптер принимает столбцы, указанные в массиве `sourceColumns`, и записывает их в элементы пользовательского интерфейса массива `targetResources` для каждой записи календаря в курсоре. Используемое здесь действие является подклассом `ListActivity`; Он содержит свойство `ListAdapter`, для которого задается адаптер.

Ниже приведен снимок экрана с конечным результатом, в котором отображаются сведения о календаре в `ListView`:

[![Календардемо в эмуляторе с отображением двух записей календаря](calendar-images/11-calendar.png)](calendar-images/11-calendar.png#lightbox)

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

В этом коде мы создаем намерение открыть действие типа `EventListActivity`, передав идентификатор календаря в цель. Нам потребуется идентификатор, чтобы знать, в каком календаре нужно запросить события. В методе `OnCreate` `EventListActivity`можно получить идентификатор из `Intent`, как показано ниже:

```csharp
_calId = Intent.GetIntExtra ("calId", -1);
```

Теперь давайте запрашивать события для этого идентификатора календаря. Процесс запроса событий аналогичен тому, как мы запрашивали список календарей ранее, только на этот раз мы будем работать с классом `CalendarContract.Events`. Следующий код создает запрос для получения событий:

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

В этом коде сначала получается `Uri` содержимого для событий из свойства `CalendarContract.Events.ContentUri`. Затем мы указываем столбцы событий, которые нужно получить в массиве Евентспрожектион.
Наконец, мы создаем экземпляр `CursorLoader` с этими сведениями и вызываем метод `LoadInBackground` загрузчика для возврата `Cursor` с данными события.

Чтобы отобразить данные события в пользовательском интерфейсе, мы можем использовать разметку и код так же, как и раньше, чтобы отобразить список календарей. Опять же, мы используем `SimpleCursorAdapter` для привязки данных к `ListView`, как показано в следующем коде:

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

Основное различие между этим кодом и кодом, который мы использовали ранее для отображения списка календаря, заключается в использовании `ViewBinder`, которая задается в строке:

```csharp
adapter.ViewBinder = new ViewBinder ();
```

Класс `ViewBinder` позволяет дополнительно контролировать способ привязки значений к представлениям. В этом случае мы используем его для преобразования времени начала события из миллисекунд в строку даты, как показано в следующей реализации:

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

[Снимок экрана![пример приложения, отображающий три события календаря](calendar-images/12-events.png)](calendar-images/12-events.png#lightbox)

## <a name="adding-a-calendar-event"></a>Добавление события календаря

Мы рассмотрели, как считывать данные календаря. Теперь давайте посмотрим, как добавить событие в календарь. Чтобы это работало, обязательно включите `android.permission.WRITE_CALENDAR`ное разрешение, упомянутое ранее. Чтобы добавить событие в календарь, мы будем делать следующее:

1. Создайте экземпляр `ContentValues`.
1. Используйте ключи из класса `CalendarContract.Events.InterfaceConsts` для заполнения экземпляра `ContentValues`.
1. Задайте Часовые пояса для времени начала и окончания события.
1. Используйте `ContentResolver` для вставки данных события в календарь.

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

Обратите внимание, что если не задать часовой пояс, будет выдано исключение типа `Java.Lang.IllegalArgumentException`. Так как значения времени события должны быть выражены в миллисекундах с момента создания эпохи, мы создаем метод `GetDateTimeMS` (в `EventListActivity`) для преобразования спецификаций даты в формат миллисекунд:

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

[![снимок экрана примера приложения с событиями календаря, а затем нажмите кнопку Добавить пример события](calendar-images/13.png)](calendar-images/13.png#lightbox)

Если открыть приложение календаря, мы увидим, что это событие также написано здесь:

[![снимок экрана приложения календаря, отображающего выбранное событие календаря](calendar-images/14.png)](calendar-images/14.png#lightbox)

Как видите, Android предоставляет мощный и простой доступ для получения и сохранения данных календаря, позволяя приложениям легко интегрировать возможности календаря.

## <a name="related-links"></a>Связанные ссылки

- [Демонстрация календаря (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/calendardemo)
- [Знакомство со Южным Сандвичевым](https://www.android.com/about/ice-cream-sandwich/)
- [Платформа Android 4,0](https://developer.android.com/sdk/android-4.0.html)
