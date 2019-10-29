---
title: Общие сведения о главе 19. Представления коллекций
description: 'Создание мобильных приложений с помощью Xamarin. Forms: сводка по главе 19. Представления коллекций'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 0AEC3A5C-586E-4D0F-9895-67E99A053A79
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: bffbd2dec4a8494723597ba6e0f0af69e57f3718
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032857"
---
# <a name="summary-of-chapter-19-collection-views"></a>Общие сведения о главе 19. Представления коллекций

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)

> [!NOTE] 
> Примечания на этой странице указывают области, в которых Xamarin. Forms рассходится от материала, представленного в книге.

Xamarin. Forms определяет три представления, которые поддерживают коллекции и отображают их элементы:

- [`Picker`](xref:Xamarin.Forms.Picker) является относительно коротким списком элементов строк, позволяющих пользователю выбрать один из них
- [`ListView`](xref:Xamarin.Forms.ListView) часто является длинным списком элементов, которые обычно имеют одинаковый тип и форматирование, а также позволяют пользователю выбрать один из них.
- [`TableView`](xref:Xamarin.Forms.TableView) — это коллекция *ячеек* (обычно различных типов и представлений) для отображения данных или управления вводом пользователя.

Обычно приложения MVVM используют `ListView` для показа выбираемой коллекции объектов.

## <a name="program-options-with-picker"></a>Параметры программы с помощью средства выбора

[`Picker`](xref:Xamarin.Forms.Picker) является хорошим выбором, когда необходимо разрешить пользователю выбирать вариант из относительно короткого списка `string` элементов.

### <a name="the-picker-and-event-handling"></a>Средство выбора и обработка событий

В примере [**пиккердемо**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerDemo) показано, как использовать XAML для задания свойства `Picker` [`Title`](xref:Xamarin.Forms.Picker.Title) и добавления элементов `string` в коллекцию [`Items`](xref:Xamarin.Forms.Picker.Items) . Когда пользователь выбирает `Picker`, он отображает элементы в коллекции `Items` в зависимости от платформы.

Событие [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) указывает, когда пользователь выбрал элемент. Свойство [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) , начинающееся с нуля, затем указывает выбранный элемент. Если элемент не выбран, `SelectedIndex` равно &ndash;1.

Можно также использовать `SelectedIndex` для инициализации выбранного элемента, но он должен быть задан после заполнения коллекции `Items`. В XAML это означает, что вы, вероятно, используете элемент свойства для установки `SelectedIndex`.

### <a name="data-binding-the-picker"></a>Привязка данных средство выбора

Свойство `SelectedIndex` поддерживается связываемым свойством, но `Items` не является, поэтому использование привязки данных с `Picker` сложно. Одним из решений является использование `Picker` в сочетании с [`ObjectToIndexConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ObjectToIndexConverter.cs) , например, в библиотеке [**Xamarin. формсбук. Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) . [**Пиккербиндинг**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerBinding) демонстрирует, как это работает.

> [!NOTE] 
> `Picker` Xamarin. Forms теперь включает свойства `ItemsSource` и `SelectedItem`, поддерживающие привязку данных. См. раздел [средство выбора](~/xamarin-forms/user-interface/picker/index.md).

## <a name="rendering-data-with-listview"></a>Подготовка данных к просмотру с помощью ListView

[`ListView`](xref:Xamarin.Forms.ListView) является единственным классом, производным от [`ItemsView<TVisual>`](xref:Xamarin.Forms.ItemsView`1) , от которого он наследует свойства [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) и [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) .

`ItemsSource` имеет тип `IEnumerable` но он `null` по умолчанию и должен быть явно инициализирован или (более часто) задан коллекцией через привязку данных. Элементы в этой коллекции могут быть любого типа.

`ListView` определяет свойство [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) , которое либо устанавливается в один из элементов в `ItemsSource` коллекции, либо `null`, если ни один элемент не выбран. `ListView` запускает событие [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) при выборе нового элемента.

### <a name="collections-and-selections"></a>Коллекции и выборки

Образец [**листвиевлист**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewList) заполняет `ListView` с 17 `Color` значениями в `List<Color>`ной коллекции. Элементы можно выбирать, но по умолчанию они отображаются с непривлекательными `ToString` представлениями. Несколько примеров в этой главе показывают, как исправить это отображение и сделать его привлекательным по желанию.

### <a name="the-row-separator"></a>Разделитель строк

На экранах iOS и Android тонкая линия разделяет строки. Это можно контролировать с помощью свойств [`SeparatorVisibility`](xref:Xamarin.Forms.ListView.SeparatorVisibility) и [`SeparatorColor`](xref:Xamarin.Forms.ListView.SeparatorColor) . `SeparatorVisibility` свойство имеет тип [`SeparatorVisibility`](xref:Xamarin.Forms.SeparatorVisibility), перечисление с двумя членами:

- [`Default`](xref:Xamarin.Forms.SeparatorVisibility.Default), параметр по умолчанию
- [`None`](xref:Xamarin.Forms.SeparatorVisibility.None)

### <a name="data-binding-the-selected-item"></a>Привязка данных к выбранному элементу

Свойство `SelectedItem` поддерживается связываемым свойством, поэтому оно может быть источником или назначением привязки данных. Его `BindingMode` по умолчанию `OneWayToSource`, но обычно он является целевым объектом двухсторонней привязки данных, особенно в сценариях MVVM. В образце [**листвиеваррай**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewArray) показан этот тип привязки.

### <a name="the-observablecollection-difference"></a>Разница между ObservableCollection

Пример [**листвиевлогжер**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewLogger) устанавливает свойство `ItemsSource` `ListView` в коллекцию `List<DateTime>`, а затем постепенно добавляет новый объект `DateTime` в коллекцию каждую секунду с помощью таймера.

Однако `ListView` не обновляется автоматически, так как в коллекции `List<T>` нет механизма уведомления, указывающего, когда элементы добавляются в коллекцию или удаляются из нее.

Более эффективный класс, используемый в таких сценариях, [`ObservableCollection<T>`](xref:System.Collections.ObjectModel.ObservableCollection`1) определен в пространстве имен `System.Collections.ObjectModel`. Этот класс реализует интерфейс [`INotifyCollectionChanged`](xref:System.Collections.Specialized.INotifyCollectionChanged) и, следовательно, запускает событие [`CollectionChanged`](xref:System.Collections.ObjectModel.ObservableCollection`1.CollectionChanged) при добавлении или удалении элементов из коллекции или при их замене или перемещении в коллекции. Когда `ListView` внутренне обнаруживает, что класс, реализующий `INotifyCollectionChanged`, был установлен в его свойство `ItemsSource`, он присоединяет обработчик к событию `CollectionChanged` и обновляет его отображение при изменении коллекции.

В примере [**обсерваблелогжер**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ObservableLogger) демонстрируется использование `ObservableCollection`.

### <a name="templates-and-cells"></a>Шаблоны и ячейки

По умолчанию `ListView` отображает элементы в коллекции с помощью метода `ToString` каждого элемента. Лучшим подходом является определение шаблона для вывода элементов.

Чтобы поэкспериментировать с этой функцией, можно использовать класс [`NamedColor`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) в библиотеке [**Xamarin. формсбук. Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) . Этот класс определяет статическое свойство `All` типа `IList<NamedColor>`, содержащее 141 `NamedColor` объектов, соответствующих открытым полям структуры `Color`.

Образец [**наивенамедколорлист**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/NaiveNamedColorList) задает `ItemsSource` `ListView` для этого свойства `NamedColor.All`, но отображаются только полные имена классов `NamedColor` объектов.

`ListView` требуется шаблон для вывода этих элементов. В коде можно задать свойство [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) , определенное `ItemsView<TVisual>`, в объект [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) с помощью [конструктора`DataTemplate`](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type)) , который ссылается на производный класс [`Cell`](xref:Xamarin.Forms.Cell) . `Cell` имеет пять производных:

- [`TextCell`](xref:Xamarin.Forms.TextCell) &mdash; содержит два представления `Label` (концептуально говоря)
- [`ImageCell`](xref:Xamarin.Forms.ImageCell) &mdash; добавляет представление `Image` в `TextCell`
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) &mdash; содержит представление `Entry` с `Label`
- [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) &mdash; содержит `Switch` с `Label`
- [`ViewCell`](xref:Xamarin.Forms.ViewCell) &mdash; может быть любым `View` (возможно, с дочерними элементами)

Затем вызовите [`SetValue`](xref:Xamarin.Forms.DataTemplate.SetValue(Xamarin.Forms.BindableProperty,System.Object)) и [`SetBinding`](xref:Xamarin.Forms.DataTemplate.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) в объекте `DataTemplate`, чтобы связать значения со свойствами `Cell` или задать привязки данных в свойствах `Cell`, ссылающихся на свойства элементов в коллекции `ItemsSource`. Это продемонстрировано в примере [**текстцелллисткоде**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListCode) .

Поскольку каждый элемент отображается `ListView`, небольшое визуальное дерево строится на основе шаблона, а привязки данных устанавливаются между элементом и свойствами элементов в этом визуальном дереве. Вы можете получить представление об этом процессе, установив обработчики для [`ItemAppearing`](xref:Xamarin.Forms.ListView.ItemAppearing) и [`ItemDisappearing`](xref:Xamarin.Forms.ListView.ItemDisappearing) событий `ListView`или с помощью альтернативного [конструктора`DataTemplate`](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Func{System.Object})) , использующего функцию, которая вызывается каждый раз, когда визуальное дерево элемента должно быть созданы.

[**Текстцелллистксамл**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListXaml) показывает полностью идентичную программу в XAML. Тег `DataTemplate` устанавливается в свойство `ItemTemplate` `ListView`, а затем `TextCell` устанавливается в `DataTemplate`. Привязки к свойствам элементов в коллекции задаются непосредственно в свойствах [`Text`](xref:Xamarin.Forms.TextCell.Text) и [`Detail`](xref:Xamarin.Forms.TextCell.Detail) `TextCell`.

### <a name="custom-cells"></a>Пользовательские ячейки

В XAML можно задать [`ViewCell`](xref:Xamarin.Forms.ViewCell) `DataTemplate` а затем определить пользовательское визуальное дерево как свойство [`View`](xref:Xamarin.Forms.ViewCell.View) `ViewCell`. (`View` является свойством содержимого `ViewCell` поэтому теги `ViewCell.View` не требуются.) Этот метод демонстрируется в примере [**кустомнамедколорлист**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CustomNamedColorList) :

[![Тройной снимок экрана с пользовательским списком цветов](images/ch19fg11-small.png "Список настраиваемых именованных цветов")](images/ch19fg11-large.png#lightbox "Список настраиваемых именованных цветов")

Получить право на изменение размера для всех платформ может быть непросто. Свойство [`RowHeight`](xref:Xamarin.Forms.ListView.RowHeight) является полезным, но в некоторых случаях необходимо прибегнуть к свойству [`HasUnevenRows`](xref:Xamarin.Forms.ListView.HasUnevenRows) , которое менее эффективно, но заставляет `ListView` изменять размер строк. Для iOS и Android необходимо использовать одно из этих двух свойств для получения соответствующего размера строки.

### <a name="grouping-the-listview-items"></a>Группирование элементов ListView

`ListView` поддерживает группирование элементов и навигацию между этими группами. Свойству `ItemsSource` должно быть присвоено значение коллекции коллекций: для объекта, `ItemsSource` для которого задано значение, необходимо реализовать `IEnumerable`, а каждый элемент в коллекции также должен реализовывать `IEnumerable`. Каждая группа должна включать два свойства: текстовое описание группы и трехзначное обозначение.

Класс [`NamedColorGroup`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColorGroup.cs) в библиотеке [**Xamarin. формсбук. Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) создает семь групп `NamedColor` объектов. В образце [**колорграуплист**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorGroupList) показано, как использовать эти группы со свойством [`IsGroupingEnabled`](xref:Xamarin.Forms.ListView.IsGroupingEnabled) `ListView`, для которого задано значение `true`, а также свойства [`GroupDisplayBinding`](xref:Xamarin.Forms.ListView.GroupDisplayBinding) и [`GroupShortNameBinding`](xref:Xamarin.Forms.ListView.GroupShortNameBinding) , привязанные к свойствам в каждой группе.

### <a name="custom-group-headers"></a>Заголовки пользовательской группы

Можно создать пользовательские заголовки для групп `ListView`, заменив свойство `GroupDisplayBinding` [`GroupHeaderTemplate`](xref:Xamarin.Forms.ListView.GroupHeaderTemplate) определяя шаблон для заголовков.

### <a name="listview-and-interactivity"></a>ListView и интерактивность

Как правило, приложение получает взаимодействие пользователя с `ListView`, подключив обработчик к событию `ItemSelected` или [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) или установив привязку данных в свойстве `SelectedItem`. Но некоторые типы ячеек (`EntryCell` и `SwitchCell`) позволяют взаимодействовать с пользователем, а также создавать пользовательские ячейки, которые сами могут взаимодействовать с пользователем. [**Интерактивелиствиев**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/InteractiveListView) создает 100 экземпляров [`ColorViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) и позволяет пользователю изменять каждый цвет с помощью три элементов `Slider`. Кроме того, программа использует [`ColorToContrastColorConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorToContrastColorConverter.cs) в [**Xamarin. формсбук. Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit).

## <a name="listview-and-mvvm"></a>ListView и MVVM

`ListView` играет значительную роль в сценариях MVVM. Каждый раз, когда в ViewModel существует коллекция `IEnumerable`, она часто привязывается к `ListView`. Кроме того, элементы в коллекции часто реализуют `INotifyPropertyChanged` для привязки со свойствами в шаблоне.

### <a name="a-collection-of-viewmodels"></a>Коллекция ViewModels

Для этого библиотека [**счулоффинеартс**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) создает несколько классов на основе [файла данных XML](https://xamarin.github.io/xamarin-forms-book-samples/SchoolOfFineArt/students.xml) и изображений вымышленных учащихся в этом вымышленном учебном заведении.

Класс [`Student`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/Student.cs) является производным от [`ViewModelBase`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/ViewModelBase.cs). Класс [`StudentBody`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/StudentBody.cs) — это коллекция объектов `Student`, которые также являются производными от `ViewModelBase`. [`SchoolViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/SchoolViewModel.cs) скачивает XML-файл и собирает все объекты.

Программа [**студентлист**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/StudentList) использует `ImageCell` для показа учащихся и их изображений в `ListView`:

[![Тройной снимок экрана списка учащихся](images/ch19fg18-small.png "Список учащихся")](images/ch19fg18-large.png#lightbox "Список учащихся")

В примере [**листвиевхеадер**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewHeader) добавляется свойство [`Header`](xref:Xamarin.Forms.ListView.Header) , но оно отображается только в Android.

### <a name="selection-and-the-binding-context"></a>Выбор и контекст привязки

Программа [**селектедстудентдетаил**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/SelectedStudentDetail) привязывает `BindingContext` `StackLayout` к свойству `SelectedItem` `ListView`. Это позволяет программе отображать подробные сведения о выбранном учащемся.

### <a name="context-menus"></a>Контекстные меню

В ячейке может быть определено контекстное меню, реализованное в соответствии с конкретной платформой. Чтобы создать это меню, добавьте [`MenuItem`](xref:Xamarin.Forms.MenuItem) объекты в свойство [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) `Cell`.

`MenuItem` определяет пять свойств:

- [`Text`](xref:Xamarin.Forms.MenuItem.Text) типа `string`
- [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) типа `FileImageSource`
- [`IsDestructive`](xref:Xamarin.Forms.MenuItem.IsDestructive) типа `bool`
- [`Command`](xref:Xamarin.Forms.MenuItem.Command) типа `ICommand`
- [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter) типа `object`

Свойства `Command` и `CommandParameter` подразумевают, что ViewModel для каждого элемента содержит методы для выполнения нужных команд меню. В сценариях, не использующих MVVM, `MenuItem` также определяет [`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked) событие.

[**Целлконтекстмену**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CellContextMenu) демонстрирует этот прием. Свойство `Command` каждого `MenuItem` привязано к свойству типа `ICommand` в классе `Student`. Задайте для свойства `IsDestructive` значение `true` для `MenuItem`, который удаляет или удаляет выбранный объект.

### <a name="varying-the-visuals"></a>Изменение визуальных элементов

Иногда требуется, чтобы небольшие варианты в визуальных элементах элементов в `ListView` на основе свойства. Например, если среднее значение точки на уровне учащегося ниже 2,0, в примере [**колоркодедстудентс**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorCodedStudents) отображается имя учащегося красным цветом.
Это достигается с помощью преобразователя значений привязки, [`ThresholdToObjectConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ThresholdToObjectConverter.cs)в библиотеке [**Xamarin. формсбук. Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) .

### <a name="refreshing-the-content"></a>Обновление содержимого

`ListView` поддерживает жест вытягивания для обновления данных. Чтобы включить это, программа должна задать для свойства [`IsPullToRefresh`](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) значение `true`. `ListView` реагирует на жест растягивания, устанавливая для свойства [`IsRefreshing`](xref:Xamarin.Forms.ListView.IsRefreshing) значение `true`, а также вызывая событие [`Refreshing`](xref:Xamarin.Forms.ListView.Refreshing) и (для сценариев MVVM) вызов метода `Execute` его свойства [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand) .

Код, обрабатывающий событие `Refresh` или `RefreshCommand`, возможно, обновляет данные, отображаемые `ListView`, и устанавливает `IsRefreshing` обратно в `false`.

В примере [**RSS-канал**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/RssFeed) демонстрируется использование [`RssFeedViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/RssFeed/RssFeed/RssFeed/RssFeedViewModel.cs) , реализующей свойства `RefreshCommand` и `IsRefreshing` для привязки данных.

## <a name="the-tableview-and-its-intents"></a>Таблевиев и его намерения

Хотя в `ListView` обычно отображается несколько экземпляров одного типа, [`TableView`](xref:Xamarin.Forms.TableView) обычно нацелен на предоставление пользовательского интерфейса для нескольких свойств различных типов. Каждый элемент связан с собственным [`Cell`](xref:Xamarin.Forms.Cell) , производным для отображения свойства или предоставления ему пользовательского интерфейса.

### <a name="properties-and-hierarchies"></a>Свойства и иерархии

`TableView` определяет только четыре свойства:

- [`Intent`](xref:Xamarin.Forms.TableView.Intent) типа [`TableIntent`](xref:Xamarin.Forms.TableIntent), перечисление
- [`Root`](xref:Xamarin.Forms.TableView.Root) типа [`TableRoot`](xref:Xamarin.Forms.TableRoot), свойство Content объекта `TableView`
- [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight) типа `int`
- [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows) типа `bool`

Перечисление `TableIntent` указывает, как предполагается использовать `TableView`:

- [`Data`](xref:Xamarin.Forms.TableIntent.Data)
- [`Form`](xref:Xamarin.Forms.TableIntent.Form)
- [`Settings`](xref:Xamarin.Forms.TableIntent.Settings)
- [`Menu`](xref:Xamarin.Forms.TableIntent.Menu)

Эти члены также предлагают некоторые варианты использования `TableView`.

При определении таблицы участвуют некоторые другие классы:

- [`TableSectionBase`](xref:Xamarin.Forms.TableSectionBase) является абстрактным классом, производным от `BindableObject` и определяющим свойство [`Title`](xref:Xamarin.Forms.TableSectionBase.Title)

- [`TableSectionBase<T>`](xref:Xamarin.Forms.TableSectionBase`1) — это абстрактный класс, производный от `TableSectionBase` и реализующий `IList<T>` и `INotifyCollectionChanged`

- [`TableSection`](xref:Xamarin.Forms.TableSection) является производным от `TableSectionBase<Cell>`

- [`TableRoot`](xref:Xamarin.Forms.TableRoot) является производным от `TableSectionBase<TableSection>`

Вкратце, `TableView` имеет свойство `Root`, для которого задан объект `TableRoot`, который представляет собой коллекцию объектов `TableSection`, каждый из которых является коллекцией объектов `Cell`. Таблица содержит несколько разделов, и каждый раздел содержит несколько ячеек. Сама таблица может иметь заголовок, а каждый раздел может иметь заголовок. Хотя `TableView` использует производные от `Cell`, он не использует `DataTemplate`.

### <a name="a-prosaic-form"></a>Форма просаик

Образец [**ентриформ**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/EntryForm) определяет модель представления [`PersonalInformation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) , экземпляр которой преобразуется в `BindingContext` `TableView`. Каждый `Cell`, производный в его `TableSection`, может иметь привязки к свойствам класса `PersonalInformation`.

### <a name="custom-cells"></a>Пользовательские ячейки

Пример [**кондитионалцеллс**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalCells) разворачивается на **ентриформ**. Класс [`ProgrammerInformation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) включает логическое свойство, которое регулирует применимость двух дополнительных свойств. Для этих двух дополнительных свойств программа использует пользовательский `PickerCell` на основе [пиккерцелл. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml) и [PickerCell.XAML.CS](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml.cs) в библиотеке [**Xamarin. формсбук. Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) .

Несмотря на то, что `IsEnabled` свойства двух `PickerCell` элементов привязаны к логическому свойству в `ProgrammerInformation`, этот прием не работает, что запрашивает следующую выборку.

### <a name="conditional-sections"></a>Условные разделы

Пример [**кондитионалсектион**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalSection) помещает два элемента, условные, в зависимости от выбранного логического элемента, в отдельном `TableSection`. Файл кода программной части удаляет этот раздел из `TableView` или добавляет его обратно на основе логического свойства.

### <a name="a-tableview-menu"></a>Меню Таблевиев

Другим применением `TableView` является меню. В образце [**команды MenuCommand**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/MenuCommands) показано меню, которое позволяет перемещать небольшие `BoxView` вокруг экрана.

## <a name="related-links"></a>Связанные ссылки

- [Глава 19. полный текст (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch19-Apr2016.pdf)
- [Глава 19. примеры](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)
- [Средство выбора](~/xamarin-forms/user-interface/picker/index.md)
- [ListView](~/xamarin-forms/user-interface/listview/index.md)
- [TableView](~/xamarin-forms/user-interface/tableview.md)
