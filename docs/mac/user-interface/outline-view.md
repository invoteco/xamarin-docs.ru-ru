---
title: Структурированные представления в Xamarin. Mac
description: В этой статье рассматривается работа с представлениями структуры в приложении Xamarin. Mac. Здесь описывается создание и обслуживание представлений структуры в Xcode и Interface Builder и работа с ними программными средствами.
ms.prod: xamarin
ms.assetid: 043248EE-11DA-4E96-83A3-08824A4F2E01
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: dd710f13d9324ce31e08641f214241e0433fe809
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73004342"
---
# <a name="outline-views-in-xamarinmac"></a>Структурированные представления в Xamarin. Mac

_В этой статье рассматривается работа с представлениями структуры в приложении Xamarin. Mac. Здесь описывается создание и обслуживание представлений структуры в Xcode и Interface Builder и работа с ними программными средствами._

При работе с C# и .NET в приложении Xamarin. Mac у вас есть доступ к тем же представлениям структуры, что и разработчик, работающий на *уровне цели-C* и *Xcode* . Так как Xamarin. Mac интегрируется непосредственно с Xcode, можно использовать _Interface Builder_ Xcode для создания и сопровождения представлений структуры (или при необходимости создавать их непосредственно в C# коде).

Представление структуры — это тип таблицы, позволяющий пользователю разворачивать или сворачивать строки иерархических данных. Как и табличное представление, в представлении структуры отображаются данные для набора связанных элементов, строки которых представляют отдельные элементы и столбцы, представляющие атрибуты этих элементов. В отличие от табличного представления элементы в представлении структуры не расположены в плоском списке, они организованы в виде иерархии, например файлов и папок на жестком диске.

[![](outline-view-images/populate03.png "An example app run")](outline-view-images/populate03.png#lightbox)

В этой статье рассматриваются основные принципы работы с представлениями структуры в приложении Xamarin. Mac. Мы настоятельно рекомендуем сначала ознакомиться со статьей [Hello, Mac](~/mac/get-started/hello-mac.md) , в частности [Знакомство с Xcode и Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) , а также с разделом "возможности [и действия](~/mac/get-started/hello-mac.md#outlets-and-actions) ", так как в нем рассматриваются основные понятия и методы, которые мы будем использовать в Эта статья.

Возможно, вы захотите ознакомиться с разделом [ C# классы и методы для цели-C](~/mac/internals/how-it-works.md) в документе о внутренних компонентах [Xamarin. Mac](~/mac/internals/how-it-works.md) , а также объясняются`Register`и`Export`команды, используемые для подключения C# классов к цели-C. объекты и элементы пользовательского интерфейса.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-outline-views"></a>Общие сведения о представлениях структуры

Представление структуры — это тип таблицы, позволяющий пользователю разворачивать или сворачивать строки иерархических данных. Как и табличное представление, в представлении структуры отображаются данные для набора связанных элементов, строки которых представляют отдельные элементы и столбцы, представляющие атрибуты этих элементов. В отличие от табличного представления элементы в представлении структуры не расположены в плоском списке, они организованы в виде иерархии, например файлов и папок на жестком диске.

Если элемент в режиме структуры содержит другие элементы, он может быть развернут или свернут пользователем. Развертываемый элемент отображает треугольник раскрытия, который указывает вправо, когда элемент свернут, и указывает на то, что элемент развернут. Щелчок на треугольнике раскрытия приводит к тому, что элемент разворачивается или сворачивается.

Представление структуры (`NSOutlineView`) является подклассом табличного представления (`NSTableView`) и, как таковое, наследует большую часть его поведения от родительского класса. В результате многие операции, поддерживаемые табличным представлением, такие как выбор строк или столбцов, изменение расположения столбцов путем перетаскивания заголовков столбцов и т. д., также поддерживаются в режиме структуры. Приложение Xamarin. Mac управляет этими функциями и может настраивать параметры представления структуры (в коде или Interface Builder), чтобы разрешить или запретить определенные операции.

В режиме структуры не сохраняются собственные данные, вместо этого используется источник данных (`NSOutlineViewDataSource`) для предоставления требуемых строк и столбцов по мере необходимости.

Поведение представления структуры можно настроить путем предоставления подкласса делегата представления структуры (`NSOutlineViewDelegate`) для поддержки управления столбцами структуры, ввода для выбора функциональности, выбора строк и редактирования, настраиваемого отслеживания и пользовательских представлений для отдельных столбцов. и строки.

Так как представление структуры использует большую часть поведения и функций в табличном представлении, перед продолжением работы с этой статьей может потребоваться перейти к документации по [табличным представлениям](~/mac/user-interface/table-view.md) .

<a name="Creating_and_Maintaining_Outline_Views_in_Xcode" />

## <a name="creating-and-maintaining-outline-views-in-xcode"></a>Создание и обслуживание представлений структуры в Xcode

При создании нового приложения Xamarin. Mac Cocoa по умолчанию получается стандартное пустое окно. Эти окна определяются в `.storyboard` файле, который автоматически включается в проект. Чтобы изменить структуру Windows, в **Обозреватель решений**дважды щелкните файл `Main.storyboard`:

[![](outline-view-images/edit01.png "Selecting the main storyboard")](outline-view-images/edit01.png#lightbox)

Это приведет к открытию структуры окна в Interface Builder Xcode:

[![](outline-view-images/edit02.png "Editing the UI in Xcode")](outline-view-images/edit02.png#lightbox)

Введите `outline` в поле поиска **инспектора библиотек** , чтобы упростить поиск элементов управления в режиме структуры:

[![](outline-view-images/edit03.png "Selecting an Outline View from the Library")](outline-view-images/edit03.png#lightbox)

Перетащите представление структуры на контроллер представления в **редакторе интерфейса**, сделайте его частью области содержимого контроллера представления и установите его в то место, где оно сжимается и растет с окном в **редакторе ограничений**:

[![](outline-view-images/edit04.png "Editing the constraints")](outline-view-images/edit04.png#lightbox)

Выберите представление структуры в **иерархии интерфейс** , и в **инспекторе атрибутов**доступны следующие свойства:

[![](outline-view-images/edit05.png "The Attribute Inspector")](outline-view-images/edit05.png#lightbox)

- **Столбец структуры** — столбец таблицы, в котором отображаются иерархические данные.
- **Столбец структуры автосохранения** — если `true`, столбец структуры будет автоматически сохранен и восстановлен между запусками приложений.
- **Отступ** — Величина отступа для столбцов в развернутом элементе.
- **Отступы следуют за ячейками** . Если `true`, то отступ будет отображаться вместе с ячейками.
- **Автосохранение расширенных элементов** . Если `true`, развернутое или свернутое состояние элементов будет автоматически сохранено и восстановлено между запусками приложений.
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
> Если вы не обслуживаете устаревшее приложение Xamarin. Mac, `NSView` представления структуры на основе данных следует использовать для представлений таблиц на основе `NSCell`. `NSCell` считается устаревшим и может не поддерживаться в дальнейшем.

Выберите столбец таблицы в **иерархии интерфейсов** , и в **инспекторе атрибутов**доступны следующие свойства:

[![](outline-view-images/edit06.png "The Attribute Inspector")](outline-view-images/edit06.png#lightbox)

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

[![](outline-view-images/edit07.png "The Attribute Inspector")](outline-view-images/edit07.png#lightbox)

Это все свойства стандартного представления. Кроме того, здесь можно изменить размер строк для этого столбца.

Выберите ячейку представления таблицы (по умолчанию это `NSTextField`) в **иерархии интерфейсов** , а в **инспекторе атрибутов**доступны следующие свойства:

[![](outline-view-images/edit08.png "The Attribute Inspector")](outline-view-images/edit08.png#lightbox)

У вас будут все свойства стандартного текстового поля, которые будут заданы здесь. По умолчанию для вывода данных для ячейки в столбце используется стандартное текстовое поле.

Выберите табличное представление таблицы (`NSTableFieldCell`) в **иерархии интерфейсов** , и в **инспекторе атрибутов**доступны следующие свойства:

[![](outline-view-images/edit09.png "The Attribute Inspector")](outline-view-images/edit09.png#lightbox)

Ниже приведены наиболее важные параметры.

- **Макет** — выберите порядок расположения ячеек в этом столбце.
- **Использует однострочный режим** — если `true`, ячейка ограничена одной строкой.
- **Ширина первой структуры среды выполнения** — если `true`, то в ячейке будет предпочтительнее задать ширину (вручную или автоматически), если она отображается при первом запуске приложения.
- **Действие** — управляет тем, когда **действие** редактирования отправляется для ячейки.
- **Поведение** — определяет, является ли ячейка выбираемой или изменяемой.
- **Форматированный текст** — если `true`, в ячейке может отображаться форматированный текст и стиль текста.
- **Отменить** — если `true`, ячейка принимает ответственность за ее поведение при отмене.

Выберите представление ячейки таблицы (`NSTableFieldCell`) в нижней части столбца таблицы в **иерархии интерфейсов**:

[![](outline-view-images/edit11.png "Selecting the table cell view")](outline-view-images/edit10.png#lightbox)

Это позволяет изменить представление ячейки таблицы, используемое в качестве базового _шаблона_ для всех ячеек, созданных для данного столбца.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Добавление действий и розеток

Как и любой другой элемент управления пользовательского интерфейса Cocoa, нам нужно предоставить представление структуры, а столбцы и ячейки — C# коду с помощью **действий** и **розеток** (в зависимости от требуемых функций).

Этот процесс одинаков для любого элемента представления структуры, который мы хотим предоставить:

1. Перейдите в **Редактор помощника** и убедитесь, что выбран файл `ViewController.h`:

    [![](outline-view-images/edit11.png "Selecting the correct .h file")](outline-view-images/edit11.png#lightbox)
2. Выберите представление структуры в **иерархии интерфейс**, нажмите клавишу Control и перетащите в файл `ViewController.h`.
3. Создайте **выход** для представления структуры с именем `ProductOutline`:

    [![](outline-view-images/edit13.png "Configuring an Outlet")](outline-view-images/edit13.png#lightbox)
4. Создание **розеток** для столбцов таблиц также называется `ProductColumn` и `DetailsColumn`:

    [![](outline-view-images/edit14.png "Configuring an Outlet")](outline-view-images/edit14.png#lightbox)
5. Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Далее мы напишем код, отображающий некоторые данные для структуры при запуске приложения.

<a name="Populating_the_Table_View" />

## <a name="populating-the-outline-view"></a>Заполнение представления структуры

С представлением структуры, разработанным в Interface Builder и предоставляемых через **розетку**, далее необходимо создать C# код для его заполнения.

Сначала создадим новый класс `Product` для хранения информации по отдельным строкам и группам вспомогательных продуктов. В **Обозреватель решений**щелкните правой кнопкой мыши проект и выберите **Добавить** > **новый файл...** Выберите **общие** > **пустой класс**, введите `Product` для **имени** и нажмите кнопку **создать** :

[![](outline-view-images/populate01.png "Creating an empty class")](outline-view-images/populate01.png#lightbox)

Сделайте файл `Product.cs` выглядеть следующим образом:

```csharp
using System;
using Foundation;
using System.Collections.Generic;

namespace MacOutlines
{
    public class Product : NSObject
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Computed Properties
        public string Title { get; set;} = "";
        public string Description { get; set;} = "";
        public bool IsProductGroup {
            get { return (Products.Count > 0); }
        }
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

Далее необходимо создать подкласс `NSOutlineDataSource`, чтобы предоставить данные для нашего структуры по запросу. В **Обозреватель решений**щелкните правой кнопкой мыши проект и выберите **Добавить** > **новый файл...** Выберите **общие** > **пустой класс**, введите `ProductOutlineDataSource` для **имени** и нажмите кнопку **создать** .

Измените файл `ProductTableDataSource.cs` и сделайте его следующим:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacOutlines
{
    public class ProductOutlineDataSource : NSOutlineViewDataSource
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Constructors
        public ProductOutlineDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override nint GetChildrenCount (NSOutlineView outlineView, NSObject item)
        {
            if (item == null) {
                return Products.Count;
            } else {
                return ((Product)item).Products.Count;
            }

        }

        public override NSObject GetChild (NSOutlineView outlineView, nint childIndex, NSObject item)
        {
            if (item == null) {
                return Products [childIndex];
            } else {
                return ((Product)item).Products [childIndex];
            }

        }

        public override bool ItemExpandable (NSOutlineView outlineView, NSObject item)
        {
            if (item == null) {
                return Products [0].IsProductGroup;
            } else {
                return ((Product)item).IsProductGroup;
            }

        }
        #endregion
    }
}
```

Этот класс имеет хранилище для элементов представления структуры и переопределяет `GetChildrenCount`, чтобы вернуть число строк в таблице. `GetChild` возвращает конкретный родительский или дочерний элемент (согласно запросу в режиме структуры), а `ItemExpandable` определяет указанный элемент как родительский или дочерний.

Наконец, необходимо создать подкласс `NSOutlineDelegate`, чтобы обеспечить поведение для нашего контура. В **Обозреватель решений**щелкните правой кнопкой мыши проект и выберите **Добавить** > **новый файл...** Выберите **общие** > **пустой класс**, введите `ProductOutlineDelegate` для **имени** и нажмите кнопку **создать** .

Измените файл `ProductOutlineDelegate.cs` и сделайте его следующим:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacOutlines
{
    public class ProductOutlineDelegate : NSOutlineViewDelegate
    {
        #region Constants
        private const string CellIdentifier = "ProdCell";
        #endregion

        #region Private Variables
        private ProductOutlineDataSource DataSource;
        #endregion

        #region Constructors
        public ProductOutlineDelegate (ProductOutlineDataSource datasource)
        {
            this.DataSource = datasource;
        }
        #endregion

        #region Override Methods

        public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)outlineView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Cast item
            var product = item as Product;

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Product":
                view.StringValue = product.Title;
                break;
            case "Details":
                view.StringValue = product.Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

При создании экземпляра `ProductOutlineDelegate`мы также передаем экземпляр `ProductOutlineDataSource`, который предоставляет данные для структуры. Метод `GetView` отвечает за возврат представления (данных) для отображения ячейки для столбца и строки предоставления. Если это возможно, для отображения ячейки будет использоваться существующее представление, если не нужно создавать новое представление.

Чтобы заполнить структуру, отредактируйте файл `MainWindow.cs` и сделайте метод `AwakeFromNib` выглядеть следующим образом:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create data source and populate
    var DataSource = new ProductOutlineDataSource ();

    var Vegetables = new Product ("Vegetables", "Greens and Other Produce");
    Vegetables.Products.Add (new Product ("Cabbage", "Brassica oleracea - Leaves, axillary buds, stems, flowerheads"));
    Vegetables.Products.Add (new Product ("Turnip", "Brassica rapa - Tubers, leaves"));
    Vegetables.Products.Add (new Product ("Radish", "Raphanus sativus - Roots, leaves, seed pods, seed oil, sprouting"));
    Vegetables.Products.Add (new Product ("Carrot", "Daucus carota - Root tubers"));
    DataSource.Products.Add (Vegetables);

    var Fruits = new Product ("Fruits", "Fruit is a part of a flowering plant that derives from specific tissues of the flower");
    Fruits.Products.Add (new Product ("Grape", "True Berry"));
    Fruits.Products.Add (new Product ("Cucumber", "Pepo"));
    Fruits.Products.Add (new Product ("Orange", "Hesperidium"));
    Fruits.Products.Add (new Product ("Blackberry", "Aggregate fruit"));
    DataSource.Products.Add (Fruits);

    var Meats = new Product ("Meats", "Lean Cuts");
    Meats.Products.Add (new Product ("Beef", "Cow"));
    Meats.Products.Add (new Product ("Pork", "Pig"));
    Meats.Products.Add (new Product ("Veal", "Young Cow"));
    DataSource.Products.Add (Meats);

    // Populate the outline
    ProductOutline.DataSource = DataSource;
    ProductOutline.Delegate = new ProductOutlineDelegate (DataSource);

}
```

При запуске приложения отображается следующее:

[![](outline-view-images/populate02.png "The collapsed view")](outline-view-images/populate02.png#lightbox)

Если развернуть узел в режиме структуры, он будет выглядеть следующим образом:

[![](outline-view-images/populate03.png "The expanded view")](outline-view-images/populate03.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Сортировка по столбцу

Давайте разрешите пользователю сортировать данные в структуре, щелкая заголовок столбца. Сначала дважды щелкните файл `Main.storyboard`, чтобы открыть его для редактирования в Interface Builder. Выберите столбец `Product`, введите `Title` для **ключа сортировки**, `compare:` для **селектора** и выберите `Ascending` для **заказа**:

[![](outline-view-images/sort01.png "Setting the sort key order")](outline-view-images/sort01.png#lightbox)

Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Теперь изменим файл `ProductOutlineDataSource.cs` и добавим следующие методы:

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
    }
}

public override void SortDescriptorsChanged (NSOutlineView outlineView, NSSortDescriptor[] oldDescriptors)
{
    // Sort the data
    Sort (oldDescriptors [0].Key, oldDescriptors [0].Ascending);
    outlineView.ReloadData ();
}
```

Метод `Sort` позволяет нам сортировать данные в источнике данных на основе заданного поля `Product` класса в порядке возрастания или убывания. Переопределенный метод `SortDescriptorsChanged` будет вызываться каждый раз при нажатии кнопки мыши на заголовке столбца. Ему будет передано значение **ключа** , установленное в Interface Builder, и порядок сортировки для этого столбца.

Если запустить приложение и щелкнуть в заголовках столбцов, строки будут отсортированы по этому столбцу:

[![](outline-view-images/sort02.png "Example of sorted output")](outline-view-images/sort02.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Выбор строк

Если вы хотите разрешить пользователю выбирать одну строку, дважды щелкните файл `Main.storyboard`, чтобы открыть его для редактирования в Interface Builder. Выберите режим структуры в **иерархии интерфейсов** и снимите флажок **несколько** в **инспекторе атрибутов**:

[![](outline-view-images/select01.png "The Attribute Inspector")](outline-view-images/select01.png#lightbox)

Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Затем измените файл `ProductOutlineDelegate.cs` и добавьте следующий метод:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Это позволит пользователю выбрать одну строку в режиме структуры. Возвращает `false` для `ShouldSelectItem` любого элемента, который пользователь не должен иметь возможность выбирать или `false` для каждого элемента, если вы не хотите, чтобы пользователь мог выбрать какие бы то ни было элементы.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Выбор нескольких строк

Если вы хотите разрешить пользователю выбирать несколько строк, дважды щелкните файл `Main.storyboard`, чтобы открыть его для редактирования в Interface Builder. Выберите режим структуры в **иерархии интерфейс** и установите флажок **несколько** в **инспекторе атрибутов**:

[![](outline-view-images/select02.png "The Attribute Inspector")](outline-view-images/select02.png#lightbox)

Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Затем измените файл `ProductOutlineDelegate.cs` и добавьте следующий метод:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Это позволит пользователю выбрать одну строку в режиме структуры. Возвращает `false` для `ShouldSelectRow` любого элемента, который пользователь не должен иметь возможность выбирать или `false` для каждого элемента, если вы не хотите, чтобы пользователь мог выбрать какие бы то ни было элементы.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Введите, чтобы выбрать строку

Если вы хотите разрешить пользователю вводить символ с выбранным режимом структуры и выбрать первую строку с этим символом, дважды щелкните файл `Main.storyboard`, чтобы открыть его для редактирования в Interface Builder. Выберите режим структуры в **иерархии интерфейс** и установите флажок **тип SELECT** в **инспекторе атрибутов**:

[![](outline-view-images/type01.png "Editing the row type")](outline-view-images/type01.png#lightbox)

Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Теперь изменим файл `ProductOutlineDelegate.cs` и добавим следующий метод:

```csharp
public override NSObject GetNextTypeSelectMatch (NSOutlineView outlineView, NSObject startItem, NSObject endItem, string searchString)
{
    foreach(Product product in DataSource.Products) {
        if (product.Title.Contains (searchString)) {
            return product;
        }
    }

    // Not found
    return null;
}
```

Метод `GetNextTypeSelectMatch` принимает заданную `searchString` и возвращает элемент первой `Product`, в которой эта строка находится в `Title`.

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Изменение порядка столбцов

Если вы хотите разрешить пользователю перетаскивать столбцы переупорядочения в режиме структуры, дважды щелкните файл `Main.storyboard`, чтобы открыть его для редактирования в Interface Builder. Выберите режим структуры в **иерархии интерфейс** и установите флажок **изменить порядок** в **инспекторе атрибутов**:

[![](outline-view-images/reorder01.png "The Attribute Inspector")](outline-view-images/reorder01.png#lightbox)

Если присвоить значение свойству **автосохранения** и проверить поле **сведения о столбце** , все изменения, внесенные в макет таблицы, будут автоматически сохранены для нас и восстановлены при следующем запуске приложения.

Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Теперь изменим файл `ProductOutlineDelegate.cs` и добавим следующий метод:

```csharp
public override bool ShouldReorder (NSOutlineView outlineView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

Метод `ShouldReorder` должен возвращать `true` для любого столбца, который требуется перетаскивать в `newColumnIndex`, иначе возвращает `false`.

При запуске приложения можно перетаскивать заголовки столбцов для изменения порядка наших столбцов:

[![](outline-view-images/reorder02.png "Example of reordering columns")](outline-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Редактирование ячеек

Если вы хотите разрешить пользователю изменять значения для определенной ячейки, измените файл `ProductOutlineDelegate.cs` и измените метод `GetViewForItem` следующим образом:

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTextField view = (NSTextField)outlineView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTextField ();
        view.Identifier = tableColumn.Title;
        view.BackgroundColor = NSColor.Clear;
        view.Bordered = false;
        view.Selectable = false;
        view.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.StringValue;
            break;
        case "Details":
            prod.Description = view.StringValue;
            break;
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.StringValue = product.Title;
        break;
    case "Details":
        view.StringValue = product.Description;
        break;
    }

    return view;
}
```

Теперь, если приложение запускается, пользователь может изменять ячейки в представлении таблицы:

[![](outline-view-images/editing01.png "An example of editing cells")](outline-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Outline_Views" />

## <a name="using-images-in-outline-views"></a>Использование изображений в представлениях структуры

Чтобы включить изображение как часть ячейки в `NSOutlineView`, необходимо изменить способ, которым данные будут возвращены методом `NSTableViewDelegate's` `GetView` в режиме структуры, чтобы использовать `NSTableCellView` вместо типичного `NSTextField`. Пример:

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)outlineView.MakeView (tableColumn.Title, this);
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
        view.TextField.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.TextField.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.TextField.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.TextField.StringValue;
            break;
        case "Details":
            prod.Description = view.TextField.StringValue;
            break;
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed (product.IsProductGroup ? "tags.png" : "tag.png");
        view.TextField.StringValue = product.Title;
        break;
    case "Details":
        view.TextField.StringValue = product.Description;
        break;
    }

    return view;
}
```

Дополнительные сведения см. в разделе [Использование образов с представлениями структуры](~/mac/app-fundamentals/image.md) в документации по [работе с изображением](~/mac/app-fundamentals/image.md) .

<a name="Data_Binding_Outline_Views" />

## <a name="data-binding-outline-views"></a>Представления структуры привязки данных

Благодаря использованию кодирования и методов привязки данных в приложении Xamarin. Mac можно значительно сократить объем кода, который необходимо написать и поддерживать, чтобы заполнить элементы пользовательского интерфейса и работать с ними. Кроме того, у вас есть преимущество в дальнейшем разделять резервные данные (_модель данных_) от интерфейса пользователя переднего плана (_модель-представление-контроллер_), что упрощает обслуживание, более гибкое проектирование приложений.

Кодирование "ключ-значение" (КВК) — это механизм непрямого доступа к свойствам объекта с помощью клавиш (специально отформатированных строк) для обнаружения свойств вместо доступа к ним через переменные экземпляра или методы доступа (`get/set`). Реализуя методы доступа, совместимые с кодированием значений, в приложении Xamarin. Mac, вы получаете доступ к другим macOS функциям, таким как контроль значений ключа (кво), привязка данных, основные данные, привязки Cocoa и сценарии.

Дополнительные сведения см. в разделе [Привязка данных в режиме структуры](~/mac/app-fundamentals/databinding.md#Outline_View_Data_Binding) [привязки данных и документации по кодированию "ключ-значение](~/mac/app-fundamentals/databinding.md) ".

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье подробно рассматривается работа с представлениями структуры в приложении Xamarin. Mac. Мы видели различные типы и использование представлений структуры, как создавать и поддерживать представления структуры в Interface Builder Xcode и как работать с представлениями структуры в C# коде.

## <a name="related-links"></a>Связанные ссылки

- [Макаутлинес (пример)](https://docs.microsoft.com/samples/xamarin/mac-samples/macoutlines)
- [MacImages (пример)](https://docs.microsoft.com/samples/xamarin/mac-samples/macimages)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Представления таблиц](~/mac/user-interface/table-view.md)
- [Списки источников](~/mac/user-interface/source-list.md)
- [Привязка данных и кодирование типа "ключ — значение"](~/mac/app-fundamentals/databinding.md)
- [Рекомендации по работе с человеческим интерфейсом OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Общие сведения о представлениях структуры](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [нсаутлиневиев](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [нсаутлиневиевдатасаурце](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [нсаутлиневиевделегате](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
