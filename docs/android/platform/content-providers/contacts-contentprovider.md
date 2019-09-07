---
title: Использование контактов ContentProvider
ms.prod: xamarin
ms.assetid: 21C5D1B4-3783-6090-33AB-78A484E65925
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/22/2018
ms.openlocfilehash: 0784fe5fe42fc82d7067c976bdda6f0436e140b5
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757581"
---
# <a name="using-the-contacts-contentprovider"></a>Использование контактов ContentProvider

Код, использующий данные доступа, `ContentProvider` предоставляемый, не требует ссылки `ContentProvider` на класс. Вместо этого URI используется для создания курсора на основе `ContentProvider`данных, предоставляемых. Android использует универсальный код ресурса (URI) для поиска приложения в системе, `ContentProvider` которое предоставляет идентификатор. URI — это строка, обычно в формате обратных DNS, например `com.android.contacts/data`.

Вместо того, чтобы заставить разработчиков запомнить эту строку, поставщик *контактов* Android предоставляет свои метаданные в `android.provider.ContactsContract` классе. Этот класс используется для определения универсального кода ресурса ( `ContentProvider` URI), а также имен таблиц и столбцов, к которым можно получить запрос.

Для доступа к некоторым типам данных также требуется специальное разрешение. Для встроенного списка контактов требуется `android.permission.READ_CONTACTS` разрешение в файле **AndroidManifest. XML** .

Существует три способа создания курсора из универсального кода ресурса (URI):

1. **Манажедкуери ()** Предпочтительный подход в Android 2,3 (API уровня 10) и более ранних `ManagedQuery` версий — возвращает курсор, а также автоматически управляет обновлением данных и закрытием курсора. &ndash; Этот метод является устаревшим в Android 3,0 (API уровня 11).

1. **ContentResolver. Query ()** &ndash; Возвращает неуправляемый курсор, который означает, что он должен быть обновлен и закрыт явным образом в коде.

1. **Курсорлоадер (). Лоадинбаккграунд ()** &ndash; , появившийся в Android 3,0 (API уровня 11 `CursorLoader` ), теперь является предпочтительным способом использования `ContentProvider` . `CursorLoader`запрашивает `ContentResolver` объект в фоновом потоке, чтобы пользовательский интерфейс не блокировался.
   Доступ к этому классу можно получить в более ранних версиях Android с помощью библиотеки совместимости v4.

Каждый из этих методов имеет один и тот же базовый набор входных данных:

- **Универсальный код ресурса** &ndash; Полное имя `ContentProvider` .
- **Проекция** &ndash; Спецификация выбора столбцов для курсора.
- **Выделенный фрагмент** Аналогично предложению `WHERE`SQL. &ndash;
- **Селектионаргс** &ndash; Параметры, подставляемые в выделенном фрагменте.
- **SortOrder** &ndash; Столбцы, по которым выполняется сортировка.

## <a name="creating-inputs-for-a-query"></a>Создание входных данных для запроса

`ContactsProvider` Пример кода выполняет очень простой запрос к встроенному поставщику контактов Android. Вам не нужно знать фактический URI или имена столбцов. все сведения, необходимые для запроса контактов `ContentProvider` , доступны в виде констант, предоставляемых `ContactsContract` классом.

Независимо от того, какой метод используется для получения курсора, эти же объекты используются в качестве параметров, как показано в файле *контактспровидер/контактсадаптер. CS* :

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName,
   ContactsContract.Contacts.InterfaceConsts.PhotoId,
};
```

В `selection` `sortOrder` этом примере, `null`и будут игнорироваться, задав для них значение. `selectionArgs`

## <a name="creating-a-cursor-from-a-content-provider-uri"></a>Создание курсора из URI поставщика содержимого

После создания объектов параметров их можно использовать одним из следующих трех способов:

### <a name="using-a-managed-query"></a>Использование управляемого запроса

В приложениях, предназначенных для Android 2,3 (на уровне API 10) или более ранней версии, должен использоваться следующий метод:

```csharp
var cursor = activity.ManagedQuery(uri, projection, null, null, null);
```

Этот курсор будет управляться Android, поэтому вам не нужно его закрывать.

### <a name="using-contentresolver"></a>Использование ContentResolver

Прямой `ContentResolver` доступ для получения курсора по `ContentProvider` может быть выполнен следующим образом:

```csharp
var cursor = activity.ContentResolver(uri, projection, null, null, null);
```

Этот курсор является неуправляемым, поэтому он должен быть закрыт, когда он больше не требуется.
Убедитесь, что код закрывает открытый курсор, в противном случае возникнет ошибка.

```csharp
cursor.Close();
```

Кроме того, можно вызвать `StartManagingCursor()` метод `StopManagingCursor()` и для управления курсором. Управляемые курсоры автоматически отключаются и повторно запрашиваются при остановке и перезапуске действий.

### <a name="using-cursorloader"></a>Использование Курсорлоадер

Приложения, созданные для Android 3,0 (уровень API 11) или более поздней версии, должны использовать этот метод:

```csharp
var loader = new CursorLoader (activity, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
```

`CursorLoader` Гарантирует, что все операции с курсорами выполняются в фоновом потоке и могут при перезапуске повторно использовать существующий курсор в экземплярах действия, когда действие перезапускается (например, из-за изменения конфигурации), а затем повторно загружает данные.

Более ранние версии Android также могут `CursorLoader` использовать класс с помощью [библиотек поддержки v4](https://developer.android.com/tools/support-library/index.html).

## <a name="displaying-the-cursor-data-with-a-custom-adapter"></a>Отображение данных курсора с помощью пользовательского адаптера

Чтобы отобразить Контактное изображение, мы будем использовать пользовательский адаптер, чтобы можно было вручную разрешить `PhotoId` ссылку на путь к файлу изображения.

Чтобы отобразить данные с помощью пользовательского адаптера, в примере используется `CursorLoader` для получения всех данных о контактах в локальную коллекцию в методе **филлконтактс** из **контактспровидер/контактсадаптер. CS**:

```csharp
void FillContacts ()
{
   var uri = ContactsContract.Contacts.ContentUri;
   string[] projection = {
       ContactsContract.Contacts.InterfaceConsts.Id,
       ContactsContract.Contacts.InterfaceConsts.DisplayName,
       ContactsContract.Contacts.InterfaceConsts.PhotoId
  };
   // CursorLoader introduced in Honeycomb (3.0, API11)
   var loader = new CursorLoader(activity, uri, projection, null, null, null);
   var cursor = (ICursor)loader.LoadInBackground();
   contactList = new List<Contact> ();
   if (cursor.MoveToFirst ()) {
      do {
          contactList.Add (new Contact{
              Id = cursor.GetLong (cursor.GetColumnIndex (projection [0])),
              DisplayName = cursor.GetString (cursor.GetColumnIndex (projection [1])),
              PhotoId = cursor.GetString (cursor.GetColumnIndex (projection [2]))
          });
       } while (cursor.MoveToNext());
   }
}
```

Затем реализуйте методы басеадаптер с помощью `contactList` коллекции. Адаптер реализуется так же, как и в любой другой коллекции &ndash; , здесь нет специальной обработки, так как источником данных является источник. `ContentProvider`

```csharp
Activity activity;
public ContactsAdapter (Activity activity)
{
   this.activity = activity;
   FillContacts ();
}
public override int Count {
   get { return contactList.Count; }
}
public override Java.Lang.Object GetItem (int position)
{
  return null; // could wrap a Contact in a Java.Lang.Object to return it here if needed
}
public override long GetItemId (int position)
{
   return contactList [position].Id;
}
public override View GetView (int position, View convertView, ViewGroup parent)
{
   var view = convertView ?? activity.LayoutInflater.Inflate (Resource.Layout.ContactListItem, parent, false);
   var contactName = view.FindViewById<TextView> (Resource.Id.ContactName);
   var contactImage = view.FindViewById<ImageView> (Resource.Id.ContactImage);
   contactName.Text = contactList [position].DisplayName;
   if (contactList [position].PhotoId == null) {
       contactImage = view.FindViewById<ImageView> (Resource.Id.ContactImage);
       contactImage.SetImageResource (Resource.Drawable.ContactImage);
   } else {
       var contactUri = ContentUris.WithAppendedId (ContactsContract.Contacts.ContentUri, contactList [position].Id);
       var contactPhotoUri = Android.Net.Uri.WithAppendedPath (contactUri, Contacts.Photos.ContentDirectory);
       contactImage.SetImageURI (contactPhotoUri);
   }
   return view;
}
```

Изображение отображается (если оно существует), используя универсальный код ресурса (URI) для файла изображения на устройстве. Приложение выглядит следующим образом:

[![Снимок экрана приложения, отображающего контакты в ListView; изображение отображается слева от одной записи](contacts-contentprovider-images/contactsprovider.png)](contacts-contentprovider-images/contactsprovider.png#lightbox)

Используя аналогичный шаблон кода, приложение может получить доступ к разнообразным системным данным, включая фотографии, видео и музыку пользователя.
Некоторые типы данных должны запрашивать специальные разрешения в **файле AndroidManifest. XML**проекта.

## <a name="displaying-the-cursor-data-with-a-simplecursoradapter"></a>Отображение данных курсора с помощью Симплекурсорадаптер

Курсор также можно отобразить с `SimpleCursorAdapter` помощью (хотя будет отображаться только имя, а не фотография). В этом коде показано, как использовать `ContentProvider` с `SimpleCursorAdapter` (этот код не отображается в примере):

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName
};
var loader = new CursorLoader (this, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
var fromColumns = new string[] {ContactsContract.Contacts.InterfaceConsts.DisplayName};
var toControlIds = new int[] {Android.Resource.Id.Text1};
adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor, fromColumns, toControlsIds);
listView.Adapter = adapter;
```

Дополнительные сведения о реализации `SimpleCursorAdapter`см. в [listviewх и адаптерах](~/android/user-interface/layouts/list-view/index.md) .

## <a name="related-links"></a>Связанные ссылки

- [Демонстрация Контактсадаптер (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-contactsadapterdemo)
