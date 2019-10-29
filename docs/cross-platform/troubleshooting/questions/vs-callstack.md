---
title: Как собрать текущие стеки вызовов процесса Visual Studio?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 64c24b09-2c4a-43ad-b94d-6cd05a1aee44
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: 9ed79b2273758b8051a96169d4c9b53870de1fb1
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73013026"
---
# <a name="how-do-i-collect-the-current-call-stacks-of-the-visual-studio-process"></a>Как собрать текущие стеки вызовов процесса Visual Studio?

Когда графический интерфейс пользователя заблокируется (зависает) в Visual Studio, важным элементом диагностических данных является набор стеков вызовов из всех потоков процесса Visual Studio. Чтобы сохранить эти сведения для зависших экземпляров Visual Studio, можно использовать второй экземпляр Visual Studio:

1. Запустите второй экземпляр (новое окно) Visual Studio.

2. Закройте все открытые решения в новом экземпляре Visual Studio.

3. Выберите **Отладка > Присоединить к процессу**.

   ![](vs-callstack-images/image1.png "Select Debug > Attach to Process")

4. Выберите исходный зависла экземпляр `devenv.exe` из списка **доступных процессов**.

5. Выберите **отладка > остановить все**.

   ![](vs-callstack-images/image2.png "Select Debug > Break All")

6. Выберите **отладка > сохранить дамп как**.

   ![](vs-callstack-images/image3.png "Select Debug > Save Dump As")

7. Измените **тип "Сохранить как** **" на малый дамп (\*. dmp)** . Это приведет к созданию намного меньшего файла, чем мини- **дамп с кучей**, и куча обычно не имеет значения для диагностики заморозок.

   ![](vs-callstack-images/image4.png "This will produce a much smaller file than Minidump with Heap, and the heap is usually not relevant for diagnosing freezes")

8. Сохраните файл дампа. При отправке файла в сети его можно заархивировать, чтобы уменьшить размер.
