---
title: Сводка Глава 15. Интерактивный интерфейс
description: 'Создание мобильных приложений с помощью Xamarin.Forms: Сводка Глава 15. Интерактивный интерфейс'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F54E86F4-1CDA-474E-9B09-242060C2C13D
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 5f96d2f4b619bbb10bb58e9b1b5dc7007c1ce888
ms.sourcegitcommit: ccbf914615c0ce6b3f308d930f7a77418aeb4dbc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/11/2020
ms.locfileid: "77131099"
---
# <a name="summary-of-chapter-15-the-interactive-interface"></a>Сводка Глава 15. Интерактивный интерфейс

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)

В этой главе рассматриваются восемь `View` производных, которые позволяют взаимодействовать с пользователем.

## <a name="view-overview"></a>Просмотр общих сведений

Xamarin. Forms содержит 20 экземпляров классов, производных от `View`, но не `Layout`. Шесть из них уже описаны в предыдущих главах:

- `Label`: [ **глава 2. Анатомия приложения**](chapter02.md)
- `BoxView`. [ **глава 3. Прокрутка стека**](chapter03.md)
- `Button`. [ **Глава 6. Нажатие кнопки**](chapter06.md)
- `Image`: [ **глава 13. Точечные рисунки**](chapter13.md)
- `ActivityIndicator`: [ **глава 13. Точечные рисунки**](chapter13.md)
- `ProgressBar`: [ **глава 14. Абсолутелайаут**](chapter14.md)

Восемь представлений в этой главе эффективно разрешить пользователю взаимодействовать с базовых типов данных .NET:

|Тип данных|Представления|
|--- |--- |
|`Double`|[`Slider`](xref:Xamarin.Forms.Slider), [`Stepper`](xref:Xamarin.Forms.Stepper)|
|`Boolean`|[`Switch`](xref:Xamarin.Forms.Switch)|
|`String`|[`Entry`](xref:Xamarin.Forms.Entry), [`Editor`](xref:Xamarin.Forms.Editor) [`SearchBar`](xref:Xamarin.Forms.SearchBar)|
|`DateTime`|[`DatePicker`](xref:Xamarin.Forms.DatePicker), [`TimePicker`](xref:Xamarin.Forms.TimePicker)|

Эти представления можно считать интерактивные визуальные представления базовых типов данных. Эта концепция подробно рассмотрена в следующей главе [**16. Привязка данных**](chapter16.md).

Остальные шесть представления рассматриваются в следующих разделах:

- `WebView`: [ **глава 16. Привязка данных**](chapter16.md)
- `Picker`: [ **глава 19. Представления коллекций**](chapter19.md)
- `ListView`: [ **глава 19. Представления коллекций**](chapter19.md)
- `TableView`: [ **глава 19. Представления коллекций**](chapter19.md)
- `Map`: [ **глава 28. Расположение и карты**](chapter28.md)
- `OpenGLView`: не рассматривается в этой книге (и не поддерживает платформы Windows)

## <a name="slider-and-stepper"></a>Ползунок и несопоставимого

И [`Slider`](xref:Xamarin.Forms.Slider) , и [`Stepper`](xref:Xamarin.Forms.Stepper) позволяют пользователю выбрать числовое значение из диапазона. `Slider` является непрерывным диапазоном, а `Stepper` включает дискретные значения.

### <a name="slider-basics"></a>Основные сведения "ползунок"

[`Slider`](xref:Xamarin.Forms.Slider) является горизонтальной полосой, представляющей диапазон значений от минимума слева до максимального значения справа. Он определяет три открытых свойства:

- [`Value`](xref:Xamarin.Forms.Slider.Value) типа `double`, значение по умолчанию — 0
- [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) типа `double`, значение по умолчанию — 0
- [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) типа `double`, значение по умолчанию — 1

Привязываемые свойства, которые поддерживают эти свойства убедитесь, что они согласованы:

- Для всех трех свойств метод [`coerceValue`](xref:Xamarin.Forms.BindableProperty.CoerceValueDelegate) , указанный для свойства BIND, гарантирует, что `Value` находится между `Minimum` и `Maximum`.
- Метод [`validateValue`](xref:Xamarin.Forms.BindableProperty.ValidateValueDelegate) в `MinimumProperty` возвращает `false`, если для `Minimum` задано значение, большее или равное `Maximum`, и аналогично `MaximumProperty`. Возврат `false` из метода `validateValue` приводит к возникновению `ArgumentException`.

`Slider` запускает событие [`ValueChanged`](xref:Xamarin.Forms.Slider.ValueChanged) с аргументом [`ValueChangedEventArgs`](xref:Xamarin.Forms.ValueChangedEventArgs) при изменении свойства `Value` либо программно, либо при работе пользователя с `Slider`.

В образце [**слидердемо**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SliderDemo) демонстрируется простое использование `Slider`.

### <a name="common-pitfalls"></a>Типичные проблемы

Как в коде, так и в XAML свойства `Minimum` и `Maximum` задаются в указанном порядке. Не забудьте инициализировать эти свойства, чтобы `Maximum` всегда было больше `Minimum`. В противном случае возникнет исключение.

Инициализация свойств `Slider` может привести к изменению свойства `Value` и срабатыванию события `ValueChanged`. Следует убедиться, что обработчик событий `Slider` не имеет доступа к представлениям, которые еще не были созданы во время инициализации страницы.

Событие `ValueChanged` не срабатывает во время инициализации `Slider`, если только свойство `Value` не изменяется. Обработчик `ValueChanged` можно вызвать непосредственно из кода.

### <a name="slider-color-selection"></a>Выбранный цвет ползунка

Программа [**ргбслидерс**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/RgbSliders) содержит три элемента `Slider`, которые позволяют в интерактивном режиме выбирать цвет, УКАЗЫВАЯ значения RGB:

[![Тройной снимок экрана с ползунками R G B](images/ch15fg03-small.png "Ползунки RGB")](images/ch15fg03-large.png#lightbox "Ползунки RGB")

В образце [**текстфаде**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/TextFade) используются два элемента `Slider` для перемещения двух элементов `Label` в `AbsoluteLayout` и появления одного из них в другой.

### <a name="the-stepper-difference"></a>Разница несопоставимого

[`Stepper`](xref:Xamarin.Forms.Stepper) определяет те же свойства и события, что и `Slider` но свойство `Maximum` инициализируется значением 100, а `Stepper` определяет Четвертое свойство:

- [`Increment`](xref:Xamarin.Forms.Stepper.Increment) типа `double`, инициализируется значением 1

Визуально `Stepper` состоит из двух кнопок с надписью **&ndash;** и **+** . Нажатие **&ndash;** сокращает `Value` `Increment` до минимума `Minimum`. Нажатие **+** увеличивает `Value`, `Increment` до максимума `Maximum`.

Это продемонстрировано в примере [**степпердемо**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/StepperDemo) .

## <a name="switch-and-checkbox"></a>Переключатель и флажок

[`Switch`](xref:Xamarin.Forms.Switch) позволяет пользователю указать логическое значение.

### <a name="switch-basics"></a>Основы коммутатора

Визуально `Switch` состоит из переключателя, который может быть выключен и включен. Этот класс определяет одно свойство:

- [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) типа `bool`

`Switch` определяет одно событие:

- [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) , сопровождаемый объектом [`ToggledEventArgs`](xref:Xamarin.Forms.ToggledEventArgs) , возникает при изменении свойства `IsToggled`.

В программе [**свитчдемо**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SwitchDemo) демонстрируется `Switch`.

### <a name="a-traditional-checkbox"></a>Традиционные флажок

Некоторые разработчики могут предпочесть более традиционные `CheckBox` `Switch`. Библиотека [**Xamarin. формсбук. Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) содержит класс `CheckBox`, производный от `ContentView`. `CheckBox` реализуется с помощью файлов [CheckBox. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml) и [CheckBox.XAML.CS](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml.cs) . `CheckBox` определяет три свойства (`Text`, `FontSize`и `IsChecked`) и событие `CheckedChanged`.

Этот `CheckBox`показан в образце [**чеккбоксдемо**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/CheckBoxDemo) .

## <a name="typing-text"></a>Ввода текста

Xamarin.Forms определяет три представления дают пользователям возможность вводить и редактировать текст:

- [`Entry`](xref:Xamarin.Forms.Entry) одной строки текста
- [`Editor`](xref:Xamarin.Forms.Editor) для нескольких строк текста
- [`SearchBar`](xref:Xamarin.Forms.SearchBar) для одной строки текста в целях поиска.

`Entry` и `Editor` являются производными от [`InputView`](xref:Xamarin.Forms.InputView), которые являются производными от `View`. `SearchBar` наследуется непосредственно от `View`.

### <a name="keyboard-and-focus"></a>И фокус клавиатуры

На телефонах и планшетах без физических клавиатур все элементы `Entry`, `Editor`и `SearchBar` вызывают всплывающие окна с помощью виртуальной клавиатуры. Наличие этой клавиатуры на экране связана с фокус ввода. У представления должны быть свойства [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) и [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) , для которых задано значение `true`, чтобы получить фокус ввода.

С фокусом ввода участвуют два метода, одно свойство только для чтения и два события. Все они определяются `VisualElement`:

- Метод [`Focus`](xref:Xamarin.Forms.VisualElement.Focus) пытается установить фокус ввода на элемент и возвращает `true` в случае успеха
- Метод [`Unfocus`](xref:Xamarin.Forms.VisualElement.Unfocus) удаляет фокус ввода из элемента
- Свойство [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused) только для чтения указывает, имеет ли элемент фокус ввода
- Событие [`Focused`](xref:Xamarin.Forms.VisualElement.Focused) указывает, когда элемент получает фокус ввода
- Событие [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused) указывает, когда элемент теряет фокус ввода

### <a name="choosing-the-keyboard"></a>Выбор клавиатуры

Класс [`InputView`](xref:Xamarin.Forms.InputView) , из которого `Entry` и `Editor` являются производными, определяет только одно свойство:

- [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) типа [`Keyboard`](xref:Xamarin.Forms.Keyboard).

Указывает тип клавиатуры, который отображается. Неотъемлемыми оптимизированы для URI или их числа.

Класс `Keyboard` позволяет определить клавиатуру со статическим методом [`Keyboard.Create`](xref:Xamarin.Forms.Keyboard.Create(Xamarin.Forms.KeyboardFlags)) с аргументом типа [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags), перечислением со следующими битовыми флагами:

- `None` значение 0
- для [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) задано значение 1
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) значение 2
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) значение 4
- для [`All`](xref:Xamarin.Forms.KeyboardFlags.All) задано значение \ксфффффффф

При использовании многострочного [`Editor`](xref:Xamarin.Forms.Editor) в случае, если ожидается абзац или больше текста, вызов `Keyboard.Create` является хорошим подходом к выбору клавиатуры. Для однострочного [`Entry`](xref:Xamarin.Forms.Entry)следующие статические свойства `Keyboard`, доступные только для чтения, полезны:

- [`Default`](xref:Xamarin.Forms.Keyboard.Default)
- [`Text`](xref:Xamarin.Forms.Keyboard.Text)
- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat)
- [`Url`](xref:Xamarin.Forms.Keyboard.Url)
- [`Email`](xref:Xamarin.Forms.Keyboard.Email)
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone)
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) для положительных чисел с десятичной запятой или без нее.

[`KeyboardTypeConverter`](xref:Xamarin.Forms.KeyboardTypeConverter) позволяет указывать эти свойства в XAML, как показано в программе [**ентрикэйбоардс**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/EntryKeyboards) .

### <a name="entry-properties-and-events"></a>Запись свойства и события

Однострочный [`Entry`](xref:Xamarin.Forms.Entry) определяет следующие свойства:

- [`Text`](xref:Xamarin.Forms.InputView.Text) типа `string`, текст, отображаемый в `Entry`
- [`TextColor`](xref:Xamarin.Forms.InputView.TextColor) типа `Color`
- [`FontFamily`](xref:Xamarin.Forms.Entry.FontFamily) типа `string`
- [`FontSize`](xref:Xamarin.Forms.Entry.FontSize) типа `double`
- [`FontAttributes`](xref:Xamarin.Forms.Entry.FontAttributes) типа `FontAttributes`
- [`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword) типа `bool`, что приводит к маскированию символов
- [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) типа `string`для цветного текста лезут, который отображается в `Entry` перед тем, как что-либо введено
- [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor) типа `Color`

`Entry` также определяет два события:

- [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) с [`TextChangedEventArgs`ным](xref:Xamarin.Forms.TextChangedEventArgs) объектом, срабатывает при каждом изменении свойства `Text`
- [`Completed`](xref:Xamarin.Forms.Entry.Completed), срабатывает после завершения работы пользователя и закрытия клавиатуры. Пользователь указывает завершения в виде платформы

В образце [**куадратицекуатионс**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/QuadaticEquations) показаны эти два события.

### <a name="the-editor-difference"></a>Разница редактора

Многострочный [`Editor`](xref:Xamarin.Forms.Editor) определяет те же свойства `Text` и `Font`, что и `Entry`, но не другие свойства. `Editor` также определяет те же два свойства, что и `Entry`.

[**Жустнотес**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/JustNotes) — это программа для создания заметок в свободной форме, которая сохраняет и восстанавливает содержимое `Editor`.

### <a name="the-searchbar"></a>SearchBar

[`SearchBar`](xref:Xamarin.Forms.SearchBar) не является производным от `InputView`, поэтому он не имеет свойства `Keyboard`. Но у него есть все свойства `Text`, `Font`и `Placeholder`, которые `Entry` определяет. Кроме того, `SearchBar` определяет три дополнительных свойства:

- [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor) типа `Color`
- [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand) типа [`ICommand`](xref:System.Windows.Input.ICommand) для использования с ПРИВЯЗКАми данных и MVVM
- [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) типа `Object`для использования с `SearchCommand`

Специфические для платформы отменить кнопки удаляет текст. `SearchBar` также содержит кнопку поиска для конкретной платформы. Нажатие любой из этих кнопок вызывает одно из двух событий, которые `SearchBar` определяет:

- [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) , сопровождаемый объектом [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs)
- [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)

В примере [**сеарчбардемо**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SearchBarDemo) демонстрируется `SearchBar`.

## <a name="date-and-time-selection"></a>Выбор даты и времени

Представления [`DatePicker`](xref:Xamarin.Forms.DatePicker) и [`TimePicker`](xref:Xamarin.Forms.TimePicker) реализуют элементы управления для конкретной платформы, позволяющие пользователю указать дату или время.

### <a name="the-datepicker"></a>DatePicker

[`DatePicker`](xref:Xamarin.Forms.DatePicker) определяет четыре свойства:

- [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) типа `DateTime`, инициализированного с 1 января 1900 г.
- [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) типа `DateTime`, инициализированный 31 декабря 2100 г.
- [`Date`](xref:Xamarin.Forms.DatePicker.Date) типа `DateTime`, инициализированный на `DateTime.Today`
- [`Format`](xref:Xamarin.Forms.DatePicker.Format) типа `string`строка форматирования .NET, инициализированная в "d", короткий шаблон даты, в результате чего отображается дата, например "7/20/1969" в США.

Можно задать свойства `DateTime` в XAML, указав свойства в виде элементов свойств и используя неизменяемый формат короткого языка и региональных параметров ("7/20/1969").   

Образец [**дайсбетвиндатес**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/DaysBetweenDates) вычисляет число дней между двумя датами, выбранными пользователем.

### <a name="the-timepicker-or-is-it-a-timespanpicker"></a>Элемента управления TimePicker (или TimeSpanPicker?)

[`TimePicker`](xref:Xamarin.Forms.TimePicker) определяет два свойства и не содержит событий:

- [`Time`](xref:Xamarin.Forms.TimePicker.Time) имеет тип `TimeSpan`, а не `DateTime`, указывающий время, прошедшее с полуночи
- [`Format`](xref:Xamarin.Forms.TimePicker.Format) типа `string`строка форматирования .NET, инициализированная в "t", короткий шаблон времени, в результате чего отображается время, например "1:45 PM" в США.

Программа [**сеттимер**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SetTimer) демонстрирует использование `TimePicker` для задания времени таймера. Программа работает только в том случае, если вы храните на переднем плане.

**Сеттимер** также демонстрирует использование метода [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) `Page` для вывода окна предупреждения.

## <a name="related-links"></a>Связанные ссылки

- [Глава 15 Full Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch15-Apr2016.pdf)
- [Глава 15 примеров](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)
- [Slider](~/xamarin-forms/user-interface/slider.md)
- [Ввод](~/xamarin-forms/user-interface/text/entry.md)
- [Редактор](~/xamarin-forms/user-interface/text/editor.md)
- [DatePicker](~/xamarin-forms/user-interface/datepicker.md)
