---
title: Создание приложения Xamarin. iOS с помощью API элементов
description: Эта статья посвящена информации, приведенной в статье Общие сведения о диалоговом окне с ограниченным касанием. В нем представлено пошаговое руководство, в котором показано, как использовать «некасание» (MT. D). API элементов, чтобы быстро приступить к созданию приложения с помощью MT. Четырехмерного.
ms.prod: xamarin
ms.assetid: F1124734-DF44-F1F3-0832-46F52A788CDC
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: davidortinau
ms.author: daortin
ms.openlocfilehash: 2258b2e8451f896aff59c89478833976ef7086e3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73002371"
---
# <a name="creating-a-xamarinios-application-using-the-elements-api"></a>Создание приложения Xamarin. iOS с помощью API элементов

_Эта статья посвящена информации, приведенной в статье Общие сведения о диалоговом окне с ограниченным касанием. В нем представлено пошаговое руководство, в котором показано, как использовать «некасание» (MT. D). API элементов, чтобы быстро приступить к созданию приложения с помощью MT. Четырехмерного._

В этом пошаговом руководстве мы будем использовать MT. D элементов API для создания стиля приложения "основной/подробности", отображающего список задач. Когда пользователь нажимает кнопку **+** на панели навигации, в таблицу добавляется новая строка для задачи. Выбор строки приведет к переходу на экран сведений, который позволяет обновить описание задачи и дату выполнения, как показано ниже:

[![](elements-api-walkthrough-images/01-task-list-app.png "Selecting the row will navigate to the detail screen that allows us to update the task description and the due date")](elements-api-walkthrough-images/01-task-list-app.png#lightbox)

## <a name="setting-up-mtd"></a>Настройка MT. Четырехмерного

Машин. D распространяется с помощью Xamarin. iOS. Чтобы использовать его, щелкните правой кнопкой мыши узел **ссылки** проекта Xamarin. iOS в Visual Studio 2017 или Visual Studio для Mac и добавьте ссылку на сборку " **котушь. Dialog-1** ". Затем при необходимости добавьте в исходный код инструкции `using MonoTouch.Dialog`.

## <a name="elements-api-walkthrough"></a>Пошаговое руководство по API элементов

В статье [Общие сведения о диалоговом окне с ограниченным касанием](~/ios/user-interface/monotouch.dialog/index.md) мы получили полное представление о различных частях MT. Четырехмерного. Давайте будем использовать API Elements, чтобы объединить их в приложение.

## <a name="setting-up-the-multi-screen-application"></a>Настройка приложения для нескольких экранов

Чтобы начать процесс создания экрана, можно создать `DialogViewController`, а затем добавить `RootElement`.

Чтобы создать Многоэкранное приложение с помощью одноуровневого диалогового окна, необходимо выполнить следующие действия.

1. Создание `UINavigationController.`
1. Создание `DialogViewController.`
1. Добавьте `DialogViewController` в качестве корня `UINavigationController.` 
1. Добавление `RootElement` в `DialogViewController.`
1. Добавление `Sections` и `Elements` в `RootElement.` 

### <a name="using-a-uinavigationcontroller"></a>Использование Уинавигатионконтроллер

Чтобы создать приложение в стиле навигации, необходимо создать `UINavigationController`, а затем добавить его в качестве `RootViewController` в метод `FinishedLaunching` `AppDelegate`. Чтобы сделать `UINavigationController` работать с помощью элемента управления "некасание", добавьте `DialogViewController` в `UINavigationController`, как показано ниже:

```csharp
public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{
    _window = new UIWindow (UIScreen.MainScreen.Bounds);
            
    _rootElement = new RootElement ("To Do List"){new Section ()};

    // code to create screens with MT.D will go here …

    _rootVC = new DialogViewController (_rootElement);
    _nav = new UINavigationController (_rootVC);
    _window.RootViewController = _nav;
    _window.MakeKeyAndVisible ();
            
    return true;
}
```

Приведенный выше код создает экземпляр `RootElement` и передает его в `DialogViewController`. `DialogViewController` всегда имеет `RootElement` в верхней части иерархии. В этом примере `RootElement` создается со строкой «Do List», которая служит заголовком на панели навигации контроллера навигации. На этом этапе при запуске приложения появится экран, показанный ниже:

 [![](elements-api-walkthrough-images/02-to-do-list-screen-.png "Running the application will present the screen shown here")](elements-api-walkthrough-images/02-to-do-list-screen-.png#lightbox)

Давайте посмотрим, как использовать иерархическую структуру диалогового окна `Sections` и `Elements`, чтобы добавить дополнительные экраны.

### <a name="creating-the-dialog-screens"></a>Создание экранов диалогового окна

`DialogViewController` — это `UITableViewController` подкласс, который используется для добавления экранов в диалоговое окно. В диалоговом окне «однокасание.» создаются экраны путем добавления `RootElement` к `DialogViewController`, как было показано выше. `RootElement` может иметь `Section` экземпляров, представляющих разделы таблицы.
Разделы состоят из элементов, других разделов или даже других `RootElements`. Вложение `RootElements`, позволяет автоматически создать приложение в стиле навигации, как будет показано далее.

### <a name="using-dialogviewcontroller"></a>Использование Диалогвиевконтроллер

`DialogViewController`, который является под`UITableViewController`ным подклассом, имеет `UITableView` в качестве представления. В этом примере мы хотим добавлять элементы в таблицу каждый раз при нажатии кнопки **+** . Так как `DialogViewController` был добавлен в `UINavigationController`, можно использовать свойство `RightBarButton` `NavigationItem`, чтобы добавить кнопку **+** , как показано ниже:

```csharp
_addButton = new UIBarButtonItem (UIBarButtonSystemItem.Add);
_rootVC.NavigationItem.RightBarButtonItem = _addButton;
```

Когда мы создали `RootElement` ранее, мы передали ему единственный экземпляр `Section`, чтобы мы могли добавлять элементы при нажатии пользователем кнопки **+** . Чтобы сделать это в обработчике событий для кнопки, можно использовать следующий код:

```csharp
_addButton.Clicked += (sender, e) => {                
    ++n;
                
    var task = new Task{Name = "task " + n, DueDate = DateTime.Now};
                
    var taskElement = new RootElement (task.Name) {
        new Section () {
            new EntryElement (task.Name, "Enter task description", task.Description)
        },
        new Section () {
            new DateElement ("Due Date", task.DueDate)
        }
    };
    _rootElement [0].Add (taskElement);
};
```

Этот код создает новый объект `Task` каждый раз при нажатии кнопки. Ниже показана простая реализация класса `Task`.

```csharp
public class Task
{   
    public Task ()
    {
    }
      
    public string Name { get; set; }
        
    public string Description { get; set; }

    public DateTime DueDate { get; set; }
}
```

Свойство `Name` задачи используется для создания заголовка `RootElement`вместе с переменной счетчика с именем `n`, увеличивающейся для каждой новой задачи. Однокасание. диалоговое окно преобразует элементы в строки, добавляемые к `TableView` при добавлении каждой `taskElement`.

## <a name="presenting-and-managing-dialog-screens"></a>Представление экранов диалоговых окон и управление ими

Мы использовали `RootElement`, чтобы в диалоговом окне было автоматически создано новое окно для сведений о каждой задаче и переходить к ней при выборе строки.

Экран сведений о задаче состоит из двух разделов. Каждый из этих разделов содержит один элемент. Первый элемент создается из `EntryElement` для предоставления редактируемой строки для свойства `Description` задачи. При выборе элемента клавиатура для редактирования текста представлена, как показано ниже:

 [![](elements-api-walkthrough-images/03-create-task.png "When the element is selected, a keyboard for text editing is presented as shown")](elements-api-walkthrough-images/03-create-task.png#lightbox)

Во втором разделе содержится `DateElement`, позволяющий управлять свойством `DueDate` задачи. При выборе даты автоматически загружается средство выбора даты, как показано ниже.

 [![](elements-api-walkthrough-images/04-date-picker.png "Selecting the date automatically loads a date picker as")](elements-api-walkthrough-images/04-date-picker.png#lightbox)

В `EntryElement` и `DateElement` вариантах (или для любого элемента ввода данных в виде однокасания. диалоговое окно) все изменения значений сохраняются автоматически. Это можно продемонстрировать, изменив дату, а затем перейдя между корневым экраном и различными сведениями о задачах, где значения на экранах сведений сохраняются.

## <a name="summary"></a>Сводка

В этой статье представлено пошаговое руководство, в котором показано, как использовать API элементов с элементами управления "только для касания". В нем описаны основные шаги по созданию многоэкранного приложения с помощью MT. D, включая использование `DialogViewController` и добавление элементов и разделов для создания экранов. Кроме того, в нем было показано, как использовать MT. D в сочетании с `UINavigationController`.

## <a name="related-links"></a>Связанные ссылки

- [Мтдвалксраугх (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/mtdwalkthrough)
- [Введение в бескасание. Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Пошаговое руководство по API отражения](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Пошаговое руководство по элементу JSON](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [Диалоговое окно с несенсорным касанием на GitHub](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [Приложение Твитстатион](https://github.com/migueldeicaza/TweetStation)
- [Справочник по классам Уитаблевиевконтроллер](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Справочник по классам Уинавигатионконтроллер](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
