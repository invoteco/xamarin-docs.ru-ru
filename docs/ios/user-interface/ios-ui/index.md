---
title: Пользовательские интерфейсы в iOS
description: В этом документе содержатся ссылки на руководства, в которых описывается создание пользовательских интерфейсов в приложении Xamarin. iOS. Связанные руководства охватывают API внешнего вида, создание объектов пользовательского интерфейса, параметры макета и многое другое.
ms.prod: xamarin
ms.assetid: 1BB46561-F503-491E-A27C-7878E7EBE00B
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/14/2017
ms.openlocfilehash: 106cadc6fef43c06597cf1e3b846bd48cecb6fa5
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70287050"
---
# <a name="user-interfaces-in-ios"></a>Пользовательские интерфейсы в iOS

## <a name="appearance-apiintroduction-to-the-appearance-apimd"></a>[API внешнего вида](introduction-to-the-appearance-api.md)

iOS позволяет создавать несколько визуальных атрибутов элементов управления пользовательского интерфейса с помощью API-интерфейсов Уиаппеаранце.

## <a name="creating-user-interface-objectsiosuser-interfaceios-uicreating-ui-objectsmd"></a>[Создание объектов пользовательского интерфейса](~/ios/user-interface/ios-ui/creating-ui-objects.md)

Apple группирует связанные элементы функциональности в "frameworks", которые соответствуют пространствам имен Xamarin. iOS. `UIKit`пространство имен, содержащее все элементы управления пользовательского интерфейса для iOS.

## <a name="layout-optionsiosuser-interfaceios-uilayout-optionsmd"></a>[Параметры макета](~/ios/user-interface/ios-ui/layout-options.md)

Существует два разных механизма управления макетом при изменении размера или поворота представления. Автоматическое изменение размеров и Автомакет.

## <a name="providing-haptic-feedbackiosuser-interfaceios-uihaptic-feedbackmd"></a>[Обеспечение обратной связи Haptic](~/ios/user-interface/ios-ui/haptic-feedback.md)

В этой статье рассматриваются новые типы отзывов о хаптик, доступные в iOS 10, и способы их реализации в Xamarin. iOS.

## <a name="working-with-the-ui-threadiosuser-interfaceios-uiui-threadmd"></a>[Работа с потоком пользовательского интерфейса](~/ios/user-interface/ios-ui/ui-thread.md)

Код должен вносить изменения только в элементы управления пользовательского интерфейса из основного потока (или пользовательского интерфейса). Любые обновления пользовательского интерфейса, происходящие в другом потоке (например, обратный вызов или фоновый поток), могут не отображаться на экране или даже вызывать сбой.




