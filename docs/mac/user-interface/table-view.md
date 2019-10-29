---
title: Табличные представления в Xamarin. Mac
description: В этой статье рассматривается работа с табличными представлениями в приложении Xamarin. Mac. Он описывает создание табличных представлений в Xcode и Interface Builder и взаимодействие с ними в коде.
ms.prod: xamarin
ms.assetid: 3B55B858-4769-4331-966A-7F53B3B7C720
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 36bed05c1e60004125406c3ed2df66fcfe2be10b
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73008325"
---
# <a name="table-views-in-xamarinmac"></a>Табличные представления в Xamarin. Mac

_В этой статье рассматривается работа с табличными представлениями в приложении Xamarin. Mac. Он описывает создание табличных представлений в Xcode и Interface Builder и взаимодействие с ними в коде._

При работе с C# и .NET в приложении Xamarin. Mac у вас есть доступ к тем же представлениям таблиц, что и разработчик, работающий на *уровне цели-C* и *Xcode* . Так как Xamarin. Mac интегрируется напрямую с Xcode, можно использовать _Interface Builder_ Xcode для создания и обслуживания табличных представлений (или при необходимости создавать их непосредственно в C# коде).

Табличное представление отображает данные в табличном формате, содержащем один или несколько столбцов данных в нескольких строках. В зависимости от типа создаваемого представления таблицы пользователь может сортировать по столбцам, реорганизовывать столбцы, добавлять столбцы, удалять столбцы или изменять данные, содержащиеся в таблице.

[![](table-view-images/intro01.png "An example table")](table-view-images/intro01.png#lightbox)

В этой статье рассматриваются основные принципы работы с табличными представлениями в приложении Xamarin. Mac. Мы настоятельно рекомендуем сначала ознакомиться со статьей [Hello, Mac](~/mac/get-started/hello-mac.md) , в частности [Знакомство с Xcode и Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) , а также с разделом "возможности [и действия](~/mac/get-started/hello-mac.md#outlets-and-actions) ", так как в нем рассматриваются основные понятия и методы, которые мы будем использовать в Эта статья.

Возможно, вы захотите ознакомиться с разделом [ C# классы и методы для цели-C](~/mac/internals/how-it-works.md) в документе о внутренних компонентах [Xamarin. Mac](~/mac/internals/how-it-works.md) , а также объясняются`Register`и`Export`команды, используемые для подключения C# классов к цели-C. объекты и элементы пользовательского интерфейса.

<a name="Introduction_to_Table_Views" />

## <a name="introduction-to-table-views"></a>Общие сведения о табличных представлениях

Табличное представление отображает данные в табличном формате, содержащем один или несколько столбцов данных в нескольких строках. Табличные представления отображаются в представлениях прокрутки (`NSScrollView`) и начиная с macOS 10,7, для отображения строк и столбцов можно использовать любые `NSView` вместо ячеек (`NSCell`). Тем не менее, вы по-прежнему можете использовать `NSCell` однако вы, как правило, создаете подкласс `NSTableCellView` и создадите пользовательские строки и столбцы.

Табличное представление не сохраняет собственные данные, вместо этого использует источник данных (`NSTableViewDataSource`) для предоставления требуемых строк и столбцов по мере необходимости.

Поведение табличного представления можно настроить, предоставив подкласс делегата представления таблицы (`NSTableViewDelegate`) для поддержки управления столбцами таблицы, тип для выбора функциональности, выбора и редактирования строк, настраиваемого отслеживания и пользовательских представлений для отдельных столбцов и строк.

При создании табличных представлений Apple предлагает следующее:

- Разрешает пользователю сортировать таблицу, щелкая заголовки столбцов.
- Создайте заголовки столбцов, которые являются существительными или короткими фразами существительное, описывающими данные, отображаемые в этом столбце.

Дополнительные сведения см. в разделе " [представления содержимого](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) " [руководства по использованию интерфейса пользователя ОС Apple X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Creating-and-Maintaining-Table-Views-in-Xcode" />

## <a name="creating-and-maintaining-table-views-in-xcode"></a>Создание и обслуживание табличных представлений в Xcode

При создании нового приложения Xamarin. Mac Cocoa по умолчанию получается стандартное пустое окно. Эти окна определяются в `.storyboard` файле, который автоматически включается в проект. Чтобы изменить структуру Windows, в **Обозреватель решений**дважды щелкните файл `Main.storyboard`:

[![](table-view-images/edit01.png "Selecting the main storyboard")](table-view-images/edit01.png#lightbox)

Это приведет к открытию структуры окна в Interface Builder Xcode:

[![](table-view-images/edit02.png "Editing the UI in Xcode")](table-view-images/edit02.png#lightbox)

Введите `table` в поле поиска **инспектора библиотек** , чтобы упростить поиск элементов управления представления таблицы.

[![](table-view-images/edit03.png "Selecting a Table View from the Library")](table-view-images/edit03.png#lightbox)

Перетащите табличное представление на контроллер представления в **редакторе интерфейса**, сделайте его частью области содержимого контроллера представления и установите его в то место, где оно сжимается и растет с окном в **редакторе ограничений**:

[![](table-view-images/edit04.png "Editing constraints")](table-view-images/edit04.png#lightbox)

Выберите табличное представление в **иерархии интерфейсов** , и в **инспекторе атрибутов**доступны следующие свойства:

[![](table-view-images/edit05.png "The Attribute Inspector")](table-view-images/edit05.png#lightbox)

- **Режим содержимого** . позволяет использовать либо представления (`NSView`), либо ячейки (`NSCell`) для отображения данных в строках и столбцах. Начиная с macOS 10,7, следует использовать представления.
- **Строки группы с плавающей запятой** — если `true`, то табличное представление будет рисовать сгруппированные ячейки так, как если бы они были плавающими.
- **Columns (столбцы** ) — определяет количество отображаемых столбцов.
- **Заголовки** — если `true`, столбцы будут иметь заголовки.
- **Переупорядочение** — если `true`, то пользователь сможет перетаскивать столбцы в таблице.
- **Изменение размера** — если `true`, то пользователь сможет перетаскивать заголовки столбцов для изменения размера столбцов.
- **Изменение размера столбца** — управляет тем, как таблица будет иметь автоматический размер столбцов.
- **Выделение** — управляет типом выделения, используемым таблицей при выборе ячейки.
- **Альтернативные строки** — если `true`, то когда бы другие строки имели другой цвет фона.
- **Горизонтальная сетка** — выбор типа границы, нарисованной между ячейками по горизонтали.
- **Вертикальная сетка** — выбор типа границы, нарисованной между ячейками по вертикали.
- **Цвет сетки** — задает цвет границы ячейки.
- **Фон** — задает цвет фона ячейки.
- **Выбор** — позволяет контролировать, как пользователь может выбирать ячейки в таблице следующим образом:
  - Если `true`**несколько** , пользователь может выбрать несколько строк и столбцов.
  - **Столбец** — если `true`, пользователь может выбрать столбцы.
  - **Введите SELECT** — если `true`, пользователь может ввести символ, чтобы выбрать строку.
  - **Пусто** — если `true`, пользователю не нужно выбирать строку или столбец, в таблице допускается отсутствие выбора.
- **Автосохранение** — имя, в котором автоматически сохраняется формат таблиц.
- **Сведения о столбце** — если `true`, порядок и ширина столбцов будут сохраняться автоматически.
- **Разрывы строк** — выберите, как ячейка обрабатывает разрывы строк.
- **Усекает последнюю видимую строку** — если `true`, ячейка будет обрезана в данных, не помещается в границы внутри нее.

> [!IMPORTANT]
> Если вы не обслуживаете устаревшее приложение Xamarin. Mac, `NSView` представления таблиц должны использоваться для представления таблиц на основе `NSCell`. `NSCell` считается устаревшим и может не поддерживаться в дальнейшем.

Выберите столбец таблицы в **иерархии интерфейсов** , и в **инспекторе атрибутов**доступны следующие свойства:

[![](table-view-images/edit06.png "The Attribute Inspector")](table-view-images/edit06.png#lightbox)

- **Заголовок** — задает заголовок столбца.
- **Выравнивание** — задает выравнивание текста внутри ячеек.
- **Шрифт заголовка** — выбирает шрифт для текста заголовка ячейки.
- **Ключ сортировки** — ключ, используемый для сортировки данных в столбце. Оставьте пустым, если пользователь не может сортировать этот столбец.
- **Selector** — **действие** , используемое для выполнения сортировки. Оставьте пустым, если пользователь не может сортировать этот столбец.
- **Order** — порядок сортировки данных столбцов.
- **Изменение размера** — выбор типа изменения размера столбца.
- **Редактируемый** — если `true`, пользователь может изменять ячейки в таблице на основе ячейки.
- **Скрытый** — если `true`, столбец скрыт.

Можно также изменить размер столбца, перетащив его маркер (вертикально на правой стороне столбца) влево или вправо.

Давайте выберем каждый столбец в представлении таблицы и присвойте первому столбцу **заголовок** `Product`, а второй — `Details`.

Выберите табличное представление таблицы (`NSTableViewCell`) в **иерархии интерфейсов** , и в **инспекторе атрибутов**доступны следующие свойства:

[![](table-view-images/edit07.png "The Attribute Inspector")](table-view-images/edit07.png#lightbox)

Это все свойства стандартного представления. Кроме того, здесь можно изменить размер строк для этого столбца.

Выберите ячейку представления таблицы (по умолчанию это `NSTextField`) в **иерархии интерфейсов** , а в **инспекторе атрибутов**доступны следующие свойства:

[![](table-view-images/edit08.png "The Attribute Inspector")](table-view-images/edit08.png#lightbox)

У вас будут все свойства стандартного текстового поля, которые будут заданы здесь. По умолчанию для вывода данных для ячейки в столбце используется стандартное текстовое поле.

Выберите табличное представление таблицы (`NSTableFieldCell`) в **иерархии интерфейсов** , и в **инспекторе атрибутов**доступны следующие свойства:

[![](table-view-images/edit09.png "The Attribute Inspector")](table-view-images/edit09.png#lightbox)

Ниже приведены наиболее важные параметры.

- **Макет** — выберите порядок расположения ячеек в этом столбце.
- **Использует однострочный режим** — если `true`, ячейка ограничена одной строкой.
- **Ширина первой структуры среды выполнения** — если `true`, то в ячейке будет предпочтительнее задать ширину (вручную или автоматически), если она отображается при первом запуске приложения.
- **Действие** — управляет тем, когда **действие** редактирования отправляется для ячейки.
- **Поведение** — определяет, является ли ячейка выбираемой или изменяемой.
- **Форматированный текст** — если `true`, в ячейке может отображаться форматированный текст и стиль текста.
- **Отменить** — если `true`, ячейка принимает ответственность за ее поведение при отмене.

Выберите представление ячейки таблицы (`NSTableFieldCell`) в нижней части столбца таблицы в **иерархии интерфейсов**:

[![](table-view-images/edit10.png "Selecting the Table Cell View")](table-view-images/edit10.png#lightbox)

Это позволяет изменить представление ячейки таблицы, используемое в качестве базового _шаблона_ для всех ячеек, созданных для данного столбца.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Добавление действий и розеток

Как и любой другой элемент управления пользовательского интерфейса Cocoa, нам нужно предоставить представление таблицы, а столбцы и ячейки — C# коду с помощью **действий** и **розеток** (в зависимости от требуемых функций).

Этот процесс одинаков для любого элемента представления таблицы, который мы хотим предоставить:

1. Перейдите в **Редактор помощника** и убедитесь, что выбран файл `ViewController.h`: 

    [![](table-view-images/edit11.png "The Assistant Editor")](table-view-images/edit11.png#lightbox)
2. Выберите табличное представление в **иерархии интерфейс**, щелкните Управление и перетащите в файл `ViewController.h`.
3. Создайте **выход** для представления таблицы с именем `ProductTable`: 

    [![](table-view-images/edit13.png "Configuring an Outlet")](table-view-images/edit13.png#lightbox)
4. Создание **розеток** для столбцов таблиц также называется `ProductColumn` и `DetailsColumn`: 

    [![](table-view-images/edit14.png "Configuring an Outlet")](table-view-images/edit14.png#lightbox)
5. Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Далее мы напишем код, отображающий некоторые данные для таблицы при запуске приложения.

<a name="Populating_the_Table_View" />

## <a name="populating-the-table-view"></a>Заполнение табличного представления

С представлением таблицы, разработанным в Interface Builder и предоставляемых через **розетку**, далее необходимо создать C# код для его заполнения.

Сначала создадим новый класс `Product` для хранения информации по отдельным строкам. В **Обозреватель решений**щелкните правой кнопкой мыши проект и выберите **Добавить** > **новый файл...** Выберите **общие** > **пустой класс**, введите `Product` для **имени** и нажмите кнопку **создать** :

[![](table-view-images/populate01.png "Creating an empty class")](table-view-images/populate01.png#lightbox)

Сделайте файл `Product.cs` выглядеть следующим образом:

```csharp
using System;

namespace MacTables
{
  public class Product
  {
    #region Computed Properties
    public string Title { get; set;} = "";
    public string Description { get; set;} = "";
    #endregion

    #region Constructors
    public Product ()
    {
    }

    public Product (string title, string description)
    {
      this.Title = title;
      this.Description = description;
    }
    #endregion
  }
}

```

Далее необходимо создать подкласс `NSTableDataSource`, чтобы предоставить данные для таблицы по мере запроса. В **Обозреватель решений**щелкните правой кнопкой мыши проект и выберите **Добавить** > **новый файл...** Выберите **общие** > **пустой класс**, введите `ProductTableDataSource` для **имени** и нажмите кнопку **создать** .

Измените файл `ProductTableDataSource.cs` и сделайте его следующим:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacTables
{
  public class ProductTableDataSource : NSTableViewDataSource
  {
    #region Public Variables
    public List<Product> Products = new List<Product>();
    #endregion

    #region Constructors
    public ProductTableDataSource ()
    {
    }
    #endregion

    #region Override Methods
    public override nint GetRowCount (NSTableView tableView)
    {
      return Products.Count;
    }
    #endregion
  }
}

```

Этот класс имеет хранилище для элементов представления таблицы и переопределяет `GetRowCount`, чтобы вернуть число строк в таблице.

Наконец, необходимо создать подкласс `NSTableDelegate`, чтобы обеспечить поведение для нашей таблицы. В **Обозреватель решений**щелкните правой кнопкой мыши проект и выберите **Добавить** > **новый файл...** Выберите **общие** > **пустой класс**, введите `ProductTableDelegate` для **имени** и нажмите кнопку **создать** .

Измените файл `ProductTableDelegate.cs` и сделайте его следующим:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacTables
{
  public class ProductTableDelegate: NSTableViewDelegate
  {
    #region Constants 
    private const string CellIdentifier = "ProdCell";
    #endregion

    #region Private Variables
    private ProductTableDataSource DataSource;
    #endregion

    #region Constructors
    public ProductTableDelegate (ProductTableDataSource datasource)
    {
      this.DataSource = datasource;
    }
    #endregion

    #region Override Methods
    public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
    {
      // This pattern allows you reuse existing views when they are no-longer in use.
      // If the returned view is null, you instance up a new view
      // If a non-null view is returned, you modify it enough to reflect the new data
      NSTextField view = (NSTextField)tableView.MakeView (CellIdentifier, this);
      if (view == null) {
        view = new NSTextField ();
        view.Identifier = CellIdentifier;
        view.BackgroundColor = NSColor.Clear;
        view.Bordered = false;
        view.Selectable = false;
        view.Editable = false;
      }

      // Setup view based on the column selected
      switch (tableColumn.Title) {
      case "Product":
        view.StringValue = DataSource.Products [(int)row].Title;
        break;
      case "Details":
        view.StringValue = DataSource.Products [(int)row].Description;
        break;
      }

      return view;
    }
    #endregion
  }
}
```

При создании экземпляра `ProductTableDelegate`мы также передаем экземпляр `ProductTableDataSource`, который предоставляет данные для таблицы. Метод `GetViewForItem` отвечает за возврат представления (данных) для отображения ячейки для столбца и строки предоставления. Если это возможно, для отображения ячейки будет использоваться существующее представление, если не нужно создавать новое представление.

Чтобы заполнить таблицу, измените файл `ViewController.cs` и сделайте метод `AwakeFromNib` выглядеть следующим образом:

```csharp
public override void AwakeFromNib ()
{
  base.AwakeFromNib ();

  // Create the Product Table Data Source and populate it
  var DataSource = new ProductTableDataSource ();
  DataSource.Products.Add (new Product ("Xamarin.iOS", "Allows you to develop native iOS Applications in C#"));
  DataSource.Products.Add (new Product ("Xamarin.Android", "Allows you to develop native Android Applications in C#"));
  DataSource.Products.Add (new Product ("Xamarin.Mac", "Allows you to develop Mac native Applications in C#"));

  // Populate the Product Table
  ProductTable.DataSource = DataSource;
  ProductTable.Delegate = new ProductTableDelegate (DataSource);
}
```

При запуске приложения отображается следующее:

[![](table-view-images/populate02.png "A sample app run")](table-view-images/populate02.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Сортировка по столбцу

Давайте разрешите пользователю сортировать данные в таблице, щелкнув заголовок столбца. Сначала дважды щелкните файл `Main.storyboard`, чтобы открыть его для редактирования в Interface Builder. Выберите столбец `Product`, введите `Title` для **ключа сортировки**, `compare:` для **селектора** и выберите `Ascending` для **заказа**:

[![](table-view-images/sort01.png "Setting the sort key")](table-view-images/sort01.png#lightbox)

Выберите столбец `Details`, введите `Description` для **ключа сортировки**, `compare:` для **селектора** и выберите `Ascending` для **заказа**:

[![](table-view-images/sort02.png "Setting the sort key")](table-view-images/sort02.png#lightbox)

Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Теперь изменим файл `ProductTableDataSource.cs` и добавим следующие методы:

```csharp
public void Sort(string key, bool ascending) {

  // Take action based on key
  switch (key) {
  case "Title":
    if (ascending) {
      Products.Sort ((x, y) => x.Title.CompareTo (y.Title));
    } else {
      Products.Sort ((x, y) => -1 * x.Title.CompareTo (y.Title));
    }
    break;
  case "Description":
    if (ascending) {
      Products.Sort ((x, y) => x.Description.CompareTo (y.Description));
    } else {
      Products.Sort ((x, y) => -1 * x.Description.CompareTo (y.Description));
    }
    break;
  }

}

public override void SortDescriptorsChanged (NSTableView tableView, NSSortDescriptor[] oldDescriptors)
{
  // Sort the data
  if (oldDescriptors.Length > 0) {
    // Update sort
    Sort (oldDescriptors [0].Key, oldDescriptors [0].Ascending);
  } else {
    // Grab current descriptors and update sort
    NSSortDescriptor[] tbSort = tableView.SortDescriptors; 
    Sort (tbSort[0].Key, tbSort[0].Ascending); 
  }
      
  // Refresh table
  tableView.ReloadData ();
}
```

Метод `Sort` позволяет нам сортировать данные в источнике данных на основе заданного поля `Product` класса в порядке возрастания или убывания. Переопределенный метод `SortDescriptorsChanged` будет вызываться каждый раз при нажатии кнопки мыши на заголовке столбца. Ему будет передано значение **ключа** , установленное в Interface Builder, и порядок сортировки для этого столбца.

Если запустить приложение и щелкнуть в заголовках столбцов, строки будут отсортированы по этому столбцу:

[![](table-view-images/sort03.png "An example app run")](table-view-images/sort03.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Выбор строк

Если вы хотите разрешить пользователю выбирать одну строку, дважды щелкните файл `Main.storyboard`, чтобы открыть его для редактирования в Interface Builder. Выберите табличное представление в **иерархии интерфейсов** и снимите флажок **несколько** в **инспекторе атрибутов**:

[![](table-view-images/select01.png "The Attribute Inspector")](table-view-images/select01.png#lightbox)

Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Затем измените файл `ProductTableDelegate.cs` и добавьте следующий метод:

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
  return true;
}
```

Это позволит пользователю выбрать одну строку в представлении таблицы. Возвращают `false` для `ShouldSelectRow` для любой строки, которая не хочет, чтобы пользователь мог выбрать или `false` для каждой строки, если вы не хотите, чтобы пользователь мог выбрать какие бы то ни было строки.

Табличное представление (`NSTableView`) содержит следующие методы для работы с выбором строк:

- `DeselectRow(nint)` — отменяет выделение данной строки в таблице.
- `SelectRow(nint,bool)` — выбирает заданную строку. Передайте `false` для второго параметра, чтобы выбрать только одну строку за раз.
- `SelectedRow` — возвращает текущую строку, выбранную в таблице.
- `IsRowSelected(nint)` — возвращает значение, `true`, если выбрана указанная строка.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Выбор нескольких строк

Если вы хотите разрешить пользователю выбирать несколько строк, дважды щелкните файл `Main.storyboard`, чтобы открыть его для редактирования в Interface Builder. Выберите табличное представление в **иерархии интерфейсов** и установите флажок **несколько** в **инспекторе атрибутов**:

[![](table-view-images/select02.png "The Attribute Inspector")](table-view-images/select02.png#lightbox)

Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Затем измените файл `ProductTableDelegate.cs` и добавьте следующий метод:

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
  return true;
}
```

Это позволит пользователю выбрать одну строку в представлении таблицы. Возвращают `false` для `ShouldSelectRow` для любой строки, которая не хочет, чтобы пользователь мог выбрать или `false` для каждой строки, если вы не хотите, чтобы пользователь мог выбрать какие бы то ни было строки.

Табличное представление (`NSTableView`) содержит следующие методы для работы с выбором строк:

- `DeselectAll(NSObject)` — отменяет выбор всех строк в таблице. Используйте `this`, чтобы первый параметр отправлялся в объекте, выполняющем выборку. 
- `DeselectRow(nint)` — отменяет выделение данной строки в таблице.
- `SelectAll(NSobject)` — выбирает все строки в таблице. Используйте `this`, чтобы первый параметр отправлялся в объекте, выполняющем выборку.
- `SelectRow(nint,bool)` — выбирает заданную строку. Передайте `false` второго параметра снимите выделение и выберите только одну строку, передайте `true`, чтобы расширить выбор и включить эту строку.
- `SelectRows(NSIndexSet,bool)` — выбирает заданный набор строк. Передайте `false` второго параметра снимите выделение и выберите только те строки, передайте `true`, чтобы расширить выбор и включить эти строки.
- `SelectedRow` — возвращает текущую строку, выбранную в таблице.
- `SelectedRows` — возвращает `NSIndexSet`, содержащий индексы выбранных строк.
- `SelectedRowCount` — возвращает число выбранных строк.
- `IsRowSelected(nint)` — возвращает значение, `true`, если выбрана указанная строка.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Введите, чтобы выбрать строку

Если вы хотите разрешить пользователю вводить символ с выбранным табличным представлением и выбрать первую строку с этим символом, дважды щелкните файл `Main.storyboard`, чтобы открыть его для редактирования в Interface Builder. Выберите табличное представление в **иерархии интерфейсов** и установите флажок **тип SELECT** в **инспекторе атрибутов**:

[![](table-view-images/type01.png "Setting the selection type")](table-view-images/type01.png#lightbox)

Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Теперь изменим файл `ProductTableDelegate.cs` и добавим следующий метод:

```csharp
public override nint GetNextTypeSelectMatch (NSTableView tableView, nint startRow, nint endRow, string searchString)
{
  nint row = 0;
  foreach(Product product in DataSource.Products) {
    if (product.Title.Contains(searchString)) return row;

    // Increment row counter
    ++row;
  }

  // If not found select the first row
  return 0;
}
```

Метод `GetNextTypeSelectMatch` принимает заданную `searchString` и возвращает строку первого `Product`, которая содержит эту строку в `Title`.

Если запустить приложение и ввести символ, выбирается строка:

[![](table-view-images/type02.png "A sample app run")](table-view-images/type02.png#lightbox)

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Изменение порядка столбцов

Если вы хотите разрешить пользователю перетаскивать столбцы переупорядочения в табличном представлении, дважды щелкните файл `Main.storyboard`, чтобы открыть его для редактирования в Interface Builder. Выберите табличное представление в **иерархии интерфейсов** и установите флажок **изменить порядок** в **инспекторе атрибутов**:

[![](table-view-images/reorder01.png "The Attribute Inspector")](table-view-images/reorder01.png#lightbox)

Если присвоить значение свойству **автосохранения** и проверить поле **сведения о столбце** , все изменения, внесенные в макет таблицы, будут автоматически сохранены для нас и восстановлены при следующем запуске приложения.

Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Теперь изменим файл `ProductTableDelegate.cs` и добавим следующий метод:

```csharp
public override bool ShouldReorder (NSTableView tableView, nint columnIndex, nint newColumnIndex)
{
  return true;
}
```

Метод `ShouldReorder` должен возвращать `true` для любого столбца, который требуется перетаскивать в `newColumnIndex`, иначе возвращает `false`.

При запуске приложения можно перетаскивать заголовки столбцов для изменения порядка наших столбцов:

[![](table-view-images/reorder02.png "An example of the reordered columns")](table-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Редактирование ячеек

Если вы хотите разрешить пользователю изменять значения для определенной ячейки, измените файл `ProductTableDelegate.cs` и измените метод `GetViewForItem` следующим образом:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{
  // This pattern allows you reuse existing views when they are no-longer in use.
  // If the returned view is null, you instance up a new view
  // If a non-null view is returned, you modify it enough to reflect the new data
  NSTextField view = (NSTextField)tableView.MakeView (tableColumn.Title, this);
  if (view == null) {
    view = new NSTextField ();
    view.Identifier = tableColumn.Title;
    view.BackgroundColor = NSColor.Clear;
    view.Bordered = false;
    view.Selectable = false;
    view.Editable = true;

    view.EditingEnded += (sender, e) => {
          
      // Take action based on type
      switch(view.Identifier) {
      case "Product":
        DataSource.Products [(int)view.Tag].Title = view.StringValue;
        break;
      case "Details":
        DataSource.Products [(int)view.Tag].Description = view.StringValue;
        break; 
      }
    };
  }

  // Tag view
  view.Tag = row;

  // Setup view based on the column selected
  switch (tableColumn.Title) {
  case "Product":
    view.StringValue = DataSource.Products [(int)row].Title;
    break;
  case "Details":
    view.StringValue = DataSource.Products [(int)row].Description;
    break;
  }

  return view;
}
```

Теперь, если приложение запускается, пользователь может изменять ячейки в представлении таблицы:

[![](table-view-images/editing01.png "An example of editing a cell")](table-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Table_Views" />

## <a name="using-images-in-table-views"></a>Использование изображений в представлениях таблиц

Чтобы включить изображение как часть ячейки в `NSTableView`, необходимо изменить способ возврата данных `NSTableViewDelegate's`ным методом `GetViewForItem` табличного представления, чтобы использовать `NSTableCellView` вместо типичного `NSTextField`. Пример:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

  // This pattern allows you reuse existing views when they are no-longer in use.
  // If the returned view is null, you instance up a new view
  // If a non-null view is returned, you modify it enough to reflect the new data
  NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
  if (view == null) {
    view = new NSTableCellView ();
    if (tableColumn.Title == "Product") {
      view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
      view.AddSubview (view.ImageView);
      view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
    } else {
      view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
    }
    view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
    view.AddSubview (view.TextField);
    view.Identifier = tableColumn.Title;
    view.TextField.BackgroundColor = NSColor.Clear;
    view.TextField.Bordered = false;
    view.TextField.Selectable = false;
    view.TextField.Editable = true;

    view.TextField.EditingEnded += (sender, e) => {

      // Take action based on type
      switch(view.Identifier) {
      case "Product":
        DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
        break;
      case "Details":
        DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
        break; 
      }
    };
  }

  // Tag view
  view.TextField.Tag = row;

  // Setup view based on the column selected
  switch (tableColumn.Title) {
  case "Product":
    view.ImageView.Image = NSImage.ImageNamed ("tags.png");
    view.TextField.StringValue = DataSource.Products [(int)row].Title;
    break;
  case "Details":
    view.TextField.StringValue = DataSource.Products [(int)row].Description;
    break;
  }

  return view;
}
```

Дополнительные сведения см. в разделе [Использование изображений с табличными представлениями](~/mac/app-fundamentals/image.md) документации по [работе с изображением](~/mac/app-fundamentals/image.md) .

<a name="Adding-a-Delete-Button-to-a-Row" />

## <a name="adding-a-delete-button-to-a-row"></a>Добавление кнопки «Удалить» в строку

В зависимости от требований приложения в некоторых случаях необходимо указать кнопку действия для каждой строки в таблице. В качестве примера можно развернуть пример представления таблицы, созданный выше, чтобы включить кнопку **Delete** в каждую строку.

Сначала измените `Main.storyboard` в Interface Builder Xcode, выберите табличное представление и увеличьте число столбцов до трех (3). Затем измените **заголовок** нового столбца на `Action`:

[![](table-view-images/delete01.png "Editing the column name")](table-view-images/delete01.png#lightbox)

Сохраните изменения в раскадровке и вернитесь в Visual Studio для Mac, чтобы синхронизировать изменения.

Затем измените файл `ViewController.cs` и добавьте следующий открытый метод:

```csharp
public void ReloadTable ()
{
  ProductTable.ReloadData ();
}
```

В том же файле измените создание нового делегата представления таблицы в методе `ViewDidLoad` следующим образом:

```csharp
// Populate the Product Table
ProductTable.DataSource = DataSource;
ProductTable.Delegate = new ProductTableDelegate (this, DataSource);
```

Теперь измените файл `ProductTableDelegate.cs`, включив в него частное подключение к контроллеру представления и воспринимающее контроллер в качестве параметра при создании нового экземпляра делегата:

```csharp
#region Private Variables
private ProductTableDataSource DataSource;
private ViewController Controller;
#endregion

#region Constructors
public ProductTableDelegate (ViewController controller, ProductTableDataSource datasource)
{
  this.Controller = controller;
  this.DataSource = datasource;
}
#endregion
```

Затем добавьте в класс следующий новый частный метод:

```csharp
private void ConfigureTextField (NSTableCellView view, nint row)
{
  // Add to view
  view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
  view.AddSubview (view.TextField);

  // Configure
  view.TextField.BackgroundColor = NSColor.Clear;
  view.TextField.Bordered = false;
  view.TextField.Selectable = false;
  view.TextField.Editable = true;

  // Wireup events
  view.TextField.EditingEnded += (sender, e) => {

    // Take action based on type
    switch (view.Identifier) {
    case "Product":
      DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
      break;
    case "Details":
      DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
      break;
    }
  };

  // Tag view
  view.TextField.Tag = row;
}
```

Это принимает все конфигурации текстового представления, которые ранее выполнялись в `GetViewForItem` методе, и помещает их в единое, вызываемое расположение (поскольку последний столбец таблицы не включает текстовое представление, а кнопку).

Наконец, измените метод `GetViewForItem` и сделайте его следующим:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

  // This pattern allows you reuse existing views when they are no-longer in use.
  // If the returned view is null, you instance up a new view
  // If a non-null view is returned, you modify it enough to reflect the new data
  NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
  if (view == null) {
    view = new NSTableCellView ();

    // Configure the view
    view.Identifier = tableColumn.Title;

    // Take action based on title
    switch (tableColumn.Title) {
    case "Product":
      view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
      view.AddSubview (view.ImageView);
      view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
      ConfigureTextField (view, row);
      break;
    case "Details":
      view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
      ConfigureTextField (view, row);
      break;
    case "Action":
      // Create new button
      var button = new NSButton (new CGRect (0, 0, 81, 16));
      button.SetButtonType (NSButtonType.MomentaryPushIn);
      button.Title = "Delete";
      button.Tag = row;

      // Wireup events
      button.Activated += (sender, e) => {
        // Get button and product
        var btn = sender as NSButton;
        var product = DataSource.Products [(int)btn.Tag];

        // Configure alert
        var alert = new NSAlert () {
          AlertStyle = NSAlertStyle.Informational,
          InformativeText = $"Are you sure you want to delete {product.Title}? This operation cannot be undone.",
          MessageText = $"Delete {product.Title}?",
        };
        alert.AddButton ("Cancel");
        alert.AddButton ("Delete");
        alert.BeginSheetForResponse (Controller.View.Window, (result) => {
          // Should we delete the requested row?
          if (result == 1001) {
            // Remove the given row from the dataset
            DataSource.Products.RemoveAt((int)btn.Tag);
            Controller.ReloadTable ();
          }
        });
      };

      // Add to view
      view.AddSubview (button);
      break;
    }

  }

  // Setup view based on the column selected
  switch (tableColumn.Title) {
  case "Product":
    view.ImageView.Image = NSImage.ImageNamed ("tag.png");
    view.TextField.StringValue = DataSource.Products [(int)row].Title;
    view.TextField.Tag = row;
    break;
  case "Details":
    view.TextField.StringValue = DataSource.Products [(int)row].Description;
    view.TextField.Tag = row;
    break;
  case "Action":
    foreach (NSView subview in view.Subviews) {
      var btn = subview as NSButton;
      if (btn != null) {
        btn.Tag = row;
      }
    }
    break;
  }

  return view;
}
```

Рассмотрим несколько разделов этого кода более подробно. Во-первых, если создается новое `NSTableViewCell` действие выполняется на основе имени столбца. Для первых двух столбцов (**Product** и **Details**) вызывается новый метод `ConfigureTextField`.

Для столбца **Action (действие** ) создается новый `NSButton` и добавляется в ячейку как Подпредставление:

```csharp
// Create new button
var button = new NSButton (new CGRect (0, 0, 81, 16));
button.SetButtonType (NSButtonType.MomentaryPushIn);
button.Title = "Delete";
button.Tag = row;
...

// Add to view
view.AddSubview (button);
```

Свойство `Tag` кнопки используется для хранения номера строки, которая обрабатывается в данный момент. Этот номер будет использоваться позже, когда пользователь запросит строку для удаления в событии `Activated` кнопки:

```csharp
// Wireup events
button.Activated += (sender, e) => {
  // Get button and product
  var btn = sender as NSButton;
  var product = DataSource.Products [(int)btn.Tag];

  // Configure alert
  var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = $"Are you sure you want to delete {product.Title}? This operation cannot be undone.",
    MessageText = $"Delete {product.Title}?",
  };
  alert.AddButton ("Cancel");
  alert.AddButton ("Delete");
  alert.BeginSheetForResponse (Controller.View.Window, (result) => {
    // Should we delete the requested row?
    if (result == 1001) {
      // Remove the given row from the dataset
      DataSource.Products.RemoveAt((int)btn.Tag);
      Controller.ReloadTable ();
    }
  });
};
```

В начале обработчика событий мы получаем кнопку и продукт, который находится в указанной строке таблицы. Затем пользователю предлагается предупреждение об удалении строки. Если пользователь выбирает удаление строки, заданная строка удаляется из источника данных, а таблица перегружается:

```csharp
// Remove the given row from the dataset
DataSource.Products.RemoveAt((int)btn.Tag);
Controller.ReloadTable ();
```

Наконец, если вместо создания новой ячейки табличного представления используется повторно, следующий код настраивает его на основе обрабатываемого столбца:

```csharp
// Setup view based on the column selected
switch (tableColumn.Title) {
case "Product":
  view.ImageView.Image = NSImage.ImageNamed ("tag.png");
  view.TextField.StringValue = DataSource.Products [(int)row].Title;
  view.TextField.Tag = row;
  break;
case "Details":
  view.TextField.StringValue = DataSource.Products [(int)row].Description;
  view.TextField.Tag = row;
  break;
case "Action":
  foreach (NSView subview in view.Subviews) {
    var btn = subview as NSButton;
    if (btn != null) {
      btn.Tag = row;
    }
  }
  break;
}

```

Для столбца **Action** все вложенные представления проверяются до тех пор, пока не будет найден `NSButton`, а затем свойство `Tag` обновляется, чтобы указывать на текущую строку.

После внесения этих изменений при запуске приложения каждая строка будет иметь кнопку **Удалить** :

[![](table-view-images/delete02.png "The table view with deletion buttons")](table-view-images/delete02.png#lightbox)

Когда пользователь нажимает кнопку " **Удалить** ", отобразится предупреждение с запросом на удаление заданной строки:

[![](table-view-images/delete03.png "A delete row alert")](table-view-images/delete03.png#lightbox)

Если пользователь выберет DELETE, строка будет удалена, а таблица будет перерисована:

[![](table-view-images/delete04.png "The table after the row is deleted")](table-view-images/delete04.png#lightbox)

<a name="Data_Binding_Table_Views" />

## <a name="data-binding-table-views"></a>Представления таблиц привязки данных

Благодаря использованию кодирования и методов привязки данных в приложении Xamarin. Mac можно значительно сократить объем кода, который необходимо написать и поддерживать, чтобы заполнить элементы пользовательского интерфейса и работать с ними. Кроме того, у вас есть преимущество в дальнейшем разделять резервные данные (_модель данных_) от интерфейса пользователя переднего плана (_модель-представление-контроллер_), что упрощает обслуживание, более гибкое проектирование приложений.

Кодирование "ключ-значение" (КВК) — это механизм непрямого доступа к свойствам объекта с помощью клавиш (специально отформатированных строк) для обнаружения свойств вместо доступа к ним через переменные экземпляра или методы доступа (`get/set`). Реализуя методы доступа, совместимые с кодированием значений, в приложении Xamarin. Mac, вы получаете доступ к другим macOS функциям, таким как контроль значений ключа (кво), привязка данных, основные данные, привязки Cocoa и сценарии.

Дополнительные сведения см. в разделе [Привязка данных табличного представления](~/mac/app-fundamentals/databinding.md#Table_View_Data_Binding) в нашей [привязке данных и документации по кодированию "ключ-значение](~/mac/app-fundamentals/databinding.md) ".

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье подробно рассматривается работа с табличными представлениями в приложении Xamarin. Mac. Мы видели различные типы и использование табличных представлений, как создавать и поддерживать табличные представления в Interface Builder Xcode и как работать с табличными представлениями в C# коде.

## <a name="related-links"></a>Связанные ссылки

- [Мактаблес (пример)](https://docs.microsoft.com/samples/xamarin/mac-samples/mactables)
- [MacImages (пример)](https://docs.microsoft.com/samples/xamarin/mac-samples/macimages)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Представления структур](~/mac/user-interface/outline-view.md)
- [Списки источников](~/mac/user-interface/source-list.md)
- [Привязка данных и кодирование типа "ключ — значение"](~/mac/app-fundamentals/databinding.md)
- [Рекомендации по работе с человеческим интерфейсом OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [нстаблевиев](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSTableView_Class/index.html#//apple_ref/doc/uid/TP40004125)
- [нстаблевиевделегате](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSTableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008622)
- [нстаблевиевдатасаурце](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSTableDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004178)
