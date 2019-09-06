---
title: Создание кроссплатформенных приложений
description: В этом разделе рассказывается о том, как создавать приложения с помощью платформы разработки Xamarin, а также как работать с приложением Xamarin для разработки мобильных приложений, а затем тестировать и развертывать их в различных магазинах приложений.
ms.prod: xamarin
ms.assetid: 442FC40A-84DD-A218-0D15-EAD86594B6D7
author: conceptdev
ms.author: crdun
ms.date: 01/28/2016
ms.openlocfilehash: 2c630e58ff5a1221ec9e95099df4781041ecf2b4
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70288054"
---
# <a name="building-cross-platform-applications"></a>Создание кроссплатформенных приложений

Существует два варианта совместного использования кода между несколькими платформами для мобильных приложений: Проекты общих ресурсов и переносимые библиотеки классов. Эти параметры [обсуждаются здесь](~/cross-platform/app-fundamentals/code-sharing.md). также доступны дополнительные сведения о [переносимых библиотеках классов](~/cross-platform/app-fundamentals/pcl.md) и [общих проектах](~/cross-platform/app-fundamentals/shared-projects.md) .

<a name="Sections" />

 [Обзор](~/cross-platform/app-fundamentals/building-cross-platform-applications/overview.md)

 [Часть 1. Общие сведения о платформе Xamarin Mobile](~/cross-platform/app-fundamentals/building-cross-platform-applications/understanding-the-xamarin-mobile-platform.md)

 [Часть 2 — Архитектура](~/cross-platform/app-fundamentals/building-cross-platform-applications/architecture.md)

 [Часть 3. Настройка решения для кросс-платформенной платформы Xamarin](~/cross-platform/app-fundamentals/building-cross-platform-applications/setting-up-a-xamarin-cross-platform-solution.md)

 [Часть 4. Работа с несколькими платформами](~/cross-platform/app-fundamentals/building-cross-platform-applications/platform-divergence-abstraction-divergent-implementation.md)

 [Часть 5. практические рекомендации по совместному использованию кода](~/cross-platform/app-fundamentals/building-cross-platform-applications/practical-code-sharing-strategies.md)

 [Часть 6. Тестирование и утверждение магазина приложений](~/cross-platform/app-fundamentals/building-cross-platform-applications/testing-and-app-store-approvals.md)

 <a name="Cross-Platform_Mobile_Application_Case_Studies" />

## <a name="case-studies"></a>Примеры использования

Принципы, описанные в этом документе, приводятся на практике к *задачам*примера приложения, а также к [предварительно созданным приложениям](https://xamarin.com/prebuilt) , таким как [Xamarin CRM](https://xamarin.com/prebuilt/#xamarincrm).

 <a name="Tasky" />

### <a name="tasky"></a>Tasky

Задача — это простое приложение списка задач для iOS, Android и Windows Phone.
В нем демонстрируются основы создания кросс-платформенного приложения с помощью Xamarin и используется локальная база данных SQLite.

 Список задач списка задач [ ![](images/iphone-list-sml.png)](images/iphone-list.png#lightbox) [ ![](images/iphone-list-sml.png)](images/iphone-list.png#lightbox)

Прочтите [Пример использования задач](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md).

## <a name="summary"></a>Сводка

В этом разделе описываются средства разработки приложений Xamarin и обсуждаются способы создания приложений, предназначенных для нескольких мобильных платформ.

Она охватывает многоуровневую архитектуру, которая позволяет структурировать код для повторного использования на нескольких платформах и описывает различные шаблоны программного обеспечения, которые могут использоваться в этой архитектуре.

Примеры представляют собой общие функции приложений (такие как файловые и сетевые операции), а также способы их построения на кросс-платформенном уровне.

И, наконец, он кратко обсуждает тестирование и предоставляет ссылки на пример использования, в котором эти принципы помещаются в действие.

## <a name="related-links"></a>Связанные ссылки

- [Параметры общего доступа к коду](~/cross-platform/app-fundamentals/code-sharing.md)
- [Пример использования: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Пример приложения для задач (GitHub)](https://docs.microsoft.com/samples/xamarin/mobile-samples/taskyportable/)
- [Разработка мобильных приложений Xamarin: Основы кросс- C# платформенного и Xamarin. Forms (Amazon)](http://www.amazon.com/Xamarin-Mobile-Application-Development-Cross-Platform/dp/1484202155/)
- [Разработка мобильных приложений C# с помощью Грег Шакклес (O'Reilly)](http://shop.oreilly.com/product/0636920024002.do)
- [Профессиональная кросс-платформенная разработка мобильных C# приложений в Скотт Олсон, Джон Хантер, Бен Хоржен, Кенни вернувшиеся (Wrox)](http://www.wrox.com/WileyCDA/WroxTitle/Professional-Cross-Platform-Mobile-Development-in-C-.productCd-1118157702.html)
