---
title: Кнопка Xamarin. Forms
description: Кнопка реагирует на касание или щелчок, которое направляет приложение для выполнения определенной задачи.
ms.prod: xamarin
ms.assetid: 62CAEB63-0800-44F4-9B8C-EE632138C2F5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/19/2018
ms.openlocfilehash: 0dde561b3b494415986e0cddf99f8ad145e34687
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696573"
---
# <a name="xamarinforms-button"></a>Кнопка Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)

_Кнопка реагирует на касание или щелчок, которое направляет приложение для выполнения определенной задачи._

[@No__t_1](xref:Xamarin.Forms.Button) является самым фундаментальным интерактивным элементом управления во всех Xamarin. Forms. @No__t_0 обычно отображает короткую текстовую строку, указывающую команду, но может также отображать точечный рисунок или сочетание текста и изображения. Пользователь нажимает `Button` с помощью пальца или щелкает его с помощью мыши, чтобы запустить эту команду.

Большинство описанных ниже разделов соответствуют страницам образца [**буттондемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) .

## <a name="handling-button-clicks"></a>Обработка нажатий кнопок

`Button` определяет событие [`Clicked`](xref:Xamarin.Forms.Button.Clicked) , которое возникает, когда пользователь отменяет `Button` с помощью пальца или указателя мыши. Событие возникает при отпускании кнопки пальца или мыши с поверхности `Button`. Свойство [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) для `Button` должно иметь значение `true`, чтобы оно отвечало на касания.

На странице **"основные"** в образце [**буттондемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) показано, как создать экземпляр `Button` в XAML и обработать его `Clicked` событие. Файл **басикбуттонкликкпаже. XAML** содержит `StackLayout` с `Label` и `Button`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.BasicButtonClickPage"
             Title="Basic Button Click">
    <StackLayout>

        <Label x:Name="label"
               Text="Click the Button below"
               FontSize="Large"
               VerticalOptions="CenterAndExpand"
               HorizontalOptions="Center" />

        <Button Text="Click to Rotate Text!"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Clicked="OnButtonClicked" />

    </StackLayout>
</ContentPage>
```

@No__t_0, как правило, занимают все пространство, допустимое для него. Например, если не задать для свойства `HorizontalOptions` `Button` значение, отличное от `Fill`, `Button` будет занимать всю ширину родительского элемента.

По умолчанию `Button` является прямоугольным, но можно передать скругленные углы с помощью свойства [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) , как описано ниже в разделе « [**вид кнопки раздела»** ](#button-appearance).

Свойство [`Text`](xref:Xamarin.Forms.Button.Text) указывает текст, отображаемый в `Button`. Для события [`Clicked`](xref:Xamarin.Forms.Button.Clicked) задается обработчик событий с именем `OnButtonClicked`. Этот обработчик находится в файле кода программной части **BasicButtonClickPage.XAML.CS**:

```csharp
public partial class BasicButtonClickPage : ContentPage
{
    public BasicButtonClickPage ()
    {
        InitializeComponent ();
    }

    async void OnButtonClicked(object sender, EventArgs args)
    {
        await label.RelRotateTo(360, 1000);
    }
}
```

При нажатии `Button` выполняется метод `OnButtonClicked`. Аргумент `sender` — это `Button` объект, отвечающий за это событие. Его можно использовать для доступа к объекту `Button` или для различения нескольких объектов `Button`, совместно использующих одно и то же событие `Clicked`.

Этот конкретный обработчик `Clicked` вызывает функцию анимации, которая поворачивает `Label` 360 градусов в 1000 миллисекунд. Вот программа, выполняемая на устройствах iOS и Android, а также как приложение универсальная платформа Windows (UWP) на рабочем столе Windows 10:

[![Нажатие кнопки "базовый"](button-images/BasicButtonClick.png "Нажатие кнопки "базовый"")](button-images/BasicButtonClick-Large.png#lightbox "Нажатие кнопки "базовый"")

Обратите внимание, что метод `OnButtonClicked` включает модификатор `async`, так как `await` используется в обработчике событий. Обработчик событий `Clicked` требует модификатора `async` только в том случае, если тело обработчика использует `await`.

Каждая платформа подготавливает `Button` к просмотру отдельно от конкретного способа. В разделе [**вид кнопки**](#button-appearance) вы узнаете, как задать цвета и сделать границу `Button` видимой для более настраиваемых представлений. `Button` реализует интерфейс [`IFontElement`](xref:Xamarin.Forms.Internals.IFontElement) , поэтому он включает свойства [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily), [`FontSize`](xref:Xamarin.Forms.Button.FontSize)и [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes) .

## <a name="creating-a-button-in-code"></a>Создание кнопки в коде

Обычно создается экземпляр `Button` в XAML, но в коде также можно создать `Button`. Это может быть удобно, когда приложению нужно создать несколько кнопок на основе данных, которые перечислены с помощью цикла `foreach`.

На странице Нажатие **кнопки кода** показано, как создать страницу, которая функционально эквивалентна **базовой щелчку кнопки** Page, но полностью в C#:

```csharp
public class CodeButtonClickPage : ContentPage
{
    public CodeButtonClickPage ()
    {
        Title = "Code Button Click";

        Label label = new Label
        {
            Text = "Click the Button below",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            VerticalOptions = LayoutOptions.CenterAndExpand,
            HorizontalOptions = LayoutOptions.Center
        };

        Button button = new Button
        {
            Text = "Click to Rotate Text!",
            VerticalOptions = LayoutOptions.CenterAndExpand,
            HorizontalOptions = LayoutOptions.Center
        };
        button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);

        Content = new StackLayout
        {
            Children =
            {
                label,
                button
            }
        };
    }
}
```

Все выполняется в конструкторе класса. Так как обработчик `Clicked` является только одной инструкцией, он может быть присоединен к событию очень просто:

```csharp
button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);
```

Разумеется, можно также определить обработчик событий как отдельный метод (точно так же, как метод `OnButtonClick` при **нажатии кнопки Basic**) и присоединить этот метод к событию:

```csharp
button.Clicked += OnButtonClicked;
```

## <a name="disabling-the-button"></a>Отключение кнопки

Иногда приложение находится в определенном состоянии, в котором определенное `Button` щелчком не является допустимой операцией. В таких случаях `Button` следует отключить, задав свойству `IsEnabled` значение `false`. Классический пример — это `Entry` элемент управления для имени файла, сопровождающего `Button` открытия файлов: `Button` должен быть включен, только если в `Entry` был введен какой-либо текст.
Для этой задачи можно использовать `DataTrigger`, как показано в статье [**триггеры данных**](~/xamarin-forms/app-fundamentals/triggers.md#data-triggers) .

## <a name="using-the-command-interface"></a>Использование интерфейса команд

Приложение может реагировать на `Button` касаний без обработки события `Clicked`. @No__t_0 реализует альтернативный механизм уведомления, называемый _командой_ или интерфейсом _команд_ . Это состоит из двух свойств:

- [`Command`](xref:Xamarin.Forms.Button.Command) типа [`ICommand`](xref:System.Windows.Input.ICommand), интерфейс, определенный в пространстве имен [`System.Windows.Input`](xref:System.Windows.Input) .
- Свойство [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) типа [`Object`](xref:System.Object).

Этот подход особенно удобен в связи с привязкой данных, особенно при реализации архитектуры Model-View-ViewModel (MVVM). Эти разделы обсуждаются в статье [Привязка](~/xamarin-forms/app-fundamentals/data-binding/index.md)данных, [из привязок данных к MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)и [MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md).

В приложении MVVM ViewModel определяет свойства типа `ICommand`, которые затем соединяются с элементами `Button` XAML с привязками данных. Xamarin. Forms также определяет классы [`Command`](xref:Xamarin.Forms.Command) и [`Command<T>`](xref:Xamarin.Forms.Command`1) , реализующие интерфейс `ICommand` и помогающие ViewModel при определении свойств типа `ICommand`.

Команда описывается более подробно в статье о [**интерфейсе команды**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) , но на странице **команды Basic** в образце [**буттондемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) показан базовый подход.

Класс `CommandDemoViewModel` — это очень простой ViewModel, определяющий свойство типа `double` с именем `Number`, и два свойства типа `ICommand` с именами `MultiplyBy2Command` и `DivideBy2Command`:

```csharp
class CommandDemoViewModel : INotifyPropertyChanged
{
    double number = 1;

    public event PropertyChangedEventHandler PropertyChanged;

    public CommandDemoViewModel()
    {
        MultiplyBy2Command = new Command(() => Number *= 2);

        DivideBy2Command = new Command(() => Number /= 2);
    }

    public double Number
    {
        set
        {
            if (number != value)
            {
                number = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Number"));
            }
        }
        get
        {
            return number;
        }
    }

    public ICommand MultiplyBy2Command { private set; get; }

    public ICommand DivideBy2Command { private set; get; }
}
```

Два свойства `ICommand` инициализируются в конструкторе класса с двумя объектами типа `Command`. @No__t_0 конструкторы содержат небольшую функцию (аргумент конструктора `execute`), которая либо вдвое, либо вдвое является свойством `Number`.

Файл **басикбуттонкомманд. XAML** задает для его `BindingContext` экземпляр `CommandDemoViewModel`. Элемент `Label` и два элемента `Button` содержат привязки к трем свойствам в `CommandDemoViewModel`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.BasicButtonCommandPage"
             Title="Basic Button Command">

    <ContentPage.BindingContext>
        <local:CommandDemoViewModel />
    </ContentPage.BindingContext>

    <StackLayout>
        <Label Text="{Binding Number, StringFormat='Value is now {0}'}"
               FontSize="Large"
               VerticalOptions="CenterAndExpand"
               HorizontalOptions="Center" />

        <Button Text="Multiply by 2"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Command="{Binding MultiplyBy2Command}" />

        <Button Text="Divide by 2"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Command="{Binding DivideBy2Command}" />
    </StackLayout>
</ContentPage>
```

По мере касания двух элементов `Button` выполняются команды, а число изменяется в значении:

[![Команда "основные"](button-images/BasicButtonCommand.png "Команда "основные"")](button-images/BasicButtonCommand-Large.png#lightbox)

Преимущество такого подхода `Clicked` обработчиков заключается в том, что вся логика, включающая в себя функциональность этой страницы, расположена в ViewModel, а не в файле кода программной части, что обеспечивает более эффективное разделение пользовательского интерфейса от бизнес-логики.

Кроме того, `Command` объекты могут управлять включением и отключением элементов `Button`. Например, предположим, что требуется ограничить диапазон числовых значений от 2<sup>10</sup> до 2<sup> &ndash;10</sup>. Можно добавить в конструктор другую функцию (называемую `canExecute` аргументом), которая возвращает `true`, если `Button` должны быть включены. Ниже приведены изменения в конструкторе `CommandDemoViewModel`.

```csharp
class CommandDemoViewModel : INotifyPropertyChanged
{
    ···
    public CommandDemoViewModel()
    {
        MultiplyBy2Command = new Command(
            execute: () =>
            {
                Number *= 2;
                ((Command)MultiplyBy2Command).ChangeCanExecute();
                ((Command)DivideBy2Command).ChangeCanExecute();
            },
            canExecute: () => Number < Math.Pow(2, 10));

        DivideBy2Command = new Command(
            execute: () =>
            {
                Number /= 2;
                ((Command)MultiplyBy2Command).ChangeCanExecute();
                ((Command)DivideBy2Command).ChangeCanExecute();
            },
            canExecute: () => Number > Math.Pow(2, -10));
    }
    ···
}
```

Вызовы метода `ChangeCanExecute` `Command` необходимы, чтобы метод `Command` мог вызвать метод `canExecute` и определить, следует ли отключать `Button`. При изменении этого кода, когда число достигает предела, `Button` отключается:

[![Команда "основные" — изменено](button-images/BasicButtonCommandModified.png "Команда "основные" — изменено")](button-images/BasicButtonCommandModified-Large.png#lightbox)

Возможно, несколько `Button` элементов будут привязаны к одному и тому же свойству `ICommand`. Элементы `Button` можно отличать с помощью свойства [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) `Button`. В этом случае необходимо использовать универсальный класс [`Command<T>`](xref:Xamarin.Forms.Command`1) . Затем объект `CommandParameter` передается в качестве аргумента методам `execute` и `canExecute`. Этот метод подробно описан в разделе " [**базовый" команд**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) в статье о [**интерфейсе команд**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) .

В примере [**буттондемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) этот метод также используется в своем `MainPage`ном классе. Файл **MainPage. XAML** содержит `Button` для каждой страницы примера:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.MainPage"
             Title="Button Demos">
    <ScrollView>
        <FlexLayout Direction="Column"
                    JustifyContent="SpaceEvenly"
                    AlignItems="Center">

            <Button Text="Basic Button Click"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:BasicButtonClickPage}" />

            <Button Text="Code Button Click"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:CodeButtonClickPage}" />

            <Button Text="Basic Button Command"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:BasicButtonCommandPage}" />

            <Button Text="Press and Release Button"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:PressAndReleaseButtonPage}" />

            <Button Text="Button Appearance"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ButtonAppearancePage}" />

            <Button Text="Toggle Button Demo"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ToggleButtonDemoPage}" />

            <Button Text="Image Button Demo"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ImageButtonDemoPage}" />

        </FlexLayout>
    </ScrollView>
</ContentPage>
```

Каждый `Button` имеет свойство `Command`, привязанное к свойству с именем `NavigateCommand`, а `CommandParameter` устанавливается в [`Type`](xref:System.Type) объект, соответствующий одному из классов страниц в проекте.

Это `NavigateCommand` свойство имеет тип `ICommand` и определено в файле кода программной части:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(async (Type pageType) =>
        {
            Page page = (Page)Activator.CreateInstance(pageType);
            await Navigation.PushAsync(page);
        });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

Конструктор инициализирует свойство `NavigateCommand` для объекта `Command<Type>`, так как `Type` является типом объекта `CommandParameter`, установленного в файле XAML. Это означает, что метод `execute` имеет аргумент типа `Type`, соответствующий этому объекту `CommandParameter`. Функция создает экземпляр страницы, а затем переходит к ней.

Обратите внимание, что конструктор завершается путем установки `BindingContext` самого себя. Это необходимо для привязки свойств в файле XAML к свойству `NavigateCommand`.

## <a name="pressing-and-releasing-the-button"></a>Нажатие и отпускание кнопки

Помимо события `Clicked` `Button` также определяет события [`Pressed`](xref:Xamarin.Forms.Button.Pressed) и [`Released`](xref:Xamarin.Forms.Button.Released). Событие `Pressed` возникает, когда палец нажимает на `Button` или нажата кнопка мыши с указателем, расположенным на `Button`. Событие `Released` возникает при отпускании кнопки мыши или пальца. Как правило, событие `Clicked` также срабатывает одновременно с событием `Released`, но если палец или указатель мыши выходит за пределы поверхности `Button` перед тем, как выпустить, событие `Clicked` может не произойти.

События `Pressed` и `Released` часто не используются, но их можно использовать для специальных целей, как показано на странице нажатия **и выпуска** . Файл XAML содержит `Label` и `Button` с обработчиками, присоединенными к событиям `Pressed` и `Released`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.PressAndReleaseButtonPage"
             Title="Press and Release Button">
    <StackLayout>

        <Label x:Name="label"
               Text="Press and hold the Button below"
               FontSize="Large"
               VerticalOptions="CenterAndExpand"
               HorizontalOptions="Center" />

        <Button Text="Press to Rotate Text!"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Pressed="OnButtonPressed"
                Released="OnButtonReleased" />

    </StackLayout>
</ContentPage>
```

Файл кода программной части анимируется `Label` при возникновении события `Pressed`, но приостанавливает вращение при возникновении события `Released`:

```csharp
public partial class PressAndReleaseButtonPage : ContentPage
{
    bool animationInProgress = false;
    Stopwatch stopwatch = new Stopwatch();

    public PressAndReleaseButtonPage ()
    {
        InitializeComponent ();
    }

    void OnButtonPressed(object sender, EventArgs args)
    {
        stopwatch.Start();
        animationInProgress = true;

        Device.StartTimer(TimeSpan.FromMilliseconds(16), () =>
        {
            label.Rotation = 360 * (stopwatch.Elapsed.TotalSeconds % 1);

            return animationInProgress;
        });
    }

    void OnButtonReleased(object sender, EventArgs args)
    {
        animationInProgress = false;
        stopwatch.Stop();
    }
}
```

В результате `Label` поворачивается только тогда, когда палец связан с `Button` и останавливается при отснятии пальца:

[![Нажатие кнопки "Пуск"](button-images/PressAndReleaseButton.png "Нажатие кнопки "Пуск"")](button-images/PressAndReleaseButton-Large.png)

Этот тип поведения имеет приложения для игр: палец, находящегося на `Button`, может сделать объект на экране в определенном направлении.

<a name="button-appearance" />

## <a name="button-appearance"></a>Внешний вид кнопки

@No__t_0 наследует или определяет несколько свойств, влияющих на его внешний вид:

- [`TextColor`](xref:Xamarin.Forms.Button.TextColor) — цвет текста `Button`
- [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) — цвет фона для этого текста
- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) — это цвет области, окружающей `Button`
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily) — это семейство шрифтов, используемое для текста
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize) — это размер текста
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes) указывает, что текст выделен курсивом или полужирным шрифтом
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) — ширина границы
- [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) является радиусом угла `Button`
- `CharacterSpacing` интервал между символами `Button`ого текста

> [!NOTE]
> Класс `Button` также имеет [`Margin`](xref:Xamarin.Forms.View.Margin) и [`Padding`](xref:Xamarin.Forms.Button.Padding) свойства, управляющие поведением макета `Button`. Дополнительные сведения см. в статье [Поля и заполнение](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

Последствия шести из этих свойств (за исключением `FontFamily` и `FontAttributes`) показаны на странице « **Оформление кнопки»** . Другое свойство, [`Image`](xref:Xamarin.Forms.Button.ImageSource), рассматривается в разделе [**Использование растровых изображений с кнопкой**](#image-button).

Все представления и привязки данных на странице « **Оформление кнопки»** определены в файле XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.ButtonAppearancePage"
             Title="Button Appearance">
    <StackLayout>
        <Button x:Name="button"
                Text="Button"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                TextColor="{Binding Source={x:Reference textColorPicker},
                                    Path=SelectedItem.Color}"
                BackgroundColor="{Binding Source={x:Reference backgroundColorPicker},
                                          Path=SelectedItem.Color}"
                BorderColor="{Binding Source={x:Reference borderColorPicker},
                                      Path=SelectedItem.Color}" />

        <StackLayout BindingContext="{x:Reference button}"
                     Padding="10">

            <Slider x:Name="fontSizeSlider"
                    Maximum="48"
                    Minimum="1"
                    Value="{Binding FontSize}" />

            <Label Text="{Binding Source={x:Reference fontSizeSlider},
                                  Path=Value,
                                  StringFormat='FontSize = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Slider x:Name="borderWidthSlider"
                    Minimum="-1"
                    Maximum="12"
                    Value="{Binding BorderWidth}" />

            <Label Text="{Binding Source={x:Reference borderWidthSlider},
                                  Path=Value,
                                  StringFormat='BorderWidth = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Slider x:Name="cornerRadiusSlider"
                    Minimum="-1"
                    Maximum="24"
                    Value="{Binding CornerRadius}" />

            <Label Text="{Binding Source={x:Reference cornerRadiusSlider},
                                  Path=Value,
                                  StringFormat='CornerRadius = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Grid.Resources>
                    <Style TargetType="Label">
                        <Setter Property="VerticalOptions" Value="Center" />
                    </Style>
                </Grid.Resources>

                <Label Text="Text Color:"
                       Grid.Row="0" Grid.Column="0" />

                <Picker x:Name="textColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="0" Grid.Column="1" />

                <Label Text="Background Color:"
                       Grid.Row="1" Grid.Column="0" />

                <Picker x:Name="backgroundColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="1" Grid.Column="1" />

                <Label Text="Border Color:"
                       Grid.Row="2" Grid.Column="0" />

                <Picker x:Name="borderColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="2" Grid.Column="1" />
            </Grid>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

В `Button` в верхней части страницы есть три свойства `Color`, привязанные к элементам `Picker` в нижней части страницы. Элементы в `Picker` являются цветами из класса `NamedColor`, который входит в проект. Три `Slider` элементов содержат двусторонние привязки к свойствам `FontSize`, `BorderWidth` и `CornerRadius` `Button`.

Эта программа позволяет экспериментировать с сочетаниями всех этих свойств:

[![Внешний вид кнопки](button-images/ButtonAppearance.png "Внешний вид кнопки")](button-images/ButtonAppearance-Large.png)

Чтобы увидеть `Button` границу, необходимо задать для `BorderColor` значение, отличное от `Default`, а `BorderWidth` положительным значением.

В iOS вы заметите, что большая ширина границы будет воздействовать на внутреннюю часть `Button` и повлияет на отображение текста. Если вы решили использовать границу с `Button` iOS, то, вероятно, захотите начать и завершить свойство `Text` с пробелами, чтобы оставаться видимым.

В UWP выбор `CornerRadius`, размер которого превышает половину высоты `Button` вызывает исключение.

## <a name="button-visual-states"></a>Визуальные состояния кнопки

[`Button`](xref:Xamarin.Forms.Button) имеет `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) , который можно использовать для инициации визуального изменения `Button` при нажатии пользователем при условии, что он включен.

В следующем примере XAML показано, как определить визуальное состояние для состояния `Pressed`.

```xaml
<Button Text="Click me!"
        ...>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="1" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="0.8" />
                </VisualState.Setters>
            </VisualState>

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Button>
```

@No__t_0 [`VisualState`](xref:Xamarin.Forms.VisualState) указывает, что при нажатии [`Button`](xref:Xamarin.Forms.Button) его свойство [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) будет заменено значением по умолчанию от 1 до 0,8. @No__t_0 `VisualState` указывает, что если `Button` находится в нормальном состоянии, его свойство `Scale` будет установлено в значение 1. Таким образом, общий результат заключается в том, что при нажатии `Button` его масштаб уменьшается, и при освобождении `Button` его размер изменяется до размера по умолчанию.

Дополнительные сведения о визуальных состояниях см. [в разделе Диспетчер визуальных состояний Xamarin. Forms](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="creating-a-toggle-button"></a>Создание выключателя

Можно создать подкласс `Button`, чтобы он работал как параметр "вкл.". Нажмите кнопку один раз, чтобы включить кнопку, и коснитесь кнопки еще раз, чтобы отключить ее.

Следующий класс `ToggleButton` является производным от `Button` и определяет новое событие с именем `Toggled` и логическое свойство с именем `IsToggled`. Это те же два свойства, которые определены [`Switch`](xref:Xamarin.Forms.Switch)Xamarin. Forms:

```csharp
class ToggleButton : Button
{
    public event EventHandler<ToggledEventArgs> Toggled;

    public static BindableProperty IsToggledProperty =
        BindableProperty.Create("IsToggled", typeof(bool), typeof(ToggleButton), false,
                                propertyChanged: OnIsToggledChanged);

    public ToggleButton()
    {
        Clicked += (sender, args) => IsToggled ^= true;
    }

    public bool IsToggled
    {
        set { SetValue(IsToggledProperty, value); }
        get { return (bool)GetValue(IsToggledProperty); }
    }

    protected override void OnParentSet()
    {
        base.OnParentSet();
        VisualStateManager.GoToState(this, "ToggledOff");
    }

    static void OnIsToggledChanged(BindableObject bindable, object oldValue, object newValue)
    {
        ToggleButton toggleButton = (ToggleButton)bindable;
        bool isToggled = (bool)newValue;

        // Fire event
        toggleButton.Toggled?.Invoke(toggleButton, new ToggledEventArgs(isToggled));

        // Set the visual state
        VisualStateManager.GoToState(toggleButton, isToggled ? "ToggledOn" : "ToggledOff");
    }
}
```

Конструктор `ToggleButton` присоединяет обработчик к событию `Clicked`, чтобы он мог изменить значение свойства `IsToggled`. Метод `OnIsToggledChanged` запускает событие `Toggled`.

Последняя строка метода `OnIsToggledChanged` вызывает статический метод `VisualStateManager.GoToState` с двумя текстовыми строками "Тоггледон" и "Тоггледофф". Вы можете прочитать об этом методе и о том, как приложение может реагировать на визуальные состояния в статье [**Диспетчер визуальных состояний Xamarin. Forms**](~/xamarin-forms/user-interface/visual-state-manager.md).

Поскольку `ToggleButton` выполняет вызов `VisualStateManager.GoToState`, классу не нужно включать дополнительные средства для изменения внешнего вида кнопки на основе ее состояния `IsToggled`. Это несет ответственность за XAML, в котором размещается `ToggleButton`.

**Демонстрационная Страница переключателя** содержит два экземпляра `ToggleButton`, включая разметку диспетчера визуального состояния, которая задает `Text`, `BackgroundColor` и `TextColor` кнопки на основе визуального состояния:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.ToggleButtonDemoPage"
             Title="Toggle Button Demo">

    <ContentPage.Resources>
        <Style TargetType="local:ToggleButton">
            <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            <Setter Property="HorizontalOptions" Value="Center" />
        </Style>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <local:ToggleButton Toggled="OnItalicButtonToggled">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ToggleStates">
                    <VisualState Name="ToggledOff">
                        <VisualState.Setters>
                            <Setter Property="Text" Value="Italic Off" />
                            <Setter Property="BackgroundColor" Value="#C0C0C0" />
                            <Setter Property="TextColor" Value="Black" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="ToggledOn">
                        <VisualState.Setters>
                            <Setter Property="Text" Value=" Italic On " />
                            <Setter Property="BackgroundColor" Value="#404040" />
                            <Setter Property="TextColor" Value="White" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </local:ToggleButton>

        <local:ToggleButton Toggled="OnBoldButtonToggled">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ToggleStates">
                    <VisualState Name="ToggledOff">
                        <VisualState.Setters>
                            <Setter Property="Text" Value="Bold Off" />
                            <Setter Property="BackgroundColor" Value="#C0C0C0" />
                            <Setter Property="TextColor" Value="Black" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="ToggledOn">
                        <VisualState.Setters>
                            <Setter Property="Text" Value=" Bold On " />
                            <Setter Property="BackgroundColor" Value="#404040" />
                            <Setter Property="TextColor" Value="White" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </local:ToggleButton>

        <Label x:Name="label"
               Text="Just a little passage of some sample text that can be formatted in italic or boldface by toggling the two buttons."
               FontSize="Large"
               HorizontalTextAlignment="Center"
               VerticalOptions="CenterAndExpand" />

    </StackLayout>
</ContentPage>
```

Обработчики событий `Toggled` находятся в файле кода программной части. Они отвечают за установку свойства `FontAttributes` `Label` в зависимости от состояния кнопок:

```csharp
public partial class ToggleButtonDemoPage : ContentPage
{
    public ToggleButtonDemoPage ()
    {
        InitializeComponent ();
    }

    void OnItalicButtonToggled(object sender, ToggledEventArgs args)
    {
        if (args.Value)
        {
            label.FontAttributes |= FontAttributes.Italic;
        }
        else
        {
            label.FontAttributes &= ~FontAttributes.Italic;
        }
    }

    void OnBoldButtonToggled(object sender, ToggledEventArgs args)
    {
        if (args.Value)
        {
            label.FontAttributes |= FontAttributes.Bold;
        }
        else
        {
            label.FontAttributes &= ~FontAttributes.Bold;
        }
    }
}
```

Вот программа, выполняемая в iOS, Android и UWP:

[![Демонстрация выключателя](button-images/ToggleButtonDemo.png "Демонстрация выключателя")](button-images/ToggleButtonDemo-Large.png#lightbox)

<a name="image-button" />

## <a name="using-bitmaps-with-buttons"></a>Использование точечных рисунков с кнопками

Класс `Button` определяет свойство [`ImageSource`](xref:Xamarin.Forms.Button.Image) , которое позволяет отображать растровое изображение на `Button` либо отдельно, либо в сочетании с текстом. Можно также указать способ упорядочения текста и изображения.

Свойство `ImageSource` имеет тип [`ImageSource`](xref:Xamarin.Forms.ImageSource). Это означает, что точечные рисунки можно загружать из файла, внедренного ресурса, URI или потока.

Каждая платформа, поддерживаемая Xamarin. Forms, позволяет хранить изображения в нескольких размерах для различных разрешающих точек различных устройств, на которых может работать приложение. Эти несколько точечных рисунков называются или хранятся таким образом, что операционная система может выбрать лучшее соответствие для разрешения экрана устройства.

Для точечных рисунков в `Button` наилучший размер обычно составляет от 32 до 64 единиц, независимых от устройств, в зависимости от того, насколько оно должно быть. Изображения, используемые в этом примере, основаны на размере 48 единиц, не зависящих от устройства.

В проекте iOS папка **Resources** содержит три размера этого образа:

- Точечный рисунок размером 48 пикселей, хранящийся как **/ресаурцес/монкэйфаце.ПНГ**
- Точечный рисунок размером 96 пикселей, хранящийся как **/Resource/MonkeyFace@2x.png**
- Точечный рисунок размером 144 пикселей, хранящийся как **/Resource/MonkeyFace@3x.png**

Всем трем точечным рисункам было предоставлено **действие сборки** **BundleResource**.

Для проекта Android все точечные рисунки имеют одинаковое имя, но хранятся в разных вложенных папках папки **Resources** :

- Точечный рисунок размером 72 пикселей, хранящийся как **/ресаурцес/дравабле-хдпи/монкэйфаце.ПНГ**
- Точечный рисунок размером 96 пикселей, хранящийся как **/ресаурцес/дравабле-ксхдпи/монкэйфаце.ПНГ**
- Точечный рисунок размером 144 пикселей, хранящийся как **/ресаурцес/дравабле-ксксхдпи/монкэйфаце.ПНГ**
- Точечный рисунок размером 192 пикселей, хранящийся как **/ресаурцес/дравабле-ксксксхдпи/монкэйфаце.ПНГ**

Им было предоставлено **действие сборки** **AndroidResource**.

В проекте UWP точечные рисунки можно хранить в любом месте проекта, но они обычно хранятся в пользовательской папке или в существующей папке **Assets** . Проект UWP содержит следующие растровые изображения:

- Точечный рисунок размером 48 пикселей, хранящийся как **/Assets/MonkeyFace.Scale-100.png**
- Точечный рисунок размером 96 пикселей, хранящийся как **/Assets/MonkeyFace.Scale-200.png**
- Точечный рисунок размером 192 пикселей, хранящийся как **/Assets/MonkeyFace.Scale-400.png**

Всем им было предоставлено **действие сборки** **содержимого**.

Можно указать, как свойства `Text` и `ImageSource` упорядочиваются в `Button` с помощью свойства [`ContentLayout`](xref:Xamarin.Forms.Button.ContentLayout) `Button`. Это свойство имеет тип [`ButtonContentLayout`](xref:Xamarin.Forms.Button.ButtonContentLayout), который является внедренным классом в `Button`. [Конструктор](xref:Xamarin.Forms.Button.ButtonContentLayout.%23ctor(Xamarin.Forms.Button.ButtonContentLayout.ImagePosition,System.Double)) имеет два аргумента:

- Член перечисления [`ImagePosition`](xref:Xamarin.Forms.Button.ButtonContentLayout.ImagePosition) : `Left`, `Top`, `Right` или `Bottom`, указывающее, как точечный рисунок отображается относительно текста.
- Значение `double` интервала между точечным рисунком и текстом.

Значения по умолчанию: `Left` и 10 единиц. Два свойства только для чтения `ButtonContentLayout` с именами [`Position`](xref:Xamarin.Forms.Button.ButtonContentLayout.Position) и [`Spacing`](xref:Xamarin.Forms.Button.ButtonContentLayout.Spacing) предоставляют значения этих свойств.

В коде можно создать `Button` и задать свойство `ContentLayout` следующим образом:

```csharp
Button button = new Button
{
    Text = "button text",
    ImageSource = new FileImageSource
    {
        File = "image filename"
    },
    ContentLayout = new Button.ButtonContentLayout(Button.ButtonContentLayout.ImagePosition.Right, 20)
};
```

В XAML необходимо указать только член перечисления, пробелы или оба значения в любом порядке, разделенном запятыми:

```xaml
<Button Text="button text"
        ImageSource="image filename"
        ContentLayout="Right, 20" />
```

На странице **Демонстрационная Страница с изображением** используется `OnPlatform`, чтобы указать разные имена файлов для точечных рисунков iOS, Android и UWP. Если вы хотите использовать одно и то же имя файла для каждой платформы и не используете `OnPlatform`, необходимо сохранить точечные рисунки UWP в корневом каталоге проекта.

Первая `Button` на странице **демонстрационная страница изображения** задает свойство `Image`, но не свойство `Text`:

```xaml
<Button>
    <Button.ImageSource>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="MonkeyFace.png" />
            <On Platform="UWP" Value="Assets/MonkeyFace.png" />
        </OnPlatform>
    </Button.ImageSource>
</Button>
```

Если точечные рисунки UWP хранятся в корневом каталоге проекта, эта разметка может значительно упроститься:

```xaml
<Button ImageSource="MonkeyFace.png" />
```

Чтобы избежать большого объема разметки какой в файле **имажебуттондемо. XAML** , также определяется неявная `Style` для установки свойства `ImageSource`. Этот `Style` автоматически применяется к пяти другим элементам `Button`. Вот полный XAML файл:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.ImageButtonDemoPage">

    <FlexLayout Direction="Column"
                JustifyContent="SpaceEvenly"
                AlignItems="Center">

        <FlexLayout.Resources>
            <Style TargetType="Button">
                <Setter Property="ImageSource">
                    <OnPlatform x:TypeArguments="ImageSource">
                        <On Platform="iOS, Android" Value="MonkeyFace.png" />
                        <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                    </OnPlatform>
                </Setter>
            </Style>
        </FlexLayout.Resources>

        <Button>
            <Button.ImageSource>
                <OnPlatform x:TypeArguments="ImageSource">
                    <On Platform="iOS, Android" Value="MonkeyFace.png" />
                    <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                </OnPlatform>
            </Button.ImageSource>
        </Button>

        <Button Text="Default" />

        <Button Text="Left - 10"
                ContentLayout="Left, 10" />

        <Button Text="Top - 10"
                ContentLayout="Top, 10" />

        <Button Text="Right - 20"
                ContentLayout="Right, 20" />

        <Button Text="Bottom - 20"
                ContentLayout="Bottom, 20" />
    </FlexLayout>
</ContentPage>
```

Последние четыре элемента `Button` используют свойство `ContentLayout` для указания расположения и расстояния текста и точечного рисунка:

[![Демонстрация кнопки изображения](button-images/ImageButtonDemo.png "Демонстрация кнопки изображения")](button-images/ImageButtonDemo-Large.png#lightbox)

Теперь вы узнали о различных способах, с помощью которых можно выполнять `Button` события и изменять внешний вид `Button`.

## <a name="related-links"></a>Связанные ссылки

- [Пример Буттондемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)
- [API Button](xref:Xamarin.Forms.Button)
