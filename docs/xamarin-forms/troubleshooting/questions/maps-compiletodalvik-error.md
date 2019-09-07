---
title: Почему в проекте Android Xamarin. Forms. Maps происходит сбой с КОМПИЛЕТОДАЛВИК НЕПРЕДВИДЕНной ошибкой верхнего уровня?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C0251EB1-F509-47AD-98D6-846AF46425E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 2872cc7b54e26d07b388f08d650048e8d3861930
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70759969"
---
# <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik-unexpected-top-level-error"></a>Почему в проекте Android Xamarin. Forms. Maps происходит сбой с КОМПИЛЕТОДАЛВИК НЕПРЕДВИДЕНной ошибкой верхнего уровня?

Эта ошибка может отображаться на панели ошибок Visual Studio для Mac или в окне вывода сборки Visual Studio. в проектах Android с использованием Xamarin. Forms. Maps.

Чаще всего это устраняется путем увеличения размера кучи Java для проекта Xamarin. Android. Чтобы увеличить размер кучи, выполните следующие действия:

## <a name="visual-studio"></a>Visual Studio

1. Щелкните правой кнопкой мыши проект Android & откройте параметры проекта.
2. Выберите **Параметры Android — > дополнительно** .
3. В текстовом поле Размер кучи Java введите 1 ГБ.
4. Перестройте проект.

![Снимок экрана: параметры проекта Visual Studio](maps-compiletodalvik-error-images/vsjavaheap.png "Параметры сборки Android в Visual Studio")

## <a name="visual-studio-for-mac"></a>Visual Studio для Mac

1. Щелкните правой кнопкой мыши проект Android & откройте параметры проекта.
2. Последовательно выберите **Сборка-> сборка Android — > дополнительно** .
3. В текстовом поле Размер кучи Java введите 1 ГБ.
4. Перестройте проект.  

![Снимок экрана параметров проекта Visual Studio для Mac](maps-compiletodalvik-error-images/xsjavaheap.png "Параметры сборки Android в Visual Studio для Mac")
