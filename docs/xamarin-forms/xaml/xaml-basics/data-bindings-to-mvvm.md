---
title: Часть 5. От привязки данных до MVVM
description: Шаблон MVVM обеспечивает разделение между тремя уровнями программного обеспечения — пользовательским интерфейсом XAML, называемым представлением; базовые данные, называемые моделью; и посредник между представлением и моделью, называемой ViewModel.
ms.prod: xamarin
ms.custom: video
ms.assetid: 48B37D44-4FB1-41B2-9A5E-6D383B041F81
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: 80386780d52f9a28d421d2a83981085956d06ea5
ms.sourcegitcommit: efbc69acf4ea484d8815311b058114379c9db8a2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/08/2019
ms.locfileid: "73842942"
---
# <a name="part-5-from-data-bindings-to-mvvm"></a>Часть 5. От привязки данных до MVVM

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_Шаблон архитектуры Model-View-ViewModel (MVVM) был создан с учетом XAML. Шаблон обеспечивает разделение между тремя уровнями программного обеспечения — пользовательским интерфейсом XAML, называемым представлением; базовые данные, называемые моделью; и посредник между представлением и моделью, называемой ViewModel. Представления и ViewModel часто соединяются с помощью привязок данных, определенных в файле XAML. BindingContext для представления обычно является экземпляром ViewModel._

## <a name="a-simple-viewmodel"></a>Простой ViewModel

В качестве введения в ViewModels Давайте сначала рассмотрим программу без участия пользователя.
Ранее было показано, как определить новое объявление пространства имен XML, чтобы файл XAML можно было ссылаться на классы в других сборках. Вот программа, которая определяет объявление пространства имен XML для пространства имен `System`:

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Программа может использовать `x:Static`, чтобы получить текущие дату и время из статического свойства `DateTime.Now` и установить значение `DateTime` `BindingContext` в `StackLayout`:

```xaml
<StackLayout BindingContext="{x:Static sys:DateTime.Now}" …>
```

`BindingContext` является специальным свойством: при задании `BindingContext` для элемента он наследуется всеми дочерними элементами этого элемента. Это означает, что все дочерние элементы `StackLayout` имеют одинаковый `BindingContext`и могут содержать простые привязки к свойствам этого объекта.

В программе **DateTime с одним снимком** два дочерних элемента содержат привязки к свойствам этого `DateTime` значения, но два других дочерних элемента содержат привязки, для которых кажется, что отсутствует путь привязки. Это означает, что для `StringFormat`используется само значение `DateTime`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             x:Class="XamlSamples.OneShotDateTimePage"
             Title="One-Shot DateTime Page">

    <StackLayout BindingContext="{x:Static sys:DateTime.Now}"
                 HorizontalOptions="Center"
                 VerticalOptions="Center">

        <Label Text="{Binding Year, StringFormat='The year is {0}'}" />
        <Label Text="{Binding StringFormat='The month is {0:MMMM}'}" />
        <Label Text="{Binding Day, StringFormat='The day is {0}'}" />
        <Label Text="{Binding StringFormat='The time is {0:T}'}" />

    </StackLayout>
</ContentPage>
```

Проблема заключается в том, что дата и время задаются один раз при первой сборке страницы и никогда не изменяются.

[![](data-bindings-to-mvvm-images/oneshotdatetime.png "View Displaying Date and Time")](data-bindings-to-mvvm-images/oneshotdatetime-large.png#lightbox "View Displaying Date and Time")

XAML-файл может отображать часы, в которых всегда отображается текущее время, но для облегчения требуется некоторый код. При обдумывании с точки зрения MVVM модель и ViewModel являются классами, написанными полностью в коде. Представление часто представляет собой XAML-файл, который ссылается на свойства, определенные в ViewModel с помощью привязок данных.

Надлежащей моделью является игнорирующих в ViewModel, а правильное значение ViewModel — игнорирующих представления. Однако зачастую программисты, которые представляют типы данных, предоставляемые ViewModel, с типами данных, связанными с определенными пользовательскими интерфейсами. Например, если модель обращается к базе данных, содержащей 8-разрядные символьные строки ASCII, ViewModel пришлось бы преобразовать эти строки в строки Юникода, чтобы обеспечить эксклюзивное использование Юникода в пользовательском интерфейсе.

В простых примерах MVVM (например, показанных здесь) часто не существует модели, и шаблон включает только представление и ViewModel, связанные с привязками данных.

Ниже приведено значение ViewModel для часов с единственным свойством с именем `DateTime`, которое обновляет `DateTime` свойство каждую секунду:

```csharp
using System;
using System.ComponentModel;
using Xamarin.Forms;

namespace XamlSamples
{
    class ClockViewModel : INotifyPropertyChanged
    {
        DateTime dateTime;

        public event PropertyChangedEventHandler PropertyChanged;

        public ClockViewModel()
        {
            this.DateTime = DateTime.Now;

            Device.StartTimer(TimeSpan.FromSeconds(1), () =>
                {
                    this.DateTime = DateTime.Now;
                    return true;
                });
        }

        public DateTime DateTime
        {
            set
            {
                if (dateTime != value)
                {
                    dateTime = value;

                    if (PropertyChanged != null)
                    {
                        PropertyChanged(this, new PropertyChangedEventArgs("DateTime"));
                    }
                }
            }
            get
            {
                return dateTime;
            }
        }
    }
}
```

В целом, ViewModels реализует интерфейс `INotifyPropertyChanged`, что означает, что класс запускает `PropertyChanged` событие при каждом изменении одного из его свойств. Механизм привязки данных в Xamarin. Forms прикрепляет обработчик к этому `PropertyChanged`у событию, поэтому он может получать уведомления при изменении свойства и поддерживать обновление целевого объекта новым значением.

Часы на основе этого ViewModel могут быть простыми:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ClockPage"
             Title="Clock Page">

    <Label Text="{Binding DateTime, StringFormat='{0:T}'}"
           FontSize="Large"
           HorizontalOptions="Center"
           VerticalOptions="Center">
        <Label.BindingContext>
            <local:ClockViewModel />
        </Label.BindingContext>
    </Label>
</ContentPage>
```

Обратите внимание на то, как `ClockViewModel` задается `BindingContext` `Label` с помощью тегов элемента свойства. Кроме того, можно создать экземпляр `ClockViewModel` в коллекции `Resources` и задать его `BindingContext` с помощью расширения разметки `StaticResource`. Или же файл кода программной части может создать экземпляр ViewModel.

Расширение разметки `Binding` для свойства `Text` `Label` форматирует свойство `DateTime`. Вот как выглядит этот экран:

[![](data-bindings-to-mvvm-images/clock.png "View Displaying Date and Time via ViewModel")](data-bindings-to-mvvm-images/clock-large.png#lightbox "View Displaying Date and Time via ViewModel")

Также можно получить доступ к отдельным свойствам `DateTime` свойства ViewModel, разделяя свойства точками.

```xaml
<Label Text="{Binding DateTime.Second, StringFormat='{0}'}" … >
```

## <a name="interactive-mvvm"></a>Интерактивный MVVM

MVVM часто используется с двусторонними привязками данных для интерактивного представления на основе базовой модели данных.

Ниже приведен класс с именем `HslViewModel`, который преобразует `Color` значение в значения `Hue`, `Saturation`и `Luminosity`, и наоборот:

```csharp
using System;
using System.ComponentModel;
using Xamarin.Forms;

namespace XamlSamples
{
    public class HslViewModel : INotifyPropertyChanged
    {
        double hue, saturation, luminosity;
        Color color;

        public event PropertyChangedEventHandler PropertyChanged;

        public double Hue
        {
            set
            {
                if (hue != value)
                {
                    hue = value;
                    OnPropertyChanged("Hue");
                    SetNewColor();
                }
            }
            get
            {
                return hue;
            }
        }

        public double Saturation
        {
            set
            {
                if (saturation != value)
                {
                    saturation = value;
                    OnPropertyChanged("Saturation");
                    SetNewColor();
                }
            }
            get
            {
                return saturation;
            }
        }

        public double Luminosity
        {
            set
            {
                if (luminosity != value)
                {
                    luminosity = value;
                    OnPropertyChanged("Luminosity");
                    SetNewColor();
                }
            }
            get
            {
                return luminosity;
            }
        }

        public Color Color
        {
            set
            {
                if (color != value)
                {
                    color = value;
                    OnPropertyChanged("Color");

                    Hue = value.Hue;
                    Saturation = value.Saturation;
                    Luminosity = value.Luminosity;
                }
            }
            get
            {
                return color;
            }
        }

        void SetNewColor()
        {
            Color = Color.FromHsla(Hue, Saturation, Luminosity);
        }

        protected virtual void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

Изменения свойств `Hue`, `Saturation`и `Luminosity` приводят к изменению свойства `Color`, а изменения в `Color` приводят к изменению других трех свойств. Это может показаться бесконечным циклом, за исключением того, что класс не вызывает событие `PropertyChanged`, если только свойство не изменилось. Это помещает в цикл обратной связи в противном случае неконтролируемых.

Следующий XAML-файл содержит `BoxView`, свойство `Color` которого привязано к свойству `Color` класса ViewModel, а также три `Slider` и три `Label` представления, привязанные к свойствам `Hue`, `Saturation`и `Luminosity`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.HslColorScrollPage"
             Title="HSL Color Scroll Page">
    <ContentPage.BindingContext>
        <local:HslViewModel Color="Aqua" />
    </ContentPage.BindingContext>

    <StackLayout Padding="10, 0">
        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}"
               HorizontalOptions="Center" />

        <Slider Value="{Binding Hue, Mode=TwoWay}" />

        <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}"
               HorizontalOptions="Center" />

        <Slider Value="{Binding Saturation, Mode=TwoWay}" />

        <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}"
               HorizontalOptions="Center" />

        <Slider Value="{Binding Luminosity, Mode=TwoWay}" />
    </StackLayout>
</ContentPage>
```

Привязка для каждого `Label` является `OneWay`по умолчанию. Оно должно отображать только значение. Однако привязка для каждой `Slider` `TwoWay`. Это позволяет инициализировать `Slider` из ViewModel. Обратите внимание, что для свойства `Color` задано значение `Aqua` при создании экземпляра ViewModel. Но изменение в `Slider` также должно задать новое значение для свойства в ViewModel, которое затем вычисляет новый цвет.

[![](data-bindings-to-mvvm-images/hslcolorscroll.png "MVVM using Two-Way Data Bindings")](data-bindings-to-mvvm-images/hslcolorscroll-large.png#lightbox "MVVM using Two-Way Data Bindings")

## <a name="commanding-with-viewmodels"></a>Командная команда с ViewModels

Во многих случаях шаблон MVVM ограничивается обработкой элементов данных: объектами интерфейса пользователя в представлении Parallel Data Objects в ViewModel.

Однако иногда представление должно содержать кнопки, которые запускают различные действия в ViewModel. Но ViewModel не должен содержать обработчики `Clicked` для кнопок, так как он будет привязывать ViewModel к определенной парадигме пользовательского интерфейса.

Чтобы разрешить ViewModel более независимым от конкретных объектов пользовательского интерфейса, но по-прежнему разрешать вызов методов в ViewModel, существует интерфейс *команды* . Этот интерфейс команды поддерживается следующими элементами в Xamarin. Forms:

- `Button`
- `MenuItem`
- `ToolbarItem`
- `SearchBar`
- `TextCell` (и, следовательно, `ImageCell`)
- `ListView`
- `TapGestureRecognizer`

За исключением элемента `SearchBar` и `ListView`, эти элементы определяют два свойства:

- `Command` типа `System.Windows.Input.ICommand`
- `CommandParameter` типа `Object`

`SearchBar` определяет свойства `SearchCommand` и `SearchCommandParameter`, а `ListView` определяет `RefreshCommand` свойства типа `ICommand`.

Интерфейс `ICommand` определяет два метода и одно событие:

- `void Execute(object arg)`
- `bool CanExecute(object arg)`
- `event EventHandler CanExecuteChanged`

ViewModel может определять свойства типа `ICommand`. Затем можно привязать эти свойства к свойству `Command` каждого `Button` или другого элемента или, возможно, к пользовательскому представлению, реализующему этот интерфейс. При необходимости можно задать свойство `CommandParameter`, чтобы определить отдельные объекты `Button` (или другие элементы), привязанные к этому свойству ViewModel. На внутреннем уровне `Button` вызывает метод `Execute` каждый раз, когда пользователь отменяет `Button`, передавая методу `Execute` его `CommandParameter`.

Метод `CanExecute` и событие `CanExecuteChanged` используются в случаях, когда касание `Button` может быть недопустимым, в этом случае `Button` должен отключить себя. `Button` вызывает `CanExecute` при первом задании свойства `Command` и при каждом срабатывании события `CanExecuteChanged`. Если `CanExecute` возвращает `false`, `Button` отключается и не создает вызовы `Execute`.

Чтобы получить справку по добавлению команд в ViewModels, Xamarin. Forms определяет два класса, реализующих `ICommand`: `Command` и `Command<T>`, где `T` — это тип аргументов для `Execute` и `CanExecute`. Эти два класса определяют несколько конструкторов и метод `ChangeCanExecute`, который ViewModel может вызвать, чтобы заставить объект `Command` вызывать событие `CanExecuteChanged`.

Ниже приведено значение ViewModel для простой клавиатуры, предназначенной для ввода телефонных номеров. Обратите внимание, что методы `Execute` и `CanExecute` определены как лямбда-функции прямо в конструкторе:

```csharp
using System;
using System.ComponentModel;
using System.Windows.Input;
using Xamarin.Forms;

namespace XamlSamples
{
    class KeypadViewModel : INotifyPropertyChanged
    {
        string inputString = "";
        string displayText = "";
        char[] specialChars = { '*', '#' };

        public event PropertyChangedEventHandler PropertyChanged;

        // Constructor
        public KeypadViewModel()
        {
            AddCharCommand = new Command<string>((key) =>
                {
                    // Add the key to the input string.
                    InputString += key;
                });

            DeleteCharCommand = new Command(() =>
                {
                    // Strip a character from the input string.
                    InputString = InputString.Substring(0, InputString.Length - 1);
                },
                () =>
                {
                    // Return true if there's something to delete.
                    return InputString.Length > 0;
                });
        }

        // Public properties
        public string InputString
        {
            protected set
            {
                if (inputString != value)
                {
                    inputString = value;
                    OnPropertyChanged("InputString");
                    DisplayText = FormatText(inputString);

                    // Perhaps the delete button must be enabled/disabled.
                    ((Command)DeleteCharCommand).ChangeCanExecute();
                }
            }

            get { return inputString; }
        }

        public string DisplayText
        {
            protected set
            {
                if (displayText != value)
                {
                    displayText = value;
                    OnPropertyChanged("DisplayText");
                }
            }
            get { return displayText; }
        }

        // ICommand implementations
        public ICommand AddCharCommand { protected set; get; }

        public ICommand DeleteCharCommand { protected set; get; }

        string FormatText(string str)
        {
            bool hasNonNumbers = str.IndexOfAny(specialChars) != -1;
            string formatted = str;

            if (hasNonNumbers || str.Length < 4 || str.Length > 10)
            {
            }
            else if (str.Length < 8)
            {
                formatted = String.Format("{0}-{1}",
                                          str.Substring(0, 3),
                                          str.Substring(3));
            }
            else
            {
                formatted = String.Format("({0}) {1}-{2}",
                                          str.Substring(0, 3),
                                          str.Substring(3, 3),
                                          str.Substring(6));
            }
            return formatted;
        }

        protected void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

В этом ViewModel предполагается, что свойство `AddCharCommand` привязано к свойству `Command` нескольких кнопок (или любому другому, имеющему интерфейс команды), каждый из которых определяется `CommandParameter`. Эти кнопки добавляют символы в свойство `InputString`, которое затем форматируется как номер телефона для свойства `DisplayText`.

Существует также второе свойство типа `ICommand` с именем `DeleteCharCommand`. Это связано с кнопкой обратного расстояния, но кнопка должна быть отключена, если нет знаков для удаления.

Следующая клавиша не так сложнее, как может быть. Вместо этого разметка уменьшилась до минимума, чтобы продемонстрировать более четкое использование интерфейса команды:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.KeypadPage"
             Title="Keypad Page">

    <Grid HorizontalOptions="Center"
          VerticalOptions="Center">
        <Grid.BindingContext>
            <local:KeypadViewModel />
        </Grid.BindingContext>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="80" />
            <ColumnDefinition Width="80" />
            <ColumnDefinition Width="80" />
        </Grid.ColumnDefinitions>

        <!-- Internal Grid for top row of items -->
        <Grid Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="3">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="Auto" />
            </Grid.ColumnDefinitions>

            <Frame Grid.Column="0"
                   OutlineColor="Accent">
                <Label Text="{Binding DisplayText}" />
            </Frame>

            <Button Text="&#x21E6;"
                    Command="{Binding DeleteCharCommand}"
                    Grid.Column="1"
                    BorderWidth="0" />
        </Grid>

        <Button Text="1"
                Command="{Binding AddCharCommand}"
                CommandParameter="1"
                Grid.Row="1" Grid.Column="0" />

        <Button Text="2"
                Command="{Binding AddCharCommand}"
                CommandParameter="2"
                Grid.Row="1" Grid.Column="1" />

        <Button Text="3"
                Command="{Binding AddCharCommand}"
                CommandParameter="3"
                Grid.Row="1" Grid.Column="2" />

        <Button Text="4"
                Command="{Binding AddCharCommand}"
                CommandParameter="4"
                Grid.Row="2" Grid.Column="0" />

        <Button Text="5"
                Command="{Binding AddCharCommand}"
                CommandParameter="5"
                Grid.Row="2" Grid.Column="1" />

        <Button Text="6"
                Command="{Binding AddCharCommand}"
                CommandParameter="6"
                Grid.Row="2" Grid.Column="2" />

        <Button Text="7"
                Command="{Binding AddCharCommand}"
                CommandParameter="7"
                Grid.Row="3" Grid.Column="0" />

        <Button Text="8"
                Command="{Binding AddCharCommand}"
                CommandParameter="8"
                Grid.Row="3" Grid.Column="1" />

        <Button Text="9"
                Command="{Binding AddCharCommand}"
                CommandParameter="9"
                Grid.Row="3" Grid.Column="2" />

        <Button Text="*"
                Command="{Binding AddCharCommand}"
                CommandParameter="*"
                Grid.Row="4" Grid.Column="0" />

        <Button Text="0"
                Command="{Binding AddCharCommand}"
                CommandParameter="0"
                Grid.Row="4" Grid.Column="1" />

        <Button Text="#"
                Command="{Binding AddCharCommand}"
                CommandParameter="#"
                Grid.Row="4" Grid.Column="2" />
    </Grid>
</ContentPage>
```

Свойство `Command` первого `Button`, которое отображается в этой разметке, привязано к `DeleteCharCommand`у; остальные привязываются к `AddCharCommand` с `CommandParameter`, который совпадает с символом, отображаемым на `Button` лицевой стороне. Вот эта программа в действии:

[![](data-bindings-to-mvvm-images/keypad.png "Calculator using MVVM and Commands")](data-bindings-to-mvvm-images/keypad-large.png#lightbox "Calculator using MVVM and Commands")

### <a name="invoking-asynchronous-methods"></a>Вызов асинхронных методов

Команды также могут вызывать асинхронные методы. Это достигается с помощью ключевых слов `async` и `await` при указании метода `Execute`:

```csharp
DownloadCommand = new Command (async () => await DownloadAsync ());
```

Это означает, что метод `DownloadAsync` является `Task` и должен быть ожидаемым:

```csharp
async Task DownloadAsync ()
{
    await Task.Run (() => Download ());
}

void Download ()
{
    ...
}
```

## <a name="implementing-a-navigation-menu"></a>Реализация меню навигации

Программа [ксамлсамплес](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples) , содержащая весь исходный код в этой серии статей, использует ViewModel для своей домашней страницы. Этот ViewModel является определением короткого класса с тремя свойствами с именами `Type`, `Title`и `Description`, которые содержат тип каждого из образцов страниц, заголовок и краткое описание. Кроме того, ViewModel определяет статическое свойство с именем `All`, которое является коллекцией всех страниц в программе:

```csharp
public class PageDataViewModel
{
    public PageDataViewModel(Type type, string title, string description)
    {
        Type = type;
        Title = title;
        Description = description;
    }

    public Type Type { private set; get; }

    public string Title { private set; get; }

    public string Description { private set; get; }

    static PageDataViewModel()
    {
        All = new List<PageDataViewModel>
        {
            // Part 1. Getting Started with XAML
            new PageDataViewModel(typeof(HelloXamlPage), "Hello, XAML",
                                  "Display a Label with many properties set"),

            new PageDataViewModel(typeof(XamlPlusCodePage), "XAML + Code",
                                  "Interact with a Slider and Button"),

            // Part 2. Essential XAML Syntax
            new PageDataViewModel(typeof(GridDemoPage), "Grid Demo",
                                  "Explore XAML syntax with the Grid"),

            new PageDataViewModel(typeof(AbsoluteDemoPage), "Absolute Demo",
                                  "Explore XAML syntax with AbsoluteLayout"),

            // Part 3. XAML Markup Extensions
            new PageDataViewModel(typeof(SharedResourcesPage), "Shared Resources",
                                  "Using resource dictionaries to share resources"),

            new PageDataViewModel(typeof(StaticConstantsPage), "Static Constants",
                                  "Using the x:Static markup extensions"),

            new PageDataViewModel(typeof(RelativeLayoutPage), "Relative Layout",
                                  "Explore XAML markup extensions"),

            // Part 4. Data Binding Basics
            new PageDataViewModel(typeof(SliderBindingsPage), "Slider Bindings",
                                  "Bind properties of two views on the page"),

            new PageDataViewModel(typeof(SliderTransformsPage), "Slider Transforms",
                                  "Use Sliders with reverse bindings"),

            new PageDataViewModel(typeof(ListViewDemoPage), "ListView Demo",
                                  "Use a ListView with data bindings"),

            // Part 5. From Data Bindings to MVVM
            new PageDataViewModel(typeof(OneShotDateTimePage), "One-Shot DateTime",
                                  "Obtain the current DateTime and display it"),

            new PageDataViewModel(typeof(ClockPage), "Clock",
                                  "Dynamically display the current time"),

            new PageDataViewModel(typeof(HslColorScrollPage), "HSL Color Scroll",
                                  "Use a view model to select HSL colors"),

            new PageDataViewModel(typeof(KeypadPage), "Keypad",
                                  "Use a view model for numeric keypad logic")
        };
    }

    public static IList<PageDataViewModel> All { private set; get; }
}
```

XAML-файл для `MainPage` определяет `ListBox`, свойству `ItemsSource` которого присвоено это свойство `All`, которое содержит `TextCell` для отображения свойств `Title` и `Description` каждой страницы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.MainPage"
             Padding="5, 0"
             Title="XAML Samples">

    <ListView ItemsSource="{x:Static local:PageDataViewModel.All}"
              ItemSelected="OnListViewItemSelected">
        <ListView.ItemTemplate>
            <DataTemplate>
                <TextCell Text="{Binding Title}"
                          Detail="{Binding Description}" />
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>
```

Страницы отображаются в прокручиваемом списке:

[![](data-bindings-to-mvvm-images/mainpage.png "Scrollable list of pages")](data-bindings-to-mvvm-images/mainpage-large.png#lightbox "Scrollable list of pages")

Обработчик в файле кода программной части активируется, когда пользователь выбирает элемент. Обработчик задает для свойства `SelectedItem` `ListBox` обратно `null`, а затем создает экземпляр выбранной страницы и переходит к ней:

```csharp
private async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs args)
{
    (sender as ListView).SelectedItem = null;

    if (args.SelectedItem != null)
    {
        PageDataViewModel pageData = args.SelectedItem as PageDataViewModel;
        Page page = (Page)Activator.CreateInstance(pageData.Type);
        await Navigation.PushAsync(page);
    }
}
```

## <a name="video"></a>Видео

> [!VIDEO https://youtube.com/embed/DYRLcqG2BAY]

**Xamarin развивается 2016: MVVM стало простым благодаря Xamarin. Forms и Prism**

## <a name="summary"></a>Сводка

XAML — это мощный инструмент для определения пользовательских интерфейсов в приложениях Xamarin. Forms, особенно при использовании привязки данных и MVVM. Результатом является четкое, элегантное и потенциально доступное для инструментария представление пользовательского интерфейса со всей фоновой поддержкой в коде.

## <a name="related-links"></a>Связанные ссылки

- [ксамлсамплес](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [Часть 1. начало работы с XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Часть 2. Важный синтаксис XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Часть 3. Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Часть 4. Основы привязки данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)

## <a name="related-videos"></a>Связанные видео

> [!Video https://channel9.msdn.com/Series/Xamarin-101/XamarinForms-MVVM-with-XAML-6-of-11/player]

> [!Video https://channel9.msdn.com/Series/Xamarin-101/XamarinForms-Navigation-with-XAML-7-of-11/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
