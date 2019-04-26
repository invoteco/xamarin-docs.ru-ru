---
title: Почему Visual Studio не включает проект библиотеки в сборку, которая на него ссылается?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: b9009db8-e716-43aa-b40e-6f28a8eb1b82
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: d7aeac2f433e8fdf231f5887f1537f15e2bd1976
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61341312"
---
# <a name="why-doesnt-visual-studio-include-my-referenced-library-project-in-my-build"></a>Почему Visual Studio не включает проект библиотеки в сборку, которая на него ссылается?

Visual Studio использует **Configuration Manager** для определения, какие проекты в решении, автоматически включаются в заданной конфигурации сборки или развертывания.

Некоторые шаблоны, которые создаются в проекте библиотеки, на которую указывает ссылка, уже будет иметь указанную библиотеку, включенных в конфигурацию; но, в противном случае его необходимо настроить вручную.

## <a name="how-to-use-the-configuration-manager"></a>Как использовать Configuration Manager

1. Откройте **сборки > Configuration Manager**
2. Выберите конфигурацию для настройки, например **отладка | iPhone**
3. Установите флажки для проектов, которые нужно включить.

> [!NOTE]
> Недоступен для выбора поля обрабатываются автоматически и вносить изменения не требуется.

Демонстрационный ролик из следующих действий: [http://screencast.com/t/zLoQOpEn](http://screencast.com/t/zLoQOpEn)
