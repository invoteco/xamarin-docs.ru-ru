---
title: Общие сведения о Xamarin.Forms CollectionView
description: CollectionView — это гибкий и эффективный представление для представления списков данных, используя спецификации другой макет.
ms.prod: xamarin
ms.assetid: 5C08F687-B9E6-4CE4-8726-F287F6D0B6A7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 889c78ea6849cdd094d34ed0cf74ceebd33ce51d
ms.sourcegitcommit: 0596004d4a0e599c1da1ddd75a6ac928f21191c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/22/2019
ms.locfileid: "66005137"
---
# <a name="xamarinforms-collectionview-introduction"></a>Общие сведения о Xamarin.Forms CollectionView

![](~/media/shared/preview.png "Этот API в настоящее время предоставляется в режиме предварительной версии")

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) Просмотр для представления списков данных с помощью спецификации другой макет. Оно предоставляет предоставляют более гибкий и эффективной альтернативой для [ `ListView` ](xref:Xamarin.Forms.ListView). Например, ниже снимках экрана, `CollectionView` , использующий два столбца вертикальную сетку, и что позволяет выделение нескольких элементов:

[![Снимок экрана: макет CollectionView вертикальной сетки, в iOS и Android](introduction-images/verticalgrid-multipleselection.png "CollectionView вертикальную сетку макета Выбор нескольких элементов")](introduction-images/verticalgrid-multipleselection-large.png#lightbox "CollectionView вертикальную сетку макета с Выбор нескольких элементов")

[`CollectionView`](xref:Xamarin.Forms.CollectionView) доступен в Xamarin.Forms 4.0. Тем не менее, он является в настоящее время экспериментальным и может использоваться только, добавив следующую строку кода, чтобы ваши `AppDelegate` класс на iOS или для вашей `MainActivity` класс на устройствах Android, перед вызовом метода `Forms.Init`:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) доступен только в iOS и Android.

## <a name="collectionview-and-listview-differences"></a>Различия CollectionView и ListView

Хотя [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) и [ `ListView` ](xref:Xamarin.Forms.ListView) интерфейсы API похожи, существуют некоторые важные различия:

- [`CollectionView`](xref:Xamarin.Forms.CollectionView) имеет модель гибкий макет, что позволяет представлять горизонтально или вертикально, в списке или сетке данные.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) поддерживает единичный и выбор нескольких элементов.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) не поддерживает понятие ячеек. Вместо этого шаблон данных используется для определения внешнего вида каждого элемента данных в списке.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) автоматически использует виртуализации, предоставляемые базовые собственные элементы управления.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) уменьшает область API [ `ListView` ](xref:Xamarin.Forms.ListView). Многие свойства и события из [ `ListView` ](xref:Xamarin.Forms.ListView) не существуют в `CollectionView`.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) не поддерживает встроенные разделители.

## <a name="move-from-listview-to-collectionview"></a>Переместить из ListView CollectionView

[`ListView`](xref:Xamarin.Forms.ListView) реализации в существующих реализациях Xamarin.Forms могут быть перенесены в [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) реализации с помощью следующей таблицы:

| Понятие | ListView API | CollectionView |
|---|---|---|
| Данные | `ItemsSource` | Объект [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) заполняется данными, задав его `ItemsSource` свойство. Дополнительные сведения см. в разделе [заполнения CollectionView с данными](populate-data.md#populate-a-collectionview-with-data). |
| Внешний вид элемента | `ItemTemplate` | Внешний вид каждого элемента в [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) может быть определена путем задания `ItemTemplate` свойства [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Дополнительные сведения см. в разделе [определения внешнего вида элемента](populate-data.md#define-item-appearance). |
| Ячейки | `TextCell`, `ImageCell`, `ViewCell` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) не поддерживает понятие ячеек. Вместо этого шаблон данных используется для определения внешнего вида каждого элемента данных в списке. |
| Разделители строк | `SeparatorColor`, `SeparatorVisibility` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) не поддерживает встроенные разделители. Они могут быть предоставлены, при необходимости, в шаблоне элемента. |
| Выбранное | `SelectionMode`, `SelectedItem` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) поддерживает единичный и выбор нескольких элементов. Дополнительные сведения см. в разделе [Xamarin.Forms CollectionView выбора](selection.md). |
| Высота строки | `HasUnevenRows`, `RowHeight` | В `CollectionView`, высоты каждого элемента определяется `ItemSizingStrategy` свойство. Дополнительные сведения см. в разделе [изменения размера элемента](layout.md#item-sizing).|
| Кэширование | `CachingStrategy` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) автоматически использует виртуализацию, предоставляемые базовые собственные элементы управления. |
| Верхние и нижние колонтитулы | `Header`, `HeaderElement`, `HeaderTemplate`, `Footer`, `FooterElement`, `FooterTemplate` | Верхние и нижние колонтитулы в настоящее время поддерживаются `CollectionView`, но будут добавлены в будущем выпуске.|
| Группирование | `GroupDisplayBinding`, `GroupHeaderTemplate`, `GroupShortNameBinding`, `IsGroupingEnabled` | Группирование в настоящее время не поддерживается в `CollectionView`, но будут добавлены в будущем выпуске. |
| Потяните, чтобы обновить | `IsPullToRefreshEnabled`, `IsRefreshing`, `RefreshAllowed`, `RefreshCommand`, `RefreshControlColor`, `BeginRefresh()`, `EndRefresh()` | Потяните, чтобы обновить сейчас не поддерживается в `CollectionView`, но будут добавлены в будущем выпуске. |
| Контекстные действия | `ContextActions` | В настоящее время не поддерживаются контекстные действия `CollectionView`, но будут добавлены в будущем выпуске. |
| Прокрутка | `ScrollTo()` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) Определяет `ScrollTo` методы, которые прокрутки элементов в представлении. Дополнительные сведения см. в разделе [прокрутка](scrolling.md). |

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)
