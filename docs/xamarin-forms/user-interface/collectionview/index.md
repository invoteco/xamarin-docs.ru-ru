---
title: Xamarin.Forms CollectionView
description: CollectionView — это гибкий и эффективный представление для представления списков данных, используя спецификации другой макет.
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/15/2019
ms.openlocfilehash: b22449659d2d4b7791328d53ed2d2b29d405ffc8
ms.sourcegitcommit: 5d4e6677224971e2bc0268f405d192d0358c74b8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/21/2019
ms.locfileid: "58330039"
---
# <a name="xamarinforms-collectionview"></a>Xamarin.Forms CollectionView

![Предварительный просмотр](~/media/shared/preview.png)

> [!IMPORTANT]
> `CollectionView` В настоящее время доступна в предварительной версии, а не имеет части плановой функциональных возможностей. Кроме того API может измениться, как реализация завершилась.

`CollectionView` Просмотр для представления списков данных с помощью спецификации другой макет. Оно предоставляет предоставляют более гибкий и эффективной альтернативой для [ `ListView` ](xref:Xamarin.Forms.ListView). Хотя `CollectionView` и `ListView` интерфейсы API похожи, существуют некоторые важные различия:

- `CollectionView` имеет модель гибкий макет, что позволяет представлять горизонтально или вертикально, в списке или сетке данные.
- `CollectionView` не поддерживает понятие ячеек. Вместо этого шаблон данных используется для определения внешнего вида каждого элемента данных в списке.
- `CollectionView` автоматически использует виртуализации, предоставляемые базовые собственные элементы управления.
- `CollectionView` уменьшает область API [ `ListView` ](xref:Xamarin.Forms.ListView). Многие свойства и события из `ListView` не существуют в `CollectionView`.
- `CollectionView` не поддерживает встроенные разделители.

`CollectionView` доступен в предварительной версии Xamarin.Forms 4.0. Тем не менее, он является в настоящее время экспериментальным и может использоваться только, добавив следующую строку кода, чтобы ваши `AppDelegate` класс на iOS или для вашей `MainActivity` класс на устройствах Android, перед вызовом метода `Forms.Init`:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

> [!NOTE]
> `CollectionView` доступен только в iOS и Android.

## <a name="populate-collectionview-with-datapopulate-datamd"></a>[Заполнить данными CollectionView](populate-data.md)

Объект `CollectionView` заполняется данными, задав его `ItemsSource` свойство к любой коллекции, реализующий `IEnumerable`. Внешний вид каждого элемента в списке может быть определена путем задания `ItemTemplate` свойства [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate).

## <a name="specify-collectionview-layoutlayoutmd"></a>[Задать макет CollectionView](layout.md)

По умолчанию `CollectionView` будет отображения элементов в вертикальном списке. Тем не менее можно указать вертикальные и горизонтальные списки и сеток.

## <a name="set-collectionview-selection-modeselectionmd"></a>[Установка режима выбора CollectionView](selection.md)

По умолчанию `CollectionView` выбор отключен. Тем не менее можно включить одиночное и множественное выделение.

## <a name="display-an-emptyview-when-data-is-unavailableemptyviewmd"></a>[Отобразить EmptyView, когда данные недоступны](emptyview.md)

В `CollectionView`, можно указать пустое представление, предоставляющий отзыв для пользователя, когда данные недоступны для отображения. Пустое представление может быть строка, представление или несколько представлений.

## <a name="scroll-an-item-into-viewscrollingmd"></a>[Прокрутки элемента в представлении](scrolling.md)

Когда пользователь вставляет, чтобы инициировать прокрутки, конечное положение прокрутки можно управлять, чтобы полностью отображаются элементы. Кроме того `CollectionView` определяет два `ScrollTo` методы, которые программно прокрутки элементов в представлении. Одной из перегрузок прокручивает элемент по указанному индексу в представление, пока другой прокрутке указанного элемента.
