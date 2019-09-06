---
title: 'Ошибка при отправке в App Store: В отправке обнаружены недопустимые параметры пакета, которые могут быть внедрены в bitcode.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: 84244e0c4c24a8ca6ac71a79de963bedf5c1ee68
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292536"
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>Ошибка при отправке в App Store: В отправке обнаружены недопустимые параметры пакета, которые могут быть внедрены в bitcode.

приложения watchOS и tvOS _должны_ bitcode, когда они передаются в App Store. При создании и отправке приложений watchOS и tvOS с помощью Xcode 8,3 или более ранней версии может возникнуть следующая ошибка (через уведомление по электронной почте) при попытке отправить в магазин приложений:

>Недопустимый пакет — приложение не может быть обработано, так как в отправке обнаружены параметры, которые не разрешено внедрять в bitcode. Скорее всего, вы не создаете приложение с цепочки инструментов, предоставленным в Xcode.

Решением этой проблемы является создание приложений с помощью Xcode 9 и последней версии Xamarin. iOS.
