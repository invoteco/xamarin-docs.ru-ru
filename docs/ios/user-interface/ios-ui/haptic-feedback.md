---
title: Предоставление обратной связи Хаптик в Xamarin. iOS
description: В этом документе описывается, как обеспечить обратную связь хаптик в приложении Xamarin. iOS. В нем обсуждаются Уиимпактфидбаккженератор, Уинотификатионфидбаккженератор и Уиселектионфидбаккженератор.
ms.prod: xamarin
ms.assetid: 888106D1-58F4-453F-BACC-91D51FA39C80
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 156af7a5336ac091c0202e38a3a59a32846e281a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73003348"
---
# <a name="providing-haptic-feedback-in-xamarinios"></a>Предоставление обратной связи Хаптик в Xamarin. iOS

<a name="Overview" />

## <a name="overview"></a>Обзор

На iPhone 7 и iPhone 7 Plus компания Apple включила новые ответы хаптик, которые предоставляют дополнительные способы физического привлечения пользователя. Обратная связь хаптик (часто именуемая просто AS Хаптикс) использует сенсорное касание (с помощью Force, вибрации или движения) в разработке пользовательского интерфейса. Используйте эти новые параметры обратной связи тактиле для получения внимания пользователя и подготовьте свои действия.

Будут подробно рассмотрены следующие темы:

- [О Хаптик отзыва](#About-Haptic-Feedback)
- [уиимпактфидбаккженератор](#UIImpactFeedbackGenerator)
- [уинотификатионфидбаккженератор](#UINotificationFeedbackGenerator)
- [уиселектионфидбаккженератор](#UISelectionFeedbackGenerator)

<a name="About-Haptic-Feedback" />

## <a name="about-haptic-feedback"></a>О Хаптик отзыва

Несколько встроенных элементов пользовательского интерфейса уже предоставляют хаптик отзывы, такие как подборки, переключатели и ползунки. iOS 10 теперь добавляет возможность программного запуска хаптикс с помощью конкретного подкласса `UIFeedbackGenerator` класса.

Разработчик может использовать один из следующих подклассов `UIFeedbackGenerator` для программного запуска обратной связи хаптик:

- `UIImpactFeedbackGenerator` — используйте этот генератор обратной связи, чтобы дополнить действие или задачу, такие как представление "суд" при наличии слайдов представления или при конфликте двух объектов на экране.
- `UINotificationFeedbackGenerator` — используйте этот генератор обратной связи для таких уведомлений, как завершение действия, сбой или предупреждение любого другого типа.
- `UISelectionFeedbackGenerator` — используйте этот генератор обратной связи для изменяемого выбора, например комплектации элемента из списка.

<a name="UIImpactFeedbackGenerator" />

### <a name="uiimpactfeedbackgenerator"></a>уиимпактфидбаккженератор

Используйте этот генератор обратной связи, чтобы дополнить действие или задачу, например выпустить «суд» при наличии слайдов представления или при конфликте двух объектов на экране.

Используйте следующий код, чтобы активировать отзыв о влиянии:

```csharp
using UIKit;
...

// Initialize feedback
var impact = new UIImpactFeedbackGenerator (UIImpactFeedbackStyle.Heavy);
impact.Prepare ();

// Trigger feedback
impact.ImpactOccurred ();
```

Когда разработчик создает новый экземпляр класса `UIImpactFeedbackGenerator`, он предоставляет `UIImpactFeedbackStyle`, задавая стойкость отзыва:

- `Heavy`
- `Medium`
- `Light`

Метод `Prepare` `UIImpactFeedbackGenerator` вызывается для информирования системы о том, что хаптик обратная связь, чтобы она могла максимально сокращать задержку.

Затем метод `ImpactOccurred` активирует отзыв хаптик.

<a name="UINotificationFeedbackGenerator" />

### <a name="uinotificationfeedbackgenerator"></a>уинотификатионфидбаккженератор

Используйте этот генератор обратной связи для таких уведомлений, как завершение действия, сбой или предупреждение любого другого типа.

Чтобы активировать отзыв уведомлений, используйте следующий код:

```csharp
using UIKit;
...

// Initialize feedback
var notification = new UINotificationFeedbackGenerator ();
notification.Prepare ();

// Trigger feedback
notification.NotificationOccurred (UINotificationFeedbackType.Error);
```

Создается новый экземпляр класса `UINotificationFeedbackGenerator` и вызывается его `Prepare` метод для информирования системы о том, что хаптик обратная связь, чтобы она могла максимально сокращать задержку.

`NotificationOccurred` вызывается для активации обратной связи хаптик с заданным `UINotificationFeedbackType`:

- `Success`
- `Warning`
- `Error`

<a name="UISelectionFeedbackGenerator" />

### <a name="uiselectionfeedbackgenerator"></a>уиселектионфидбаккженератор

Используйте этот генератор обратной связи для активно изменяемого выбора, например комплектации элемента из списка.

Используйте следующий код, чтобы активировать отзыв о выборе:

```csharp
using UIKit;
...

// Initialize feedback
var selection = new UISelectionFeedbackGenerator ();
selection.Prepare ();

// Trigger feedback
selection.SelectionChanged ();
```

Создается новый экземпляр класса `UISelectionFeedbackGenerator` и вызывается его `Prepare` метод для информирования системы о том, что хаптик обратная связь, чтобы она могла максимально сокращать задержку.

Затем метод `SelectionChanged` активирует отзыв хаптик.

## <a name="summary"></a>Сводка

В этой статье были рассмотрены новые типы отзывов о хаптик, доступные в iOS 10, и способы их реализации в Xamarin. iOS.

## <a name="related-links"></a>Связанные ссылки

- [Примеры iOS 10](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
