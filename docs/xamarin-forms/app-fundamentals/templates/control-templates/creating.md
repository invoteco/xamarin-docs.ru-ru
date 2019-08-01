---
title: Создание шаблона ControlTemplate
description: Шаблоны элементов управления можно определять на уровне приложения или на уровне страницы. В этой статье содержатся сведения о создании и использовании шаблонов элементов управления.
ms.prod: xamarin
ms.assetid: A9AEB052-FBF5-4589-9BD4-6D6F62BED7F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/14/2019
ms.openlocfilehash: fbc966fdf1d79ecc9794d2156db81b583694ce36
ms.sourcegitcommit: 84764b9c51e769d6d6570a362af8451607c7e0d2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68665686"
---
# <a name="create-a-controltemplate"></a>Создание шаблона ControlTemplate

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-controltemplates-simpletheme)

_Шаблоны элементов управления можно определять на уровне приложения или на уровне страницы. В этой статье содержатся сведения о создании и использовании шаблонов элементов управления._

## <a name="create-a-controltemplate-in-xaml"></a>Создание шаблона ControlTemplate в XAML

Чтобы определить шаблон [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) на уровне приложения, необходимо добавить [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) в класс `App`. По умолчанию все приложения Xamarin.Forms, создаваемые на основе шаблона, используют класс **App** для реализации подкласса [`Application`](xref:Xamarin.Forms.Application). Чтобы объявить шаблон `ControlTemplate` на уровне приложения в словаре `ResourceDictionary` приложения на основе XAML, необходимо заменить класс **App** по умолчанию классом **App** XAML и соответствующим кодом программной части, как показано в следующем примере кода.

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.App">
    <Application.Resources>
        <ResourceDictionary>
            <ControlTemplate x:Key="TealTemplate">
                <Grid>
                    ...
                    <BoxView ... />
                    <Label Text="Control Template Demo App"
                           TextColor="White"
                           VerticalOptions="Center" ... />
                    <ContentPresenter ... />
                    <BoxView Color="Teal" ... />
                    <Label Text="(c) Xamarin 2016"
                           TextColor="White"
                           VerticalOptions="Center" ... />
                </Grid>
            </ControlTemplate>
            <ControlTemplate x:Key="AquaTemplate">
                ...
            </ControlTemplate>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Каждый экземпляр [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) создается как повторно используемый объект в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary).  Для этого каждому объявлению присваивается уникальный атрибут `x:Key`, который предоставляет описательный ключ в `ResourceDictionary`.

В следующем примере показан соответствующий код программной части класса `App`.

```csharp
public partial class App : Application
{
  public App ()
  {
    InitializeComponent ();
    MainPage = new HomePage ();
  }
}
```

Помимо задания свойства [`MainPage`](xref:Xamarin.Forms.Application.MainPage), в коде программной части должен вызываться метод `InitializeComponent` для загрузки и анализа соответствующего кода XAML.

В следующем примере кода показан объект [`ContentPage`](xref:Xamarin.Forms.ContentPage), который применяет `TealTemplate` к [`ContentView`](xref:Xamarin.Forms.ContentView).

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0"
                 ControlTemplate="{StaticResource TealTemplate}">
        <StackLayout VerticalOptions="CenterAndExpand">
            <Label Text="Welcome to the app!" HorizontalOptions="Center" />
            <Button Text="Change Theme" Clicked="OnButtonClicked" />
        </StackLayout>
    </ContentView>
</ContentPage>
```

Шаблон `TealTemplate` присваивается свойству [`ContentView.ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate) с помощью расширения разметки `StaticResource`. Свойству [`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content) присваивается экземпляр [`StackLayout`](xref:Xamarin.Forms.StackLayout), который определяет содержимое, отображаемое на странице [`ContentPage`](xref:Xamarin.Forms.ContentPage). Оно будет отображаться объектом [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter), содержащимся в шаблоне `TealTemplate`. Результат показан на следующих снимках экрана.

![](creating-images/teal-theme.png "Сине-зеленый шаблон элемента управления")

### <a name="re-theming-an-application-at-runtime"></a>Изменение темы приложения во время выполнения

При нажатии кнопки **Change Theme** (Сменить тему) выполняется метод `OnButtonClicked`, который показан в следующем примере кода.

```csharp
void OnButtonClicked (object sender, EventArgs e)
{
  originalTemplate = !originalTemplate;
  contentView.ControlTemplate = (originalTemplate) ? tealTemplate : aquaTemplate;
}
```

Этот метод заменяет активный экземпляр [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) на альтернативный экземпляр `ControlTemplate`. Результат показан на следующем снимке экрана.

![](creating-images/aqua-theme.png "Темно-бирюзовый шаблон элемента управления")

> [!NOTE]
> На странице `ContentPage` могут быть одновременно заданы свойства `Content` и `ControlTemplate`. В этом случае, если шаблон `ControlTemplate` содержит экземпляр `ContentPresenter`, содержимое, присвоенное свойству `Content`, будет отображаться объектом `ContentPresenter` в `ControlTemplate`.

### <a name="set-a-controltemplate-with-a-style"></a>Настройка шаблона ControlTemplate с использованием стиля

Шаблон [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) можно также применять с помощью стиля [`Style`](xref:Xamarin.Forms.Style) для расширения возможностей настройки тем. Для этого можно создать *неявный* или *явный* стиль для целевого представления в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) и задать свойство `ControlTemplate` целевого представления в экземпляре [`Style`](xref:Xamarin.Forms.Style). В следующем примере кода показан *неявный* стиль, добавленный в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) на уровне приложения.

```xaml
<Style TargetType="ContentView">
    <Setter Property="ControlTemplate" Value="{StaticResource TealTemplate}" />
</Style>
```

Так как экземпляр [`Style`](xref:Xamarin.Forms.Style) *неявный*, он применяется ко всем экземплярам `ContentView` в приложении. Поэтому задавать свойство [`ContentView.ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate) не нужно, как показано в следующем примере кода.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0">
      ...
    </ContentView>
</ContentPage>
```

Дополнительные сведения о стилях см. в статье [Стили](~/xamarin-forms/user-interface/styles/index.md).

### <a name="create-a-controltemplate-at-page-level"></a>Создание шаблона ControlTemplate на уровне страницы

Экземпляры [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) можно создавать не только на уровне приложения, но и на уровне страницы, как показано в следующем примере кода.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentPage.Resources>
        <ResourceDictionary>
            <ControlTemplate x:Key="TealTemplate">
                ...
            </ControlTemplate>
            <ControlTemplate x:Key="AquaTemplate">
                ...
            </ControlTemplate>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
        ...
    </ContentView>
</ContentPage>
```

Когда шаблон [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) добавляется на уровне страницы, объект [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) добавляется в [`ContentPage`](xref:Xamarin.Forms.ContentPage), после чего экземпляры `ControlTemplate` включаются в `ResourceDictionary`.

## <a name="create-a-controltemplate-in-c35"></a>Создание шаблона ControlTemplate в C&#35;

Чтобы определить шаблон [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) на уровне приложения, необходимо создать `class`, представляющий `ControlTemplate`. Этот класс должен быть производным от [макета](~/xamarin-forms/user-interface/layouts/index.md), используемого для шаблона, как показано в следующем примере кода.

```csharp
class TealTemplate : Grid
{
  public TealTemplate ()
  {
    ...
    var contentPresenter = new ContentPresenter ();
    Children.Add (contentPresenter, 0, 1);
    Grid.SetColumnSpan (contentPresenter, 2);
    ...
  }
}

class AquaTemplate : Grid
{
  ...
}
```

Класс `AquaTemplate` идентичен классу `TealTemplate` за тем исключением, что для свойств [`BoxView.Color`](xref:Xamarin.Forms.BoxView.Color) и [`Label.TextColor`](xref:Xamarin.Forms.Label.TextColor) используются другие цвета.

В следующем примере кода показан объект [`ContentPage`](xref:Xamarin.Forms.ContentPage), который применяет `TealTemplate` к [`ContentView`](xref:Xamarin.Forms.ContentView).

```csharp
public class HomePageCS : ContentPage
{
  ...
  ControlTemplate tealTemplate = new ControlTemplate (typeof(TealTemplate));
  ControlTemplate aquaTemplate = new ControlTemplate (typeof(AquaTemplate));

  public HomePageCS ()
  {
    var button = new Button { Text = "Change Theme" };
    var contentView = new ContentView {
      Padding = new Thickness (0, 20, 0, 0),
      Content = new StackLayout {
        VerticalOptions = LayoutOptions.CenterAndExpand,
        Children = {
          new Label { Text = "Welcome to the app!", HorizontalOptions = LayoutOptions.Center },
          button
        }
      },
      ControlTemplate = tealTemplate
    };
    ...
    Content = contentView;
  }
}
```

Экземпляры [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) создаются путем указания типа классов, которые определяют шаблоны элементов управления, в конструкторе `ControlTemplate`.

Свойству [`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content) присваивается экземпляр [`StackLayout`](xref:Xamarin.Forms.StackLayout), который определяет содержимое, отображаемое на странице [`ContentPage`](xref:Xamarin.Forms.ContentPage). Оно будет отображаться объектом [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter), содержащимся в шаблоне `TealTemplate`. Для изменения темы на `AquaTheme` во время выполнения используется описанный выше механизм.

## <a name="get-a-named-element-from-a-template"></a>Извлечение именованного элемента из шаблона

Когда экземпляр шаблона элемента управления будет создан, из него можно извлекать именованные элементы. Это можно сделать с помощью метода `GetTemplateChild`, который возвращает именованный элемент в созданном экземпляре визуального дерева [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate).

Когда экземпляр шаблона элемента управления будет создан, вызовется метод шаблона `OnApplyTemplate`. Следовательно, метод `GetTemplateChild` должен вызываться из переопределения `OnApplyTemplate` на производной странице [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage), например [`ContentPage`](xref:Xamarin.Forms.ContentPage), или в производном представлении [`TemplatedView`](xref:Xamarin.Forms.TemplatedView), например [`ContentView`](xref:Xamarin.Forms.ContentView).

> [!IMPORTANT]
> Метод `GetTemplateChild` должен вызываться только после вызова метода `OnApplyTemplate`.

В следующем примере показан шаблон пользовательского элемента управления:

```xaml
<controls:MyCustomControl ...>
    <controls:MyCustomControl.ControlTemplate>
         <ControlTemplate>
              <Label x:Name="myLabel" />
         </ControlTemplate>
    <controls:MyCustomControl.ControlTemplate>
</controls:MyCustomControl>
```

Элемент [`Label`](xref:Xamarin.Forms.Label) именованный, поэтому его можно извлечь из кода программной части для пользовательского элемента управления. Для этого нужно вызвать метод `GetTemplateChild` из переопределения `OnApplyTemplate` для пользовательского элемента управления:

```csharp
class MyCustomControl : ContentView
{
    Label myLabel;

    protected override void OnApplyTemplate()
    {  
        myLabel = GetTemplateChild("myLabel");
    }
    //...
}
```

В этом примере извлекается объект [`Label`](xref:Xamarin.Forms.Label) с именем `myLabel`. После этого объект `myLabel` станет доступным для использования классом `MyCustomControl`.

## <a name="related-links"></a>Связанные ссылки

- [Стили](~/xamarin-forms/user-interface/styles/index.md)
- [Простая тема (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-controltemplates-simpletheme)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentPresenter](xref:Xamarin.Forms.ContentPresenter)
- [ContentView](xref:Xamarin.Forms.ContentView)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
