---
title: Почему Visual Studio не включает проект библиотеки в сборку, которая на него ссылается?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: b9009db8-e716-43aa-b40e-6f28a8eb1b82
author: conceptdev
ms.author: crdun
ms.date: 12/02/2016
ms.openlocfilehash: 37fa93ef7377456d61d1a5f5de56d5de6b0f3c7f
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70282910"
---
# <a name="why-doesnt-visual-studio-include-my-referenced-library-project-in-my-build"></a>Почему Visual Studio не включает проект библиотеки в сборку, которая на него ссылается?

Visual Studio использует **Configuration Manager** , чтобы определить, какие проекты в решении автоматически включаются в данную сборку или конфигурацию развертывания.

Некоторые шаблоны, созданные в проекте библиотеки, на которую указывает ссылка, уже содержат указанную библиотеку в конфигурации. но в противном случае его нужно будет задать вручную.

## <a name="how-to-use-the-configuration-manager"></a>Использование Configuration Manager

1. Открыть **Configuration Manager > сборки**
2. Выберите конфигурацию для настройки, например " **Отладка" | iPhone**
3. Установите флажки для проектов, которые вы хотите включить.

> [!NOTE]
> Серые поля обрабатываются автоматически и не должны изменяться.

Презентация следующих шагов:[http://screencast.com/t/zLoQOpEn](http://screencast.com/t/zLoQOpEn)
