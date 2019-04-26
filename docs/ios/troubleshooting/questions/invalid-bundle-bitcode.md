---
title: 'Ошибка при отправке в App Store: «Недопустимый пакет — не допускается для внедрения в bitcode параметры обнаружения в отправки»'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
ms.openlocfilehash: 867ad29abfa6a38971b60ac9ebf181905949dafd
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61421733"
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>Ошибка при отправке в App Store: «Недопустимый пакет — не допускается для внедрения в bitcode параметры обнаружения в отправки»

watchOS и tvOS приложения _требуют_ bitcode, когда они отправляются App Store. При создании и отправке приложений watchOS и tvOS с помощью Xcode 8.3 или более ранней версии, (с помощью уведомления по электронной почте) может возникнуть следующая ошибка при попытке отправить в App Store:

>Недопустимый пакет — приложение не может обработать, так как не допускается для внедрения в bitcode параметры обнаружения на отправку. Вполне вероятно, что вы не создаете приложения с собственным набором инструментов, предоставляемых в Xcode.

Решение этой проблемы является создание приложений с помощью Xcode 9 и последнюю версию Xamarin.iOS.
