---
title: TimePicker Xamarin. Forms
description: TimePicker — это представление Xamarin. Forms, которое позволяет пользователю выбрать время. В этой статье объясняется, как использовать TimePicker в приложении Xamarin. Forms.
ms.prod: xamarin
ms.assetid: 2E99FB23-B82D-4EB4-AFB3-5002E736E7B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/16/2018
ms.openlocfilehash: aae0791199b0e3042a3c619fcb11e7b877f52012
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72695923"
---
# <a name="xamarinforms-timepicker"></a>TimePicker Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker)

_Представление Xamarin. Forms, которое позволяет пользователю выбрать время._

[@No__t_1](xref:Xamarin.Forms.TimePicker) Xamarin. Forms вызывает элемент управления средства выбора времени платформы и позволяет пользователю выбрать время. `TimePicker` определяет следующие свойства:

- [`Time`](xref:Xamarin.Forms.TimePicker.Time) типа `TimeSpan`, выбранное время по умолчанию равно `TimeSpan` 0. Тип `TimeSpan` указывает длительность времени с полуночи.
- [`Format`](xref:Xamarin.Forms.TimePicker.Format) типа `string`, [стандартной](/dotnet/standard/base-types/standard-date-and-time-format-strings/) или [пользовательской](/dotnet/standard/base-types/custom-date-and-time-format-strings/) строки форматирования .NET, которая по умолчанию имеет значение t, это короткий шаблон времени.
- [`TextColor`](xref:Xamarin.Forms.TimePicker.TextColor) типа [`Color`](xref:Xamarin.Forms.Color), который используется для показа выбранного времени, по умолчанию [`Color.Default`](xref:Xamarin.Forms.Color.Default).
- [`FontAttributes`](xref:Xamarin.Forms.TimePicker.FontAttributes) типа [`FontAttributes`](xref:Xamarin.Forms.FontAttributes), значение по умолчанию [`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None).
- [`FontFamily`](xref:Xamarin.Forms.TimePicker.FontFamily) типа `string`, значение по умолчанию `null`.
- [`FontSize`](xref:Xamarin.Forms.TimePicker.FontSize) типа `double`, значение по умолчанию — 1,0.
- `CharacterSpacing` типа `double` — интервал между символами `TimePicker`ого текста.

Все эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами. Это означает, что их можно использовать в качестве стиля, а свойства могут быть целями привязок данных. Свойство [`Time`](xref:Xamarin.Forms.TimePicker.Time) имеет режим привязки по умолчанию [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay), то есть он может быть целевым объектом привязки данных в приложении, использующем архитектуру [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) .

[@No__t_1](xref:Xamarin.Forms.TimePicker) не содержит событие для указания нового выбранного значения [`Time`](xref:Xamarin.Forms.TimePicker.Time) . Если необходимо получать уведомления об этом, можно добавить обработчик для события [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) .

## <a name="initializing-the-time-property"></a>Инициализация свойства времени

В коде можно инициализировать свойство [`Time`](xref:Xamarin.Forms.TimePicker.Time) для значения типа `TimeSpan`:

```csharp
TimePicker timePicker = new TimePicker
{
  Time = new TimeSpan(4, 15, 26) // Time set to "04:15:26"
};
```

Если свойство [`Time`](xref:Xamarin.Forms.TimePicker.Time) указано в XAML, значение преобразуется в `TimeSpan` и проверяется, чтобы убедиться, что число миллисекунд больше или равно 0, а число часов меньше 24. Компоненты времени должны быть разделены двоеточиями:

```xaml
<TimePicker Time="4:15:26" />
```

Если для свойства [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) [`TimePicker`](xref:Xamarin.Forms.TimePicker) задан экземпляр ViewModel, содержащий свойство типа `TimeSpan` с именем `SelectedTime` (например,), можно создать экземпляр `TimePicker` следующим образом:

```xaml
<TimePicker Time="{Binding SelectedTime}" />
```

В этом примере свойство [`Time`](xref:Xamarin.Forms.TimePicker.Time) инициализируется свойством `SelectedTime` в ViewModel. Поскольку свойство `Time` имеет режим привязки [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay), любое новое время, выбранное пользователем, автоматически распространяется на ViewModel.

Если [`TimePicker`](xref:Xamarin.Forms.TimePicker) не содержит привязки к свойству [`Time`](xref:Xamarin.Forms.TimePicker.Time) , приложение должно присоединить обработчик к событию [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) , чтобы получать уведомления о том, что пользователь выбрал новое время.

Дополнительные сведения о настройке свойств шрифта см. в разделе [шрифты](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="timepicker-and-layout"></a>TimePicker и макет

Можно использовать неограниченный горизонтальный макет, например `Center`, `Start` или `End` с [`TimePicker`](xref:Xamarin.Forms.TimePicker):

```xaml
<TimePicker ···
            HorizontalOptions="Center"
            ··· />
```

Однако это не рекомендуется. В зависимости от значения свойства [`Format`](xref:Xamarin.Forms.TimePicker.Format) для выбранного времени может потребоваться другая ширина дисплея. Например, строка формата "T" заставляет представление [`TimePicker`](xref:Xamarin.Forms.TimePicker) отображать время в длинном формате, а "4:15:26 AM" требует больше ширины отображения, чем короткий формат времени ("T") "4:15 AM". В зависимости от платформы это различие может привести к тому, что `TimePicker` представление изменит ширину в макете или для усечения изображения.

> [!TIP]
> Лучше использовать параметр `HorizontalOptions` по умолчанию `Fill` с [`TimePicker`](xref:Xamarin.Forms.TimePicker), а не использовать ширину `Auto` при помещении `TimePicker` в ячейку `Grid`.

## <a name="timepicker-in-an-application"></a>TimePicker в приложении

Образец [**сеттимер**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker) включает [`TimePicker`](xref:Xamarin.Forms.TimePicker), [`Entry`](xref:Xamarin.Forms.Entry)и представления [`Switch`](xref:Xamarin.Forms.Switch) на странице. @No__t_0 можно использовать для выбора времени, а в это время появляется диалоговое окно предупреждения, напоминающее пользователю текст в `Entry`, при условии, что `Switch` включен. Вот файл XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:SetTimer"
             x:Class="SetTimer.MainPage">
    <StackLayout>
        ...
        <Entry x:Name="_entry"
               Placeholder="Enter event to be reminded of" />
        <Label Text="Select the time below to be reminded at." />
        <TimePicker x:Name="_timePicker"
                    Time="11:00:00"
                    Format="T"
                    PropertyChanged="OnTimePickerPropertyChanged" />
        <StackLayout Orientation="Horizontal">
            <Label Text="Enable timer:" />
            <Switch x:Name="_switch"
                    HorizontalOptions="EndAndExpand"
                    Toggled="OnSwitchToggled" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

[@No__t_1](xref:Xamarin.Forms.Entry) позволяет ввести текст напоминания, который будет отображаться при наступлении выбранного времени. [@No__t_1](xref:Xamarin.Forms.TimePicker) присваивается свойство [`Format`](xref:Xamarin.Forms.TimePicker.Format) "t" для длинного формата времени. Он имеет обработчик событий, присоединенный к событию [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) , а [`Switch`](xref:Xamarin.Forms.Switch) имеет обработчик, присоединенный к событию [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) . Эти обработчики событий находятся в файле кода программной части и вызывают метод `SetTriggerTime`:

```csharp
public partial class MainPage : ContentPage
{
    DateTime _triggerTime;

    public MainPage()
    {
        InitializeComponent();

        Device.StartTimer(TimeSpan.FromSeconds(1), OnTimerTick);
    }

    bool OnTimerTick()
    {
        if (_switch.IsToggled && DateTime.Now >= _triggerTime)
        {
            _switch.IsToggled = false;
            DisplayAlert("Timer Alert", "The '" + _entry.Text + "' timer has elapsed", "OK");
        }
        return true;
    }

    void OnTimePickerPropertyChanged(object sender, PropertyChangedEventArgs args)
    {
        if (args.PropertyName == "Time")
        {
            SetTriggerTime();
        }
    }

    void OnSwitchToggled(object sender, ToggledEventArgs args)
    {
        SetTriggerTime();
    }

    void SetTriggerTime()
    {
        if (_switch.IsToggled)
        {
            _triggerTime = DateTime.Today + _timePicker.Time;
            if (_triggerTime < DateTime.Now)
            {
                _triggerTime += TimeSpan.FromDays(1);
            }
        }
    }
}
```

Метод `SetTriggerTime` вычисляет время таймера на основе значения свойства `DateTime.Today` и значения `TimeSpan`, возвращенного из [`TimePicker`](xref:Xamarin.Forms.TimePicker). Это необходимо потому, что свойство `DateTime.Today` возвращает `DateTime`, указывающее текущую дату, но с временем полуночи. Если время таймера уже прошло сегодня, предполагается, что он будет завтра.

Таймер тикает каждую секунду, выполняя метод `OnTimerTick`, который проверяет, включен ли [`Switch`](xref:Xamarin.Forms.Switch) , и является ли текущее время большим или равным времени таймера. Когда происходит время таймера, метод [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) предоставляет пользователю диалоговое окно с предупреждением в виде напоминания.

При первом запуске образца [`TimePicker`](xref:Xamarin.Forms.TimePicker) представление инициализируется значением 11:00. Коснувшись `TimePicker` вызывает средство выбора времени платформы. Платформы реализуют средство выбора времени различными способами, но каждый подход знаком пользователям этой платформы:

[![Выбрать время](timepicker-images/timepicker-open.png "Выбрать время")](timepicker-images/timepicker-open-large.png#lightbox "Выбрать время")

> [!TIP]
> В Android диалоговое окно [`TimePicker`](xref:Xamarin.Forms.TimePicker) можно настроить, переопределив метод `CreateTimePickerDialog` в настраиваемом модуле подготовки отчетов. Это позволяет, например, добавить дополнительные кнопки в диалоговое окно.

После выбора времени в [`TimePicker`](xref:Xamarin.Forms.TimePicker)отображается выбранное время.

[![Выбранное время](timepicker-images/timepicker-selected.png "Выбранное время")](timepicker-images/timepicker-selected-large.png#lightbox "Выбранное время")

При условии, что [`Switch`](xref:Xamarin.Forms.Switch) переключается на положение ON, в приложении отображается диалоговое окно предупреждения с напоминанием о пользователе текста в [`Entry`](xref:Xamarin.Forms.Entry) , когда происходит выбранное время:

[![Всплывающее окно таймера](timepicker-images/timer-test.png "Всплывающее окно таймера")](timepicker-images/timer-test-large.png#lightbox "Всплывающее окно таймера")

Как только откроется диалоговое окно оповещения, [`Switch`](xref:Xamarin.Forms.Switch) переключается в положение Выкл.

## <a name="related-links"></a>Связанные ссылки

- [Пример Сеттимер](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker)
- [API TimePicker](xref:Xamarin.Forms.TimePicker)
