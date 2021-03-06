---
title: Страницы Xamarin.Forms
description: Страницы Xamarin.Forms представляют экраны кросс платформенных мобильных приложений. В этой статье перечислены страницы, которые включены в Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 101b7aa014064e07de69e8c6b12f0ad1000ae0dd
ms.sourcegitcommit: 211fed94fb96127a3e158ae1ff5d7eb831a203d8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/15/2020
ms.locfileid: "75955741"
---
# <a name="xamarinforms-pages"></a>Страницы Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Страницы Xamarin.Forms представляют экраны кросс платформенных мобильных приложений._

Все типы страниц, которые описаны ниже, являются производными от Xamarin.Forms [ `Page` ](xref:Xamarin.Forms.Page) класса. Эти визуальные элементы занимают всего или большей части экрана. Объект `Page` представляет объект `ViewController` в iOS и `Page` в универсальной платформы Windows. В Android, каждая страница занимает экран, похожий `Activity`, но страницы Xamarin.Forms являются *не* `Activity` объектов.

[![](pages-images/pages-sml.png "Xamarin.Forms Page Types")](pages-images/pages.png#lightbox "Xamarin.Forms Page Types")

## <a name="pages"></a>Pages

Xamarin.Forms поддерживает следующие типы страниц:

<a name="contentPage" />

### <a name="contentpage"></a>ContentPage

|     |     |
| --- | --- |
| [`ContentPage`](xref:Xamarin.Forms.ContentPage) — Это самый простой и наиболее распространенный тип страницы. Задайте [ `Content` ](xref:Xamarin.Forms.ContentPage.Content) свойство к одному [ `View` ](views.md) объект, который чаще всего [ `Layout` ](layouts.md) например [ `StackLayout` ](layouts.md#stackLayout), [ `Grid` ](layouts.md#grid), или [ `ScrollView` ](layouts.md#scrollView).<br /><br />[Документация по API](xref:Xamarin.Forms.ContentPage) | [![Пример ContentPage](pages-images/ContentPage.png "Пример ContentPage")](pages-images/ContentPage-Large.png#lightbox "Пример ContentPage")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     |
| --- | --- |
| Объект [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) управляет двух областей данных. Задайте [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) свойство на страницу, обычно отображается в списке или меню. Задайте [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) свойства страница со списком выбранный элемент из главной страницы. [ `IsPresented` ](xref:Xamarin.Forms.MasterDetailPage.IsPresented) Регулирует ли страница основной или подробности.<br /><br />[Документация по API](xref:Xamarin.Forms.MasterDetailPage) / [руководство](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-masterdetailpage) | [![Пример Мастердетаилпаже](pages-images/MasterDetailPage.png "Пример Мастердетаилпаже")](pages-images/MasterDetailPage-Large.png#lightbox "Пример Мастердетаилпаже")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs) |
|     |     |

### <a name="navigationpage"></a>NavigationPage

|     |     |
| --- | --- |
| [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) Управляет перемещением других страниц, используя архитектуру на основе стека. При использовании Навигация по страницам в приложении, домашней страницы экземпляра должно передаваться конструктору `NavigationPage` объекта.<br /><br />[Документация по API](xref:Xamarin.Forms.NavigationPage) / [руководство](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) / [пример 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-hierarchical), [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-passingdata), и [3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-loginflow)  | [![Пример Навигатионпаже](pages-images/NavigationPage.png "Пример Навигатионпаже")](pages-images/NavigationPage-Large.png#lightbox "Пример Навигатионпаже")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml) с [код = за](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) |
|     |     |

### <a name="tabbedpage"></a>TabbedPage

|     |     |
| --- | --- |
| [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) является производным от абстрактного [ `MultiPage` ](xref:Xamarin.Forms.MultiPage`1) класса и позволяет навигацию между дочерние страницы, с помощью вкладок. Задайте [ `Children` ](xref:Xamarin.Forms.MultiPage`1.Children) свойство в коллекцию страниц или набора [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) свойство в коллекцию объектов данных и [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) свойства [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) описывающее как визуально представить каждый объект.<br /><br />[Документация по API](xref:Xamarin.Forms.TabbedPage) / [руководство](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) / [пример 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpage) и [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpagewithnavigationpage) | [![Пример Таббедпаже](pages-images/TabbedPage.png "Пример Таббедпаже")](pages-images/TabbedPage-Large.png#lightbox "Пример Таббедпаже")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     |
| --- | --- |
| [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) является производным от абстрактного [ `MultiPage` ](xref:Xamarin.Forms.MultiPage`1) класса и позволяет навигацию между дочерние страницы посредством считывания палец. Задайте [ `Children` ](xref:Xamarin.Forms.MultiPage`1.Children) свойство в коллекцию [ `ContentPage` ](#contentPage) объектов или набор [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) свойство в коллекцию объектов данных и [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) свойства [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) описывающее как визуально представить каждый объект.<br /><br />[Документация по API](xref:Xamarin.Forms.CarouselPage) / [руководство](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md) / [пример 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpage) и [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpagetemplate) | [![Пример Карауселпаже](pages-images/CarouselPage.png "Пример Карауселпаже")](pages-images/CarouselPage-Large.png#lightbox "Пример Карауселпаже")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) |
|     |     |

### <a name="templatedpage"></a>TemplatedPage

|     |     |
| --- | --- |
| [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage) Отображает содержимое весь экран с помощью шаблона элемента управления, и является базовым классом для [ `ContentPage` ](#contentPage).<br /><br />[Документация по API](xref:Xamarin.Forms.TemplatedPage) / [руководство](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![Пример Темплатедпаже](pages-images/TemplatedPage.png "Пример Темплатедпаже")](pages-images/TemplatedPage.png "Пример Темплатедпаже") |
|     |     |

## <a name="related-links"></a>Связанные ссылки

- [Пример Xamarin.Forms FormsGallery](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Примеры Xamarin.Forms](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Документация по API Xamarin.Forms](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
