---
title: функции платформы watchOS
description: В этом документе содержатся ссылки на различные руководства, в которых описываются функции платформы watchOS, такие как Apple Pay, уведомления, сложности, упреждающие предложения, тренировки и многое другое.
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 10/05/2018
ms.openlocfilehash: 2b987992bcb3dd4d2575a46e21a2302ed78d8d70
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70282290"
---
# <a name="watchos-platform-features"></a>функции платформы watchOS

## <a name="introduction-to-watchos-5introduction-to-watchos5indexmd"></a>[Введение в watchOS 5](introduction-to-watchos5/index.md)

В этом документе представлен общий обзор новых и обновленных функций watchOS 5, которые можно использовать при создании watchOS приложений с помощью Xamarin.

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[Введение в watchOS 4](introduction-to-watchos4.md)

В этом документе представлен общий обзор компонентов, добавленных и обновленных в watchOS 4.

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[Введение в watchOS 3](introduction-to-watchos3/index.md)

В этой статье описываются новые и обновленные API-интерфейсы в watchOS 3.

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[Улучшения Apple Pay](~/ios/watchos/platform/apple-pay.md)

В watchOS 3 платформа PassKit была расширена, чтобы обеспечить поддержку безопасных и внутренних платежей в приложении (как физических товаров, так и услуг) для приложений, работающих на Apple Watch.

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[Фоновые задачи](~/ios/watchos/platform/background-tasks.md)

в watchOS 3 представлено несколько фоновых задач, которые приложение может использовать для обновления сведений, гарантирующих, что оно должно иметь содержимое, необходимое пользователю, прежде чем открыть его.

## <a name="notificationsnotificationsmd"></a>[Уведомления](notifications.md)

Узнайте, как обеспечить настраиваемую обработку уведомлений в приложении для наблюдения.

## <a name="complicationscomplicationsmd"></a>[Усложнения](complications.md)

Добавьте поддержку усложнения для показа актуальных данных на экране просмотра.

## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[Упреждающие предложения](~/ios/watchos/platform/proactive-suggestions.md)

watchOS 3 позволяет приложению заранее предоставлять сведения пользователю в рамках заданных контекстов. Для поддержки этой функции [нсусерактивити](https://developer.apple.com/reference/foundation/nsuseractivity) теперь включает `MapItem` свойство, которое позволяет приложению предоставлять сведения о расположении для последующего использования другими приложениями.

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[Методы быстрого взаимодействия](~/ios/watchos/platform/quick-interaction-techniques.md)

Обеспечение быстрого взаимодействия с пользователем очень важно для создания привлекательных Apple Watch приложений и сложностей. Впервые watchOS 3, компания Apple добавила поддержку распознавателей жестов, доступа к Digital Crownу и новым методам уведомления и навигации пользователей. Кроме того, благодаря добавленной поддержке как для SceneKit, так и для SpriteKit, разработчики могут легко создавать многофункциональные и быстрые интерфейсы, быстро и реагирующие.

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[Усовершенствования тренировок приложений](~/ios/watchos/platform/workout-apps.md)

Новые возможности для watchOS 3. приложения, связанные с тренировками, могут работать в фоновом режиме на Apple Watch. Чтобы включить эту функцию (и получить доступ к данным HealthKit), приложение должно включить `WKBackgroundModes` ключ `Info.plist` в файл со значением `workout-processing`.
