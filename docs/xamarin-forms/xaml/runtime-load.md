---
title: Загрузка XAML во время выполнения в Xamarin. Forms
description: XAML можно загрузить и проанализировать во время выполнения с помощью методов расширения Лоадфромксамл.
ms.prod: xamarin
ms.assetid: 25F73FBF-2DD3-468E-A2D8-0897414F0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/12/2018
ms.openlocfilehash: e253d2ba949a94637d7773fdc50b479679fd3f41
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68657257"
---
# <a name="loading-xaml-at-runtime-in-xamarinforms"></a>Загрузка XAML во время выполнения в Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-loadruntimexaml)

Пространство имен содержит два [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) метода расширения, которые можно использовать для загрузки и синтаксического анализа XAML во время выполнения. [`Xamarin.Forms.Xaml`](xref:Xamarin.Forms.Xaml)

## <a name="background"></a>Фон

При создании класса XAML Xamarin. Forms вызывается метод [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) с прямым вызовом. Это происходит потому, что файл кода программной части для класса XAML вызывает `InitializeComponent` метод из своего конструктора:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
}
```

Когда Visual Studio создает проект, содержащий XAML-файл, он анализирует XAML-файл для создания файла C# кода (например, **MainPage.XAML.g.CS**), содержащего `InitializeComponent` определение метода:

```csharp
private void InitializeComponent()
{
    global::Xamarin.Forms.Xaml.Extensions.LoadFromXaml(this, typeof(MainPage));
    ...
}
```

`InitializeComponent` [Метод`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) вызывает метод, чтобы извлечь XAML-файл (или его скомпилированный двоичный файл) из библиотеки .NET Standard. После извлечения он инициализирует все объекты, определенные в файле XAML, соединяет их вместе в отношениях типа «родители-потомки», присоединяет обработчики событий, определенные в коде к событиям, заданным в файле XAML, и устанавливает результирующее дерево объектов в качестве содержимого элемента страниц.

## <a name="loading-xaml-at-runtime"></a>Загрузка XAML во время выполнения

[`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) Методы являются`public`, и поэтому их можно вызывать из приложений Xamarin. Forms для загрузки, а также анализировать XAML во время выполнения. Это позволяет выполнять такие сценарии, как приложение, которое загружает XAML из веб-службы, создает требуемое представление из XAML и отображает его в приложении.

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

В этом примере [`Button`](xref:Xamarin.Forms.Button) создается экземпляр, а [`Text`](xref:Xamarin.Forms.Button.Text) его значение свойства задается из XAML, определенного в `string`. Затем добавляется в объект [`StackLayout`](xref:Xamarin.Forms.StackLayout) , который был определен в XAML для страницы. `Button`

> [!NOTE]
> Методы [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) расширения позволяют указать аргумент универсального типа. Однако нередко требуется указывать аргумент типа, так как он будет выведен из типа экземпляра, на котором он работает.

Метод можно использовать для расширения любого XAML-кода, в следующем примере — [`ContentPage`](xref:Xamarin.Forms.ContentPage) путем расширения и перехода к нему. [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)

```csharp
using Xamarin.Forms.Xaml;
...

// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n</ContentPage>";

ContentPage page = new ContentPage().LoadFromXaml(pageXAML);
await Navigation.PushAsync(page);
```

## <a name="accessing-elements"></a>Доступ к элементам

Загрузка XAML во время выполнения с [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) помощью метода не допускайте строго типизированного доступа к элементам XAML, которые имеют указанные имена объектов среды `x:Name`выполнения (с использованием). Однако эти элементы XAML можно получить с помощью [`FindByName`](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) метода, а затем получить к ним доступ по мере необходимости:

```csharp
// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n<StackLayout>\n<Label x:Name=\"monkeyName\"\n />\n</StackLayout>\n</ContentPage>";
ContentPage page = new ContentPage().LoadFromXaml(pageXAML);

Label monkeyLabel = page.FindByName<Label>("monkeyName");
monkeyLabel.Text = "Seated Monkey";
...
```

В этом примере XAML для для [`ContentPage`](xref:Xamarin.Forms.ContentPage) является неизменным. Этот XAML-код [`Label`](xref:Xamarin.Forms.Label) включает `monkeyName`именованный объект, [`FindByName`](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) который извлекается с помощью метода до [`Text`](xref:Xamarin.Forms.Label.Text) установки свойства.

## <a name="related-links"></a>Связанные ссылки

- [Лоадрунтимексамл (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-loadruntimexaml)
