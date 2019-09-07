---
title: Apple Pay в watchOS в Xamarin
description: В этой статье рассматриваются улучшения, внесенные компанией Apple для Apple Pay в watchOS 3 и способы их реализации в Xamarin. iOS для Apple Watch.
ms.prod: xamarin
ms.assetid: 32FF5D21-C252-485D-83AC-A7E592237962
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: 579f2afd8e52251973900f35ef91ac086adf7603
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768653"
---
# <a name="apple-pay-on-watchos-in-xamarin"></a>Apple Pay в watchOS в Xamarin

Компания Apple внесла несколько усовершенствований в Apple Pay watchOS 3, которые добавляют поддержку платежей в приложении. Это позволяет пользователю безопасно предоставлять сведения о платежах и контактных данных для оплаты физических товаров и услуг непосредственно из Apple Watch.

## <a name="about-apple-pay-enhancements"></a>Об улучшениях Apple Pay

Как упоминалось выше, Apple предоставила несколько усовершенствований для Apple Pay в watchOS 3, которые обеспечивают безопасность платежей и контактных данных для оплаты физических товаров и услуг непосредственно из Apple Watch. Эти улучшения предоставляются изменениями в PassKit Framework.

В iOS 10 и watchOS 3 добавлено несколько новых API, которые работают с iOS и watchOS для поддержки динамических платежных сетей и новой тестовой среды "песочницы".

## <a name="passkit-framework-enhancements"></a>Усовершенствования платформы PassKit

В iOS 10 платформа PassKit была расширена для поддержки Apple Pay вне `UIKit` и позволяет поставщикам карт предоставлять свои карты в своих приложениях. 

### <a name="supporting-apple-pay-outside-of-uikit"></a>Поддержка Apple Pay за пределами UIKit

С помощью [пкпайментаусоризатионконтроллер](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) и [пкпайментаусориксатионконтроллерделегате](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate)приложение может поддерживать те же функциональные возможности, что и [пкпайментаусоризатионвиевконтроллер](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) , без использования UIKit. Хотя этот новый API необходим для поддержки Apple Pay Apple Watch (а также в конкретных целях), он является необязательным в других ситуациях (например, в существующих приложениях). Однако компания Apple предлагает как можно быстрее перейти на новый API, чтобы обеспечить широкую Apple Payную поддержку во всех приложениях разработчика с единой базой кода. Дополнительные сведения об целей и интеграции Siri см. в документации по [SiriKit](~/ios/platform/sirikit/index.md) .

### <a name="presenting-issuer-cards-from-within-apps"></a>Представление карточек поставщика в приложениях

В iOS 10 и watchOS 3 в платформу PassKit были добавлены новые функции, позволяющие поставщикам карт предоставлять свои платежные карты в своих приложениях. Разработчик может добавить `PKPaymentButtonTypeInStore` уибуттон в пользовательский интерфейс приложения, который будет отображать кнопку Apple Pay для карточки.

Метод `PresentPaymentPass` класса [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) также можно использовать для программного вывода карты.

## <a name="new-payment-network-support"></a>Поддержка новой оплаты по сети

Новые возможности iOS 10 и watchOS 3. приложение может автоматически поддерживать новую платежную сеть, когда она станет доступной, без необходимости изменения, повторной компиляции приложения и повторной отправки в App Store.

Новый `PKPaymentNetwork` метод [аваилабленетворкс](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) класса позволяет приложению обнаруживать сети, доступные на устройстве пользователя во время выполнения. Кроме того, свойство [суппортеднетворкс](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) было расширено, чтобы взять имя поставщика платежа в качестве аргумента. Используя эти методы, приложение может автоматически поддерживать любую сеть, поддерживаемую поставщиком платежей.

Дополнительные сведения см. в нашей [Apple Pay конфигурации](~/ios/platform/apple-pay.md) и в разделе [руководств по Apple Pay](https://developer.apple.com/apple-pay/)Apple.

## <a name="new-testing-environment"></a>Новая тестовая среда

В iOS 10 и watchOS 3 Компания Apple представила новую среду тестирования, позволяющую разработчику подготавливать тестовые платежные карты непосредственно на устройстве iOS. Затем эта новая тестовая среда возвращает в приложение зашифрованные проверочные данные.

Чтобы включить новую среду тестирования, выполните следующие действия.

1. Создайте новую тестовую учетную запись iCloud в iTunes Connect.
2. Войдите на устройство iOS с помощью новой учетной записи тестирования.
3. Задайте нужный регион для тестирования приложения в.
4. Используйте одну из платежных [Apple Pay](https://developer.apple.com/apple-pay/) карт для создания платежей.

> [!NOTE]
> При переключении учетных записей iCloud устройство автоматически переключается на новую среду тестирования. Тем не менее Apple по-прежнему **требует** , чтобы приложение проверялось на реальные карты в рабочей среде перед отправкой в магазин приложений iTunes.

## <a name="summary"></a>Сводка

В этой статье были рассмотрены улучшения, внесенные компанией Apple для Apple Pay в watchOS 3 и их реализации в Xamarin. iOS.
