---
title: Собственные представления в XAML
description: Исходные представления из iOS, Android и универсальной платформы Windows можно непосредственно ссылаться из файлов XAML Xamarin.Forms. На исходные представления можно задать свойства и обработчики событий, и они могут взаимодействовать с представления Xamarin.Forms. В этой статье показано, как использовать исходные представления из файлов XAML Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 7A856D31-B300-409E-9AEB-F8A4DB99B37E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2019
ms.openlocfilehash: 6d3954f44cd769ed02535eb260b9952e81e67c98
ms.sourcegitcommit: d83c6af42ed26947aa7c0ecfce00b9ef60f33319
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/25/2020
ms.locfileid: "80247630"
---
# <a name="native-views-in-xaml"></a>Собственные представления в XAML

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeswitch)

_На собственные представления из iOS, Android и универсальная платформа Windows можно напрямую ссылаться из файлов XAML в Xamarin. Forms. Свойства и обработчики событий могут быть установлены для собственных представлений и могут взаимодействовать с представлениями Xamarin. Forms. В этой статье показано, как использовать собственные представления из XAML файлов Xamarin. Forms._

В этой статье рассматриваются следующие темы.

- [Использование собственных представлений](#consuming) — процесс использования собственного представления из XAML.
- [Использование собственных привязок](#native_bindings) — привязка данных к свойствам собственных представлений и обратно.
- [Передача аргументов в собственные представления](#passing_arguments) — передача аргументов в собственные конструкторы представлений и вызов собственных методов собственного представления.
- [Ссылки на собственные представления из кода](#native_view_code) — извлечение экземпляров собственного представления, объявленных в файле XAML, из его файла кода программной части.
- [Подклассировать собственные представления](#subclassing) — подклассировать собственные представления для определения интерфейса API, удобного для XAML.  

<a name="overview" />

## <a name="overview"></a>Обзор

Внедрение нативных представлений в файл XAML Xamarin.Forms:

1. Добавьте объявление пространства имен `xmlns` в файл XAML для пространства имен, содержащего собственное представление.
1. Создайте экземпляр собственного представления в файле XAML.

> [!IMPORTANT]
> Скомпилированный XAML должен быть отключен для всех страниц XAML, использующих собственные представления. Это можно сделать, добавив класс кода программной части для страницы XAML с помощью атрибута `[XamlCompilation(XamlCompilationOptions.Skip)]`. Дополнительные сведения о компиляции XAML см. [в разделе Компиляция XAML в Xamarin. Forms](~/xamarin-forms/xaml/xamlc.md).

Чтобы сослаться на собственного представления из файла кода, необходимо использовать общий ресурс проекта (SAP) и поместите код конкретной платформы с помощью директив условной компиляции. Дополнительные сведения см. [в разделе ссылки на собственные представления из кода](#native_view_code).

<a name="consuming" />

## <a name="consuming-native-views"></a>Использование собственного представления

В следующем примере кода показано использование собственных представлений для каждой платформы в [`ContentPage`](xref:Xamarin.Forms.ContentPage)Xamarin. Forms:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        x:Class="NativeViews.NativeViewDemo">
    <StackLayout Margin="20">
        <ios:UILabel Text="Hello World" TextColor="{x:Static ios:UIColor.Red}" View.HorizontalOptions="Start" />
        <androidWidget:TextView Text="Hello World" x:Arguments="{x:Static androidLocal:MainActivity.Instance}" />
        <win:TextBlock Text="Hello World" />
    </StackLayout>
</ContentPage>
```

Кроме того, чтобы указать `clr-namespace` и `assembly` для пространства имен собственного представления, необходимо также указать `targetPlatform`. Он должен иметь значение `iOS`, `Android`, `UWP`, `Windows` (что эквивалентно `UWP`), `macOS`, `GTK`, `Tizen`или `WPF`. Во время выполнения средство синтаксического анализа XAML игнорирует все префиксы пространства имен XML, у которых есть `targetPlatform`, который не соответствует платформе, на которой выполняется приложение.

Каждое объявление пространства имен можно использовать для ссылки любого класса или структуры из указанного пространства имен. Например, объявление пространства имен `ios` может использоваться для ссылки на любой класс или структуру из пространства имен `UIKit` iOS. Можно задать свойства собственного представления с помощью XAML, но типы свойств и объектов должны совпадать. Например, свойство `UILabel.TextColor` имеет значение `UIColor.Red` с помощью расширения разметки `x:Static` и пространства имен `ios`.

Привязываемые свойства и присоединенные привязываемые свойства также могут быть установлены для собственных представлений с помощью синтаксиса `Class.BindableProperty="value"`. Каждое собственное представление упаковывается в экземпляр `NativeViewWrapper`, зависящий от платформы, который является производным от класса [`Xamarin.Forms.View`](xref:Xamarin.Forms.View) . Установка может быть привязано или присоединенное свойство, используемое для собственного представления передает значение свойства оболочки. Например, можно задать горизонтальный макет по центру, задав `View.HorizontalOptions="Center"` в собственном представлении.

> [!NOTE]
> Обратите внимание, что стили нельзя использовать с собственными представлениями, так как стили могут указывать только свойства, которые поддерживаются `BindableProperty` объектами.

Конструкторам мини-приложений Android обычно требуется объект `Context` Android в качестве аргумента, и это можно сделать доступным через статическое свойство в классе `MainActivity`. Поэтому при создании мини-приложения Android в XAML объект `Context` должен быть обычно передан в конструктор мини-приложения с помощью атрибута `x:Arguments` с расширением разметки `x:Static`. Дополнительные сведения см. [в разделе Передача аргументов в собственные представления](#passing_arguments).

> [!NOTE]
> Обратите внимание, что именование собственного представления с `x:Name` невозможно в проекте библиотеки .NET Standard или в проекте общего ресурса (SAP). Это приведет к созданию переменной собственный тип, который приведет к ошибке компиляции. Однако собственные представления можно упаковывать в `ContentView` экземпляры и получать в файле кода программной части при условии, что используется SAP. Дополнительные сведения см. в разделе [ссылка на собственное представление из кода](#native_view_code).

<a name="native_bindings" />

## <a name="native-bindings"></a>Собственные привязки

Привязка данных используется для синхронизации пользовательского интерфейса со своим источником данных и упрощает как приложения Xamarin.Forms отображает и взаимодействует с данными. При условии, что исходный объект реализует интерфейс `INotifyPropertyChanged`, изменения в *исходном* объекте автоматически отправляются в *целевой* объект платформой привязки, а изменения в *целевом* объекте могут быть переданы в *Исходный* объект.

Исходные представления свойств также можно использовать привязку данных. В следующем примере кода демонстрируется привязка данных, с помощью свойств класса исходные представления:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeSwitch"
        x:Class="NativeSwitch.NativeSwitchPage">
    <StackLayout Margin="20">
        <Label Text="Native Views Demo" FontAttributes="Bold" HorizontalOptions="Center" />
        <Entry Placeholder="This Entry is bound to the native switch" IsEnabled="{Binding IsSwitchOn}" />
        <ios:UISwitch On="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=ValueChanged}"
            OnTintColor="{x:Static ios:UIColor.Red}"
            ThumbTintColor="{x:Static ios:UIColor.Blue}" />
        <androidWidget:Switch x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
            Checked="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=CheckedChange}"
            Text="Enable Entry?" />
        <win:ToggleSwitch Header="Enable Entry?"
            OffContent="No"
            OnContent="Yes"
            IsOn="{Binding IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=Toggled}" />
    </StackLayout>
</ContentPage>

```

Страница содержит [`Entry`](xref:Xamarin.Forms.Entry) , свойство [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) которого привязано к свойству `NativeSwitchPageViewModel.IsSwitchOn`. [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) страницы задается новый экземпляр класса `NativeSwitchPageViewModel` в файле кода программной части с классом ViewModel, реализующим интерфейс `INotifyPropertyChanged`.

Страница также содержит встроенного коммутатора для каждой платформы. Каждый собственный коммутатор использует привязку [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) для обновления значения свойства `NativeSwitchPageViewModel.IsSwitchOn`. Таким образом, если параметр выключен, `Entry` отключен, а при включенном параметре `Entry` включается. На следующих снимках экрана показано эту функцию на каждой платформе:

![](xaml-images/native-switch-disabled.png "Native Switch Disabled")
![](xaml-images/native-switch-enabled.png "Native Switch Enabled")

Двусторонние привязки поддерживаются автоматически при условии, что собственное свойство реализует `INotifyPropertyChanged`или поддерживает наблюдение по значению ключа (кво) в iOS или является `DependencyProperty`ом в UWP. Тем не менее многие собственного представления не поддерживают уведомления об изменении свойства. Для этих представлений можно указать значение свойства [`UpdateSourceEventName`](xref:Xamarin.Forms.Binding.UpdateSourceEventName) как часть выражения привязки. Это свойство должно быть присвоено имя события в собственном представлении, сигнализирующее о при изменении целевого свойства. Затем при изменении значения собственного параметра класс `Binding` уведомляется о том, что пользователь изменил значение переключателя, а значение свойства `NativeSwitchPageViewModel.IsSwitchOn` обновляется.

<a name="passing_arguments" />

## <a name="passing-arguments-to-native-views"></a>Передача аргументов исходные представления

Аргументы конструктора можно передать в собственные представления с помощью атрибута `x:Arguments` с расширением разметки `x:Static`. Кроме того, собственные методы фабрики представлений (`public static` методы, которые возвращают объекты или значения того же типа, что и класс или структура, определяющие методы), могут вызываться путем указания имени метода с помощью атрибута `x:FactoryMethod` и его аргументов с помощью атрибута `x:Arguments`.

В следующем примере кода показаны оба способа:

```xaml
<ContentPage ...
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidGraphics="clr-namespace:Android.Graphics;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winMedia="clr-namespace:Windows.UI.Xaml.Media;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winText="clr-namespace:Windows.UI.Text;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winui="clr-namespace:Windows.UI;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows">
        ...
        <ios:UILabel Text="Simple Native Color Picker" View.HorizontalOptions="Center">
            <ios:UILabel.Font>
                <ios:UIFont x:FactoryMethod="FromName">
                    <x:Arguments>
                        <x:String>Papyrus</x:String>
                        <x:Single>24</x:Single>
                    </x:Arguments>
                </ios:UIFont>
            </ios:UILabel.Font>
        </ios:UILabel>
        <androidWidget:TextView x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                    Text="Simple Native Color Picker"
                    TextSize="24"
                    View.HorizontalOptions="Center">
            <androidWidget:TextView.Typeface>
                <androidGraphics:Typeface x:FactoryMethod="Create">
                    <x:Arguments>
                        <x:String>cursive</x:String>
                        <androidGraphics:TypefaceStyle>Normal</androidGraphics:TypefaceStyle>
                    </x:Arguments>
                </androidGraphics:Typeface>
            </androidWidget:TextView.Typeface>
        </androidWidget:TextView>
        <winControls:TextBlock Text="Simple Native Color Picker"
                    FontSize="20"
                    FontStyle="{x:Static winText:FontStyle.Italic}"
                    View.HorizontalOptions="Center">
            <winControls:TextBlock.FontFamily>
                <winMedia:FontFamily>
                    <x:Arguments>
                        <x:String>Georgia</x:String>
                    </x:Arguments>
                </winMedia:FontFamily>
            </winControls:TextBlock.FontFamily>
        </winControls:TextBlock>
        ...
</ContentPage>
```

Метод [`UIFont.FromName`](xref:UIKit.UIFont.FromName*) Factory используется для задания свойства [`UILabel.Font`](xref:UIKit.UILabel.Font) для нового [`UIFont`](xref:UIKit.UIFont) в iOS. Имя и размер `UIFont` задаются аргументами метода, которые являются дочерними для атрибута `x:Arguments`.

Метод [`Typeface.Create`](xref:Android.Graphics.Typeface.Create*) Factory используется для задания свойства [`TextView.Typeface`](xref:Android.Widget.TextView.Typeface) для нового [`Typeface`](xref:Android.Graphics.Typeface) в Android. Имя и стиль семейства `Typeface` задаются аргументами метода, которые являются дочерними для атрибута `x:Arguments`.

Конструктор [`FontFamily`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.fontfamily) используется для присвоения свойству [`TextBlock.FontFamily`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.fontfamily) нового `FontFamily` на универсальная платформа Windows (UWP). Имя `FontFamily` задается аргументом метода, который является дочерним для атрибута `x:Arguments`.

> [!NOTE]
> Аргументы должны совпадать с типами, необходимый для метода, конструктора или фабрики.

На следующих снимках экрана показано результатом указания аргументов метода и конструктора фабрики для задания шрифта на различные собственного представления:

![](xaml-images/passing-arguments.png "Setting Fonts on Native Views")

Дополнительные сведения о передаче аргументов в XAML см. в разделе [Передача аргументов в XAML](~/xamarin-forms/xaml/passing-arguments.md).

<a name="native_view_code" />

## <a name="referring-to-native-views-from-code"></a>Ссылки на исходные представления из кода

Хотя нельзя присвоить собственное представление атрибуту `x:Name`, можно получить экземпляр собственного представления, объявленный в файле XAML, из файла кода программной части в проекте общего доступа, при условии, что собственное представление является дочерним для [`ContentView`](xref:Xamarin.Forms.ContentView) , которое задает значение атрибута `x:Name`. Затем внутри директив условной компиляции в файл с выделенным кодом необходимо сделать следующее.

1. Получите значение свойства [`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content) и приведите его к типу `NativeViewWrapper`, зависящему от платформы.
1. Получите свойство `NativeViewWrapper.NativeElement` и приведите его к типу собственного представления.

Машинный интерфейс API может быть вызвана собственного представления для выполнения требуемой операции. Этот подход также дает преимущество, поскольку несколько собственных представлений XAML для разных платформ могут быть дочерними по отношению к одному и тому же [`ContentView`](xref:Xamarin.Forms.ContentView). В следующем примере кода демонстрируется этот метод:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeViewInsideContentView"
        x:Class="NativeViewInsideContentView.NativeViewInsideContentViewPage">
    <StackLayout Margin="20">
        <ContentView x:Name="contentViewTextParent" HorizontalOptions="Center" VerticalOptions="CenterAndExpand">
            <ios:UILabel Text="Text in a UILabel" TextColor="{x:Static ios:UIColor.Red}" />
            <androidWidget:TextView x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                Text="Text in a TextView" />
              <winControls:TextBlock Text="Text in a TextBlock" />
        </ContentView>
        <ContentView x:Name="contentViewButtonParent" HorizontalOptions="Center" VerticalOptions="EndAndExpand">
            <ios:UIButton TouchUpInside="OnButtonTap" View.HorizontalOptions="Center" View.VerticalOptions="Center" />
            <androidWidget:Button x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                Text="Scale and Rotate Text"
                Click="OnButtonTap" />
            <winControls:Button Content="Scale and Rotate Text" />
        </ContentView>
    </StackLayout>
</ContentPage>
```

В приведенном выше примере собственные представления для каждой платформы являются дочерними для элементов управления [`ContentView`](xref:Xamarin.Forms.ContentView) , при этом значение атрибута `x:Name` используется для получения `ContentView` в коде программной части:

```csharp
public partial class NativeViewInsideContentViewPage : ContentPage
{
    public NativeViewInsideContentViewPage()
    {
        InitializeComponent();

#if __IOS__
        var wrapper = (Xamarin.Forms.Platform.iOS.NativeViewWrapper)contentViewButtonParent.Content;
        var button = (UIKit.UIButton)wrapper.NativeView;
        button.SetTitle("Scale and Rotate Text", UIKit.UIControlState.Normal);
        button.SetTitleColor(UIKit.UIColor.Black, UIKit.UIControlState.Normal);
#endif
#if __ANDROID__
        var wrapper = (Xamarin.Forms.Platform.Android.NativeViewWrapper)contentViewTextParent.Content;
        var textView = (Android.Widget.TextView)wrapper.NativeView;
        textView.SetTextColor(Android.Graphics.Color.Red);
#endif
#if WINDOWS_UWP
        var textWrapper = (Xamarin.Forms.Platform.UWP.NativeViewWrapper)contentViewTextParent.Content;
        var textBlock = (Windows.UI.Xaml.Controls.TextBlock)textWrapper.NativeElement;
        textBlock.Foreground = new Windows.UI.Xaml.Media.SolidColorBrush(Windows.UI.Colors.Red);
        var buttonWrapper = (Xamarin.Forms.Platform.UWP.NativeViewWrapper)contentViewButtonParent.Content;
        var button = (Windows.UI.Xaml.Controls.Button)buttonWrapper.NativeElement;
        button.Click += (sender, args) => OnButtonTap(sender, EventArgs.Empty);
#endif
    }

    async void OnButtonTap(object sender, EventArgs e)
    {
        contentViewButtonParent.Content.IsEnabled = false;
        contentViewTextParent.Content.ScaleTo(2, 2000);
        await contentViewTextParent.Content.RotateTo(360, 2000);
        contentViewTextParent.Content.ScaleTo(1, 2000);
        await contentViewTextParent.Content.RelRotateTo(360, 2000);
        contentViewButtonParent.Content.IsEnabled = true;
    }
}
```

Доступ к свойству [`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content) осуществляется для получения собственного представления с оболочкой в виде экземпляра `NativeViewWrapper`, зависящего от платформы. Затем осуществляется доступ к свойству `NativeViewWrapper.NativeElement` для получения собственного представления в виде собственного типа. API собственного представления вызывается, чтобы выполнить требуемые операции.

Собственные кнопки iOS и Android используют один обработчик событий `OnButtonTap`, поскольку каждая собственная кнопка использует делегат `EventHandler` в ответ на событие касания. Однако универсальная платформа Windows (UWP) использует отдельный `RoutedEventHandler`, который, в свою очередь, потребляет обработчик событий `OnButtonTap` в этом примере. Таким образом, при нажатии на встроенную кнопку обработчик событий `OnButtonTap` выполняет, который масштабирует и поворачивает собственный элемент управления, содержащийся в [`ContentView`](xref:Xamarin.Forms.ContentView) с именем `contentViewTextParent`. Далее на снимках экрана показаны такого развития событий на каждой платформе.

![](xaml-images/contentview.png "ContentView Containing a Native Control")

<a name="subclassing" />

## <a name="subclassing-native-views"></a>Создание подкласса исходные представления

Многие iOS и Android исходные представления не подходят для создания экземпляров в XAML, поскольку они используют методы, а не свойства, для настройки элемента управления. Решением этой проблемы является подкласс исходные представления в оболочках, которые определяют дополнительные API XAML с поддержкой, использует свойства для настройки элемента управления, и события независимо от платформы. Упакованное исходные представления можно поместить в общий ресурс проекта (SAP) и окружено директив условной компиляции или помещены в проекты под конкретные платформы и ссылка из XAML в проекте библиотеки .NET Standard.

В следующем примере кода показано, что страницы Xamarin.Forms, которое использует подкласса исходные представления:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:iosLocal="clr-namespace:SubclassedNativeControls.iOS;assembly=SubclassedNativeControls.iOS;targetPlatform=iOS"
        xmlns:android="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SubclassedNativeControls.Droid;assembly=SubclassedNativeControls.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:SubclassedNativeControls"
        x:Class="SubclassedNativeControls.SubclassedNativeControlsPage">
    <StackLayout Margin="20">
        <Label Text="Subclassed Native Views Demo" FontAttributes="Bold" HorizontalOptions="Center" />
        <StackLayout Orientation="Horizontal">
          <Label Text="You have chosen:" />
          <Label Text="{Binding SelectedFruit}" />      
        </StackLayout>
        <iosLocal:MyUIPickerView ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectedItemChanged}" />
        <androidLocal:MySpinner x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
            ItemsSource="{Binding Fruits}"
            SelectedObject="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=ItemSelected}" />
        <winControls:ComboBox ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectionChanged}" />
    </StackLayout>
</ContentPage>
```

Страница содержит [`Label`](xref:Xamarin.Forms.Label) , отображающая фруктов, выбранную пользователем из собственного элемента управления. `Label` привязывается к свойству `SubclassedNativeControlsPageViewModel.SelectedFruit`. [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) страницы задается новый экземпляр класса `SubclassedNativeControlsPageViewModel` в файле кода программной части с классом ViewModel, реализующим интерфейс `INotifyPropertyChanged`.

Страница также содержит собственного средства выбора представления для каждой платформы. Каждое собственное представление отображает коллекцию фруктам, привязывая свое свойство `ItemSource` к коллекции `SubclassedNativeControlsPageViewModel.Fruits`. Это позволяет пользователю выбрать фруктов, как показано на следующем снимке экрана:

![](xaml-images/sub-classed.png "Subclassed Native Views")

В iOS и Android native выбора использовать методы для настройки элементов управления. Таким образом эти средства выбора должен быть подклассом предоставить доступ к свойствам, чтобы сделать их XAML с поддержкой. На универсальная платформа Windows (UWP) `ComboBox` уже является понятным XAML и поэтому не требует подкласса.

### <a name="ios"></a>iOS

В реализации iOS подклассы [`UIPickerView`](xref:UIKit.UIPickerView) представление, а также предоставляются свойства и событие, которое можно легко использовать из XAML:

```csharp
public class MyUIPickerView : UIPickerView
{
    public event EventHandler<EventArgs> SelectedItemChanged;

    public MyUIPickerView()
    {
        var model = new PickerModel();
        model.ItemChanged += (sender, e) =>
        {
            if (SelectedItemChanged != null)
            {
                SelectedItemChanged.Invoke(this, e);
            }
        };
        Model = model;
    }

    public IList<string> ItemsSource
    {
        get
        {
            var pickerModel = Model as PickerModel;
            return (pickerModel != null) ? pickerModel.Items : null;
        }
        set
        {
            var model = Model as PickerModel;
            if (model != null)
            {
                model.Items = value;
            }
        }
    }

    public string SelectedItem
    {
        get { return (Model as PickerModel).SelectedItem; }
        set { }
    }
}
```

Класс `MyUIPickerView` предоставляет свойства `ItemsSource` и `SelectedItem`, а также событие `SelectedItemChanged`. [`UIPickerView`](xref:UIKit.UIPickerView) требуется базовая модель данных [`UIPickerViewModel`](xref:UIKit.UIPickerViewModel) , доступ к которой осуществляется `MyUIPickerView` свойствами и событиями. Модель данных `UIPickerViewModel` предоставляется классом `PickerModel`:

```csharp
class PickerModel : UIPickerViewModel
{
    int selectedIndex = 0;
    public event EventHandler<EventArgs> ItemChanged;
    public IList<string> Items { get; set; }

    public string SelectedItem
    {
        get
        {
            return Items != null && selectedIndex >= 0 && selectedIndex < Items.Count ? Items[selectedIndex] : null;
        }
    }

    public override nint GetRowsInComponent(UIPickerView pickerView, nint component)
    {
        return Items != null ? Items.Count : 0;
    }

    public override string GetTitle(UIPickerView pickerView, nint row, nint component)
    {
        return Items != null && Items.Count > row ? Items[(int)row] : null;
    }

    public override nint GetComponentCount(UIPickerView pickerView)
    {
        return 1;
    }

    public override void Selected(UIPickerView pickerView, nint row, nint component)
    {
        selectedIndex = (int)row;
        if (ItemChanged != null)
        {
            ItemChanged.Invoke(this, new EventArgs());
        }
    }
}
```

Класс `PickerModel` предоставляет базовое хранилище для класса `MyUIPickerView` с помощью свойства `Items`. При изменении выбранного элемента в `MyUIPickerView` выполняется метод [`Selected`](xref:UIKit.UIPickerViewModel.Selected*) , который обновляет выбранный индекс и запускает событие `ItemChanged`. Это гарантирует, что свойство `SelectedItem` всегда будет возвращать последний элемент, выбранный пользователем. Кроме того, класс `PickerModel` переопределяет методы, используемые для настройки экземпляра `MyUIPickerView`.

### <a name="android"></a>Android

В реализации Android подклассы [`Spinner`](xref:Android.Widget.Spinner) представление, а также предоставляет свойства и событие, которое можно легко использовать из XAML:

```csharp
class MySpinner : Spinner
{
    ArrayAdapter adapter;
    IList<string> items;

    public IList<string> ItemsSource
    {
        get { return items; }
        set
        {
            if (items != value)
            {
                items = value;
                adapter.Clear();

                foreach (string str in items)
                {
                    adapter.Add(str);
                }
            }
        }
    }

    public string SelectedObject
    {
        get { return (string)GetItemAtPosition(SelectedItemPosition); }
        set
        {
            if (items != null)
            {
                int index = items.IndexOf(value);
                if (index != -1)
                {
                    SetSelection(index);
                }
            }
        }
    }

    public MySpinner(Context context) : base(context)
    {
        ItemSelected += OnBindableSpinnerItemSelected;

        adapter = new ArrayAdapter(context, Android.Resource.Layout.SimpleSpinnerItem);
        adapter.SetDropDownViewResource(Android.Resource.Layout.SimpleSpinnerDropDownItem);
        Adapter = adapter;
    }

    void OnBindableSpinnerItemSelected(object sender, ItemSelectedEventArgs args)
    {
        SelectedObject = (string)GetItemAtPosition(args.Position);
    }
}
```

Класс `MySpinner` предоставляет свойства `ItemsSource` и `SelectedObject`, а также событие `ItemSelected`. Элементы, отображаемые классом `MySpinner`, предоставляются [`Adapter`](xref:Android.Widget.Adapter) , связанным с представлением, а элементы заполняются `Adapter` при первом задании свойства `ItemsSource`. При изменении выбранного элемента в классе `MySpinner` обработчик событий `OnBindableSpinnerItemSelected` обновляет свойство `SelectedObject`.

## <a name="summary"></a>Сводка

В этой статье было показано, как использовать исходные представления из файлов XAML Xamarin.Forms. На исходные представления можно задать свойства и обработчики событий, и они могут взаимодействовать с представления Xamarin.Forms.

## <a name="related-links"></a>Связанные ссылки

- [Нативесвитч (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeswitch)
- [Forms2Native (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/forms2native)
- [Нативевиевинсидеконтентвиев (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeviewinsidecontentview)
- [Субкласседнативеконтролс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-subclassednativecontrols)
- [Исходные формы](~/xamarin-forms/platform/native-forms.md)
- [Передача аргументов в XAML](~/xamarin-forms/xaml/passing-arguments.md)
