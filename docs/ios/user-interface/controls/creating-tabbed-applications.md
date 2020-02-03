---
title: Панели вкладки панели вкладок и контроллеры в Xamarin.iOS
description: В этом документе описываются iOS вкладку панели контроллеров и способы их использования с Xamarin.iOS. Показывается, как настроить UITabBarController, работать с изображениями, задайте значения эмблемы, работа с событиями и многое другое.
ms.prod: xamarin
ms.assetid: 7C772899-2900-F139-D642-F3C4F3F14DDC
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 25d8563288cce614bc2823b0146e5121688c6f02
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725484"
---
# <a name="tab-bars-and-tab-bar-controllers-in-xamarinios"></a>Панели вкладок и контроллеры панели вкладок в Xamarin. iOS

В iOS используются с вкладками приложения для поддержки пользовательских интерфейсов, где может быть доступно несколько экранов в произвольном порядке. С помощью класса `UITabBarController` приложения могут легко включать поддержку таких сценариев с несколькими экранами. `UITabBarController` осуществляет управление несколькими экранами, позволяя разработчику приложения сосредоточиться на деталях каждого экрана.

Как правило, приложения с вкладками создаются с `UITabBarController` `RootViewController` главного окна. Тем не менее применив немного дополнительного кода, с вкладками приложения также можно последовательно, чтобы некоторые другие начальный экран, подобная ситуация, где приложение сначала появится экран входа, следуют интерфейс с вкладками.

На этой странице обсуждаются оба сценария: Если вкладки находятся в корне иерархии представления приложения, а также в сценарии, не`RootViewController`.

## <a name="introducing-uitabbarcontroller"></a>Знакомство с UITabBarController

`UITabBarController` поддерживает разработку приложений с вкладками следующим образом:

- Разрешение нескольких контроллеров для добавления к нему.
- Предоставление пользовательского интерфейса с вкладками через класс `UITabBar`, позволяющий пользователю переключаться между контроллерами и их представлениями.

Контроллеры добавляются в `UITabBarController` через свойство `ViewControllers`, которое является массивом `UIViewController`. `UITabBarController` сам обрабатывает загрузку соответствующего контроллера и представление его представления на основе выбранной вкладки.

Вкладки являются экземплярами класса `UITabBarItem`, которые содержатся в экземпляре `UITabBar`. Каждый экземпляр `UITabBar` доступен через свойство `TabBarItem` контроллера на каждой вкладке.

Чтобы понять, как работать с `UITabBarController`, давайте рассмотрим создание простого приложения, использующего его.

## <a name="tabbed-application-walkthrough"></a>Пошаговое руководство по приложению с вкладками

В этом пошаговом руководстве мы создадим следующие приложения:

[![пример приложения с вкладками](creating-tabbed-applications-images/00-app.png)](creating-tabbed-applications-images/00-app.png#lightbox)

Хотя в Visual Studio для Mac уже есть шаблон приложения с вкладками, в этом примере эти инструкции работают из пустого проекта, чтобы лучше понять, как создается приложение.

### <a name="creating-the-application"></a>Создание приложения

Начните с создания нового приложения.

Выберите пункт меню **файл > создать > решение** в Visual Studio для Mac и выберите **приложение > iOS > пустой шаблон проекта** , назовите проект `TabbedApplication`, как показано ниже:

[![](creating-tabbed-applications-images/newsolution1.png "Select the Empty Project template")](creating-tabbed-applications-images/newsolution1.png#lightbox)

[![](creating-tabbed-applications-images/newsolution2.png "Name the project TabbedApplication")](creating-tabbed-applications-images/newsolution2.png#lightbox)

### <a name="adding-the-uitabbarcontroller"></a>Добавление UITabBarController

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

#### <a name="setting-the-tabcontroller-as-the-rootviewcontroller"></a>Установка TabController RootViewController

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

![Вторая показанная вкладка](creating-tabbed-applications-images/03-secondtab-sml.png)

### <a name="modifying-tabbaritems"></a>Изменение TabBarItems

Теперь, когда у нас есть приложение для работы с вкладками, измените `TabBarItem`, чтобы изменить отображаемое изображение и текст, а также добавить значок на одну из вкладок.

#### <a name="setting-a-system-item"></a>Задание системного элемента

Во-первых давайте установим первую вкладку, чтобы использовать элемент "система". В конструкторе `TabController`удалите строку, которая задает `Title` контроллера для экземпляра `tab1`, и замените ее следующим кодом, чтобы задать свойство `TabBarItem` контроллера:

```csharp
tab1.TabBarItem = new UITabBarItem (UITabBarSystemItem.Favorites, 0);
```

При создании `UITabBarItem` с помощью `UITabBarSystemItem`заголовок и изображение предоставляются автоматически iOS, как показано на снимке экрана ниже, где показаны значок и заголовок элемента **"Избранное"** на первой вкладке:

 ![Первая вкладка со значком звездочки](creating-tabbed-applications-images/04a-tabimage-sml.png)

#### <a name="setting-the-image"></a>Настройка образа

В дополнение к использованию системного элемента в качестве заголовка и изображения `UITabBarItem` можно задать пользовательские значения. Например, измените код, задающий свойство `TabBarItem` контроллера с именем `tab2` следующим образом:

```csharp
tab2 = new UIViewController ();
tab2.TabBarItem = new UITabBarItem ();
tab2.TabBarItem.Image = UIImage.FromFile ("second.png");
tab2.TabBarItem.Title = "Second";
tab2.View.BackgroundColor = UIColor.Orange;
```

В приведенном выше коде предполагается, что в корневую папку проекта (или в каталоге **ресурсов** ) добавлен образ с именем `second.png`. Для поддержки всех плотий экрана требуются три изображения, как показано ниже:

![Изображения, добавленные в проект](creating-tabbed-applications-images/tabbedimages7new.png)

Инструкции по правильным измерениям см. в разделе **Размер значка панели вкладок** на [странице пользовательских значков Apple](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/custom-icons/) . Рекомендуемый размер зависит от стиля изображения (круглая, квадратная, широкая или высота).

Свойству `Image` необходимо задать только **второе имя PNG** , IOS автоматически загрузит файлы с более высоким разрешением при необходимости. Дополнительные сведения см. в руководствах по [работе с изображениями](~/ios/app-fundamentals/images-icons/index.md) . По умолчанию элементов этой панели, серый значок с синей оттенок, при выборе.

#### <a name="overriding-the-title"></a>Переопределение заголовка

Если свойство `Title` задано непосредственно в `TabBarItem`, оно переопределит любое значение, установленное для `Title` на самом контроллере.

На второй вкладке (средняя) на этом снимке экрана показан пользовательский заголовок и изображение:

![Вторая вкладка с квадратным значком](creating-tabbed-applications-images/05-customtab-sml.png)

#### <a name="setting-the-badge-value"></a>Задание значения эмблемы

Вкладки также можно отобразить индикатор событий. Например добавьте следующую строку кода, чтобы задать эмблему на третью вкладку:

```csharp
tab3.TabBarItem.BadgeValue = "Hi";
```

Под управлением, это приведет к красная метка со строки «Hi» в верхнем левом углу вкладки, как показано ниже:

![Вторая вкладка с эмблемой Hi](creating-tabbed-applications-images/06-badge-sml.png)

Эмблема часто используется для отображения чисел значение, указывающее непрочитанные, новые элементы. Чтобы удалить значок, задайте для `BadgeValue` значение null, как показано ниже:

```csharp
tab3.TabBarItem.BadgeValue = null;
```

## <a name="tabs-in-non-rootviewcontroller-scenarios"></a>Вкладки в сценариях, не Рутвиевконтроллер

В приведенном выше примере мы показали, как работать с `UITabBarController`, когда это `RootViewController` окна. В этом примере мы рассмотрим, как использовать `UITabBarController`, если это не `RootViewController` и покажу, как это было создано, используя раскадровки.

### <a name="initial-screen-example"></a>Пример начального экрана

В этом сценарии начальный экран загружается из контроллера, который не является `UITabBarController`. Когда пользователь взаимодействует с экраном, коснувшись кнопки, тот же контроллер представления загружается в `UITabBarController`, который затем предоставляется пользователю. На следующем снимке экрана показана блок-схема приложения.

[![](creating-tabbed-applications-images/inital-screen-application.png "This screenshot shows the application flow")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)

Начнем с нового приложения в этом примере. Опять же, мы будем использовать шаблон **iPhone > App > Empty ProjectC#()** , на этот раз наименовать проект `InitialScreenDemo`.

В этом примере раскадровка используется для размещения контроллеров представления. Чтобы добавить раскадровку, сделайте следующее:

- Щелкните правой кнопкой мыши имя проекта и выберите **добавить > новый файл**.

- Когда появится диалоговое окно Создание файла, перейдите к **iOS > пустая раскадровка iPhone**.

Давайте назовем этот новый **файл mainstoryboard** раскадровки, как показано ниже:

[![](creating-tabbed-applications-images/new-file-dialog.png "Add a MainStoryboard file to the project")](creating-tabbed-applications-images/new-file-dialog.png#lightbox)

Есть несколько важных шагов, которые необходимо учитывать при добавлении раскадровки в ранее не раскадровый файл, который рассматривается в руководстве [Общие сведения о раскадровках](~/ios/user-interface/storyboards/index.md) . а именно:

1. Добавьте имя раскадровки в раздел " **основной интерфейс** " `Info.plist`:

    [![](creating-tabbed-applications-images/project-options.png "Set the Main Interface to MainStoryboard")](creating-tabbed-applications-images/project-options.png#lightbox)
1. В `App Delegate`Переопределите метод Window, используя следующий код:

    ```csharp
    public override UIWindow Window {
        get;
        set;
    }
    ```

Мы собираемся требуется три контроллера представления для этого примера. Один из них, именуемый `ViewController1`, будет использоваться как первый контроллер представления и на первой вкладке. Два других, именуемые `ViewController2` и `ViewController3`, которые будут использоваться на второй и третьей вкладках соответственно.

Откройте конструктор, дважды щелкнув файл MainStoryboard.storyboard и перетащите три контроллера представления в область конструктора. Мы хотим, чтобы каждый из этих контроллеров представлений имел свой собственный класс, соответствующий приведенному выше имени, поэтому в поле **удостоверение > класс**введите имя, как показано на снимке экрана ниже:

[![](creating-tabbed-applications-images/class-name.png "Set the Class to ViewController1")](creating-tabbed-applications-images/class-name.png#lightbox)

Visual Studio для Mac автоматически создает классы и необходимые файлы конструктора, это можно увидеть на панели решения, как показано ниже:

[![](creating-tabbed-applications-images/solution-pad2.png "Auto-generated files in the project")](creating-tabbed-applications-images/solution-pad2.png#lightbox)

#### <a name="creating-the-ui"></a>Создание пользовательского интерфейса

Далее мы создадим простой пользовательский интерфейс для каждого из представлений ViewController использовании конструктора iOS Xamarin.

Нам нужно перетащить `Label` и `Button` на ViewController1 из **области элементов** в правой части. Далее при помощи панели свойств для редактирования, именем и текстом элементов управления следующего:

- **Метка** : `Text` = **один**
- **Кнопка** : `Title` = **пользователь выполняет некоторое начальное действие**

Мы будем контролировать видимость нашей кнопки в `TouchUpInside` событии, и нам нужно будет ссылаться на него в коде программной части. Давайте обйдем его с **именем** `aButton` в панель свойств, как показано на следующем снимке экрана:

[![](creating-tabbed-applications-images/abutton-properties.png "Set the Name to aButton in the Properties Pad")](creating-tabbed-applications-images/abutton-properties.png#lightbox)

Поверхность конструктора теперь должен выглядеть как на снимке экрана ниже:

[![](creating-tabbed-applications-images/design-surface1.png "Your Design Surface should now look similar to this screenshot")](creating-tabbed-applications-images/design-surface1.png#lightbox)

Давайте добавим немного более подробно к `ViewController2` и `ViewController3`, добавив метку к каждому из них и изменив текст на "два" и "три" соответственно. Это подчеркивает пользователь какие вкладки или представления, мы рассматриваем.

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

Сохраните файл и запустить приложение. Мы должны увидеть отображается один экран и кнопку решаются при Touch вверх.

#### <a name="adding-the-tab-bar-controller"></a>Добавление контроллера панели вкладок

Теперь у нас есть нашего начального представления, работает должным образом. Затем мы хотим добавить его в `UITabBarController`, а также представления 2 и 3. Давайте откройте раскадровку, в конструкторе.

На **панели элементов**найдите **контроллер панели вкладок** в разделе Controllers & Objects и перетащите его на область конструктора. Как показано на следующем снимке экрана, контроллер панели вкладок является без интерфейса пользователя и таким образом, дает двумя контроллерами представлений с ним по умолчанию:

[![](creating-tabbed-applications-images/tabbarcontroller.png "Adding a Tab Bar Controller to the layout")](creating-tabbed-applications-images/tabbarcontroller.png#lightbox)

Удалите эти новые контроллеры представления, выбрав черную полосу в нижней и нажав клавишу delete.

В нашем раскадровке мы используем Segues обрабатывать переходы между TabBarController и наши контроллеров представлений. После взаимодействия с исходное представление, мы хотим загрузить их в TabBarController, представленные пользователю. Давайте настроим это в конструкторе.

**Удерживая нажатой клавишу CTRL** , **перетащите указатель** мыши к таббарконтроллер. На доступ к мыши появится контекстное меню. Мы хотим использовать модального перехода.

Чтобы настроить каждую из вкладок, **нажмите клавишу CTRL** для каждого из таббарконтроллер на каждом из наших контроллеров представления в порядке от одного до трех и выберите **вкладку** связь в контекстном меню, как показано ниже:

[![](creating-tabbed-applications-images/context-menu.png "Select the Tab Relationship")](creating-tabbed-applications-images/context-menu.png#lightbox)

Раскадровка должна выглядеть так на снимке экрана ниже:

[![](creating-tabbed-applications-images/segue-layout.png "The Storyboard should resemble this screenshot")](creating-tabbed-applications-images/segue-layout.png#lightbox)

Если мы щелкните один из элементов этой панели и изучите панели «свойства», вы увидите несколько различных вариантов, как показано ниже:

[![](creating-tabbed-applications-images/properties-panel.png "Setting the tab options in the Properties Explorer")](creating-tabbed-applications-images/properties-panel.png#lightbox)

Это можно использовать для изменения определенных атрибутов, таких как эмблема, заголовок и идентификатор iOS, а также других.

Если сохранить и запустить приложение сейчас, очень скоро найдется что кнопки появится снова при загрузке экземпляра ViewController1 в TabBarController. Исправим это путем проверки на наличие родительского контроллера представления для текущего представления. Если Да, мы знаем, мы внутри TabBarController, и поэтому должны быть скрыты кнопки. Давайте добавим приведенный ниже код к классу ViewController1:

```csharp
public override void ViewDidLoad ()
{
    if (ParentViewController != null){
        aButton.Hidden = true;
    }
}
```

Загружается после запуска приложения и пользователь нажимает кнопку на первом экране UITabBarController, с помощью представления на первом экране помещены на первой вкладке, как показано ниже:

[![выходных данных примера приложения](creating-tabbed-applications-images/first-view-sml.png)](creating-tabbed-applications-images/first-view.png#lightbox)

## <a name="summary"></a>Сводка

В этой статье описано, как использовать `UITabBarController` в приложении. Мы рассмотрели способ загрузки контроллеров в каждой вкладке, а также настройке свойств на вкладках такие названия, изображения и эмблемы. Затем мы проверили, используя раскадровки, как загрузить `UITabBarController` во время выполнения, если это не `RootViewController` окна.

## <a name="related-links"></a>Связанные ссылки

- [Создание приложений с вкладками (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/creatingtabbedapplications)
- [Images. zip](https://github.com/xamarin/ios-samples/blob/master/CreatingTabbedApplications/Resources/images.zip?raw=true)
- [Справочник по классам Уитаббарконтроллер](https://developer.apple.com/library/ios/#documentation/uikit/reference/UITabBarController_Class/Reference/Reference.html)
