---
title: Счетчик Xamarin. Android
ms.prod: xamarin
ms.assetid: 004089E9-7C1D-2285-765A-B69143091F2A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: ba4a83eb997b879e8a2398f9857e2fd80221f8ef
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029167"
---
# <a name="xamarinandroid-spinner"></a>Счетчик Xamarin. Android

[`Spinner`](xref:Android.Widget.Spinner) — это мини-приложение, представляющее раскрывающийся список для выбора элементов. В этом руководство объясняется, как создать простое приложение, отображающее список вариантов в счетчике, за которым следуют изменения, отображающие другие значения, связанные с выбранным выбором.

## <a name="basic-spinner"></a>Базовый счетчик

В первой части этого руководства вы создадите простое мини-приложение счетчика, которое отображает список планет. Когда выбрана Планета, всплывающее сообщение отображает выбранный элемент:

[![пример снимков экрана приложения Хеллоспиннер](spinner-images/01-example-screenshots-sml.png)](spinner-images/01-example-screenshots.png#lightbox)

Запустите новый проект с именем **хеллоспиннер**.

Откройте файл **Resources/Layout/Main. axml** и вставьте следующий код XML:

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

Обратите внимание, что `android:text` атрибут [`TextView`](xref:Android.Widget.TextView)и атрибут `android:prompt` [`Spinner`](xref:Android.Widget.Spinner)ссылаются на один и тот же строковый ресурс. Этот текст ведет себя в качестве заголовка для мини-приложения. При применении к [`Spinner`](xref:Android.Widget.Spinner)текст заголовка будет отображаться в диалоговом окне выбора, которое появляется при выборе мини-приложения.

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

Второй элемент `<string>` определяет строку заголовка, на которую ссылается [`TextView`](xref:Android.Widget.TextView) , и [`Spinner`](xref:Android.Widget.Spinner) в приведенном выше макете.
Элемент `<string-array>` определяет список строк, которые будут отображаться в виде списка в мини-приложении [`Spinner`](xref:Android.Widget.Spinner) .

Теперь откройте **MainActivity.CS** и добавьте следующую инструкцию `using`:

```csharp
using System;
```

Затем вставьте следующий код для метода [`OnCreate()`](xref:Android.App.Activity.OnCreate*)):

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

После того как в качестве представления содержимого задается макет `Main.axml`, Мини-приложение [`Spinner`](xref:Android.Widget.Spinner) записывается из макета с [`FindViewById<>(int)`](xref:Android.App.Activity.FindViewById*).
[`CreateFromResource()`](xref:Android.Widget.ArrayAdapter.CreateFromResource*)
затем создает новый [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter), который привязывает каждый элемент в массиве строк к начальному внешнему виду для [`Spinner`](xref:Android.Widget.Spinner) (то есть, как каждый элемент будет отображаться в счетчике при выборе). Идентификатор `Resource.Array.planets_array` ссылается на `string-array`, определенный выше, а идентификатор `Android.Resource.Layout.SimpleSpinnerItem` ссылается на макет для стандартного внешнего вида, определенного платформой.
[`SetDropDownViewResource`](xref:Android.Widget.ArrayAdapter.SetDropDownViewResource*)
вызывается для определения внешнего вида каждого элемента при открытии мини-приложения. Наконец, [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter) задается для связывания всех своих элементов с [`Spinner`](xref:Android.Widget.Spinner) путем установки свойства [`Adapter`](xref:Android.Widget.ArrayAdapter) .

Теперь Предоставьте метод обратного вызова, который оповещает приложение о выборе элемента из [`Spinner`](xref:Android.Widget.Spinner). Вот как должен выглядеть этот метод:

```csharp
private void spinner_ItemSelected (object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format ("The planet is {0}", spinner.GetItemAtPosition (e.Position));
    Toast.MakeText (this, toast, ToastLength.Long).Show ();
}
```

При выборе элемента отправитель приводится к [`Spinner`](xref:Android.Widget.Spinner) , что позволяет получить доступ к элементам. С помощью свойства `Position` на `ItemEventArgs`можно узнать текст выбранного объекта и использовать его для вывода [`Toast`](xref:Android.Widget.Toast).

Запустите приложение. Он должен выглядеть следующим образом:

[![снимок экрана. Пример счетчика с выбранным режимом MARS в качестве планеты](spinner-images/02-basic-example-sml.png)](spinner-images/02-basic-example.png#lightbox)

## <a name="spinner-using-keyvalue-pairs"></a>Счетчик с использованием пар "ключ-значение"

Часто необходимо использовать `Spinner` для вывода значений ключей, связанных с некоторыми типами данных, используемыми приложением. Поскольку `Spinner` не работает непосредственно с парами "ключ-значение", необходимо хранить пару "ключ-значение" отдельно, заполнять `Spinner` значениями ключа, а затем использовать для поиска связанного значения данных расположение выбранного ключа в счетчике. 

На следующих шагах приложение **хеллоспиннер** изменяется для вывода средней температуры для выбранной планеты:

Добавьте следующую инструкцию `using` в **MainActivity.CS**:

```csharp
using System.Collections.Generic;
```

Добавьте следующую переменную экземпляра в класс `MainActivity`.
Этот список будет содержать пары "ключ-значение" для планет и их средние температуры:

```csharp
private List<KeyValuePair<string, string>> planets;
```

В методе `OnCreate` добавьте следующий код перед объявлением `adapter`:

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

Передайте этот список конструктору `ArrayAdapter` (вместо ресурса `planets_array`):

```csharp
var adapter = new ArrayAdapter<string>(this,
    Android.Resource.Layout.SimpleSpinnerItem, planetNames);
```

Измените `spinner_ItemSelected` таким образом, чтобы выбранная область использовалась для поиска значения (температуры), связанного с выбранной планетами:

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

[![пример выбора планеты с отображением температуры](spinner-images/03-keyvalue-example-sml.png)](spinner-images/03-keyvalue-example.png#lightbox)

## <a name="resources"></a>Ресурсы

- [`Resource.Layout`](xref:Android.Resource.Layout)
- [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)
- [`Spinner`](xref:Android.Widget.Spinner)

*Части этой страницы являются изменениями на основе работы, созданной и совместно используемой проектом Android с открытым кодом, и используются в соответствии с терминами, описанными в* [*лицензии
Creative Commons Attribution 2,5*](https://creativecommons.org/licenses/by/2.5/).
