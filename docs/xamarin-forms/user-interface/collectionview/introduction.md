---
title: Общие сведения о Xamarin.Forms CollectionView
description: CollectionView — это гибкий и эффективный представление для представления списков данных, используя спецификации другой макет.
ms.prod: xamarin
ms.assetid: 5C08F687-B9E6-4CE4-8726-F287F6D0B6A7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 23de838a395b68f656f95c5665dc97a1d928bece
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65054654"
---
# <a name="xamarinforms-collectionview-introduction"></a>Общие сведения о Xamarin.Forms CollectionView

![](~/media/shared/preview.png "Этот API в настоящее время в предварительной версии")

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

`CollectionView` Просмотр для представления списков данных с помощью спецификации другой макет. Оно предоставляет предоставляют более гибкий и эффективной альтернативой для [ `ListView` ](xref:Xamarin.Forms.ListView). Например, ниже снимках экрана, `CollectionView` , использующий два столбца вертикальную сетку, и что позволяет выделение нескольких элементов:

[![Снимок экрана: макет CollectionView вертикальной сетки, в iOS и Android](introduction-images/verticalgrid-multipleselection.png "CollectionView вертикальную сетку макета Выбор нескольких элементов")](introduction-images/verticalgrid-multipleselection-large.png#lightbox "CollectionView вертикальную сетку макета с Выбор нескольких элементов")

`CollectionView` доступен в предварительной версии Xamarin.Forms 4.0. Тем не менее, он является в настоящее время экспериментальным и может использоваться только, добавив следующую строку кода, чтобы ваши `AppDelegate` класс на iOS или для вашей `MainActivity` класс на устройствах Android, перед вызовом метода `Forms.Init`:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

> [!IMPORTANT]
> `CollectionView` доступен только в iOS и Android.

## <a name="collectionview-and-listview-differences"></a>Различия CollectionView и ListView

Хотя `CollectionView` и `ListView` интерфейсы API похожи, существуют некоторые важные различия:

- `CollectionView` имеет модель гибкий макет, что позволяет представлять горизонтально или вертикально, в списке или сетке данные.
- `CollectionView` поддерживает единичный и выбор нескольких элементов.
- `CollectionView` не поддерживает понятие ячеек. Вместо этого шаблон данных используется для определения внешнего вида каждого элемента данных в списке.
- `CollectionView` автоматически использует виртуализации, предоставляемые базовые собственные элементы управления.
- `CollectionView` уменьшает область API [ `ListView` ](xref:Xamarin.Forms.ListView). Многие свойства и события из `ListView` не существуют в `CollectionView`.
- `CollectionView` не поддерживает встроенные разделители.

## <a name="move-from-listview-to-collectionview"></a>Переместить из ListView CollectionView

[`ListView`](xref:Xamarin.Forms.ListView) реализации в существующих реализациях Xamarin.Forms могут быть перенесены в `CollectionView` реализации с помощью следующей таблицы:

| Понятие | ListView API | CollectionView |
|---|---|---|
| Данные | `ItemsSource` | Объект `CollectionView` заполняется данными, задав его `ItemsSource` свойство. Дополнительные сведения см. в разделе [заполнения CollectionView с данными](populate-data.md#populate-a-collectionview-with-data). |
| Внешний вид элемента | `ItemTemplate` | Внешний вид каждого элемента в `CollectionView` может быть определена путем задания `ItemTemplate` свойства [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Дополнительные сведения см. в разделе [определения внешнего вида элемента](populate-data.md#define-item-appearance). |
| Ячейки | `TextCell`, `ImageCell`, `ViewCell` | `CollectionView` не поддерживает понятие ячеек. Вместо этого шаблон данных используется для определения внешнего вида каждого элемента данных в списке. |
| Разделители строк | `SeparatorColor`, `SeparatorVisibility` | `CollectionView` не поддерживает встроенные разделители. Они могут быть предоставлены, при необходимости, в шаблоне элемента. |
| Выбранное | `SelectionMode`, `SelectedItem` | `CollectionView` поддерживает единичный и выбор нескольких элементов. Дополнительные сведения см. в разделе [Xamarin.Forms CollectionView выбора](selection.md). |
| Высота строки | `HasUnevenRows`, `RowHeight` | В `CollectionView`, высоты каждого элемента определяется `ItemSizingStrategy` свойство. Дополнительные сведения см. в разделе [изменения размера элемента](layout.md#item-sizing).|
| Кэширование | `CachingStrategy` | `CollectionView` автоматически использует виртуализацию, предоставляемые базовые собственные элементы управления. |
| Верхние и нижние колонтитулы | `Header`, `HeaderElement`, `HeaderTemplate`, `Footer`, `FooterElement`, `FooterTemplate` | Верхние и нижние колонтитулы в настоящее время поддерживаются `CollectionView`, но будут добавлены в будущем выпуске.|
| Группирование | `GroupDisplayBinding`, `GroupHeaderTemplate`, `GroupShortNameBinding`, `IsGroupingEnabled` | Группирование в настоящее время не поддерживается в `CollectionView`, но будут добавлены в будущем выпуске. |
| Потяните, чтобы обновить | `IsPullToRefreshEnabled`, `IsRefreshing`, `RefreshAllowed`, `RefreshCommand`, `RefreshControlColor`, `BeginRefresh()`, `EndRefresh()` | Потяните, чтобы обновить сейчас не поддерживается в `CollectionView`, но будут добавлены в будущем выпуске. |
| Контекстные действия | `ContextActions` | В настоящее время не поддерживаются контекстные действия `CollectionView`, но будут добавлены в будущем выпуске. |
| Прокрутка | `ScrollTo()` | `CollectionView` Определяет `ScrollTo` методы, которые прокрутки элементов в представлении. Дополнительные сведения см. в разделе [прокрутка](scrolling.md). |

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
