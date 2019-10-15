---
title: Введение в Xamarin. Forms CollectionView
description: CollectionView — это гибкое и производительное представление для представления списков данных с использованием различных спецификаций макета.
ms.prod: xamarin
ms.assetid: 5C08F687-B9E6-4CE4-8726-F287F6D0B6A7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/24/2019
ms.openlocfilehash: 89afb0f2bfe93a5f78b0cd162f2a65e585b54b4b
ms.sourcegitcommit: 43423d4018cc0d4b0b8c98a4b3da0704495eb0cf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2019
ms.locfileid: "72303236"
---
# <a name="xamarinforms-collectionview-introduction"></a>Введение в Xamarin. Forms CollectionView

![Сейчас этот API предварительно выпущен](~/media/shared/preview.png)

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

Представление [`CollectionView`](xref:Xamarin.Forms.CollectionView) служит для вывода списков данных с различными спецификациями макета. Она нацелена на предоставление более гибкой и производительной альтернативы [`ListView`](xref:Xamarin.Forms.ListView). Например, на следующих снимках экрана показан `CollectionView`, в котором используется вертикальная сетка двух столбцов, которая позволяет выбрать несколько вариантов:

[![Снимок экрана: вертикальный макет сетки CollectionView, в iOS и Android](introduction-images/verticalgrid-multipleselection.png "CollectionView вертикальная сетка с множественным выделением")](introduction-images/verticalgrid-multipleselection-large.png#lightbox "Вертикальный макет сетки CollectionView с множественным выделением")

[`CollectionView`](xref:Xamarin.Forms.CollectionView) доступен в Xamarin. forms 4,0. Однако в настоящее время он экспериментальен и может использоваться только путем добавления следующей строки кода в класс `AppDelegate` в iOS или в класс `MainActivity` в Android или в `App.xaml.cs` в UWP перед вызовом `Forms.Init`:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) доступен в iOS и Android, но [частично доступен](https://gist.github.com/hartez/7d0edd4182dbc7de65cebc6c67f72e14) на универсальная платформа Windows.

## <a name="collectionview-and-listview-differences"></a>Различия в CollectionView и ListView

Хотя интерфейсы API [`CollectionView`](xref:Xamarin.Forms.CollectionView) и [`ListView`](xref:Xamarin.Forms.ListView) похожи, существуют некоторые важные отличия.

- [`CollectionView`](xref:Xamarin.Forms.CollectionView) имеет гибкую модель макета, которая позволяет представлять данные по вертикали или по горизонтали, в списке или в сетке.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) поддерживает выбор одного и нескольких элементов.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) не имеет концепции ячеек. Вместо этого шаблон данных используется для определения внешнего вида каждого элемента данных в списке.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) автоматически использует виртуализацию, предоставляемую базовыми элементами управления.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) сокращает поверхность API [`ListView`](xref:Xamarin.Forms.ListView). Многие свойства и события из [`ListView`](xref:Xamarin.Forms.ListView) отсутствуют в `CollectionView`.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) не включает встроенные разделители.

## <a name="move-from-listview-to-collectionview"></a>Переход из ListView в CollectionView

[`ListView`](xref:Xamarin.Forms.ListView) реализации в существующих реализациях Xamarin. Forms можно перенести в реализации [`CollectionView`](xref:Xamarin.Forms.CollectionView) с помощью следующей таблицы:

| Понятие | API ListView | CollectionView |
|---|---|---|
| Data | `ItemsSource` | [@No__t-1](xref:Xamarin.Forms.CollectionView) заполняется данными путем установки свойства `ItemsSource`. Дополнительные сведения см. [в разделе Заполнение CollectionView данными](populate-data.md#populate-a-collectionview-with-data). |
| Внешний вид элемента | `ItemTemplate` | Внешний вид каждого элемента в [`CollectionView`](xref:Xamarin.Forms.CollectionView) можно определить, задав для свойства `ItemTemplate` значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate). Дополнительные сведения см. в разделе [Определение внешнего вида элемента](populate-data.md#define-item-appearance). |
| Ячейки | `TextCell`, `ImageCell`, `ViewCell` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) не имеет концепции ячеек. Вместо этого шаблон данных используется для определения внешнего вида каждого элемента данных в списке. |
| Разделители строк | `SeparatorColor`, `SeparatorVisibility` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) не включает встроенные разделители. При необходимости они могут быть предоставлены в шаблоне элемента. |
| Выбранное | `SelectionMode`, `SelectedItem` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) поддерживает выбор одного и нескольких элементов. Дополнительные сведения см. в разделе [Xamarin. Forms CollectionView Selection](selection.md). |
| Высота строки | `HasUnevenRows`, `RowHeight` | В `CollectionView` высота строки каждого элемента определяется свойством `ItemSizingStrategy`. Дополнительные сведения см. в разделе [изменение размера элемента](layout.md#item-sizing).|
| Кэширование | `CachingStrategy` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) автоматически использует виртуализацию, предоставляемую базовыми элементами управления. |
| Верхние и нижние колонтитулы | `Header`, `HeaderElement`, `HeaderTemplate`, `Footer`, `FooterElement`, `FooterTemplate` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) может представлять верхний и нижний колонтитулы для прокрутки элементов списка с помощью свойств `Header`, `Footer`, `HeaderTemplate` и `FooterTemplate`. Дополнительные сведения см. в разделе [верхние и нижние колонтитулы](layout.md#headers-and-footers). |
| Группирование | `GroupDisplayBinding`, `GroupHeaderTemplate`, `GroupShortNameBinding`, `IsGroupingEnabled` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) отображает правильно сгруппированные данные, присвоив свойству `IsGrouped` значение `true`. Заголовки групп и нижние колонтитулы групп можно настроить, задав свойства `GroupHeaderTemplate` и `GroupFooterTemplate` для объектов [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) . Дополнительные сведения см. в разделе [Xamarin. Forms CollectionView GROUPING](grouping.md). |
| Извлечь для обновления | `IsPullToRefreshEnabled`, `IsRefreshing`, `RefreshAllowed`, `RefreshCommand`, `RefreshControlColor`, `BeginRefresh()`, `EndRefresh()` | Запрос на обновление в настоящее время не поддерживается в `CollectionView`, но будет добавлен в следующем выпуске. |
| Контекстные действия | `ContextActions` | Контекстные действия в настоящее время не поддерживаются в `CollectionView`, но будут добавлены в следующем выпуске. |
| Прокрутка | `ScrollTo()` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) определяет методы `ScrollTo`, прокручивает элементы в представление. Дополнительные сведения см. в разделе [прокрутка](scrolling.md). |

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
