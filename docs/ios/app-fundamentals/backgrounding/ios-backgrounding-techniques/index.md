---
title: Методы фоновой обработки в iOS
description: 'В этом документе содержатся ссылки на руководства, в которых описываются различные методы фонового применения в iOS: фоновые задачи, фоновая служба передачи, фоновая выборка и удаленные уведомления.'
ms.prod: xamarin
ms.assetid: 011A8D48-1CDC-486A-A2B0-C4946118E7A9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: a81d6a651ee980b444da19341a89206afa8c4375
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73010818"
---
# <a name="ios-backgrounding-techniques"></a>Методы фоновой обработки в iOS

В следующих разделах мы рассмотрим следующие функции iOS наряду с существующими вариантами фоновой настройки.

- [Уступающей фоновые задачи](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background_tasks_in_iOS_7) сохраняют время работы батареи, запуская фоновые задачи в уступающей части, когда устройство находится в спящем режиме для другой обработки.
- [Служба фоновой передачи данных](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background-transfers) обеспечивает надежную отправку и загрузку файлов независимо от состояния сети или размера файла.
- [Фоновая выборка](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#background_fetch) — обновление приложения на фоне через определенные системой интервалы.
- [Удаленные уведомления](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#remote_notifications) — используйте push-уведомления для активации обновлений содержимого в фоновом режиме перед открытием приложения пользователем с возможностью уведомления пользователя или автоматического обновления.
- **Фоновые обновления пользовательского интерфейса** . Подготовьте пользовательский интерфейс приложения и обновите моментальный снимок приложения в фоновом режиме.
