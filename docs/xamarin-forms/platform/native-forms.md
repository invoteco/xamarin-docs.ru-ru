---
title: Xamarin. Forms в проектах Xamarin Native
description: В этой статье объясняется, как использовать страницы, производные от ContentPage, которые непосредственно добавляются в собственные проекты Xamarin, и как осуществляется переход между ними.
ms.prod: xamarin
ms.assetid: f343fc21-dfb1-4364-a332-9da6705d36bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/19/2019
ms.openlocfilehash: 0c84b844455b8a792b8cbe2f4dac97097e5ebd97
ms.sourcegitcommit: dad4dfcd194b63ec9e903363351b6d9e543d4888
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/18/2019
ms.locfileid: "69621057"
---
# <a name="xamarinforms-in-xamarin-native-projects"></a>Xamarin. Forms в проектах Xamarin Native

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/native2forms)

Как правило, приложение Xamarin. Forms включает одну или несколько страниц, производных от [`ContentPage`](xref:Xamarin.Forms.ContentPage), и эти страницы совместно используются всеми платформами в проекте библиотеки .NET Standard или в общем проекте. Однако собственные формы позволяют добавлять `ContentPage` страницы, которые можно добавить непосредственно в собственные приложения Xamarin. iOS, Xamarin. Android и UWP. По сравнению с тем, что собственный проект использует страницы, производные от `ContentPage`, из проекта библиотеки .NET Standard или из общего проекта, преимущество добавления страниц непосредственно в собственные проекты заключается в том, что страницы можно расширять с помощью собственных представлений. Собственные представления могут называться в XAML с `x:Name` и ссылаться на них из кода программной части. Дополнительные сведения о собственных представлениях см. в разделе [собственные представления](~/xamarin-forms/platform/native-views/index.md).

Процесс использования на странице, производной от Xamarin. Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage)в проекте машинного кода, выглядит следующим образом:

1. Добавьте пакет NuGet Xamarin. Forms в собственный проект.
1. Добавьте в собственный проект страницу, производную от [`ContentPage`](xref:Xamarin.Forms.ContentPage), и все зависимости.
1. Вызовите метод `Forms.Init`.
1. Создайте экземпляр страницы, производной от [`ContentPage`](xref:Xamarin.Forms.ContentPage), и преобразуйте ее в соответствующий собственный тип с помощью одного из следующих методов расширения: `CreateViewController` для iOS, `CreateSupportFragment` для Android или `CreateFrameworkElement` для UWP.
1. Перейдите к представлению собственного типа для страницы, производной от [`ContentPage`](xref:Xamarin.Forms.ContentPage), с помощью собственного API навигации.

Xamarin. Forms необходимо инициализировать, вызвав метод `Forms.Init`, прежде чем собственный проект сможет создать страницу, производную от [`ContentPage`](xref:Xamarin.Forms.ContentPage). Выбор времени выполнения этого действия зависит от того, когда он наиболее удобен в потоке приложения. он может быть выполнен при запуске приложения или непосредственно перед созданием страницы, производной от `ContentPage`. В этой статье и сопутствующих примерах приложений метод `Forms.Init` вызывается при запуске приложения.

> [!NOTE]
> Решение примера приложения **нативеформс** не содержит проектов Xamarin. Forms. Вместо этого он состоит из проекта Xamarin. iOS, проекта Xamarin. Android и проекта UWP. Каждый проект является собственным проектом, который использует собственные формы для использования производных от [`ContentPage`](xref:Xamarin.Forms.ContentPage)страниц. Однако нет причин, по которым в собственных проектах не удалось использовать страницы, производные от `ContentPage`, из проекта библиотеки .NET Standard или из общего проекта.

При использовании собственных форм такие функции Xamarin. Forms, как [`DependencyService`](xref:Xamarin.Forms.DependencyService), [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter)и механизм привязки данных, все еще работают. Однако Навигация по страницам должна выполняться с помощью собственного API навигации.

## <a name="ios"></a>iOS

В iOS переопределение `FinishedLaunching` в классе `AppDelegate` обычно является местом для выполнения задач, связанных с запуском приложения. Он вызывается после запуска приложения и обычно переопределяется для настройки главного окна и контроллера представления. В следующем примере кода показан класс `AppDelegate` в примере приложения:

```csharp
[Register("AppDelegate")]
public class AppDelegate : UIApplicationDelegate
{
    public static AppDelegate Instance;
    UIWindow _window;
    AppNavigationController _navigation;

    public static string FolderPath { get; private set; }

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

        _navigation = new AppNavigationController(mainPage);
        _window.RootViewController = _navigation;
        _window.MakeKeyAndVisible();

        return true;
    }
    // ...
}
```

Метод `FinishedLaunching` выполняет следующие задачи:

- Xamarin. Forms инициализируется путем вызова метода `Forms.Init`.
- Ссылка на класс `AppDelegate` хранится в поле `static` `Instance`. Это необходимо для предоставления другим классам механизма вызова методов, определенных в классе `AppDelegate`.
- Создается `UIWindow`, который является основным контейнером для представлений в собственных приложениях iOS.
- Свойство `FolderPath` инициализируется по пути на устройстве, где будут храниться данные о заметках.
- Класс `NotesPage`, являющийся производной от Xamarin. Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage)страницей, определенной в XAML, создается и преобразуется в `UIViewController` с помощью метода расширения `CreateViewController`.
- Свойство `Title` `UIViewController` задано, которое будет отображаться на `UINavigationBar`.
- Для управления иерархической навигацией создается `AppNavigationController`. Это класс пользовательского контроллера навигации, который является производным от `UINavigationController`. Объект `AppNavigationController` управляет стеком контроллеров представления, и `UIViewController`, переданный в конструктор, будут первоначально представлены при загрузке `AppNavigationController`.
- Объект `AppNavigationController` задается в качестве `UIViewController` верхнего уровня для `UIWindow`, а `UIWindow` задается как ключевое окно для приложения и становится видимым.

После выполнения метода `FinishedLaunching` будет отображаться пользовательский интерфейс, определенный в классе Xamarin. Forms `NotesPage`, как показано на следующем снимке экрана:

[![Снимок экрана приложения Xamarin. iOS, использующего пользовательский интерфейс, определенный в XAML](native-forms-images/ios-notespage.png "Приложение Xamarin. iOS с пользовательским интерфейсом XAML")](native-forms-images/ios-notespage-large.png#lightbox "Приложение Xamarin. iOS с пользовательским интерфейсом XAML")

При взаимодействии с пользовательским интерфейсом, например при нажатии **+** [`Button`](xref:Xamarin.Forms.Button), в `NotesPage` кода программной части будет создан следующий обработчик событий:

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    AppDelegate.Instance.NavigateToNoteEntryPage(new Note());
}
```

Поле `static` `AppDelegate.Instance` позволяет вызывать метод `AppDelegate.NavigateToNoteEntryPage`, который показан в следующем примере кода:

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    var noteEntryPage = new NoteEntryPage
    {
        BindingContext = note
    }.CreateViewController();
    noteEntryPage.Title = "Note Entry";
    _navigation.PushViewController(noteEntryPage, true);
}
```

Метод `NavigateToNoteEntryPage` преобразует страницу, производную от [`ContentPage`](xref:Xamarin.Forms.ContentPage)Xamarin. Forms, в `UIViewController` с помощью метода расширения `CreateViewController` и задает свойство `Title` `UIViewController`. Затем `UIViewController` передается на `AppNavigationController` методом `PushViewController`. Таким образом, Пользовательский интерфейс, определенный в классе Xamarin. Forms `NoteEntryPage`, будет отображаться, как показано на следующем снимке экрана:

[![Снимок экрана приложения Xamarin. iOS, использующего пользовательский интерфейс, определенный в XAML](native-forms-images/ios-noteentrypage.png "Приложение Xamarin. iOS с пользовательским интерфейсом XAML")](native-forms-images/ios-noteentrypage-large.png#lightbox "Приложение Xamarin. iOS с пользовательским интерфейсом XAML")

Когда отображается `NoteEntryPage`, обратная Навигация будет открывать `UIViewController` класса `NoteEntryPage` из `AppNavigationController`, возвращая пользователю `UIViewController` для класса `NotesPage`. Однако извлечение `UIViewController` из собственного стека навигации iOS не приводит к автоматическому удалению `UIViewController` и присоединенного `Page` объекта. Таким образом, класс `AppNavigationController` переопределяет метод `PopViewController`, чтобы удалить контроллеры представления при обратной навигации:

```csharp
public class AppNavigationController : UINavigationController
{
    //...
    public override UIViewController PopViewController(bool animated)
    {
        UIViewController topView = TopViewController;
        if (topView != null)
        {
            // Dispose of ViewController on back navigation.
            topView.Dispose();
        }
        return base.PopViewController(animated);
    }
}
```

Переопределение `PopViewController` вызывает метод `Dispose` для объекта `UIViewController`, извлеченного из собственного стека навигации iOS. Невыполнение этого действия приведет к потере `UIViewController` и присоединенного `Page` объекта.

> [!IMPORTANT]
> Потерянные объекты не могут быть собраны в мусор и поэтому вызывают утечку памяти.

## <a name="android"></a>Android

В Android переопределение `OnCreate` в классе `MainActivity` обычно является местом для выполнения задач, связанных с запуском приложения. В следующем примере кода показан класс `MainActivity` в примере приложения:

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

Метод `OnCreate` выполняет следующие задачи:

- Xamarin. Forms инициализируется путем вызова метода `Forms.Init`.
- Ссылка на класс `MainActivity` хранится в поле `static` `Instance`. Это необходимо для предоставления другим классам механизма вызова методов, определенных в классе `MainActivity`.
- Содержимое `Activity` задается из ресурса макета. В примере приложения макет состоит из `LinearLayout`, содержащего `Toolbar`, и `FrameLayout`, который будет использоваться в качестве контейнера фрагментов.
- @No__t_0 извлекается и задается в качестве панели действий для `Activity`, а также задается заголовок панели действий.
- Свойство `FolderPath` инициализируется по пути на устройстве, где будут храниться данные о заметках.
- Класс `NotesPage`, являющийся производной от Xamarin. Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage)страницей, определенной в XAML, создается и преобразуется в `Fragment` с помощью метода расширения `CreateSupportFragment`.
- Класс `SupportFragmentManager` создает и фиксирует транзакцию, которая заменяет экземпляр `FrameLayout` `Fragment` для класса `NotesPage`.

Дополнительные сведения о фрагментах см. в разделе [фрагменты](~/android/platform/fragments/index.md).

После выполнения метода `OnCreate` будет отображаться пользовательский интерфейс, определенный в классе Xamarin. Forms `NotesPage`, как показано на следующем снимке экрана:

[![Снимок экрана приложения Xamarin. Android, использующего пользовательский интерфейс, определенный в XAML](native-forms-images/android-notespage.png "Приложение Xamarin. Android с пользовательским интерфейсом XAML")](native-forms-images/android-notespage-large.png#lightbox "Приложение Xamarin. Android с пользовательским интерфейсом XAML")

При взаимодействии с пользовательским интерфейсом, например при нажатии **+** [`Button`](xref:Xamarin.Forms.Button), в `NotesPage` кода программной части будет создан следующий обработчик событий:

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    MainActivity.Instance.NavigateToNoteEntryPage(new Note());
}
```

Поле `static` `MainActivity.Instance` позволяет вызывать метод `MainActivity.NavigateToNoteEntryPage`, который показан в следующем примере кода:

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

Метод `NavigateToNoteEntryPage` преобразует страницу, производную от [`ContentPage`](xref:Xamarin.Forms.ContentPage)Xamarin. Forms, в `Fragment` с помощью метода расширения `CreateSupportFragment` и добавляет `Fragment` в стек назад фрагмента. Таким образом, Пользовательский интерфейс, определенный в `NoteEntryPage` Xamarin. Forms, будет отображаться, как показано на следующем снимке экрана:

[![Снимок экрана приложения Xamarin. Android, использующего пользовательский интерфейс, определенный в XAML](native-forms-images/android-noteentrypage.png "Приложение Xamarin. Android с пользовательским интерфейсом XAML")](native-forms-images/android-noteentrypage-large.png#lightbox "Приложение Xamarin. Android с пользовательским интерфейсом XAML")

Когда отображается `NoteEntryPage`, коснитесь стрелки назад, чтобы открыть `Fragment` для `NoteEntryPage` из стека фрагмента, возвращая пользователя в `Fragment` для класса `NotesPage`.

### <a name="enable-back-navigation-support"></a>Включить поддержку обратной навигации

Класс `SupportFragmentManager` имеет событие `BackStackChanged`, которое срабатывает при каждом изменении содержимого стека фрагмента. Метод `OnCreate` в классе `MainActivity` содержит анонимный обработчик событий для этого события:

```csharp
SupportFragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = SupportFragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Note Entry" : "Notes";
};
```

Этот обработчик событий отображает кнопку назад на панели действий при условии, что один или несколько экземпляров `Fragment` в стеке назад фрагмента. Ответ на нажатие кнопки назад обрабатывается переопределением `OnOptionsItemSelected`:

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

Переопределение `OnOptionsItemSelected` вызывается всякий раз, когда выбран элемент в меню Параметры. Эта реализация выводит текущий фрагмент из стека фрагмента, при условии что выбрана кнопка «назад» и имеется один или несколько экземпляров `Fragment` в стеке «фрагмент».

### <a name="multiple-activities"></a>Несколько действий

Если приложение состоит из нескольких действий, страницы, производные от [`ContentPage`](xref:Xamarin.Forms.ContentPage), могут быть внедрены в каждое из действий. В этом сценарии `Forms.Init` метод должен вызываться только в переопределении `OnCreate` первого `Activity`, который внедряет `ContentPage` Xamarin. Forms. Однако это имеет следующие последствия.

- Значение `Xamarin.Forms.Color.Accent` будет взято из `Activity`, который вызвал метод `Forms.Init`.
- Значение `Xamarin.Forms.Application.Current` будет связано с `Activity`, который вызвал метод `Forms.Init`.

### <a name="choose-a-file"></a>Выбор файла

При внедрении страницы, производной от [`ContentPage`](xref:Xamarin.Forms.ContentPage), которая использует [`WebView`](xref:Xamarin.Forms.WebView) , который должен поддерживать HTML-кнопку "выбрать файл", `Activity` потребуется переопределить метод `OnActivityResult`:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    ActivityResultCallbackRegistry.InvokeCallback(requestCode, resultCode, data);
}
```

## <a name="uwp"></a>UWP

В UWP класс Native `App` обычно является местом для выполнения задач, связанных с запуском приложения. Xamarin. Forms обычно инициализируются в приложениях UWP Xamarin. Forms в `OnLaunched` переопределении в собственном `App` классе для передачи аргумента `LaunchActivatedEventArgs` в метод `Forms.Init`. По этой причине собственные приложения UWP, которые используют страницу, производную от Xamarin. Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage), наиболее легко вызывают метод `Forms.Init` из метода `App.OnLaunched`.

По умолчанию класс собственного `App` запускает класс `MainPage` в качестве первой страницы приложения. В следующем примере кода показан класс `MainPage` в примере приложения:

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

Конструктор `MainPage` выполняет следующие задачи:

- Для страницы включено кэширование, чтобы новая `MainPage` не создавалась, когда пользователь переходит на страницу.
- Ссылка на класс `MainPage` хранится в поле `static` `Instance`. Это необходимо для предоставления другим классам механизма вызова методов, определенных в классе `MainPage`.
- Свойство `FolderPath` инициализируется по пути на устройстве, где будут храниться данные о заметках.
- Класс `NotesPage`, который является страницей, производной от [`ContentPage`](xref:Xamarin.Forms.ContentPage)Xamarin. Forms, определенной в XAML, создается и преобразуется в `FrameworkElement` с помощью метода расширения `CreateFrameworkElement`, а затем устанавливается в качестве содержимого класса `MainPage`.

После выполнения конструктора `MainPage` будет отображаться пользовательский интерфейс, определенный в классе Xamarin. Forms `NotesPage`, как показано на следующем снимке экрана:

[![Снимок экрана приложения UWP, использующего пользовательский интерфейс, определенный с помощью Xamarin. Forms XAML](native-forms-images/uwp-notespage.png "Приложение UWP с пользовательским интерфейсом XAML Xamarin. Forms")](native-forms-images/uwp-notespage-large.png#lightbox "Приложение UWP с пользовательским интерфейсом XAML Xamarin. Forms")

При взаимодействии с пользовательским интерфейсом, например при нажатии **+** [`Button`](xref:Xamarin.Forms.Button), в `NotesPage` кода программной части будет создан следующий обработчик событий:

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    MainPage.Instance.NavigateToNoteEntryPage(new Note());
}
```

Поле `static` `MainPage.Instance` позволяет вызывать метод `MainPage.NavigateToNoteEntryPage`, который показан в следующем примере кода:

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    this.Frame.Navigate(new NoteEntryPage
    {
        BindingContext = note
    });
}
```

Навигация в UWP обычно выполняется с помощью метода `Frame.Navigate`, который принимает `Page` аргумент. Xamarin. Forms определяет `Frame.Navigate` метод расширения, который принимает экземпляр страницы, производной от [`ContentPage`](xref:Xamarin.Forms.ContentPage). Поэтому при выполнении метода `NavigateToNoteEntryPage` будет отображаться пользовательский интерфейс, определенный в `NoteEntryPage` Xamarin. Forms, как показано на следующем снимке экрана:

[![Снимок экрана приложения UWP, использующего пользовательский интерфейс, определенный с помощью Xamarin. Forms XAML](native-forms-images/uwp-noteentrypage.png "Приложение UWP с пользовательским интерфейсом XAML Xamarin. Forms")](native-forms-images/uwp-noteentrypage-large.png#lightbox "Приложение UWP с пользовательским интерфейсом XAML Xamarin. Forms")

Когда отображается `NoteEntryPage`, коснитесь стрелки назад, чтобы открыть `FrameworkElement` для `NoteEntryPage` из стека обратного приложения, возвращая пользователя в `FrameworkElement` для класса `NotesPage`.

### <a name="enable-back-navigation-support"></a>Включить поддержку обратной навигации

В UWP приложения должны включить обратную навигацию для всех кнопок возврата оборудования и программного обеспечения в различных конструктивных параметрах устройства. Это можно сделать, зарегистрировав обработчик событий для события `BackRequested`, которое может быть выполнено в методе `OnLaunched` в собственном `App` классе:

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

При запуске приложения метод `GetForCurrentView` извлекает объект `SystemNavigationManager`, связанный с текущим представлением, а затем регистрирует обработчик событий для события `BackRequested`. Приложение получает это событие только в том случае, если оно является приложением переднего плана, а в ответе вызывает обработчик событий `OnBackRequested`:

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

Обработчик событий `OnBackRequested` вызывает метод `GoBack` в корневом фрейме приложения и задает для свойства `BackRequestedEventArgs.Handled` значение `true`, чтобы пометить событие как обработанное. Сбой пометки события как обработанного может привести к тому, что событие будет пропущено.

Приложение выбирает, следует ли отображать кнопку "назад" в строке заголовка. Это достигается путем присвоения свойству `AppViewBackButtonVisibility` одного из значений перечисления `AppViewBackButtonVisibility`.

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

Обработчик событий `OnNavigated`, выполняемый в ответ на срабатывание события `Navigated`, обновляет видимость кнопки «назад» в строке заголовка при переходе по страницам. Это гарантирует, что кнопка «назад» в строке заголовка будет видна, если стек в приложении не пуст или удален из строки заголовка, если резервный стек в приложении пуст.

Дополнительные сведения о поддержке обратной навигации в UWP см. в статье [журнал навигации и обратная Навигация для приложений UWP](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/).

## <a name="related-links"></a>Связанные ссылки

- [Нативеформс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/native2forms)
- [Исходные представления](~/xamarin-forms/platform/native-views/index.md)
