---
title: Отключены флажки развертывания в Configuration Manager
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
author: davidortinau
ms.author: daortin
ms.date: 12/02/2016
ms.openlocfilehash: edf471f1d9a2ee4adc11f09e0c7b7ad3cf6f78f1
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014253"
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>Отключены флажки развертывания в Configuration Manager

Так как Xamarin 3,5 проекты Xamarin. iOS разворачиваются автоматически при нажатии кнопки **Пуск** на панели инструментов или выборе пункта меню **Отладка > начать отладку** . Перед выполнением любой из этих команд вам все равно потребуется задать проект приложения Xamarin. iOS в качестве **запускаемого проекта** .

По этой причине флажки **развертывания** намеренно отключены в Configuration Manager Visual Studio для проектов Xamarin. iOS:

![](deploy-checkboxes-images/configuration.png "Visual Studio Configuration Manager showing the 'Deploy' checkbox disabled for a Xamarin.iOS project in Xamarin 3.5")

Это изменение устраняет ошибку, которая может появиться в более старых версиях Xamarin (версии 3,3 и более ранних), если проект приложения Xamarin. iOS не был настроен для развертывания:

![](deploy-checkboxes-images/error.png "Error dialog: The project iPhoneApp1 needs to be deployed before it can be started. Verify the project is selected to be deployed in the Solution Configuration Manager.")
