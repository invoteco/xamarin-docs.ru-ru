---
title: Работа с watchOS группы приложений в Xamarin
description: В этом документе описываются группы приложений и их использовании в приложении watchOS. В этом примере рассматривается настройка группы приложений, подготовки требования, вопросы Entitlements.plist и развертывания.
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 6968606B-C287-424F-A321-2492E12BC0BB
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 78f6c03f73f0e4d8a74f826dd7bc25bbe325d545
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61411722"
---
# <a name="working-with-watchos-app-groups-in-xamarin"></a>Работа с watchOS группы приложений в Xamarin


Группы приложений предоставляют различным приложениям (или одному приложению и его расширениям) общее место хранения файлов. Группы приложений можно использовать для следующих данных:

- Apple Watch [параметры](~/ios/watchos/app-fundamentals/settings.md).
- Общие [NSUserDefaults](~/ios/watchos/app-fundamentals/parent-app.md#nsuserdefaults).
- Общие [файлы](~/ios/watchos/app-fundamentals/parent-app.md#files).

## <a name="configure-an-app-group"></a>Настройка группы приложений

Общее расположение настраивается с помощью [групп приложений](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19), настроенный в **сертификаты, идентификаторы и профили** разделе [Центр разработчиков iOS](https://developer.apple.com/devcenter/ios/). Это значение также должно быть указано в каждом проекте **Entitlements.plist**.

### <a name="provisioning"></a>Подготовка

Группа приложений получает идентификатор, который обычно является ваш идентификатор пакета с `group.` префикс. Например, мы могли бы использовать идентификатор пакета `com.xamarin.WatchSettings` и группы приложений `group.com.xamarin.WatchSettings`.

[![](app-groups-images/app-group-sml.png "Используйте идентификатор пакета com.xamarin.WatchSettings и group.com.xamarin.WatchSettings группы приложений")](app-groups-images/app-group.png#lightbox)

### <a name="entitlementsplist"></a>Entitlements.plist

А также настройка профиля подготовки, **включить группы приложений** в **Entitlements.plist** и введите идентификатор, который вы выбрали:

[![](app-groups-images/entitlements-sml.png "Настройка plist и введите идентификатор")](app-groups-images/entitlements.png#lightbox)


### <a name="deployment"></a>Развертывание

Обязательно Настройте эту группу приложений функций в вашей [развертывания](~/ios/watchos/deploy-test/index.md#App_Groups) подготовки.


Дополнительные сведения см. в разделе [возможностей групп приложений](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) документации.


## <a name="related-links"></a>Связанные ссылки

- [Apple обмена данными с содержащего приложение](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
- [Apple doc групп приложений](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)
