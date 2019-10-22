---
title: Интерактивное взаимодействие с ListView
description: В этой статье объясняется, как добавить интерактивность в ListView Xamarin. Forms путем реализации выбора, контекстных действий и получения обновления.
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/25/2019
ms.openlocfilehash: aa717792bdaefe24d957c9781934933b67aaf92b
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696872"
---
# <a name="listview-interactivity"></a>Интерактивное взаимодействие с ListView

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)

Класс Xamarin. Forms [`ListView`](xref:Xamarin.Forms.ListView) поддерживает взаимодействие пользователя с данными, которые он представляет.

## <a name="selection-and-taps"></a>Выделение и касания

Режим выбора [`ListView`](xref:Xamarin.Forms.ListView) контролируется путем присвоения свойству [`ListView.SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) значения перечисления [`ListViewSelectionMode`](xref:Xamarin.Forms.ListViewSelectionMode) .

- [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) указывает, что можно выбрать один элемент, выделив выделенный элемент. Это значение используется по умолчанию.
- [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) указывает, что элементы не могут быть выбраны.

Когда пользователь касается элемента, запускаются два события:

- [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) срабатывает при выборе нового элемента.
- [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) срабатывает при касании элемента.

Если коснуться одного и того же элемента дважды, будет срабатывать два [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) событий, но будет запущено только одно событие [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) .

> [!NOTE]
> Класс [`ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs) , который содержит аргументы события для события [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) , имеет свойства [`Group`](xref:Xamarin.Forms.ItemTappedEventArgs.Group) и [`Item`](xref:Xamarin.Forms.ItemTappedEventArgs.Item) , а также свойство `ItemIndex`, значение которого представляет индекс в [0е](xref:Xamarin.Forms.ListView) элемента с касанием. Аналогично, класс [`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs) , который содержит аргументы события для события [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) , имеет свойство [`SelectedItem`](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) и свойство `SelectedItemIndex`, значение которого представляет индекс в `ListView` выбранного элемента.

Если свойство [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) имеет значение [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single), можно выбрать элементы в [`ListView`](xref:Xamarin.Forms.ListView) , будут срабатывать события [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) и [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) , а для свойства [1](xref:Xamarin.Forms.ListView.SelectedItem) будет задано значение выбранного элемента. детализирован.

Если свойство [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) имеет значение [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None), то элементы в [`ListView`](xref:Xamarin.Forms.ListView) не могут быть выбраны, событие [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) не будет запущено, а свойство [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) останется 0. Тем не менее, [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) события по-прежнему будут срабатывать, и при касании элемент будет кратковременно выделен.

При выборе элемента и изменении свойства [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) с [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) на [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None)свойству [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) будет присвоено значение `null`, а событие [0](xref:Xamarin.Forms.ListView.ItemSelected) будет запущено с элементом 1.

На следующих снимках экрана показан [`ListView`](xref:Xamarin.Forms.ListView) с режимом выбора по умолчанию:

![](interactivity-images/selection-default.png "ListView with Selection Enabled")

### <a name="disable-selection"></a>Отключить выделение

Чтобы отключить выбор [`ListView`](xref:Xamarin.Forms.ListView) установите для свойства [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) значение [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None).

```xaml
<ListView ... SelectionMode="None" />
```

```csharp
var listView = new ListView { ... SelectionMode = ListViewSelectionMode.None };
```

## <a name="context-actions"></a>Контекстные действия

Часто пользователи хотят предпринять действия с элементом в `ListView`. Например, рассмотрим список сообщений электронной почты в почтовом приложении. В iOS можно прокрутить, чтобы удалить сообщение:

![](interactivity-images/context-default.png "ListView with Context Actions")

Контекстные действия могут быть реализованы в C# и XAML. Ниже вы найдете конкретные руководства для обоих, но сначала рассмотрим некоторые основные сведения о реализации для обоих.

Действия контекста создаются с помощью элементов `MenuItem`. События TAP для `MenuItems` объекты вызываются самим `MenuItem`, а не `ListView`. Это отличается от того, как события TAP обрабатываются для ячеек, где `ListView` вызывает событие, а не ячейку. Поскольку `ListView` вызывает событие, его обработчик событий получает ключевые сведения, в том числе выбор или касание элемента.

По умолчанию `MenuItem` не имеет возможности узнать, к какой ячейке она принадлежит. Свойство `CommandParameter` доступно в `MenuItem` для хранения объектов, таких как объект за `ViewCell` `MenuItem`. Свойство `CommandParameter` может быть задано как в XAML, C#так и в.

### <a name="xaml"></a>XAML

`MenuItem` элементы могут быть созданы в коллекции XAML. В XAML-коде ниже показана пользовательская ячейка с двумя реализованными контекстными действиями:

```xaml
<ListView x:Name="ContextDemoList">
  <ListView.ItemTemplate>
    <DataTemplate>
      <ViewCell>
         <ViewCell.ContextActions>
            <MenuItem Clicked="OnMore"
                      CommandParameter="{Binding .}"
                      Text="More" />
            <MenuItem Clicked="OnDelete"
                      CommandParameter="{Binding .}"
                      Text="Delete" IsDestructive="True" />
         </ViewCell.ContextActions>
         <StackLayout Padding="15,0">
              <Label Text="{Binding title}" />
         </StackLayout>
      </ViewCell>
    </DataTemplate>
  </ListView.ItemTemplate>
</ListView>
```

Убедитесь, что в файле кода программной части реализованы методы `Clicked`:

```csharp
public void OnMore (object sender, EventArgs e)
{
    var mi = ((MenuItem)sender);
    DisplayAlert("More Context Action", mi.CommandParameter + " more context action", "OK");
}

public void OnDelete (object sender, EventArgs e)
{
    var mi = ((MenuItem)sender);
    DisplayAlert("Delete Context Action", mi.CommandParameter + " delete context action", "OK");
}
```

> [!NOTE]
> @No__t_0 для Android имеет переопределяемый метод `UpdateMenuItemIcon`, который можно использовать для загрузки значков из настраиваемого `Drawable`. Это переопределение позволяет использовать изображения SVG в качестве значков на `MenuItem` экземпляров в Android.

### <a name="code"></a>Код

Контекстные действия можно реализовать в любом подклассе `Cell` (если он не используется как заголовок группы), создав `MenuItem` экземпляры и добавив их в коллекцию `ContextActions` для ячейки. Для контекстного действия можно настроить следующие свойства:

- **Текст** &ndash; строку, которая отображается в пункте меню.
- **Щелчок** &ndash; событие при щелчке элемента.
- **Обратимое** &ndash; (необязательно) при значении true элемент отображается в iOS по-разному.

В ячейку можно добавить несколько контекстных действий, но только одна из них должна иметь `IsDestructive` установлен в значение `true`. В следующем коде показано, как можно добавить контекстные действия в `ViewCell`.

```csharp
var moreAction = new MenuItem { Text = "More" };
moreAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
moreAction.Clicked += async (sender, e) =>
{
    var mi = ((MenuItem)sender);
    Debug.WriteLine("More Context Action clicked: " + mi.CommandParameter);
};

var deleteAction = new MenuItem { Text = "Delete", IsDestructive = true }; // red background
deleteAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
deleteAction.Clicked += async (sender, e) =>
{
    var mi = ((MenuItem)sender);
    Debug.WriteLine("Delete Context Action clicked: " + mi.CommandParameter);
};
// add to the ViewCell's ContextActions property
ContextActions.Add (moreAction);
ContextActions.Add (deleteAction);
```

## <a name="pull-to-refresh"></a>Извлечь для обновления

Пользователи будут ждать, что список данных обновится. Элемент управления " [`ListView`](xref:Xamarin.Forms.ListView) " поддерживает это встроенное. Чтобы включить функцию получения обновлений, задайте для [`IsPullToRefreshEnabled`](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) значение `true`.

```xaml
<ListView ...
          IsPullToRefreshEnabled="true" />
```

Эквивалентный код на C# выглядит так:

```csharp
listView.IsPullToRefreshEnabled = true;
```

Счетчик отображается во время обновления, который по умолчанию является черным. Однако цвет счетчика можно изменить в iOS и Android, задав для свойства `RefreshControlColor` [`Color`](xref:Xamarin.Forms.Color).

```xaml
<ListView ...
          IsPullToRefreshEnabled="true"
          RefreshControlColor="Red" />
```

Эквивалентный код на C# выглядит так:

```csharp
listView.RefreshControlColor = Color.Red;
```

На следующих снимках экрана показан запрос на обновление по мере извлечения пользователя:

![](interactivity-images/refresh-start.png "ListView Pull to Refresh In-Progress")

На следующих снимках экрана показан запрос на обновление после того, как пользователь освободил запрос на вытягивание, а при обновлении [`ListView`](xref:Xamarin.Forms.ListView) отображается счетчик.

![](interactivity-images/refresh-in-progress.png "ListView Pull to Refresh Complete")

[`ListView`](xref:Xamarin.Forms.ListView) запускает событие [`Refreshing`](xref:Xamarin.Forms.ListView.Refreshing) для запуска обновления, а для свойства [`IsRefreshing`](xref:Xamarin.Forms.ListView.IsRefreshing) устанавливается значение `true`. Любой код, необходимый для обновления содержимого `ListView`, должен затем выполняться обработчиком событий для события `Refreshing` или методом, выполняемым [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand). После обновления `ListView` свойству `IsRefreshing` должно быть присвоено значение `false`, или метод [`EndRefresh`](xref:Xamarin.Forms.ListView.EndRefresh) должен быть вызван, чтобы указать, что обновление завершено.

> [!NOTE]
> При определении [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand)можно указать метод `CanExecute` команды, чтобы включить или отключить команду.

## <a name="detect-scrolling"></a>Обнаружение прокрутки

[`ListView`](xref:Xamarin.Forms.ListView) определяет событие `Scrolled`, которое срабатывает для указания на то, что прокрутка выполнена. В следующем примере XAML показан `ListView`, который задает обработчик событий для события `Scrolled`:

```xaml
<ListView Scrolled="OnListViewScrolled">
    ...
</ListView>
```

Эквивалентный код на C# выглядит так:

```csharp
ListView listView = new ListView();
listView.Scrolled += OnListViewScrolled;
```

В этом примере кода обработчик событий `OnListViewScrolled` выполняется при срабатывании события `Scrolled`:

```csharp
void OnListViewScrolled(object sender, ScrolledEventArgs e)
{
    Debug.WriteLine("ScrollX: " + e.ScrollX);
    Debug.WriteLine("ScrollY: " + e.ScrollY);  
}
```

Обработчик событий `OnListViewScrolled` выводит значения объекта `ScrolledEventArgs`, сопровождающего событие.

## <a name="related-links"></a>Связанные ссылки

- [Взаимодействие с ListView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)
