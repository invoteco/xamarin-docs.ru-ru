---
title: Xamarin.Forms в проектах Xamarin Native
description: В этой статье объясняется, как использовать производные ContentPage страниц, которые напрямую добавляются в собственные проекты Xamarin и как перемещаться между ними.
ms.prod: xamarin
ms.assetid: f343fc21-dfb1-4364-a332-9da6705d36bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/03/2019
ms.openlocfilehash: f0ad4e3271ac8c1f8d30a0440b38d8a46c57783e
ms.sourcegitcommit: b4a12607ca944de10fd166139765241a4501831c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2019
ms.locfileid: "66687116"
---
# <a name="xamarinforms-in-xamarin-native-projects"></a>Xamarin.Forms в проектах Xamarin Native

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://developer.xamarin.com/samples/xamarin-forms/Native2Forms/)

Как правило, приложения Xamarin.Forms включает в себя одну или несколько страниц, которые являются производными от [ `ContentPage` ](xref:Xamarin.Forms.ContentPage), и эти страницы являются общими для всех платформ в проекте библиотеки .NET Standard или общий проект. Тем не менее, исходные формы позволяет `ContentPage`-производным страницы, чтобы добавить непосредственно в собственных приложений Xamarin.iOS, Xamarin.Android и универсальной платформы Windows. По сравнению с необходимости использовать собственный проект `ContentPage`-производных страниц из проекта библиотеки .NET Standard или общий проект, добавление страниц проектов в машинном коде преимущество — что страниц можно расширить с помощью собственного представления. Исходные представления затем можно присвоить имя в XAML с `x:Name` и на которые имеются ссылки из кода. Дополнительные сведения о собственном представлениях см. в разделе [исходные представления](~/xamarin-forms/platform/native-views/index.md).

Процесс получения Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-производных от page в собственный проект выглядит следующим образом:

1. Добавьте пакет Xamarin.Forms NuGet в собственный проект.
1. Добавить [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-производных страницы и зависимые компоненты в собственный проект.
1. Вызовите метод `Forms.Init`.
1. Сконструировать экземпляр [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-страница производной и преобразовать его в соответствующий собственный тип, с помощью одного из следующих методов расширения: `CreateViewController` для iOS, `CreateSupportFragment` для Android, или `CreateFrameworkElement` для UWP.
1. Перейдите в представление собственного типа [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-страница, с помощью собственного API-навигации производной.

Xamarin.Forms должны инициализироваться, вызвав `Forms.Init` метод, прежде чем можно создать собственный проект [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-страница производной. Выбор места для этого в основном зависит от времени наиболее удобное в блок-схеме приложения — может быть выполнена при запуске приложения или непосредственно перед `ContentPage`-создается производный страницы. В этой статье и сопутствующие примеры приложений `Forms.Init` метод вызывается при запуске приложения.

> [!NOTE]
> **NativeForms** примера приложения решения не содержит все проекты Xamarin.Forms. Вместо этого он состоит из проекта Xamarin.iOS, проект Xamarin.Android и проекта универсальной платформы Windows. Каждый проект имеет собственный проект, использующий собственные формы для использования [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-производным страниц. Тем не менее, нет причин, почему не могли использовать собственные проекты `ContentPage`-страниц на основе проекта библиотеки .NET Standard или общий проект.

Если вы используете собственные формы, Xamarin.Forms такие функции, как [ `DependencyService` ](xref:Xamarin.Forms.DependencyService), [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter)и механизм привязки данных, все по-прежнему работали. Тем не менее Навигация по страницам выполняется с помощью собственного API-навигации.

## <a name="ios"></a>iOS

В iOS `FinishedLaunching` в Переопределите `AppDelegate` класса обычно является местом для выполнения приложения задач, связанных с запуска. Он вызывается после запуска и приложения обычно переопределяется, чтобы настроить главное окно и просмотреть контроллера. В следующем коде показано в примере `AppDelegate` класс в приложении-примере:

```csharp
[Register("AppDelegate")]
public class AppDelegate : UIApplicationDelegate
{
    public static string FolderPath { get; private set; }

    public static AppDelegate Instance;

    UIWindow _window;
    UINavigationController _navigation;

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        Forms.Init();

        Instance = this;
        _window = new UIWindow(UIScreen.MainScreen.Bounds);

        UINavigationBar.Appearance.SetTitleTextAttributes(new UITextAttributes
        {
            TextColor = UIColor.Black
        });

        FolderPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData));
        UIViewController mainPage = new NotesPage().CreateViewController();
        mainPage.Title = "Notes";

        _navigation = new UINavigationController(mainPage);
        _window.RootViewController = _navigation;
        _window.MakeKeyAndVisible();

        return true;
    }
    ...
}
```

`FinishedLaunching` Метод выполняет следующие задачи:

- Xamarin.Forms инициализируется путем вызова `Forms.Init` метод.
- Ссылку на `AppDelegate` класса хранится в `static` `Instance` поля. Это позволяет предоставить механизм для других классов для вызова методов, определенных в `AppDelegate` класса.
- `UIWindow`, Который является основной контейнер для представлений в собственные приложения iOS, создается.
- `FolderPath` Путь на устройстве, где будут храниться данные Примечание инициализируется свойство.
- `NotesPage` Класс, являющийся Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-производным страницы, определенные в XAML, создается и преобразовать `UIViewController` с помощью `CreateViewController` метода расширения.
- `Title` Свойство `UIViewController` имеет значение, отображаемые на `UINavigationBar`.
- Объект `UINavigationController` создается для управления иерархической навигации. `UINavigationController` Класс управляет стеком контроллеров представлений и `UIViewController` передан в конструктор отображается изначально при `UINavigationController` загружается.
- `UINavigationController` Экземпляр задан в качестве верхнего уровня `UIViewController` для `UIWindow`и `UIWindow` задан в качестве ключа окна для приложения и является видимым.

Один раз `FinishedLaunching` успешного выполнения метода, определенного пользовательского интерфейса в Xamarin.Forms `NotesPage` класс будет отображаться, как показано на следующем снимке экрана:

[![Снимок экрана приложения Xamarin.iOS, которое использует пользовательский Интерфейс определен в XAML](native-forms-images/ios-notespage.png "приложение Xamarin.iOS с пользовательским Интерфейсом XAML")](native-forms-images/ios-notespage-large.png#lightbox "приложения Xamarin.iOS с помощью пользовательского интерфейса XAML")

Взаимодействие с пользовательским Интерфейсом, например, коснувшись **+** [ `Button` ](xref:Xamarin.Forms.Button), приведет к следующий обработчик событий в `NotesPage` выполнения кода:

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    AppDelegate.Instance.NavigateToNoteEntryPage(new Note());
}
```

`static` `AppDelegate.Instance` Поле позволяет `AppDelegate.NavigateToNoteEntryPage` метод должен быть вызван, как показано в следующем примере кода:

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    UIViewController noteEntryPage = new NoteEntryPage
    {
        BindingContext = note
    }.CreateViewController();
    noteEntryPage.Title = "Note Entry";
    _navigation.PushViewController(noteEntryPage, true);
}
```

`NavigateToNoteEntryPage` Метод преобразует Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-страница для производной `UIViewController` с `CreateViewController` метод расширения и наборы `Title` свойство `UIViewController`. `UIViewController` Затем помещается в `UINavigationController` по `PushViewController` метод. Таким образом, пользовательский Интерфейс определен в Xamarin.Forms `NoteEntryPage` класс будет отображаться, как показано на следующем снимке экрана:

[![Снимок экрана приложения Xamarin.iOS, которое использует пользовательский Интерфейс определен в XAML](native-forms-images/ios-noteentrypage.png "приложение Xamarin.iOS с пользовательским Интерфейсом XAML")](native-forms-images/ios-noteentrypage-large.png#lightbox "приложения Xamarin.iOS с помощью пользовательского интерфейса XAML")

При `NoteEntryPage` отображается, коснувшись обратной стрелки появится всплывающее `UIViewController` для `NoteEntryPage` класса из `UINavigationController`, возвращая пользователю `UIViewController` для `NotesPage` класса.

## <a name="android"></a>Android

В Android `OnCreate` в Переопределите `MainActivity` класса обычно является местом для выполнения приложения задач, связанных с запуска. В следующем коде показано в примере `MainActivity` класс в приложении-примере:

```csharp
public class MainActivity : AppCompatActivity
{
    public static string FolderPath { get; private set; }

    public static MainActivity Instance;

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);

        Forms.Init(this, bundle);
        Instance = this;

        SetContentView(Resource.Layout.Main);
        var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
        SetSupportActionBar(toolbar);
        SupportActionBar.Title = "Notes";

        FolderPath = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.LocalApplicationData));
        Android.Support.V4.App.Fragment mainPage = new NotesPage().CreateSupportFragment(this);
        SupportFragmentManager
            .BeginTransaction()
            .Replace(Resource.Id.fragment_frame_layout, mainPage)
            .Commit();
        ...
    }
    ...
}
```

`OnCreate` Метод выполняет следующие задачи:

- Xamarin.Forms инициализируется путем вызова `Forms.Init` метод.
- Ссылку на `MainActivity` класса хранится в `static` `Instance` поля. Это позволяет предоставить механизм для других классов для вызова методов, определенных в `MainActivity` класса.
- `Activity` Содержимое устанавливается из ресурса макета. В приложении-примере макет состоит из `LinearLayout` , содержащий `Toolbar`и `FrameLayout` в качестве контейнера фрагмента.
- `Toolbar` Извлекается и задать в качестве панели действий для `Activity`, и задайте заголовок панели действий.
- `FolderPath` Путь на устройстве, где будут храниться данные Примечание инициализируется свойство.
- `NotesPage` Класс, являющийся Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-производным страницы, определенные в XAML, создается и преобразовать `Fragment` с помощью `CreateSupportFragment` метода расширения.
- `SupportFragmentManager` Класс создает и фиксирует транзакцию, которая заменяет `FrameLayout` с экземпляром `Fragment` для `NotesPage` класса.

Дополнительные сведения о фрагментах см. в разделе [фрагментов](~/android/platform/fragments/index.md).

Один раз `OnCreate` успешного выполнения метода, определенного пользовательского интерфейса в Xamarin.Forms `NotesPage` класс будет отображаться, как показано на следующем снимке экрана:

[![Снимок экрана приложения Xamarin.Android, которое использует пользовательский Интерфейс определен в XAML](native-forms-images/android-notespage.png "приложение Xamarin.Android с пользовательским Интерфейсом XAML")](native-forms-images/android-notespage-large.png#lightbox "приложение Xamarin.Android с помощью пользовательского интерфейса XAML")

Взаимодействие с пользовательским Интерфейсом, например, коснувшись **+** [ `Button` ](xref:Xamarin.Forms.Button), приведет к следующий обработчик событий в `NotesPage` выполнения кода:

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    MainActivity.Instance.NavigateToNoteEntryPage(new Note());
}
```

`static` `MainActivity.Instance` Поле позволяет `MainActivity.NavigateToNoteEntryyPage` метод должен быть вызван, как показано в следующем примере кода:

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    Android.Support.V4.App.Fragment noteEntryPage = new NoteEntryPage
    {
        BindingContext = note
    }.CreateSupportFragment(this);
    SupportFragmentManager
        .BeginTransaction()
        .AddToBackStack(null)
        .Replace(Resource.Id.fragment_frame_layout, noteEntryPage)
        .Commit();
}
```

`NavigateToNoteEntryPage` Метод преобразует Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-страница для производной `Fragment` с `CreateSupportFragment` метод расширения и добавляет `Fragment` в тот фрагмент обратно в стек. Таким образом, пользовательский Интерфейс определен в Xamarin.Forms `NoteEntryPage` будет отображаться, как показано на следующем снимке экрана:

[![Снимок экрана приложения Xamarin.Android, которое использует пользовательский Интерфейс определен в XAML](native-forms-images/android-noteentrypage.png "приложение Xamarin.Android с пользовательским Интерфейсом XAML")](native-forms-images/android-noteentrypage-large.png#lightbox "приложение Xamarin.Android с помощью пользовательского интерфейса XAML")

При `NoteEntryPage` отображается, коснувшись обратной стрелки появится всплывающее `Fragment` для `NoteEntryPage` из фрагмента стек переходов назад, возвращая пользователю `Fragment` для `NotesPage` класса.

### <a name="enable-back-navigation-support"></a>Включить поддержку переходов назад

`SupportFragmentManager` Класс имеет `BackStackChanged` события, возникающего при каждом изменении содержимого фрагмента стек переходов назад. `OnCreate` Метод в `MainActivity` класс содержит анонимный обработчик для этого события:

```csharp
SupportFragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = SupportFragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Note Entry" : "Notes";
};
```

Этот обработчик событий отображает кнопки "Назад" на панели действий, при условии, что имеется один или несколько `Fragment` свернет во фрагменте стека. Ответ при нажатии кнопки "Назад", обрабатывается `OnOptionsItemSelected` переопределить:

```csharp
public override bool OnOptionsItemSelected(Android.Views.IMenuItem item)
{
    if (item.ItemId == global::Android.Resource.Id.Home && SupportFragmentManager.BackStackEntryCount > 0)
    {
        SupportFragmentManager.PopBackStack();
        return true;
    }
    return base.OnOptionsItemSelected(item);
}
```

`OnOptionsItemSelected` Переопределение вызывается каждый раз при выборе элемента в меню "Параметры". Эта реализация извлекает текущий фрагмент из фрагмента стек переходов назад, если был выбран "Назад", а также есть один или несколько `Fragment` свернет во фрагменте стека.

### <a name="multiple-activities"></a>Несколько действий

Если приложение состоит из нескольких действий [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-производных страниц могут быть внедрены в каждое из действий. В этом случае `Forms.Init` метод должны вызываться только в `OnCreate` переопределить первого `Activity` , внедряет Xamarin.Forms `ContentPage`. Тем не менее это имеет следующие последствия:

- Значение `Xamarin.Forms.Color.Accent` будет взято из `Activity` вызвавшую `Forms.Init` метод.
- Значение `Xamarin.Forms.Application.Current` будут связаны с `Activity` вызвавшую `Forms.Init` метод.

### <a name="choose-a-file"></a>Выберите файл

При внедрении [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-производным страницы, использующей [ `WebView` ](xref:Xamarin.Forms.WebView) , требуется для поддержки HTML «Выбрать файл» кнопки, `Activity` необходимо переопределить `OnActivityResult` метод:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    ActivityResultCallbackRegistry.InvokeCallback(requestCode, resultCode, data);
}
```

## <a name="uwp"></a>UWP

На UWP, собственного `App` класса обычно является местом для выполнения приложения задач, связанных с запуска. Xamarin.Forms обычно инициализируется, в приложениях универсальной платформы Windows в `OnLaunched` переопределить в собственной `App` класс, для передачи `LaunchActivatedEventArgs` аргумент `Forms.Init` метод. По этой причине собственных приложений универсальной платформы Windows, которые используют Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-производных от page, проще всего можно вызвать `Forms.Init` метода из `App.OnLaunched` метод.

По умолчанию, собственного `App` класса запускает `MainPage` класс как первая страница приложения. В следующем коде показано в примере `MainPage` класс в приложении-примере:

```csharp
public sealed partial class MainPage : Page
{
    public static MainPage Instance;

    public static string FolderPath { get; private set; }

    public MainPage()
    {
        this.InitializeComponent();
        this.NavigationCacheMode = NavigationCacheMode.Enabled;
        Instance = this;
        FolderPath = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.LocalApplicationData));
        this.Content = new Notes.UWP.Views.NotesPage().CreateFrameworkElement();
    }
    ...
}
```

`MainPage` Конструктор выполняет следующие задачи:

- На странице включено кэширование, чтобы новый `MainPage` не создается, когда пользователь переходит на страницу.
- Ссылку на `MainPage` класса хранится в `static` `Instance` поля. Это позволяет предоставить механизм для других классов для вызова методов, определенных в `MainPage` класса.
- `FolderPath` Путь на устройстве, где будут храниться данные Примечание инициализируется свойство.
- `NotesPage` Класс, являющийся Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-производным страницы, определенные в XAML, создается и преобразовать `FrameworkElement` с помощью `CreateFrameworkElement` метод расширения, а затем задайте как содержимое `MainPage` класса.

Один раз `MainPage` конструктор выполнен, пользовательский Интерфейс определен в Xamarin.Forms `NotesPage` класс будет отображаться, как показано на следующем снимке экрана:

[![Снимок экрана приложения универсальной платформы Windows, которое использует пользовательский Интерфейс определен с XAML Xamarin.Forms](native-forms-images/uwp-notespage.png "приложения универсальной платформы Windows с пользовательским Интерфейсом XAML Xamarin.Forms")](native-forms-images/uwp-notespage-large.png#lightbox "приложения универсальной платформы Windows с пользовательским Интерфейсом XAML Xamarin.Forms")

Взаимодействие с пользовательским Интерфейсом, например, коснувшись **+** [ `Button` ](xref:Xamarin.Forms.Button), приведет к следующий обработчик событий в `NotesPage` выполнения кода:

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    MainPage.Instance.NavigateToNoteEntryPage(new Note());
}
```

`static` `MainPage.Instance` Поле позволяет `MainPage.NavigateToNoteEntryPage` метод должен быть вызван, как показано в следующем примере кода:

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    this.Frame.Navigate(new NoteEntryPage
    {
        BindingContext = note
    });
}
```

Навигация в универсальной платформы Windows обычно выполняется с помощью `Frame.Navigate` метод, который принимает `Page` аргумент. Xamarin.Forms определяет `Frame.Navigate` метод расширения, который принимает [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-производный экземпляр страницы. Таким образом, когда `NavigateToNoteEntryPage` выполняет метод, пользовательский Интерфейс, определенный в Xamarin.Forms `NoteEntryPage` будет отображаться, как показано на следующем снимке экрана:

[![Снимок экрана приложения универсальной платформы Windows, которое использует пользовательский Интерфейс определен с XAML Xamarin.Forms](native-forms-images/uwp-noteentrypage.png "приложения универсальной платформы Windows с пользовательским Интерфейсом XAML Xamarin.Forms")](native-forms-images/uwp-noteentrypage-large.png#lightbox "приложения универсальной платформы Windows с пользовательским Интерфейсом XAML Xamarin.Forms")

При `NoteEntryPage` отображается, коснувшись обратной стрелки появится всплывающее `FrameworkElement` для `NoteEntryPage` из стек переходов назад в приложении, возвращая пользователю `FrameworkElement` для `NotesPage` класса.

### <a name="enable-back-navigation-support"></a>Включить поддержку переходов назад

На универсальной платформы Windows приложения необходимо включить переходов назад для всех оборудования и программного обеспечения назад кнопок, на другое устройство конструктивными. Это можно сделать, зарегистрировав обработчик событий для `BackRequested` событие, которое может быть выполнена в `OnLaunched` метод в собственной `App` класса:

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    if (rootFrame == null)
    {
        ...      
        // Place the frame in the current Window
        Window.Current.Content = rootFrame;

        SystemNavigationManager.GetForCurrentView().BackRequested += OnBackRequested;
    }
    ...
}
```

При запуске приложения, `GetForCurrentView` метод извлекает `SystemNavigationManager` объект связанный с текущим представлением, а затем регистрирует обработчик событий для `BackRequested` событий. Приложение получает это событие, только если его активным, а в ответ, вызывает `OnBackRequested` обработчик событий:

```csharp
void OnBackRequested(object sender, BackRequestedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        e.Handled = true;
        rootFrame.GoBack();
    }
}
```

`OnBackRequested` Вызовов обработчика событий `GoBack` метод корневого фрейма приложения и наборы `BackRequestedEventArgs.Handled` свойства `true` пометить событие как обработанное. Пометить событие как обработанное может появиться в событии обрабатываются.

Приложение выбирает, нужно ли отображать кнопки "Назад" в строке заголовка. Это достигается путем установки `AppViewBackButtonVisibility` задается одно из `AppViewBackButtonVisibility` значений перечисления:

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

`OnNavigated` Обработчик событий, который выполняется в ответ на `Navigated` события, обновляет видимость заголовка окна кнопки "Назад", когда происходит переход по страницам. Это гарантирует, что Назад кнопка панели заголовка является видимым, если стек переходов назад в приложении не является пустым или удалены из заголовка, если стек переходов назад в приложении является пустым.

Дополнительные сведения о поддержке переходов назад на UWP, см. в разделе [истории переходов и обратной навигации для приложений UWP](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/).

## <a name="related-links"></a>Связанные ссылки

- [NativeForms (пример)](https://developer.xamarin.com/samples/xamarin-forms/Native2Forms/)
- [Исходные представления](~/xamarin-forms/platform/native-views/index.md)
