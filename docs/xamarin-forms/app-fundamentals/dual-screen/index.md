---
title: Двухэкранный режим в Xamarin.Forms
description: Это руководство рассказывает, как создавать приложения с поддержкой двухэкранных устройств с помощью Xamarin.Forms.
ms.prod: xamarin
ms.assetid: f9906e83-f8ae-48f9-997b-e1540b96ee8e
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: 41a7ef4e447bb71582264b4e73629566d3ffd4e7
ms.sourcegitcommit: 524fc148bad17272bda83c50775771daa45bfd7e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/20/2020
ms.locfileid: "77480511"
---
# <a name="xamarinforms-dual-screen"></a>Двухэкранный режим в Xamarin.Forms

![](~/media/shared/preview.png "This API is currently pre-release")

Теперь на устройствах Surface Duo (Android) и Surface Neo (Windows 10X) доступны новые шаблоны разработки для приложений с сенсорным управлением. Xamarin.Forms включает классы `TwoPaneView` и `DualScreenInfo`, позволяющие разрабатывать приложения для этих устройств.

## <a name="dual-screen-patterns"></a>[Шаблоны для двух экранов](design-patterns.md)

Наше руководство по шаблонам поможет вам подобрать оптимальный вариант использования интерфейса приложения на устройстве с двумя экранами.

## <a name="twopaneview"></a>[TwoPaneView](twopaneview.md)

Класс `TwoPaneView` в Xamarin.Forms, созданный в стиле элемента управления UWP с таким же именем, представляет собой кроссплатформенный макет, оптимизированный для двухэкранных устройств.

## <a name="dualscreeninfo"></a>[DualScreenInfo](dual-screen-info.md)

Класс `DualScreenInfo` позволяет определить, в какой области отображается ваше представление, сколько места оно занимает, в каком положении находится устройство, каков угол сгиба и т. д.

## <a name="dualscreenhelper"></a>[DualScreenHelper](dual-screen-helper.md)

Класс `DualScreenHelper` позволяет проверить, поддерживает ли платформа открытие нового окна в режиме "картинка в картинке". Если вы используете устройство Neo в режиме создания, окно откроется на панели WonderBar.
