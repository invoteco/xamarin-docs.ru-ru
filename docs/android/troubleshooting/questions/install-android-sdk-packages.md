---
title: Какие пакеты SDK для Android следует установить?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F136AAE0-C6D2-4B0F-8F8C-7A6A94877266
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/30/2018
ms.openlocfilehash: 24c70c2e869f59091a1519af6d1165dbea9cc467
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "76725064"
---
# <a name="which-android-sdk-packages-should-i-install"></a>Какие пакеты SDK для Android следует установить?

Установка пакета SDK для Android не предусматривает автоматическую установку всех минимально необходимых для разработки пакетов. Хотя отдельные потребности для разработчиков различаются, для разработки с помощью Xamarin.Android обычно требуются следующие пакеты:

## <a name="tools"></a>Инструменты

Установите новейшие средства из папки "Средства" в диспетчере пакетов SDK:

- Android SDK Tools
- Пакет SDK со средствами платформы для Android
- Пакет SDK со средствами сборки для Android

## <a name="android-platforms"></a>Платформы Android

Установите "платформу SDK" для версий Android, которые вы задали в качестве минимального и целевого уровней.

Примеры

- Целевой уровень API — 23
- Минимальный уровень API — 23

Необходимо установить только платформу SDK для API 23

- Целевой уровень API — 23
- Минимальный уровень API — 15

Необходимо установить платформу SDK для API 15 и 23. Обратите внимание, что устанавливать уровни API между минимальным и целевым уровнем не требуется (даже если вы выполняете неограниченный перенос на эти уровни API).

## <a name="system-images"></a>Образы системы

Они требуются только в том случае, если вы хотите использовать готовые эмуляторы Android из Google. Дополнительные сведения см. в разделе [Настройка эмулятора Android](~/android/get-started/installation/android-emulator/index.md)

## <a name="extras"></a>Дополнения
Комплект дополнений для Android SDK обычно не требуется; однако о них полезно знать, так как они могут потребоваться в зависимости от вашего варианта использования.
