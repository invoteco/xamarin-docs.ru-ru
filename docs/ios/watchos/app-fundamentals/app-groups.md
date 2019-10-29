---
title: Работа с группами приложений watchOS в Xamarin
description: В этом документе описываются группы приложений и их использование в приложении watchOS. В нем обсуждается настройка группы приложений, требований к подготовке, прав и рекомендаций. plist и развертывание.
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 6968606B-C287-424F-A321-2492E12BC0BB
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: e117fce77e9cdc8d9e9dc8b9ed7b3aa22eca4e39
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73001715"
---
# <a name="working-with-watchos-app-groups-in-xamarin"></a>Работа с группами приложений watchOS в Xamarin

Группы приложений предоставляют различным приложениям (или одному приложению и его расширениям) общее место хранения файлов. Группы приложений можно использовать для следующих данных:

- [Параметры](~/ios/watchos/app-fundamentals/settings.md)Apple Watch.
- Общий [нсусердефаултс](~/ios/watchos/app-fundamentals/parent-app.md#nsuserdefaults).
- Общие [файлы](~/ios/watchos/app-fundamentals/parent-app.md#files).

## <a name="configure-an-app-group"></a>Настройка группы приложений

Общее расположение настраивается с помощью [группы приложений](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19), которая настроена в разделе **сертификаты, идентификаторы & профилей** в [центре разработки iOS](https://developer.apple.com/devcenter/ios/). Это значение также должно быть задано в списке прав каждого проекта **. plist**.

### <a name="provisioning"></a>Подготовка

Группа приложений будет иметь идентификатор, который обычно является ИДЕНТИФИКАТОРом пакета с префиксом `group.`. Например, можно использовать идентификатор пакета `com.xamarin.WatchSettings` и группу приложений `group.com.xamarin.WatchSettings`.

[![](app-groups-images/app-group-sml.png "Use the Bundle ID com.xamarin.WatchSettings and the app group   group.com.xamarin.WatchSettings")](app-groups-images/app-group.png#lightbox)

### <a name="entitlementsplist"></a>Entitlements.plist

Кроме настройки профиля подготовки, **включите группы приложений** в поле "права **. plist** " и введите идентификатор, который вы выбрали:

[![](app-groups-images/entitlements-sml.png "Configure the plist and enter the ID")](app-groups-images/entitlements.png#lightbox)

### <a name="deployment"></a>Развертывание

Убедитесь, что группа приложений правильно настроена в подготовке [развертывания](~/ios/watchos/deploy-test/index.md#App_Groups) .

Дополнительные сведения см. в документации по [возможностям группы приложений](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) .

## <a name="related-links"></a>Связанные ссылки

- [Совместное использование данных Apple с содержащим приложением](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
- [Документ группы приложений Apple](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)
