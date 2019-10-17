---
title: Создание объектов пользовательского интерфейса в Xamarin. iOS
description: В этом документе содержатся общие сведения о создании пользовательского интерфейса в Xamarin. iOS. В нем обсуждается конструктор iOS, Xcode Interface Builder, C#и раскадровки.
ms.prod: xamarin
ms.assetid: 4D6B136C-744A-4936-8655-A77E62BA7A60
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: 204bf087a51132fdd204990c3b92453ecce96a53
ms.sourcegitcommit: 20c645f41620d5124da75943de1b690261d00660
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2019
ms.locfileid: "72426575"
---
# <a name="creating-user-interface-objects-in-xamarinios"></a>Создание объектов пользовательского интерфейса в Xamarin. iOS

Apple группирует связанные элементы функциональности в "frameworks", которые соответствуют пространствам имен Xamarin. iOS. `UIKit` — это пространство имен, которое содержит все элементы управления пользовательского интерфейса для iOS.

Каждый раз, когда код должен ссылаться на элемент управления пользовательского интерфейса, например метку или кнопку, не забудьте включить следующую инструкцию using:

```csharp
using UIKit;
```

Все элементы управления, обсуждаемые в этой главе, находятся в пространстве имен UIKit, а каждое имя класса пользовательского элемента управления имеет префикс `UI`.

Элементы управления и макеты пользовательского интерфейса можно редактировать тремя способами:

- **[Xamarin iOS Designer](~/ios/user-interface/designer/index.md)** — использование встроенного конструктора макетов Xamarin для разработки экранов. Дважды щелкните файл Storyboard или XIB, чтобы изменить его с помощью встроенного конструктора.
- **Xcode Interface Builder** — перетащите элементы управления на макеты экрана с помощью Interface Builder. Откройте файл Storyboard или XIB в Xcode, щелкнув правой кнопкой мыши файл в **панель решения** и выбрав пункт **открыть с помощью > Xcode Interface Builder**.
- **Using C#**  — элементы управления можно также создавать программно с помощью кода и добавлять в иерархию представлений.

Новые файлы Storyboard и XIB можно добавить, щелкнув правой кнопкой мыши проект iOS и выбрав **добавить > новый файл...** .

Независимо от используемого метода, Управление свойствами и событиями может осуществляться C# в логике приложения.

## <a name="using-xamarin-ios-designer"></a>Использование Xamarin iOS Designer

Чтобы приступить к созданию пользовательского интерфейса в конструкторе iOS, дважды щелкните файл раскадровки. Элементы управления можно перетащить в область конструктора из **панели элементов** , как показано ниже:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

 [![](creating-ui-objects-images/image2b.png "Toolbox Pad")](creating-ui-objects-images/image2b.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

 [![](creating-ui-objects-images/image2b-vs.png "Toolbox Pad - Visual Stuio")](creating-ui-objects-images/image2b.png#lightbox)

-----

При выборе элемента управления в области конструктора **панель свойств** покажет атрибуты для этого элемента управления. Поле **имени > удостоверений мини** -приложения >, которое заполняется на снимке экрана ниже, используется в качестве имени *розетки* . Вот как можно ссылаться на элемент управления в C#:

 [![](creating-ui-objects-images/image3b.png "Properties Widget Pad")](creating-ui-objects-images/image3b.png#lightbox)

Более подробные сведения об использовании конструктора iOS см. в руководстве введение в [конструктор iOS](~/ios/user-interface/designer/introduction.md) .

## <a name="using-xcode-interface-builder"></a>Использование Xcode Interface Builder

Если вы не знакомы с использованием Interface Builder, см. документацию по [Interface Builder](https://developer.apple.com/xcode/interface-builder/) Apple.

Чтобы открыть раскадровку в Xcode, щелкните правой кнопкой мыши для доступа к контекстному меню файла раскадровки и выберите команду Открыть с помощью **Xcode Interface Builder**:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

 [![](creating-ui-objects-images/imagexcode.png "Storyboard context menu - Xcode")](creating-ui-objects-images/imagexcode.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](creating-ui-objects-images/imagexcode-vs.png "Storyboard context menu - Xcode")](creating-ui-objects-images/imagexcode-vs.png#lightbox)

-----

Элементы управления можно перетаскивать на область конструктора из **библиотеки объектов** , как показано ниже:

 [![](creating-ui-objects-images/image5a.png "Xcode Object Library")](creating-ui-objects-images/image5a.png#lightbox)

При проектировании пользовательского интерфейса с Interface Builder необходимо создать **выход** для каждого элемента управления, на который вы хотите сослаться C#. Это можно сделать, включив **Редактор помощника** с помощью кнопки Center **Editor** (Центральная панель) на панели инструментов Xcode:

 [![](creating-ui-objects-images/image6a.png "Assistant Editor button")](creating-ui-objects-images/image6a.png#lightbox)

Щелкните объект пользовательского интерфейса; затем **перетащите элемент управления** в h файл. Чтобы **управлять перетаскиванием**, удерживая нажатой клавишу CTRL, щелкните и удерживайте за объектом пользовательского интерфейса, для которого создается выход (или действие). Удерживайте нажатой клавишу Control при перетаскивании в файл заголовка. Завершите перетаскивание под определением `@interface`. На рисунке ниже показана синяя линия с подписью вставить розетку или выход из коллекции.

Когда вы отпустите кнопку, появится запрос на ввод имени розетки, которая будет использоваться для создания C# свойства, на которое можно ссылаться в коде:

 [![](creating-ui-objects-images/image8a.png "Creating an outlet")](creating-ui-objects-images/image8a.png#lightbox)

Дополнительные сведения о том, как Interface Builder Xcode интегрируется с Visual Studio для Mac, см. в документе о [создании кода XIB](~/ios/internals/xib-code-generation.md#generated) .

## <a name="using-c"></a>Использование C @ no__t-0

Если вы решили создать объект пользовательского интерфейса с помощью C# (например, в представлении или контроллере представления), выполните следующие действия.

- Объявите поле уровня класса для объекта пользовательского интерфейса. Создайте элемент управления один раз, в `ViewDidLoad`, например. Затем на объект можно ссылаться во всех методах жизненного цикла контроллера представления (например,
`ViewWillAppear`).
- Создайте `CGRect`, определяющий фрейм элемента управления (его координаты X и Y на экране, а также ширину и высоту). Для этого необходимо убедиться в наличии директивы `using CoreGraphics`.
- Вызовите конструктор, чтобы создать и назначить элемент управления.
- Задайте любые свойства или обработчики событий.
- Вызовите `Add()`, чтобы добавить элемент управления в иерархию представления.

Ниже приведен простой пример создания `UILabel` в контроллере представления с помощью C#:

```csharp
UILabel label1;
public override void ViewDidLoad () {
    base.ViewDidLoad ();
    var frame = new CGRect(10, 10, 300, 30);
    label1 = new UILabel(frame);
    label1.Text = "New Label";
    View.Add (label1);
}
```

<a name="partial_classes" />

## <a name="using-c-and-storyboards"></a>Использование C# и раскадровки

При добавлении контроллеров представления в область конструктора в проекте создаются два C# соответствующих файла. В этом примере `ControlsViewController.cs` и `ControlsViewController.designer.cs` были созданы автоматически:

 [![](creating-ui-objects-images/image9b.png "ViewController partial class")](creating-ui-objects-images/image9b.png#lightbox)

Файл `ControlsViewController.cs` предназначен для *вашего кода*. Именно здесь реализуются такие методы жизненного цикла `View`, как `ViewDidLoad` и `ViewWillAppear`, и где можно добавлять собственные свойства, поля и методы.

@No__t-0 генерируется код, содержащий разделяемый класс. Когда вы назначите имя элемента управления в области конструктора в Visual Studio для Mac или создаете розетку или действие в Xcode, соответствующее свойство или разделяемый метод добавляется в файл конструктора (designer.cs). В приведенном ниже коде показан пример кода, созданного для двух кнопок и текстового представления, где одна из кнопок также имеет событие `TouchUpInside`.

Эти элементы разделяемого класса позволяют коду ссылаться на элементы управления и реагировать на действия, объявленные в области конструктора:

```csharp
[Register ("ControlsViewController")]
    partial class ControlsViewController
    {
        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UIButton Button1 { get; set; }

        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UIButton Button2 { get; set; }

        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UITextField textfield1 { get; set; }

        [Action ("button2_TouchUpInside:")]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        partial void button2_TouchUpInside (UIButton sender);

        void ReleaseDesignerOutlets ()
        {
            if (Button1 != null) {
                Button1.Dispose ();
                Button1 = null;
            }
            if (Button2 != null) {
                Button2.Dispose ();
                Button2 = null;
            }
            if (textfield1 != null) {
                textfield1.Dispose ();
                textfield1 = null;
            }
        }
    }
}
```

Файл `designer.cs` не следует изменять вручную — интегрированная среда разработки (Visual Studio для Mac или Visual Studio) отвечает за синхронизацию с раскадровкой.

Когда объекты пользовательского интерфейса добавляются программно в `View` или `ViewController`, вы сами создаете и управляете ссылки на объект, поэтому файл конструктора не требуется.

## <a name="related-links"></a>Связанные ссылки

- [Элементы управления (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
