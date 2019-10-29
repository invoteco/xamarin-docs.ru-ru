---
title: Специализированные классы фрагментов
ms.prod: xamarin
ms.assetid: 7A0AEB2C-EE77-63BF-652A-DA049B691C64
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/08/2018
ms.openlocfilehash: b004fbf121374a2bb3bf5d85f45d8cae293573bf
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027305"
---
# <a name="specialized-fragment-classes"></a>Специализированные классы фрагментов

API фрагментов предоставляет другие подклассы, которые инкапсулируют некоторые из наиболее распространенных функциональных возможностей, имеющихся в приложениях. Эти подклассы:

- **Листфрагмент** &ndash; этот фрагмент используется для вывода списка элементов, привязанных к источнику данных, например к массиву или курсору.

- **Диалогфрагмент** &ndash; этот фрагмент используется как оболочка вокруг диалогового окна. Фрагмент отобразит диалоговое окно над его действием.

- **Преференцефрагмент** &ndash; этот фрагмент используется для отображения объектов предпочтений в виде списков.

## <a name="the-listfragment"></a>Листфрагмент

`ListFragment` очень похожа на концепцию и функциональные возможности `ListActivity`; Это оболочка, в которой размещается `ListView` в фрагменте. На рисунке ниже показана `ListFragment`, работающая на планшете и телефоне:

[![снимков экрана Листфрагмент на планшете и на телефоне](specialized-fragment-classes-images/intro-screenshot-sml.png)](specialized-fragment-classes-images/intro-screenshot.png#lightbox)

### <a name="binding-data-with-the-listadapter"></a>Привязка данных с помощью Листадаптер

Класс `ListFragment` уже предоставляет макет по умолчанию, поэтому нет необходимости переопределять `OnCreateView` для вывода содержимого `ListFragment`. `ListView` привязывается к данным с помощью реализации `ListAdapter`. В следующем примере показано, как это можно сделать с помощью простого массива строк:

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

При задании `ListAdapter`важно использовать свойство `ListFragment.ListAdapter`, а не свойство `ListView.ListAdapter`. Использование `ListView.ListAdapter` приведет к пропуску важного кода инициализации.

### <a name="responding-to-user-selection"></a>Реагирование на выбор пользователя

Чтобы реагировать на выборы пользователей, приложение должно переопределить метод `OnListItemClick`. В следующем примере показана одна такая возможность:

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

В приведенном выше коде, когда пользователь выбирает элемент в `ListFragment`, в действии размещения отображается новый фрагмент, в котором отображаются дополнительные сведения о выбранном элементе.

## <a name="dialogfragment"></a>диалогфрагмент

*Диалогфрагмент* — это фрагмент, который используется для вывода объекта диалогового окна внутри фрагмента, который будет перемещаться поверх окна действия. Он предназначен для замены управляемых диалоговых окон API (начиная с Android 3,0). На следующем снимке экрана показан пример `DialogFragment`.

[Снимок экрана Диалогфрагмент, отображающий Добавление нового ТС EditBox![](specialized-fragment-classes-images/dialog-fragment-example.png)](specialized-fragment-classes-images/dialog-fragment-example.png#lightbox)

`DialogFragment` гарантирует, что состояние между фрагментом и диалоговым окном остается согласованным. Все взаимодействия и управление объектом диалогового окна должно осуществляться через API `DialogFragment`, а не с прямыми вызовами для объекта диалогового окна. `DialogFragment` API предоставляет каждый экземпляр с помощью метода `Show()`, который используется для вывода фрагмента. Существует два способа удалить фрагмент:

- Вызовите `DialogFragment.Dismiss()` в экземпляре `DialogFragment`. 

- Отображение другого `DialogFragment`.

Чтобы создать `DialogFragment`, класс наследует от `Android.App.DialogFragment,`, а затем переопределяет один из следующих двух методов:

- **Онкреатевиев** &ndash; создает и возвращает представление.

- **Онкреатедиалог** &ndash; создает пользовательское диалоговое окно. Обычно он используется для отображения *алертдиалог*. При переопределении этого метода нет необходимости переопределять `OnCreateView`.

### <a name="a-simple-dialogfragment"></a>Простой Диалогфрагмент

На следующем снимке экрана показан простой `DialogFragment` с `TextView` и двумя `Button`s:

[![пример Диалогфрагмент с TextView и двумя кнопками](specialized-fragment-classes-images/dialog-fragment-example-2.png)](specialized-fragment-classes-images/dialog-fragment-example-2.png#lightbox)

В `TextView` будет показано, сколько раз пользователь щелкнул одну кнопку в `DialogFragment`, при нажатии другой кнопки будет закрыт фрагмент. Код для `DialogFragment`:

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

Метод `Show()` в `DialogFragment` принимает в качестве входных данных `FragmentTransaction` и `string`. Диалоговое окно будет добавлено в действие и `FragmentTransaction` зафиксировано.

Следующий код демонстрирует один из возможных способов, которым действие может использовать метод `Show()` для отображения `DialogFragment`:

```csharp
public void ShowDialog()
{
    var transaction = FragmentManager.BeginTransaction();
    var dialogFragment = new MyDialogFragment();
    dialogFragment.Show(transaction, "dialog_fragment");
}
```

### <a name="dismissing-a-fragment"></a>Закрытие фрагмента

Вызов `Dismiss()` на экземпляре `DialogFragment` приводит к удалению фрагмента из действия и фиксирует эту транзакцию.
Будут вызваны стандартные методы жизненного цикла фрагментов, участвующие в уничтожении фрагмента.

### <a name="alert-dialog"></a>Диалоговое окно предупреждения

Вместо переопределения `OnCreateView``DialogFragment` может переопределять `OnCreateDialog`. Это позволяет приложению создавать [алертдиалог](xref:Android.App.AlertDialog) , управление которыми осуществляется с помощью фрагмента. Следующий код является примером, в котором для создания `Dialog`используется `AlertDialog.Builder`:

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

## <a name="preferencefragment"></a>преференцефрагмент

Чтобы упростить управление параметрами, API фрагментов предоставляет подкласс `PreferenceFragment`. `PreferenceFragment` похожа на [преференцеактивити](xref:Android.Preferences.PreferenceActivity) &ndash; он покажет иерархию предпочтений пользователю в фрагменте. При взаимодействии пользователя с предпочтениями они будут автоматически сохранены в [SharedPreferences](https://developer.android.com/reference/android/content/SharedPreferences.html).
В приложениях Android 3,0 или более поздней версии используйте `PreferenceFragment`, чтобы работать с предпочтениями в приложениях. На следующем рисунке показан пример `PreferenceFragment`.

[![пример Преференцесфрагмент с помощью встроенных параметров, диалогового окна и настроек запуска](specialized-fragment-classes-images/preferences-dialog.png)](specialized-fragment-classes-images/preferences-dialog.png#lightbox)

### <a name="create-a-preference-fragment-from-a-resource"></a>Создание фрагмента предпочтения на основе ресурса

Фрагмент предпочтений может быть сведен из XML-файла ресурсов с помощью метода [преференцефрагмент. аддпреференцесфромресаурце](xref:Android.Preferences.PreferenceFragment.AddPreferencesFromResource*) . Логическое место вызова этого метода в жизненном цикле фрагмента было бы в методе `OnCreate`.

`PreferenceFragment`, представленный выше, был создан путем загрузки ресурса из XML. Файл ресурсов:

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

Код для фрагмента предпочтений выглядит следующим образом:

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

### <a name="querying-activities-to-create-a-preference-fragment"></a>Запрос действий для создания фрагмента предпочтений

Другой способ создания `PreferenceFragment` включает в себя запросы действий. Каждое действие может использовать [метаданные\_ключом\_атрибут предпочтения](xref:Android.Preferences.PreferenceManager.MetadataKeyPreferences) , указывающий на файл ресурсов XML. В Xamarin. Android это делается путем украшения действия с `MetaDataAttribute`, а затем указания файла ресурсов для использования. Класс `PreferenceFragment` предоставляет метод [аддпреференцефроминтент](xref:Android.Preferences.PreferenceFragment.AddPreferencesFromIntent*) , который можно использовать для запроса действия, чтобы найти этот XML-ресурс и расувеличить для него иерархию предпочтений.

Пример этого процесса приведен в следующем фрагменте кода, который использует `AddPreferencesFromIntent` для создания `PreferenceFragment`:

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

В Android будет рассмотрен класс `MyActivityWithPreference`. Класс должен быть оформлен `MetaDataAttribute,`, как показано в следующем фрагменте кода:

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

`MetaDataAttribute` объявляет XML-файл ресурсов, который `PreferenceFragment` будет использовать для расширения иерархии предпочтений. Если `MetatDataAttribute` не предоставлен, то во время выполнения будет создано исключение. При выполнении этого кода `PreferenceFragment` отображается как на следующем снимке экрана:

[![снимок экрана примера приложения с Преференцефрагмент](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png)](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png#lightbox)
