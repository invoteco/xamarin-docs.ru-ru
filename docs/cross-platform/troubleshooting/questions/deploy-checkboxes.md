---
title: Отключены флажки развертывания в Configuration Manager
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
author: conceptdev
ms.author: crdun
ms.date: 12/02/2016
ms.openlocfilehash: 82ff1a684ffad75a301f0db6b0f8e3116be6746d
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70285064"
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>Отключены флажки развертывания в Configuration Manager

Так как Xamarin 3,5 проекты Xamarin. iOS разворачиваются автоматически при нажатии кнопки **Пуск** на панели инструментов или выборе пункта меню **Отладка > начать отладку** . Перед выполнением любой из этих команд вам все равно потребуется задать проект приложения Xamarin. iOS в качестве **запускаемого проекта** .

По этой причине флажки **развертывания** намеренно отключены в Configuration Manager Visual Studio для проектов Xamarin. iOS:

![](deploy-checkboxes-images/configuration.png "Visual Studio Configuration Manager отображение флажка \"развернуть\" для проекта Xamarin. iOS в Xamarin 3,5")

Это изменение устраняет ошибку, которая может появиться в более старых версиях Xamarin (версии 3,3 и более ранних), если проект приложения Xamarin. iOS не был настроен для развертывания:

![](deploy-checkboxes-images/error.png "Диалоговое окно ошибки: Перед запуском проекта iPhoneApp1 его необходимо развернуть. Убедитесь, что проект выбран для развертывания в Configuration Manager решения.")
