---
title: Навигация по корпоративному приложению
description: В этой главе объясняется, как мобильное приложение eShopOnContainers выполняет навигацию по модели в первом режиме из моделей представления.
ms.prod: xamarin
ms.assetid: 4cad57b5-7fe4-4527-a988-d9b60c9620b4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 0f523c7149366cff85164f26f3f47b87801002cb
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306371"
---
# <a name="enterprise-app-navigation"></a>Навигация по корпоративному приложению

Xamarin. Forms включает поддержку навигации по страницам, что обычно приводит к взаимодействию пользователя с пользовательским ИНТЕРФЕЙСом или из самого приложения в результате внутренних изменений состояния, управляемых логическими операциями. Однако Навигация может быть сложной для реализации в приложениях, использующих шаблон Model-View-ViewModel (MVVM), так как должны выполняться следующие задачи:

- Определение представления, к которому осуществляется переход, с использованием подхода, который не обеспечивает тесное связывание и зависимости между представлениями.
- Способ координации процесса, с которым будет осуществляться переход к представлению, — это экземпляр и инициализирован. При использовании MVVM модель представления и представления необходимо создать и связать друг с другом через контекст привязки представления. Если приложение использует контейнер внедрения зависимостей, то для создания экземпляров представлений и моделей представлений может потребоваться специальный механизм построения.
- Следует ли выполнять навигацию или Просмотр первой модели. С помощью навигации «Просмотр — первый» страница, к которой выполняется переход, ссылается на имя типа представления. Во время навигации создается экземпляр указанного представления вместе с соответствующей моделью представления и другими зависимыми службами. Альтернативный подход заключается в использовании навигации «Просмотр модели — первый», где страница, к которой осуществляется переход, ссылается на имя типа модели представления.
- Как четко разделить поведение навигации приложения по представлениям и моделям представления. Шаблон MVVM обеспечивает разделение между пользовательским интерфейсом приложения и его представлением и бизнес-логикой. Однако поведение навигации приложения часто будет охватывать части пользовательского интерфейса и презентации приложения. Пользователь часто инициирует навигацию из представления, и представление будет заменено в результате навигации. Однако при этом часто бывает необходимо инициировать или координировать навигацию в модели представления.
- Передача параметров во время навигации для целей инициализации. Например, если пользователь переходит к представлению для обновления сведений о заказе, данные заказа должны быть переданы в представление, чтобы они могли отображать правильные данные.
- Принцип совместной навигации для обеспечения соблюдения определенных бизнес-правил. Например, пользователи могут получить запрос перед переходом от представления, чтобы они могли исправлять любые недопустимые данные, или выдать запрос на отправку или отмену изменений данных, внесенных в представление.

Эта глава решает эти проблемы, предоставляя класс `NavigationService`, который используется для представления модели просмотра первой страницы.

> [!NOTE]
> `NavigationService`, используемое приложением, предназначено только для выполнения иерархической навигации между экземплярами ContentPage. Использование службы для перехода между другими типами страниц может привести к непредвиденному поведению.

## <a name="navigating-between-pages"></a>Переход между страницами

Логика навигации может находиться в коде программной части представления или в модели представления с привязкой к данным. Хотя логика навигации в представлении может быть самым простым подходом, ее легко тестировать с помощью модульных тестов. Размещение логики навигации в классах модели представления означает, что логику можно использовать в модульных тестах. Кроме того, модель представления может реализовать логику для управления навигацией, чтобы обеспечить применение определенных бизнес-правил. Например, приложение может не позволить пользователю перейти с страницы на другую страницу, не убедившись в том, что входные данные являются допустимыми.

Класс `NavigationService`, как правило, вызывается из моделей представления для повышения пригодности для тестирования. Однако переход к представлениям из моделей представлений потребует, чтобы модели представления ссылались на представления, а в частности представления, которые не связаны с моделью активного представления, что не рекомендуется. Таким образом, `NavigationService`, представленный здесь, указывает тип модели представления в качестве целевого объекта для перехода.

Мобильное приложение eShopOnContainers использует класс `NavigationService`, чтобы обеспечить навигацию по модели представления. Этот класс реализует интерфейс `INavigationService`, который показан в следующем примере кода:

```csharp
public interface INavigationService  
{  
    ViewModelBase PreviousPageViewModel { get; }  
    Task InitializeAsync();  
    Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase;  
    Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase;  
    Task RemoveLastFromBackStackAsync();  
    Task RemoveBackStackAsync();  
}
```

Этот интерфейс указывает, что реализующий класс должен предоставлять следующие методы:

|Метод|Назначение|
|--- |--- |
|`InitializeAsync`|Выполняет переход на одну из двух страниц при запуске приложения.|
|`NavigateToAsync`|Выполняет иерархическую навигацию на указанной странице.|
|`NavigateToAsync(parameter)`|Выполняет иерархическую навигацию на указанной странице, передавая параметр.|
|`RemoveLastFromBackStackAsync`|Удаляет предыдущую страницу из стека навигации.|
|`RemoveBackStackAsync`|Удаляет все предыдущие страницы из стека навигации.|

Кроме того, интерфейс `INavigationService` указывает, что реализующий класс должен предоставлять свойство `PreviousPageViewModel`. Это свойство возвращает тип модели представления, связанный с предыдущей страницей в стеке навигации.

> [!NOTE]
> Интерфейс `INavigationService` обычно также задает метод `GoBackAsync`, который используется для программного возврата на предыдущую страницу стека навигации. Однако этот метод отсутствует в мобильном приложении eShopOnContainers, так как он не является обязательным.

### <a name="creating-the-navigationservice-instance"></a>Создание экземпляра NavigationService

Класс `NavigationService`, который реализует интерфейс `INavigationService`, регистрируется как одноэлементный с контейнером внедрения зависимостей Autofac, как показано в следующем примере кода:

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

Интерфейс `INavigationService` разрешается в конструкторе класса `ViewModelBase`, как показано в следующем примере кода:

```csharp
NavigationService = ViewModelLocator.Resolve<INavigationService>();
```

Он возвращает ссылку на объект `NavigationService`, который хранится в контейнере внедрения зависимостей Autofac, который создается методом `InitNavigation` в классе `App`. Дополнительные сведения см. [в разделе переходы при запуске приложения](#navigating_when_the_app_is_launched).

Класс `ViewModelBase` сохраняет экземпляр `NavigationService` в свойстве `NavigationService` типа `INavigationService`. Таким образом, все классы модели представления, производные от класса `ViewModelBase`, могут использовать свойство `NavigationService` для доступа к методам, заданным интерфейсом `INavigationService`. Это позволяет избежать издержек при внедрении объекта `NavigationService` из контейнера внедрения зависимостей Autofac в каждый класс модели представления.

### <a name="handling-navigation-requests"></a>Обработка запросов навигации

Xamarin. Forms предоставляет класс [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) , реализующий иерархическую навигацию, в которой пользователь может перемещаться по страницам, перемещая и обратнее по мере необходимости. Дополнительные сведения об иерархической навигации см. в статье [Иерархическая навигация](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

Вместо непосредственного использования класса [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) приложение eShopOnContainers заключает класс `NavigationPage` в класс `CustomNavigationView`, как показано в следующем примере кода:

```csharp
public partial class CustomNavigationView : NavigationPage  
{  
    public CustomNavigationView() : base()  
    {  
        InitializeComponent();  
    }  

    public CustomNavigationView(Page root) : base(root)  
    {  
        InitializeComponent();  
    }  
}
```

Цель этого переноса заключается в простоте стилизации экземпляра [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) в XAML-файле для класса.

Навигация выполняется внутри классов модели представления путем вызова одного из методов `NavigateToAsync`, указывающих тип модели представления для страницы, к которой осуществляется переход, как показано в следующем примере кода:

```csharp
await NavigationService.NavigateToAsync<MainViewModel>();
```

В следующем примере кода показаны методы `NavigateToAsync`, предоставляемые классом `NavigationService`:

```csharp
public Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), null);  
}  

public Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), parameter);  
}
```

Каждый метод позволяет любому классу модели представления, производному от класса `ViewModelBase`, выполнять иерархическую навигацию, вызывая метод `InternalNavigateToAsync`. Кроме того, второй метод `NavigateToAsync` позволяет указать данные навигации в качестве аргумента, который передается в модель представления, к которой осуществляется переход, где обычно используется для выполнения инициализации. Дополнительные сведения см. в разделе [Передача параметров во время навигации](#passing_parameters_during_navigation).

Метод `InternalNavigateToAsync` выполняет запрос навигации и показан в следующем примере кода:

```csharp
private async Task InternalNavigateToAsync(Type viewModelType, object parameter)  
{  
    Page page = CreatePage(viewModelType, parameter);  

    if (page is LoginView)  
    {  
        Application.Current.MainPage = new CustomNavigationView(page);  
    }  
    else  
    {  
        var navigationPage = Application.Current.MainPage as CustomNavigationView;  
        if (navigationPage != null)  
        {  
            await navigationPage.PushAsync(page);  
        }  
        else  
        {  
            Application.Current.MainPage = new CustomNavigationView(page);  
        }  
    }  

    await (page.BindingContext as ViewModelBase).InitializeAsync(parameter);  
}  

private Type GetPageTypeForViewModel(Type viewModelType)  
{  
    var viewName = viewModelType.FullName.Replace("Model", string.Empty);  
    var viewModelAssemblyName = viewModelType.GetTypeInfo().Assembly.FullName;  
    var viewAssemblyName = string.Format(  
                CultureInfo.InvariantCulture, "{0}, {1}", viewName, viewModelAssemblyName);  
    var viewType = Type.GetType(viewAssemblyName);  
    return viewType;  
}  

private Page CreatePage(Type viewModelType, object parameter)  
{  
    Type pageType = GetPageTypeForViewModel(viewModelType);  
    if (pageType == null)  
    {  
        throw new Exception($"Cannot locate page type for {viewModelType}");  
    }  

    Page page = Activator.CreateInstance(pageType) as Page;  
    return page;  
}
```

Метод `InternalNavigateToAsync` выполняет переход к модели представления, сначала вызывая метод `CreatePage`. Этот метод находит представление, соответствующее указанному типу модели представления, и создает и возвращает экземпляр этого типа представления. Поиск представления, соответствующего типу модели представления, использует подход на основе соглашения, который предполагает, что:

- Представления находятся в той же сборке, что и типы моделей представления.
- Представления находятся в. Дочернее пространство имен views.
- Модели представления находятся в. Дочернее пространство имен ViewModel.
- Имена представлений соответствуют именам моделей представления с удаленным "моделью".

При создании экземпляра представления он связывается с соответствующей моделью представления. Дополнительные сведения о том, как это происходит, см. в разделе [Автоматическое создание модели представления с локатором модели представления](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

Если создаваемое представление является `LoginView`, оно упаковывается внутри нового экземпляра класса `CustomNavigationView` и назначается свойству [`Application.Current.MainPage`](xref:Xamarin.Forms.Application.MainPage) . В противном случае извлекается экземпляр `CustomNavigationView` и при условии, что он не равен null, вызывается метод [`PushAsync`](xref:Xamarin.Forms.NavigationPage) для отправки представления, создаваемого в стеке навигации. Однако если полученный `CustomNavigationView` экземпляр `null`, создаваемое представление упаковывается внутри нового экземпляра класса `CustomNavigationView` и назначается свойству `Application.Current.MainPage`. Этот механизм гарантирует, что при переходе страницы будут правильно добавляться в стек навигации, когда он пуст и когда он содержит данные.

> [!TIP]
> Рассмотрите возможность кэширования страниц. Кэширование страниц приводит к потреблению памяти для представлений, которые в настоящее время не отображаются. Однако без кэширования страницы это означает, что синтаксический анализ XAML и создание страницы и ее модели представления будут выполняться каждый раз при переходе к новой странице, что может оказать влияние на производительность сложной страницы. Для хорошо спроектированной страницы, которая не использует слишком много элементов управления, производительность должна быть достаточной. Однако кэширование страниц может помочь в случае возникновения длительной загрузки страниц.

После создания представления и перехода к нему выполняется метод `InitializeAsync` модели представления, связанной с представлением. Дополнительные сведения см. в разделе [Передача параметров во время навигации](#passing_parameters_during_navigation).

<a name="navigating_when_the_app_is_launched" />

### <a name="navigating-when-the-app-is-launched"></a>Навигация при запуске приложения

При запуске приложения вызывается метод `InitNavigation` в классе `App`. Этот метод показан в следующем примере кода:

```csharp
private Task InitNavigation()  
{  
    var navigationService = ViewModelLocator.Resolve<INavigationService>();  
    return navigationService.InitializeAsync();  
}
```

Метод создает новый объект `NavigationService` в контейнере вставки зависимостей Autofac и возвращает ссылку на него перед вызовом метода `InitializeAsync`.

> [!NOTE]
> Когда `INavigationService` интерфейс разрешается классом `ViewModelBase`, контейнер возвращает ссылку на объект `NavigationService`, который был создан при вызове метода Инитнавигатион.

В следующем примере кода показан метод `NavigationService` `InitializeAsync`:

```csharp
public Task InitializeAsync()  
{  
    if (string.IsNullOrEmpty(Settings.AuthAccessToken))  
        return NavigateToAsync<LoginViewModel>();  
    else  
        return NavigateToAsync<MainViewModel>();  
}
```

Переход к `MainView` осуществляется, если приложение имеет кэшированный маркер доступа, который используется для проверки подлинности. В противном случае выполняется переход к `LoginView`.

Дополнительные сведения о контейнере внедрения зависимостей Autofac см. [в разделе Введение в внедрение зависимостей](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

<a name="passing_parameters_during_navigation" />

### <a name="passing-parameters-during-navigation"></a>Передача параметров во время навигации

Один из `NavigateToAsync` методов, указанных в интерфейсе `INavigationService`, позволяет указать данные навигации в качестве аргумента, который передается в модель представления, к которой осуществляется переход, где обычно используется для выполнения инициализации.

Например, класс `ProfileViewModel` содержит `OrderDetailCommand`, который выполняется, когда пользователь выбирает порядок на странице `ProfileView`. В свою очередь, выполняет метод `OrderDetailAsync`, который показан в следующем примере кода:

```csharp
private async Task OrderDetailAsync(Order order)  
{  
    await NavigationService.NavigateToAsync<OrderDetailViewModel>(order);  
}
```

Этот метод вызывает навигацию в `OrderDetailViewModel`, передавая экземпляр `Order`, который представляет порядок, выбранный пользователем на странице `ProfileView`. Когда класс `NavigationService` создает `OrderDetailView`, создается экземпляр класса `OrderDetailViewModel` и ему назначается [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)представления. После перехода к `OrderDetailView`метод `InternalNavigateToAsync` выполняет метод `InitializeAsync` модели представления, связанной с представлением.

Метод `InitializeAsync` определяется в классе `ViewModelBase` как метод, который можно переопределить. Этот метод задает `object` аргумент, представляющий данные, передаваемые в модель представления во время операции навигации. Поэтому Просмотрите классы моделей, которые хотят получить данные из операции перехода, и предоставьте собственную реализацию метода `InitializeAsync`, чтобы выполнить необходимую инициализацию. В следующем примере кода показан метод `InitializeAsync` из класса `OrderDetailViewModel`:

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    if (navigationData is Order)  
    {  
        ...  
        Order = await _ordersService.GetOrderAsync(  
                        Convert.ToInt32(order.OrderNumber), authToken);  
        ...  
    }  
}
```

Этот метод извлекает экземпляр `Order`, который был передан в модель представления во время операции навигации, и использует его для получения полных сведений о заказе из экземпляра `OrderService`.

<a name="invoking_navigation_using_behaviors" />

### <a name="invoking-navigation-using-behaviors"></a>Вызов навигации с помощью поведений

Навигация обычно запускается из представления с помощью взаимодействия с пользователем. Например, `LoginView` выполняет навигацию после успешной проверки подлинности. В следующем примере кода показано, как вызывается Навигация с помощью поведения:

```xaml
<WebView ...>  
    <WebView.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="Navigating"  
            EventArgsConverter="{StaticResource WebNavigatingEventArgsConverter}"  
            Command="{Binding NavigateCommand}" />  
    </WebView.Behaviors>  
</WebView>
```

Во время выполнения `EventToCommandBehavior` будет реагировать на взаимодействие с [`WebView`](xref:Xamarin.Forms.WebView). Когда `WebView` переходит на веб-страницу, будет срабатывать событие [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) , которое будет выполнять `NavigateCommand` в `LoginViewModel`. По умолчанию аргументы события для события передаются в команду. Эти данные преобразуются при передаче между источником и целевым объектом преобразователем, заданным в свойстве `EventArgsConverter`, которое возвращает [`Url`](xref:Xamarin.Forms.WebNavigationEventArgs.Url) из [`WebNavigatingEventArgs`](xref:Xamarin.Forms.WebNavigatingEventArgs). Таким образом, при выполнении `NavigationCommand` URL-адрес страницы передается в качестве параметра зарегистрированной `Action`.

В свою очередь, `NavigationCommand` выполняет метод `NavigateAsync`, который показан в следующем примере кода:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...          
    await NavigationService.NavigateToAsync<MainViewModel>();  
    await NavigationService.RemoveLastFromBackStackAsync();  
    ...  
}
```

Этот метод вызывает навигацию на `MainViewModel`, а после навигации Удаляет страницу `LoginView` из стека навигации.

### <a name="confirming-or-cancelling-navigation"></a>Подтверждение или отмена навигации

Приложению может потребоваться взаимодействовать с пользователем во время операции навигации, чтобы пользователь мог подтвердить или отменить навигацию. Это может быть необходимо, например, когда пользователь пытается выполнить переход до полного заполнения страницы ввода данных. В этом случае приложение должно предоставить уведомление, позволяющее пользователю перейти с страницы или отменить операцию перехода до ее возникновения. Это можно сделать в классе модели представления с помощью ответа от уведомления, чтобы определить, вызывается ли переход.

## <a name="summary"></a>Сводка

Xamarin. Forms включает поддержку навигации по страницам, что обычно приводит к взаимодействию пользователя с пользовательским ИНТЕРФЕЙСом или из самого приложения в результате внутренних изменений состояния, управляемых логическими операциями. Однако Навигация может быть сложной для реализации в приложениях, использующих шаблон MVVM.

В этой главе представлен класс `NavigationService`, который используется для выполнения навигации по модели представления в режиме предварительного просмотра. Размещение логики навигации в классах представления позволяет выполнять логику с помощью автоматических тестов. Кроме того, модель представления может реализовать логику для управления навигацией, чтобы обеспечить применение определенных бизнес-правил.

## <a name="related-links"></a>Связанные ссылки

- [Скачать электронную книгу (2 МБ в формате PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (пример)](https://github.com/dotnet-architecture/eShopOnContainers)
