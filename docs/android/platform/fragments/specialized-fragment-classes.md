---
title: Специализированные классы фрагментов
ms.prod: xamarin
ms.assetid: 7A0AEB2C-EE77-63BF-652A-DA049B691C64
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/08/2018
ms.openlocfilehash: b004fbf121374a2bb3bf5d85f45d8cae293573bf
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027305"
---
# <a name="specialized-fragment-classes"></a>Специализированные классы фрагментов

API фрагментов предоставляет дополнительные подклассы, которые инкапсулируют несколько функций, часто используемых в приложениях. Вот эти подклассы:

- **ListFragment** &ndash; этот фрагмент используется для вывода списка элементов, привязанных к источнику данных, например к массиву или курсору.

- **DialogFragment** &ndash; этот фрагмент используется в качестве оболочки для диалога. Этот фрагмент позволяет отобразить диалоговое окно поверх соответствующего действия.

- **PreferenceFragment** &ndash; этот фрагмент используется для отображения объектов Preference в качестве списков.

## <a name="the-listfragment"></a>ListFragment

По концепции и функциональным возможностям `ListFragment` очень похож на `ListActivity`: это оболочка, которая размещает `ListView` в фрагменте. На рисунке ниже показан подкласс `ListFragment`, работающий на планшете и телефоне:

[![Снимки экрана с ListFragment на планшете и на телефоне](specialized-fragment-classes-images/intro-screenshot-sml.png)](specialized-fragment-classes-images/intro-screenshot.png#lightbox)

### <a name="binding-data-with-the-listadapter"></a>Привязка данных с помощью ListAdapter

Класс `ListFragment` уже предоставляет макет по умолчанию. Поэтому нет необходимости переопределять `OnCreateView` для отображения содержимого `ListFragment`. `ListView` привязывается к данным с помощью реализации `ListAdapter`. В следующем примере показано, как это сделать, используя простой массив строк.

```csharp
public override void OnActivityCreated(Bundle savedInstanceState)
{
    base.OnActivityCreated(savedInstanceState);
    string[] values = new[] { "Android", "iPhone", "WindowsMobile",
                "Blackberry", "WebOS", "Ubuntu", "Windows7", "Max OS X",
                "Linux", "OS/2" };
    this.ListAdapter = new ArrayAdapter<string>(Activity, Android.Resource.Layout.SimpleExpandableListItem1, values);
}
```

При настройке `ListAdapter` важно использовать свойство `ListFragment.ListAdapter`, а не свойство `ListView.ListAdapter`. Использование `ListView.ListAdapter` приведет к тому, что важный код инициализации будет пропущен.

### <a name="responding-to-user-selection"></a>Реагирование на выбор пользователя

Чтобы реагировать на выбор, сделанный пользователем, приложение должно переопределить метод `OnListItemClick`. Следующий пример иллюстрирует один из возможных вариантов.

```csharp
public override void OnListItemClick(ListView l, View v, int index, long id)
{
    // We can display everything in place with fragments.
    // Have the list highlight this item and show the data.
    ListView.SetItemChecked(index, true);

    // Check what fragment is shown, replace if needed.
    var details = FragmentManager.FindFragmentById<DetailsFragment>(Resource.Id.details);
    if (details == null || details.ShownIndex != index)
    {
        // Make new fragment to show this selection.
        details = DetailsFragment.NewInstance(index);

        // Execute a transaction, replacing any existing
        // fragment with this one inside the frame.
        var ft = FragmentManager.BeginTransaction();
        ft.Replace(Resource.Id.details, details);
        ft.SetTransition(FragmentTransit.FragmentFade);
        ft.Commit();
    }
}
```

В приведенном выше коде, когда пользователь выбирает элемент в `ListFragment`, в действии размещения появляется новый фрагмент, в котором отображаются дополнительные сведения о выбранном элементе.

## <a name="dialogfragment"></a>DialogFragment

*DialogFragment* — это фрагмент, который используется для отображения объекта диалога внутри фрагмента, который размещается поверх окна действия. Он заменяет собой API-интерфейсы управляемых диалогов (начиная с Android 3.0). На следующем снимке экрана приведен пример `DialogFragment`:

[![Снимок экрана, где с помощью DialogFragment отображается поле для добавления нового автомобиля](specialized-fragment-classes-images/dialog-fragment-example.png)](specialized-fragment-classes-images/dialog-fragment-example.png#lightbox)

`DialogFragment` гарантирует поддержание согласованности состояний фрагмента и диалога. Все процессы взаимодействия и управления для объекта диалога должны осуществляться через API `DialogFragment`, а не прямыми вызовами этого объекта диалога. API `DialogFragment` предоставляет каждому экземпляру метод `Show()`, который используется для отображения фрагмента. Есть два способа избавиться от фрагмента.

- Вызовите `DialogFragment.Dismiss()` для экземпляра `DialogFragment`. 

- Отобразите другой подкласс `DialogFragment`.

Чтобы создать `DialogFragment`, класс наследует от `Android.App.DialogFragment,` и переопределяет один из следующих двух методов:

- **OnCreateView** &ndash; позволяет создать и вернуть представление;

- **OnCreateDialog** &ndash; позволяет создать пользовательское диалоговое окно. Обычно он используется для отображения *AlertDialog*. При переопределении этого метода нет необходимости переопределять `OnCreateView`.

### <a name="a-simple-dialogfragment"></a>Простой DialogFragment

На следующем снимке экрана показан простой пример `DialogFragment` с одним элементом `TextView` и двумя элементами `Button`.

[![Пример DialogFragment с TextView и двумя кнопками](specialized-fragment-classes-images/dialog-fragment-example-2.png)](specialized-fragment-classes-images/dialog-fragment-example-2.png#lightbox)

В `TextView` будет отображаться количество нажатий одной из кнопок в `DialogFragment`. При нажатии другой фрагмент закрывается. Вот код для этого подкласса `DialogFragment`:

```csharp
public class MyDialogFragment : DialogFragment
{
    private int _clickCount;
    public override void OnCreate(Bundle savedInstanceState)
    {
        _clickCount = 0;
    }

    public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState)
        
        var view = inflater.Inflate(Resource.Layout.dialog_fragment_layout, container, false);
        var textView = view.FindViewById<TextView>(Resource.Id.dialog_text_view);
            
        view.FindViewById<Button>(Resource.Id.dialog_button).Click += delegate
            {
                textView.Text = "You clicked the button " + _clickCount++ + " times.";
            };

        // Set up a handler to dismiss this DialogFragment when this button is clicked.
        view.FindViewById<Button>(Resource.Id.dismiss_dialog_button).Click += (sender, args) => Dismiss();
        return view;
        }
    }
}
```

### <a name="displaying-a-fragment"></a>Отображение фрагмента

Как и все фрагменты, `DialogFragment` отображается в контексте `FragmentTransaction`.

Метод `Show()` в `DialogFragment` принимает `FragmentTransaction` и `string` в качестве входных данных. Диалоговое окно добавляется в действие и `FragmentTransaction` фиксируется.

Следующий код демонстрирует один из возможных применений метода `Show()` в действии для отображения `DialogFragment`:

```csharp
public void ShowDialog()
{
    var transaction = FragmentManager.BeginTransaction();
    var dialogFragment = new MyDialogFragment();
    dialogFragment.Show(transaction, "dialog_fragment");
}
```

### <a name="dismissing-a-fragment"></a>Удаление фрагмента

Вызов `Dismiss()` для экземпляра `DialogFragment` приводит к удалению фрагмента из действия и фиксации транзакции.
Для фрагмента будут вызваны стандартные методы жизненного цикла, участвующие в уничтожении фрагмента.

### <a name="alert-dialog"></a>Диалоговое окно оповещения

Вместо `OnCreateView` `DialogFragment` может переопределять `OnCreateDialog`. Это позволяет приложению создавать управляемый фрагментом класс [AlertDialog](xref:Android.App.AlertDialog). Следующий код является примером использования `AlertDialog.Builder` для создания `Dialog`.

```csharp
public class AlertDialogFragment : DialogFragment
{
    public override Dialog OnCreateDialog(Bundle savedInstanceState)
    {
        EventHandler<DialogClickEventArgs> okhandler;
        var builder = new AlertDialog.Builder(Activity)
            .SetMessage("This is my dialog.")
            .SetPositiveButton("Ok", (sender, args) =>
                                         {
                                             // Do something when this button is clicked.
                                         })
            .SetTitle("Custom Dialog");
        return builder.Create();
    }
}
```

## <a name="preferencefragment"></a>PreferenceFragment

Чтобы было удобнее управлять настройками, API фрагментов предоставляет подкласс `PreferenceFragment`. `PreferenceFragment` действует так же, как и [PreferenceActivity](xref:Android.Preferences.PreferenceActivity) &ndash; отображает во фрагменте иерархию параметром для пользователя. Когда пользователь взаимодействует с этими параметрами, они автоматически сохраняются в [SharedPreferences](https://developer.android.com/reference/android/content/SharedPreferences.html).
В приложениях для Android 3.0 и более поздних версий для работы с настройками в приложениях следует использовать `PreferenceFragment`. На рисунке ниже показан пример `PreferenceFragment`.

[![Пример PreferencesFragment со встроенными параметрами, а также настройками диалога и запуска](specialized-fragment-classes-images/preferences-dialog.png)](specialized-fragment-classes-images/preferences-dialog.png#lightbox)

### <a name="create-a-preference-fragment-from-a-resource"></a>Создание фрагмента Preference на основе ресурса

Фрагмент настроек можно создать из XML-файла ресурсов, используя метод [PreferenceFragment.AddPreferencesFromResource](xref:Android.Preferences.PreferenceFragment.AddPreferencesFromResource*). Для вызова этого метода самым логичным этапом жизненного цикла фрагмента является метод `OnCreate`.

Представленный выше класс `PreferenceFragment` был создан путем загрузки ресурса из XML-файла. Используется следующий файл ресурсов:

```xml
<?xml version="1.0" encoding="utf-8"?>

<PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android">

  <PreferenceCategory android:title="Inline Preferences">
    <CheckBoxPreference android:key="checkbox_preference"
                        android:title="Checkbox Preference Title"
                        android:summary="Checkbox Preference Summary" />

  </PreferenceCategory>

  <PreferenceCategory android:title="Dialog Based Preferences">

    <EditTextPreference android:key="edittext_preference"
                        android:title="EditText Preference Title"
                        android:summary="EditText Preference Summary"
                        android:dialogTitle="Edit Text Preferrence Dialog Title" />

  </PreferenceCategory>

  <PreferenceCategory android:title="Launch Preferences">

    <!-- This PreferenceScreen tag serves as a screen break (similar to page break
             in word processing). Like for other preference types, we assign a key
             here so it is able to save and restore its instance state. -->
    <PreferenceScreen android:key="screen_preference"
                      android:title="Title Screen Preferences"
                      android:summary="Summary Screen Preferences">

      <!-- You can place more preferences here that will be shown on the next screen. -->

      <CheckBoxPreference android:key="next_screen_checkbox_preference"
                          android:title="Next Screen Toggle Preference Title"
                          android:summary="Next Screen Toggle Preference Summary" />

    </PreferenceScreen>

    <PreferenceScreen android:title="Intent Preference Title"
                      android:summary="Intent Preference Summary">

      <intent android:action="android.intent.action.VIEW"
              android:data="http://www.android.com" />

    </PreferenceScreen>

  </PreferenceCategory>

</PreferenceScreen>
```

Ниже представлен код для фрагмента настроек:

```csharp
public class PrefFragment : PreferenceFragment
{
    public override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        AddPreferencesFromResource(Resource.Xml.preferences);
    }
}
```

### <a name="querying-activities-to-create-a-preference-fragment"></a>Запрос к действиям для создания фрагмента настроек

Еще один метод для создания `PreferenceFragment` включает запрос к действиям. Каждое действие может использовать атрибут [METADATA\_KEY\_PREFERENCE](xref:Android.Preferences.PreferenceManager.MetadataKeyPreferences) для указания XML-файла ресурсов. В Xamarin.Android это выполняется путем присвоения действию атрибута `MetaDataAttribute` с указанием файла ресурсов для использования. Класс `PreferenceFragment` предоставляет метод [AddPreferenceFromIntent](xref:Android.Preferences.PreferenceFragment.AddPreferencesFromIntent*), который можно использовать для запроса к действию, чтобы найти XML-ресурс и развернуть его иерархию настроек.

Пример этого процесса предоставляется в следующем фрагменте кода, где `AddPreferencesFromIntent` используется для создания `PreferenceFragment`.

```csharp
public class MyPreferenceFragment : PreferenceFragment
{
    public override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        var intent = new Intent(this.Activity, typeof (MyActivityWithPreferences));
        AddPreferencesFromIntent(intent);
    }
}
```

Android использует класс `MyActivityWithPreference`. Этот класс нужно пометить атрибутом `MetaDataAttribute,`, как показано в следующем фрагменте кода:

```csharp
[Activity(Label = "My Activity with Preferences")]
[MetaData(PreferenceManager.MetadataKeyPreferences, Resource = "@xml/preference_from_intent")]
public class MyActivityWithPreferences : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        // This is deliberately blank
    }
}
```

`MetaDataAttribute` объявляет XML-файл ресурсов, который в `PreferenceFragment` будет применен для развертывания иерархии настроек. Если атрибут `MetatDataAttribute` не предоставлен, во время выполнения будет создано исключение. При выполнении этого кода отображается `PreferenceFragment`, как показано на следующем снимке экрана:

[![Снимок экрана с примером приложения, где отображается PreferenceFragment](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png)](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png#lightbox)
