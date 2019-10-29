---
title: Создание приложения Xamarin. iOS с помощью API отражения
description: В этом документе описан API-интерфейс отражения на основе атрибутов, который создает пользовательский интерфейс на основе классов, оформленных атрибутами.
ms.prod: xamarin
ms.assetid: C0F923D2-300E-DB9D-F390-9FA71B22DFD6
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: davidortinau
ms.author: daortin
ms.openlocfilehash: 323b92190dc3ea18bc78871f5c19e51d0a6ea94e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73002206"
---
# <a name="creating-a-xamarinios-application-using-the-reflection-api"></a>Создание приложения Xamarin. iOS с помощью API отражения

MT. API отражения D позволяет декорированию классов с помощью атрибутов, которые MT. D использует для автоматического создания экранов. API отражения обеспечивает привязку между этими классами и то, что отображается на экране. Хотя этот API не предоставляет детализированный элемент управления, который делает API элементов, он сокращает сложность за счет автоматического создания иерархии элементов на основе декорирования классов.

## <a name="setting-up-mtd"></a>Настройка MT. Четырехмерного

Машин. D распространяется с помощью Xamarin. iOS. Чтобы использовать его, щелкните правой кнопкой мыши узел **ссылки** проекта Xamarin. iOS в Visual Studio 2017 или Visual Studio для Mac и добавьте ссылку на сборку " **котушь. Dialog-1** ". Затем при необходимости добавьте в исходный код инструкции `using MonoTouch.Dialog`.

## <a name="getting-started-with-the-reflection-api"></a>Приступая к работе с API отражения

Использовать API отражения очень просто:

1. Создание класса, снабженного MT. Атрибуты D.
1. Создание экземпляра `BindingContext`, передавая ему экземпляр указанного выше класса. 
1. Создание `DialogViewController` с передачей `RootElement` `BindingContext’s`. 

Рассмотрим пример, иллюстрирующий использование API отражения. В этом примере мы создадим простой экран ввода данных, как показано ниже:

 [![](reflection-api-walkthrough-images/01-expense-entry.png "In this example, we'll build a simple data entry screen as shown here")](reflection-api-walkthrough-images/01-expense-entry.png#lightbox)

## <a name="creating-a-class-with-mtd-attributes"></a>Создание класса с помощью MT. Атрибуты D

В первую очередь нам нужно использовать API отражения — это класс, дополненный атрибутами. Эти атрибуты будут использоваться MT. D — внутренне создание объектов из API элементов. Например, рассмотрим следующее определение класса:

```csharp
public class Expense
{
    [Section("Expense Entry")]

    [Entry("Enter expense name")]
    public string Name;
        
    [Section("Expense Details")]
  
    [Caption("Description")]
    [Entry]
    public string Details;
        
    [Checkbox]
    public bool IsApproved = true;
}
```

`SectionAttribute` приведет к созданию разделов создаваемого `UITableView` со строковым аргументом, используемым для заполнения заголовка раздела. После объявления раздела все поля, следующие за ним, будут включаться в этот раздел, пока не будет объявлен другой раздел.
Тип элемента пользовательского интерфейса, созданного для поля, будет зависеть от типа поля и MT. Атрибут D.

Например, `Name` поле является `string` и дополнено `EntryAttribute`. Это приводит к добавлению строки в таблицу с полем ввода текста и указанным заголовком. Аналогичным образом поле `IsApproved` — это `bool` с `CheckboxAttribute`, в результате чего строка таблицы с флажком справа от ячейки таблицы. Машин. D использует имя поля, автоматически добавляя пробел, чтобы создать заголовок в этом случае, так как он не указан в атрибуте.

## <a name="adding-the-bindingcontext"></a>Добавление BindingContext

Чтобы использовать класс `Expense`, необходимо создать `BindingContext`. `BindingContext` — это класс, который будет привязывать данные из класса с атрибутами для создания иерархии элементов. Чтобы создать его, просто создайте экземпляр класса и передайте его в конструктор.

Например, чтобы добавить пользовательский интерфейс, объявленный с помощью атрибута в классе `Expense`, включите следующий код в метод `FinishedLaunching` `AppDelegate`:

```csharp
var expense = new Expense ();
var bctx = new BindingContext (null, expense, "Create a task");
```

Затем все, что нужно сделать для создания пользовательского интерфейса, — добавить `BindingContext` в `DialogViewController` и задать его в качестве `RootViewController` окна, как показано ниже:

```csharp
UIWindow window;

public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{   
    window = new UIWindow (UIScreen.MainScreen.Bounds);
            
    var expense = new Expense ();
    var bctx = new BindingContext (null, expense, "Create a task");
    var dvc = new DialogViewController (bctx.Root);
            
    window.RootViewController = dvc;
    window.MakeKeyAndVisible ();
            
    return true;
}
```

Запуск приложения теперь приводит к отображению экрана, показанного выше.

### <a name="adding-a-uinavigationcontroller"></a>Добавление Уинавигатионконтроллер

Обратите внимание, что заголовок «создание задачи», который мы передали в `BindingContext`, не отображается. Это обусловлено тем, что `DialogViewController` не является частью `UINavigatonController`. Измените код, добавив `UINavigationController` в качестве `RootViewController,` окна и добавьте `DialogViewController` в качестве корня `UINavigationController`, как показано ниже:

```csharp
nav = new UINavigationController(dvc);
window.RootViewController = nav;
```

Теперь при запуске приложения заголовок отображается на панели навигации `UINavigationController’s`, как показано ниже.

 [![](reflection-api-walkthrough-images/02-create-task.png "Now when we run the application, the title appears in the UINavigationControllers navigation bar")](reflection-api-walkthrough-images/02-create-task.png#lightbox)

Благодаря включению `UINavigationController`можно воспользоваться преимуществами других функций MT. D, для которого требуется Навигация. Например, можно добавить перечисление в класс `Expense`, чтобы определить категорию для расходов и MT. D создаст экран выбора автоматически. Чтобы продемонстрировать, измените класс `Expense`, включив в него поле `ExpenseCategory` следующим образом:

```csharp
public enum Category
{
    Travel,
    Lodging,
    Books
}
        
public class Expense
{
    …

    [Caption("Category")]
    public Category ExpenseCategory;
}
```

Выполнение приложения теперь приводит к отображению новой строки в таблице для категории, как показано ниже.

 [![](reflection-api-walkthrough-images/03-set-details.png "Running the application now results in a new row in the table for the category as shown")](reflection-api-walkthrough-images/03-set-details.png#lightbox)

Выбор строки приводит к переходу приложения на новый экран со строками, соответствующими перечислениям, как показано ниже:

 [![](reflection-api-walkthrough-images/04-set-category.png "Selecting the row results in the application navigating to a new screen with rows corresponding to the enumeration")](reflection-api-walkthrough-images/04-set-category.png#lightbox)

 <a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье представлено пошаговое руководство по API отражения. Мы показали, как добавить атрибуты в класс для управления отображаемыми сведениями. Мы также рассмотрели, как использовать `BindingContext` для привязки данных из класса к создаваемой иерархии элементов, а также как использовать MT. D с `UINavigationController`.

## <a name="related-links"></a>Связанные ссылки

- [Мтдрефлектионвалксраугх (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/mtdreflectionwalkthrough)
- [Общие сведения о диалоговом окне для бескасания](~/ios/user-interface/monotouch.dialog/index.md)
- [Пошаговое руководство по API элементов](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Пошаговое руководство по элементу JSON](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Диалоговое окно с несенсорным касанием на GitHub](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [Приложение Твитстатион](https://github.com/migueldeicaza/TweetStation)
- [Справочник по классам Уитаблевиевконтроллер](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Справочник по классам Уинавигатионконтроллер](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
