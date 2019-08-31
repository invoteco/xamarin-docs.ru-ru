---
title: Шаг 1. Регистрация приложения для использования Azure Active Directory
description: В этом документе описывается регистрация приложения Azure с Azure Active Directory для безопасного доступа мобильных клиентов.
ms.prod: xamarin
ms.assetid: 0B17991A-4573-4F6C-9E86-D4B9D1A47E4D
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 732c1ee241b39a4bb1422b8c27820631bf8c6b0c
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/31/2019
ms.locfileid: "70199048"
---
# <a name="step-1-register-an-app-to-use-azure-active-directory"></a>Шаг 1. Регистрация приложения для использования Azure Active Directory

1. Перейдите по адресу [WindowsAzure.com](https://manage.windowsazure.com) и войдите с помощью учетной записи Майкрософт или учетной записи организации на портале Azure. Если у вас нет подписки Azure, вы можете получить пробную версию от [Azure.com](https://www.azure.com) .

2. После входа перейдите в раздел **Active Directory** (1) и выберите каталог, в котором нужно зарегистрировать приложение (2).

   [![](register-images/01.-active-directory-in-azure-portal-sml.jpg "(раздел) и выберите каталог, в котором необходимо зарегистрировать приложение.")](register-images/01.-active-directory-in-azure-portal.jpg#lightbox)

3. Нажмите кнопку **Добавить** , чтобы создать новое приложение, а затем выберите **Добавить приложение, разрабатываемое моей организацией** .

   [![](register-images/02.-add-new-application-sml.jpg "Добавление приложения, разрабатываемого моей организацией")](register-images/02.-add-new-application.jpg#lightbox)

4. На следующем экране присвойте имя приложению (например, КСАМ — ДЕМОНСТРАЦИЯ).
   Убедитесь, что в качестве типа приложения выбрано **собственное клиентское приложение** .

   ![](register-images/03.-app-name.jpg "Убедитесь, что в качестве типа приложения выбрано собственное клиентское приложение.")

5. На последнем экране введите **URI перенаправления* , уникальный для приложения, так как он вернется к этому URI после завершения проверки подлинности.

   ![](register-images/04.-app-redirect.jpg "На последнем экране укажите URI перенаправления, уникальный для приложения, так как он вернется к этому URI после завершения проверки подлинности.")

6. После создания приложения перейдите на вкладку **Настройка** . Запишите **идентификатор клиента** , который мы будем использовать в нашем приложении позже. Кроме того, на этом экране можно предоставить мобильному приложению доступ к Active Directory или добавить другое приложение, например веб-API или Office 365, которое может использоваться мобильным приложением после завершения проверки подлинности.

   ![](register-images/05.-configure.jpg "Кроме того, на этом экране можно предоставить мобильному приложению доступ к Active Directory или добавить другое приложение, например веб-API или Office 365.")



## <a name="related-links"></a>Связанные ссылки

- [Пример Microsoft NativeClient](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
