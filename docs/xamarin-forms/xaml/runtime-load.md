---
title: Загрузка XAML во время выполнения в Xamarin. Forms
description: XAML можно загрузить и проанализировать во время выполнения с помощью методов расширения Лоадфромксамл.
ms.prod: xamarin
ms.assetid: 25F73FBF-2DD3-468E-A2D8-0897414F0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/12/2018
ms.openlocfilehash: 71f510cd37d4bed2a5a6077ed63f748ce9894725
ms.sourcegitcommit: ae5557c5024d4b7bd52b2f33cb96114ce2b8e086
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/06/2020
ms.locfileid: "77045069"
---
# <a name="loading-xaml-at-runtime-in-xamarinforms"></a>Загрузка XAML во время выполнения в Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-loadruntimexaml)

Пространство имен [`Xamarin.Forms.Xaml`](xref:Xamarin.Forms.Xaml) содержит два метода расширения [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) , которые можно использовать для загрузки и синтаксического анализа XAML во время выполнения.

## <a name="background"></a>Историческая справка

При создании класса XAML Xamarin. Forms метод [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) вызывается неявно. Это происходит потому, что файл кода программной части для класса XAML вызывает метод `InitializeComponent` из своего конструктора:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
}
```

Когда Visual Studio создает проект, содержащий XAML-файл, он анализирует XAML-файл для создания файла C# кода (например, **MainPage.XAML.g.CS**), содержащего определение метода `InitializeComponent`:

```csharp
private void InitializeComponent()
{
    global::Xamarin.Forms.Xaml.Extensions.LoadFromXaml(this, typeof(MainPage));
    ...
}
```

Метод `InitializeComponent` вызывает метод [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) , чтобы извлечь XAML-файл (или его скомпилированный двоичный файл) из библиотеки .NET Standard. После извлечения он инициализирует все объекты, определенные в файле XAML, соединяет их вместе в отношениях типа «родители-потомки», присоединяет обработчики событий, определенные в коде к событиям, заданным в файле XAML, и устанавливает результирующее дерево объектов в качестве содержимого элемента страниц.

## <a name="loading-xaml-at-runtime"></a>Загрузка XAML во время выполнения

[`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) методы `public`, поэтому их можно вызывать из приложений Xamarin. Forms для загрузки и анализировать XAML во время выполнения. Это позволяет выполнять такие сценарии, как приложение, которое загружает XAML из веб-службы, создает требуемое представление из XAML и отображает его в приложении.

> [!WARNING]
> Загрузка XAML во время выполнения имеет значительные затраты на производительность, и обычно ее следует избегать.

В следующем примере кода показано простое использование:

```csharp
using Xamarin.Forms.Xaml;
...

string navigationButtonXAML = "<Button Text=\"Navigate\" />";
Button navigationButton = new Button().LoadFromXaml(navigationButtonXAML);
...
_stackLayout.Children.Add(navigationButton);
```

В этом примере создается экземпляр [`Button`](xref:Xamarin.Forms.Button) со значением свойства [`Text`](xref:Xamarin.Forms.Button.Text) , которое ЗАдается из XAML, определенного в `string`. Затем `Button` добавляется в [`StackLayout`](xref:Xamarin.Forms.StackLayout) , который был ОПРЕДЕЛЕН в XAML для страницы.

> [!NOTE]
> Методы расширения [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) позволяют указать аргумент универсального типа. Однако нередко требуется указывать аргумент типа, так как он будет выведен из типа экземпляра, на котором он работает.

Метод [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) можно использовать для расширения любого XAML-кода, а следующий пример — для расширения [`ContentPage`](xref:Xamarin.Forms.ContentPage) и перехода к нему:

```csharp
using Xamarin.Forms.Xaml;
...

// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n</ContentPage>";

ContentPage page = new ContentPage().LoadFromXaml(pageXAML);
await Navigation.PushAsync(page);
```

## <a name="accessing-elements"></a>Доступ к элементам

Загрузка XAML во время выполнения с помощью метода [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) не допускайте строго типизированного доступа к элементам XAML, которые имеют указанные имена объектов среды выполнения (с помощью `x:Name`). Однако эти элементы XAML можно получить с помощью метода [`FindByName`](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) , а затем получить доступ по мере необходимости:

```csharp
// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n<StackLayout>\n<Label x:Name=\"monkeyName\"\n />\n</StackLayout>\n</ContentPage>";
ContentPage page = new ContentPage().LoadFromXaml(pageXAML);

Label monkeyLabel = page.FindByName<Label>("monkeyName");
monkeyLabel.Text = "Seated Monkey";
...
```

В этом примере XAML для [`ContentPage`](xref:Xamarin.Forms.ContentPage) является неизменным. Этот код XAML включает [`Label`](xref:Xamarin.Forms.Label) с именем `monkeyName`, который извлекается с помощью метода [`FindByName`](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) , прежде чем его свойство [`Text`](xref:Xamarin.Forms.Label.Text) задается.

## <a name="related-links"></a>Связанные ссылки

- [Лоадрунтимексамл (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-loadruntimexaml)
