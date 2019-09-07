---
title: Макеты с вкладками
description: Обзор макетов с вкладками в Android
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/08/2017
ms.openlocfilehash: 1e0632eb921b25fef40b8f0483ab80d62c9e1235
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762192"
---
# <a name="tabbed-layouts"></a>Макеты с вкладками

## <a name="overview"></a>Обзор

Вкладки — это популярный шаблон пользовательского интерфейса в мобильных приложениях из-за простоты и удобства использования. Они обеспечивают единообразный и простой способ перехода между различными экранами в приложении. У Android есть несколько API для интерфейсов с вкладками: 

- **Актионбар** &ndash; Это часть нового набора API, представленного в Android 3,0 (API уровня 11) с целью предоставления единообразного интерфейса навигации и переключения просмотра. Он был перенесен на Android 2,2 (API уровня 8) с помощью [библиотеки поддержки Android версии 7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/). 

- **Пажертабстрип** Указывает на текущую, следующую и предыдущую страницы `ViewPager`. &ndash; `ViewPager`доступно только через [библиотеку поддержки Android версии 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).
     Дополнительные сведения о `PagerTabStrip`см. в разделе [ViewPager](~/android/user-interface/controls/view-pager/index.md).

- **Панель инструментов** — Это более новый и гибкий компонент панели действий, который `ActionBar`заменяет. &ndash; `Toolbar` `Toolbar`доступен в версии Android 5,0 без описания операций или более поздней версии. Она также доступна для более старых версий Android с помощью версии 7 пакета NuGet [библиотеки поддержки Android](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) . 
    `Toolbar`в настоящее время является рекомендуемым компонентом панели действий для использования в приложениях Android.
    Дополнительные сведения см. в разделе [панель инструментов](~/android/user-interface/controls/tool-bar/index.md). 

## <a name="related-links"></a>Связанные ссылки

- [Проектирование материалов — вкладки](https://material.io/guidelines/components/tabs.html)- [актионбар](https://developer.android.com/guide/topics/ui/actionbar.html)
- [Пакет NuGet библиотеки поддержки Android версии 7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [Библиотека AppCompat версии 7](https://developer.android.com/tools/support-library/features.html#v7-appcompat)
