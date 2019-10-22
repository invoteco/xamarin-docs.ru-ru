---
title: DatePicker для Xamarin. Forms
description: DatePicker — это представление Xamarin. Forms, которое позволяет пользователю выбрать дату. В этой статье объясняется, как использовать DatePicker в приложении Xamarin. Forms.
ms.prod: xamarin
ms.assetid: 68E8EF8A-42E7-4939-8ABE-64D060E609D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/04/2018
ms.openlocfilehash: 15d4159d89a463c0335d9c91b24b55151c91de8c
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72695918"
---
# <a name="xamarinforms-datepicker"></a>DatePicker для Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker)

_Представление Xamarin. Forms, позволяющее пользователю выбрать дату._

[@No__t_1](xref:Xamarin.Forms.DatePicker) Xamarin. Forms вызывает элемент управления "Выбор даты" платформы и позволяет пользователю выбрать дату. `DatePicker` определяет восемь свойств:

- [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) типа [`DateTime`](xref:System.DateTime), который по умолчанию равен первому дню года 1900.
- [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) типа `DateTime`, который по умолчанию равен последнему дню года 2100.
- [`Date`](xref:Xamarin.Forms.DatePicker.Date) типа `DateTime`, выбранная дата по умолчанию равна значению [`DateTime.Today`](xref:System.DateTime.Today).
- [`Format`](xref:Xamarin.Forms.DatePicker.Format) типа `string`, [Стандартная](/dotnet/standard/base-types/standard-date-and-time-format-strings/) или [Настраиваемая](/dotnet/standard/base-types/custom-date-and-time-format-strings/) строка форматирования .NET, которая по умолчанию имеет значение D, шаблон длинной даты.
- [`TextColor`](xref:Xamarin.Forms.DatePicker.TextColor) типа [`Color`](xref:Xamarin.Forms.Color)— цвет, используемый для показа выбранной даты, по умолчанию — [`Color.Default`](xref:Xamarin.Forms.Color.Default).
- [`FontAttributes`](xref:Xamarin.Forms.DatePicker.FontAttributes) типа [`FontAttributes`](xref:Xamarin.Forms.FontAttributes), значение по умолчанию [`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None).
- [`FontFamily`](xref:Xamarin.Forms.DatePicker.FontFamily) типа `string`, значение по умолчанию `null`.
- [`FontSize`](xref:Xamarin.Forms.DatePicker.FontSize) типа `double`, значение по умолчанию — 1,0.
- `CharacterSpacing` типа `double` — интервал между символами `DatePicker`ого текста.

@No__t_0 запускает событие [`DateSelected`](xref:Xamarin.Forms.DatePicker.DateSelected) , когда пользователь выбирает дату.

> [!WARNING]
> При установке `MinimumDate` и `MaximumDate` убедитесь, что `MinimumDate` всегда меньше или равно `MaximumDate`. В противном случае `DatePicker` вызовет исключение.

На внутреннем уровне `DatePicker` гарантирует, что `Date` находится между `MinimumDate` и `MaximumDate` включительно. Если `MinimumDate` или `MaximumDate` заданы так, что `Date` не между ними, `DatePicker` будет изменять значение `Date`.

Все восемь свойств поддерживаются [`BindableProperty`ными](xref:Xamarin.Forms.BindableProperty) объектами. Это означает, что их можно использовать в качестве стиля, а свойства могут быть целями привязок данных. Свойство `Date` имеет режим привязки по умолчанию [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay), то есть он может быть целевым объектом привязки данных в приложении, использующем архитектуру [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) .

## <a name="initializing-the-datetime-properties"></a>Инициализация свойств DateTime

В коде можно инициализировать свойства `MinimumDate`, `MaximumDate` и `Date` для значений типа `DateTime`.

```csharp
DatePicker datePicker = new DatePicker
{
    MinimumDate = new DateTime(2018, 1, 1),
    MaximumDate = new DateTime(2018, 12, 31),
    Date = new DateTime(2018, 6, 21)
};
```

Если значение `DateTime` указано в XAML, средство синтаксического анализа XAML использует метод `DateTime.Parse` с аргументом `CultureInfo.InvariantCulture` для преобразования строки в значение `DateTime`. Даты должны быть указаны в точном формате: двузначное число месяцев, две цифры и годы, состоящие из четырех цифр, разделенных косыми чертами:

```xaml
<DatePicker MinimumDate="01/01/2018"
            MaximumDate="12/31/2018"
            Date="06/21/2018" />
```

Если для свойства `BindingContext` `DatePicker` задан экземпляр ViewModel, содержащий свойства типа `DateTime` с именем `MinDate`, `MaxDate` и `SelectedDate` (например), можно создать экземпляр `DatePicker` следующим образом. :

```xaml
<DatePicker MinimumDate="{Binding MinDate}"
            MaximumDate="{Binding MaxDate}"
            Date="{Binding SelectedDate}" />
```

В этом примере все три свойства инициализируются с соответствующими свойствами в ViewModel. Поскольку свойство `Date` имеет режим привязки `TwoWay`, любая новая дата, выбираемая пользователем, автоматически отражается в ViewModel.

Если `DatePicker` не содержит привязки к свойству `Date`, приложение должно присоединить обработчик к событию `DateSelected`, чтобы получать уведомления о том, что пользователь выбирает новую дату.

Дополнительные сведения о настройке свойств шрифта см. в разделе [шрифты](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="datepicker-and-layout"></a>DatePicker и макет

Можно использовать неограниченный горизонтальный макет, например `Center`, `Start` или `End` с `DatePicker`:

```xaml
<DatePicker ···
            HorizontalOptions="Center"
            ··· />
```

Однако это не рекомендуется. В зависимости от значения свойства `Format` для выбранных дат может потребоваться другая ширина дисплея. Например, строка формата "D" приводит к тому, что `DateTime` отображает даты в длинном формате, а "Среда, 12 сентября, 2018" требует больше ширины экрана, чем "Пятница, Май 4, 2018". В зависимости от платформы это различие может привести к тому, что `DateTime` представление изменит ширину в макете или для усечения изображения.

> [!TIP]
> Лучше использовать параметр `HorizontalOptions` по умолчанию `Fill` с `DatePicker`, а не использовать ширину `Auto` при помещении `DatePicker` в ячейку `Grid`.

## <a name="datepicker-in-an-application"></a>DatePicker в приложении

Образец [**дайсбетвиндатес**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker) содержит два представления `DatePicker` на странице. Их можно использовать для выбора двух дат, и программа вычисляет количество дней между этими датами. Программа не изменяет параметры `MinimumDate` и `MaximumDate` свойства, поэтому две даты должны находиться в диапазоне от 1900 до 2100.

Вот файл XAML:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DaysBetweenDates"
             x:Class="DaysBetweenDates.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <StackLayout Margin="10">
        <Label Text="Days Between Dates"
               Style="{DynamicResource TitleStyle}"
               Margin="0, 20"
               HorizontalTextAlignment="Center" />

        <Label Text="Start Date:" />

        <DatePicker x:Name="startDatePicker"
                    Format="D"
                    Margin="30, 0, 0, 30"
                    DateSelected="OnDateSelected" />

        <Label Text="End Date:" />

        <DatePicker x:Name="endDatePicker"
                    MinimumDate="{Binding Source={x:Reference startDatePicker},
                                          Path=Date}"
                    Format="D"
                    Margin="30, 0, 0, 30"
                    DateSelected="OnDateSelected" />

        <StackLayout Orientation="Horizontal"
                     Margin="0, 0, 0, 30">
            <Label Text="Include both days in total: "
                   VerticalOptions="Center" />
            <Switch x:Name="includeSwitch"
                    Toggled="OnSwitchToggled" />
        </StackLayout>

        <Label x:Name="resultLabel"
               FontAttributes="Bold"
               HorizontalTextAlignment="Center" />

    </StackLayout>
</ContentPage>
```

Каждому `DatePicker`у присваивается `Format` свойство "D" для длинного формата даты. Обратите внимание, что объект `endDatePicker` имеет привязку, которая нацелена на свойство `MinimumDate`. Источником привязки является выбранное свойство `Date` объекта `startDatePicker`. Это гарантирует, что Дата окончания всегда будет позже или равна дате начала. В дополнение к двум `DatePicker` объектам `Switch` помечается как "включить оба дня в сумму".

Два представления `DatePicker` имеют обработчики, прикрепленные к событию `DateSelected`, а `Switch` имеет обработчик, присоединенный к его событию `Toggled`. Эти обработчики событий находятся в файле кода программной части и активируют новое вычисление дней между двумя датами:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }

    void OnDateSelected(object sender, DateChangedEventArgs args)
    {
        Recalculate();
    }

    void OnSwitchToggled(object sender, ToggledEventArgs args)
    {
        Recalculate();
    }

    void Recalculate()
    {
        TimeSpan timeSpan = endDatePicker.Date - startDatePicker.Date +
            (includeSwitch.IsToggled ? TimeSpan.FromDays(1) : TimeSpan.Zero);

        resultLabel.Text = String.Format("{0} day{1} between dates",
                                            timeSpan.Days, timeSpan.Days == 1 ? "" : "s");
    }
}
```

При первом запуске образца `DatePicker` представления инициализируются до сегодняшней даты. На следующем снимке экрана показана программа, выполняемая в iOS, Android и универсальная платформа Windows:

[![Начальные дни между датами](datepicker-images/DaysBetweenDatesStart.png "Начальные дни между датами")](datepicker-images/DaysBetweenDatesStart-Large.png#lightbox "Начальные дни между датами")

При нажатии любой из `DatePicker` отображается функция выбора даты платформы. Эти платформы реализуют этот элемент выбора даты различными способами, но каждый подход знаком пользователям этой платформы:

[![Число дней между датами SELECT](datepicker-images/DaysBetweenDatesSelect.png "Число дней между датами SELECT")](datepicker-images/DaysBetweenDatesSelect-Large.png#lightbox "Число дней между датами SELECT")

> [!TIP]
> В Android диалоговое окно `DatePicker` можно настроить, переопределив метод `CreateDatePickerDialog` в настраиваемом модуле подготовки отчетов. Это позволяет, например, добавить дополнительные кнопки в диалоговое окно.

После выбора двух дат в приложении отображается число дней между этими датами:

[![Результат в днях между датами](datepicker-images/DaysBetweenDatesResult.png "Результат в днях между датами")](datepicker-images/DaysBetweenDatesResult-Large.png#lightbox "Результат в днях между датами")

## <a name="related-links"></a>Связанные ссылки

- [Пример Дайсбетвиндатес](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker)
- [API DatePicker](xref:Xamarin.Forms.DatePicker)
