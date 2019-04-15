---
title: Создание кроссплатформенных приложений
description: В этом разделе описывается, сводку, а также шести частей, создание приложений с помощью платформы разработки Xamarin — от понимания, как работает Xamarin для разработки мобильных приложений и затем протестировать и развернуть в различных магазинов приложений.
ms.prod: xamarin
ms.assetid: 442FC40A-84DD-A218-0D15-EAD86594B6D7
author: asb3993
ms.author: amburns
ms.date: 01/28/2016
ms.openlocfilehash: 683400e24844308769f0562552641216d45e7d11
ms.sourcegitcommit: 91a4fcb715506e18e8070bc89bf2cb14d079ad32
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/15/2019
ms.locfileid: "59574797"
---
# <a name="building-cross-platform-applications"></a>Создание кроссплатформенных приложений

Существует два варианта для совместного использования кода между кросс платформенных мобильных приложений: Общие проекты активов и переносимых библиотек классов. Эти параметры являются [рассматриваемые здесь](~/cross-platform/app-fundamentals/code-sharing.md); Дополнительные сведения о [переносимые библиотеки классов](~/cross-platform/app-fundamentals/pcl.md) и [проектах общих](~/cross-platform/app-fundamentals/shared-projects.md) также доступна.

<a name="Sections" />

 [Обзор](~/cross-platform/app-fundamentals/building-cross-platform-applications/overview.md)

 [Часть 1 – основные сведения о мобильной платформе Xamarin](~/cross-platform/app-fundamentals/building-cross-platform-applications/understanding-the-xamarin-mobile-platform.md)

 [Часть 2 — архитектура](~/cross-platform/app-fundamentals/building-cross-platform-applications/architecture.md)

 [Часть 3 – Настройка кроссплатформенного решения Xamarin](~/cross-platform/app-fundamentals/building-cross-platform-applications/setting-up-a-xamarin-cross-platform-solution.md)

 [Часть 4 – Работа с несколькими платформами](~/cross-platform/app-fundamentals/building-cross-platform-applications/platform-divergence-abstraction-divergent-implementation.md)

 [Часть 5 – Практические стратегии совместного использования кода](~/cross-platform/app-fundamentals/building-cross-platform-applications/practical-code-sharing-strategies.md)

 [Часть 6. Тестирование и утверждение магазина приложений](~/cross-platform/app-fundamentals/building-cross-platform-applications/testing-and-app-store-approvals.md)

 <a name="Cross-Platform_Mobile_Application_Case_Studies" />

## <a name="case-studies"></a>Примеры внедрения

Принципам, описанным в этом документе, помещаются в практику в приложении-примере *Tasky*, а также [предварительно созданные приложения](https://xamarin.com/prebuilt) как [Xamarin CRM](https://xamarin.com/prebuilt/#xamarincrm).

 <a name="Tasky" />

### <a name="tasky"></a>Tasky

Tasky простое приложение списка задач для iOS, Android и Windows Phone.
Он демонстрирует основные принципы создания кросс платформенные приложения с помощью Xamarin и использует локальной базы данных SQLite.

 [![Список tasky](images/iphone-list-sml.png)](images/iphone-list.png#lightbox) [ ![tasky списка](images/iphone-list-sml.png)](images/iphone-list.png#lightbox)

Чтение [Tasky практический пример](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md).

## <a name="summary"></a>Сводка

В этом разделе представлены средства разработки приложений Xamarin и обсуждаются способы разработки приложений, предназначенных для нескольких платформ мобильных устройств.

Он охватывает многоуровневой архитектуры структуры кода для повторного использования на разных платформах и описание шаблонов другом программном обеспечении, которые могут использоваться в пределах этой архитектуры.

Примеры приведены общие функции приложения (например, файл и сетевые операции) и как они могут строиться образом между различными платформами.

Наконец она кратко рассматривается проверка и предоставляет ссылки на обучающий материал, помещает эти принципы в действие.

## <a name="related-links"></a>Связанные ссылки

- [Варианты совместного использования кода](~/cross-platform/app-fundamentals/code-sharing.md)
- [Пример внедрения: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky пример приложения (github)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [Разработка приложений Xamarin Mobile: Кросс платформенные C# и принципы работы Xamarin.Forms (Amazon)](http://www.amazon.com/Xamarin-Mobile-Application-Development-Cross-Platform/dp/1484202155/)
- [Разработка мобильных приложений с C# , Грег Shackles (O'Reilly)](http://shop.oreilly.com/product/0636920024002.do)
- [Профессиональные кросс платформенной мобильной разработки в C# по Олсон Скотт Хантер Джон, Бен Horgen, Кенни Вернувшиеся (Wrox)](http://www.wrox.com/WileyCDA/WroxTitle/Professional-Cross-Platform-Mobile-Development-in-C-.productCd-1118157702.html)
