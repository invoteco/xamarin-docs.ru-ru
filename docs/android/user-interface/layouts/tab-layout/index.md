---
title: Макеты с вкладками
description: Обзор макетов с вкладками в Android
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/08/2017
ms.openlocfilehash: 4ca4200d0f9036ed76e20e3a1840303e7bb3b7e3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028779"
---
# <a name="tabbed-layouts"></a>Макеты с вкладками

## <a name="overview"></a>Обзор

Вкладки — это популярный шаблон пользовательского интерфейса в мобильных приложениях из-за простоты и удобства использования. Они обеспечивают единообразный и простой способ перехода между различными экранами в приложении. У Android есть несколько API для интерфейсов с вкладками: 

- **Актионбар** &ndash; это является частью нового набора API, который появился в Android 3,0 (API уровня 11) с целью предоставления единообразного интерфейса навигации и переключения просмотра. Он был перенесен на Android 2,2 (API уровня 8) с помощью [библиотеки поддержки Android версии 7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/). 

- **Пажертабстрип** &ndash; указывает на текущую, следующую и предыдущую страницы `ViewPager`. `ViewPager` доступен только через [библиотеку поддержки Android версии 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).
     Дополнительные сведения о `PagerTabStrip`см. в разделе [ViewPager](~/android/user-interface/controls/view-pager/index.md).

- **Панель инструментов** &ndash; `Toolbar` — это более новый и гибкий компонент панели действий, который заменяет `ActionBar`. `Toolbar` доступна в интерфейсе поддержки Android 5,0 или более поздней версии. Кроме того, она также доступна для более ранних версий Android с помощью пакета NuGet [версии 7 Library (поддержка Android](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) ). 
    в настоящее время `Toolbar` является рекомендуемым компонентом панели действий для использования в приложениях Android.
    Дополнительные сведения см. в разделе [панель инструментов](~/android/user-interface/controls/tool-bar/index.md). 

## <a name="related-links"></a>Связанные ссылки

- [Проектирование материалов — вкладки](https://material.io/guidelines/components/tabs.html)- [актионбар](https://developer.android.com/guide/topics/ui/actionbar.html)
- [Пакет NuGet библиотеки поддержки Android версии 7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [Библиотека AppCompat версии 7](https://developer.android.com/tools/support-library/features.html#v7-appcompat)
