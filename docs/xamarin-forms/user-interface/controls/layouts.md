---
title: Макеты Xamarin.Forms
description: Макеты Xamarin.Forms используются для создания элементов управления пользовательского интерфейса в visual структуры. В этой статье перечислены макеты, включенные в Xamarin.Forms.
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/21/2018
ms.openlocfilehash: 59c6f9e7ec6c40c938fda2665bbae5685e7de830
ms.sourcegitcommit: 4cf434b126eb7df6b2fd9bb1d71613bf2b6aac0e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/07/2019
ms.locfileid: "71997216"
---
# <a name="xamarinforms-layouts"></a>Макеты Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_Макеты Xamarin.Forms используются для создания элементов управления пользовательского интерфейса в visual структуры._

[ `Layout` ](xref:Xamarin.Forms.Layout) И [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) классы в Xamarin.Forms — специализированные подтипы представлений, которые являются контейнерами для представлений и других значений. `Layout` Класс сам является производным от [ `View` ](views.md). Объект `Layout` производных обычно содержит логику, чтобы задать положение и размер дочерних элементов в приложениях Xamarin.Forms.

[![Типы макетов Xamarin. Forms]типы(layouts-images/layouts-sml.png "макета Xamarin. Forms")](layouts-images/layouts.png#lightbox "Типы макета Xamarin. Forms")

Классы, производные от `Layout` можно разделить на две категории:

## <a name="layouts-with-single-content"></a>Макеты с одиночное содержимое

Эти классы являются производными от [ `Layout` ](xref:Xamarin.Forms.Layout), который определяет [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) и [ `IsClippedToBounds` ](xref:Xamarin.Forms.Layout.IsClippedToBounds) свойства.

<a name="contentView" />

### <a name="contentview"></a>ContentView

|     |     |
| --- | --- |
| [`ContentView`](xref:Xamarin.Forms.ContentView) содержит один дочерний элемент, в котором заданы [ `Content` ](xref:Xamarin.Forms.ContentView.Content) свойство. `Content` Свойство может устанавливаться к любому `View` производных продуктов, включая другие `Layout` производные от него. `ContentView` обычно используется как элемент структурного и служит в качестве базового класса для [ `Frame` ](#frame).<br /><br />[Документация по API](xref:Xamarin.Forms.ContentView) / [руководство](~/xamarin-forms/user-interface/layouts/contentview.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-cardview/) | [Пример(layouts-images/ContentView.png "ContentView") . пример ![ContentView]](layouts-images/ContentView-Large.png#lightbox "Пример ContentView")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentViewDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentViewDemoPage.xaml) |
|     |     |

<a named="frame" />

### <a name="frame"></a>Frame

|     |     |
| --- | --- |
| Класс [`Frame`](xref:Xamarin.Forms.Frame) является производным от [`ContentView`](#contentView) и отображает границу (рамку) вокруг ее дочернего элемента. Класс `Frame` имеет значение по умолчанию [`Padding`](xref:Xamarin.Forms.Layout.Padding) , равное 20, а также определяет свойства [`BorderColor`](xref:Xamarin.Forms.Frame.BorderColor), [`CornerRadius`](xref:Xamarin.Forms.Frame.CornerRadius)и [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow) .<br /><br />[Документация по API](xref:Xamarin.Forms.Frame) / [руководство](~/xamarin-forms/user-interface/layouts/frame.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-frame/) | [![](layouts-images/Frame.png "Пример кадра") example](layouts-images/Frame-Large.png#lightbox "Пример кадра")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FrameDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FrameDemoPage.xaml) |
|     |     |

<a name="scrollView" />

### <a name="scrollview"></a>ScrollView

|     |     |
| --- | --- |
| [`ScrollView`](xref:Xamarin.Forms.ScrollView) возможна прокрутка его содержимое. Задайте [ `Content` ](xref:Xamarin.Forms.ScrollView.Content) свойство к представлению или макета слишком большой помещаются на экране. (Содержимое `ScrollView` очень часто [ `StackLayout` ](#stackLayout).) Задайте [ `Orientation` ](xref:Xamarin.Forms.ScrollView.Orientation) свойство для указания если прокрутка по вертикали, горизонтали или оба.<br /><br />[Документация по API](xref:Xamarin.Forms.ScrollView) / [руководство](~/xamarin-forms/user-interface/layouts/scroll-view.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [Пример(layouts-images/ScrollView.png "скроллвиев") . пример ![скроллвиев]](layouts-images/ScrollView-Large.png#lightbox "Пример Скроллвиев")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ScrollViewDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ScrollViewDemoPage.xaml) |
|     |     |

### <a name="templatedview"></a>TemplatedView

|     |     |
| --- | --- |
| [`TemplatedView`](xref:Xamarin.Forms.TemplatedView) Отображает содержимое с помощью шаблона элемента управления, и является базовым классом для [ `ContentView` ](#contentView).<br /><br />[Документация по API](xref:Xamarin.Forms.TemplatedView) / [руководство](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [Пример(layouts-images/TemplatedView.png "темплатедвиев") . пример ![темплатедвиев]](layouts-images/TemplatedView.png#lightbox "Пример Темплатедвиев") |
|     |     |

### <a name="contentpresenter"></a>ContentPresenter

|     |     |
| --- | --- |
| [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) является руководителем макета для шаблонного представлений, используемых в [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) для пометки, где отображается содержимое, должна присутствовать.<br /><br />[Документация по API](xref:Xamarin.Forms.ContentPresenter) / [руководство](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [Пример(layouts-images/ContentPresenter.png "ContentPresenter") . пример ![ContentPresenter]](layouts-images/ContentPresenter.png#lightbox "Пример ContentPresenter") |
|     |     |

## <a name="layouts-with-multiple-children"></a>Макеты с нескольких дочерних элементов

Эти классы являются производными от [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1).

<a name="stackLayout" />

### <a name="stacklayout"></a>StackLayout

|     |     |
| --- | --- |
| [`StackLayout`](xref:Xamarin.Forms.StackLayout) размещает дочерние элементы в стеке, либо горизонтально или вертикально на основании [ `Orientation` ](xref:Xamarin.Forms.StackLayout.Orientation) свойство. [ `Spacing` ](xref:Xamarin.Forms.StackLayout.Spacing) Свойство определяет расстояние между дочерние элементы и имеет значение по умолчанию 6.<br /><br />[Документация по API](xref:Xamarin.Forms.StackLayout) / [руководство](~/xamarin-forms/user-interface/layouts/stack-layout.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)| [Пример(layouts-images/StackLayout.png "StackLayout") . пример ![StackLayout]](layouts-images/StackLayout-Large.png#lightbox "Пример StackLayout")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StackLayoutDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StackLayoutDemoPage.xaml) |
|     |     |

<a name="grid" />

### <a name="grid"></a>Grid

|     |     |
| --- | --- |
| [`Grid`](xref:Xamarin.Forms.Grid) помещает его дочерних элементов в сетке из строк и столбцов. Положение дочернего элемента указывается с помощью [присоединенных свойств](~/xamarin-forms/xaml/attached-properties.md) [ `Row` ](xref:Xamarin.Forms.Grid.RowProperty), [ `Column` ](xref:Xamarin.Forms.Grid.ColumnProperty), [ `RowSpan` ](xref:Xamarin.Forms.Grid.RowSpanProperty), и [ `ColumnSpan` ](xref:Xamarin.Forms.Grid.ColumnSpanProperty).<br /><br />[Документация по API](xref:Xamarin.Forms.Grid) / [руководство](~/xamarin-forms/user-interface/layouts/grid.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![](layouts-images/Grid.png "Пример сетки") в примере таблицы](layouts-images/Grid-Large.png#lightbox "Пример сетки")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/GridDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/GridDemoPage.xaml) |
|     |     |

### <a name="absolutelayout"></a>AbsoluteLayout

|     |     |
| --- | --- |
| [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) размещает дочерние элементы в определенные расположения относительно его родительского элемента. Положение дочернего элемента указывается с помощью [присоединенных свойств](~/xamarin-forms/xaml/attached-properties.md) [ `LayoutBounds` ](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) и [ `LayoutFlags` ](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty). `AbsoluteLayout` Удобно для анимации положения представления.<br /><br />[Документация по API](xref:Xamarin.Forms.AbsoluteLayout) / [руководство](~/xamarin-forms/user-interface/layouts/absolute-layout.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [Пример(layouts-images/AbsoluteLayout.png "абсолутелайаут") . пример ![абсолутелайаут]](layouts-images/AbsoluteLayout-Large.png#lightbox "Пример Абсолутелайаут")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/AbsoluteLayoutdDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml.cs) |
|     |     |

### <a name="relativelayout"></a>RelativeLayout

|     |     |
| --- | --- |
| [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) размещает дочерние элементы относительно `RelativeLayout` себя или своих одноуровневых элементов. Положение дочернего элемента указывается с помощью [присоединенных свойств](~/xamarin-forms/xaml/attached-properties.md) , которые задаются для объектов типа [ `Constraint` ](xref:Xamarin.Forms.Constraint) и [ `BoundsConstraint` ](xref:Xamarin.Forms.Constraint).<br /><br />[Документация по API](xref:Xamarin.Forms.RelativeLayout) / [руководство](~/xamarin-forms/user-interface/layouts/relative-layout.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [Пример(layouts-images/RelativeLayout.png "RelativeLayout") . пример ![RelativeLayout]](layouts-images/RelativeLayout-Large.png#lightbox "Пример RelativeLayout")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RelativeLayoutDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RelativeLayoutDemoPage.xaml) |
|     |     |

### <a name="flexlayout"></a>FlexLayout

|     |     |
| --- | --- |
| [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) основан на CSS [гибкий модуль макета рамок](http://www.w3.org/TR/css-flexbox-1/), известный как _гибкий макет_ или _flex-box_. `FlexLayout` Определяет шесть привязываемые свойства и пять вложенных привязываемые свойства, которые позволяют дочерним элементам с накоплением и оболочку с многие параметры выравнивания и ориентацию.<br /><br />[Документация по API](xref:Xamarin.Forms.FlexLayout) / [руководство](~/xamarin-forms/user-interface/layouts/flex-layout.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos) | [Пример(layouts-images/FlexLayout.png "флекслайаут") . пример ![флекслайаут]](layouts-images/FlexLayout-Large.png#lightbox "Пример Флекслайаут")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FlexLayoutDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FlexLayoutDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Связанные ссылки

- [Пример Xamarin.Forms FormsGallery](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Примеры Xamarin.Forms](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Документация по API Xamarin.Forms](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
