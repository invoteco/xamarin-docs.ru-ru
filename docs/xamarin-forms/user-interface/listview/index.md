---
title: ListView Xamarin.Forms
description: В этом руководством рассматривается ListView. Forms, который можно использовать для представления данных в интерактивных списках.
ms.prod: xamarin
ms.assetid: FEFDF7E0-720F-4BD1-863F-4477226AA695
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/04/2019
ms.openlocfilehash: 5d09d76a44a6322285a143230173d244848ba4a6
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770230"
---
# <a name="xamarinforms-listview"></a>ListView Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview)

[`ListView`](xref:Xamarin.Forms.ListView)представляет собой представление для представления списков данных, особенно длинных списков, требующих прокрутки.

> [!IMPORTANT]
> Представление [`CollectionView`](xref:Xamarin.Forms.CollectionView) служит для вывода списков данных с различными спецификациями макета. Она нацелена на предоставление более гибкой и производительной альтернативы [`ListView`](xref:Xamarin.Forms.ListView). Дополнительные сведения см. в статье [Представление CollectionView в Xamarin.Forms](~/xamarin-forms/user-interface/collectionview/index.md).

## <a name="use-cases"></a>Варианты использования

`ListView` Элемент управления может использоваться в любой ситуации, когда отображаются прокручиваемые списки данных. `ListView` Класс поддерживает действия контекста и привязку данных.

Элемент управления не должен путать [`TableView`](~/xamarin-forms/user-interface/tableview.md) с элементом управления. `ListView` `TableView` Элемент управления является лучшим вариантом при наличии непривязанного списка параметров или данных, так как он позволяет указывать в XAML предопределенные параметры. Например, приложение iOS Settings, которое имеет преимущественно предопределенный набор параметров, лучше подходит для использования `TableView` , `ListView`чем.

Класс не поддерживает определение элементов списка в XAML. `ItemTemplate` для определения элементов в списке `ItemsSource` необходимо использовать свойство или привязку данных с помощью. `ListView`

`ListView` Лучше всего подходит для коллекций, состоящих из одного типа данных. Это требование связано с тем, что для каждой строки в списке можно использовать только один тип ячеек. `TableView` Элемент управления может поддерживать несколько типов ячеек, поэтому это лучший вариант, если требуется отобразить несколько типов данных.

Дополнительные сведения о привязке данных к `ListView` экземпляру см. в разделе [Источники данных в ListView](~/xamarin-forms/user-interface/listview/data-and-databinding.md).

## <a name="components"></a>Компоненты
У `ListView` элемента управления есть ряд компонентов, позволяющих использовать собственные функции каждой платформы. Эти компоненты определены в следующих разделах.

### <a name="headers-and-footerscustomizing-list-appearancemdheaders-and-footers"></a>[Верхние и нижние колонтитулы](customizing-list-appearance.md#headers-and-footers)

Компоненты верхнего и нижнего колонтитулов отображаются в начале и в конце списка, отделены от данных списка. Верхние и нижние колонтитулы можно привязать к отдельному источнику данных из источника данных ListView.

### <a name="groupscustomizing-list-appearancemdgrouping"></a>[Группы](customizing-list-appearance.md#grouping)

Данные в `ListView` могут быть сгруппированы для упрощения навигации. Группы обычно привязаны к данным. На следующем снимке экрана `ListView` показан элемент с сгруппированными данными:

(images/grouping-depth-cropped.png)](images/grouping-depth.png#lightbox "Сгруппированные") данные в ListView "сгруппированные данные" [ ![]

### <a name="cellscustomizing-cell-appearancemd"></a>[Ячейки](customizing-cell-appearance.md)

Элементы данных в `ListView` называются ячейками. Каждая ячейка соответствует строке данных. Встроенные ячейки для выбора или можно определить собственные настраиваемые ячейки. Встроенные и настраиваемые ячейки могут быть использовать/определенного в XAML или кода.

- [Встроенные ячейки](customizing-cell-appearance.md#built-in-cells), такие как `TextCell` и `ImageCell`, соответствуют собственным элементам управления и являются особенно производительными.
  - [`TextCell`](customizing-cell-appearance.md#textcell) Отображает строку текста (при необходимости с текстом подробностей). Текст сведений отображается как вторая строка меньший размер шрифта и Цвет диакритических знаков.
  - [`ImageCell`](customizing-cell-appearance.md#imagecell) Отображает изображение с текстом. Отображается как объект `TextCell` с изображением слева.
- [Пользовательские ячейки](customizing-cell-appearance.md#customcells) используются для представления сложных данных. Например, пользовательскую ячейку можно использовать для представления списка композиций, включающих альбом и исполнителя.

На следующем снимке экрана `ListView` показан элемент с имажецелл элементами:

["Имажецелл элементы в ListView" имажецелл элементы в ListView ![](images/image-cell-default-cropped.png)](images/image-cell-default.png#lightbox "")

Дополнительные сведения о настройке ячеек в `ListView`см. в разделе [Настройка внешнего вида ячеек ListView](customizing-cell-appearance.md).

## <a name="functionality"></a>Функция
`ListView` Класс поддерживает несколько стилей взаимодействия.

- Запрос [на обновление](interactivity.md#pull-to-refresh) позволяет пользователю извлекаться `ListView` вниз для обновления содержимого.
- [Контекстные действия](interactivity.md#context-actions) позволяют разработчику указывать настраиваемые действия для отдельных элементов списка. Например можно реализовать действие прокрутки на iOS, или удерживайте действий на устройстве Android.
- [Выбор](interactivity.md#selectiontaps) позволяет разработчику прикреплять функции к событиям выбора и девыделения для элементов списка.

На следующем снимке экрана `ListView` показана контекстная операция.

Действия контекста ["контекстные действия в ListView" в ListView ![](images/context-default-cropped.png)](images/context-default.png#lightbox "")

Дополнительные сведения о возможностях `ListView`взаимодействия см. в разделе [действия & интерактивном представлении с помощью ListView](interactivity.md).

## <a name="related-links"></a>Связанные ссылки

- [Работа с ListView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview)
- [Двусторонней привязки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)
- [Встроенные в ячейках (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-builtincells)
- [Пользовательские ячейки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)
- [Группирование (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)
- [Пользовательское представление модуля подготовки отчетов (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative/)
- [ListView интерактивности (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)
