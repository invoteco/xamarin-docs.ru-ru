---
title: Двухэкранный режим в Xamarin.Forms
description: Это руководство рассказывает, как создавать приложения с поддержкой двухэкранных устройств с помощью Xamarin.Forms.
ms.prod: xamarin
ms.assetid: f9906e83-f8ae-48f9-997b-e1540b96ee8e
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: 344b6293090ffa4281ea6351f7f176a5be37e5bd
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "78165551"
---
# <a name="xamarinforms-dual-screen"></a>Двухэкранный режим в Xamarin.Forms

![](~/media/shared/preview.png "This API is currently pre-release")

Теперь на устройствах Surface Duo (Android) и Surface Neo (Windows 10X) доступны новые шаблоны разработки для приложений с сенсорным управлением. Xamarin.Forms включает классы `TwoPaneView` и `DualScreenInfo`, позволяющие разрабатывать приложения для этих устройств.

## <a name="dual-screen-design-patterns"></a>[Конструктивные шаблоны для двухэкранных устройств](design-patterns.md)

Наше руководство по шаблонам поможет вам подобрать оптимальный вариант использования интерфейса приложения на устройстве с двумя экранами.

## <a name="dual-screen-layout"></a>[Макет для двух экранов](twopaneview.md)

Класс `TwoPaneView` в Xamarin.Forms, созданный в стиле элемента управления UWP с таким же именем, представляет собой кроссплатформенный макет, оптимизированный для двухэкранных устройств.

## <a name="dual-screen-device-capabilities"></a>[Возможности устройства с двумя экранами](dual-screen-info.md)

Класс `DualScreenInfo` позволяет определить, в какой области отображается ваше представление, сколько места оно занимает, в каком положении находится устройство, каков угол сгиба и т. д.

## <a name="dual-screen-platform-helpers"></a>[Вспомогательные функции платформы для двух экранов](dual-screen-helper.md)

Класс `DualScreenHelper` позволяет проверить, поддерживает ли платформа открытие нового окна в режиме "картинка в картинке". Если вы используете устройство Neo в режиме создания, окно откроется на панели WonderBar.

## <a name="dual-screen-triggers"></a>[Триггеры для двух экранов](triggers.md)

Пространство имен [`Xamarin.Forms.DualScreen`](xref:Xamarin.Forms.DualScreen) включает два триггера состояния, которые активируют изменение [`VisualState`](xref:Xamarin.Forms.VisualState) при изменении режима просмотра в присоединенном макете или окне.
