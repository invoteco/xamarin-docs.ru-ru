---
title: Подписывание приложений Xamarin.Mac с помощью идентификатора разработчика
description: Этот документ описывает подпись приложения Xamarin.Mac с помощью идентификатора разработчика, чтобы его можно было распространять за пределами Mac App Store. В нем описаны параметры подписи кода и сборка.
ms.prod: xamarin
ms.assetid: cf7b733b-e08f-4f56-a233-264b29ee4c97
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: eb8df9f7088c4954ed48ce8c7c145334004207cd
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73017935"
---
# <a name="signing-xamarinmac-apps-with-a-developer-id"></a>Подписывание приложений Xamarin.Mac с помощью идентификатора разработчика

Если приложение планируется распространять напрямую пользователям macOS, компания Apple рекомендует подписать его код с помощью идентификатора разработчика, чтобы устанавливать в системах macOS с включенным **привратником**. Если приложение не было подписано, **привратник** запретит его устанавливать, выводя предупреждение (пользователи могут обойти это ограничение, удерживая нажатой клавишу CTRL при запуске).

Дополнительные сведения об [идентификаторе разработчика и привратнике](https://developer.apple.com/resources/developer-id/) и [распространении за пределами Mac App Store](https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html) см. на веб-сайте Apple.

## <a name="code-signing-options"></a>Параметры подписывания кода

Чтобы создать приложение, напрямую развертываемое для пользователей (а не через Mac App Store), в **параметрах подписывания** используйте значение **Идентификатор разработчика**. Параметр "Конфигурация" должен иметь значение **Выпуск**.

 [![](signing-images/config02.png "The Mac Signing options")](signing-images/config02.png#lightbox)

## <a name="build"></a>Построить

Перед выполнением сборки убедитесь, что выбрана правильная конфигурация, и в окне **Сборка Mac** выберите параметр создания пакета установки.

[![](signing-images/config03.png "The build options")](signing-images/config03.png#lightbox)

При сборке приложения выводится предложение об использовании обоих сертификатов:

 [![](signing-images/image57.png "Allowing keychain access")](signing-images/image57.png#lightbox)

 [![](signing-images/image58.png "Allowing keychain access")](signing-images/image58.png#lightbox)

После сборки приложения щелкните проект правой кнопкой мыши и выберите команду **Открыть содержащую папку**, чтобы найти файл пакета (в каталоге `bin/Release`). В этом файле содержится установщик для приложения, поэтому его можно распространять любому пользователю macOS для установки.

 [![](signing-images/image59.png "Selecting the app package in Finder")](signing-images/image59.png#lightbox)

## <a name="related-links"></a>Связанные ссылки

- [Установка](~//mac/get-started/installation.md)
- [Пример кода приложения "Привет, Mac"](~//mac/get-started/hello-mac.md)
- [Распространение приложений в Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Руководство по работе с инструментами. Подписывание кода приложения](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Идентификатор разработчика и привратник](https://developer.apple.com/resources/developer-id/)
