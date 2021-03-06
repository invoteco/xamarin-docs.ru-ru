---
title: Диалоговые окна в Xamarin. Mac
description: В этой статье рассматривается работа с диалогами и модальными окнами в приложении Xamarin. Mac. Здесь описывается создание модальных окон в Xcode и конструкторе интерфейсов, работа с стандартными диалоговыми окнами и взаимодействие с C# этими элементами управления в коде.
ms.prod: xamarin
ms.assetid: 55451990-B77B-4D44-B8BB-F874EC503B0C
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: a50445307156fc051edbab7abaea6b7bd21aa1fd
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032838"
---
# <a name="dialogs-in-xamarinmac"></a>Диалоговые окна в Xamarin. Mac

При работе с C# и .NET в приложении Xamarin. Mac у вас есть доступ к тем же диалоговым окнам и модальным окнам, что и разработчик, работающий в *цели-C* и *Xcode* . Так как Xamarin. Mac интегрируется напрямую с Xcode, можно использовать _Interface Builder_ Xcode для создания и обслуживания модальных окон (или при необходимости создавать их непосредственно в C# коде).

Диалоговое окно появляется в ответ на действие пользователя и, как правило, предоставляет способы, с помощью которых пользователи могут выполнить действие. Перед закрытием диалогового окна необходимо получить от него ответ от пользователя.

Windows можно использовать в немодальном состоянии (например, в текстовом редакторе, который может одновременно открывать несколько документов) или быть модальным (например, диалоговое окно экспорта, которое должно быть закрыто до того, как приложение может быть продолжено).

[![](dialog-images/dialog03.png "An open dialog box")](dialog-images/dialog03.png#lightbox)

В этой статье рассматриваются основы работы с диалогами и модальными окнами в приложении Xamarin. Mac. Мы настоятельно рекомендуем сначала ознакомиться со статьей [Hello, Mac](~/mac/get-started/hello-mac.md) , в частности [Знакомство с Xcode и Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) , а также с разделом "возможности [и действия](~/mac/get-started/hello-mac.md#outlets-and-actions) ", так как в нем рассматриваются основные понятия и методы, которые мы будем использовать в Эта статья.

Возможно, вы захотите ознакомиться с разделом [ C# классы и методы для цели-C](~/mac/internals/how-it-works.md) в документе о внутренних компонентах [Xamarin. Mac](~/mac/internals/how-it-works.md) , а также объясняются`Register`и`Export`команды, используемые для подключения C# классов к цели-C. объекты и элементы пользовательского интерфейса.

<a name="Introduction_to_Dialogs" />

## <a name="introduction-to-dialogs"></a>Общие сведения о диалоговых окнах

Диалоговое окно появляется в ответ на действие пользователя (например, сохранение файла) и предоставляет пользователям способ выполнения этого действия. Перед закрытием диалогового окна необходимо получить от него ответ от пользователя.

В соответствии с Apple существует три способа представления диалогового окна:

- **Модальное** диалоговое окно документа. в этом случае пользователь не может выполнить никаких других действий в определенном документе, пока он не будет закрыт.
- **Модальное приложение** — модальное диалоговое окно приложения предотвращает взаимодействие пользователя с приложением, пока оно не будет закрыто.
- **Немодальное** Немодальное диалоговое окно позволяет пользователям изменять параметры диалогового окна, при этом взаимодействуя с окном документа.

### <a name="modal-window"></a>Модальное окно

Любой стандартный `NSWindow` можно использовать как настраиваемое диалоговое окно, отображая его модально:

[![](dialog-images/modal01.png "An example modal window")](dialog-images/modal01.png#lightbox)

### <a name="document-modal-dialog-sheets"></a>Модальные диалоговые листы документа

_Лист_ — это модальное диалоговое окно, которое прикрепляется к конкретному окну документа, предотвращая взаимодействие пользователей с окном до закрытия диалогового окна. Лист прикрепляется к окну, из которого оно отображается, и только один лист может быть открыт для окна в любой момент времени.

[![](dialog-images/sheet08.png "An example modal sheet")](dialog-images/sheet08.png#lightbox)

### <a name="preferences-windows"></a>Окна параметров

Окно настроек — это немодальное диалоговое окно, содержащее параметры приложения, которые пользователь изменяет редко. Windows часто содержит панель инструментов, позволяющую пользователю переключаться между различными группами параметров:

[![](dialog-images/dialog02.png "An example preference window")](dialog-images/dialog02.png#lightbox)

### <a name="open-dialog"></a>Открыть диалоговое окно

Диалоговое окно "Открыть" предоставляет пользователям единообразный способ поиска и открытия элемента в приложении:

[![](dialog-images/dialog03.png "A open dialog box")](dialog-images/dialog03.png#lightbox)

### <a name="print-and-page-setup-dialogs"></a>Диалоговые окна настройки печати и страницы

macOS предоставляет стандартные диалоговые окна печати и настройки страницы, которые могут быть отображены в приложении, чтобы пользователи могли иметь единообразный интерфейс печати в каждом используемом приложении.

Диалоговое окно Печать может отображаться как диалоговое окно с произвольным плавающей точкой:

[![](dialog-images/print01.png "A print dialog box")](dialog-images/print01.png#lightbox)

Или его можно отобразить в виде листа:

[![](dialog-images/print02.png "A print sheet")](dialog-images/print02.png#lightbox)

Диалоговое окно Параметры страницы может отображаться как диалоговое окно с произвольным плавающей запятой:

[![](dialog-images/print03.png "A page setup dialog")](dialog-images/print03.png#lightbox)

Или его можно отобразить в виде листа:

[![](dialog-images/print04.png "A page setup sheet")](dialog-images/print04.png#lightbox)

### <a name="save-dialogs"></a>Сохранить диалоговые окна

Диалоговое окно сохранения позволяет пользователям единообразно сохранять элементы в приложении. Диалоговое окно сохранения имеет два состояния: **минимальное** (также называется свернутым):

[![](dialog-images/save01.png "A save dialog")](dialog-images/save01.png#lightbox)

И **развернутое** состояние:

[![](dialog-images/save02.png "An expanded save dialog")](dialog-images/save02.png#lightbox)

Диалоговое окно **минимального** сохранения также может отображаться как лист:

[![](dialog-images/save03.png "A minimal save sheet")](dialog-images/save03.png#lightbox)

Как можно **развернутое** диалоговое окно сохранения:

[![](dialog-images/save04.png "An expanded save sheet")](dialog-images/save04.png#lightbox)

Дополнительные сведения см. в разделе " [диалоговые окна](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowDialogs.html#//apple_ref/doc/uid/20000957-CH43-SW1) [" руководства по использованию интерфейса](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) пользователя в ОС Apple.

<a name="Adding_a_Modal_Window_to_a_Project" />

## <a name="adding-a-modal-window-to-a-project"></a>Добавление модального окна в проект

Помимо главного окна документа, приложению Xamarin. Mac может потребоваться отобразить другие типы окон, например настройки или панели инспектора.

Чтобы добавить новое окно, выполните следующие действия.

1. В **Обозреватель решений**откройте файл `Main.storyboard` для редактирования в Interface Builder Xcode.
2. Перетащите новый **контроллер представления** в область конструктора:

    [![](dialog-images/new01.png "Selecting a View Controller from the Library")](dialog-images/new01.png#lightbox)
3. В **инспекторе удостоверений**введите `CustomDialogController` для **имени класса**: 

    [![](dialog-images/new02.png "Setting the class name")](dialog-images/new02.png#lightbox)
4. Вернитесь к Visual Studio для Mac, разрешите синхронизацию с Xcode и создайте файл `CustomDialogController.h`.
5. Вернитесь в Xcode и разработайте интерфейс: 

    [![](dialog-images/new03.png "Designing the UI in Xcode")](dialog-images/new03.png#lightbox)
6. Создайте **модальное перехода** из главного окна приложения на новый контроллер представления, перетащив элемент управления из элемента пользовательского интерфейса, который откроет диалоговое окно для диалогового окна. Назначьте **идентификатор** `ModalSegue`: 

    [![](dialog-images/new06.png "A modal segue")](dialog-images/new06.png#lightbox)
7. Подсоединить любые **действия** и **розетки**: 

    [![](dialog-images/new04.png "Configuring an Action")](dialog-images/new04.png#lightbox)
8. Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Сделайте файл `CustomDialogController.cs` выглядеть следующим образом:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDialog
{
    public partial class CustomDialogController : NSViewController
    {
        #region Private Variables
        private string _dialogTitle = "Title";
        private string _dialogDescription = "Description";
        private NSViewController _presentor;
        #endregion

        #region Computed Properties
        public string DialogTitle {
            get { return _dialogTitle; }
            set { _dialogTitle = value; }
        }

        public string DialogDescription {
            get { return _dialogDescription; }
            set { _dialogDescription = value; }
        }

        public NSViewController Presentor {
            get { return _presentor; }
            set { _presentor = value; }
        }
        #endregion

        #region Constructors
        public CustomDialogController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Set initial title and description
            Title.StringValue = DialogTitle;
            Description.StringValue = DialogDescription;
        }
        #endregion

        #region Private Methods
        private void CloseDialog() {
            Presentor.DismissViewController (this);
        }
        #endregion

        #region Custom Actions
        partial void AcceptDialog (Foundation.NSObject sender) {
            RaiseDialogAccepted();
            CloseDialog();
        }

        partial void CancelDialog (Foundation.NSObject sender) {
            RaiseDialogCanceled();
            CloseDialog();
        }
        #endregion

        #region Events
        public EventHandler DialogAccepted;

        internal void RaiseDialogAccepted() {
            if (this.DialogAccepted != null)
                this.DialogAccepted (this, EventArgs.Empty);
        }

        public EventHandler DialogCanceled;

        internal void RaiseDialogCanceled() {
            if (this.DialogCanceled != null)
                this.DialogCanceled (this, EventArgs.Empty);
        }
        #endregion
    }
}
```

Этот код предоставляет несколько свойств для задания заголовка и описания диалогового окна, а также несколько событий, реагирующих на то, что диалоговое окно будет отменено или принято.

Затем измените файл `ViewController.cs`, переопределите метод `PrepareForSegue` и сделайте его следующим:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on the segue name
    switch (segue.Identifier) {
    case "ModalSegue":
        var dialog = segue.DestinationController as CustomDialogController;
        dialog.DialogTitle = "MacDialog";
        dialog.DialogDescription = "This is a sample dialog.";
        dialog.DialogAccepted += (s, e) => {
            Console.WriteLine ("Dialog accepted");
            DismissViewController (dialog);
        };
        dialog.Presentor = this;
        break;
    }
}
```

Этот код инициализирует перехода, определенный в Interface Builderе Xcode в нашем диалоговом окне, и настраивает заголовок и описание. Он также управляет выбором, который пользователь делает в диалоговом окне.

Мы можем запустить наше приложение и отобразить пользовательское диалоговое окно:

[![](dialog-images/new05.png "An example dialog")](dialog-images/new05.png#lightbox)

Дополнительные сведения об использовании Windows в приложении Xamarin. Mac см. в документации по [Windows](~/mac/user-interface/window.md) .

<a name="Creating_a_Custom_Sheet" />

## <a name="creating-a-custom-sheet"></a>Создание пользовательского листа

_Лист_ — это модальное диалоговое окно, которое прикрепляется к конкретному окну документа, предотвращая взаимодействие пользователей с окном до закрытия диалогового окна. Лист прикрепляется к окну, из которого оно отображается, и только один лист может быть открыт для окна в любой момент времени. 

Чтобы создать настраиваемый лист в Xamarin. Mac, давайте выполним следующие действия.

1. В **Обозреватель решений**откройте файл `Main.storyboard` для редактирования в Interface Builder Xcode.
2. Перетащите новый **контроллер представления** в область конструктора:

    [![](dialog-images/new01.png "Selecting a View Controller from the Library")](dialog-images/new01.png#lightbox)
3. Проектирование пользовательского интерфейса:

    [![](dialog-images/sheet01.png "The UI design")](dialog-images/sheet01.png#lightbox)
4. Создайте **лист, перехода** из главного окна в новый контроллер представления: 

    [![](dialog-images/sheet02.png "Selecting the Sheet segue type")](dialog-images/sheet02.png#lightbox)
5. В **инспекторе удостоверений**назовите **класс** контроллера представления `SheetViewController`: 

    [![](dialog-images/sheet03.png "Setting the class name")](dialog-images/sheet03.png#lightbox)
6. Определите необходимые **розетки** и **действия**: 

    [![](dialog-images/sheet04.png "Defining the required Outlets and Actions")](dialog-images/sheet04.png#lightbox)
7. Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации.

Затем измените файл `SheetViewController.cs` и сделайте его следующим:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDialog
{
    public partial class SheetViewController : NSViewController
    {
        #region Private Variables
        private string _userName = "";
        private string _password = "";
        private NSViewController _presentor;
        #endregion

        #region Computed Properties
        public string UserName {
            get { return _userName; }
            set { _userName = value; }
        }

        public string Password {
            get { return _password;}
            set { _password = value;}
        }

        public NSViewController Presentor {
            get { return _presentor; }
            set { _presentor = value; }
        }
        #endregion

        #region Constructors
        public SheetViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Set initial values
            NameField.StringValue = UserName;
            PasswordField.StringValue = Password;

            // Wireup events
            NameField.Changed += (sender, e) => {
                UserName = NameField.StringValue;
            };
            PasswordField.Changed += (sender, e) => {
                Password = PasswordField.StringValue;
            };
        }
        #endregion

        #region Private Methods
        private void CloseSheet() {
            Presentor.DismissViewController (this);
        }
        #endregion

        #region Custom Actions
        partial void AcceptSheet (Foundation.NSObject sender) {
            RaiseSheetAccepted();
            CloseSheet();
        }

        partial void CancelSheet (Foundation.NSObject sender) {
            RaiseSheetCanceled();
            CloseSheet();
        }
        #endregion

        #region Events
        public EventHandler SheetAccepted;

        internal void RaiseSheetAccepted() {
            if (this.SheetAccepted != null)
                this.SheetAccepted (this, EventArgs.Empty);
        }

        public EventHandler SheetCanceled;

        internal void RaiseSheetCanceled() {
            if (this.SheetCanceled != null)
                this.SheetCanceled (this, EventArgs.Empty);
        }
        #endregion
    }
}
```

Затем измените файл `ViewController.cs`, измените метод `PrepareForSegue` и сделайте его следующим:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on the segue name
    switch (segue.Identifier) {
    case "ModalSegue":
        var dialog = segue.DestinationController as CustomDialogController;
        dialog.DialogTitle = "MacDialog";
        dialog.DialogDescription = "This is a sample dialog.";
        dialog.DialogAccepted += (s, e) => {
            Console.WriteLine ("Dialog accepted");
            DismissViewController (dialog);
        };
        dialog.Presentor = this;
        break;
    case "SheetSegue":
        var sheet = segue.DestinationController as SheetViewController;
        sheet.SheetAccepted += (s, e) => {
            Console.WriteLine ("User Name: {0} Password: {1}", sheet.UserName, sheet.Password);
        };
        sheet.Presentor = this;
        break;
    }
}
```

Если мы запустим приложение и открыли лист, он будет присоединен к окну:

[![](dialog-images/sheet08.png "An example sheet")](dialog-images/sheet08.png#lightbox)

<a name="Creating_a_Preferences_Dialog" />

## <a name="creating-a-preferences-dialog"></a>Создание диалогового окна настроек

Прежде чем размещать представление предпочтений в Interface Builder, необходимо добавить пользовательский тип перехода для управления параметрами. Добавьте в проект новый класс и вызовите его `ReplaceViewSeque`. Измените класс и сделайте его следующим:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacWindows
{
    [Register("ReplaceViewSeque")]
    public class ReplaceViewSeque : NSStoryboardSegue
    {
        #region Constructors
        public ReplaceViewSeque() {

        }

        public ReplaceViewSeque (string identifier, NSObject sourceController, NSObject destinationController) : base(identifier,sourceController,destinationController) {

        }

        public ReplaceViewSeque (IntPtr handle) : base(handle) {
        }

        public ReplaceViewSeque (NSObjectFlag x) : base(x) {
        }
        #endregion

        #region Override Methods
        public override void Perform ()
        {
            // Cast the source and destination controllers
            var source = SourceController as NSViewController;
            var destination = DestinationController as NSViewController;

            // Is there a source?
            if (source == null) {
                // No, get the current key window
                var window = NSApplication.SharedApplication.KeyWindow;

                // Swap the controllers
                window.ContentViewController = destination;

                // Release memory
                window.ContentViewController?.RemoveFromParentViewController ();
            } else {
                // Swap the controllers
                source.View.Window.ContentViewController = destination;

                // Release memory
                source.RemoveFromParentViewController ();
            }
        
        }
        #endregion

    }

}
```

После создания настраиваемого перехода можно добавить новое окно в Interface Builder Xcode для удовлетворения наших предпочтений.

Чтобы добавить новое окно, выполните следующие действия.

1. В **Обозреватель решений**откройте файл `Main.storyboard` для редактирования в Interface Builder Xcode.
2. Перетащите новый **контроллер окна** в область конструктора:

    [![](dialog-images/pref01.png "Select a Window Controller from the Library")](dialog-images/pref01.png#lightbox)
3. Расположите окно рядом с конструктором **строки меню** :

    [![](dialog-images/pref02.png "Adding the new Window")](dialog-images/pref02.png#lightbox)
4. Создайте копии присоединенного контроллера представления, так как в представлении настроек будут доступны вкладки:

    [![](dialog-images/pref03.png "Adding the required View Controllers")](dialog-images/pref03.png#lightbox)
5. Перетащите новый **контроллер панели инструментов** из **библиотеки**:

    [![](dialog-images/pref04.png "Select a Toolbar Controller from the Library")](dialog-images/pref04.png#lightbox)
6. И поместите его в окно область конструктора:

    [![](dialog-images/pref05.png "Adding a new Toolbar Controller")](dialog-images/pref05.png#lightbox)
7. Разработайте макет панели инструментов:

    [![](dialog-images/pref06.png "Layout the toolbar")](dialog-images/pref06.png#lightbox)
8. Нажмите кнопку «Control» (управление) и перетащите кнопку из каждой **кнопки панели инструментов** в представления, созданные выше. Выберите **Пользовательский** тип перехода:

    [![](dialog-images/pref07.png "Setting the segue type")](dialog-images/pref07.png#lightbox)
9. Выберите новый перехода и задайте **класс** для `ReplaceViewSegue`:

    [![](dialog-images/pref08.png "Setting the segue class")](dialog-images/pref08.png#lightbox)
10. В **конструкторе Menubar** область конструктора в меню приложение выберите пункт **предпочтения...** , щелкните элемент управления и перетащите его в окно настройки, чтобы создать **Показ** перехода:

    [![](dialog-images/pref09.png "Setting the segue type")](dialog-images/pref09.png#lightbox)
11. Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации.

Если выполнить код и выбрать **Параметры...** в **меню приложение**, откроется окно:

[![](dialog-images/pref10.png "An example preferences window")](dialog-images/pref10.png#lightbox)

Дополнительные сведения о работе с окнами и панелями инструментов см. в документации по [Windows](~/mac/user-interface/window.md) и [панелям инструментов](~/mac/user-interface/toolbar.md) .

<a name="Saving-and-Loading-Preferences" />

### <a name="saving-and-loading-preferences"></a>Сохранение и загрузка настроек

В типичном приложении macOS, когда пользователь вносит изменения в какие-либо пользовательские настройки приложения, эти изменения сохраняются автоматически. Самый простой способ справиться с этим в приложении Xamarin. Mac — создать единый класс для управления всеми предпочтениями пользователя и совместно использовать его в масштабах всей системы.

Сначала добавьте новый класс `AppPreferences` в проект и наследуйте его `NSObject`. Эти предпочтения будут использоваться для [привязки данных и кодирования значений типа «ключ-значение](~/mac/app-fundamentals/databinding.md) », которые значительно упрощают процесс создания и обслуживания форм предпочтений. Поскольку настройки будут состоять из небольшого количества простых типов данных, используйте встроенную `NSUserDefaults` для хранения и извлечения значений.

Измените файл `AppPreferences.cs` и сделайте его следующим:

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    [Register("AppPreferences")]
    public class AppPreferences : NSObject
    {
        #region Computed Properties
        [Export("DefaultLanguage")]
        public int DefaultLanguage {
            get { 
                var value = LoadInt ("DefaultLanguage", 0);
                return value; 
            }
            set {
                WillChangeValue ("DefaultLanguage");
                SaveInt ("DefaultLanguage", value, true);
                DidChangeValue ("DefaultLanguage");
            }
        }

        [Export("SmartLinks")]
        public bool SmartLinks {
            get { return LoadBool ("SmartLinks", true); }
            set {
                WillChangeValue ("SmartLinks");
                SaveBool ("SmartLinks", value, true);
                DidChangeValue ("SmartLinks");
            }
        }

        // Define any other required user preferences in the same fashion
        ...

        [Export("EditorBackgroundColor")]
        public NSColor EditorBackgroundColor {
            get { return LoadColor("EditorBackgroundColor", NSColor.White); }
            set {
                WillChangeValue ("EditorBackgroundColor");
                SaveColor ("EditorBackgroundColor", value, true);
                DidChangeValue ("EditorBackgroundColor");
            }
        }
        #endregion

        #region Constructors
        public AppPreferences ()
        {
        }
        #endregion

        #region Public Methods
        public int LoadInt(string key, int defaultValue) {
            // Attempt to read int
            var number = NSUserDefaults.StandardUserDefaults.IntForKey(key);

            // Take action based on value
            if (number == null) {
                return defaultValue;
            } else {
                return (int)number;
            }
        }
            
        public void SaveInt(string key, int value, bool sync) {
            NSUserDefaults.StandardUserDefaults.SetInt(value, key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }

        public bool LoadBool(string key, bool defaultValue) {
            // Attempt to read int
            var value = NSUserDefaults.StandardUserDefaults.BoolForKey(key);

            // Take action based on value
            if (value == null) {
                return defaultValue;
            } else {
                return value;
            }
        }

        public void SaveBool(string key, bool value, bool sync) {
            NSUserDefaults.StandardUserDefaults.SetBool(value, key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }

        public string NSColorToHexString(NSColor color, bool withAlpha) {
            //Break color into pieces
            nfloat red=0, green=0, blue=0, alpha=0;
            color.GetRgba (out red, out green, out blue, out alpha);

            // Adjust to byte
            alpha *= 255;
            red *= 255;
            green *= 255;
            blue *= 255;

            //With the alpha value?
            if (withAlpha) {
                return String.Format ("#{0:X2}{1:X2}{2:X2}{3:X2}", (int)alpha, (int)red, (int)green, (int)blue);
            } else {
                return String.Format ("#{0:X2}{1:X2}{2:X2}", (int)red, (int)green, (int)blue);
            }
        }

        public NSColor NSColorFromHexString (string hexValue)
        {
            var colorString = hexValue.Replace ("#", "");
            float red, green, blue, alpha;

            // Convert color based on length
            switch (colorString.Length) {
            case 3 : // #RGB
                red = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(0, 1)), 16) / 255f;
                green = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(1, 1)), 16) / 255f;
                blue = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(2, 1)), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, 1.0f);
            case 6 : // #RRGGBB
                red = Convert.ToInt32(colorString.Substring(0, 2), 16) / 255f;
                green = Convert.ToInt32(colorString.Substring(2, 2), 16) / 255f;
                blue = Convert.ToInt32(colorString.Substring(4, 2), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, 1.0f);
            case 8 : // #AARRGGBB
                alpha = Convert.ToInt32(colorString.Substring(0, 2), 16) / 255f;
                red = Convert.ToInt32(colorString.Substring(2, 2), 16) / 255f;
                green = Convert.ToInt32(colorString.Substring(4, 2), 16) / 255f;
                blue = Convert.ToInt32(colorString.Substring(6, 2), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, alpha);
            default :
                throw new ArgumentOutOfRangeException(string.Format("Invalid color value '{0}'. It should be a hex value of the form #RBG, #RRGGBB or #AARRGGBB", hexValue));
            }
        }

        public NSColor LoadColor(string key, NSColor defaultValue) {

            // Attempt to read color
            var hex = NSUserDefaults.StandardUserDefaults.StringForKey(key);

            // Take action based on value
            if (hex == null) {
                return defaultValue;
            } else {
                return NSColorFromHexString (hex);
            }
        }

        public void SaveColor(string key, NSColor color, bool sync) {
            // Save to default
            NSUserDefaults.StandardUserDefaults.SetString(NSColorToHexString(color,true), key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }
        #endregion
    }
}
```

Этот класс содержит несколько вспомогательных подпрограмм, таких как `SaveInt`, `LoadInt`, `SaveColor`, `LoadColor`и т. д., чтобы упростить работу с `NSUserDefaults`. Кроме того, поскольку `NSUserDefaults` не имеет встроенного способа для работы с `NSColors`, методы `NSColorToHexString` и `NSColorFromHexString` используются для преобразования цветов в шестнадцатеричные строки в Интернете (`#RRGGBBAA`, где `AA` — это альфа-прозрачность), которую можно легко сохранить и извлечь.

В файле `AppDelegate.cs` создайте экземпляр объекта **апппреференцес** , который будет использоваться на уровне приложения:

```csharp
using AppKit;
using Foundation;
using System.IO;
using System;

namespace SourceWriter
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewWindowNumber { get; set;} = -1;

        public AppPreferences Preferences { get; set; } = new AppPreferences();
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion
        
        ...
```

<a name="Wiring-Preferences-to-Preference-Views" />

### <a name="wiring-preferences-to-preference-views"></a>Настройки подключения к представлениям предпочтений

Затем соедините класс предпочтений с элементами пользовательского интерфейса в окне предпочтений и представлениях, созданных выше. В Interface Builder выберите контроллер представления предпочтений и перейдите в **инспектор удостоверений**и создайте настраиваемый класс для контроллера: 

[![](dialog-images/prefs12.png "The Identity Inspector")](dialog-images/prefs12.png#lightbox)

Вернитесь к Visual Studio для Mac, чтобы синхронизировать изменения и открыть только что созданный класс для редактирования. Сделайте класс похожим на следующий:

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    public partial class EditorPrefsController : NSViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        [Export("Preferences")]
        public AppPreferences Preferences {
            get { return App.Preferences; }
        }
        #endregion

        #region Constructors
        public EditorPrefsController (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

Обратите внимание, что этот класс выполнил две вещи: во-первых, существует вспомогательное `App` свойство, чтобы упростить доступ к **AppDelegate** . Во вторых, свойство `Preferences` предоставляет Глобальный класс **апппреференцес** для привязки данных с любыми элементами управления пользовательского интерфейса, помещенными в это представление.

Затем дважды щелкните файл раскадровки, чтобы снова открыть его в Interface Builder (и просмотрите изменения, внесенные выше). Перетащите все элементы управления пользовательского интерфейса, необходимые для создания интерфейса предпочтений, в представлении. Для каждого элемента управления переключитесь на **инспектор привязки** и привяжите к отдельным свойствам класса **апппреференце** :

[![](dialog-images/prefs13.png "The Binding Inspector")](dialog-images/prefs13.png#lightbox)

Повторите описанные выше действия для всех панелей (контроллеров представления) и свойств предпочтений.

<a name="Applying-Preference-Changes-to-All-Open-Windows" />

### <a name="applying-preference-changes-to-all-open-windows"></a>Применение изменений предпочтений ко всем открытым окнам

Как упоминалось выше, в типичном macOS приложении, когда пользователь вносит изменения в какие-либо пользовательские настройки приложения, эти изменения сохраняются автоматически и применяются к любым окнам, которые пользователь мог открыть в приложении.

Тщательное планирование и проектирование предпочтений приложения и Windows позволит этому процессу беспрепятственно и прозрачно работать с конечным пользователем с минимальным объемом кодирования.

Для любого окна, которое будет использовать настройки приложения, добавьте следующее вспомогательное свойство в свой контроллер представления содержимого, чтобы упростить доступ к нашему **AppDelegate** :

```csharp
#region Application Access
public static AppDelegate App {
    get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
}
#endregion
```

Затем добавьте класс для настройки содержимого или поведения в зависимости от настроек пользователя:

```csharp
public void ConfigureEditor() {

    // General Preferences
    TextEditor.AutomaticLinkDetectionEnabled = App.Preferences.SmartLinks;
    TextEditor.AutomaticQuoteSubstitutionEnabled = App.Preferences.SmartQuotes;
    ...

}
``` 

При первом открытии окна необходимо вызвать метод настройки, чтобы убедиться, что он соответствует предпочтениям пользователя:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Configure editor from user preferences
    ConfigureEditor ();
    ...
}
```

Затем измените файл `AppDelegate.cs` и добавьте следующий метод, чтобы применить изменения предпочтений ко всем открытым окнам:

```csharp
public void UpdateWindowPreferences() {

    // Process all open windows
    for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
        var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
        if (content != null ) {
            // Reformat all text
            content.ConfigureEditor ();
        }
    }

}
```

Затем добавьте класс `PreferenceWindowDelegate` в проект и сделайте его следующим:

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class PreferenceWindowDelegate : NSWindowDelegate
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public NSWindow Window { get; set;}
        #endregion

        #region constructors
        public PreferenceWindowDelegate (NSWindow window)
        {
            // Initialize
            this.Window = window;

        }
        #endregion

        #region Override Methods
        public override bool WindowShouldClose (Foundation.NSObject sender)
        {
            
            // Apply any changes to open windows
            App.UpdateWindowPreferences();

            return true;
        }
        #endregion
    }
}
```

Это приведет к тому, что любые изменения предпочтений будут отправляться во все открытые окна при закрытии окна предпочтений.

Наконец, измените контроллер окна предпочтений и добавьте созданный ранее делегат:

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    public partial class PreferenceWindowController : NSWindowController
    {
        #region Constructors
        public PreferenceWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Initialize
            Window.Delegate = new PreferenceWindowDelegate(Window);
            Toolbar.SelectedItemIdentifier = "General";
        }
        #endregion
    }
}
```

После внесения этих изменений, если пользователь редактирует настройки приложения и закрывает окно предпочтений, изменения будут применены ко всем открытым окнам:

[![](dialog-images/prefs14.png "An example preferences window")](dialog-images/prefs14.png#lightbox)

<a name="The_Open_Dialog" />

## <a name="the-open-dialog"></a>Диалоговое окно открытия

Диалоговое окно Открыть предоставляет пользователям единообразный способ поиска и открытия элемента в приложении. Чтобы отобразить открытое диалоговое окно в приложении Xamarin. Mac, используйте следующий код:

```csharp
var dlg = NSOpenPanel.OpenPanel;
dlg.CanChooseFiles = true;
dlg.CanChooseDirectories = false;
dlg.AllowedFileTypes = new string[] { "txt", "html", "md", "css" };

if (dlg.RunModal () == 1) {
    // Nab the first file
    var url = dlg.Urls [0];

    if (url != null) {
        var path = url.Path;

        // Create a new window to hold the text
        var newWindowController = new MainWindowController ();
        newWindowController.Window.MakeKeyAndOrderFront (this);

        // Load the text into the window
        var window = newWindowController.Window as MainWindow;
        window.Text = File.ReadAllText(path);
        window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
        window.RepresentedUrl = url;

    }
}
```

В приведенном выше коде открывается новое окно документа, в котором отображается содержимое файла. Вам потребуется заменить этот код функциями, необходимыми для приложения.

При работе с `NSOpenPanel`доступны следующие свойства:

- **Канчусефилес** — если `true` пользователь может выбрать файлы.
- **Канчуседиректориес** — если `true` пользователь может выбрать каталоги.
- **Алловсмултиплеселектион** — если `true` пользователь может выбрать более одного файла одновременно.
- **Ресолвеалиасес** — если `true` выборе и псевдонима, разрешается в путь к исходному файлу.
- **Алловедфилетипес** — это строковый массив типов файлов, которые пользователь может выбрать либо как расширение, либо как _UTI_. Значение по умолчанию — `null`, что позволяет открывать любой файл.

Метод `RunModal ()` открывает диалоговое окно Открыть и позволяет пользователю выбрать файлы или каталоги (как указано в свойствах) и возвращает значение `1`, если пользователь нажмет кнопку **Открыть** .

Диалоговое окно Открыть возвращает выбранные файлы или каталоги пользователя в виде массива URL-адресов в свойстве `URL`.

Если запустить программу и выбрать пункт **Открыть...** в меню **файл** , отобразится следующее: 

[![](dialog-images/dialog03.png "An open dialog box")](dialog-images/dialog03.png#lightbox)

<a name="The_Print_and_Page_Setup_Dialogs" />

## <a name="the-print-and-page-setup-dialogs"></a>Диалоговые окна настройки печати и страницы

macOS предоставляет стандартные диалоговые окна печати и настройки страницы, которые могут быть отображены в приложении, чтобы пользователи могли иметь единообразный интерфейс печати в каждом используемом приложении.

В следующем коде будет показано стандартное диалоговое окно печати:

```csharp
public bool ShowPrintAsSheet { get; set;} = true;
...

[Export ("showPrinter:")]
void ShowDocument (NSObject sender) {
    var dlg = new NSPrintPanel();

    // Display the print dialog as dialog box
    if (ShowPrintAsSheet) {
        dlg.BeginSheet(new NSPrintInfo(),this,this,null,new IntPtr());
    } else {
        if (dlg.RunModalWithPrintInfo(new NSPrintInfo()) == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to print the document here...",
                MessageText = "Print Document",
            };
            alert.RunModal ();
        }
    }
}

```

Если задать для свойства `ShowPrintAsSheet` значение `false`, запустите приложение и отобразится диалоговое окно Печать, после чего будет отображено следующее:

[![](dialog-images/print01.png "A print dialog box")](dialog-images/print01.png#lightbox)

Если для свойства `ShowPrintAsSheet` задано значение `true`, запустите приложение и отобразится диалоговое окно Печать, после чего будет отображено следующее:

[![](dialog-images/print02.png "A print sheet")](dialog-images/print02.png#lightbox)

В следующем коде отображается диалоговое окно "макет страницы":

```csharp
[Export ("showLayout:")]
void ShowLayout (NSObject sender) {
    var dlg = new NSPageLayout();

    // Display the print dialog as dialog box
    if (ShowPrintAsSheet) {
        dlg.BeginSheet (new NSPrintInfo (), this);
    } else {
        if (dlg.RunModal () == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to print the document here...",
                MessageText = "Print Document",
            };
            alert.RunModal ();
        }
    }
}
```

Если задать для свойства `ShowPrintAsSheet` значение `false`, запустить приложение и открыть диалоговое окно макета печати, будут отображены следующие данные:

[![](dialog-images/print03.png "A page setup dialog")](dialog-images/print03.png#lightbox)

Если задать для свойства `ShowPrintAsSheet` значение `true`, запустить приложение и открыть диалоговое окно макета печати, будут отображены следующие данные:

[![](dialog-images/print04.png "A page setup sheet")](dialog-images/print04.png#lightbox)

Дополнительные сведения о работе с диалоговыми окнами печати и настройки страницы см. в документации Apple [нспринтпанел](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPrintPanel_Class/index.html#//apple_ref/doc/uid/TP40004092) и [нспажелайаут](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPageLayout_Class/index.html#//apple_ref/doc/uid/TP40004080) .

<a name="The_Save_Dialog" />

## <a name="the-save-dialog"></a>Диалоговое окно сохранения

Диалоговое окно сохранения позволяет пользователям единообразно сохранять элементы в приложении.

В следующем коде будет показано стандартное диалоговое окно сохранения:

```csharp
public bool ShowSaveAsSheet { get; set;} = true;
...

[Export("saveDocumentAs:")]
void ShowSaveAs (NSObject sender)
{
    var dlg = new NSSavePanel ();
    dlg.Title = "Save Text File";
    dlg.AllowedFileTypes = new string[] { "txt", "html", "md", "css" };

    if (ShowSaveAsSheet) {
        dlg.BeginSheet(mainWindowController.Window,(result) => {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        });
    } else {
        if (dlg.RunModal () == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        }
    }

}
```

Свойство `AllowedFileTypes` — это строковый массив типов файлов, которые пользователь может выбрать для сохранения файла как. Тип файла можно указать как расширение или _UTI_. Значение по умолчанию — `null`, что позволяет использовать любой тип файла.

Если задать для свойства `ShowSaveAsSheet` значение `false`, запустите приложение и выберите **Сохранить как...** в меню **файл** отобразится следующее:

[![](dialog-images/save01.png "A save dialog box")](dialog-images/save01.png#lightbox)

Пользователь может развернуть диалоговое окно:

[![](dialog-images/save02.png "An expanded save dialog box")](dialog-images/save02.png#lightbox)

Если задать для свойства `ShowSaveAsSheet` значение `true`, запустите приложение и выберите **Сохранить как...** в меню **файл** отобразится следующее:

[![](dialog-images/save03.png "A save sheet")](dialog-images/save03.png#lightbox)

Пользователь может развернуть диалоговое окно:

[![](dialog-images/save04.png "An expanded save sheet")](dialog-images/save04.png#lightbox)

Дополнительные сведения о работе с диалоговым окном сохранения см. в документации Apple [нссавепанел](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSSavePanel_Class/index.html#//apple_ref/doc/uid/TP40004098) .

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье подробно рассматривается работа с модальными окнами, листами и стандартными системными диалоговыми окнами в приложении Xamarin. Mac. Мы видели различные типы и использование модальных окон, листов и диалогов, как создавать и поддерживать модальные окна и листы в Interface Builder Xcode и как работать с модальными окнами, листами и диалогами в C# коде.

## <a name="related-links"></a>Связанные ссылки

- [Маквиндовс (пример)](https://docs.microsoft.com/samples/xamarin/mac-samples/macwindows)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Меню](~/mac/user-interface/menu.md)
- [Windows](~/mac/user-interface/window.md)
- [Панели инструментов](~/mac/user-interface/toolbar.md)
- [Рекомендации по работе с человеческим интерфейсом OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Введение в Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [Общие сведения о листах](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Sheets/Sheets.html#//apple_ref/doc/uid/10000002i)
- [Общие сведения о печати](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Printing/osxp_aboutprinting/osxp_aboutprt.html)
