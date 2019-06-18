---
title: Навигация в Xamarin.Forms
description: В этом руководстве содержатся сведения о переходах в приложениях Xamarin.Forms. Xamarin.Forms предоставляет ряд различных способов перехода по страницам в зависимости от используемого типа страницы.
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 682e3bd0ac4cdd651203496dd28586db2cef3165
ms.sourcegitcommit: c2bba24233624c2ec0e9ee9827310ca022212a2c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/11/2019
ms.locfileid: "66835266"
---
# <a name="xamarinforms-navigation"></a>Навигация в Xamarin.Forms

_Xamarin.Forms предоставляет ряд различных способов перехода по страницам в зависимости от используемого типа страницы._

![](images/page-types.png "Типы страниц в Xamarin.Forms")

Кроме того, приложения оболочки Xamarin.Forms предоставляют улучшенные возможности навигации на основе URI по интерфейсу без соблюдения строгой заданной иерархии. Дополнительные сведения см. в статье [о навигации в оболочке Xamarin.Forms](~/xamarin-forms/app-fundamentals/shell/navigation.md).

## <a name="hierarchical-navigationhierarchicalmd"></a>[Иерархическая навигация](hierarchical.md)

Класс [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) обеспечивает иерархическую навигацию, при которой пользователь может переходить по страницам вперед и назад по своему желанию. Этот класс реализует навигацию на основе стека объектов [`Page`](xref:Xamarin.Forms.Page) по методу LIFO (последним поступил — первым обслужен).

## <a name="tabbedpagetabbed-pagemd"></a>[TabbedPage](tabbed-page.md)

Xamarin.Forms [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) состоит из списка вкладок и большой области сведений, где каждая вкладка загружает содержимое в область сведений.

## <a name="carouselpagecarousel-pagemd"></a>[CarouselPage](carousel-page.md)

Xamarin.Forms [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) — это страница, по которой пользователи могут проводить из стороны в сторону, чтобы переходить по страницам содержимого, например по страницам коллекции.

## <a name="masterdetailpagemaster-detail-pagemd"></a>[MasterDetailPage](master-detail-page.md)

Xamarin.Forms [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) представляет собой страницу, управляющую двумя страницами связанных данных — главной страницей, которая представляет элементы, и страницей сведений, которая представляет сведения об элементах на главной странице.

## <a name="modal-pagesmodalmd"></a>[Модальные страницы](modal.md)

Xamarin.Forms также поддерживает модальные страницы. На модальной странице пользователь должен выполнить отдельную задачу, причем он не может уйти с этой страницы, пока задача не будет выполнена или отменена.
