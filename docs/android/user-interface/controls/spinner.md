---
title: Счетчик Xamarin. Android
ms.prod: xamarin
ms.assetid: 004089E9-7C1D-2285-765A-B69143091F2A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 2c7f0de2347e614b8c24de32bf3f88362a212a94
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510413"
---
# <a name="xamarinandroid-spinner"></a>Счетчик Xamarin. Android

[`Spinner`](xref:Android.Widget.Spinner)— Это мини-приложение, представляющее раскрывающийся список для выбора элементов. В этом руководство объясняется, как создать простое приложение, отображающее список вариантов в счетчике, за которым следуют изменения, отображающие другие значения, связанные с выбранным выбором.

## <a name="basic-spinner"></a>Базовый счетчик

В первой части этого руководства вы создадите простое мини-приложение счетчика, которое отображает список планет. Когда выбрана Планета, всплывающее сообщение отображает выбранный элемент:

[![Примеры снимков экрана приложения Хеллоспиннер](spinner-images/01-example-screenshots-sml.png)](spinner-images/01-example-screenshots.png#lightbox)

Запустите новый проект с именем **хеллоспиннер**.

Откройте файл Resources **/Layout/Main. axml** и вставьте следующий код XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:padding="10dip"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content">
    <TextView
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="10dip"
        android:text="@string/planet_prompt"
    />
    <Spinner
        android:id="@+id/spinner"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:prompt="@string/planet_prompt"
    />
</LinearLayout>
```

Обратите внимание [`TextView`](xref:Android.Widget.TextView), `android:text` что атрибут и [`Spinner`](xref:Android.Widget.Spinner) `android:prompt` атрибут атрибута ссылаются на один и тот же строковый ресурс. Этот текст ведет себя в качестве заголовка для мини-приложения. При применении к параметру [`Spinner`](xref:Android.Widget.Spinner)текст заголовка будет отображаться в диалоговом окне выбора, которое появляется при выборе мини-приложения.

Измените **Resources/Values/strings. XML** и измените файл, чтобы он выглядел следующим образом:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  <string name="app_name">HelloSpinner</string>
  <string name="planet_prompt">Choose a planet</string>
  <string-array name="planets_array">
    <item>Mercury</item>
    <item>Venus</item>
    <item>Earth</item>
    <item>Mars</item>
    <item>Jupiter</item>
    <item>Saturn</item>
    <item>Uranus</item>
    <item>Neptune</item>
  </string-array>
</resources>
```

Второй `<string>` элемент определяет строку заголовка, на которую ссылается объект [`TextView`](xref:Android.Widget.TextView) и [`Spinner`](xref:Android.Widget.Spinner) в приведенном выше макете.
Элемент определяет список строк, которые будут отображаться [`Spinner`](xref:Android.Widget.Spinner) в виде списка в мини-приложении. `<string-array>`

Теперь откройте **MainActivity.CS** и добавьте следующий `using` оператор:

```csharp
using System;
```

Затем вставьте следующий код для [`OnCreate()`](xref:Android.App.Activity.OnCreate*)метода):

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "Main" layout resource
    SetContentView (Resource.Layout.Main);

    Spinner spinner = FindViewById<Spinner> (Resource.Id.spinner);

    spinner.ItemSelected += new EventHandler<AdapterView.ItemSelectedEventArgs> (spinner_ItemSelected);
    var adapter = ArrayAdapter.CreateFromResource (
            this, Resource.Array.planets_array, Android.Resource.Layout.SimpleSpinnerItem);

    adapter.SetDropDownViewResource (Android.Resource.Layout.SimpleSpinnerDropDownItem);
    spinner.Adapter = adapter;
}
```

После того как [`Spinner`](xref:Android.Widget.Spinner) [`FindViewById<>(int)`](xref:Android.App.Activity.FindViewById*)макет будет установлен в качестве представления содержимого, Мини-приложение будет захвачено из макета с помощью. `Main.axml`
Тот[`CreateFromResource()`](xref:Android.Widget.ArrayAdapter.CreateFromResource*)
затем создает новый [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)объект, который привязывает каждый элемент в массиве строк к начальному внешнему виду для [`Spinner`](xref:Android.Widget.Spinner) объекта (то есть каждый элемент будет отображаться в счетчике при выборе). Идентификатор ссылается на `string-array` указанный выше, а `Android.Resource.Layout.SimpleSpinnerItem` идентификатор ссылается на макет для стандартного внешнего вида, определенного платформой. `Resource.Array.planets_array`
[`SetDropDownViewResource`](xref:Android.Widget.ArrayAdapter.SetDropDownViewResource*)
вызывается для определения внешнего вида каждого элемента при открытии мини-приложения. Наконец, [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter) [`Adapter`](xref:Android.Widget.ArrayAdapter) свойство задается для связывания всех его элементов [`Spinner`](xref:Android.Widget.Spinner) с путем установки свойства.

Теперь Предоставьте метод обратного вызова, который оповещает приложение о выборе элемента из [`Spinner`](xref:Android.Widget.Spinner). Вот как должен выглядеть этот метод:

```csharp
private void spinner_ItemSelected (object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format ("The planet is {0}", spinner.GetItemAtPosition (e.Position));
    Toast.MakeText (this, toast, ToastLength.Long).Show ();
}
```

При выборе элемента отправитель приводится к [`Spinner`](xref:Android.Widget.Spinner) типу, что позволяет получить доступ к элементам. С помощью `ItemEventArgs` [`Toast`](xref:Android.Widget.Toast)свойства в можно узнать текст выбранного объекта и использовать его для вывода. `Position`

Запустите приложение. Он должен выглядеть следующим образом:

[![Снимок экрана. Пример счетчика с выбранным режимом MARS в качестве планеты](spinner-images/02-basic-example-sml.png)](spinner-images/02-basic-example.png#lightbox)

## <a name="spinner-using-keyvalue-pairs"></a>Счетчик с использованием пар "ключ-значение"

Часто необходимо использовать `Spinner` для вывода значений ключей, связанных с некоторыми типами данных, используемыми приложением. Поскольку `Spinner` не работает непосредственно с парами "ключ-значение", необходимо сохранить пару "ключ-значение" отдельно, заполнить `Spinner` значениями ключа, а затем использовать расположение выбранного ключа в счетчике для поиска связанного значения данных. 

На следующих шагах приложение **хеллоспиннер** изменяется для вывода средней температуры для выбранной планеты:

Добавьте следующую `using` инструкцию в **MainActivity.CS**:

```csharp
using System.Collections.Generic;
```

Добавьте в `MainActivity` класс следующую переменную экземпляра.
Этот список будет содержать пары "ключ-значение" для планет и их средние температуры:

```csharp
private List<KeyValuePair<string, string>> planets;
```

В метод добавьте следующий код перед `adapter` объявлением: `OnCreate`

```csharp
planets = new List<KeyValuePair<string, string>>
{
    new KeyValuePair<string, string>("Mercury", "167 degrees C"),
    new KeyValuePair<string, string>("Venus", "464 degrees C"),
    new KeyValuePair<string, string>("Earth", "15 degrees C"),
    new KeyValuePair<string, string>("Mars", "-65 degrees C"),
    new KeyValuePair<string, string>("Jupiter" , "-110 degrees C"),
    new KeyValuePair<string, string>("Saturn", "-140 degrees C"),
    new KeyValuePair<string, string>("Uranus", "-195 degrees C"),
    new KeyValuePair<string, string>("Neptune", "-200 degrees C")
};
```

Этот код создает простое хранилище для планет и связанных с ними значений температуры. (В реальном приложении база данных обычно используется для хранения ключей и связанных с ними данных.)

Сразу после приведенного выше кода добавьте следующие строки, чтобы извлечь ключи и поместить их в список (по порядку):

```csharp
List<string> planetNames = new List<string>();
foreach (var item in planets)
    planetNames.Add (item.Key);
```

Передайте этот список `ArrayAdapter` конструктору (вместо `planets_array` ресурса):

```csharp
var adapter = new ArrayAdapter<string>(this,
    Android.Resource.Layout.SimpleSpinnerItem, planetNames);
```

Измените `spinner_ItemSelected` таким образом, чтобы выбранное расположение использовалось для поиска значения (температуры), связанного с выбранной Планета.

```csharp
private void spinner_ItemSelected(object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format("The mean temperature for planet {0} is {1}",
        spinner.GetItemAtPosition(e.Position), planets[e.Position].Value);
    Toast.MakeText(this, toast, ToastLength.Long).Show();
}
```

Запустите приложение. всплывающее уведомление должно выглядеть следующим образом:

[![Пример выбора планеты с отображением температуры](spinner-images/03-keyvalue-example-sml.png)](spinner-images/03-keyvalue-example.png#lightbox)

## <a name="resources"></a>Ресурсы

- [`Resource.Layout`](xref:Android.Resource.Layout)
- [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)
- [`Spinner`](xref:Android.Widget.Spinner)

*Части этой страницы являются изменениями на основе работы, созданной и совместно используемой проектом Android с открытым кодом, и используются в соответствии с терминами,* 
описанными в[*лицензии Creative Commons Attribution 2,5*](http://creativecommons.org/licenses/by/2.5/).
