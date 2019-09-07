---
title: Создание настраиваемого поставщика ContentProvider
description: В предыдущем разделе было показано, как использовать данные из встроенной реализации ContentProvider. В этом разделе объясняется, как создать пользовательский ContentProvider и затем использовать его данные.
ms.prod: xamarin
ms.assetid: 36742B59-607E-070E-5D0E-B9C18917D3F4
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/07/2018
ms.openlocfilehash: f72baaa4c74eb4bf0bb5eec64211d6ea2b18076c
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70756682"
---
# <a name="creating-a-custom-contentprovider"></a>Создание настраиваемого поставщика ContentProvider

_В предыдущем разделе было показано, как использовать данные из встроенной реализации ContentProvider. В этом разделе объясняется, как создать пользовательский ContentProvider и затем использовать его данные._

## <a name="about-contentproviders"></a>О Контентпровидерс

Класс поставщика содержимого должен наследовать от `ContentProvider`. Он должен состоять из внутреннего хранилища данных, которое используется для реагирования на запросы и должно предоставлять универсальные коды ресурса (URI) и типы MIME в качестве констант, которые помогают использовать код для выполнения допустимых запросов к данным.

### <a name="uri-authority"></a>URI (Authority)

`ContentProviders`доступны в Android с помощью URI. Приложение, предоставляющее объект `ContentProvider` , устанавливает URI, на которые он будет реагировать в своем файле **AndroidManifest. XML** . При установке приложения эти URI регистрируются, чтобы другие приложения могли получить к ним доступ.

В Mono для Android класс поставщика содержимого должен иметь `[ContentProvider]` атрибут, указывающий URI (или URI), которые должны быть добавлены в **AndroidManifest. XML**.

### <a name="mime-type"></a>Тип MIME

Стандартный формат для типов MIME состоит из двух частей. В `ContentProviders` Android обычно используются две строки для первой части типа MIME:

1. `vnd.android.cursor.item`чтобы представить одну строку, `ContentResolver.CursorItemBaseType` используйте константу в коде. &ndash;

1. `vnd.android.cursor.dir`для нескольких строк `ContentResolver.CursorDirBaseType` используйте константу в коде. &ndash;

Вторая часть типа MIME относится к конкретному приложению и должна использовать стандарт DNS с `vnd.` префиксом. В примере кода используется `vnd.com.xamarin.sample.Vegetables`.

### <a name="data-model-metadata"></a>Метаданные модели данных

Использование приложений требует создания запросов URI для доступа к различным типам данных. Базовый URI можно расширить, чтобы он ссылался на определенную таблицу данных, а также может включать параметры для фильтрации результатов. Также должны быть объявлены столбцы и предложения, используемые в результирующем курсоре для вывода данных.

Чтобы обеспечить построение только допустимых запросов URI, пользователь может предоставить допустимые строки как постоянные значения. Это упрощает доступ к, `ContentProvider` так как обеспечивает возможность обнаружения значений посредством завершения кода и предотвращает опечатки в строках.

В предыдущем примере `android.provider.ContactsContract` класс предоставлял метаданные для данных Contacts. Для нашего пользовательского `ContentProvider` класса мы просто предоставляем константы в самом классе.

## <a name="implementation"></a>Реализация

Создание и использование настраиваемых `ContentProvider`действий состоит из трех этапов.

1. **Создание класса базы данных** &ndash; Реализуйте `SQLiteOpenHelper`.

2. **Создайте`ContentProvider` класс** `ContentProvider` , реализующий с экземпляром базы данных, метаданные, предоставляемые как постоянные значения и методы для доступа к данным. &ndash;

3. **Доступ к через его URI заполняется с помощью, доступ к которому осуществляется через его URI. `ContentProvider`**  &ndash; `CursorAdapter` `ContentProvider`

Как уже говорилось `ContentProviders` , можно использовать приложения, отличные от того, где они определены. В этом примере данные используются в одном приложении, но следует помнить, что другие приложения также могут обращаться к нему, пока они узнают URI и сведения о схеме (которые обычно предоставляются как постоянные значения).

## <a name="create-a-database"></a>Создание базы данных

Большинство `ContentProvider` реализаций будет основано `SQLite` на базе данных. Пример кода базы данных в **симплеконтентпровидер/вежетабледатабасе. CS** создает очень простую базу данных из двух столбцов, как показано ниже.

```csharp
class VegetableDatabase  : SQLiteOpenHelper {
  const string create_table_sql =
    "CREATE TABLE [vegetables] ([_id] INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL UNIQUE, [name] TEXT NOT NULL UNIQUE)";
  const string DatabaseName = "vegetables.db";
  const int DatabaseVersion = 1;

  public VegetableDatabase(Context context) : base(context, DatabaseName, null, DatabaseVersion) { }
  public override void OnCreate(SQLiteDatabase db)
  {
    db.ExecSQL(create_table_sql);
    // seed with data
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Vegetables')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Fruits')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Flower Buds')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Legumes')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Bulbs')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Tubers')");
  }
  public override void OnUpgrade(SQLiteDatabase db, int oldVersion, int newVersion)
  {
    throw new NotImplementedException();
  }
}
```

Сама реализация базы данных не требует предоставления каких `ContentProvider`-либо специальных рекомендаций, однако если предполагается `ContentProvider's` привязать данные к `ListView` элементу управления, то уникальный целочисленный столбец с именем `_id` должен быть частью результирующий набор. Дополнительные сведения об использовании `ListView` элемента управления см. в документе [listviews and Adapters](~/android/user-interface/layouts/list-view/index.md) .

## <a name="create-the-contentprovider"></a>Создание ContentProvider

В оставшейся части этого раздела приводятся пошаговые инструкции о том, как был создан класс примера **симплеконтентпровидер/вежетаблепровидер. CS** .

### <a name="initialize-the-database"></a>Инициализация базы данных

Первый шаг — подкласс `ContentProvider` и Добавление базы данных, которую он будет использовать.

```csharp
public class VegetableProvider : ContentProvider 
{
    VegetableDatabase vegeDB;
    public override bool OnCreate()
    {
       vegeDB = new VegetableDatabase(Context);
        return true;
    }
}
```

Остальная часть кода будет формировать фактическую реализацию поставщика содержимого, которая позволяет обнаруживать и запрашивать данные.

## <a name="add-metadata-for-consumers"></a>Добавление метаданных для потребителей

Существует четыре типа метаданных, которые мы будем предоставлять в `ContentProvider` классе. Требуются только полномочия, остальные выполняются по соглашению.

- **Центр сертификации** Атрибут необходимо добавить в класс, чтобы он был зарегистрирован в Android при установке приложения. &ndash; `ContentProvider`

- **Универсальный код ресурса** &ndash; Объект`CONTENT_URI` предоставляется как константа, чтобы его можно было легко использовать в коде. Он должен соответствовать центру, но включать схему и базовый путь.

- **Типы MIME** &ndash; Списки результатов и отдельные результаты рассматриваются как различные типы содержимого, поэтому для их представления определяются два типа MIME.

- **Интерфацеконстс** &ndash; Укажите постоянное значение для каждого имени столбца данных, чтобы использование кода было легко обнаруживать и ссылаться на них без риска типографских ошибок.

Этот код показывает, как реализуется каждый из этих элементов, добавляя к определению базы данных из предыдущего шага:

```csharp
[ContentProvider(new string[] { CursorTableAdapter.VegetableProvider.AUTHORITY })]
public class VegetableProvider : ContentProvider 
{
   public const string AUTHORITY = "com.xamarin.sample.VegetableProvider";
   static string BASE_PATH = "vegetables";
   public static readonly Android.Net.Uri CONTENT_URI = Android.Net.Uri.Parse("content://" + AUTHORITY + "/" + BASE_PATH);
   // MIME types used for getting a list, or a single vegetable
   public const string VEGETABLES_MIME_TYPE = ContentResolver.CursorDirBaseType + "/vnd.com.xamarin.sample.Vegetables";
   public const string VEGETABLE_MIME_TYPE = ContentResolver.CursorItemBaseType + "/vnd.com.xamarin.sample.Vegetables";
   // Column names
   public static class InterfaceConsts {
       public const string Id = "_id";
       public const string Name = "name";
   }
   VegetableDatabase vegeDB;
   public override bool OnCreate()
   {
       vegeDB = new VegetableDatabase(Context);
       return true;
   }
}
```

## <a name="implement-the-uri-parsing-helper"></a>Реализация вспомогательного метода синтаксического анализа URI

Поскольку использование кода использует URI для выполнения запросов `ContentProvider`, необходимо иметь возможность анализировать эти запросы, чтобы определить возвращаемые данные. Класс может помочь проанализировать URI, когда он был инициализирован с помощью шаблонов URI `ContentProvider` , поддерживаемых. `UriMatcher`

`UriMatcher` В примере будет инициализировано два URI:

1. *"com. Xamarin. Sample. вежетаблепровидер/овощей"* &ndash; запрос на возврат полного списка овощей.

2. *"com. Xamarin. Sample. вежетаблепровидер/овощей/\#"* &ndash; , где \# `_id` — это заполнитель для числового параметра (строки в базе данных). Для сопоставления с текстовым\*параметром можно также использовать заполнитель звездочки ("").

В коде мы используем константы для ссылки на такие значения метаданных, как центр и базовый\_путь. Коды возврата будут использоваться в методах, которые выполняют синтаксический анализ URI, чтобы определить возвращаемые данные.

```csharp
const int GET_ALL = 0; // return code when list of Vegetables requested
const int GET_ONE = 1; // return code when a single Vegetable is requested by ID
static UriMatcher uriMatcher = BuildUriMatcher();
static UriMatcher BuildUriMatcher()
{
  var matcher = new UriMatcher(UriMatcher.NoMatch);
  // Uris to match, and the code to return when matched
  matcher.AddURI(AUTHORITY, BASE_PATH, GET_ALL); // all vegetables
  matcher.AddURI(AUTHORITY, BASE_PATH + "/#", GET_ONE); // specific vegetable by numeric ID
  return matcher;
}
```

Этот код является закрытым для `ContentProvider` класса. Дополнительные сведения см. в [документации по Уриматчер Google](xref:Android.Content.UriMatcher) .

## <a name="implement-the-querymethod"></a>Реализация Куеримесод

Простейшим `ContentProvider` методом для реализации `Query` является метод. Приведенная ниже реализация использует `UriMatcher` для `uri` анализа параметра и вызова правильного метода базы данных. Если содержит параметр ID, то выполняется синтаксический анализ целого числа (с `LastPathSegment`помощью) и используется в запросе к базе данных. `uri`

```csharp
public override Android.Database.ICursor Query(Android.Net.Uri uri, string[] projection, string selection, string[] selectionArgs, string sortOrder)
{
  switch (uriMatcher.Match(uri)) {
  case GET_ALL:
    return GetFromDatabase();
  case GET_ONE:
    var id = uri.LastPathSegment;
    return GetFromDatabase(id); // the ID is the last part of the Uri
  default:
    throw new Java.Lang.IllegalArgumentException("Unknown Uri: " + uri);
  }
}
Android.Database.ICursor GetFromDatabase()
{
  return vegeDB.ReadableDatabase.RawQuery("SELECT _id, name FROM vegetables", null);
}
Android.Database.ICursor GetFromDatabase(string id)
{
  return vegeDB.ReadableDatabase.RawQuery("SELECT _id, name FROM vegetables WHERE _id = " + id, null);
}
```

`GetType` Метод также должен быть переопределен. Этот метод может быть вызван для определения типа содержимого, который будет возвращен для данного URI.
Это может сообщить приложению об обработке этих данных.

```csharp
public override String GetType(Android.Net.Uri uri)
{
  switch (uriMatcher.Match(uri)) {
  case GET_ALL:
    return VEGETABLES_MIME_TYPE; // list
  case GET_ONE:
    return VEGETABLE_MIME_TYPE; // single item
  default:
    throw new Java.Lang.IllegalArgumentExceptoin ("Unknown Uri: " + uri);
   }
}
```

## <a name="implement-the-other-overrides"></a>Реализация других переопределений

Наш простой пример не позволяет изменять или удалять данные, но методы Insert, Update и DELETE должны быть реализованы, поэтому их можно добавить без реализации:

```csharp
public override int Delete(Android.Net.Uri uri, string selection, string[] selectionArgs)
{
  throw new Java.Lang.UnsupportedOperationException();
}
public override Android.Net.Uri Insert(Android.Net.Uri uri, ContentValues values)
{
  throw new Java.Lang.UnsupportedOperationException();
}
public override int Update(Android.Net.Uri uri, ContentValues values, string selection, string[] selectionArgs)
{
  throw new Java.Lang.UnsupportedOperationException();
}
```

Это завершает базовую `ContentProvider` реализацию. После установки приложения предоставляемые им данные будут доступны как внутри приложения, так и для любого другого приложения, которое знает универсальный код ресурса (URI) для ссылки на него.

## <a name="access-the-contentprovider"></a>Доступ к ContentProvider

`VegetableProvider` После реализации доступ к нему осуществляется так же, как поставщик контактов в начале этого документа: получение курсора по указанному универсальному коду ресурса (URI), а затем использование адаптера для доступа к данным.

## <a name="bind-a-listview-to-a-contentprovider"></a>Привязка ListView к ContentProvider

Для заполнения `ListView` данными используется URI, соответствующий нефильтрованному списку овощей. В коде мы используем значение `VegetableProvider.CONTENT_URI`константы, которое мы понимаем разрешениями. `com.xamarin.sample.vegetableprovider/vegetables` Наша `VegetableProvider.Query` реализация вернет курсор, который затем может быть привязан `ListView`к.

В `SimpleContentProvider/HomeScreen.cs` примере кода показано, как просто отобразить данные `ContentProvider`из:

```csharp
listView = FindViewById<ListView>(Resource.Id.List);
string[] projection = new string[] { VegetableProvider.InterfaceConsts.Id, VegetableProvider.InterfaceConsts.Name} ;
string[] fromColumns = new string[] { VegetableProvider.InterfaceConsts.Name };
int[] toControlIds = new int[] { Android.Resource.Id.Text1 };

// CursorLoader introduced in Honeycomb (3.0, API_11)
var loader = new CursorLoader(this,
   VegetableProvider.CONTENT_URI, projection, null, null, null);
cursor = (ICursor)loader.LoadInBackground();

// Create a SimpleCursorAdapter
adapter = new SimpleCursorAdapter(this, Android.Resource.Layout.SimpleListItem1, cursor, fromColumns, toControlIds);
listView.Adapter = adapter;
```

Полученное приложение выглядит следующим образом:

[![Снимок экрана: список приложений овощей, фруктам, цветок будс, Легумес, лампы, Туберс](custom-contentprovider-images/api11-contentprovider2.png)](custom-contentprovider-images/api11-contentprovider2.png#lightbox)

## <a name="retrieve-a-single-item-from-a-contentprovider"></a>Получение одного элемента из ContentProvider

Приложению может потребоваться доступ к отдельным строкам данных, что можно сделать, создав другой URI, ссылающийся на конкретную строку (например,).

Используйте `ContentResolver` непосредственно для доступа к одному элементу, создав URI с требуемым `Id`.

```csharp
Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
```

Полный метод выглядит следующим образом:

```csharp
protected void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
  var id = e.Id;
  string[] projection = new string[] { "name" };
  var uri = Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
  ICursor vegeCursor = ContentResolver.Query(uri, projection, null, new string[] { id.ToString() }, null);
  string text = "";
  if (vegeCursor.MoveToFirst()) {
    text = vegeCursor.GetInt(0) + " " + vegeCursor.GetString(1);
    Android.Widget.Toast.MakeText(this, text, Android.Widget.ToastLength.Short).Show();
  }
  vegeCursor.Close();
}
```

## <a name="related-links"></a>Связанные ссылки

- [Симплеконтентпровидер (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-simplecontentprovider)
