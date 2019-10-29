---
title: Шаг 2. Настройка доступа к службе для мобильного приложения
description: В этом документе описывается, как предоставить приложению Xamarin доступ к приложению Azure, защищенному с помощью Azure Active Directory.
ms.prod: xamarin
ms.assetid: 8A14A457-F72E-4B08-B4B6-801F7619F893
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: eeac7b0b70b2f11304a374de7522f28d4bcad6c6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016670"
---
# <a name="step-2-configure-service-access-for-mobile-application"></a>Шаг 2. Настройка доступа к службе для мобильного приложения

Каждый ресурс, например веб-приложение, веб-служба и т. д., должен быть защищен с помощью Azure Active Directory, он должен быть зарегистрирован. Все безопасные приложения и службы можно просмотреть на вкладке **приложения** . Здесь можно выбрать приложение, доступ к которому необходимо получить из мобильного приложения, и предоставить ему доступ к нему.

1. На вкладке **Настройка** перейдите к разделу **разрешения для других приложений** :

   ![](configure-images/2.1-configure.png "On the Configure tab, locate permissions to other applications section")

2. Нажмите кнопку **Добавить приложение** . На следующем экране появится список всех приложений, защищенных с помощью Azure Active Directory. Выберите приложения, к которым требуется доступ из мобильного приложения.

   ![](configure-images/2.2-add-application.png "Select the applications that needs to be accessed from the mobile application")

3. После выбора приложения снова выберите только что добавленное приложение в разделе **разрешения для других приложений** и предоставьте соответствующие права.

   ![](configure-images/2.3-permissions.png "After selecting the application, once again select the newly-added application in permissions to other   applications section and give appropriate rights")

4. Наконец, **Сохраните** конфигурацию. Эти службы теперь должны быть доступны в мобильных приложениях!

## <a name="related-links"></a>Связанные ссылки

- [Пример Microsoft NativeClient](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
