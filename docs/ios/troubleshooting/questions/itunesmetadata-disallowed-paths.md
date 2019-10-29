---
title: 'Почему отправка приложения завершается с ошибкой: недопустимые пути (файла itunesmetadata. plist) найдены в...?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: AE1BBDC6-4D3A-4471-876B-FE28B6E59A24
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: 0255c918f7d6e984c9af2b396d9a2a00286286e4
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030995"
---
# <a name="why-does-my-app-submission-fail-with-disallowed-paths--itunesmetadataplist--found-at--"></a>Почему отправка приложения завершается с ошибкой: недопустимые пути (файла itunesmetadata. plist) найдены в...?

> Ошибка: ошибка ИТМС-90047: "запрещенные пути (" файла itunesmetadata. plist ") найдены в: полезные данные/iPhoneApp1. App"

Эта ошибка является результатом изменения в процессе проверки в магазине приложений Apple, чтобы предотвратить возникновение таких проблем с пользователями, как [ошибка 29180](https://bugzilla.xamarin.com/show_bug.cgi?id=29180). Эта конкретная ошибка _не_ связана с конкретной версией Xamarin, которую вы установили, поэтому переход на использование более ранней версии _не_ поможет.

Сведения о решении и последние обновления для этой проблемы см. [в обсуждении](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1) форума.
