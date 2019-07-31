---
title: Углубленное углубление в руководстве по Xamarin. Forms
description: Эта статья содержит общие сведения о разработке приложений с использованием Xamarin.Forms. Помимо прочего, здесь описываются структура приложения Xamarin.Forms и принципы его работы, а также пользовательский интерфейс.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 7B2340A1-6883-41D8-860C-0BB6C4E0C316
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/27/2018
ms.openlocfilehash: c0e772e0207d2ccc9a94aefd9655dc1bb6776bba
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68653485"
---
# <a name="xamarinforms-quickstart-deep-dive"></a>Углубленное углубление в руководстве по Xamarin. Forms

В [кратком руководстве по Xamarin. Forms](~/get-started/index.yml)было создано приложение Notes. В этой статье выполняется проверка созданных компонентов, позволяющая получить представление об основных принципах работы приложений Xamarin.Forms.

::: zone pivot="windows"

## <a name="introduction-to-visual-studio"></a>Введение в Visual Studio

Код в Visual Studio упорядочен по *решениям* и *проектам*. Решение — это контейнер для одного или нескольких проектов. Проект может представлять собой приложение, вспомогательную библиотеку, тестовое приложение и т. д. Приложение Notes состоит из одного решения, содержащего четыре проекта, как показано на следующем снимке экрана:

![](deepdive-images/vs/solution.png "Обозреватель решений Visual Studio")

Решение содержит следующие проекты:

- Примечания. Этот проект является проектом .NET Standard Library, который содержит весь общий код и общий пользовательский интерфейс.
- Notes. Android — этот проект содержит код, относящийся к Android, и является точкой входа для приложения Android.
- Notes. iOS — этот проект содержит код, относящийся к iOS, и является точкой входа для приложения iOS.
- Notes. UWP — этот проект содержит код, относящийся к универсальная платформа Windows (UWP), и является точкой входа для приложения UWP.

## <a name="anatomy-of-a-xamarinforms-application"></a>Анатомия приложения Xamarin. Forms

На следующем снимке экрана показано содержимое проекта Библиотека Notes .NET Standard Library в Visual Studio:

![](deepdive-images/vs/net-standard-project.png "Содержимое проекта .NET Standard Phoneword")

В этом проекте есть узел **Зависимости**, который содержит узлы **NuGet** и **SDK**:

- **NuGet** &ndash; пакеты NuGet Xamarin. Forms и SQLite-NET-PCL, добавленные в проект.
- **SDK** &ndash; метапакет `NETStandard.Library`, который ссылается на полный набор пакетов NuGet, определяющих библиотеку .NET Standard.

::: zone-end
::: zone pivot="macos"

## <a name="introduction-to-visual-studio-for-mac"></a>Введение в Visual Studio для Mac

В [Visual Studio для Mac](/visualstudio/mac/), так же как в Visual Studio, код упорядочивается по *решениям* и *проектам*. Решение — это контейнер для одного или нескольких проектов. Проект может представлять собой приложение, вспомогательную библиотеку, тестовое приложение и т. д. Приложение Notes состоит из одного решения, содержащего три проекта, как показано на следующем снимке экрана:

![](deepdive-images/vsmac/solution.png "Область решений в Visual Studio для Mac")

Решение содержит следующие проекты:

- Примечания. Этот проект является проектом .NET Standard Library, который содержит весь общий код и общий пользовательский интерфейс.
- Notes. Android — этот проект содержит код, относящийся к Android, и является точкой входа для приложений Android.
- Notes. iOS — этот проект содержит специальный код для iOS и является точкой входа для приложений iOS.

## <a name="anatomy-of-a-xamarinforms-application"></a>Анатомия приложения Xamarin. Forms

На следующем снимке экрана показано содержимое проекта Библиотека Notes .NET Standard Library в Visual Studio для Mac.

![](deepdive-images/vsmac/net-standard-project.png "Содержимое проекта библиотеки .NET Standard Phoneword")

В этом проекте есть узел **Зависимости**, который содержит узлы **NuGet** и **SDK**:

- **NuGet** &ndash; пакеты NuGet Xamarin. Forms и SQLite-NET-PCL, добавленные в проект.
- **SDK** &ndash; метапакет `NETStandard.Library`, который ссылается на полный набор пакетов NuGet, определяющих библиотеку .NET Standard.

::: zone-end

Проект также состоит из нескольких файлов:

- **Дата\нотедатабасе.КС** — этот класс содержит код для создания базы данных, считывания данных из нее, записи данных в нее и удаления данных из нее.
- **Моделс\ноте.КС** — этот класс определяет `Note` модель, экземпляры которой хранят данные о каждой заметке в приложении.
- **App.xaml** — разметка XAML для класса `App`, который определяет словарь ресурсов для приложения.
- **App.xaml.cs** — код программной части для класса `App`, который обеспечивает создание экземпляра первой страницы, отображаемой приложением на каждой платформе, а также обработку событий жизненного цикла приложения.
- **AssemblyInfo.CS** — этот файл содержит атрибут приложения о проекте, который применяется на уровне сборки.
- **Нотеспаже. XAML** — разметка XAML для `NotesPage` класса, определяющая пользовательский интерфейс для страницы, отображаемой при запуске приложения.
- **NotesPage.XAML.CS** — код программной части для `NotesPage` класса, который содержит бизнес-логику, которая выполняется, когда пользователь взаимодействует со страницей.
- **Нотинтрипаже. XAML** — разметка XAML для `NoteEntryPage` класса, определяющая пользовательский интерфейс для страницы, отображаемой при вводе пользователем заметки.
- **NoteEntryPage.XAML.CS** — код программной части для `NoteEntryPage` класса, который содержит бизнес-логику, которая выполняется, когда пользователь взаимодействует со страницей.

Дополнительные сведения о структуре приложения Xamarin.iOS см. [здесь](~/ios/get-started/hello-ios/hello-ios-deepdive.md#anatomy-of-a-xamarinios-application). Дополнительные сведения о структуре приложения Xamarin.Android см. [здесь](~/android/get-started/hello-android/hello-android-deepdive.md#anatomy).

## <a name="architecture-and-application-fundamentals"></a>Основы архитектуры и приложений

По своей архитектуре приложения Xamarin.Forms не отличаются от традиционных кроссплатформенных приложений. Общий код обычно помещается в библиотеку .NET Standard и используется приложениями для конкретных платформ. На следующей схеме показан обзор этой связи для приложения Notes.

::: zone pivot="windows"

![](deepdive-images/vs/architecture.png "Архитектура заметок")

::: zone-end
::: zone pivot="macos"

![](deepdive-images/vsmac/architecture.png "Архитектура заметок")

::: zone-end

Чтобы код запуска как можно больше использовался повторно, в приложениях Xamarin.Forms есть отдельный класс `App`, который отвечает за создание экземпляра первой страницы, отображаемой приложением на каждой платформе, как показано в следующем примере кода:

```csharp
using Xamarin.Forms;

namespace Notes
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            MainPage = new NavigationPage(new NotesPage());
        }
        ...
    }
}
```

Этот код задает `MainPage` свойству `App` класса [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) экземпляр, содержимое которого является `NotesPage` экземпляром.

Кроме того, файл **AssemblyInfo.CS** содержит один атрибут приложения, который применяется на уровне сборки:

```csharp
using Xamarin.Forms.Xaml;

[assembly: XamlCompilation(XamlCompilationOptions.Compile)]
```

[`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) Атрибут включает компилятор XAML, чтобы код XAML был скомпилирован непосредственно в промежуточный язык. Дополнительные сведения см. в разделе [Компиляция XAML](~/xamarin-forms/xaml/xamlc.md).

## <a name="launching-the-application-on-each-platform"></a>Запуск приложения на каждой платформе

### <a name="ios"></a>iOS

Чтобы запустить начальную страницу Xamarin. Forms в iOS, проект notess. iOS определяет `AppDelegate` класс, наследующий `FormsApplicationDelegate` от класса:

```csharp
namespace Notes.iOS
{
    [Register("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        public override bool FinishedLaunching(UIApplication app, NSDictionary options)
        {
            global::Xamarin.Forms.Forms.Init();
            LoadApplication(new App());
            return base.FinishedLaunching(app, options);
        }
    }
}
```

Переопределение `FinishedLaunching` инициализирует платформу Xamarin.Forms, вызывая метод `Init`. В результате реализация Xamarin.Forms для iOS загружается в приложении, прежде чем корневой контроллер представления задается путем вызова метода `LoadApplication`.

### <a name="android"></a>Android

Чтобы запустить начальную страницу Xamarin. Forms в Android, проект notess. Android включает в себя код `Activity` , который `MainLauncher` создает с атрибутом и `FormsAppCompatActivity` действием, наследующим от класса:

```csharp
namespace Notes.Droid
{
    [Activity(Label = "Notes",
              Icon = "@mipmap/icon",
              Theme = "@style/MainTheme",
              MainLauncher = true,
              ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle savedInstanceState)
        {
            TabLayoutResource = Resource.Layout.Tabbar;
            ToolbarResource = Resource.Layout.Toolbar;

            base.OnCreate(savedInstanceState);
            global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
            LoadApplication(new App());
        }
    }
}
```

Переопределение `OnCreate` инициализирует платформу Xamarin.Forms, вызывая метод `Init`. В результате реализация Xamarin.Forms для Android загружается в приложении перед загрузкой самого приложения Xamarin.Forms.

::: zone pivot="windows"

### <a name="universal-windows-platform"></a>Универсальная платформа Windows

В приложениях универсальной платформы Windows (UWP) метод `Init`, который инициализирует платформу Xamarin.Forms, вызывается из класса `App`:

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

В результате в приложении загружается реализация Xamarin.Forms для UWP. Начальная страница Xamarin. Forms запускается `MainPage` классом:

```csharp
namespace Notes.UWP
{
    public sealed partial class MainPage
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.LoadApplication(new Notes.App());
        }
    }
}
```

Приложение Xamarin.Forms загружается с помощью метода `LoadApplication`.

> [!NOTE]
> Универсальная платформа Windows приложения можно создавать с помощью Xamarin. Forms, но только с помощью Visual Studio в Windows.

::: zone-end

## <a name="user-interface"></a>Пользовательский интерфейс

Существует четыре основные группы управления, используемые для создания пользовательского интерфейса приложения Xamarin. Forms:

1. **Страницы** — страницы Xamarin.Forms представляют экраны в кроссплатформенных мобильных приложениях. Приложение Notes использует [`ContentPage`](xref:Xamarin.Forms.ContentPage) класс для показа отдельных экранов. Дополнительные сведения о страницах см. в статье [Страницы Xamarin.Forms](~/xamarin-forms/user-interface/controls/pages.md).
1. **Представления** — представления Xamarin.Forms являются элементами управления, отображаемыми в пользовательском интерфейсе, например метки, кнопки и поля ввода текста. В завершении работы с заметками [`ListView`](xref:Xamarin.Forms.ListView)используются [`Button`](xref:Xamarin.Forms.Button) представления, [`Editor`](xref:Xamarin.Forms.Editor)и. Дополнительные сведения о представлениях см. в статье [Представления Xamarin.Forms](~/xamarin-forms/user-interface/controls/views.md).
1. **Макеты** — макеты Xamarin.Forms представляют собой контейнеры, которые служат для объединения представлений в логические структуры. Приложение Notes использует [`StackLayout`](xref:Xamarin.Forms.StackLayout) класс для упорядочивания представлений в вертикальном стеке [`Grid`](xref:Xamarin.Forms.Grid) и класс для упорядочивания кнопок по горизонтали. Дополнительные сведения о макетах см. в статье [Макеты Xamarin.Forms](~/xamarin-forms/user-interface/controls/layouts.md).
1. **Ячейки** — ячейки Xamarin.Forms являются специальными объектами, представляющими элементы списка и описывающими способ их отрисовки. Приложение Notes использует [`TextCell`](xref:Xamarin.Forms.TextCell) для вывода двух элементов для каждой строки в списке. Дополнительные сведения о ячейках см. в статье [Ячейки Xamarin.Forms](~/xamarin-forms/user-interface/controls/cells.md).

Во время выполнения каждый элемент управления сопоставляется с собственным аналогом, который и будет отрисовываться.

### <a name="layout"></a>Макет

Приложение Notes использует [`StackLayout`](xref:Xamarin.Forms.StackLayout) для упрощения разработки кросс-платформенных приложений путем автоматического упорядочения представлений на экране независимо от размера экрана. Дочерние элементы размещаются поочередно (по горизонтали или по вертикали) в порядке добавления. Область, занимаемая макетом `StackLayout`, зависит от значений свойств [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) и [`VerticalOptions`](xref:Xamarin.Forms.View.HorizontalOptions), но по умолчанию макет `StackLayout` будет пытаться использовать весь экран.

В следующем коде XAML показан пример использования [`StackLayout`](xref:Xamarin.Forms.StackLayout) для `NoteEntryPage`макета для:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Notes.NoteEntryPage"
             Title="Note Entry">
    ...    
    <StackLayout Margin="{StaticResource PageMargin}">
        <Editor Placeholder="Enter your note"
                Text="{Binding Text}"
                HeightRequest="100" />
        <Grid>
            ...
        </Grid>
    </StackLayout>    
</ContentPage>
```

По умолчанию [`StackLayout`](xref:Xamarin.Forms.StackLayout) для предполагается вертикальная ориентация. Однако его можно изменить на горизонтальную ориентацию, присвоив [`StackLayout.Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) свойству [`StackOrientation.Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal) элемент перечисления.

> [!NOTE]
> Размер представлений можно задать с помощью `HeightRequest` свойств и. `WidthRequest`

Дополнительные сведения о классе [`StackLayout`](xref:Xamarin.Forms.StackLayout) см. в статье [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md).

### <a name="responding-to-user-interaction"></a>Реакция на действия пользователя

Объект, определенный в XAML, может инициировать событие, которое обрабатывается в файле кода программной части. В следующем `OnSaveButtonClicked` примере кода показан метод в коде программной части `NoteEntryPage` для класса, который [`Clicked`](xref:Xamarin.Forms.Button.Clicked) выполняется в ответ на срабатывание события на кнопке *Save* .

```csharp
async void OnSaveButtonClicked(object sender, EventArgs e)
{
    var note = (Note)BindingContext;
    note.Date = DateTime.UtcNow;
    await App.Database.SaveNoteAsync(note);
    await Navigation.PopAsync();
}
```

`OnSaveButtonClicked` Метод сохраняет заметку в базе данных и переходит назад к предыдущей странице.

> [!NOTE]
> Файл кода программной части для класса XAML может получать доступ к объекту, определенному в XAML, по имени, которое назначено ему в атрибуте `x:Name`. Значение, присваиваемое этому атрибуту, должно соответствовать правилам C# в отношении переменных, то есть должно начинаться с буквы или знака подчеркивания и не содержать внутренних пробелов.

Привязка кнопки сохранить к `OnSaveButtonClicked` методу происходит в разметке XAML `NoteEntryPage` для класса:

```xaml
<Button Text="Save"
        Clicked="OnSaveButtonClicked" />
```

### <a name="lists"></a>Списки

Объект [`ListView`](xref:Xamarin.Forms.ListView) отвечает за отображение коллекции элементов по вертикали в списке. Каждый элемент в `ListView` будет содержаться в одной ячейке.

В следующем примере кода показано [`ListView`](xref:Xamarin.Forms.ListView) `NotesPage`из:

```xaml
<ListView x:Name="listView"
          Margin="{StaticResource PageMargin}"
          ItemSelected="OnListViewItemSelected">
    <ListView.ItemTemplate>
        <DataTemplate>
            <TextCell Text="{Binding Text}"
                      Detail="{Binding Date}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Макет каждой строки в [`ListView`](xref:Xamarin.Forms.ListView) задается [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) внутри элемента и использует привязку данных для вывода всех заметок, извлекаемых приложением. Свойство устанавливается в источник данных в `NotesPage.xaml.cs`: [`ListView.ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource)

```csharp
protected override async void OnAppearing()
{
    base.OnAppearing();

    listView.ItemsSource = await App.Database.GetNotesAsync();
}
```    

Этот код заполняет [`ListView`](xref:Xamarin.Forms.ListView) с помощью любых заметок, хранящихся в базе данных.

При выборе строки в [`ListView`](xref:Xamarin.Forms.ListView) [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) вызывается событие. Обработчик событий с именем `OnListViewItemSelected`выполняется при срабатывании события:

```csharp
async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
{
    if (e.SelectedItem != null)
    {
        ...
    }
}
```

Событие может обращаться к объекту, который был связан с ячейкой [`e.SelectedItem`](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) через свойство. [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)

Дополнительные сведения о классе см [`ListView`](xref:Xamarin.Forms.ListView) . в разделе [ListView](~/xamarin-forms/user-interface/listview/index.md).

## <a name="navigation"></a>Навигация

Xamarin.Forms предоставляет ряд различных способов перехода по страницам в зависимости от используемого типа объекта [`Page`](xref:Xamarin.Forms.Page). Для [`ContentPage`](xref:Xamarin.Forms.ContentPage) навигации по экземплярам может быть иерархическим или модальным. Сведения о модальной навигации см. в разделе [модальные страницы Xamarin. Forms](~/xamarin-forms/app-fundamentals/navigation/modal.md).

> [!NOTE]
> Классы [`CarouselPage`](xref:Xamarin.Forms.CarouselPage), [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) и [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) предоставляют альтернативные способы перехода. Дополнительные сведения см. в разделе [Переходы](~/xamarin-forms/app-fundamentals/navigation/index.md).

В иерархическом переходе [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) класс используется для перемещения по [`ContentPage`](xref:Xamarin.Forms.ContentPage) стеку объектов, а также пересылает и обратно по мере необходимости. Этот класс реализует навигацию на основе стека объектов [`Page`](xref:Xamarin.Forms.Page) по методу LIFO (последним поступил — первым обслужен). Для перехода с одной страницы на другую приложение помещает новую страницу в стек навигации, где она становится активной страницей. Для возврата к предыдущей странице приложение выбирает текущую страницу из стека навигации, после чего активной становится верхняя страница в стеке.

Класс `NavigationPage` также добавляет панель навигации в верхнюю часть страницы, где отображается заголовок и соответствующая платформе кнопка **Назад**, позволяющая вернуться на предыдущую страницу.

Первая страница, добавленная в стек навигации, называется *корневой* страницей приложения, а в следующем примере кода показано, как это делается в приложении notess:

```csharp
public App ()
{
    ...
    MainPage = new NavigationPage (new NotesPage ());
}
```

У всех экземпляров [`ContentPage`](xref:Xamarin.Forms.ContentPage) есть свойство[`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation), которое предоставляет методы для изменения стека страниц. Эти методы должны вызываться только в случае, если приложение содержит класс [`NavigationPage`](xref:Xamarin.Forms.NavigationPage). Для перехода к `NoteEntryPage` необходимо вызвать метод [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)), как показано в следующем примере кода:

```csharp
await Navigation.PushAsync(new NoteEntryPage());
```

Это приводит к тому `NoteEntryPage` , что новый объект помещается в стек навигации, где он превращается в активную страницу.

Активная страница может быть извлечена из стека навигации путем нажатия кнопки *Назад* на устройстве, причем это может быть как физическая кнопка, так и кнопка на экране. Чтобы вернуться на исходную страницу программным образом, объект `NoteEntryPage` должен вызвать метод [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync), как показано в следующем примере кода:

```csharp
await Navigation.PopAsync();
```

Дополнительные сведения об иерархической навигации см. в статье [Иерархическая навигация](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

## <a name="data-binding"></a>Привязка данных

Привязка данных используется для упрощения способа, которым приложение Xamarin.Forms отображает данные и взаимодействует с ними. Она устанавливает связь между пользовательским интерфейсом и базовым приложением. Класс [`BindableObject`](xref:Xamarin.Forms.BindableObject) содержит основную часть инфраструктуры для поддержки привязки данных.

Привязка данных связывает два объекта, которые называются *источником* и *целевым объектом*. *Источник* предоставляет данные. *Целевой* объект будет использовать (и часто отображать) данные из источника. [`Editor`](xref:Xamarin.Forms.Editor) Например, (*целевой* объект) будет как правило привязывать свое [`Text`](xref:Xamarin.Forms.Editor.Text) свойство к открытому `string` свойству в *исходном* объекте. На следующей схеме показано отношение привязки:

![](deepdive-images/data-binding.png "Привязка данных")

Главным преимуществом привязки данных является то, что вам больше не нужно беспокоиться о синхронизации данных между представлениями и источником данных. Изменения в *источнике* автоматически передаются в *целевой* объект платформой привязки. При необходимости изменения в целевом объекте также могут передаваться в *источник*.

Установка привязки данных состоит из двух этапов:

- Свойству [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) *целевого* объекта должен быть присвоен *источник*.
- Между *целевым* объектом и *источником* должна быть установлена привязка. В XAML для этого используется расширение разметки [`Binding`](xref:Xamarin.Forms.Xaml.BindingExtension).

В приложении Notes [`Editor`](xref:Xamarin.Forms.Editor) целевым объектом привязки является, который отображает Примечание, в `Note` то время как [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) набор `NoteEntryPage` экземпляров является источником привязки.

Объект `BindingContext`задается во время навигации по странице, как показано в следующем примере кода: `NoteEntryPage`

```csharp
async void OnNoteAddedClicked(object sender, EventArgs e)
{
    await Navigation.PushAsync(new NoteEntryPage
    {
        BindingContext = new Note()
    });
}

async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
{
    if (e.SelectedItem != null)
    {
        await Navigation.PushAsync(new NoteEntryPage
        {
            BindingContext = e.SelectedItem as Note
        });
    }
}
```

В методе, который выполняется при добавлении новой заметки в приложение [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) , `NoteEntryPage` для задается новый `Note` экземпляр. `OnNoteAddedClicked` `Note` `BindingContext` [`ListView`](xref:Xamarin.Forms.ListView) [`e.SelectedItem`](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) `NoteEntryPage` В методе, который выполняется при выборе существующей заметки в, свойство объекта задается для выбранного экземпляра, доступ к которому осуществляется с помощью свойства. `OnListViewItemSelected`

> [!IMPORTANT]
> Хотя свойство [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) каждого *целевого* объекта можно задавать по отдельности, это не является обязательным. `BindingContext` — это особое свойство, которое наследуется всеми дочерними объектами. Таким образом, если `BindingContext` [`ContentPage`](xref:Xamarin.Forms.ContentPage) в задан `Note` `BindingContext` `Note` экземпляр, все дочерние элементы объекта имеютодинаковыезначенияимогутбытьпривязаныкоткрытымсвойствамобъекта.`ContentPage`

[`Editor`](xref:Xamarin.Forms.Editor) Затемв`NoteEntryPage` затем привязывается `Note` к свойствуобъекта:`Text`

```xaml
<Editor Placeholder="Enter your note"
        Text="{Binding Text}"
        ... />
```

Создается привязка между свойством [`Editor.Text`](xref:Xamarin.Forms.Editor.Text) и свойством `Text` *источника*. Изменения, внесенные `Editor` в, будут автоматически распространяться `Note` на объект. Аналогично, если в `Note.Text` свойство вносятся изменения, механизм привязки Xamarin. Forms также обновляет содержимое. `Editor` Это называется *двусторонней привязкой*.

Дополнительные сведения о привязке данных см. в разделе [Привязки данных в Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md).

## <a name="styling"></a>Задание стиля

Приложения Xamarin. Forms часто содержат несколько визуальных элементов с одинаковым внешним видом. Установка внешнего вида для каждого визуального элемента может быть повторена и подвержена ошибкам. Вместо этого можно создать стили, которые определяют внешний вид, а затем применять их к необходимым визуальным элементам.

[`Style`](xref:Xamarin.Forms.Style) Класс группирует коллекцию значений свойств в один объект, который затем можно применить к нескольким экземплярам визуальных элементов. Стили хранятся в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), на уровне приложения, на уровне страницы или на уровне представления. Выбор места определения влияния, `Style` когда его можно использовать:

- [`Style`](xref:Xamarin.Forms.Style)экземпляры, определенные на уровне приложения, можно применять ко всему приложению.
- [`Style`](xref:Xamarin.Forms.Style)экземпляры, определенные на уровне страницы, могут быть применены к странице и ее дочерним элементам.
- [`Style`](xref:Xamarin.Forms.Style)экземпляры, определенные на уровне представления, можно применить к представлению и его дочерним элементам.

> [!IMPORTANT]
> Все стили, используемые в приложении, хранятся в словаре ресурсов приложения во избежание дублирования. Однако код XAML, относящийся к странице, не должен включаться в словарь ресурсов приложения, так как после этого ресурсы будут анализироваться при запуске приложения, а не при необходимости на странице.

Каждый [`Style`](xref:Xamarin.Forms.Style) экземпляр содержит коллекцию из одного или нескольких [`Setter`](xref:Xamarin.Forms.Setter) объектов [`Property`](xref:Xamarin.Forms.Setter.Property) , [`Value`](xref:Xamarin.Forms.Setter.Value)каждый `Setter` из которых имеет и. — Это имя привязываемого свойства элемента, к которому применяется стиль, `Value` а — это значение, которое применяется к свойству. `Property` В следующем примере кода показан стиль из `NoteEntryPage`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Notes.NoteEntryPage"
             Title="Note Entry">
    <ContentPage.Resources>
        <!-- Implicit styles -->
        <Style TargetType="{x:Type Editor}">
            <Setter Property="BackgroundColor"
                    Value="{StaticResource AppBackgroundColor}" />
        </Style>
        ...
    </ContentPage.Resources>
    ...
</ContentPage>
```

Этот стиль применяется к любым [`Editor`](xref:Xamarin.Forms.Editor) экземплярам на странице.

При создании [`Style`](xref:Xamarin.Forms.Style) [`TargetType`](xref:Xamarin.Forms.Style.TargetType) свойство всегда является обязательным.

> [!NOTE]
> Стиль приложения Xamarin. Forms традиционно реализуется с помощью стилей XAML. Однако Xamarin. Forms также поддерживает визуальные элементы стилизации с помощью каскадные таблицы стилей (CSS). Дополнительные сведения см. в разделе [стилизация приложений Xamarin. Forms с помощью каскадные таблицы стилей (CSS)](~/xamarin-forms/user-interface/styles/css/index.md).

Дополнительные сведения о стилях XAML см. в руководстве по [оформлению приложений Xamarin.Forms с использованием стилей XAML](~/xamarin-forms/user-interface/styles/xaml/index.md).

### <a name="providing-platform-specific-styles"></a>Предоставление стилей для конкретной платформы

Расширения `OnPlatform` разметки позволяют настраивать внешний вид пользовательского интерфейса на уровне платформы:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Notes.App">
    <Application.Resources>
        ...
        <Color x:Key="iOSNavigationBarColor">WhiteSmoke</Color>
        <Color x:Key="AndroidNavigationBarColor">#2196F3</Color>
        <Color x:Key="iOSNavigationBarTextColor">Black</Color>
        <Color x:Key="AndroidNavigationBarTextColor">White</Color>

        <Style TargetType="{x:Type NavigationPage}">
            <Setter Property="BarBackgroundColor"
                    Value="{OnPlatform iOS={StaticResource iOSNavigationBarColor},
                                       Android={StaticResource AndroidNavigationBarColor}}" />
             <Setter Property="BarTextColor"
                    Value="{OnPlatform iOS={StaticResource iOSNavigationBarTextColor},
                                       Android={StaticResource AndroidNavigationBarTextColor}}" />           
        </Style>
        ...
    </Application.Resources>
</Application>
```

При [`Style`](xref:Xamarin.Forms.Style) этом задаются различные [`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) [`Color`](xref:Xamarin.Forms.Color) значения [`BarTextColor`](xref:Xamarin.Forms.NavigationPage.BarTextColor) [свойств`NavigationPage`](xref:Xamarin.Forms.NavigationPage)и в, в зависимости от используемой платформы.

Дополнительные сведения о расширениях разметки XAML см. в статье [Расширения разметки XAML](~/xamarin-forms/xaml/markup-extensions/index.md). Дополнительные сведения о расширении `OnPlatform` разметки см. в разделе [расширение разметки для платформы](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension).

## <a name="testing-and-deployment"></a>Тестирование и развертывание

Как Visual Studio для Mac, так и Visual Studio предоставляют множество возможностей для тестирования и развертывания приложения. Одной из неотъемлемых стадий жизненного цикла приложения является отладка, которая позволяет выявить проблемы в коде. Дополнительные сведения см. в разделах [Установка точки останова](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint), [Пошаговая отладка кода](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code) и [Вывод данных в окно журнала](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window).

Симуляторы позволяют начать тестирование и развертывание приложения, реализуя полезные функции для тестирования приложений. Тем не менее пользователи будут работать с готовым приложением не в симуляторе, поэтому любое приложение необходимо тестировать на реальных устройствах как можно раньше и чаще. Дополнительные сведения о процессе подготовки для устройств iOS см. в разделе [Подготовка устройства](~/ios/get-started/installation/device-provisioning/index.md). Дополнительные сведения о процессе подготовки для устройств Android см. в разделе [Настройка устройства для разработки](~/android/get-started/installation/set-up-device-for-development.md).

## <a name="next-steps"></a>Следующие шаги

В этом подробном обзоре рассматриваются основы разработки приложений с помощью Xamarin. Forms. Далее рекомендуется ознакомиться с перечисленными ниже функциональными возможностями.

- Для создания пользовательского интерфейса приложения Xamarin.Forms используются четыре основные группы элементов управления. Более подробную информацию см. в разделе [Справочник по элементам управления](~/xamarin-forms/user-interface/controls/index.md).
- Привязка данных — это способ связывания свойств двух объектов так, чтобы изменения в одном свойстве автоматически отражались в другом. Более подробную информацию см. в разделе [Привязка данных](~/xamarin-forms/app-fundamentals/data-binding/index.md).
- Xamarin.Forms предоставляет ряд различных способов перехода по страницам в зависимости от используемого типа страницы. Дополнительные сведения см. в разделе [Переходы](~/xamarin-forms/app-fundamentals/navigation/index.md).
- Стили помогают снизить повторяемость разметки и упрощают изменение внешнего вида приложения. Дополнительные сведения см. в разделе [Стилизация приложений Xamarin.Forms](~/xamarin-forms/user-interface/styles/index.md).
- Расширения разметки XAML позволяют расширить функциональность и гибкость XAML, давая возможность задавать атрибуты элементов из источников, отличных от строковых литералов. Подробнее см. в разделе [Расширение разметки XAML](~/xamarin-forms/xaml/markup-extensions/index.md).
- Шаблоны данных дают возможность настраивать представление данных в поддерживаемых представлениях. Дополнительные сведения см. в статье [Шаблоны данных](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).
- Каждая страница, макет и представление отрисовываются по-разному на разных платформах с помощью класса `Renderer`, который создает собственный элемент управления, размещает его на экране и реализует поведение, определенное в общем коде. Разработчики могут реализовывать пользовательские классы `Renderer` для настройки внешнего вида или работы элемента управления. Дополнительные сведения см. в статье [Пользовательские отрисовщики](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).
- Эффекты также позволяют настраивать собственные элементы управления на каждой платформе. Эффекты создаются в проектах для конкретных платформ путем создания подклассов класса [`PlatformEffect`](xref:Xamarin.Forms.PlatformEffect`2) и используются путем их присоединения к соответствующему элементу управления Xamarin.Forms. Дополнительные сведения см. в статье [Эффекты](~/xamarin-forms/app-fundamentals/effects/index.md).
- Общий код может получать доступ к собственным функциональным возможностям посредством класса [`DependencyService`](xref:Xamarin.Forms.DependencyService). Дополнительные сведения см. в статье, посвященной [доступу к собственным функциональным возможностям с помощью DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

Много полезной информации о платформе Xamarin.Forms можно почерпнуть из книги Чарльза Петцольда (Charles Petzold) [_Creating Mobile Apps with Xamarin.Forms_](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) (Создание мобильных приложений с помощью Xamarin.Forms). Книга доступна в формате PDF, а также в нескольких форматах электронных книг.

## <a name="related-links"></a>Связанные ссылки

- [XAML (расширяемый язык разметки приложений)](~/xamarin-forms/xaml/index.md)
- [Привязка данных](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Справочник по элементам управления](~/xamarin-forms/user-interface/controls/index.md)
- [Расширения разметки XAML](~/xamarin-forms/xaml/markup-extensions/index.md)
- [Примеры Xamarin.Forms](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Примеры для начала работы](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms%20get%20started)
- [Справочник по API для Xamarin.Forms](xref:Xamarin.Forms)
- [Бесплатное самостоятельное обучение (видео)](https://university.xamarin.com/self-guided/)
