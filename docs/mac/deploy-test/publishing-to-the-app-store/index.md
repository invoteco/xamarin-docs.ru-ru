---
title: Публикация приложений Xamarin.Mac в Mac App Store
description: Этот документ описывает развертывание приложения Xamarin.Mac с помощью Visual Studio для Mac. Он содержит сведения о настройке учетной записи разработчика Mac, создании сертификатов для подписания кода и их использовании для создания приложений Mac, распространяемых напрямую или через Mac App Store.
ms.prod: xamarin
ms.assetid: D26C5E54-EAD2-5487-264D-4263AEA1EBF2
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 1a14bcc15299360a17b4c9e7a363b6d6e6f2e69a
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725029"
---
# <a name="publishing-xamarinmac-apps-to-the-mac-app-store"></a>Публикация приложений Xamarin.Mac в Mac App Store

## <a name="overview"></a>Обзор

Существует два способа распространения приложений Xamarin.Mac:

- **ИД разработчика**. Приложения, подписанные с использованием идентификатора разработчика, могут распространяться за пределами Магазина приложений, но распознаются привратником и разрешены для установки.
- **Mac App Store**. Приложения должны иметь пакет установщика. Для отправки в Mac App Store приложение и установщик должны быть подписаны.

В этом документе содержатся сведения об использовании Visual Studio для Mac и Xcode для настройки учетной записи разработчика Apple и настройки проекта Xamarin.Mac для каждого типа развертывания.

## <a name="mac-developer-program"></a>Программа для разработчиков Mac

При присоединении к [программе разработчиков Mac](https://developer.apple.com/devcenter/mac/) разработчику будет предложено принять участие в качестве отдельного лица или компании, как показано на снимке экрана ниже:

[![Портал для разработчиков Apple](images/image1.png "Портал для разработчиков Apple")](images/image1-large.png#lightbox)

Выберите подходящий тип регистрации.

> [!NOTE]
> От выбранных здесь возможностей будет зависеть отображение некоторых экранов, появляющихся при настройке учетной записи разработчика. Описания и снимки экранов в этом документе выполнены в случае выбора **индивидуальной** учетной записи разработчика. В разделе **Компания** некоторые параметры будут доступны только пользователям с правами **администратора команды**.

### <a name="certificates-and-identifiersmacdeploy-testpublishing-to-the-app-storecertificates-identifiersmd"></a>[Сертификаты и идентификаторы](~/mac/deploy-test/publishing-to-the-app-store/certificates-identifiers.md)

В этом руководстве описывается создание необходимых сертификатов и идентификаторов, которые потребуются для публикации приложения Xamarin.Mac.

### <a name="create-provisioning-profilemacdeploy-testpublishing-to-the-app-storeprofilesmd"></a>[Создание профиля подготовки](~/mac/deploy-test/publishing-to-the-app-store/profiles.md)

В этом руководстве описывается создание необходимых профилей подготовки, которые потребуются для публикации приложения Xamarin.Mac.

### <a name="mac-app-configurationmacdeploy-testpublishing-to-the-app-storeapp-configurationmd"></a>[Настройка приложений Mac](~/mac/deploy-test/publishing-to-the-app-store/app-configuration.md)

В этом руководстве описывается настройка приложения Xamarin.Mac для публикации.

### <a name="sign-with-developer-idmacdeploy-testpublishing-to-the-app-storesigningmd"></a>[Подписывание с использованием идентификатора разработчика](~/mac/deploy-test/publishing-to-the-app-store/signing.md)

В этом руководстве содержатся сведения о подписывании приложения Xamarin.Mac с использованием идентификатора разработчика для публикации.

### <a name="bundle-for-mac-app-storemacdeploy-testpublishing-to-the-app-storebundlingmd"></a>[Создание пакета для Mac App Store](~/mac/deploy-test/publishing-to-the-app-store/bundling.md)

В этом руководстве описывается создание пакета приложения Xamarin.Mac для публикации в Mac App Store.

### <a name="upload-to-mac-app-storemacdeploy-testpublishing-to-the-app-storeuploadingmd"></a>[Отправка в Mac App Store](~/mac/deploy-test/publishing-to-the-app-store/uploading.md)

В этом руководстве описывается отправка пакета приложения Xamarin.Mac для публикации в Mac App Store.

## <a name="related-links"></a>Связанные ссылки

- [Установка](/visualstudio/mac/installation/)
- [Пример кода приложения "Привет, Mac"](~/mac/get-started/hello-mac.md)
- [Идентификатор разработчика и привратник](https://developer.apple.com/developer-id/)
