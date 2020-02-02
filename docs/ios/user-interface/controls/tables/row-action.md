---
title: Работа с действиями строк в Xamarin. iOS
description: В этом руководстве показано, как создавать пользовательские действия по считыванию строк таблицы с помощью Уисвипеактионсконфигуратион или Уитаблевиевровактион
ms.prod: xamarin
ms.assetid: 340FB633-0C46-40AA-9963-FF17D7CA6858
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/25/2017
ms.openlocfilehash: 8efa116a82ba021c2a723dc6ab636f54b6b5af71
ms.sourcegitcommit: 52fb214c0e0243587d4e9ad9306b75e92a8cc8b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2020
ms.locfileid: "76940989"
---
# <a name="working-with-row-actions-in-xamarinios"></a>Работа с действиями строк в Xamarin. iOS

_В этом руководстве показано, как создавать пользовательские действия по считыванию строк таблицы с помощью Уисвипеактионсконфигуратион или Уитаблевиевровактион_

![Демонстрация действий по считыванию строк](row-action-images/action02.png)

в iOS предусмотрены два способа выполнения действий с таблицей: `UISwipeActionsConfiguration` и `UITableViewRowAction`.

`UISwipeActionsConfiguration` была введена в iOS 11 и используется для определения набора действий, которые должны выполняться, когда пользователь перемещается _в любом направлении_ на строке в табличном представлении. Такое поведение аналогично поведению собственного mail. app.

Класс `UITableViewRowAction` используется для определения действия, которое будет выполняться при горизонтальном перемещении пользователя по строке в табличном представлении.
Например, при редактировании таблицы прокрутка влево по строке по умолчанию отображает кнопку **Удалить** . Присоединив несколько экземпляров класса `UITableViewRowAction` к `UITableView`, можно определить несколько пользовательских действий, каждое из которых имеет собственный текст, форматирование и поведение.

## <a name="uiswipeactionsconfiguration"></a>уисвипеактионсконфигуратион

Для реализации действий по считыванию с помощью `UISwipeActionsConfiguration`необходимо выполнить три шага:

1. Переопределите `GetLeadingSwipeActionsConfiguration` методы и (или) `GetTrailingSwipeActionsConfiguration`. Эти методы возвращают `UISwipeActionsConfiguration`.
2. Создайте экземпляр возвращаемого `UISwipeActionsConfiguration`. Этот класс принимает массив `UIContextualAction`.
3. Создайте `UIContextualAction`.

Они более подробно описаны в следующих разделах.

### <a name="1-implementing-the-swipeactionsconfigurations-methods"></a>1. Реализация методов Свипеактионсконфигуратионс

`UITableViewController` (а также `UITableViewSource` и `UITableViewDelegate`) содержат два метода: `GetLeadingSwipeActionsConfiguration` и `GetTrailingSwipeActionsConfiguration`, которые используются для реализации набора действий по считыванию в строке представления таблицы. Первое действие по прокрутке означает прокрутку с левой стороны экрана на языке слева направо и с правой стороны экрана на языке с письмом справа налево.

В следующем примере (из примера [таблесвипеактионс](https://docs.microsoft.com/samples/xamarin/ios-samples/tableswipeactions) ) показано, как реализовать начальную конфигурацию считывания. Из контекстных действий создаются два действия, которые описаны [ниже](#create-uicontextualaction). Затем эти действия передаются в инициализированное [`UISwipeActionsConfiguration`](#create-uiswipeactionsconfigurations), которое используется в качестве возвращаемого значения.

```csharp
public override UISwipeActionsConfiguration GetLeadingSwipeActionsConfiguration(UITableView tableView, NSIndexPath indexPath)
{
    //UIContextualActions
    var definitionAction = ContextualDefinitionAction(indexPath.Row);
    var flagAction = ContextualFlagAction(indexPath.Row);

    //UISwipeActionsConfiguration
    var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction });

    leadingSwipe.PerformsFirstActionWithFullSwipe = false;

    return leadingSwipe;
}
```

<a name="create-uiswipeactionsconfigurations" />

### <a name="2-instantiate-a-uiswipeactionsconfiguration"></a>2. Создание экземпляра `UISwipeActionsConfiguration`

Создайте экземпляр `UISwipeActionsConfiguration` с помощью метода `FromActions`, чтобы добавить новый массив `UIContextualAction`s, как показано в следующем фрагменте кода:

```csharp
var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction })

leadingSwipe.PerformsFirstActionWithFullSwipe = false;
```

Важно отметить, что порядок отображения действий зависит от того, как они передаются в массив. Например, приведенный выше код для ведущих прокрутки отображает следующие действия:

![ведущие действия по считыванию, отображаемые в строке таблицы](row-action-images/action03.png)

Для прокрутки в конце будут показаны действия, показанные на следующем рисунке:

![завершающие действия по считыванию, отображаемые в строке таблицы](row-action-images/action04.png)

В этом фрагменте кода также используется новое свойство `PerformsFirstActionWithFullSwipe`. По умолчанию для этого свойства задано значение true, означающее, что первое действие в массиве произойдет, когда пользователь полностью пройдет по строке. Если у вас есть действие, которое не является разрушительным (например, "Удалить"), это может не быть идеальным поведением, поэтому необходимо задать для него значение `false`.

<a name="create-uicontextualaction" />

### <a name="create-a-uicontextualaction"></a>Создание `UIContextualAction`

В контекстном действии создается действие, которое будет отображаться при считывании пользователем строки таблицы.

Чтобы инициализировать действие, необходимо указать `UIContextualActionStyle`, заголовок и `UIContextualActionHandler`. `UIContextualActionHandler` принимает три параметра: действие, представление, в котором было показано действие, и обработчик завершения:

```csharp
public UIContextualAction ContextualFlagAction(int row)
{
    var action = UIContextualAction.FromContextualActionStyle
                    (UIContextualActionStyle.Normal,
                        "Flag",
                        (FlagAction, view, success) => {
                            var alertController = UIAlertController.Create($"Report {words[row]}?", "", UIAlertControllerStyle.Alert);
                            alertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, null));
                            alertController.AddAction(UIAlertAction.Create("Yes", UIAlertActionStyle.Destructive, null));
                            PresentViewController(alertController, true, null);

                            success(true);
                        });

    action.Image = UIImage.FromFile("feedback.png");
    action.BackgroundColor = UIColor.Blue;

    return action;
}
```

Можно изменять различные визуальные свойства, например цвет фона или изображение действия. Приведенный выше фрагмент кода демонстрирует добавление изображения к действию и задание синего цвета фона.

После создания контекстных действий они могут использовать для инициализации `UISwipeActionsConfiguration` в методе `GetLeadingSwipeActionsConfiguration`.

## <a name="uitableviewrowaction"></a>уитаблевиевровактион

Чтобы определить одно или несколько действий пользовательской строки для `UITableView`, необходимо создать экземпляр класса `UITableViewDelegate` и переопределить метод `EditActionsForRow`. Например:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using Foundation;
using UIKit;

namespace BasicTable
{
    public class TableDelegate : UITableViewDelegate
    {
        #region Constructors
        public TableDelegate ()
        {
        }

        public TableDelegate (IntPtr handle) : base (handle)
        {
        }

        public TableDelegate (NSObjectFlag t) : base (t)
        {
        }

        #endregion

        #region Override Methods
        public override UITableViewRowAction[] EditActionsForRow (UITableView tableView, NSIndexPath indexPath)
        {
            UITableViewRowAction hiButton = UITableViewRowAction.Create (
                UITableViewRowActionStyle.Default,
                "Hi",
                delegate {
                    Console.WriteLine ("Hello World!");
                });
            return new UITableViewRowAction[] { hiButton };
        }
        #endregion
    }
}
```

Статический метод `UITableViewRowAction.Create` используется для создания новой `UITableViewRowAction`, которая будет отображать кнопку **Hi** при горизонтальном прокрутке строки в таблице. Позже создается новый экземпляр `TableDelegate` и прикрепляется к `UITableView`. Например:

```csharp
TableDelegate tableDelegate;
...

// Replace the standard delete button with a "Hi" button
tableDelegate = new TableDelegate ();
table.Delegate = tableDelegate;

```

Если при выполнении приведенного выше кода пользователь проводит влево по строке таблицы, вместо кнопки **Удалить** отображается кнопка **Hi** , которая отображается по умолчанию:

[![](row-action-images/action01.png "The Hi button being displayed instead of the Delete button")](row-action-images/action01.png#lightbox)

Если пользователь нажмет кнопку **Hi** , `Hello World!` будет записана в консоль в Visual Studio для Mac или Visual Studio, когда приложение будет запущено в режиме отладки.

## <a name="related-links"></a>Связанные ссылки

- [Таблесвипеактионс (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/tableswipeactions)
- [Воркингвистаблес (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithtables)
