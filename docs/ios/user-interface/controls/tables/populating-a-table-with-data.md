---
title: Заполнение таблицы данными в Xamarin. iOS
description: В этом документе описывается заполнение таблицы данными в приложении Xamarin. iOS. В нем обсуждается Уитаблевиевсаурце, повторное использование ячеек, добавление индекса, а также верхние и нижние колонтитулы.
ms.prod: xamarin
ms.assetid: 6FE64DDF-1029-EB9B-6EEC-1C7DFDFDF3AF
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: d926e206e4bba04629163cfc3d63b51a5f2f840d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021872"
---
# <a name="populating-a-table-with-data-in-xamarinios"></a>Заполнение таблицы данными в Xamarin. iOS

Чтобы добавить строки в `UITableView` необходимо реализовать `UITableViewSource` подкласс и переопределить методы, которые вызывает табличное представление для заполнения самой себя.

В этом руководстве рассматриваются следующие вопросы:

- Подкласс Уитаблевиевсаурце
- Повторное использование ячейки
- Добавление индекса
- Добавление верхних и нижних колонтитулов

<a name="Subclassing_UITableViewSource" />

## <a name="subclassing-uitableviewsource"></a>Подклассировать Уитаблевиевсаурце

`UITableViewSource` подкласс назначается каждому `UITableView`. Табличное представление запрашивает исходный класс, чтобы определить способ отрисовки (например, количество строк и высота каждой строки, если она отличается от значения по умолчанию). Что важнее всего, источник предоставляет все представления ячеек, заполненные данными.

Для представления данных в таблице требуется только два обязательных метода:

- **Ровсинсектион** — возвращает количество [`nint`](~/cross-platform/macios/nativetypes.md) общего числа строк данных, которые должна отображать таблица.
- @ **Cell** — возврат `UITableCellView` с заполнением данными для соответствующего индекса строки, переданного в метод.

**TableSource.CS** пример файла басиктабле имеет простейшую возможную реализацию `UITableViewSource`. В следующем фрагменте кода можно увидеть, что он принимает массив строк, отображаемых в таблице, и возвращает стиль ячейки по умолчанию, содержащий каждую строку:

```csharp
public class TableSource : UITableViewSource {

        string[] TableItems;
        string CellIdentifier = "TableCell";

        public TableSource (string[] items)
        {
            TableItems = items;
        }

        public override nint RowsInSection (UITableView tableview, nint section)
        {
            return TableItems.Length;
        }

        public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
        {
            UITableViewCell cell = tableView.DequeueReusableCell (CellIdentifier);
            string item = TableItems[indexPath.Row];

            //---- if there are no cells to reuse, create a new one
            if (cell == null)
            { cell = new UITableViewCell (UITableViewCellStyle.Default, CellIdentifier); }

            cell.TextLabel.Text = item;

            return cell;
        }
}
```

`UITableViewSource` может использовать любую структуру данных из простого массива строк (как показано в этом примере) в список < > или другую коллекцию. Реализация `UITableViewSource` методов изолирует таблицу от базовой структуры данных.

Чтобы использовать этот подкласс, создайте массив строк для создания источника, а затем назначьте его экземпляру `UITableView`:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    table = new UITableView(View.Bounds); // defaults to Plain style
    string[] tableItems = new string[] {"Vegetables","Fruits","Flower Buds","Legumes","Bulbs","Tubers"};
    table.Source = new TableSource(tableItems);
    Add (table);
}
```

Результирующая таблица выглядит следующим образом:

 [![](populating-a-table-with-data-images/image3.png "Sample table running")](populating-a-table-with-data-images/image3.png#lightbox)

В большинстве таблиц пользователь может коснуться строки, чтобы выбрать ее, и выполнить какое-либо другое действие (например, воспроизведение песни или вызов контакта или отображение другого экрана). Чтобы добиться этого, необходимо выполнить несколько задач. Во-первых, создадим Алертконтроллер для вывода сообщения, когда пользователь щелкнул строку, добавив следующий элемент в метод `RowSelected`:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("Row Selected", tableItems[indexPath.Row], UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    ...

    tableView.DeselectRow (indexPath, true);
}
```

Затем создайте экземпляр нашего контроллера представления:

```csharp
HomeScreen owner;
```

Добавьте конструктор в класс Уитаблевиевсаурце, который принимает контроллер представления в качестве параметра и сохраняет его в поле:

```csharp
public TableSource (string[] items, HomeScreen owner)
{
    ...
    this.owner = owner;

}
```

Измените метод ViewDidLoad, в котором создается класс Уитаблевиевсаурце, чтобы передать ссылку на `this`:

```csharp
table.Source = new TableSource(tableItems, this);
```

Наконец, вернитесь к методу `RowSelected`, вызовите `PresentViewController` в кэшированном поле:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    ...
    owner.PresentViewController (okAlertController, true, null);

    ...
}
```

Теперь пользователь может коснуться строки, и появится предупреждение:

 [![](populating-a-table-with-data-images/image4.png "The row selected alert")](populating-a-table-with-data-images/image4.png#lightbox)

## <a name="cell-reuse"></a>Повторное использование ячейки

В этом примере имеется только шесть элементов, поэтому повторное использование ячеек не требуется. Однако при отображении сотен или тысяч строк это приведет к недостаточной памяти для создания сотен или тысяч `UITableViewCell` объектов, когда на экране помещается всего несколько элементов.

Чтобы избежать такой ситуации, когда ячейка исчезает с экрана, ее представление помещается в очередь для повторного использования. Когда пользователь прокручивается, таблица вызывает `GetCell` для запроса новых представлений для отображения — для повторного использования существующей ячейки (которая в данный момент не отображается) просто вызовите метод `DequeueReusableCell`. Если ячейка доступна для повторного использования, она будет возвращена, в противном случае возвращается значение null, и код должен создать новый экземпляр ячейки.

В этом фрагменте кода из примера демонстрируется шаблон:

```csharp
// request a recycled cell to save memory
UITableViewCell cell = tableView.DequeueReusableCell (cellIdentifier);
// if there are no cells to reuse, create a new one
if (cell == null)
    cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
```

`cellIdentifier` эффективно создает отдельные очереди для различных типов ячеек. В этом примере все ячейки выглядят одинаково, поэтому используется только один жестко закодированный идентификатор. Если существуют различные типы ячеек, каждая из которых должна иметь другую строку идентификатора, как при создании экземпляра, так и при их запросе из очереди повторного использования.

### <a name="cell-reuse-in-ios-6"></a>Повторное использование ячеек в iOS 6 +

в iOS 6 добавлен шаблон повторного использования ячеек, аналогичный приведенному в статье Введение в представления коллекций. Несмотря на то, что существующий шаблон повторного использования, показанный выше, по-прежнему поддерживается для обратной совместимости, этот новый шаблон предпочтительнее, так как он устраняет необходимость проверки значения NULL для ячейки.

С новым шаблоном приложение регистрирует класс ячейки или XIB для использования путем вызова либо `RegisterClassForCellReuse`, либо `RegisterNibForCellReuse` в конструкторе контроллера. Затем, вызвав эту ячейку в методе `GetCell`, просто вызовите `DequeueReusableCell` передав идентификатор, зарегистрированный для класса Cell или XIB, и путь индекса.

Например, следующий код регистрирует пользовательский класс ячейки в Уитаблевиевконтроллер:

```csharp
public class MyTableViewController : UITableViewController
{
  static NSString MyCellId = new NSString ("MyCellId");

  public MyTableViewController ()
  {
    TableView.RegisterClassForCellReuse (typeof(MyCell), MyCellId);
  }
  ...
}
```

После регистрации класса мицелл ячейку можно вывести из очереди в методе `GetCell` `UITableViewSource` без дополнительной проверки значения NULL, как показано ниже:

```csharp
class MyTableSource : UITableViewSource
{
  public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
  {
    // if cell is not available in reuse pool, iOS will create one automatically
    // no need to do null check and create cell manually
    var cell = (MyCell) tableView.DequeueReusableCell (MyCellId, indexPath);

    // do whatever you need to with cell, such as assigning properties, etc.

    return cell;
  }
}
```

Имейте в виду, что при использовании нового шаблона повторного использования с пользовательским классом ячейки необходимо реализовать конструктор, который принимает `IntPtr`, как показано в приведенном ниже фрагменте кода. в противном случае цель-C не сможет создать экземпляр класса Cell:

```csharp
public class MyCell : UITableViewCell
{
  public MyCell (IntPtr p):base(p)
  {
  }
  ...
}
```

Вы можете увидеть примеры разделов, описанных выше, в образце **басиктабле** , связанном с этой статьей.

<a name="Adding_an_Index" />

## <a name="adding-an-index"></a>Добавление индекса

Индекс помогает пользователю прокручивать длинные списки, как правило, упорядоченные в алфавитном порядке, хотя вы можете индексировать по любому желаемому критерию. Пример **басиктаблеиндекс** загружает из файла более длинный список элементов для демонстрации индекса. Каждый элемент в индексе соответствует "разделу" таблицы.

 [![](populating-a-table-with-data-images/image5.png "The Index display")](populating-a-table-with-data-images/image5.png#lightbox)

Для поддержки "разделов" данные, лежащие в основе таблицы, должны быть сгруппированы, поэтому образец Басиктаблеиндекс создает `Dictionary<>` из массива строк, используя первую букву каждого элемента в качестве ключа словаря:

```csharp
indexedTableItems = new Dictionary<string, List<string>>();
foreach (var t in items) {
    if (indexedTableItems.ContainsKey (t[0].ToString ())) {
        indexedTableItems[t[0].ToString ()].Add(t);
    } else {
        indexedTableItems.Add (t[0].ToString (), new List<string>() {t});
    }
}
keys = indexedTableItems.Keys.ToArray ();
```

Для использования `Dictionary<>` необходимо добавить или изменить следующие методы `UITableViewSource` подклассу.

- **Нумберофсектионс** — этот метод является необязательным. по умолчанию таблица принимает один раздел. При отображении индекса этот метод должен возвращать количество элементов в индексе (например, 26, если индекс содержит все буквы английского алфавита).
- **Ровсинсектион** — возвращает количество строк в заданном разделе.
- **Сектиониндекститлес** — возвращает массив строк, которые будут использоваться для вывода индекса. Пример кода возвращает массив букв.

Обновленные методы в примере файла **басиктаблеиндекс/таблесаурце. CS** выглядят следующим образом:

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return keys.Length;
}
public override nint RowsInSection (UITableView tableview, nint section)
{
    return indexedTableItems[keys[section]].Count;
}
public override string[] SectionIndexTitles (UITableView tableView)
{
    return keys;
}
```

Обычно индексы используются только с обычным стилем таблицы.

<a name="Adding_Headers_and_Footers" />

## <a name="adding-headers-and-footers"></a>Добавление верхних и нижних колонтитулов

Верхние и нижние колонтитулы можно использовать для визуального группирования строк в таблице. Необходимая структура данных очень похожа на добавление индекса — `Dictionary<>` работает очень хорошо. Вместо использования алфавита для группировки ячеек в этом примере будет группироваться тип овощей by ботаническом.
Вывод выглядит следующим образом.

 [![](populating-a-table-with-data-images/image6.png "Sample Headers and Footers")](populating-a-table-with-data-images/image6.png#lightbox)

Для вывода верхних и нижних колонтитулов `UITableViewSource` подклассу требуются следующие дополнительные методы:

- **Титлефорхеадер** — возвращает текст, используемый в качестве заголовка.
- **Титлефорфутер** — возвращает текст, используемый в качестве нижнего колонтитула.

Обновленные методы в примере файла **басиктаблехеадерфутер/Code/таблесаурце. CS** выглядят следующим образом:

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    return keys[section];
}
public override string TitleForFooter (UITableView tableView, nint section)
{
    return indexedTableItems[keys[section]].Count + " items";
}
```

Можно дополнительно настроить внешний вид верхнего и нижнего колонтитула с помощью объекта представления, используя переопределения методов `GetViewForHeader` и `GetViewForFooter` для `UITableViewSource`.

## <a name="related-links"></a>Связанные ссылки

- [Воркингвистаблес (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithtables)
