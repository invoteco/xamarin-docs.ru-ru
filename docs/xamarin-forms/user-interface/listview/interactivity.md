---
title: Интерактивность ListView
description: В этой статье объясняется, как для обеспечения интерактивности ListView Xamarin.Forms, реализовав выбранные параметры, контекст действия и по запросу для обновления.
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/27/2019
ms.openlocfilehash: 3949dd85492a8181ee53e23b3ba2e986e59f8f47
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/28/2019
ms.locfileid: "70121624"
---
# <a name="listview-interactivity"></a>Интерактивность ListView

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)

[`ListView`](xref:Xamarin.Forms.ListView)поддерживает взаимодействие с данными, представленными в ней.

<a name="selectiontaps" />

## <a name="selection--taps"></a>Выбор & касания

[ `ListView` ](xref:Xamarin.Forms.ListView) Режим выбора управляется заданием [ `ListView.SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) свойство в значение [ `ListViewSelectionMode` ](xref:Xamarin.Forms.ListViewSelectionMode) перечисления:

- [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) Указывает, что элемент можно выбрать, с выбранным элементом выделена. Это значение по умолчанию.
- [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) Указывает, что элементы не могут быть выбраны.

При касании элемента, два события инициируются:

- [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) активируется, когда элемент выбран.
- [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) возникает при нажатии элемента.

При касании тот же элемент дважды будет срабатывать два [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) события, но будет только вызов одной [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) событий.

> [!NOTE]
> [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) [`Item`](xref:Xamarin.Forms.ItemTappedEventArgs.Item) [`Group`](xref:Xamarin.Forms.ItemTappedEventArgs.Group) `ItemIndex` Класс, который содержит аргументы события, имеет свойства [и`ListView`](xref:Xamarin.Forms.ListView) , и свойство, значение которого представляет индекс в элементе нажатого элемента. [`ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs) Аналогично, [`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs) класс, который содержит аргументы [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) события, имеет [`SelectedItem`](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) свойство и `SelectedItemIndex` свойство, значение которого представляет индекс в `ListView` выбранном элементе.

Когда [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) свойству [ `Single` ](xref:Xamarin.Forms.ListViewSelectionMode.Single), элементы в [ `ListView` ](xref:Xamarin.Forms.ListView) можно выбрать, [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) и [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) события порождаются и [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) свойству будет присвоено значение выбранного элемента.

Когда [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) свойству [ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None), элементы в [ `ListView` ](xref:Xamarin.Forms.ListView) не могут быть выбраны, [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) не будет создаваться событие и [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) свойство будет оставаться `null`. Тем не менее [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) события по-прежнему будет создаваться и полученные элемента будут кратко выделены во время прослушивания.

Если был выбран элемент и [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) свойство меняется с [ `Single` ](xref:Xamarin.Forms.ListViewSelectionMode.Single) для [ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None), [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) свойству будет присвоено `null` и [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) событие будет запускаться с `null` элемента.

Ниже показаны снимки экрана [ `ListView` ](xref:Xamarin.Forms.ListView) с режимом выделения по умолчанию:

![](interactivity-images/selection-default.png "ListView с выбором включена")

### <a name="disabling-selection"></a>Отключение выделения

Чтобы отключить [ `ListView` ](xref:Xamarin.Forms.ListView) набора [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) свойства [ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None):

```xaml
<ListView ... SelectionMode="None" />
```

```csharp
var listView = new ListView { ... SelectionMode = ListViewSelectionMode.None };
```

<a name="Context_Actions" />

## <a name="context-actions"></a>Контекстные действия

Часто пользователей потребуется выполнить действие на элемент в `ListView`. Например рассмотрим список адресов электронной почты в почтовом приложении. В iOS можно проведите, чтобы удалить сообщение::

![](interactivity-images/context-default.png "ListView с помощью контекстных действий")

Контекстные действия можно реализовать в C# и XAML. Далее вы найдете определенных направляющих для обоих, но сперва давайте взглянем на некоторые ключевые особенности реализации для обоих.

Контекстные действия создаются с помощью `MenuItem`s. События касания для MenuItems вызываются с помощью MenuItem, не ListView. Это отличается от обработки события касания для ячеек, где ListView инициирует событие, а не ячейку. Поскольку ListView порождает событие, соответствующий обработчик событий получает сведения о ключе, например которого выбран или касание элемента.

По умолчанию элемент меню не имеет возможности узнать, какая ячейка, в которой он принадлежит. `CommandParameter` можно найти в `MenuItem` для хранения объектов, таких как объект MenuItem ViewCell. `CommandParameter` можно задать в XAML и C#.

### <a name="c"></a>C\#

Контекстные действия может быть реализован ни в одном `Cell` подкласс (пока она не используется в качестве заголовка группы), создав `MenuItem`s и добавления их в `ContextActions` коллекции для ячейки. У вас есть следующие свойства можно настроить для контекста действия:

- **Текст** &ndash; строка, которая появляется в элементе меню.
- **Нажата** &ndash; события при щелчке элемента.
- **IsDestructive** &ndash; (Дополнительно) Если значение true, элемент отображается по-разному в iOS.

Несколько контекстных действий можно добавить в ячейку, однако должны иметь только один `IsDestructive` присвоено `true`. В следующем коде показано, как будут добавлены контекстные действия `ViewCell`:

```csharp
var moreAction = new MenuItem { Text = "More" };
moreAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
moreAction.Clicked += async (sender, e) => {
    var mi = ((MenuItem)sender);
    Debug.WriteLine("More Context Action clicked: " + mi.CommandParameter);
};

var deleteAction = new MenuItem { Text = "Delete", IsDestructive = true }; // red background
deleteAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
deleteAction.Clicked += async (sender, e) => {
    var mi = ((MenuItem)sender);
    Debug.WriteLine("Delete Context Action clicked: " + mi.CommandParameter);
};
// add to the ViewCell's ContextActions property
ContextActions.Add (moreAction);
ContextActions.Add (deleteAction);
```

### <a name="xaml"></a>XAML

`MenuItem`s могут также создаваться в коллекции XAML декларативно. Ниже XAML демонстрирует настраиваемой ячейки с реализованы два контекстных действий:

```xaml
<ListView x:Name="ContextDemoList">
  <ListView.ItemTemplate>
    <DataTemplate>
      <ViewCell>
         <ViewCell.ContextActions>
            <MenuItem Clicked="OnMore" CommandParameter="{Binding .}"
               Text="More" />
            <MenuItem Clicked="OnDelete" CommandParameter="{Binding .}"
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

Убедитесь в файл с выделенным кодом `Clicked` реализуются методы:

```csharp
public void OnMore (object sender, EventArgs e) {
    var mi = ((MenuItem)sender);
    DisplayAlert("More Context Action", mi.CommandParameter + " more context action", "OK");
}

public void OnDelete (object sender, EventArgs e) {
    var mi = ((MenuItem)sender);
    DisplayAlert("Delete Context Action", mi.CommandParameter + " delete context action", "OK");
}
```

> [!NOTE]
> `NavigationPageRenderer` Для Android имеет переопределяемый `UpdateMenuItemIcon` метод, который может использоваться для загрузки значки из настраиваемого `Drawable`. Это переопределение позволяет использовать изображения SVG в виде значков на `MenuItem` экземпляров на Android.

<a name="Pull_to_Refresh" />

## <a name="pull-to-refresh"></a>Потяните, чтобы обновить

Пользователи привыкли, потянув вниз по списку данных обновит этого списка. [`ListView`](xref:Xamarin.Forms.ListView)поддерживает встроенную поддержку. Чтобы включить функцию "принудительное обновление", задайте [`IsPullToRefreshEnabled`](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) для `true`параметра значение:

```xaml
<ListView ...
          IsPullToRefreshEnabled="true" />
```

Эквивалентный код на C# выглядит так:

```csharp
listView.IsPullToRefreshEnabled = true;
```

Счетчик отображается во время обновления, который по умолчанию является черным. Однако цвет счетчика можно изменить в iOS и Android, задав `RefreshControlColor` [`Color`](xref:Xamarin.Forms.Color)для свойства значение.

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

![](interactivity-images/refresh-start.png "ListView потяните, чтобы обновить выполняющееся")

На следующих снимках экрана показан запрос на обновление после того, как пользователь освободил запрос на вытягивание, и при [`ListView`](xref:Xamarin.Forms.ListView) обновлении отображается счетчик:

![](interactivity-images/refresh-in-progress.png "Извлечение ListView для обновления завершено")

[`ListView`](xref:Xamarin.Forms.ListView)запускает событие для запуска обновления, [`IsRefreshing`](xref:Xamarin.Forms.ListView.IsRefreshing) и свойство будет установлено в `true`значение. [`Refreshing`](xref:Xamarin.Forms.ListView.Refreshing) Любой код, необходимый для обновления содержимого, `ListView` должен затем выполняться обработчиком событий `Refreshing` для события или методом, выполняемым [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand). После обновления `IsRefreshing` [свойстводолжно`EndRefresh`](xref:Xamarin.Forms.ListView.EndRefresh) иметь значение ,илиметоддолженбытьвызван,чтобыуказать,чтообновлениезавершено.`false` `ListView`

> [!NOTE]
> При определении [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand) `CanExecute` метода можно указать метод команды, чтобы включить или отключить команду.

## <a name="related-links"></a>Связанные ссылки

- [ListView интерактивности (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)
