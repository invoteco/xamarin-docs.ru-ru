---
title: Какие пакеты SDK для Android следует установить?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F136AAE0-C6D2-4B0F-8F8C-7A6A94877266
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/30/2018
ms.openlocfilehash: bd0f2a7704e5d666f6b32d4ccc489e069ec6ade6
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757251"
---
# <a name="which-android-sdk-packages-should-i-install"></a>Какие пакеты SDK для Android следует установить?

При установке пакет SDK для Android не будет автоматически включаться все необходимые пакеты для разработки. Хотя отдельные потребности для разработчиков различаются, для разработки с помощью Xamarin. Android обычно требуются следующие пакеты:

## <a name="tools"></a>Сервис

Установите новейшие средства из папки Tools в диспетчере пакетов SDK:

- Android SDK Tools
- Пакет SDK для Android Platform-Tools
- Средства сборки пакет SDK для Android

## <a name="android-platforms"></a>Платформы Android

Установите "платформу SDK" для версий Android, которые вы задали как минимум & целевое значение. 

Примеры:

- Целевой API 23
- Минимальный API 23

Необходимо установить только платформу SDK для API 23

- Целевой API 23
- Минимальный API 15

Необходимо установить платформы SDK для API 15 и 23. Обратите внимание, что устанавливать уровни API между минимальной и целевой платформой не требуется (даже если вы выполняете неограниченный перенос на эти уровни API).

## <a name="system-images"></a>Образы системы

Они требуются только в том случае, если вы хотите использовать готовые Эмуляторы Android из Google. Дополнительные сведения см. в разделе [Android Emulator Setup](~/android/get-started/installation/android-emulator/index.md) .

## <a name="extras"></a>Дополнения
Дополнительные пакет SDK для Android, как правило, не являются обязательными. но полезно помнить о них, так как они могут потребоваться в зависимости от варианта использования.

## <a name="further-reading"></a>Дополнительные сведения
В следующем руководстве рассматриваются эти возможности и приводятся более подробные сведения о различных пакетах, которые диспетчер пакетов SDK получил. [Руководством по установке пакет SDK для Android Manager](http://www.themethodology.net/2015/02/android-sdk-manager-setup-for.html?m=1)
