---
title: Работа со значками watchOS в Xamarin
description: В этом документе описываются различные значки, необходимые для приложения watchOS, и описывается, как настроить решение для включения этих значков.
ms.prod: xamarin
ms.assetid: EE3D45BD-8091-4C04-BA83-371371D8BEB9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/26/2018
ms.openlocfilehash: c8c5b8d0417fb7fd1069d2bf6fa5d9887d569453
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73001563"
---
# <a name="working-with-watchos-icons-in-xamarin"></a>Работа со значками watchOS в Xamarin

Для Apple Watch решений требуется два набора значков:

- Значки приложения iOS, которые будут отображаться на iPhone.
- Apple Watch значки, которые будут отображаться в окружности в меню контрольные значения и на экранах уведомлений. Значок контрольного приложения также отображается в приложении [Apple Watch](~/ios/watchos/app-fundamentals/settings.md) iOS.

## <a name="apple-watch-icons"></a>Значки Apple Watch

| | | |
|-|-|-|
|Значок приложения iOS|Отображается на iPhone и запускает родительское приложение.|![значок приложения iOS](icons-images/icon-ios.png)|
|Значок просмотра приложения|Отображается на начальном экране Apple Watch|![значок приложения watchOS](icons-images/icon-home.png)|
||Отображается в уведомлениях о просмотре|![значок уведомления watchOS](icons-images/notification-icon.png)|
||Отображается в [приложении Apple Watch iOS](~/ios/watchos/app-fundamentals/settings.md)|![значок приложения наблюдения iOS](icons-images/watch-app-sml.png)|

## <a name="configuring-your-solution"></a>Настройка решения

Чтобы убедиться, что приложение iOS и контрольное приложение показывают правильное имя и значок, выполните следующие инструкции для каждого проекта:

### <a name="ios-app"></a>Приложение iOS

Чтобы убедиться, что значки приложения iOS правильно настроены, обратитесь к [руководству по значкам приложения iOS](~/ios/app-fundamentals/images-icons/app-icons.md) .

#### <a name="infoplist"></a>Info.plist

Строка, отображаемая рядом с приложением Watch в [приложении Apple Watch Settings](~/ios/watchos/app-fundamentals/settings.md) , настраивается в файле **info. plist приложения iOS**.

Убедитесь, что ваша **info. plist** имеет `CFBundleName` ключ и значение (Обратите внимание, что этот `CFBundleDisplayName`может иметь и то, и другое):

```xml
<key>CFBundleName</key>
<string>Your App Name</string>
```

### <a name="apple-watch-app"></a>Приложение Apple Watch

После настройки значков [родительского приложения](~/ios/watchos/app-fundamentals/parent-app.md) необходимо добавить в приложение Watch каталог ресурсов значка приложения.

1. Щелкните проект Watch App правой кнопкой мыши и выберите **файл > добавить > новый файл... > iOS > каталога активов** , чтобы добавить каталог активов в проект.

    ![](icons-images/newasset.png "Add an asset catalog to the project")

2. Дважды щелкните файл **AppIcon. appiconset/Contents. JSON** .

    ![](icons-images/xcassets-iconset-sml.png "The AppIcon contents")

3. Добавьте все образы watchOS, как показано на следующем снимке экрана:

    [![](icons-images/appicons-sml.png "Add all the watchOS images, as shown in this screenshot")](icons-images/appicons.png#lightbox)

    См. [рекомендации по использованию значков Apple](https://developer.apple.com/design/human-interface-guidelines/watchos/icons-and-images/menu-icons/) для требуемых размеров (размеры также отображаются на экране). Помните, что эти значки будут автоматически обрезаны для отображения в окружности.

    Список значков должен выглядеть примерно так:

    ![](icons-images/xcassets-complete-sml.png "The icon list in the Solution Explorer")

4. Чтобы убедиться в том, что каталог активов включен в приложение, добавьте следующий ключ и значение в поле **info. plist приложения Watch.**

    ```xml
    <key>XSAppIconAssets</key>
    <string>Images.xcassets/AppIcon.appiconset</string>
    ```

Проверить правильность настройки значков можно, проверив [приложение Apple Watch Settings](~/ios/watchos/app-fundamentals/settings.md) в симуляторе iPhone или создав [уведомление](~/ios/watchos/platform/notifications.md) и убедившись, что значок отображается на экране уведомления.

> [!NOTE]
> Значки не могут иметь альфа-канал (приложение будет отклонено при отправке в магазине приложений, если имеется альфа-канал). Можно проверить, существует ли альфа-канал, и удалить его [с помощью предварительной версии приложения на Mac OS X](~/ios/watchos/troubleshooting.md#noalpha).

## <a name="related-links"></a>Связанные ссылки

- [Значок Apple watchOS. Guide & Images](https://developer.apple.com/design/human-interface-guidelines/watchos/icons-and-images/)
