---
title: Справочник по элементам управления
description: Описание всех элементов пользовательского интерфейса, используемых для создания приложения Xamarin. Forms. В этой статье перечислены группы управления, составляющих пользовательский интерфейс приложения Xamarin.Forms.
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2019
ms.openlocfilehash: b9436603c17eb8008f470e75a52a93e8e122034f
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75488119"
---
# <a name="controls-reference"></a>Справочник по элементам управления

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

Пользовательский интерфейс приложения Xamarin. Forms состоит из объектов, которые сопоставляются с собственными элементами управления каждой целевой платформы. Это позволяет приложениям для конкретных платформ для iOS, Android и универсальная платформа Windows использовать код Xamarin. Forms, содержащийся в [библиотеке .NET Standard](~/cross-platform/app-fundamentals/net-standard.md).

Ниже перечислены четыре основные группы управления, используемые для создания пользовательского интерфейса приложения Xamarin. Forms.

- [**Страницы**](pages.md)
- [**Макеты**](layouts.md)
- [**Представления**](views.md)
- [**Ячейки**](cells.md)

Страницы Xamarin.Forms обычно занимает весь экран. Страница обычно содержит макет, который содержит представления и, возможно, других значений. Ячейки доступны специальные компоненты, используемые в связи с [ `TableView` ](views.md#tableview) и [ `ListView` ](views.md#listview). Схема классов, показывающая иерархию типов, которые обычно используются для построения пользовательского интерфейса в Xamarin. Forms, можно найти в [иерархии классов элементов управления Xamarin. Forms](~/xamarin-forms/internals/class-hierarchy.md).

В четырех статьях на [ **страниц**](pages.md), [ **макеты**](layouts.md), [ **представления** ](views.md), и [ **ячеек**](cells.md), описывается каждый тип элемента управления со ссылками на его API документации, описывающей ее использование (если он существует) и один или несколько примеров программ (если они существуют). Каждый тип элемента управления также сопровождается снимком экрана, показывающим страницу из примера [**формсгаллери**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) , работающего на устройствах iOS и Android. Каждый снимке экрана ниже приведены ссылки на исходный код для страницы, эквивалентную страницу XAML, C# и (при необходимости) файла кода C# для страницы XAML.

> [!NOTE]
> Страницы, макеты и представления являются производными от класса `VisualElement`. Класс `VisualElement` предоставляет разнообразные свойства, методы и события, которые полезны при создании производных классов. Дополнительные сведения см. в разделе [висуалелемент Properties, Methods and Events](common-properties.md).

Помимо элементов управления, предоставляемых Xamarin. Forms, доступны сторонние элементы управления. Дополнительные сведения см. в разделе [сторонние элементы управления](thirdparty.md).

## <a name="related-links"></a>Связанные ссылки

- [Пример Xamarin.Forms FormsGallery](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Иерархия классов элементов управления Xamarin. Forms](~/xamarin-forms/internals/class-hierarchy.md)
- [Документация по API](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
