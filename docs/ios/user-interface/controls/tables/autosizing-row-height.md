---
title: Автоматическое изменение высоты строки в Xamarin. iOS
description: В этом документе описывается, как добавить в таблицу приложений Xamarin. iOS представления строк, высота которых зависит от содержимого. В нем рассматривается Макет ячеек в конструкторе iOS и включается Автомасштабирование высоты.
ms.prod: xamarin
ms.assetid: CE45A385-D40A-482A-90A0-E8382C2BFFB9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 4129370ecb465340a893e0a7f16703a08cc1db72
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021929"
---
# <a name="auto-sizing-row-height-in-xamarinios"></a>Автоматическое изменение высоты строки в Xamarin. iOS

Начиная с iOS 8, компания Apple добавила возможность создания табличного представления (`UITableView`), которое может автоматически увеличивать и уменьшать высоту заданной строки в зависимости от размера ее содержимого с помощью автоматического макета, классов размеров и ограничений.

в iOS 11 добавлена возможность автоматического расширения строк. Размеры верхних, нижних колонтитулов и ячеек теперь можно изменять автоматически в зависимости от их содержимого. Однако если таблица создана в конструкторе iOS, Interface Builder или если она имеет фиксированную высоту строк, необходимо вручную включить автоматическое изменение размера ячеек, как описано в этом разделе.

## <a name="cell-layout-in-the-ios-designer"></a>Макет ячеек в конструкторе iOS

Откройте раскадровку для табличного представления, для которой нужно Автоподбор размера строки в конструкторе iOS, выберите *прототип* ячейки и разработайте макет ячейки. Пример:

[![](autosizing-row-height-images/table01.png "The Cell's Prototype design")](autosizing-row-height-images/table01.png#lightbox)

Для каждого элемента в прототипе добавьте ограничения для сохранения элементов в правильном положении при изменении размеров представления таблицы для вращения или различных размеров экрана устройства iOS. Например, закреплять `Title` в верхней, левой и правой части *представления содержимого*ячейки:

[![](autosizing-row-height-images/table02.png "Pinning the Title to the top, left and right of the Cells Content View")](autosizing-row-height-images/table02.png#lightbox)

В нашем примере таблицы небольшие `Label` (в `Title`) — это поле, которое может сжиматься и увеличиваться для увеличения или уменьшения высоты строки. Чтобы добиться этого результата, добавьте следующие ограничения, чтобы закрепить левую, правую, верхнюю и нижнюю части Метки:

[![](autosizing-row-height-images/table03.png "These constraints to pin the left, right, top and bottom of the label")](autosizing-row-height-images/table03.png#lightbox)

Теперь, когда мы полностью ограничены элементами в ячейке, необходимо уточнить, какой элемент следует растянуть. Для этого задайте приоритет **Хуггинг содержимого** и **Сжатие содержимого** , как это необходимо в разделе **макета** панель свойств.

[![](autosizing-row-height-images/table03a.png "The Layout section of the Properties Pad")](autosizing-row-height-images/table03a.png#lightbox)

Задайте для элемента, который требуется расширить, значение приоритета **хуггинг и более** **низкое** значение приоритета сопротивления.

Далее необходимо выбрать прототип ячейки и присвоить ему уникальный **идентификатор**:

[![](autosizing-row-height-images/table04.png "Giving the Cell Prototype a unique Identifier")](autosizing-row-height-images/table04.png#lightbox)

В нашем примере `GrowCell`. Это значение будет использоваться позже при заполнении таблицы.

> [!IMPORTANT]
> Если таблица содержит более одного типа ячеек (**прототип**), необходимо убедиться, что каждый тип имеет собственные уникальные `Identifier`, чтобы автоматическое изменение размера строк работало.

Для каждого элемента прототипа ячейки присвойте ему **имя** , чтобы предоставить его C# коду. Пример:

[![](autosizing-row-height-images/table05.png "Assign a Name to expose it to C# code")](autosizing-row-height-images/table05.png#lightbox)

Затем добавьте пользовательский класс для `UITableViewController`, `UITableView` и `UITableCell` (прототип). Пример: 

[![](autosizing-row-height-images/table06.png "Adding a custom class for the UITableViewController, the UITableView and the UITableCell")](autosizing-row-height-images/table06.png#lightbox)

Наконец, чтобы убедиться, что в нашей метке отображается все ожидаемое содержимое, задайте для свойства **строки** значение `0`:

[![](autosizing-row-height-images/table06.png "The Lines property set to 0")](autosizing-row-height-images/table06a.png#lightbox)

Определив пользовательский интерфейс, добавим код, чтобы включить автоматическое изменение высоты строк.

## <a name="enabling-auto-resizing-height"></a>Включение автоподбора высоты

В представлении источника данных (`UITableViewDatasource`) или исходной (`UITableViewSource`) табличного представления при выводе ячейки в очередь необходимо использовать `Identifier`, определенные в конструкторе. Пример:

```csharp
public string CellID {
    get { return "GrowCell"; }
}
...

public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (CellID, indexPath) as GrowRowTableCell;
    var item = Items [indexPath.Row];

    // Setup
    cell.Image = UIImage.FromFile(item.ImageName);
    cell.Title = item.Title;
    cell.Description = item.Description;

    return cell;
}
```

По умолчанию табличное представление будет настроено для автоподбора высоты строки. Чтобы убедиться в этом, свойству `RowHeight` должно быть присвоено значение `UITableView.AutomaticDimension`. Также необходимо задать свойство `EstimatedRowHeight` в `UITableViewController`. Пример:

```csharp
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Initialize table
    TableView.DataSource = new GrowRowTableDataSource(this);
    TableView.Delegate = new GrowRowTableDelegate (this);
    TableView.RowHeight = UITableView.AutomaticDimension;
    TableView.EstimatedRowHeight = 40f;
    TableView.ReloadData ();
}
```

Эта оценка не должна быть точной, просто приблизительная оценка средней высоты каждой строки в табличном представлении.

При использовании этого кода при запуске приложения каждая строка будет сжиматься и увеличиваться в зависимости от высоты последней метки в прототипе ячейки. Пример:

[![](autosizing-row-height-images/table07.png "A sample table run")](autosizing-row-height-images/table07.png#lightbox)

## <a name="related-links"></a>Связанные ссылки

- [Гровровтабле (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/growrowtable)
