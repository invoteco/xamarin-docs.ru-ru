---
title: Шаг 1. Регистрация приложения для использования Azure Active Directory
description: В этом документе описывается, как зарегистрировать приложение Azure с Azure Active Directory, таким образом, он может осуществляться безопасно мобильными клиентами.
ms.prod: xamarin
ms.assetid: 0B17991A-4573-4F6C-9E86-D4B9D1A47E4D
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: fd3cb0e8201a6e84c726b211852013bd5f35fba1
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67675109"
---
# <a name="step-1-register-an-app-to-use-azure-active-directory"></a>Шаг 1. Регистрация приложения для использования Azure Active Directory

1. Перейдите к [windowsazure.com](https://manage.windowsazure.com) и войдите, используя учетную запись Майкрософт или учетную запись организации на портале Azure. Если у вас нет подписки Azure, вы можете получить пробную версию из [azure.com](http://www.azure.com)

2. После входа в систему перейдите на страницу **Active Directory** раздел (1) и выберите каталог, где вы хотите зарегистрировать приложение (2)

  [![](register-images/01.-active-directory-in-azure-portal-sml.jpg "раздел и выберите каталог, где вы хотите зарегистрировать приложение")](register-images/01.-active-directory-in-azure-portal.jpg#lightbox)

3. Нажмите кнопку **добавить** Чтобы создать новое приложение, затем выберите **добавить приложение, разрабатываемое моей организацией**

  [![](register-images/02.-add-new-application-sml.jpg "Добавить приложение, разрабатываемое моей организацией")](register-images/02.-add-new-application.jpg#lightbox)

4. На следующем экране Присвойте приложению имя (например) XAM-DEMO).
  Обязательно выберите **собственное клиентское приложение** как тип приложения.

  ![](register-images/03.-app-name.jpg "Убедитесь, что вы выберите собственное клиентское приложение в качестве типа приложения")

5. На последнем экране, введите **URI перенаправления* это уникальный для вашего приложения, так как она будет возвращать этот URI при завершении проверки подлинности.

  ![](register-images/04.-app-redirect.jpg "На последнем экране Укажите URI перенаправления, который является уникальным для вашего приложения, как она будет возвращать этот URI при завершении проверки подлинности")

6. После создания приложения, перейдите к **Настройка** вкладки. Запишите **идентификатор клиента** которой мы будем использовать в нашем приложении позже. Кроме того на этом экране можно предоставить доступ приложений к Active Directory или добавить другое приложение, например веб-API или Office 365, который может использоваться с мобильными приложениями, по завершении проверки подлинности.

    ![](register-images/05.-configure.jpg "Кроме того на этом экране можно предоставить доступ приложений к Active Directory или добавить другое приложение, например веб-API или Office 365")



## <a name="related-links"></a>Связанные ссылки

- [Пример Microsoft NativeClient](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
