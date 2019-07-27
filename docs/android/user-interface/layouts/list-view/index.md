---
title: Использование ListView в Xamarin. Android
description: ListView является важным компонентом пользовательского интерфейса приложений Android; Он используется везде из коротких списков вариантов меню с длинными списками контактов или Интернет-избранного. Он предоставляет простой способ представления прокручиваемого списка строк, которые могут быть либо форматированы встроенным стилем, либо настроены с большой настройкой.
ms.prod: xamarin
ms.assetid: C2BA2705-9B20-01C2-468D-860BDFEDC157
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 653b6e3d41f9acb4ee3e9ee3626e72220687ccb8
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510086"
---
# <a name="xamarinandroid-listview"></a>ListView Xamarin. Android

_ListView является важным компонентом пользовательского интерфейса приложений Android; Он используется везде из коротких списков вариантов меню с длинными списками контактов или Интернет-избранного. Он предоставляет простой способ представления прокручиваемого списка строк, которые могут быть либо форматированы встроенным стилем, либо настроены с большой настройкой._

## <a name="overview"></a>Обзор

Представления списков и адаптеры включены в самые фундаментальные стандартные блоки приложений Android. `ListView` Класс предоставляет гибкий способ представления данных, будь то короткое меню или длинный список с прокруткой. Она предоставляет возможности для удобства использования, такие как Быстрая прокрутка, индексы и отдельные или множественные выборки, которые позволяют создавать удобные для мобильных приложений пользовательские интерфейсы. Экземпляру `ListView` требуется *адаптер*, позволяющий заполнять его данными из представлений строк.

В этом руководство объясняется, `ListView` как реализовать и `Adapter` различные классы в Xamarin. Android. В нем также показано `ListView`, как настроить внешний вид, и в нем обсуждается важность повторного использования строк для уменьшения потребления памяти. Кроме того, существует несколько обсуждений влияния `ListView` и `Adapter` использования жизненного цикла действия. При работе с приложениями на `ListView` разных платформах с помощью Xamarin. iOS элемент управления структурно напоминает iOS `UITableView` (и `UITableViewSource`Android `Adapter` аналогичен).

Во-первых, в кратком `ListView` учебнике описывается базовый пример кода. Далее приведены ссылки на более сложные разделы, помогающие использовать `ListView` их в реальных приложениях.

> [!NOTE]
> Мини-приложение — это более сложная и гибкая `ListView`версия. `RecyclerView` Так `RecyclerView` как предназначено для `ListView` последователя (и `GridView`) `ListView` , рекомендуется использовать `RecyclerView` вместо для разработки новых приложений. Дополнительные сведения см. в разделе [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md).

## <a name="listview-tutorial"></a>Руководство по ListView

[`ListView`](xref:Android.Widget.ListView)является[`ViewGroup`](xref:Android.Views.ViewGroup)
, создающий список прокручиваемых элементов. Элементы списка автоматически вставляются в список с помощью [`IListAdapter`](xref:Android.Widget.IListAdapter).

В этом руководстве вы создадите прокручиваемый список названий стран, которые считываются из массива строк. При выборе элемента списка в всплывающем сообщении будет отображаться позиция элемента в списке.

Запустите новый проект с именем **хеллолиствиев**.

Создайте XML-файл с именем **list_item. XML** и сохраните его в папке resources **/Layout/** . Вставьте следующее:

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp"
    android:textSize="16sp">
</TextView>
```

Этот файл определяет макет для каждого элемента, который будет помещен в [`ListView`](xref:Android.Widget.ListView).

Откройте `MainActivity.cs` и измените класс для расширения [`ListActivity`](xref:Android.App.ListActivity) (вместо [`Activity`](xref:Android.App.Activity)):

```csharp
public class MainActivity : ListActivity
{
```

Вставьте следующий код для [`OnCreate()`](xref:Android.App.Activity.OnCreate*)метода):

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);

    ListView.TextFilterEnabled = true;

    ListView.ItemClick += delegate (object sender, AdapterView.ItemClickEventArgs args)
    {
        Toast.MakeText(Application, ((TextView)args.View).Text, ToastLength.Short).Show();
    };
}
```

Обратите внимание, что при этом не загружается файл макета для действия (обычно с [`SetContentView(int)`](xref:Android.App.Activity.SetContentView*)помощью)).
Вместо этого установите для свойства[`ListAdapter`](xref:Android.App.ListActivity.ListAdapter)
свойство автоматически добавляет элемент[`ListView`](xref:Android.Widget.ListView)
для заполнения всего экрана [`ListActivity`](xref:Android.App.ListActivity).
Этот метод принимает объект [`ArrayAdapter<T>`](xref:Android.Widget.ArrayAdapter`1), который управляет массивом элементов списка, которые будут помещены [`ListView`](xref:Android.Widget.ListView)в.
Тот[`ArrayAdapter<T>`](xref:Android.Widget.ArrayAdapter`1)
Конструктор принимает приложение [`Context`](xref:Android.Content.Context), описание макета для каждого элемента списка (созданного на предыдущем шаге), `T[]` а также[`Java.Util.IList<T>`](xref:Java.Util.IList)
Массив объектов для вставки в[`ListView`](xref:Android.Widget.ListView)
(определяется далее).

Тот[`TextFilterEnabled`](xref:Android.Widget.AbsListView.TextFilterEnabled)
свойство включает фильтрацию текста для [`ListView`](xref:Android.Widget.ListView), чтобы при вводе пользователем текста список был отфильтрован.

Тот[`ItemClick`](xref:Android.Widget.AdapterView.ItemClick)
событие может использоваться для подписки обработчиков на нажатия. Когда элемент в элементе[`ListView`](xref:Android.Widget.ListView)
вызывается обработчик и[`Toast`](xref:Android.Widget.Toast)
выводится сообщение с использованием текста из элемента, который был выбран.

Можно использовать макеты элементов списка, предоставляемые платформой, а не определять собственный файл макета для [`ListAdapter`](xref:Android.App.ListActivity.ListAdapter).
Например, попробуйте использовать `Android.Resource.Layout.SimpleListItem1` `Resource.Layout.list_item`вместо.

Добавьте следующую `using` инструкцию:

```csharp
using System;
```
Затем добавьте следующий массив строк в качестве члена `MainActivity`:

```csharp
static readonly string[] countries = new String[] {
    "Afghanistan","Albania","Algeria","American Samoa","Andorra",
    "Angola","Anguilla","Antarctica","Antigua and Barbuda","Argentina",
    "Armenia","Aruba","Australia","Austria","Azerbaijan",
    "Bahrain","Bangladesh","Barbados","Belarus","Belgium",
    "Belize","Benin","Bermuda","Bhutan","Bolivia",
    "Bosnia and Herzegovina","Botswana","Bouvet Island","Brazil","British Indian Ocean Territory",
    "British Virgin Islands","Brunei","Bulgaria","Burkina Faso","Burundi",
    "Cote d'Ivoire","Cambodia","Cameroon","Canada","Cape Verde",
    "Cayman Islands","Central African Republic","Chad","Chile","China",
    "Christmas Island","Cocos (Keeling) Islands","Colombia","Comoros","Congo",
    "Cook Islands","Costa Rica","Croatia","Cuba","Cyprus","Czech Republic",
    "Democratic Republic of the Congo","Denmark","Djibouti","Dominica","Dominican Republic",
    "East Timor","Ecuador","Egypt","El Salvador","Equatorial Guinea","Eritrea",
    "Estonia","Ethiopia","Faeroe Islands","Falkland Islands","Fiji","Finland",
    "Former Yugoslav Republic of Macedonia","France","French Guiana","French Polynesia",
    "French Southern Territories","Gabon","Georgia","Germany","Ghana","Gibraltar",
    "Greece","Greenland","Grenada","Guadeloupe","Guam","Guatemala","Guinea","Guinea-Bissau",
    "Guyana","Haiti","Heard Island and McDonald Islands","Honduras","Hong Kong","Hungary",
    "Iceland","India","Indonesia","Iran","Iraq","Ireland","Israel","Italy","Jamaica",
    "Japan","Jordan","Kazakhstan","Kenya","Kiribati","Kuwait","Kyrgyzstan","Laos",
    "Latvia","Lebanon","Lesotho","Liberia","Libya","Liechtenstein","Lithuania","Luxembourg",
    "Macau","Madagascar","Malawi","Malaysia","Maldives","Mali","Malta","Marshall Islands",
    "Martinique","Mauritania","Mauritius","Mayotte","Mexico","Micronesia","Moldova",
    "Monaco","Mongolia","Montserrat","Morocco","Mozambique","Myanmar","Namibia",
    "Nauru","Nepal","Netherlands","Netherlands Antilles","New Caledonia","New Zealand",
    "Nicaragua","Niger","Nigeria","Niue","Norfolk Island","North Korea","Northern Marianas",
    "Norway","Oman","Pakistan","Palau","Panama","Papua New Guinea","Paraguay","Peru",
    "Philippines","Pitcairn Islands","Poland","Portugal","Puerto Rico","Qatar",
    "Reunion","Romania","Russia","Rwanda","Sqo Tome and Principe","Saint Helena",
    "Saint Kitts and Nevis","Saint Lucia","Saint Pierre and Miquelon",
    "Saint Vincent and the Grenadines","Samoa","San Marino","Saudi Arabia","Senegal",
    "Seychelles","Sierra Leone","Singapore","Slovakia","Slovenia","Solomon Islands",
    "Somalia","South Africa","South Georgia and the South Sandwich Islands","South Korea",
    "Spain","Sri Lanka","Sudan","Suriname","Svalbard and Jan Mayen","Swaziland","Sweden",
    "Switzerland","Syria","Taiwan","Tajikistan","Tanzania","Thailand","The Bahamas",
    "The Gambia","Togo","Tokelau","Tonga","Trinidad and Tobago","Tunisia","Turkey",
    "Turkmenistan","Turks and Caicos Islands","Tuvalu","Virgin Islands","Uganda",
    "Ukraine","United Arab Emirates","United Kingdom",
    "United States","United States Minor Outlying Islands","Uruguay","Uzbekistan",
    "Vanuatu","Vatican City","Venezuela","Vietnam","Wallis and Futuna","Western Sahara",
    "Yemen","Yugoslavia","Zambia","Zimbabwe"
  };
```

Это массив строк, которые будут помещены [`ListView`](xref:Android.Widget.ListView)в.

Запустите приложение. Можно прокрутить список или ввести, чтобы отфильтровать его, а затем щелкнуть элемент, чтобы просмотреть сообщение. Результат должен быть примерно таким:

[![Пример снимка экрана ListView с названиями стран](images/01-listview-example-sml.png)](images/01-listview-example.png#lightbox)

Обратите внимание, что использование жестко запрограммированного массива строк не является лучшим подходом к проектированию. Он используется в этом руководстве для простоты, чтобы продемонстрировать[`ListView`](xref:Android.Widget.ListView)
». Рекомендуется ссылаться на массив строк, определенный внешним ресурсом, например с `string-array` ресурсом в файле resources **/Values/strings. XML** . Например:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  <string name="app_name">HelloListView</string>
  <string-array name="countries_array">
    <item>Bahrain</item>
    <item>Bangladesh</item>
    <item>Barbados</item>
    <item>Belarus</item>
    <item>Belgium</item>
    <item>Belize</item>
    <item>Benin</item>
  </string-array>
</resources>
```

Чтобы использовать эти строки ресурсов для [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter`1), замените исходный элемент[`ListAdapter`](xref:Android.App.ListActivity.ListAdapter)
строка со следующим:

```csharp
string[] countries = Resources.GetStringArray (Resource.Array.countries_array);
ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);
```

Запустите приложение. Результат должен быть примерно таким:

[![Пример снимка экрана ListView с меньшим списком имен](images/02-smaller-example-sml.png)](images/02-smaller-example.png#lightbox)

## <a name="going-further-with-listview"></a>Дальнейшие переходы с помощью ListView

В остальных разделах (связанных ниже) подробно рассматривается работа с `ListView` классом и типы адаптеров различных типов, которые можно использовать с ним. Эта структура выглядит следующим образом:

-   **Визуальный вид** &ndash; Части элементауправленияи`ListView` принципы их работы.

-   **Классы** Общие сведения о классах, `ListView`используемых для вывода. &ndash;

-   **Отображение данных в ListView** Как отобразить простой список данных, как реализовать `ListView's` функции для удобства использования, как использовать разные встроенные макеты строк и как адаптеры сохраняют память путем повторного использования представлений строк. &ndash;

-   **Пользовательский внешний вид** &ndash; Изменение стиля`ListView` с пользовательскими макетами, шрифтами и цветами.

-   **Использование SQLite** Сведения о том, как отображать данные из базы данных `CursorAdapter`SQLite с помощью. &ndash;

-   **Жизненный цикл действия** Рекомендации по проектированию `ListView` при реализации действий, включая, где в жизненном цикле следует заполнять данные и когда следует освобождать ресурсы. &ndash;

Обсуждение (разбитое на шесть частей) начинается с обзора `ListView` самого класса перед тем, как Познакомьтесь с более сложными примерами его использования.

-   [Компоненты ListView и функции](~/android/user-interface/layouts/list-view/parts-and-functionality.md)
-   [Заполнение ListView данными](~/android/user-interface/layouts/list-view/populating.md)
-   [Настройка вида ListView](~/android/user-interface/layouts/list-view/customizing-appearance.md)
-   [Использование CursorAdapters](~/android/user-interface/layouts/list-view/cursor-adapters.md)
-   [Использование ContentProvider](~/android/user-interface/layouts/list-view/content-provider.md)
-   [ListView и жизненный цикл действия](~/android/user-interface/layouts/list-view/activity-lifecycle.md)


## <a name="summary"></a>Сводка

В этом наборе разделов `ListView` представлено несколько примеров использования встроенных функций `ListActivity`. В нем обсуждаются пользовательские реализации `ListView` , разрешенные для цветных макетов и использования базы данных SQLite, и они кратко затронули важность жизненного `ListView` цикла действия в реализации.


## <a name="related-links"></a>Связанные ссылки

- [Акцессоривиевс (пример)](https://developer.xamarin.com/samples/monodroid/AccessoryViews/)
- [Басиктаблеандроид (пример)](https://developer.xamarin.com/samples/monodroid/BasicTableAndroid/)
- [Басиктаблеадаптер (пример)](https://developer.xamarin.com/samples/monodroid/BasicTableAdapter/)
- [Буилтинвиевс (пример)](https://developer.xamarin.com/samples/monodroid/BuiltInViews/)
- [Кустомроввиев (пример)](https://developer.xamarin.com/samples/monodroid/CustomRowView/)
- [Фастскролл (пример)](https://developer.xamarin.com/samples/monodroid/FastScroll/)
- [Сектиониндекс (пример)](https://developer.xamarin.com/samples/monodroid/SectionIndex/)
- [Симплекурсортаблеадаптер (пример)](https://developer.xamarin.com/samples/monodroid/SimpleCursorTableAdapter/)
- [Курсортаблеадаптер (пример)](https://developer.xamarin.com/samples/monodroid/CursorTableAdapter/)
- [Учебник по жизненному циклу действий](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Работа с таблицами и ячейками (в Xamarin. iOS)](~/ios/user-interface/controls/tables/index.md)
- [Ссылка на класс ListView](xref:Android.Widget.ListView)
- [Справочник по классам Листактивити](xref:Android.App.ListActivity)
- [Справочник по классам Басеадаптер](xref:Android.Widget.BaseAdapter)
- [Справочник по классам ArrayAdapter](xref:Android.Widget.ArrayAdapter)
- [Справочник по классам Курсорадаптер](xref:Android.Widget.CursorAdapter)
