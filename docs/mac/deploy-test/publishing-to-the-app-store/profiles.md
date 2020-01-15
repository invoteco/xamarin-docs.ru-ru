---
title: Профили подготовки для приложений Xamarin.Mac
description: В этом руководстве описывается создание необходимых профилей подготовки, которые потребуются для публикации приложения Xamarin.Mac.
ms.prod: xamarin
ms.assetid: bdff6c32-f7e3-4a97-a093-dbda48be8227
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 04/12/2017
ms.openlocfilehash: 1bb7ffba11a7135735b7599fa7f12b47d0b347c8
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75489392"
---
# <a name="provisioning-profiles-for-xamarinmac-apps"></a>Профили подготовки для приложений Xamarin.Mac

С помощью профилей подготовки разработчик может добавлять в свои приложения Xamarin.Mac несколько определенных возможностей macOS (прежнее название — Mac OS X), таких как iCloud и push-уведомления. Разработчик должен создать, скачать и установить профиль подготовки Mac для каждого создаваемого приложения, в котором используются указанные функции.

[![](profiles-images/certif13.png "The Apple Provisioning Portal")](profiles-images/certif13.png#lightbox)

## <a name="development-provisioning-profile"></a>Профиль подготовки для разработки

Профиль подготовки для разработки позволяет тестировать приложения, предназначенные для Mac App Store, на определенных компьютерах, которые были настроены в профиле. Это особенно важно при использовании таких функций macOS, как iCloud и push-уведомления.

> [!NOTE]
> Перед созданием профиля подготовки для разработки необходимо иметь уже созданный сертификат разработки Mac. Чтобы сформировать **профиль подготовки для разработки**, используемый для создания сборок, выполните действия, показанные на снимке экрана. Должен существовать допустимый сертификат разработки Mac, доступный для выбора в поле **Сертификат**, и, по меньшей мере, одна система, зарегистрированная для тестирования.

Выполните следующие действия:

1. Выберите тип создаваемого профиля подготовки и нажмите кнопку **Продолжить**: 

    [![](profiles-images/certif14.png "Selecting the profile type")](profiles-images/certif14.png#lightbox)
2. Выберите идентификатор приложения, для которого создается профиль, и нажмите кнопку **Продолжить**: 

    [![](profiles-images/certif15.png "Selecting the app ID")](profiles-images/certif15.png#lightbox)
3. Выберите идентификатор разработчика, используемый для подписывания профиля, и нажмите кнопку **Продолжить**: 

    [![](profiles-images/certif16.png "Selecting the developer ID")](profiles-images/certif16.png#lightbox)
4. Выберите компьютеры, на которых можно использовать этот профиль, и нажмите кнопку **Продолжить**: 

    [![](profiles-images/certif17.png "Selecting the allowed computers")](profiles-images/certif17.png#lightbox)
5. Введите значение в поле **Имя профиля** и нажмите кнопку **Создать**: 

    [![](profiles-images/certif18.png "Generating the profile")](profiles-images/certif18.png#lightbox)
6. Нажмите кнопку **Скачать**, чтобы скачать новый профиль: 

    [![](profiles-images/certif19.png "Downloading the profile")](profiles-images/certif19.png#lightbox)
7. Профили подготовки для разработки устанавливаются в области "Параметры профилей" в **системных настройках** приложения Mac: 

    [![](profiles-images/certif20.png "Installing the profile")](profiles-images/certif20.png#lightbox)
8. Все установленные профили будут отображены в области "Параметры профилей": 

    [![](profiles-images/image47.png "Showing all installed profiles")](profiles-images/image47.png#lightbox)
9. Если профиль необходимо скачать повторно, он также появится в **служебной программе для сертификата разработчика**: 

    [![](profiles-images/image48.png "The Developer Certificate Utility")](profiles-images/image48.png#lightbox)

Профиль подготовки для разработки требуется создавать для каждого нового приложения или при добавлении нового компьютера для тестирования.

## <a name="production-provisioning-profile"></a>Профиль подготовки для производства

Профили подготовки для производства необходимы для создания пакета для отправки в Mac App Store.

Выполните следующие действия:

1. Выберите тип создаваемого профиля и нажмите кнопку **Продолжить**: 

    [![](profiles-images/certif21.png "Selecting the type of profile")](profiles-images/certif21.png#lightbox)
2. Выберите идентификатор приложения, для которого создается профиль, и нажмите кнопку **Продолжить**: 

    [![](profiles-images/certif15.png "Selecting the app ID")](profiles-images/certif15.png#lightbox)
3. Выберите идентификатор компании, используемый для подписывания профиля, и нажмите кнопку **Продолжить**: 

    [![](profiles-images/certif23.png "Selecting the company ID")](profiles-images/certif23.png#lightbox)
4. Введите значение в поле **Имя профиля** и нажмите кнопку **Создать**: 

    [![](profiles-images/certif24.png "Generating the profile")](profiles-images/certif24.png#lightbox)
5. Нажмите кнопку **Скачать**, чтобы получить файл профиля подготовки (расширение `.provisionprofile`): 

    [![](profiles-images/certif25.png "Downloading the profile")](profiles-images/certif25.png#lightbox)
6. Перетащите его в **организатор Xcode** или дважды щелкните его, чтобы установить. Затем профиль появится в организаторе Xcode: 

    [![](profiles-images/image51.png "Installing the profile")](profiles-images/image51.png#lightbox)
7. Профиль подготовки также появится в списке: 

    [![](profiles-images/certif26.png "Showing the installed profiles")](profiles-images/certif26.png#lightbox)

В случае изменения функций, используемых идентификатором приложения (например, включение iCloud или push-уведомлений), следует повторно создать профили подготовки для этого идентификатора приложения.

## <a name="related-links"></a>Связанные ссылки

- [Установка](~//mac/get-started/installation.md)
- [Пример кода приложения "Привет, Mac"](~//mac/get-started/hello-mac.md)
- [Распространение приложений в Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Руководство по работе с инструментами. Подписывание кода приложения](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Идентификатор разработчика и привратник](https://developer.apple.com/resources/developer-id/)
