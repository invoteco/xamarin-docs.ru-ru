---
title: Работа с группами приложений watchOS в Xamarin
description: В этом документе описываются группы приложений и их использование в приложении watchOS. В нем обсуждается настройка группы приложений, требований к подготовке, прав и рекомендаций. plist и развертывание.
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 6968606B-C287-424F-A321-2492E12BC0BB
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: c75db8bd29b7a57c46610abdd5e4024938fc9e1b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70280334"
---
# <a name="working-with-watchos-app-groups-in-xamarin"></a>Работа с группами приложений watchOS в Xamarin


Группы приложений предоставляют различным приложениям (или одному приложению и его расширениям) общее место хранения файлов. Группы приложений можно использовать для следующих данных:

- [Параметры](~/ios/watchos/app-fundamentals/settings.md)Apple Watch.
- Общий [нсусердефаултс](~/ios/watchos/app-fundamentals/parent-app.md#nsuserdefaults).
- Общие [файлы](~/ios/watchos/app-fundamentals/parent-app.md#files).

## <a name="configure-an-app-group"></a>Настройка группы приложений

Общее расположение настраивается с помощью [группы приложений](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19), которая настроена в разделе **сертификаты, идентификаторы & профилей** в [центре разработки iOS](https://developer.apple.com/devcenter/ios/). Это значение также должно быть задано в списке прав каждого проекта **. plist**.

### <a name="provisioning"></a>Подготовка

Группа приложений будет иметь идентификатор, который обычно является идентификатором пакета с `group.` префиксом. Например, можно использовать идентификатор `com.xamarin.WatchSettings` пакета и группу `group.com.xamarin.WatchSettings`приложений.

[![](app-groups-images/app-group-sml.png "Используйте идентификатор пакета com. Xamarin. Ватчсеттингс и группу приложений Group. com. Xamarin. Ватчсеттингс")](app-groups-images/app-group.png#lightbox)

### <a name="entitlementsplist"></a>Entitlements.plist

Кроме настройки профиля подготовки, **включите группы приложений** в поле "права **. plist** " и введите идентификатор, который вы выбрали:

[![](app-groups-images/entitlements-sml.png "Настройте plist и введите идентификатор.")](app-groups-images/entitlements.png#lightbox)


### <a name="deployment"></a>Развертывание

Убедитесь, что группа приложений правильно настроена в подготовке [развертывания](~/ios/watchos/deploy-test/index.md#App_Groups) .


Дополнительные сведения см. в разделе [возможностей групп приложений](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) документации.


## <a name="related-links"></a>Связанные ссылки

- [Apple обмена данными с содержащего приложение](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
- [Документ группы приложений Apple](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)
