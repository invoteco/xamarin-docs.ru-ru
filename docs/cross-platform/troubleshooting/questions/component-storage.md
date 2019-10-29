---
title: Где на компьютере хранятся компоненты
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
author: davidortinau
ms.author: daortin
ms.date: 05/08/2018
ms.openlocfilehash: 9447cf903c8789078e66082e720eeecfa7bb3e0d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014240"
---
# <a name="where-are-the-components-stored-on-my-machine"></a>Где на компьютере хранятся компоненты

При установке компонента Xamarin в проект приложения он размещается в двух местах:

1. В папке Components на корневом уровне папки решения. Если удалить компонент из всех проектов в решении, он также будет удален из этой папки.

2. Копия также хранится в следующих расположениях:
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

Чтобы полностью удалить компонент из системы, удалите его из проектов и решений и из папки кэша выше.
