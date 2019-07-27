---
title: Управляющий элемент выбора даты.
description: Выбор календарных дат с помощью Датепиккердиалог и Диалогфрагмент
ms.prod: xamarin
ms.assetid: F2BCD8D4-8957-EA53-C5A8-6BB603ADB47B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/22/2018
ms.openlocfilehash: ef9abbd60fc83622631b916c50f4993c1c848b00
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510256"
---
# <a name="android-date-picker"></a>Средство выбора даты Android

## <a name="overview"></a>Обзор

Существуют случаи, когда пользователь должен ввести данные в приложение Android. Для облегчения этой помощи платформа Android предоставляет [`DatePicker`](xref:Android.Widget.DatePicker) мини-приложение [`DatePickerDialog`](xref:Android.App.DatePickerDialog) и. `DatePicker` Позволяет пользователям выбирать год, месяц и день в согласованном интерфейсе для устройств и приложений. — Это вспомогательный класс, который инкапсулирует `DatePicker` в диалоговом окне. `DatePickerDialog`

Современные приложения Android должны отображать `DatePickerDialog` [`DialogFragment`](xref:Android.App.DialogFragment)в. Это позволит приложению отображать DatePicker как всплывающее диалоговое окно или внедряться в действие. Кроме того `DialogFragment` , будет управлять жизненным циклом и выводом диалогового окна, уменьшая объем кода, который должен быть реализован.

В этом руководство будет показано `DatePickerDialog`, как использовать, заключенный `DialogFragment`в. Пример приложения будет отображать `DatePickerDialog` как модальное диалоговое окно, когда пользователь нажимает кнопку на действии. Если дата задана пользователем, то `TextView` будет обновлена с выбранной датой.

[![Снимок экрана: кнопка выбора даты, за которой следует диалоговое окно выбора даты](date-picker-images/image-01-sml.png)](date-picker-images/image-01.png#lightbox)

## <a name="requirements"></a>Требования

Пример приложения для этого руководством предназначен для Android 4,1 (уровень API
16) или выше, но применяется к Android 3,0 (API уровня 11 или более поздней версии). Можно поддерживать более старые версии Android с добавлением в проект библиотеки поддержки Android версии 4 и некоторыми изменениями кода.

## <a name="using-the-datepicker"></a>Использование DatePicker

Этот пример будет расширяться `DialogFragment`. Подкласс будет размещать и отображать `DatePickerDialog`:

![Крупный план диалогового окна выбора даты](date-picker-images/image-02.png)

Когда пользователь выбирает дату и нажимает кнопку **ОК** , `DatePickerDialog` вызывается метод [`IOnDateSetListener.OnDateSet`](xref:Android.App.DatePickerDialog.IOnDateSetListener.OnDateSet*).
Этот интерфейс реализуется размещением `DialogFragment`. Если пользователь нажмет кнопку **Отмена** , то фрагмент и диалоговое окно будут отклоняться.

Существует несколько способов `DialogFragment` вернуть выбранную дату в действие размещения.

1. **Вызов метода или установка свойства** &ndash; Действие может предоставить свойство или метод, специально для задания этого значения.

2. **Вызов события** Может определять событие, которое будет вызываться при `OnDateSet` вызове метода. &ndash; `DialogFragment`

3. С помощью можно вызвать`Action<DateTime>` метод, чтобы отобразить дату в действии. **`Action`** &ndash; `DialogFragment` Действие `Action<DateTime` предоставит при создании экземпляра `DialogFragment`. В этом примере используется третья методика и требуется, `Action<DateTime>` `DialogFragment`чтобы действие предложит.

### <a name="extending-dialogfragment"></a>Расширение Диалогфрагмент

Первым шагом в отображении `DatePickerDialog` является подкласс `DialogFragment` `IOnDateSetListener` , который реализует интерфейс:

```csharp
public class DatePickerFragment : DialogFragment, 
                                  DatePickerDialog.IOnDateSetListener
{
    // TAG can be any string of your choice.
    public static readonly string TAG = "X:" + typeof (DatePickerFragment).Name.ToUpper();
    
    // Initialize this value to prevent NullReferenceExceptions.
    Action<DateTime> _dateSelectedHandler = delegate { };
    
    public static DatePickerFragment NewInstance(Action<DateTime> onDateSelected)
    {
        DatePickerFragment frag = new DatePickerFragment();
        frag._dateSelectedHandler = onDateSelected;
        return frag;
    }
    
    public override Dialog OnCreateDialog(Bundle savedInstanceState)
    {
        DateTime currently = DateTime.Now;
        DatePickerDialog dialog = new DatePickerDialog(Activity, 
                                                       this, 
                                                       currently.Year, 
                                                       currently.Month - 1,
                                                       currently.Day);
        return dialog;
    }
    
    public void OnDateSet(DatePicker view, int year, int monthOfYear, int dayOfMonth)
    {
        // Note: monthOfYear is a value between 0 and 11, not 1 and 12!
        DateTime selectedDate = new DateTime(year, monthOfYear + 1, dayOfMonth);
        Log.Debug(TAG, selectedDate.ToLongDateString());
        _dateSelectedHandler(selectedDate);
    }
}
```

Метод вызывается для создания нового `DatePickerFragment`экземпляра. `NewInstance` Этот метод принимает объект `Action<DateTime>` , который будет вызываться, когда пользователь нажмет кнопку **ОК** в `DatePickerDialog`.

Когда фрагмент будет отображен, Android вызовет метод `OnCreateDialog`. Этот метод создает новый `DatePickerDialog` объект и инициализирует его с текущей датой и объектом обратного вызова (который является текущим экземпляром `DatePickerFragment`).

> [!NOTE]
> Имейте в виду, что значение месяца, когда `IOnDateSetListener.OnDateSet` вызывается, находится в диапазоне от 0 до 11, а не от 1 до 12. День месяца будет в диапазоне от 1 до 31 (в зависимости от выбранного месяца).

### <a name="showing-the-datepickerfragment"></a>Отображение Датепиккерфрагмент

Теперь, когда `DialogFragment` объект реализован, в этом разделе рассматривается использование фрагмента в действии. В примере приложения, прилагаемом к этому руководству, действие будет создавать экземпляр `DialogFragment` `NewInstance` с помощью метода Factory, а затем выводит его `DialogFragment.Show`вызов. В процессе создания экземпляра `DialogFragment`действие передает объект `Action<DateTime>`, который отображает дату в элементе `TextView` , размещенном в действии.

```csharp
[Activity(Label = "@string/app_name", MainLauncher = true, Icon = "@drawable/icon")]
public class MainActivity : Activity
{
    TextView _dateDisplay;
    Button _dateSelectButton;

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);

        _dateDisplay = FindViewById<TextView>(Resource.Id.date_display);
        _dateSelectButton = FindViewById<Button>(Resource.Id.date_select_button);
        _dateSelectButton.Click += DateSelect_OnClick;
    }

    void DateSelect_OnClick(object sender, EventArgs eventArgs)
    {
        DatePickerFragment frag = DatePickerFragment.NewInstance(delegate(DateTime time)
                                                                 {
                                                                     _dateDisplay.Text = time.ToLongDateString();
                                                                 });
        frag.Show(FragmentManager, DatePickerFragment.TAG);
    }
}
```

## <a name="summary"></a>Сводка

В этом примере обсуждалось отображение `DatePicker` мини-приложения в виде всплывающего модального диалогового окна в рамках действия Android. В нем предоставлен пример реализации диалогфрагмент и обсуждается `IOnDateSetListener` интерфейс. В этом примере также показано, как Диалогфрагмент может взаимодействовать с действием узла для отображения выбранной даты.

## <a name="related-links"></a>Связанные ссылки

- [диалогфрагмент](xref:Android.App.DialogFragment)
- [DatePicker](xref:Android.Widget.DatePicker)
- [датепиккердиалог](xref:Android.App.DatePickerDialog)
- [Датепиккердиалог. Иондатесетлистенер](xref:Android.App.DatePickerDialog.IOnDateSetListener)
- [Выберите дату](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/datepicker/select_a_date)
