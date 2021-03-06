---
title: Стандартные элементы управления в Xamarin. Mac
description: В этой статье рассматривается работа со стандартными элементами управления AppKit, такими как кнопки, метки, текстовые поля, флажки и сегментированные элементы управления в приложении Xamarin. Mac. Он описывает добавление их в интерфейс с Interface Builder и взаимодействие с ними в коде.
ms.prod: xamarin
ms.assetid: d2593883-d255-431f-9781-75f04d8cecea
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: fe887d837930ebc75fed0fb7c163a3f30ad83af9
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73008370"
---
# <a name="standard-controls-in-xamarinmac"></a>Стандартные элементы управления в Xamarin. Mac

_В этой статье рассматривается работа со стандартными элементами управления AppKit, такими как кнопки, метки, текстовые поля, флажки и сегментированные элементы управления в приложении Xamarin. Mac. Он описывает добавление их в интерфейс с Interface Builder и взаимодействие с ними в коде._

При работе с C# и .NET в приложении Xamarin. Mac у вас есть доступ к тем же элементам управления AppKit, что и разработчик, работающий на *уровне цели-C* и *Xcode* . Так как Xamarin. Mac интегрируется напрямую с Xcode, можно использовать _Interface Builder_ Xcode для создания и обслуживания элементов управления AppKit (или при необходимости создавать их непосредственно в C# коде).

Элементы управления AppKit — это элементы интерфейса пользователя, которые используются для создания пользовательского интерфейса приложения Xamarin. Mac. Они состоят из таких элементов, как кнопки, метки, текстовые поля, флажки и сегментированные элементы управления и вызывают мгновенные действия или видимые результаты, когда пользователь манипулирует ими.

[![](standard-controls-images/intro01.png "The example app main screen")](standard-controls-images/intro01.png#lightbox)

В этой статье рассматриваются основы работы с элементами управления AppKit в приложении Xamarin. Mac. Мы настоятельно рекомендуем сначала ознакомиться со статьей [Hello, Mac](~/mac/get-started/hello-mac.md) , в частности [Знакомство с Xcode и Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) , а также с разделом "возможности [и действия](~/mac/get-started/hello-mac.md#outlets-and-actions) ", так как в нем рассматриваются основные понятия и методы, которые мы будем использовать в Эта статья.

Возможно, вы захотите ознакомиться с разделом [ C# классы и методы для цели-C](~/mac/internals/how-it-works.md) в документе о внутренних компонентах [Xamarin. Mac](~/mac/internals/how-it-works.md) , а также объясняются`Register`и`Export`команды, используемые для подключения C# классов к цели-C. объекты и элементы пользовательского интерфейса.

<a name="Introduction_to_Controls_and_Views" />

## <a name="introduction-to-controls-and-views"></a>Общие сведения об элементах управления и представлениях

macOS (ранее назывался Mac OS X) предоставляет стандартный набор элементов управления пользовательского интерфейса с помощью платформы AppKit. Они состоят из таких элементов, как кнопки, метки, текстовые поля, флажки и сегментированные элементы управления и вызывают мгновенные действия или видимые результаты, когда пользователь манипулирует ими.

Все элементы управления AppKit имеют стандартный встроенный внешний вид, который подходит для большинства применений, некоторые задают альтернативный внешний вид для использования в области оконных кадров или в контексте эффектов « _вибрация_ », например в боковой панели или в центре уведомлений. ».

При работе с элементами управления AppKit в Apple рекомендуется следовать приведенным ниже рекомендациям.

- Старайтесь не смешивать размеры элементов управления в одном представлении.
- Как правило, старайтесь не изменять размер элементов управления по вертикали.
- Используйте системный шрифт и правильный размер текста в элементе управления.
- Использование правильного интервала между элементами управления.

Для получения дополнительных сведений ознакомьтесь с разделом [About Controls and Views (сведения об элементах управления и представления](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) ) в [руководстве по использованию интерфейса пользователя ОС](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)Apple

<a name="Using_Controls_in_a_Window_Frame" />

### <a name="using-controls-in-a-window-frame"></a>Использование элементов управления в рамке окна

Существует подмножество элементов управления AppKit, включающих стиль экрана, который позволяет включать их в область фрейма окна. Пример см. на панели инструментов почтового приложения.

[![](standard-controls-images/mailapp.png "A Mac Window frame")](standard-controls-images/mailapp.png#lightbox)

- **Кнопка с круглой текстурой** — `NSButton` с стилем `NSTexturedRoundedBezelStyle`.
- **Текстурированный Скругленный сегментированный элемент управления** — `NSSegmentedControl` с стилем `NSSegmentStyleTexturedRounded`.
- **Текстурированный Скругленный сегментированный элемент управления** — `NSSegmentedControl` с стилем `NSSegmentStyleSeparated`.
- **Всплывающее меню с круглым текстурированием** — `NSPopUpButton` с стилем `NSTexturedRoundedBezelStyle`.
- **Раскрывающееся меню с круглым текстурированием** — `NSPopUpButton` с стилем `NSTexturedRoundedBezelStyle`.
- **Панель поиска** — `NSSearchField`.

При работе с элементами управления AppKit в рамке Windows Apple рекомендует следовать приведенным ниже рекомендациям.

- Не используйте в тексте окна специальные стили элементов управления рамки окна.
- Не используйте элементы управления или стили тела окна в рамке окна.

Для получения дополнительных сведений ознакомьтесь с разделом [About Controls and Views (сведения об элементах управления и представления](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) ) в [руководстве по использованию интерфейса пользователя ОС](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)Apple

<a name="Creating_a_User_Interface_in_Interface_Builder" />

## <a name="creating-a-user-interface-in-interface-builder"></a>Создание пользовательского интерфейса в Interface Builder

При создании нового приложения Xamarin. Mac Cocoa по умолчанию получается стандартное пустое окно. Эти окна определяются в `.storyboard` файле, который автоматически включается в проект. Чтобы изменить структуру Windows, в **Обозреватель решений**дважды щелкните файл `Main.storyboard`:

[![](standard-controls-images/edit01.png "Selecting the Main Storyboard in the Solution Explorer")](standard-controls-images/edit01.png#lightbox)

Это приведет к открытию структуры окна в Interface Builder Xcode:

[![](standard-controls-images/edit02.png "Editing the storyboard in Xcode")](standard-controls-images/edit02.png#lightbox)

Чтобы создать пользовательский интерфейс, перетащите элементы пользовательского интерфейса (элементы управления AppKit) из **инспектора библиотек** в **Редактор интерфейса** Interface Builder. В приведенном ниже примере элемент управления с **вертикальным разделением имеет вид** лекарства из **инспектора библиотек** и помещен в окно **редактора интерфейса**:

[![](standard-controls-images/edit03.png "Selecting a Split View from the Library")](standard-controls-images/edit03.png#lightbox)

Дополнительные сведения о создании пользовательского интерфейса в Interface Builder см. в статье [Введение в Xcode и Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) документацию.

<a name="Sizing_and_Positioning" />

### <a name="sizing-and-positioning"></a>Изменение размера и положения

После включения элемента управления в пользовательский интерфейс используйте **Редактор ограничений** , чтобы задать его расположение и размер, введя значения вручную и контролируя автоматическое расположение и размер элемента управления при изменении размера родительского окна или представления.

[![](standard-controls-images/edit04.png "Setting the constraints")](standard-controls-images/edit04.png#lightbox)

Чтобы _прикрепить_ элемент управления к заданному расположению (x, y), используйте **красный I-Беамс** вокруг поля **ауторесизинг** . Пример: 

[![](standard-controls-images/edit05.png "Editing a constraint")](standard-controls-images/edit05.png#lightbox)

Указывает, что выбранный элемент управления (в **представлении иерархии** & **редакторе интерфейса**) будет зависнуть в верхнем и правом расположении окна или представления при изменении размера или перемещении. 

Другие элементы элемента управления редактора, такие как высота и ширина:

[![](standard-controls-images/edit06.png "Setting the height")](standard-controls-images/edit06.png#lightbox)

С помощью **редактора выравнивания**можно также управлять выравниванием элементов с ограничениями.

[![](standard-controls-images/edit07.png "The Alignment Editor")](standard-controls-images/edit07.png#lightbox)

> [!IMPORTANT]
> В отличие от iOS, где (0, 0) — верхний левый угол экрана, в macOS (0, 0) — нижний левый угол. Это обусловлено тем, что macOS использует математическую систему координат с числовыми значениями, увеличивающимися в значении вверх и вправо. Это необходимо учитывать при размещении элементов управления AppKit в пользовательском интерфейсе.

<a name="Setting_a_Custom_Class" />

### <a name="setting-a-custom-class"></a>Настройка пользовательского класса

Иногда приходится работать с элементами управления AppKit, которые необходимы для создания подклассов и существующих элементов управления, и для этого нужно создать пользовательскую версию этого класса. Например, определение пользовательской версии списка источников:

```csharp
using System;
using AppKit;
using Foundation;

namespace AppKit
{
    [Register("SourceListView")]
    public class SourceListView : NSOutlineView
    {
        #region Computed Properties
        public SourceListDataSource Data {
            get {return (SourceListDataSource)this.DataSource; }
        }
        #endregion

        #region Constructors
        public SourceListView ()
        {

        }

        public SourceListView (IntPtr handle) : base(handle)
        {

        }

        public SourceListView (NSCoder coder) : base(coder)
        {

        }

        public SourceListView (NSObjectFlag t) : base(t)
        {

        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

        }
        #endregion

        #region Public Methods
        public void Initialize() {

            // Initialize this instance
            this.DataSource = new SourceListDataSource (this);
            this.Delegate = new SourceListDelegate (this);

        }

        public void AddItem(SourceListItem item) {
            if (Data != null) {
                Data.Items.Add (item);
            }
        }
        #endregion

        #region Events
        public delegate void ItemSelectedDelegate(SourceListItem item);
        public event ItemSelectedDelegate ItemSelected;

        internal void RaiseItemSelected(SourceListItem item) {
            // Inform caller
            if (this.ItemSelected != null) {
                this.ItemSelected (item);
            }
        }
        #endregion
    }
}
```

Где инструкция `[Register("SourceListView")]` предоставляет класс `SourceListView` для цели-C, поэтому его можно использовать в Interface Builder. Дополнительные сведения см. в разделе [предоставление C# классов и методов для цели-C](~/mac/internals/how-it-works.md) документа [внутренних компонентов Xamarin. Mac](~/mac/internals/how-it-works.md) . здесь объясняются`Register`и`Export`команды, используемые для подключения C# классов к цели-C. объекты и элементы пользовательского интерфейса.

Выполнив приведенный выше код, можно перетащить элемент управления AppKit базового типа, который вы расширяете, в область конструктора (в примере ниже — **исходный список**), переключиться в **инспектор удостоверений** и присвоить **пользовательскому классу** имя, которое Вы предоставляли цель-C (пример `SourceListView`):

[![](standard-controls-images/edit10.png "Setting a custom class in Xcode")](standard-controls-images/edit10.png#lightbox)

<a name="Exposing_Outlets_and_Actions" />

### <a name="exposing-outlets-and-actions"></a>Предоставление доступа к розеткам и действиям

Перед обращением к элементу управления AppKit C# в коде его необходимо предоставить в виде **розетки** или **действия**. Для этого выберите данный элемент управления в **иерархии интерфейсов** или в **редакторе интерфейса** и переключитесь в **представление помощника** (убедитесь, что у вас `.h` окно, выбранное для редактирования):

[![](standard-controls-images/edit11.png "Selecting the correct file to edit")](standard-controls-images/edit11.png#lightbox)

Перетащите элемент управления AppKit в файл предоставления `.h`, чтобы начать создание **розетки** или **действия**:

[![](standard-controls-images/edit12.png "Dragging to create an Outlet or Action")](standard-controls-images/edit12.png#lightbox)

Выберите тип создаваемой раскрытия и укажите **имя** **розетки** или **действия** : 

[![](standard-controls-images/edit13.png "Configuring the Outlet or Action")](standard-controls-images/edit13.png#lightbox)

Дополнительные сведения о работе с **розетками** и **действиями**см. в разделе " [розетки и действия](~/mac/get-started/hello-mac.md#outlets-and-actions) " статьи [Введение в Xcode и Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) документации.

<a name="Synchronizing_Changes_with_Xcode" />

### <a name="synchronizing-changes-with-xcode"></a>Синхронизация изменений с Xcode

При переключении обратно на Visual Studio для Mac из Xcode все изменения, внесенные в Xcode, будут автоматически синхронизированы с проектом Xamarin. Mac.

При выборе `SplitViewController.designer.cs` в **Обозреватель решений** вы сможете увидеть, как **розетка** и **действия** были привязаны в C# коде:

[![](standard-controls-images/sync01.png "Synchronizing Changes with Xcode")](standard-controls-images/sync01.png#lightbox)

Обратите внимание на то, как определение в файле `SplitViewController.designer.cs`:

```csharp
[Outlet]
AppKit.NSSplitViewItem LeftController { get; set; }

[Outlet]
AppKit.NSSplitViewItem RightController { get; set; }

[Outlet]
AppKit.NSSplitView SplitView { get; set; }
```

Построчно с определением в файле `MainWindow.h` в Xcode:

```csharp
@interface SplitViewController : NSSplitViewController {
    NSSplitViewItem *_LeftController;
    NSSplitViewItem *_RightController;
    NSSplitView *_SplitView;
}

@property (nonatomic, retain) IBOutlet NSSplitViewItem *LeftController;

@property (nonatomic, retain) IBOutlet NSSplitViewItem *RightController;

@property (nonatomic, retain) IBOutlet NSSplitView *SplitView;
```

Как видите, Visual Studio для Mac прослушивает изменения в файле `.h`, а затем автоматически синхронизирует эти изменения в соответствующем `.designer.cs` файле, чтобы предоставить их приложению. Вы также можете заметить, что `SplitViewController.designer.cs` является разделяемым классом, поэтому Visual Studio для Mac не нужно изменять `SplitViewController.cs` что приведет к перезаписи всех изменений, внесенных в класс.

Обычно вам никогда не придется открывать `SplitViewController.designer.cs` самостоятельно, он был представлен здесь только для образовательных целей.

> [!IMPORTANT]
> В большинстве случаев Visual Studio для Mac автоматически увидит все изменения, внесенные в Xcode, и синхронизирует их с проектом Xamarin. Mac. В ситуации, если синхронизация не выполняется автоматически, вернитесь в Xcode и еще раз вернитесь в Visual Studio для Mac. Обычно эти действия запускают цикл синхронизации.

<a name="Working_with_Buttons" />

## <a name="working-with-buttons"></a>Работа с кнопками

AppKit предоставляет несколько типов кнопок, которые можно использовать в разработке пользовательского интерфейса. Дополнительные сведения см. в разделе " [кнопки](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) [" руководства по использованию интерфейса пользователя ОС Apple X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/buttons01.png "An example of the different button types")](standard-controls-images/buttons01.png#lightbox)

Если кнопка была доступна через **розетку**, следующий код будет реагировать на нажатие этой кнопки:

```csharp
ButtonOutlet.Activated += (sender, e) => {
        FeedbackLabel.StringValue = "Button Outlet Pressed";
};
```

Для кнопок, которые были предоставлены через **действия**, будет автоматически создан `public partial` метод с именем, выбранным в Xcode. Чтобы ответить на **действие**, завершите разделяемый метод в классе, для которого было определено **действие** . Пример:

```csharp
partial void ButtonAction (Foundation.NSObject sender) {
    // Do something in response to the Action
    FeedbackLabel.StringValue = "Button Action Pressed";
}
```

Для кнопок, имеющих состояние (например, " **вкл** ." или " **выкл**."), состояние можно проверить или задать с помощью свойства `State` в перечислении `NSCellStateValue`. Пример:

```csharp
DisclosureButton.Activated += (sender, e) => {
    LorumIpsum.Hidden = (DisclosureButton.State == NSCellStateValue.On);
};
```

Где `NSCellStateValue` может быть:

- **При** нажатии кнопки помещается или выбирается элемент управления (например, флажок).
- **Вне** — кнопка не помещается, или элемент управления не выбран.
- **Смешанный** — смесь состояний **On** и **Off** .

<a name="Mark-a-Button-as-Default-and-Set-Key-Equivalent" />

### <a name="mark-a-button-as-default-and-set-key-equivalent"></a>Пометка кнопки как значения по умолчанию и установка эквивалентного ключа

Для любой кнопки, добавленной в структуру пользовательского интерфейса, эту кнопку можно пометить как кнопку _по умолчанию_ , которая будет активирована при нажатии пользователем клавиши **return или Enter** на клавиатуре. В macOS эта кнопка по умолчанию получит синий цвет фона.

Чтобы установить кнопку по умолчанию, выберите ее в Interface Builder Xcode. Затем в **инспекторе атрибутов**выберите поле **ключевое эквивалентное** и нажмите клавишу **return или Enter** :

[![](standard-controls-images/buttons03.png "Editing the Key Equivalent")](standard-controls-images/buttons03.png#lightbox)

Аналогичным образом можно назначить любую последовательность ключей, которую можно использовать для активации кнопки, с помощью клавиатуры, а не мыши. Например, нажав клавиши Command-C на приведенном выше рисунке.

При запуске приложения, если окно с кнопкой является ключом и имеет особое значение, если пользователь нажмет клавишу Command-C, действие для кнопки будет активировано (как если бы пользователь щелкнул кнопку).

<a name="Working_with_Checkboxes_and_Radio_Buttons" />

## <a name="working-with-checkboxes-and-radio-buttons"></a>Работа с флажками и переключателями

AppKit предоставляет несколько типов флажков и групп переключателей, которые можно использовать в разработке пользовательского интерфейса. Дополнительные сведения см. в разделе " [кнопки](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) [" руководства по использованию интерфейса пользователя ОС Apple X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/buttons02.png "An example of the available checkbox types")](standard-controls-images/buttons02.png#lightbox)

Флажки и переключатели (предоставляемые посредством **розеток**) имеют состояние (например, **On** и **Off**), состояние можно проверить или задать с помощью свойства `State` для перечисления `NSCellStateValue`. Пример:

```csharp
AdjustTime.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Adjust Time: {0}",AdjustTime.State == NSCellStateValue.On);
};
```

Где `NSCellStateValue` может быть:

- **При** нажатии кнопки помещается или выбирается элемент управления (например, флажок).
- **Вне** — кнопка не помещается, или элемент управления не выбран.
- **Смешанный** — смесь состояний **On** и **Off** .

Чтобы выбрать кнопку в группе переключателей, откройте переключатель в качестве **розетки** и задайте его свойство `State`. Пример.

```csharp
partial void SelectCar (Foundation.NSObject sender) {
    TransportationCar.State = NSCellStateValue.On;
    FeedbackLabel.StringValue = "Car Selected";
}
```

Чтобы получить коллекцию переключателей, которые будут действовать как группа и автоматически обработать выбранное состояние, создайте новое **действие** и вложите в него все кнопки в группе:

![](standard-controls-images/buttons04.png "Creating a new Action")

Затем назначьте уникальный `Tag` каждому переключателю в **инспекторе атрибутов**:

![](standard-controls-images/buttons05.png "Editing a radio button tag")

Сохраните изменения и вернитесь к Visual Studio для Mac, добавьте код для выполнения **действия** , к которому присоединены все переключатели:

```csharp
partial void NumberChanged(Foundation.NSObject sender)
{
    var check = sender as NSButton;
    Console.WriteLine("Changed to {0}", check.Tag);
}
```

Чтобы узнать, какой переключатель был выбран, можно использовать свойство `Tag`.

<a name="Working_with_Menu_Controls" />

## <a name="working-with-menu-controls"></a>Работа с элементами управления Menu

AppKit предоставляет несколько типов элементов управления Menu, которые можно использовать в разработке пользовательского интерфейса. Дополнительные сведения см. в разделе " [элементы управления меню](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlswithMenus.html#//apple_ref/doc/uid/20000957-CH100-SW1) [" руководства по использованию интерфейса пользователя ОС Apple X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/menu01.png "Example menu controls")](standard-controls-images/menu01.png#lightbox)

<a name="Providing-Menu-Control-Data" />

### <a name="providing-menu-control-data"></a>Предоставление данных элемента управления Menu

Элементы управления меню, доступные для macOS, можно задать для заполнения раскрывающегося списка либо из внутреннего списка (который может быть заранее определен в Interface Builder или заполненном с помощью кода), либо путем предоставления собственного внешнего источника данных.

<a name="Working-with-Internal-Data" />

#### <a name="working-with-internal-data"></a>Работа с внутренними данными

Помимо определения элементов в Interface Builder, элементы управления Menu (такие как `NSComboBox`) предоставляют полный набор методов, позволяющих добавлять, изменять или удалять элементы из внутреннего списка, который они сохраняют.

- `Add` — добавляет новый элемент в конец списка.
- `GetItem` — Возвращает элемент по указанному индексу.
- `Insert` — вставляет новый элемент в список в заданном месте.
- `IndexOf` — возвращает индекс данного элемента.
- `Remove` — удаляет заданный элемент из списка.
- `RemoveAll` — удаляет все элементы из списка.
- `RemoveAt` — удаляет элемент по указанному индексу.
- `Count` — возвращает число элементов в списке.

> [!IMPORTANT]
> Если используется внешний источник данных (`UsesDataSource = true`), при вызове любого из приведенных выше методов будет создано исключение.

<a name="Working-with-an-External-Data-Source" />

#### <a name="working-with-an-external-data-source"></a>Работа с внешним источником данных

Вместо использования встроенных внутренних данных для предоставления строк для элемента управления Menu можно при необходимости использовать внешний источник данных и предоставить собственное резервное хранилище для элементов (например, базы данных SQLite).

Для работы с внешним источником данных необходимо создать экземпляр источника данных элемента управления Menu (например,`NSComboBoxDataSource`) и переопределить несколько методов для предоставления необходимых данных:

- `ItemCount` — возвращает число элементов в списке.
- `ObjectValueForItem` — возвращает значение элемента для заданного индекса.
- `IndexOfItem` — возвращает индекс для значения элемента предоставления.
- `CompletedString` — возвращает первое совпадающее значение элемента для частично типизированного значения элемента. Этот метод вызывается, только если включено автозаполнение (`Completes = true`).

Дополнительные сведения см. в разделе [базы данных и поля со списком](~/mac/app-fundamentals/databases.md#Databases-and-ComboBoxes) в документе [Работа с базами данных](~/mac/app-fundamentals/databases.md) .

<a name="Adjusting-the-Lists-Appearance" />

### <a name="adjusting-the-lists-appearance"></a>Настройка внешнего вида списка

Для настройки внешнего вида элемента управления Menu доступны следующие методы.

- `HasVerticalScroller` — если `true`, то элемент управления будет отображать вертикальную полосу прокрутки. 
- `VisibleItems` — Настройка количества элементов, отображаемых при открытии элемента управления. Значение по умолчанию — пять (5).
- `IntercellSpacing`-регулировать объем пространства вокруг данного элемента, предоставляя `NSSize`, где `Width` задает левое и правое поля, а `Height` указывает пробел до и после элемента.
- `ItemHeight` — задает высоту каждого элемента в списке.

Для раскрывающихся типов `NSPopupButtons`первый элемент меню содержит заголовок элемента управления. Пример. 

[![](standard-controls-images/menu02.png "An example menu control")](standard-controls-images/menu02.png#lightbox)

Чтобы изменить заголовок, предоставьте этот элемент в качестве **розетки** и используйте код, подобный следующему:

```csharp
DropDownSelected.Title = "Item 1";
```

<a name="Manipulating-the-Selected-Items" />

### <a name="manipulating-the-selected-items"></a>Управление выбранными элементами

Следующие методы и свойства позволяют управлять выбранными элементами в списке элемента управления Menu:

- `SelectItem` — выбирает элемент по заданному индексу.
- `Select` — выберите значение заданного элемента.
- `DeselectItem` — отменяет выбор элемента по указанному индексу.
- `SelectedIndex` — возвращает индекс выбранного в данный момент элемента.
- `SelectedValue` — возвращает значение выбранного в данный момент элемента.

Используйте `ScrollItemAtIndexToTop`, чтобы отобразить элемент по заданному индексу в верхней части списка, а `ScrollItemAtIndexToVisible` прокрутить в список до тех пор, пока элемент в данном индексе не станет видимым.

<a name="Responding to Events" />

### <a name="responding-to-events"></a>Реагирование на события

Элементы управления "меню" предоставляют следующие события для реагирования на взаимодействие пользователя:

- `SelectionChanged` — вызывается, когда пользователь выбрал значение из списка.
- `SelectionIsChanging` — вызывается до того, как новый выбранный пользователем элемент становится активным выделением.
- `WillPopup` — вызывается перед отображением раскрывающегося списка элементов.
- `WillDismiss` — вызывается до закрытия раскрывающегося списка элементов.

Для `NSComboBox` элементов управления они включают в себя все те же события, что и `NSTextField`, например событие `Changed`, которое вызывается при каждом изменении пользователем значения текста в поле со списком.

При необходимости можно отреагировать на элементы меню внутренние данные, определенные в Interface Builder выбранными, подключив элемент к **действию** и используя следующий код для реагирования на **действия** , активируемые пользователем.

```csharp
partial void ItemOne (Foundation.NSObject sender) {
    DropDownSelected.Title = "Item 1";
    FeedbackLabel.StringValue = "Item One Selected";
}
```

Дополнительные сведения о работе с меню и элементами управления Menu см. в справке по [меню](~/mac/user-interface/menu.md) , [всплывающей кнопке и раскрывающемся списке](~/mac/user-interface/menu.md) .

<a name="Working_with_Selection_Controls" />

## <a name="working-with-selection-controls"></a>Работа с элементами управления Selection

AppKit предоставляет несколько типов элементов управления выбора, которые можно использовать в разработке пользовательского интерфейса. Дополнительные сведения см. в разделе [элементы управления выделением](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsSelection.html#//apple_ref/doc/uid/20000957-CH49-SW1) в [руководстве по использованию интерфейса пользователя ОС Apple X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/select01.png "Example selection controls")](standard-controls-images/select01.png#lightbox)

Существует два способа определить, когда элемент управления "выбор" имеет взаимодействие с пользователем, предоставляя его как **действие**. Пример:

```csharp
partial void SegmentButtonPressed (Foundation.NSObject sender) {
    FeedbackLabel.StringValue = string.Format("Button {0} Pressed",SegmentButtons.SelectedSegment);
}
```

Или путем присоединения **делегата** к событию `Activated`. Пример:

```csharp
TickedSlider.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
};
```

Чтобы задать или прочитать значение элемента управления выбора, используйте свойство `IntValue`. Пример:

```csharp
FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
```

Специальные элементы управления (например, хорошо подходит цвет и изображение) имеют определенные свойства для их типов значений. Пример.

```csharp
ColorWell.Color = NSColor.Red;
ImageWell.Image = NSImage.ImageNamed ("tag.png");

```

`NSDatePicker` имеет следующие свойства для работы непосредственно с датой и временем:

- **DateValue** — текущее значение даты и времени в виде `NSDate`.
- **Local** — расположение пользователя в виде `NSLocal`.
- **TimeInterval** — значение времени в виде `Double`.
- **TimeZone** — часовой пояс пользователя в качестве `NSTimeZone`.

<a name="Working_with_Indicator_Controls" />

## <a name="working-with-indicator-controls"></a>Работа с элементами управления "индикатор"

AppKit предоставляет несколько типов элементов управления "индикатор", которые можно использовать в разработке пользовательского интерфейса. Дополнительные сведения см. в разделе " [элементы управления индикатором](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsIndicators.html#//apple_ref/doc/uid/20000957-CH50-SW1) [" руководства по использованию интерфейса](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)пользователя в ОС Apple. 

[![](standard-controls-images/level01.png "Example indicator controls")](standard-controls-images/level01.png#lightbox)

Существует два способа отслеживания взаимодействия пользователя с элементом управления "индикатор" путем предоставления его в качестве **действия** или **розетки** и присоединения **делегата** к событию `Activated`. Пример:

```csharp
LevelIndicator.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Level: {0:###}",LevelIndicator.DoubleValue);
};
```

Чтобы прочитать или задать значение элемента управления "индикатор", используйте свойство `DoubleValue`. Пример:

```csharp
FeedbackLabel.StringValue = string.Format("Rating: {0:###}",Rating.DoubleValue);
```

Неопределенные и асинхронные индикаторы хода выполнения должны быть анимированы при отображении. Используйте метод `StartAnimation` для запуска анимации при их отображении. Пример:

```csharp
Indeterminate.StartAnimation (this);
AsyncProgress.StartAnimation (this);
```

Вызов метода `StopAnimation` приведет к прерыванию анимации.

<a name="Working_with_Text_Controls" />

## <a name="working-with-text-controls"></a>Работа с элементами управления "текст"

AppKit предоставляет несколько типов текстовых элементов управления, которые можно использовать в разработке пользовательского интерфейса. Дополнительные сведения см. в разделе " [текстовые элементы управления](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsText.html#//apple_ref/doc/uid/20000957-CH51-SW1) [" руководства по использованию интерфейса](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)пользователя в ОС Apple. 

[![](standard-controls-images/text01.png "Example text controls")](standard-controls-images/text01.png#lightbox)

Для текстовых полей (`NSTextField`) можно использовать следующие события для трассировки взаимодействия с пользователем.

- **Changed** — запускается каждый раз, когда пользователь изменяет значение поля. Например, для каждого вводимого символа.
- **Едитингбеган** — срабатывает, когда пользователь выбирает поле для редактирования.
- **Едитинжендед** — когда пользователь нажимает клавишу ВВОД в поле или оставляет поле.

Используйте свойство `StringValue` для чтения или задания значения поля. Пример:

```csharp
FeedbackLabel.StringValue = string.Format("User ID: {0}",UserField.StringValue);
```

Для полей, отображающих или изменяющих числовые значения, можно использовать свойство `IntValue`. Пример:

```csharp
FeedbackLabel.StringValue = string.Format("Number: {0}",NumberField.IntValue);
```

`NSTextView` предоставляет полнофункциональную область редактирования и просмотра текста с встроенным форматированием. Как и `NSTextField`, используйте свойство `StringValue` для чтения или установки значения области.

Пример сложного примера работы с текстовыми представлениями в приложении Xamarin. Mac см. в примере [приложения SourceWriter](https://docs.microsoft.com/samples/xamarin/mac-samples/sourcewriter). SourceWriter — это простой редактор исходного кода, который предоставляет поддержку для автозавершения и выделения простого синтаксиса.

Код SourceWriter полностью закомментирован, и там, где это возможно, предоставлены ссылки из основных технологий и методов на соответствующую информацию в документации по руководствам для Xamarin.Mac.

<a name="Working_with_Content_Views" />

## <a name="working-with-content-views"></a>Работа с представлениями содержимого

AppKit предоставляет несколько типов представлений содержимого, которые можно использовать в разработке пользовательского интерфейса. Дополнительные сведения см. в разделе " [представления содержимого](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) " [руководства по использованию интерфейса пользователя ОС Apple X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

[![](standard-controls-images/content01.png "An example content view")](standard-controls-images/content01.png#lightbox)

<a name="Popovers" />

### <a name="popovers"></a>поповерс

Контекстном меню Action — это временный элемент пользовательского интерфейса, который предоставляет функциональные возможности, непосредственно связанные с конкретным элементом управления или областью экрана. Контекстном меню Action перемещается над окном, содержащим элемент управления или область, с которым он связан, а его граница включает стрелку, указывающую точку, из которой она была вычислена.

Чтобы создать контекстном меню Action, выполните следующие действия.

1. Откройте файл `.storyboard` окна, в которое нужно добавить контекстном меню Action, дважды щелкнув его в **Обозреватель решений**
2. Перетащите **контроллер представления** из **инспектора библиотек** в **Редактор интерфейса**: 

    [![](standard-controls-images/content02.png "Selecting a View Controller from the Library")](standard-controls-images/content02.png#lightbox)
3. Определите размер и макет **пользовательского представления**: 

    [![](standard-controls-images/content04.png "Editing the layout")](standard-controls-images/content04.png#lightbox)
4. Щелкните элемент управления и перетащите его из источника всплывающего окна на **контроллер представления**: 

    [![](standard-controls-images/content05.png "Dragging to create a segue")](standard-controls-images/content05.png#lightbox)
5. Выберите **контекстном меню Action** во всплывающем меню: 

    [![](standard-controls-images/content06.png "Setting the segue type")](standard-controls-images/content06.png#lightbox)
6. Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

<a name="Tab_Views" />

### <a name="tab-views"></a>Представления вкладок

Представления вкладок состоят из списка вкладок (который похож на сегментированный элемент управления) в сочетании с набором представлений, называемых _панелями_. Когда пользователь выбирает новую вкладку, будет отображена панель, присоединенная к ней. Каждая панель содержит собственный набор элементов управления.

При работе с представлением вкладок в Interface Builder Xcode используйте **инспектор атрибутов** , чтобы задать число вкладок:

[![](standard-controls-images/content08.png "Editing the number of tabs")](standard-controls-images/content08.png#lightbox)

Выберите каждую вкладку в **иерархии интерфейс** , чтобы задать ее **заголовок** и добавить элементы пользовательского интерфейса в его **область**:

[![](standard-controls-images/content09.png "Editing the tabs in Xcode")](standard-controls-images/content09.png#lightbox)

<a name="Data_Binding_AppKit_Controls" />

## <a name="data-binding-appkit-controls"></a>Элементы управления AppKit привязки данных

Благодаря использованию кодирования и методов привязки данных в приложении Xamarin. Mac можно значительно сократить объем кода, который необходимо написать и поддерживать, чтобы заполнить элементы пользовательского интерфейса и работать с ними. Кроме того, у вас есть преимущество в дальнейшем разделять резервные данные (_модель данных_) от интерфейса пользователя переднего плана (_модель-представление-контроллер_), что упрощает обслуживание, более гибкое проектирование приложений.

Кодирование "ключ-значение" (КВК) — это механизм непрямого доступа к свойствам объекта с помощью клавиш (специально отформатированных строк) для обнаружения свойств вместо доступа к ним через переменные экземпляра или методы доступа (`get/set`). Реализуя методы доступа, совместимые с кодированием значений, в приложении Xamarin. Mac, вы получаете доступ к другим macOS функциям, таким как контроль значений ключа (кво), привязка данных, основные данные, привязки Cocoa и сценарии.

Дополнительные сведения см. в разделе [Простая привязка данных](~/mac/app-fundamentals/databinding.md#Simple_Data_Binding) в нашей [привязке данных и документации по кодированию "ключ-значение](~/mac/app-fundamentals/databinding.md) ".

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье подробно рассматривается работа со стандартными элементами управления AppKit, такими как кнопки, метки, текстовые поля, флажки и сегментированные элементы управления в приложении Xamarin. Mac. Он охватывает добавление их в разработку пользовательского интерфейса в Interface Builder, предоставляя их коду через функции и действия и работая с элементами управления AppKit в C# коде.

## <a name="related-links"></a>Связанные ссылки

- [Макконтролс (пример)](https://docs.microsoft.com/samples/xamarin/mac-samples/maccontrols)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [Привязка данных и кодирование типа "ключ — значение"](~/mac/app-fundamentals/databinding.md)
- [Рекомендации по работе с человеческим интерфейсом OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Сведения об элементах управления и представлениях](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1)
