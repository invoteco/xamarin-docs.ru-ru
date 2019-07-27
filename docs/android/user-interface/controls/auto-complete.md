---
title: Автоматическое заполнение для Xamarin. Android
ms.prod: xamarin
ms.assetid: D4C8CA49-8369-35B7-798D-B147FDC24185
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/31/2018
ms.openlocfilehash: 810c6ddead66d191870ce97a50653f29737492b0
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510653"
---
# <a name="auto-complete-for-xamarinandroid"></a>Автоматическое заполнение для Xamarin. Android

`AutoCompleteTextView`— Это редактируемый элемент текстового представления, который автоматически показывает варианты завершения при вводе пользователем. Список предложений отображается в раскрывающемся меню, из которого пользователь может выбрать элемент для замены содержимого поля ввода на.

![Пример автоматического завершения](images/auto-complete.png)

## <a name="overview"></a>Обзор

Чтобы создать мини-приложение ввода текста, предоставляющее варианты автозавершения, используйте[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
». Предложения принимаются из коллекции строк, связанных с мини-приложением, [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)через.

В этом руководстве вы создадите элемент[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
Мини-приложение, содержащее предложения по названию страны.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:padding="5dp">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Country" />
    <AutoCompleteTextView android:id="@+id/autocomplete_country"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"/>
</LinearLayout>
```

[`TextView`](xref:Android.Widget.TextView) — Это метка, представляющая собой[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
».


## <a name="tutorial"></a>Учебник

Запустите новый проект с именем *хеллоаутокомплете*.

Создайте XML-файл с `list_item.xml` именем и сохраните его в папке **ресурсов или макета** . Задайте для `AndroidResource`действия сборки этого файла значение. Измените файл, чтобы он выглядел следующим образом:

```xml
<?xml version="1.0" encoding="utf-8"?>

<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp"
    android:textSize="16sp"
    android:textColor="#000">
</TextView> 
```

Этот файл определяет простой [`TextView`](xref:Android.Widget.TextView) , который будет использоваться для каждого элемента, отображаемого в списке предложений.

Откройте **ресурсы/макет/Main. axml** и вставьте следующее:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:padding="5dp">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Country" />
    <AutoCompleteTextView android:id="@+id/autocomplete_country"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"/>
</LinearLayout>
```

Откройте **MainActivity.CS** и вставьте следующий код для[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
Method

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "Main" layout resource
    SetContentView (Resource.Layout.Main);

    AutoCompleteTextView textView = FindViewById<AutoCompleteTextView> (Resource.Id.autocomplete_country);
    var adapter = new ArrayAdapter<String> (this, Resource.Layout.list_item, COUNTRIES);

    textView.Adapter = adapter;
}
```

После того как для `main.xml` представления содержимого задан макет, элемент[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
Мини-приложение записывается из [`FindViewById`](xref:Android.App.Activity.FindViewById*)макета с помощью. Затем инициализируется для `list_item.xml` привязки макета к `COUNTRIES` каждому элементу списка в массиве строк (определяется на следующем шаге). [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter) Наконец, `SetAdapter()` вызывается метод, чтобы [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter) связать с[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
мини-приложения, чтобы массив строк заполнил список предложений.

`MainActivity` Внутри класса добавьте массив строк:

```csharp
static string[] COUNTRIES = new string[] {
  "Afghanistan", "Albania", "Algeria", "American Samoa", "Andorra",
  "Angola", "Anguilla", "Antarctica", "Antigua and Barbuda", "Argentina",
  "Armenia", "Aruba", "Australia", "Austria", "Azerbaijan",
  "Bahrain", "Bangladesh", "Barbados", "Belarus", "Belgium",
  "Belize", "Benin", "Bermuda", "Bhutan", "Bolivia",
  "Bosnia and Herzegovina", "Botswana", "Bouvet Island", "Brazil", "British Indian Ocean Territory",
  "British Virgin Islands", "Brunei", "Bulgaria", "Burkina Faso", "Burundi",
  "Cote d'Ivoire", "Cambodia", "Cameroon", "Canada", "Cape Verde",
  "Cayman Islands", "Central African Republic", "Chad", "Chile", "China",
  "Christmas Island", "Cocos (Keeling) Islands", "Colombia", "Comoros", "Congo",
  "Cook Islands", "Costa Rica", "Croatia", "Cuba", "Cyprus", "Czech Republic",
  "Democratic Republic of the Congo", "Denmark", "Djibouti", "Dominica", "Dominican Republic",
  "East Timor", "Ecuador", "Egypt", "El Salvador", "Equatorial Guinea", "Eritrea",
  "Estonia", "Ethiopia", "Faeroe Islands", "Falkland Islands", "Fiji", "Finland",
  "Former Yugoslav Republic of Macedonia", "France", "French Guiana", "French Polynesia",
  "French Southern Territories", "Gabon", "Georgia", "Germany", "Ghana", "Gibraltar",
  "Greece", "Greenland", "Grenada", "Guadeloupe", "Guam", "Guatemala", "Guinea", "Guinea-Bissau",
  "Guyana", "Haiti", "Heard Island and McDonald Islands", "Honduras", "Hong Kong", "Hungary",
  "Iceland", "India", "Indonesia", "Iran", "Iraq", "Ireland", "Israel", "Italy", "Jamaica",
  "Japan", "Jordan", "Kazakhstan", "Kenya", "Kiribati", "Kuwait", "Kyrgyzstan", "Laos",
  "Latvia", "Lebanon", "Lesotho", "Liberia", "Libya", "Liechtenstein", "Lithuania", "Luxembourg",
  "Macau", "Madagascar", "Malawi", "Malaysia", "Maldives", "Mali", "Malta", "Marshall Islands",
  "Martinique", "Mauritania", "Mauritius", "Mayotte", "Mexico", "Micronesia", "Moldova",
  "Monaco", "Mongolia", "Montserrat", "Morocco", "Mozambique", "Myanmar", "Namibia",
  "Nauru", "Nepal", "Netherlands", "Netherlands Antilles", "New Caledonia", "New Zealand",
  "Nicaragua", "Niger", "Nigeria", "Niue", "Norfolk Island", "North Korea", "Northern Marianas",
  "Norway", "Oman", "Pakistan", "Palau", "Panama", "Papua New Guinea", "Paraguay", "Peru",
  "Philippines", "Pitcairn Islands", "Poland", "Portugal", "Puerto Rico", "Qatar",
  "Reunion", "Romania", "Russia", "Rwanda", "Sqo Tome and Principe", "Saint Helena",
  "Saint Kitts and Nevis", "Saint Lucia", "Saint Pierre and Miquelon",
  "Saint Vincent and the Grenadines", "Samoa", "San Marino", "Saudi Arabia", "Senegal",
  "Seychelles", "Sierra Leone", "Singapore", "Slovakia", "Slovenia", "Solomon Islands",
  "Somalia", "South Africa", "South Georgia and the South Sandwich Islands", "South Korea",
  "Spain", "Sri Lanka", "Sudan", "Suriname", "Svalbard and Jan Mayen", "Swaziland", "Sweden",
  "Switzerland", "Syria", "Taiwan", "Tajikistan", "Tanzania", "Thailand", "The Bahamas",
  "The Gambia", "Togo", "Tokelau", "Tonga", "Trinidad and Tobago", "Tunisia", "Turkey",
  "Turkmenistan", "Turks and Caicos Islands", "Tuvalu", "Virgin Islands", "Uganda",
  "Ukraine", "United Arab Emirates", "United Kingdom",
  "United States", "United States Minor Outlying Islands", "Uruguay", "Uzbekistan",
  "Vanuatu", "Vatican City", "Venezuela", "Vietnam", "Wallis and Futuna", "Western Sahara",
  "Yemen", "Yugoslavia", "Zambia", "Zimbabwe"
};
```

Это список предложений, которые будут предоставлены в раскрывающемся списке, когда пользователь вводит в[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
».

Запустите приложение. По мере ввода вы увидите нечто вроде:

[![Пример автозаполнения снимок экрана со списком имен, содержащих "CA"](auto-complete-images/helloautocomplete.png)](auto-complete-images/helloautocomplete.png#lightbox)



## <a name="more-information"></a>Дополнительные сведения

Обратите внимание, что использование жестко закодированного массива строк не является рекомендуемым подходом к проектированию, поскольку код приложения должен сосредоточиться на поведении, а не на содержимом. Содержимое приложения, например строки, должно быть внешним из кода для упрощения и упрощения локализации содержимого. Жестко запрограммированные строки используются в этом учебнике только для того, чтобы сделать его простым и сосредоточиться на[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
». Вместо этого приложение должно объявлять такие строковые массивы в XML-файле. Это можно сделать с помощью `<string-array>` ресурса в файле проекта. `res/values/strings.xml` Например:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
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

Чтобы использовать эти строки ресурсов для [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter), замените исходный элемент[`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)
строка конструктора со следующим:

```csharp
string[] countries = Resources.GetStringArray (Resource.array.countries_array);
var adapter = new ArrayAdapter<String> (this, Resource.layout.list_item, countries);
```


### <a name="references"></a>Ссылки

-   [Рецепт аутокомплететекствиев](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/autocomplete_text_view/add_an_autocomplete_text_input) Пример проекта Xamarin. Android `AutoCompleteTextView`для. &ndash;
-   [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)
-   [`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)

*Части этой страницы являются изменениями на основе работы, созданной и совместно используемой проектом Android с открытым кодом, и используются в соответствии с терминами,* 
описанными в[*лицензии*](http://creativecommons.org/licenses/by/2.5/) *Creative Commons Attribution 2,5. Этот*учебник основан на
[*руководстве*](https://developer.android.com/resources/tutorials/views/hello-autocomplete.html)
по автоматическому завершению Android *.*
