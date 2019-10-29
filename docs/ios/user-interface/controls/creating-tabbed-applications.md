---
title: Панели вкладок и контроллеры панели вкладок в Xamarin. iOS
description: В этом документе описываются контроллеры панели вкладок iOS и способы их использования с Xamarin. iOS. В нем демонстрируется настройка Уитаббарконтроллер, работа с изображениями, установка значений значков, работа с событиями и многое другое.
ms.prod: xamarin
ms.assetid: 7C772899-2900-F139-D642-F3C4F3F14DDC
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 9f8a5e568946e1aea8541211ec3adc45a25f1897
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022140"
---
# <a name="tab-bars-and-tab-bar-controllers-in-xamarinios"></a>Панели вкладок и контроллеры панели вкладок в Xamarin. iOS

Приложения с вкладками используются в iOS для поддержки пользовательских интерфейсов, в которых доступ к нескольким экранам можно получить без определенного порядка. С помощью класса `UITabBarController` приложения могут легко включать поддержку таких сценариев с несколькими экранами. `UITabBarController` осуществляет управление несколькими экранами, позволяя разработчику приложения сосредоточиться на деталях каждого экрана.

Как правило, приложения с вкладками создаются с `UITabBarController` `RootViewController` главного окна. Однако с помощью некоторого дополнительного кода приложения с вкладками также могут использоваться для последующего перехода на другой начальный экран, например сценарий, в котором приложение сначала отображает экран входа, а затем — интерфейс с вкладками.

Мы рассмотрим использование вкладок здесь, просматривая пошаговое руководство по работе с простым приложением. Затем мы рассмотрим работу с вкладками в сценарии, не относящемся к `RootViewController`.

## <a name="introducing-uitabbarcontroller"></a>Знакомство с Уитаббарконтроллер

`UITabBarController` поддерживает разработку приложений с вкладками следующим образом:

- Разрешение добавления к нему нескольких контроллеров.
- Предоставление пользовательского интерфейса с вкладками через класс `UITabBar`, позволяющий пользователю переключаться между контроллерами и их представлениями. 

Контроллеры добавляются в `UITabBarController` через свойство `ViewControllers`, которое является массивом `UIViewController`. `UITabBarController` сам обрабатывает загрузку соответствующего контроллера и представление его представления на основе выбранной вкладки.

Вкладки являются экземплярами класса `UITabBarItem`, которые содержатся в экземпляре `UITabBar`. Каждый экземпляр `UITabBar` доступен через свойство `TabBarItem` контроллера на каждой вкладке.

Чтобы понять, как работать с `UITabBarController`, давайте рассмотрим создание простого приложения, использующего его.

## <a name="tabbed-application-walkthrough"></a>Пошаговое руководство по приложению с вкладками

В этом пошаговом руководстве мы создадим следующее приложение:

[![](creating-tabbed-applications-images/00-app.png "Sample tabbed app")](creating-tabbed-applications-images/00-app.png#lightbox)

Несмотря на то, что в Visual Studio для Mac уже есть шаблон приложения с вкладками, в этом примере мы будем работать из пустого проекта, чтобы получить более полное представление о том, как создается приложение.

 <a name="Creating_the_Application" />

### <a name="creating-the-application"></a>Создание приложения

Начнем с создания нового приложения.

Выберите пункт меню **файл > создать > решение** в Visual Studio для Mac и выберите **приложение > iOS > пустой шаблон проекта** , назовите проект `TabbedApplication`, как показано ниже:

[![](creating-tabbed-applications-images/newsolution1.png "Select the Empty Project template")](creating-tabbed-applications-images/newsolution1.png#lightbox)

[![](creating-tabbed-applications-images/newsolution2.png "Name the project TabbedApplication")](creating-tabbed-applications-images/newsolution2.png#lightbox)

### <a name="adding-the-uitabbarcontroller"></a>Добавление Уитаббарконтроллер

Затем добавьте пустой класс, выбрав **файл > новый файл** и выбрав шаблон **Общий: пустой класс** . Присвойте файлу имя `TabController`, как показано ниже:

[![](creating-tabbed-applications-images/02-newclass.png "Add the TabController class")](creating-tabbed-applications-images/02-newclass.png#lightbox)

Класс `TabController` будет содержать реализацию `UITabBarController`, которая будет управлять массивом `UIViewControllers`. Когда пользователь выбирает вкладку, `UITabBarController` будет представлять представление для соответствующего контроллера представления.

Для реализации `UITabBarController` необходимо выполнить следующие действия.

1. Задайте для базового класса `TabController` значение `UITabBarController`. 
1. Создайте экземпляры `UIViewController` для добавления в `TabController`. 
1. Добавьте `UIViewController` экземпляры в массив, назначенный свойству `ViewControllers` `TabController`. 

Добавьте следующий код в класс `TabController` для достижения следующих шагов:

```csharp
using System;
using UIKit;

namespace TabbedApplication {
    public class TabController : UITabBarController {

        UIViewController tab1, tab2, tab3;

        public TabController ()
        {
            tab1 = new UIViewController();
            tab1.Title = "Green";
            tab1.View.BackgroundColor = UIColor.Green;

            tab2 = new UIViewController();
            tab2.Title = "Orange";
            tab2.View.BackgroundColor = UIColor.Orange;

            tab3 = new UIViewController();
            tab3.Title = "Red";
            tab3.View.BackgroundColor = UIColor.Red;

            var tabs = new UIViewController[] {
                tab1, tab2, tab3
            };

            ViewControllers = tabs;
        }
    }
}
```

Обратите внимание, что для каждого экземпляра `UIViewController` мы устанавливаем свойство `Title` `UIViewController`. При добавлении контроллеров в `UITabBarController``UITabBarController` будет считывать `Title` для каждого контроллера и отображать их на соответствующей метке вкладки, как показано ниже:

[![](creating-tabbed-applications-images/00-app.png "The sample app run")](creating-tabbed-applications-images/00-app.png#lightbox)

#### <a name="setting-the-tabcontroller-as-the-rootviewcontroller"></a>Задание Табконтроллер в качестве Рутвиевконтроллер

Порядок размещения контроллеров на вкладках соответствует порядку их добавления в массив `ViewControllers`.

Чтобы загрузить `UITabController` в качестве первого экрана, необходимо сделать его `RootViewController`ом окна, как показано в следующем коде для `AppDelegate`:

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
    UIWindow window;
    TabController tabController;
    
    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        window = new UIWindow (UIScreen.MainScreen.Bounds);
        
        tabController = new TabController ();
        window.RootViewController = tabController;
        
        window.MakeKeyAndVisible ();
        
        return true;
    }
}
```

Если запустить приложение сейчас, `UITabBarController` будет загружаться с первой вкладкой, выбранной по умолчанию. Выбор любой из других вкладок приводит к отображению представления связанного контроллера, представленного `UITabBarController,`, как показано ниже, где конечный пользователь выбрал вторую вкладку:

[![](creating-tabbed-applications-images/03-secondtab.png "The second tab shown")](creating-tabbed-applications-images/03-secondtab.png#lightbox)

 <a name="Modifying_TabBarItems" />

### <a name="modifying-tabbaritems"></a>Изменение Таббаритемс

Теперь, когда у нас есть приложение для работы с вкладками, измените `TabBarItem`, чтобы изменить отображаемое изображение и текст, а также добавить значок на одну из вкладок.

 <a name="Setting_a_System_Item" />

#### <a name="setting-a-system-item"></a>Задание системного элемента

Во-первых, давайте настроим первую вкладку на использование системного элемента. В конструкторе `TabController`удалите строку, которая задает `Title` контроллера для экземпляра `tab1`, и замените ее следующим кодом, чтобы задать свойство `TabBarItem` контроллера:

```csharp
tab1.TabBarItem = new UITabBarItem (UITabBarSystemItem.Favorites, 0);
```

При создании `UITabBarItem` с помощью `UITabBarSystemItem`заголовок и изображение предоставляются автоматически iOS, как показано на снимке экрана ниже, где показаны значок и заголовок элемента **"Избранное"** на первой вкладке:

 ![](creating-tabbed-applications-images/04a-tabimage.png "The first tab with a star icon")

 <a name="Setting_the_Title_and_Image" />

#### <a name="setting-the-title-and-image"></a>Задание заголовка и изображения

В дополнение к использованию системного элемента в качестве заголовка и изображения `UITabBarItem` можно задать пользовательские значения. Например, измените код, задающий свойство `TabBarItem` контроллера с именем `tab2` следующим образом:

```csharp
tab2 = new UIViewController ();
tab2.TabBarItem = new UITabBarItem ();
tab2.TabBarItem.Image = UIImage.FromFile ("second.png");
tab2.TabBarItem.Title = "Second";
tab2.View.BackgroundColor = UIColor.Orange;
```

В приведенном выше коде предполагается, что в корне проекта в Visual Studio для Mac был добавлен образ с именем `second.png`. Мы фактически добавили в наш проект три изображения, чтобы охватить все разрешения устройств, как показано ниже:

 [![](creating-tabbed-applications-images/tabbedimages7new.png "The images added to the project")](creating-tabbed-applications-images/tabbedimages7new.png#lightbox)

Изображение вкладки должно быть 30x30 PNG с прозрачностью для обычного разрешения, 60 x 60 для высокого разрешения и 90 x 90 для iPhone 6 Plus. В нашем коде нужно загрузить только файл с именем `second.png` и iOS автоматически загрузит высокое разрешение на устройствах с Retina дисплеем. Дополнительные сведения см. в руководствах по [работе с изображениями](~/ios/app-fundamentals/images-icons/index.md) . По умолчанию элементы панели вкладок окрашены серым цветом, при выборе которых выделяется синий цвет.

**Примечание**

Приведенные выше образы также могут быть добавлены в каталог **Resources** , который представляет собой специальный каталог, содержимое которого автоматически копируется в корневую папку пакета приложений:

[![](creating-tabbed-applications-images/tabbedapplication8.png "The images as Resources")](creating-tabbed-applications-images/tabbedapplication8.png#lightbox)

Кроме того, когда мы устанавливаем свойство `Title` непосредственно в `TabBarItem`, оно переопределит любое значение, установленное для `Title` на самом контроллере.

При запуске приложения на второй вкладке отображается пользовательский заголовок и изображение, как показано ниже:

[![](creating-tabbed-applications-images/05-customtab.png "The second tab with a square icon")](creating-tabbed-applications-images/05-customtab.png#lightbox)

 <a name="Setting_the_Badge_Value" />

#### <a name="setting-the-badge-value"></a>Задание значения эмблемы

На вкладке также может отображаться значок. Например, добавьте следующую строку кода, чтобы задать значок на третьей вкладке:

```csharp
tab3.TabBarItem.BadgeValue = "Hi";
```

При выполнении этой команды в левом верхнем углу вкладки, как показано ниже, отображается красная метка со строкой "Привет".

[![](creating-tabbed-applications-images/06-badge.png "The second tab with a Hi badge")](creating-tabbed-applications-images/06-badge.png#lightbox)

Эмблема часто используется для отображения числа непрочтенных, новых элементов. Чтобы удалить значок, задайте для `BadgeValue` значение null, как показано ниже:

```csharp
tab3.TabBarItem.BadgeValue = null;
```

 <a name="Tabs_in_Non-RootViewController_Scenarios" />

## <a name="tabs-in-non-rootviewcontroller-scenarios"></a>Вкладки в сценариях, не Рутвиевконтроллер

В приведенном выше примере мы показали, как работать с `UITabBarController`, когда это `RootViewController` окна. В этом примере мы рассмотрим, как использовать `UITabBarController`, если это не `RootViewController` и покажу, как это было создано, используя раскадровки.

 <a name="Initial_Screen_Example" />

### <a name="initial-screen-example"></a>Пример начального экрана

В этом сценарии начальный экран загружается из контроллера, который не является `UITabBarController`. Когда пользователь взаимодействует с экраном, коснувшись кнопки, тот же контроллер представления загружается в `UITabBarController`, который затем предоставляется пользователю. На следующем снимке экрана показан поток приложения:

[![](creating-tabbed-applications-images/inital-screen-application.png "This screenshot shows the application flow")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)

Давайте начнем новое приложение для этого примера. Опять же, мы будем использовать шаблон **iPhone > App > Empty ProjectC#()** , на этот раз наименовать проект`InitialScreenDemo`.

В этом примере для хранения наших контроллеров представления потребуется раскадровка. Чтобы добавить раскадровку, сделайте следующее:

- Щелкните правой кнопкой мыши имя проекта и выберите **добавить > новый файл**.

- Когда появится диалоговое окно Создание файла, перейдите к **iOS > пустая раскадровка iPhone**.

Давайте назовем этот новый **файл mainstoryboard** раскадровки, как показано ниже: 

[![](creating-tabbed-applications-images/new-file-dialog.png "Add a MainStoryboard file to the project")](creating-tabbed-applications-images/new-file-dialog.png#lightbox)

Есть несколько важных шагов, которые необходимо учитывать при добавлении раскадровки в ранее не раскадровый файл, который рассматривается в руководстве [Общие сведения о раскадровках](~/ios/user-interface/storyboards/index.md) . Эти особые значения приведены ниже.

1. Добавьте имя раскадровки в раздел " **основной интерфейс** " `Info.plist`:

    [![](creating-tabbed-applications-images/project-options.png "Set the Main Interface to MainStoryboard")](creating-tabbed-applications-images/project-options.png#lightbox)
1. В `App Delegate`Переопределите метод Window, используя следующий код:

    ```csharp
    public override UIWindow Window {
        get;
        set;
    }
    ```

В этом примере нам потребуется три контроллера представления. Один из них, именуемый `ViewController1`, будет использоваться как первый контроллер представления и на первой вкладке. Два других, именуемые `ViewController2` и `ViewController3`, которые будут использоваться на второй и третьей вкладках соответственно.

Откройте конструктор, дважды щелкнув файл файл mainstoryboard. Storyboard, и перетащите три контроллера представления в область конструктора. Мы хотим, чтобы каждый из этих контроллеров представлений имел свой собственный класс, соответствующий приведенному выше имени, поэтому в поле **удостоверение > класс**введите имя, как показано на снимке экрана ниже:

[![](creating-tabbed-applications-images/class-name.png "Set the Class to ViewController1")](creating-tabbed-applications-images/class-name.png#lightbox)

Visual Studio для Mac автоматически создаст необходимые классы и файлы конструктора, это можно увидеть в Панель решения, как показано ниже:

[![](creating-tabbed-applications-images/solution-pad2.png "Auto-generated files in the project")](creating-tabbed-applications-images/solution-pad2.png#lightbox)

 <a name="Creating_the_UI" />

#### <a name="creating-the-ui"></a>Создание пользовательского интерфейса

Далее мы создадим простой пользовательский интерфейс для каждого из представлений ViewController с помощью Xamarin iOS Designer.

Нам нужно перетащить `Label` и `Button` на ViewController1 из **области элементов** в правой части. Далее мы будем использовать Панель свойств, чтобы изменить имя и текст элементов управления следующим образом:

- **Метка** : `Text` = **один**
- **Кнопка** : `Title` = **пользователь выполняет некоторое начальное действие**

Мы будем контролировать видимость нашей кнопки в `TouchUpInside` событии, и нам нужно будет ссылаться на него в коде программной части. Давайте обйдем его с **именем** `aButton` в панель свойств, как показано на следующем снимке экрана:

[![](creating-tabbed-applications-images/abutton-properties.png "Set the Name to aButton in the Properties Pad")](creating-tabbed-applications-images/abutton-properties.png#lightbox)

Теперь область конструктора должны выглядеть примерно так, как показано на снимке экрана ниже:

[![](creating-tabbed-applications-images/design-surface1.png "Your Design Surface should now look similar to this screenshot")](creating-tabbed-applications-images/design-surface1.png#lightbox)

Давайте добавим немного более подробно к `ViewController2` и `ViewController3`, добавив метку к каждому из них и изменив текст на "два" и "три" соответственно. Это подчеркивает пользователя, на котором мы рассматриваете эту вкладку или представление.

#### <a name="wiring-up-the-button"></a>Накладка кнопки

Мы собираемся загрузить `ViewController1` при первом запуске приложения. Когда пользователь нажмет кнопку, кнопка будет скрыта и загружена `UITabBarController` с экземпляром `ViewController1` на первой вкладке.

Когда пользователь отпускает `aButton`, необходимо активировать событие Таучупинсиде. Настроим кнопку, а на **вкладке события** панели Свойства Объявите обработчик событий — `InitialActionCompleted` — так что на него можно будет ссылаться в коде. Это показано на следующем снимке экрана:

[![](creating-tabbed-applications-images/event-handler.png "When the user releases the aButton, trigger a TouchUpInside event")](creating-tabbed-applications-images/event-handler.png#lightbox)

Теперь необходимо сообщить контроллеру представления о необходимости скрыть кнопку, когда событие срабатывает `InitialActionCompleted`. В `ViewController1`добавьте следующий разделяемый метод:

```csharp
partial void InitialActionCompleted (UIButton sender)
{
    aButton.Hidden = true;  
}
```

Сохраните файл и запустите приложение. Должен отобразиться экран, а кнопка исчезнет.

#### <a name="adding-the-tab-bar-controller"></a>Добавление контроллера панели вкладок

Теперь изначальное представление работает правильно. Затем мы хотим добавить его в `UITabBarController`, а также представления 2 и 3. Откроем раскадровку в конструкторе.

На **панели элементов**найдите **контроллер панели вкладок** в разделе Controllers & Objects и перетащите его на область конструктора. Как видно на снимке экрана ниже, контроллер панели вкладок не является интерфейсом пользователя и поэтому по умолчанию открывает два контроллера представления:

[![](creating-tabbed-applications-images/tabbarcontroller.png "Adding a Tab Bar Controller to the layout")](creating-tabbed-applications-images/tabbarcontroller.png#lightbox)

Удалите эти новые контроллеры представлений, щелкнув черную полосу внизу и нажав клавишу DELETE.

В нашей раскадровке мы можем использовать переходов для управления переходами между Таббарконтроллер и нашими контроллерами представления. После взаимодействия с первоначальным представлением мы хотим загрузить его в Таббарконтроллер, представляемый пользователю. Давайте настроим это в конструкторе.

**Удерживая нажатой клавишу CTRL** , **перетащите указатель** мыши к таббарконтроллер. При наведении указателя мыши появится контекстное меню. Мы хотим использовать модальный перехода. 

Чтобы настроить каждую из вкладок, **нажмите клавишу CTRL** для каждого из таббарконтроллер на каждом из наших контроллеров представления в порядке от одного до трех и выберите **вкладку** связь в контекстном меню, как показано ниже:

[![](creating-tabbed-applications-images/context-menu.png "Select the Tab Relationship")](creating-tabbed-applications-images/context-menu.png#lightbox)

Раскадровка должна быть похожа на снимок экрана ниже:

[![](creating-tabbed-applications-images/segue-layout.png "The Storyboard should resemble this screenshot")](creating-tabbed-applications-images/segue-layout.png#lightbox)

Если щелкнуть один из элементов панели вкладок и просмотреть панель свойств, можно увидеть несколько различных параметров, как показано ниже:

[![](creating-tabbed-applications-images/properties-panel.png "Setting the tab options in the Properties Explorer")](creating-tabbed-applications-images/properties-panel.png#lightbox)

Это можно использовать для изменения определенных атрибутов, таких как значок, заголовок и [идентификатор](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/UIKitUICatalog/TabBarItem.html)iOS, а также для других

Если мы сохранили и запустили приложение, мы найдем, что кнопка появится снова при загрузке экземпляра ViewController1 в Таббарконтроллер. Чтобы исправить это, проверьте, имеет ли текущее представление родительский контроллер представления. Если это так, мы понимаем, что мы в Таббарконтроллер, и поэтому кнопка должна быть скрыта. Добавим приведенный ниже код в класс ViewController1:

```csharp
public override void ViewDidLoad ()
{
    if (ParentViewController != null){
        aButton.Hidden = true;
    }
}
```

При запуске приложения и нажатии пользователем кнопки на первом экране загружается Уитаббарконтроллер с представлением с первого экрана, размещенного на первой вкладке, как показано ниже:

[![](creating-tabbed-applications-images/first-view.png "The sample app output")](creating-tabbed-applications-images/first-view.png#lightbox)

<!--Save the files and run the application:

[![](creating-tabbed-applications-images/inital-screen-application.png "Save the files and run the application")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)-->

## <a name="summary"></a>Сводка

В этой статье описано, как использовать `UITabBarController` в приложении. Мы рассмотрели, как загружать контроллеры на каждую вкладку, а также как задавать свойства на вкладках, таких как заголовок, изображение и эмблема. Затем мы проверили, используя раскадровки, как загрузить `UITabBarController` во время выполнения, если это не `RootViewController` окна.

## <a name="related-links"></a>Связанные ссылки

- [Создание приложений с вкладками (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/creatingtabbedapplications)
- [Images. zip](https://github.com/xamarin/ios-samples/blob/master/CreatingTabbedApplications/Resources/images.zip?raw=true)
- [Справочник по классам Уитаббарконтроллер](https://developer.apple.com/library/ios/#documentation/uikit/reference/UITabBarController_Class/Reference/Reference.html)
