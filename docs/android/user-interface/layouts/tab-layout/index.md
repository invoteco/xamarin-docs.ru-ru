---
title: Макеты с вкладками
description: Общие сведения о макеты с вкладками в Android
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/08/2017
ms.openlocfilehash: 32d1ce4e440a962e02fda052375171bea7676053
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670163"
---
# <a name="tabbed-layouts"></a>Макеты с вкладками


## <a name="overview"></a>Обзор

Вкладки — шаблон популярных пользовательской интерфейс в мобильных приложениях, из-за их простоту и удобство использования. Они предоставляют согласованный и простой способ перемещаться между экранов в приложении. Android имеет несколько API для интерфейсов с вкладками. 

-   **ActionBar** &ndash; это является частью нового набора API-интерфейсов, впервые появился в Android 3.0 (уровень API 11) с целью предоставления согласованного навигации и переключения интерфейса. Он был обратно перенесен на Android 2.2 (API уровня 8) с [библиотеки поддержки Android версии 7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/). 

-   **PagerTabStrip** &ndash; показывает current, следующей и предыдущей страницы `ViewPager`. `ViewPager` доступен только через [библиотеки поддержки Android v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).
     Дополнительные сведения о `PagerTabStrip`, см. в разделе [ViewPager](~/android/user-interface/controls/view-pager/index.md).

-   **Панель инструментов** &ndash; `Toolbar` является компонентом панели действия более новые и более гибкая, который заменяет `ActionBar`. `Toolbar` доступна в Android 5.0 Lollipop или более поздней версии, и она также доступна для более старых версиях Android через [библиотеки поддержки Android версии 7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) пакет NuGet. 
    `Toolbar` в настоящее время является компонентом панели Рекомендуемое действие для использования в приложениях Android.
    Дополнительные сведения см. в разделе [инструментов](~/android/user-interface/controls/tool-bar/index.md). 



## <a name="related-links"></a>Связанные ссылки

- [Вкладки материального исполнения -](https://material.io/guidelines/components/tabs.html)- [ActionBar](https://developer.android.com/guide/topics/ui/actionbar.html)
- [Пакет NuGet версии 7 совместимости приложений для Android, поддерживают библиотеки](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [Библиотека appcompat версии 7](https://developer.android.com/tools/support-library/features.html#v7-appcompat)
