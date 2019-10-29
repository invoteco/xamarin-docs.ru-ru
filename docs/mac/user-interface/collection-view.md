---
title: Представления коллекций в Xamarin. Mac
description: В этой статье описывается работа с представлениями коллекций в приложении Xamarin. Mac. В нем рассматривается создание и обслуживание представлений коллекций в Xcode и Interface Builder и работа с ними программными средствами.
ms.prod: xamarin
ms.assetid: 6EE32256-5948-4AE4-8133-6D0B3F4173E8
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 05/24/2017
ms.openlocfilehash: 565441762bc7d9dcf7f73b42a34e3feb0bff86f1
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73025822"
---
# <a name="collection-views-in-xamarinmac"></a>Представления коллекций в Xamarin. Mac

_В этой статье описывается работа с представлениями коллекций в приложении Xamarin. Mac. В нем рассматривается создание и обслуживание представлений коллекций в Xcode и Interface Builder и работа с ними программными средствами._

При работе с C# и .NET в приложении Xamarin. Mac разработчик имеет доступ к тому же элементу управления "представление коллекции AppKit", что и разработчик, работающий в *цели-C* и *Xcode* . Поскольку Xamarin. Mac интегрируется напрямую с Xcode, разработчик использует _Interface Builder_ Xcode для создания и обслуживания представлений коллекций.

`NSCollectionView` отображает сетку вложенных представлений, упорядоченных с помощью `NSCollectionViewLayout`. Каждое Подпредставление в сетке представлено `NSCollectionViewItem`, которое управляет загрузкой содержимого представления из файла `.xib`.

[![выполнения примера приложения](collection-view-images/intro01.png)](collection-view-images/intro01.png#lightbox)

В этой статье рассматриваются основы работы с представлениями коллекций в приложении Xamarin. Mac. Мы настоятельно рекомендуем сначала ознакомиться со статьей [Hello, Mac](~/mac/get-started/hello-mac.md) , в частности [введением в разделы Xcode и Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) [, а](~/mac/get-started/hello-mac.md#outlets-and-actions) также о том, как они посвящены основным концепциям и методикам, используемым в этой статье.

Возможно, вы захотите ознакомиться с разделом [ C# классы и методы для цели-C](~/mac/internals/how-it-works.md) в документе о внутренних компонентах [Xamarin. Mac](~/mac/internals/how-it-works.md) , а также объясняются`Register`и`Export`команды, используемые для подключения C# классов к цели-C. объекты и элементы пользовательского интерфейса.

<a name="About_Collection_Views"/>

## <a name="about-collection-views"></a>О представлениях коллекций

Основной целью представления коллекции (`NSCollectionView`) является визуальное упорядочение группы объектов в упорядоченном виде с помощью макета представления коллекции (`NSCollectionViewLayout`), при котором каждый отдельный объект (`NSCollectionViewItem`) получает собственное представление в более крупной коллекции. Представления коллекций работают с помощью привязки данных и приемов кодирования значений ключа, поэтому перед продолжением работы с этой статьей следует ознакомиться с документацией по [кодированию данных и кодированием значений ключа](~/mac/app-fundamentals/databinding.md) .

В представлении коллекции нет стандартного встроенного элемента представления коллекции (например, структура или представление таблицы), поэтому разработчик отвечает за проектирование и реализацию _представления прототипа_ с помощью других элементов управления AppKit, таких как поля изображений, текстовые поля, метки, 123. Это представление прототипа будет использоваться для отображения каждого элемента, управляемого представлением коллекции, и работы с ним, который хранится в `.xib` файле.

Поскольку разработчик отвечает за внешний вид элемента представления коллекции, представление коллекции не имеет встроенной поддержки выделения выбранного элемента в сетке. Реализация этой функции будет рассмотрена в этой статье.

<a name="Defining_your_Data_Model"/>

## <a name="defining-the-data-model"></a>Определение модели данных

Перед привязкой данных к представлению коллекции в Interface Builder в приложении Xamarin. Mac необходимо определить/Кэй-валуе (кво), который будет использоваться в качестве _модели данных_ для привязки. Модель данных предоставляет все данные, которые будут отображаться в коллекции, и получает любые изменения данных, которые пользователь делает в пользовательском интерфейсе во время выполнения приложения.

Возьмем пример приложения, управляющего группой сотрудников, для определения модели данных можно использовать следующий класс:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        #region Private Variables
        private string _name = "";
        private string _occupation = "";
        private bool _isManager = false;
        private NSMutableArray _people = new NSMutableArray();
        #endregion

        #region Computed Properties
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }

        [Export("Occupation")]
        public string Occupation {
            get { return _occupation; }
            set {
                WillChangeValue ("Occupation");
                _occupation = value;
                DidChangeValue ("Occupation");
            }
        }

        [Export("isManager")]
        public bool isManager {
            get { return _isManager; }
            set {
                WillChangeValue ("isManager");
                WillChangeValue ("Icon");
                _isManager = value;
                DidChangeValue ("isManager");
                DidChangeValue ("Icon");
            }
        }

        [Export("isEmployee")]
        public bool isEmployee {
            get { return (NumberOfEmployees == 0); }
        }

        [Export("Icon")]
        public NSImage Icon
        {
            get
            {
                if (isManager)
                {
                    return NSImage.ImageNamed("IconGroup");
                }
                else
                {
                    return NSImage.ImageNamed("IconUser");
                }
            }
        }

        [Export("personModelArray")]
        public NSArray People {
            get { return _people; }
        }

        [Export("NumberOfEmployees")]
        public nint NumberOfEmployees {
            get { return (nint)_people.Count; }
        }
        #endregion

        #region Constructors
        public PersonModel ()
        {
        }

        public PersonModel (string name, string occupation)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (string name, string occupation, bool manager)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
            this.isManager = manager;
        }
        #endregion

        #region Array Controller Methods
        [Export("addObject:")]
        public void AddPerson(PersonModel person) {
            WillChangeValue ("personModelArray");
            isManager = true;
            _people.Add (person);
            DidChangeValue ("personModelArray");
        }

        [Export("insertObject:inPersonModelArrayAtIndex:")]
        public void InsertPerson(PersonModel person, nint index) {
            WillChangeValue ("personModelArray");
            _people.Insert (person, index);
            DidChangeValue ("personModelArray");
        }

        [Export("removeObjectFromPersonModelArrayAtIndex:")]
        public void RemovePerson(nint index) {
            WillChangeValue ("personModelArray");
            _people.RemoveObject (index);
            DidChangeValue ("personModelArray");
        }

        [Export("setPersonModelArray:")]
        public void SetPeople(NSMutableArray array) {
            WillChangeValue ("personModelArray");
            _people = array;
            DidChangeValue ("personModelArray");
        }
        #endregion
    }
}
```

В оставшейся части этой статьи будет использоваться модель данных `PersonModel`.

<a name="Working_with_a_Collection_View"/>

## <a name="working-with-a-collection-view"></a>Работа с представлением коллекции

Привязка данных к представлению коллекции очень похожа на привязку к табличному представлению, так как `NSCollectionViewDataSource` используется для предоставления данных коллекции. Так как представление коллекции не имеет предопределенного формата отображения, требуется больше работы, чтобы обеспечить взаимодействие с пользователем и отслеживание выбора пользователей.

<a name="Creating-the-Cell-Prototype"/>

### <a name="creating-the-cell-prototype"></a>Создание прототипа ячейки

Так как представление коллекции не включает прототип ячейки по умолчанию, разработчику необходимо добавить один или несколько файлов `.xib` в приложение Xamarin. Mac, чтобы определить макет и содержимое отдельных ячеек.

Выполните следующие действия:

1. В **Обозреватель решений**щелкните правой кнопкой мыши имя проекта и выберите **Добавить** > **новый файл...**
2. Выберите **Mac** > **представление контроллер**, присвойте ему имя (например, `EmployeeItem` в этом примере) и нажмите кнопку **создать** , чтобы создать: 

    ![Добавление нового контроллера представления](collection-view-images/proto01.png)

    Это приведет к добавлению в решение проекта `EmployeeItem.cs`, `EmployeeItemController.cs` и `EmployeeItemController.xib` файла.
3. Дважды щелкните файл `EmployeeItemController.xib`, чтобы открыть его для редактирования в Interface Builder Xcode.
4. Добавьте `NSBox`, `NSImageView` и два элемента управления `NSLabel` в представление и разметьте их следующим образом:

    ![Разработка макета прототипа ячейки](collection-view-images/proto02.png)
5. Откройте **Редактор помощника** и создайте **выход** для `NSBox`, чтобы его можно было использовать для указания состояния выбора ячейки:

    ![Предоставление NSBox в розетке](collection-view-images/proto03.png)
6. Вернитесь в **стандартный редактор** и выберите представление изображения.
7. В **инспекторе привязки**выберите **привязать к** > **владельцу файла** и введите **путь к ключу модели** `self.Person.Icon`:

    ![Привязка значка](collection-view-images/proto04.png)
8. Выберите первую метку и в **инспекторе привязки**выберите **привязать к** > **владельцу файла** и введите **путь к ключу модели** `self.Person.Name`:

    ![Привязка имени](collection-view-images/proto05.png)
9. Выберите вторую метку и в **инспекторе привязки**выберите **привязать к** > **владельцу файла** и введите **путь к ключу модели** `self.Person.Occupation`:

    ![Привязка занятий](collection-view-images/proto06.png)
10. Сохраните изменения в файле `.xib` и вернитесь в Visual Studio, чтобы синхронизировать изменения.

Измените файл `EmployeeItemController.cs` и сделайте его следующим:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Foundation;
using AppKit;

namespace MacCollectionNew
{
    /// <summary>
    /// The Employee item controller handles the display of the individual items that will
    /// be displayed in the collection view as defined in the associated .XIB file.
    /// </summary>
    public partial class EmployeeItemController : NSCollectionViewItem
    {
        #region Private Variables
        /// <summary>
        /// The person that will be displayed.
        /// </summary>
        private PersonModel _person;
        #endregion

        #region Computed Properties
        // strongly typed view accessor
        public new EmployeeItem View
        {
            get
            {
                return (EmployeeItem)base.View;
            }
        }

        /// <summary>
        /// Gets or sets the person.
        /// </summary>
        /// <value>The person that this item belongs to.</value>
        [Export("Person")]
        public PersonModel Person
        {
            get { return _person; }
            set
            {
                WillChangeValue("Person");
                _person = value;
                DidChangeValue("Person");
            }
        }

        /// <summary>
        /// Gets or sets the color of the background for the item.
        /// </summary>
        /// <value>The color of the background.</value>
        public NSColor BackgroundColor {
            get { return Background.FillColor; }
            set { Background.FillColor = value; }
        }

        /// <summary>
        /// Gets or sets a value indicating whether this <see cref="T:MacCollectionNew.EmployeeItemController"/> is selected.
        /// </summary>
        /// <value><c>true</c> if selected; otherwise, <c>false</c>.</value>
        /// <remarks>This also changes the background color based on the selected state
        /// of the item.</remarks>
        public override bool Selected
        {
            get
            {
                return base.Selected;
            }
            set
            {
                base.Selected = value;

                // Set background color based on the selection state
                if (value) {
                    BackgroundColor = NSColor.DarkGray;
                } else {
                    BackgroundColor = NSColor.LightGray;
                }
            }
        }
        #endregion

        #region Constructors
        // Called when created from unmanaged code
        public EmployeeItemController(IntPtr handle) : base(handle)
        {
            Initialize();
        }

        // Called when created directly from a XIB file
        [Export("initWithCoder:")]
        public EmployeeItemController(NSCoder coder) : base(coder)
        {
            Initialize();
        }

        // Call to load from the XIB/NIB file
        public EmployeeItemController() : base("EmployeeItem", NSBundle.MainBundle)
        {
            Initialize();
        }

        // Added to support loading from XIB/NIB
        public EmployeeItemController(string nibName, NSBundle nibBundle) : base(nibName, nibBundle) {

            Initialize();
        }

        // Shared initialization code
        void Initialize()
        {
        }
        #endregion
    }
}
```

Посмотрев на этот код, класс наследует от `NSCollectionViewItem`, так что он может служить прототипом для ячейки представления коллекции. Свойство `Person` предоставляет класс, который использовался для привязки данных к представлению изображения и меткам в Xcode. Это экземпляр `PersonModel`, созданного ранее.

Свойство `BackgroundColor` — это ярлык `FillColor` элемента управления `NSBox`, который будет использоваться для отображения состояния выбора ячейки. При переопределении свойства `Selected` `NSCollectionViewItem`следующий код задает или очищает это состояние выбора:

```csharp
public override bool Selected
{
    get
    {
        return base.Selected;
    }
    set
    {
        base.Selected = value;

        // Set background color based on the selection state
        if (value) {
            BackgroundColor = NSColor.DarkGray;
        } else {
            BackgroundColor = NSColor.LightGray;
        }
    }
}
```

<a name="Creating-the-Collection-View-Data-Source"/>

### <a name="creating-the-collection-view-data-source"></a>Создание источника данных представления коллекции

Источник данных представления коллекции (`NSCollectionViewDataSource`) предоставляет все данные для представления коллекции и создает и заполняет ячейку представления коллекции (используя прототип `.xib`) в соответствии с требованиями для каждого элемента в коллекции.

Добавьте новый класс в проект, вызовите его `CollectionViewDataSource` и сделайте так, чтобы он выглядел следующим образом:

```csharp
using System;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacCollectionNew
{
    /// <summary>
    /// Collection view data source provides the data for the collection view.
    /// </summary>
    public class CollectionViewDataSource : NSCollectionViewDataSource
    {
        #region Computed Properties
        /// <summary>
        /// Gets or sets the parent collection view.
        /// </summary>
        /// <value>The parent collection view.</value>
        public NSCollectionView ParentCollectionView { get; set; }

        /// <summary>
        /// Gets or sets the data that will be displayed in the collection.
        /// </summary>
        /// <value>A collection of PersonModel objects.</value>
        public List<PersonModel> Data { get; set; } = new List<PersonModel>();
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.CollectionViewDataSource"/> class.
        /// </summary>
        /// <param name="parent">The parent collection that this datasource will provide data for.</param>
        public CollectionViewDataSource(NSCollectionView parent)
        {
            // Initialize
            ParentCollectionView = parent;

            // Attach to collection view
            parent.DataSource = this;

        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Gets the number of sections.
        /// </summary>
        /// <returns>The number of sections.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        public override nint GetNumberOfSections(NSCollectionView collectionView)
        {
            // There is only one section in this view
            return 1;
        }

        /// <summary>
        /// Gets the number of items in the given section.
        /// </summary>
        /// <returns>The number of items.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="section">The Section number to count items for.</param>
        public override nint GetNumberofItems(NSCollectionView collectionView, nint section)
        {
            // Return the number of items
            return Data.Count;
        }

        /// <summary>
        /// Gets the item for the give section and item index.
        /// </summary>
        /// <returns>The item.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPath">Index path specifying the section and index.</param>
        public override NSCollectionViewItem GetItem(NSCollectionView collectionView, NSIndexPath indexPath)
        {
            var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
            item.Person = Data[(int)indexPath.Item];

            return item;
        }
        #endregion
    }
}
```

Просматривая этот код подробно, класс наследует от `NSCollectionViewDataSource` и предоставляет список экземпляров `PersonModel` через свойство `Data`.

Так как эта коллекция содержит только один раздел, код переопределяет метод `GetNumberOfSections` и всегда возвращает `1`. Кроме того, метод `GetNumberofItems` переопределен в нем, возвращает количество элементов в списке свойств `Data`.

Метод `GetItem` вызывается каждый раз, когда требуется новая ячейка, и выглядит следующим образом:

```csharp
public override NSCollectionViewItem GetItem(NSCollectionView collectionView, NSIndexPath indexPath)
{
    var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
    item.Person = Data[(int)indexPath.Item];

    return item;
}
```

Метод `MakeItem` представления коллекции вызывается для создания или возвращения повторно используемого экземпляра `EmployeeItemController`, а его свойство `Person` имеет значение Item, отображаемое в запрошенной ячейке. 

`EmployeeItemController` должны быть зарегистрированы в контроллере представления коллекции заранее с помощью следующего кода:

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
``` 

**Идентификатор** (`EmployeeCell`), используемый в вызове `MakeItem`, _должен_ соответствовать имени контроллера представления, зарегистрированного в представлении коллекции. Этот шаг подробно рассматривается ниже.

<a name="Handling-Item-Selection"/>

### <a name="handling-item-selection"></a>Обработка выбора элементов

Чтобы обрабатывать выделение и отменить выборку элементов в коллекции, потребуется `NSCollectionViewDelegate`. Поскольку в этом примере используется встроенный тип макета `NSCollectionViewFlowLayout`, потребуется `NSCollectionViewDelegateFlowLayout` конкретная версия этого делегата.

Добавьте в проект новый класс, вызовите его `CollectionViewDelegate` и сделайте так, чтобы он выглядел следующим образом:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacCollectionNew
{
    /// <summary>
    /// Collection view delegate handles user interaction with the elements of the 
    /// collection view for the Flow-Based layout type.
    /// </summary>
    public class CollectionViewDelegate : NSCollectionViewDelegateFlowLayout
    {
        #region Computed Properties
        /// <summary>
        /// Gets or sets the parent view controller.
        /// </summary>
        /// <value>The parent view controller.</value>
        public ViewController ParentViewController { get; set; }
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.CollectionViewDelegate"/> class.
        /// </summary>
        /// <param name="parentViewController">Parent view controller.</param>
        public CollectionViewDelegate(ViewController parentViewController)
        {
            // Initialize
            ParentViewController = parentViewController;
        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Handles one or more items being selected.
        /// </summary>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPaths">The Index paths of the items being selected.</param>
        public override void ItemsSelected(NSCollectionView collectionView, NSSet indexPaths)
        {
            // Dereference path
            var paths = indexPaths.ToArray<NSIndexPath>();
            var index = (int)paths[0].Item;

            // Save the selected item
            ParentViewController.PersonSelected = ParentViewController.Datasource.Data[index];

        }

        /// <summary>
        /// Handles one or more items being deselected.
        /// </summary>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPaths">The Index paths of the items being deselected.</param>
        public override void ItemsDeselected(NSCollectionView collectionView, NSSet indexPaths)
        {
            // Dereference path
            var paths = indexPaths.ToArray<NSIndexPath>();
            var index = paths[0].Item;

            // Clear selection
            ParentViewController.PersonSelected = null;
        }
        #endregion
    }
}
``` 

Методы `ItemsSelected` и `ItemsDeselected` переопределяются и используются для установки или очистки свойства `PersonSelected` контроллера представления, обрабатывающего представление коллекции, когда пользователь выбирает или отменяет выбор элемента. Это будет показано подробно.

<a name="Creating-the-Collection-View-in-Interface-Builder"/>

### <a name="creating-the-collection-view-in-interface-builder"></a>Создание представления коллекции в Interface Builder

При наличии всех необходимых вспомогательных элементов можно изменить основную раскадровку и добавить в нее представление коллекции.

Выполните следующие действия:

1. Дважды щелкните файл `Main.Storyboard` в **Обозреватель решений** , чтобы открыть его для редактирования в Interface Builder Xcode.
2. Перетащите представление коллекции в главное представление и измените его размер, чтобы заполнить представление:

    ![Добавление в макет представления коллекции](collection-view-images/collection01.png)
3. Выбрав представление коллекция, используйте Редактор ограничений, чтобы закрепить его в представлении при изменении размера.

    ![Добавление ограничений](collection-view-images/collection02.png)
4. Убедитесь, что в **область конструктора** выбрано представление коллекции (а не представление с **рамкой прокрутки** или **представление клипа** , содержащее его), переключитесь в **Редактор помощника** и создайте **выход** для представления коллекции:

    ![Добавление ограничений](collection-view-images/collection03.png)
5. Сохраните изменения и вернитесь в Visual Studio для синхронизации.

<a name="Bringing-it-all-Together"/>

## <a name="bringing-it-all-together"></a>Совместная работа

Все вспомогательные части теперь помещены в класс, который выступает в качестве модели данных (`PersonModel`), для предоставления данных был добавлен `NSCollectionViewDataSource`, `NSCollectionViewDelegateFlowLayout` был создан для обработки выбора элементов, а `NSCollectionView` был добавлен в основную раскадровку и предоставляется в качестве розетки (`EmployeeCollection`).

Последним шагом является изменение контроллера представления, содержащего представление коллекции, и объединение всех элементов для заполнения коллекции и выбора элементов в обработке.

Измените файл `ViewController.cs` и сделайте его следующим:

```csharp
using System;
using AppKit;
using Foundation;
using CoreGraphics;

namespace MacCollectionNew
{
    /// <summary>
    /// The View controller controls the main view that houses the Collection View.
    /// </summary>
    public partial class ViewController : NSViewController
    {
        #region Private Variables
        private PersonModel _personSelected;
        private bool shouldEdit = true;
        #endregion

        #region Computed Properties
        /// <summary>
        /// Gets or sets the datasource that provides the data to display in the 
        /// Collection View.
        /// </summary>
        /// <value>The datasource.</value>
        public CollectionViewDataSource Datasource { get; set; }

        /// <summary>
        /// Gets or sets the person currently selected in the collection view.
        /// </summary>
        /// <value>The person selected or <c>null</c> if no person is selected.</value>
        [Export("PersonSelected")]
        public PersonModel PersonSelected
        {
            get { return _personSelected; }
            set
            {
                WillChangeValue("PersonSelected");
                _personSelected = value;
                DidChangeValue("PersonSelected");
                RaiseSelectionChanged();
            }
        }
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.ViewController"/> class.
        /// </summary>
        /// <param name="handle">Handle.</param>
        public ViewController(IntPtr handle) : base(handle)
        {
        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Called after the view has finished loading from the Storyboard to allow it to
        /// be configured before displaying to the user.
        /// </summary>
        public override void ViewDidLoad()
        {
            base.ViewDidLoad();

            // Initialize Collection View
            ConfigureCollectionView();
            PopulateWithData();
        }
        #endregion

        #region Private Methods
        /// <summary>
        /// Configures the collection view.
        /// </summary>
        private void ConfigureCollectionView()
        {
            EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");

            // Create a flow layout
            var flowLayout = new NSCollectionViewFlowLayout()
            {
                ItemSize = new CGSize(150, 150),
                SectionInset = new NSEdgeInsets(10, 10, 10, 20),
                MinimumInteritemSpacing = 10,
                MinimumLineSpacing = 10
            };
            EmployeeCollection.WantsLayer = true;

            // Setup collection view
            EmployeeCollection.CollectionViewLayout = flowLayout;
            EmployeeCollection.Delegate = new CollectionViewDelegate(this);

        }

        /// <summary>
        /// Populates the Datasource with data and attaches it to the collection view.
        /// </summary>
        private void PopulateWithData()
        {
            // Make datasource
            Datasource = new CollectionViewDataSource(EmployeeCollection);

            // Build list of employees
            Datasource.Data.Add(new PersonModel("Craig Dunn", "Documentation Manager", true));
            Datasource.Data.Add(new PersonModel("Amy Burns", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Joel Martinez", "Web & Infrastructure"));
            Datasource.Data.Add(new PersonModel("Kevin Mullins", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Mark McLemore", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Tom Opgenorth", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Larry O'Brien", "API Docs Manager", true));
            Datasource.Data.Add(new PersonModel("Mike Norman", "API Documentor"));

            // Populate collection view
            EmployeeCollection.ReloadData();
        }
        #endregion

        #region Events
        /// <summary>
        /// Selection changed delegate.
        /// </summary>
        public delegate void SelectionChangedDelegate();

        /// <summary>
        /// Occurs when selection changed.
        /// </summary>
        public event SelectionChangedDelegate SelectionChanged;

        /// <summary>
        /// Raises the selection changed event.
        /// </summary>
        internal void RaiseSelectionChanged() {
            // Inform caller
            if (this.SelectionChanged != null) SelectionChanged();
        }
        #endregion
    }
}
```

Подробно взглянув на этот код, `Datasource` свойство определяется для хранения экземпляра `CollectionViewDataSource`, который предоставит данные для представления коллекции. Свойство `PersonSelected` определяется для хранения `PersonModel`, представляющего текущий выбранный элемент в представлении коллекции. Это свойство также вызывает событие `SelectionChanged` при изменении выбора.

Класс `ConfigureCollectionView` используется для регистрации контроллера представления, который выступает в качестве прототипа ячейки с представлением коллекции, используя следующую строку:

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
```

Обратите внимание, что **идентификатор** (`EmployeeCell`), используемый для регистрации прототипа, совпадает с именем, вызываемым в методе `GetItem` `CollectionViewDataSource`, определенного выше:

```csharp
var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
...
```

Кроме того, тип контроллера представления **должен** совпадать с именем файла `.xib`, который **точно**определяет прототип. В случае этого примера `EmployeeItemController` и `EmployeeItemController.xib`.

Фактический макет элементов в представлении коллекции управляется классом макета представления коллекции и может быть изменен динамически во время выполнения путем назначения нового экземпляра свойству `CollectionViewLayout`. Изменение этого свойства обновляет внешний вид коллекции без анимации изменения.

Компания Apple поставляется с представлением коллекции двух встроенных типов макетов, которые будут использовать наиболее типичные варианты использования: `NSCollectionViewFlowLayout` и `NSCollectionViewGridLayout`. Если разработчику требуется пользовательский формат, например размещение элементов в окружности, они могут создать пользовательский экземпляр `NSCollectionViewLayout` и переопределить необходимые методы для достижения желаемого результата.

В этом примере используется макет потока по умолчанию, поэтому он создает экземпляр класса `NSCollectionViewFlowLayout` и настраивает его следующим образом:

```csharp
var flowLayout = new NSCollectionViewFlowLayout()
{
    ItemSize = new CGSize(150, 150),
    SectionInset = new NSEdgeInsets(10, 10, 10, 20),
    MinimumInteritemSpacing = 10,
    MinimumLineSpacing = 10
};
```

Свойство `ItemSize` определяет размер каждой отдельной ячейки в коллекции. Свойство `SectionInset` определяет инсетс от края коллекции, в которой будут размещены ячейки. `MinimumInteritemSpacing` определяет минимальный интервал между элементами и `MinimumLineSpacing` определяет минимальный интервал между строками в коллекции.

Макет назначается представлению коллекции, а экземпляр `CollectionViewDelegate` прикрепляется к выбранному элементу управления.

```csharp
// Setup collection view
EmployeeCollection.CollectionViewLayout = flowLayout;
EmployeeCollection.Delegate = new CollectionViewDelegate(this);
```

Метод `PopulateWithData` создает новый экземпляр `CollectionViewDataSource`, заполняет его данными, присоединяет его к представлению коллекции и вызывает метод `ReloadData` для отображения элементов:

```csharp
private void PopulateWithData()
{
    // Make datasource
    Datasource = new CollectionViewDataSource(EmployeeCollection);

    // Build list of employees
    Datasource.Data.Add(new PersonModel("Craig Dunn", "Documentation Manager", true));
    ...

    // Populate collection view
    EmployeeCollection.ReloadData();
}
```

Метод `ViewDidLoad` переопределен и вызывает методы `ConfigureCollectionView` и `PopulateWithData`, чтобы отобразить конечное представление коллекции для пользователя:

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();

    // Initialize Collection View
    ConfigureCollectionView();
    PopulateWithData();
}
```

<a name="Summary"/>

## <a name="summary"></a>Сводка

В этой статье подробно рассматривается работа с представлениями коллекций в приложении Xamarin. Mac. Во-первых, он рассматривал предоставление C# класса для цели-C с помощью кодирования "ключ — значение" (КВК) и контрольного значения "ключ — значение" (кво). Затем показано, как использовать класс, совместимый с кво, и данные, привязку их к представлениям коллекций в Interface Builder Xcode. Наконец, было показано, как взаимодействовать с представлениями коллекций C# в коде.

## <a name="related-links"></a>Связанные ссылки

- [Макколлектионнев (пример)](https://docs.microsoft.com/samples/xamarin/mac-samples/maccollectionnew)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Привязка данных и кодирование типа "ключ — значение"](~/mac/app-fundamentals/databinding.md)
- [нсколлектионвиев](https://developer.apple.com/reference/appkit/nscollectionview)
- [Рекомендации по работе с человеческим интерфейсом OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
