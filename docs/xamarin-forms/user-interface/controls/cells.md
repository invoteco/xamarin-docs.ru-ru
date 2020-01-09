---
title: Ячейки Xamarin.Forms
description: Ячейки Xamarin.Forms могут добавляться к ListView и TableViews. В этой статье перечислены ячеек, включенных в Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 77DA0C89-35D6-4C09-A072-3ADE53FD56CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: e8f3d8af7aab2a7a73787021f114470726d74b72
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75488092"
---
# <a name="xamarinforms-cells"></a>Ячейки Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_Ячейки Xamarin.Forms могут добавляться к ListView и TableViews._

Объект *ячейки* специализированного элемента, используемого для элементов в таблице в нем описано, как должны отображаться каждый элемент в списке. [ `Cell` ](xref:Xamarin.Forms.Cell) Класс является производным от [ `Element` ](xref:Xamarin.Forms.Element), из которого [ `VisualElement` ](xref:Xamarin.Forms.Element) также является производным. Ячейки сама по себе не визуальный элемент; Вместо этого это шаблон для создания визуального элемента.

`Cell` используется исключительно с [ `ListView` ](views.md#listview) и [ `TableView` ](views.md#tableview) элементов управления. Чтобы узнать, как использовать и настраивать ячеек, обратитесь к [ `ListView` ](~/xamarin-forms/user-interface/listview/index.md) и [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) документации.

## <a name="cells"></a>Ячейки

Xamarin.Forms поддерживает следующие типы ячейки:

<a name="textCell" />

### <a name="textcell"></a>TextCell

|     |     |
| --- | --- |
| Объект [ `TextCell` ](xref:Xamarin.Forms.TextCell) отображает один или два текстовые строки. Задайте [ `Text` ](xref:Xamarin.Forms.TextCell.Text) свойство и, возможно, [ `Detail` ](xref:Xamarin.Forms.TextCell.Detail) свойство в эти текстовые строки.<br /><br />[Документация по API](xref:Xamarin.Forms.TextCell) / [руководство](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#textcell) | [![Пример Текстцелл](cells-images/TextCell.png "Пример Текстцелл")](cells-images/TextCell-Large.png#lightbox "Пример Текстцелл")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
|     |     |

### <a name="imagecell"></a>ImageCell

|     |     |
| --- | --- |
| [ `ImageCell` ](xref:Xamarin.Forms.ImageCell) Отображает те же сведения, что [ `TextCell` ](#textCell) , но включает в себя Битовая карта, заданное с помощью [ `Source` ](xref:Xamarin.Forms.Image.Source) свойство.<br /><br />[Документация по API](xref:Xamarin.Forms.ImageCell) / [руководство](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#imagecell) | [![Пример Имажецелл](cells-images/ImageCell.png "Пример Имажецелл")](cells-images/ImageCell-Large.png#lightbox "Пример Имажецелл")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
|     |     |

### <a name="switchcell"></a>SwitchCell

|     |     |
| --- | --- |
| [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) содержит набор текста со свойством [`Text`](xref:Xamarin.Forms.SwitchCell.Text) , а параметр включения/выключения изначально задается логическим свойством [`On`](xref:Xamarin.Forms.SwitchCell.On) . Обрабатывать [ `OnChanged` ](xref:Xamarin.Forms.SwitchCell.OnChanged) когда уведомления о событии `On` изменения свойств.<br /><br />[Документация по API](xref:Xamarin.Forms.SwitchCell) / [руководство](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![Пример Свитчцелл](cells-images/SwitchCell.png "Пример Свитчцелл")](cells-images/SwitchCell-Large.png#lightbox "Пример Свитчцелл")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
|     |     |

### <a name="entrycell"></a>EntryCell

|     |     |
| --- | --- |
| [ `EntryCell` ](xref:Xamarin.Forms.EntryCell) Определяет [ `Label` ](xref:Xamarin.Forms.EntryCell.Label) свойства, которое определяет ячейку и строку ввода и редактирования текста в [ `Text` ](xref:Xamarin.Forms.EntryCell.Text) свойство. Обрабатывать [ `Completed` ](xref:Xamarin.Forms.EntryCell.Completed) событий, чтобы получать уведомления, когда пользователь завершит ввод текста.<br /><br />[Документация по API](xref:Xamarin.Forms.EntryCell) / [руководство](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![Пример Ентрицелл](cells-images/EntryCell.png "Пример Ентрицелл")](cells-images/EntryCell-Large.png#lightbox "Пример Ентрицелл")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Связанные ссылки

- [Пример Xamarin.Forms FormsGallery](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Примеры Xamarin.Forms](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Документация по API Xamarin.Forms](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
