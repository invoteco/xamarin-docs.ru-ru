---
title: Страницы Xamarin. Forms
description: Страницы Xamarin. Forms представляют собой межплатформенные экраны мобильных приложений. В этой статье перечислены страницы, которые включены в Xamarin. Forms.
ms.prod: xamarin
ms.assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 278256f75d94fe47510ae4d15f12a3ff3a6a2b19
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "69976445"
---
# <a name="xamarinforms-pages"></a>Страницы Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Страницы Xamarin. Forms представляют собой межплатформенные экраны мобильных приложений._

Все типы страниц, описанные ниже, являются производными от класса [`Page`](xref:Xamarin.Forms.Page) Xamarin. Forms. Эти визуальные элементы занимают весь экран или большую часть экрана. Объект `Page` представляет `ViewController` в iOS и `Page` в универсальная платформа Windows. На Android каждая страница занимает экран, как `Activity`, но страницы Xamarin. Forms *не* `Activity` объекты.

[![](pages-images/pages-sml.png "Xamarin.Forms Page Types")](pages-images/pages.png#lightbox "Xamarin.Forms Page Types")

## <a name="pages"></a>Pages

Xamarin. Forms поддерживает следующие типы страниц:

<a name="contentPage" />

### <a name="contentpage"></a>ContentPage

|     |     |
| --- | --- |
| [`ContentPage`](xref:Xamarin.Forms.ContentPage) — самый простой и распространенный тип страницы. Задайте для свойства [`Content`](xref:Xamarin.Forms.ContentPage.Content) один объект [`View`](views.md) , который чаще всего представляет собой [`Layout`](layouts.md) например [`StackLayout`](layouts.md#stackLayout), [`Grid`](layouts.md#grid)или [1](layouts.md#scrollView).<br /><br />[Документация по API](xref:Xamarin.Forms.ContentPage) | [![Пример ContentPage](pages-images/ContentPage.png "Пример ContentPage")](pages-images/ContentPage-Large.png#lightbox "Пример ContentPage")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     |
| --- | --- |
| [@No__t_1](xref:Xamarin.Forms.MasterDetailPage) управляет двумя областями информации. Задайте для свойства [`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) страницу, обычно отображающую список или меню. Задайте для свойства [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) страницу, показывающую выбранный элемент на главной странице. Свойство [`IsPresented`](xref:Xamarin.Forms.MasterDetailPage.IsPresented) определяет, видима ли страница «основной» или «подробности».<br /><br />[Документация по API](xref:Xamarin.Forms.MasterDetailPage) / [руководство](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-masterdetailpage) | [![Пример Мастердетаилпаже](pages-images/MasterDetailPage.png "Пример Мастердетаилпаже")](pages-images/MasterDetailPage-Large.png#lightbox "Пример Мастердетаилпаже")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs) |
|     |     |

### <a name="navigationpage"></a>NavigationPage

|     |     |
| --- | --- |
| [@No__t_1](xref:Xamarin.Forms.NavigationPage) управляет навигацией между другими страницами с помощью архитектуры на основе стека. При использовании навигации по страницам в приложении экземпляр домашней страницы должен быть передан конструктору объекта `NavigationPage`.<br /><br />[Документация по API](xref:Xamarin.Forms.NavigationPage)  / [руководство](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)  / [Пример 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-hierarchical), [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-passingdata)и [3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-loginflow)  | [![Пример Навигатионпаже](pages-images/NavigationPage.png "Пример Навигатионпаже")](pages-images/NavigationPage-Large.png#lightbox "Пример Навигатионпаже")<br />код для этой страницы  / [страницу XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml) с [кодом = Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs) |
|     |     |

### <a name="tabbedpage"></a>TabbedPage

|     |     |
| --- | --- |
| [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) является производным от абстрактного [`MultiPage`](xref:Xamarin.Forms.MultiPage`1) класса и позволяет перемещаться между дочерними страницами с помощью вкладок. Задайте для свойства [`Children`](xref:Xamarin.Forms.MultiPage`1.Children) коллекцию страниц или задайте для свойства [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) коллекцию объектов данных, а свойству [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) — [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , описывающий, как визуально представить каждый объект.<br /><br />[Документация по API](xref:Xamarin.Forms.TabbedPage)  / [руководство](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)  / [Пример 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpage) и [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpagewithnavigationpage) | [![Пример Таббедпаже](pages-images/TabbedPage.png "Пример Таббедпаже")](pages-images/TabbedPage-Large.png#lightbox "Пример Таббедпаже")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     |
| --- | --- |
| [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) является производным от абстрактного [`MultiPage`](xref:Xamarin.Forms.MultiPage`1) класса и позволяет перемещаться между дочерними страницами посредством прокрутки пальца. Задайте для свойства [`Children`](xref:Xamarin.Forms.MultiPage`1.Children) коллекцию объектов [`ContentPage`](#contentPage) или задайте для свойства [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) коллекцию объектов данных, а для свойства [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) — [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , описывающий, как визуально каждый объект. приведен.<br /><br />[Документация по API](xref:Xamarin.Forms.CarouselPage)  / [руководство](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md)  / [Пример 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpage) и [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpagetemplate) | [![Пример Карауселпаже](pages-images/CarouselPage.png "Пример Карауселпаже")](pages-images/CarouselPage-Large.png#lightbox "Пример Карауселпаже")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) |
|     |     |

### <a name="templatedpage"></a>темплатедпаже

|     |     |
| --- | --- |
| [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage) отображает полноэкранное содержимое с помощью шаблона элемента управления, а является базовым классом для [`ContentPage`](#contentPage).<br /><br />[Документация по API](xref:Xamarin.Forms.TemplatedPage) / [руководство](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![Пример Темплатедпаже](pages-images/TemplatedPage.png "Пример Темплатедпаже")](pages-images/TemplatedPage.png "Пример Темплатедпаже") |
|     |     |

## <a name="related-links"></a>Связанные ссылки

- [Пример Xamarin.Forms FormsGallery](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Примеры Xamarin.Forms](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Документация по API Xamarin.Forms](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
