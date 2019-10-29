---
title: 'Ошибка при отправке в App Store: в отправке обнаружены недопустимые параметры пакета, которые должны быть внедрены в bitcode.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: c6a03fa3327e81f577f85b05a033d9c97495eb2e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031079"
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>Ошибка при отправке в App Store: в отправке обнаружены недопустимые параметры пакета, которые должны быть внедрены в bitcode.

приложения watchOS и tvOS _должны_ bitcode, когда они передаются в App Store. При создании и отправке приложений watchOS и tvOS с помощью Xcode 8,3 или более ранней версии может возникнуть следующая ошибка (через уведомление по электронной почте) при попытке отправить в магазин приложений:

>Недопустимый пакет — приложение не может быть обработано, так как в отправке обнаружены параметры, которые не разрешено внедрять в bitcode. Скорее всего, вы не создаете приложение с цепочки инструментов, предоставленным в Xcode.

Решением этой проблемы является создание приложений с помощью Xcode 9 и последней версии Xamarin. iOS.
