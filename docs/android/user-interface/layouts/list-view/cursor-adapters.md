---
title: Использование CursorAdapters
ms.prod: xamarin
ms.assetid: 60DE467E-A5DA-4420-52E5-D86AD1678FE6
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 10/25/2017
ms.openlocfilehash: d0b5845036ab2981a4aa06d2a01ed6b13d094bef
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028916"
---
# <a name="using-cursoradapters-with-xamarinandroid"></a>Использование Курсорадаптерс с Xamarin. Android

Android предоставляет классы адаптеров, специально предназначенные для вывода данных из запроса к базе данных SQLite:

 **Симплекурсорадаптер** — аналогичен `ArrayAdapter`, так как он может использоваться без подклассов. Просто укажите необходимые параметры (например, сведения о курсоре и макете) в конструкторе, а затем назначьте `ListView`.

 **Курсорадаптер** — базовый класс, который можно унаследовать, если требуется больший контроль над привязкой значений данных к элементам управления макета (например, скрытие или отображение элементов управления или изменение их свойств).

Адаптеры курсоров обеспечивают высокопроизводительный способ прокрутки длинных списков данных, хранящихся в SQLite. В коде, использующем код, необходимо определить SQL-запрос в `Cursor` объекте, а затем описать, как создавать и заполнять представления для каждой строки.

## <a name="creating-an-sqlite-database"></a>Создание базы данных SQLite

Для демонстрации использования адаптеров курсоров требуется простая реализация SQLite базы данных. Код в **симплекурсортаблеадаптер/вежетабледатабасе. CS** содержит код и SQL для создания таблицы и заполнения ее данными.
Полный класс `VegetableDatabase` показан здесь:

```csharp
class VegetableDatabase  : SQLiteOpenHelper {
   public static readonly string create_table_sql =
       "CREATE TABLE [vegetables] ([_id] INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL UNIQUE, [name] TEXT NOT NULL UNIQUE)";
   public static readonly string DatabaseName = "vegetables.db";
   public static readonly int DatabaseVersion = 1;
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
   {   // not required until second version :)
       throw new NotImplementedException();
   }
}
```

Экземпляр класса `VegetableDatabase` будет создан в методе `OnCreate` действия `HomeScreen`. Базовый класс `SQLiteOpenHelper` управляет настройкой файла базы данных и гарантирует, что SQL в своем методе `OnCreate` выполняется только один раз. Этот класс используется в следующих двух примерах для `SimpleCursorAdapter` и `CursorAdapter`.

Для работы `CursorAdapter` запрос курсора *должен* иметь целочисленный столбец `_id`. Если базовая таблица не содержит целочисленный столбец с именем `_id`, используйте псевдоним столбца для другого уникального целого числа в `RawQuery`, составляющем курсор. Дополнительные сведения см. в документации по [Android](xref:Android.Widget.CursorAdapter) .

### <a name="creating-the-cursor"></a>Создание курсора

В примерах используется `RawQuery` для преобразования SQL-запроса в объект `Cursor`. Список столбцов, возвращаемый курсором, определяет столбцы данных, доступные для вывода в адаптере курсора. Код, создающий базу данных в методе **симплекурсортаблеадаптер/хомескрин. cs** `OnCreate`, показан здесь:

```csharp
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null); // cursor query
StartManagingCursor(cursor);
// use either SimpleCursorAdapter or CursorAdapter subclass here!
```

Любой код, вызывающий `StartManagingCursor`, должен также вызывать `StopManagingCursor`. В примерах используется `OnCreate` для запуска и `OnDestroy` для закрытия курсора. Метод `OnDestroy` содержит следующий код:

```csharp
StopManagingCursor(cursor);
cursor.Close();
```

После того как приложение получило доступ к базе данных SQLite и создала объект Cursor, как показано ниже, он может использовать `SimpleCursorAdapter` или подкласс `CusorAdapter` для отображения строк в `ListView`.

## <a name="using-simplecursoradapter"></a>Использование Симплекурсорадаптер

`SimpleCursorAdapter` похож на `ArrayAdapter`, но он специально предназначен для использования с SQLite. Он не требует подклассов — просто задайте некоторые простые параметры при создании объекта, а затем присвойте его свойству `Adapter` `ListView`.

Параметры для конструктора Симплекурсорадаптер:

 **Context** — ссылка на содержащее действие.

 **Layout** — идентификатор ресурса для используемого представления строк.

 **Икурсор** — курсор, содержащий запрос SQLite для отображаемых данных.

 **Из** массива строк — массив строк, соответствующий именам столбцов в курсоре.

 **В** массив целых чисел — массив идентификаторов макета, соответствующих элементам управления в макете строки. Значение столбца, указанного в массиве `from`, будет привязано к ControlID, указанному в этом массиве в том же индексе.

Массивы `from` и `to` должны иметь одинаковое число записей, поскольку они формируют сопоставление источника данных с элементами управления макета в представлении.

Пример кода **симплекурсортаблеадаптер/хомескрин. CS** подсоединяет `SimpleCursorAdapter` следующим образом:

```csharp
// which columns map to which layout controls
string[] fromColumns = new string[] {"name"};
int[] toControlIDs = new int[] {Android.Resource.Id.Text1};
// use a SimpleCursorAdapter
listView.Adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor,
       fromColumns,
       toControlIDs);
```

`SimpleCursorAdapter` — это быстрый и простой способ отобразить данные SQLite в `ListView`. Основное ограничение заключается в том, что он может привязывать значения столбцов только к элементам управления для отображения, но не позволяет изменять другие аспекты макета строки (например, отображение или скрытие элементов управления или изменение свойств).

## <a name="subclassing-cursoradapter"></a>Подклассировать Курсорадаптер

Подкласс `CursorAdapter` имеет те же преимущества производительности, что и `SimpleCursorAdapter` для отображения данных из SQLite, но он также обеспечивает полный контроль над созданием и структурой каждого представления строк. Реализация `CursorAdapter` сильно отличается от подкласса `BaseAdapter` поскольку не переопределяет `GetView`, `GetItemId`, `Count` или индексатора `this[]`.

При наличии рабочей базы данных SQLite необходимо переопределить два метода, чтобы создать `CursorAdapter` подкласс:

- **Биндвиев** — заданное представление, обновите его, чтобы отобразить данные в указанном курсоре.

- **Неввиев** — вызывается, когда для отображения `ListView` требуется новое представление. `CursorAdapter` будет относиться к повторному выполнению представлений (в отличие от метода `GetView` на обычных адаптерах).

Подклассы адаптера в предыдущих примерах имеют методы для возвращения количества строк и получения текущего элемента — `CursorAdapter` не требует этих методов, так как эти сведения могут быть получены из самого курсора. Разделив создание и заполнение каждого представления этими двумя методами, `CursorAdapter` применяет повторное использование представления. Это отличается от обычного адаптера, в котором можно игнорировать параметр `convertView` метода `BaseAdapter.GetView`.

### <a name="implementing-the-cursoradapter"></a>Реализация Курсорадаптер

Код в **курсортаблеадаптер/хомескринкурсорадаптер. CS** содержит подкласс `CursorAdapter`. Он хранит ссылку на контекст, переданную в конструктор, чтобы он мог получить доступ к `LayoutInflater` в методе `NewView`. Полный класс выглядит следующим образом:

```csharp
public class HomeScreenCursorAdapter : CursorAdapter {
   Activity context;
   public HomeScreenCursorAdapter(Activity context, ICursor c)
       : base(context, c)
   {
       this.context = context;
   }
   public override void BindView(View view, Context context, ICursor cursor)
   {
       var textView = view.FindViewById<TextView>(Android.Resource.Id.Text1);
       textView.Text = cursor.GetString(1); // 'name' is column 1 in the cursor query
   }
   public override View NewView(Context context, ICursor cursor, ViewGroup parent)
   {
       return this.context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, parent, false);
   }
}
```

### <a name="assigning-the-cursoradapter"></a>Назначение Курсорадаптер

В `Activity`, где будет отображаться `ListView`, создайте курсор и `CursorAdapter` затем назначьте его представлению списка.

Код, выполняющий это действие в методе **курсортаблеадаптер/хомескрин. cs** `OnCreate`, показан здесь:

```csharp
// create the cursor
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null);
StartManagingCursor(cursor);

// create the CursorAdapter
listView.Adapter = (IListAdapter)new HomeScreenCursorAdapter(this, cursor, false);
```

Метод `OnDestroy` содержит вызов метода `StopManagingCursor`, описанный выше.

## <a name="related-links"></a>Связанные ссылки

- [Симплекурсортаблеадаптер (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/simplecursortableadapter)
- [Курсортаблеадаптер (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/cursortableadapter)
