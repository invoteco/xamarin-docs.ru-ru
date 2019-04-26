---
title: Почему проект Android xamarin.Forms.Maps завершается ошибкой COMPILETODALVIK НЕПРЕДВИДЕННАЯ ошибка верхнего уровня?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C0251EB1-F509-47AD-98D6-846AF46425E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 9df9e348440b9dd4b18b3859d64cbe47bd05b24c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61250498"
---
# <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik-unexpected-top-level-error"></a>Почему проект Android xamarin.Forms.Maps завершается ошибкой COMPILETODALVIK НЕПРЕДВИДЕННАЯ ошибка верхнего уровня?

Эта ошибка может возникать в панель ошибок из Visual Studio для Mac или в окне выходных данных сборки Visual Studio; в Android проектах, использующих Xamarin.Forms.Maps.

Наиболее часто эта проблема разрешается путем увеличения размера кучи Java для проекта Xamarin.Android. Выполните следующие действия, чтобы увеличить размер кучи.

## <a name="visual-studio"></a>Visual Studio

1. Щелкните правой кнопкой мыши проект Android & откройте параметры проекта.
2. Перейдите к **"->" Параметры Android Advanced**
3. В текстовом поле Размер кучи Java введите 1 ГБ.
4. Перестройте проект.

![Снимок экрана параметров проекта Visual Studio](maps-compiletodalvik-error-images/vsjavaheap.png "Android параметры сборки в Visual Studio")

## <a name="visual-studio-for-mac"></a>Visual Studio для Mac

1.  Щелкните правой кнопкой мыши проект Android & откройте параметры проекта.
2.  Перейдите к **построения "->" сборки Android -> Advanced**
3.  В текстовом поле Размер кучи Java введите 1 ГБ.
4.  Перестройте проект.  

![Снимок экрана Visual Studio для Mac параметров проекта](maps-compiletodalvik-error-images/xsjavaheap.png "Android параметры сборки в Visual Studio для Mac")

