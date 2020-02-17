---
title: Двухэкранный режим в Xamarin.Forms
description: Это руководство рассказывает, как легко реализовать в приложениях поддержку двухэкранных устройств с помощью Xamarin.Forms.
ms.prod: xamarin
ms.assetid: f9906e83-f8ae-48f9-997b-e1540b96ee8e
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: 1f648297a608c592d90f2c70494ae4496fe73c0f
ms.sourcegitcommit: 07941cf9704ff88cf4087de5ebdea623ff54edb1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/11/2020
ms.locfileid: "77145518"
---
# <a name="xamarinforms-dual-screen"></a>Двухэкранный режим в Xamarin.Forms

![](~/media/shared/preview.png "This API is currently pre-release")

Теперь на устройствах Surface Duo (Android) и Surface Neo (Windows 10X) доступны новые шаблоны разработки для приложений с сенсорным управлением. В Xamarin.Forms появились элементы `TwoPaneView` и `DualScreenInfo`, позволяющие воспользоваться всеми преимуществами этих новых возможностей.

## <a name="dual-screen-patternsdesign-patternsmd"></a>[Шаблоны для двух экранов](design-patterns.md)

Наше руководство по шаблонам поможет вам подобрать оптимальный вариант использования интерфейса приложения на устройстве с двумя экранами.

## <a name="twopaneviewtwopaneviewmd"></a>[TwoPaneView](twopaneview.md)

Класс `TwoPaneView` в Xamarin.Forms, созданный в стиле элемента управления UWP с таким же именем, представляет макет, оптимизированный для двухэкранных устройств с кроссплатформенной поддержкой.

## <a name="dualscreeninfodual-screen-infomd"></a>[DualScreenInfo](dual-screen-info.md)

Для максимально эффективного использования двухэкранных устройств вам может потребоваться знать, в какой области отображается ваше представление, сколько места оно занимает, в каком положении находится устройство, каков угол сгиба и т. д. Эти сведения можно получить с помощью `DualScreenInfo`.

## <a name="dualscreenhelperdual-screen-helpermd"></a>[DualScreenHelper](dual-screen-helper.md)
`DualScreenHelper` позволяет проверить, поддерживает ли платформа открытие нового окна в режиме "картинка в картинке". Если вы используете устройство Neo в режиме создания, окно откроется на панели WonderBar.