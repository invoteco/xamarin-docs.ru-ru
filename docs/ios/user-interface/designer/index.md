---
title: Создание пользовательских интерфейсов с помощью конструктора iOS
description: В этом документе описывается, как использовать Xamarin Designer для iOS для создания пользовательского интерфейса приложения с помощью раскадровок и файлов XIB. Он содержит ссылки на документы, которые обсуждают доступность средства, его базовую функциональность, конструктивные элементы управления и предоставляет пошаговые инструкции по его использованию.
ms.prod: xamarin
ms.assetid: E35EFB69-EBBA-40E3-ADBE-CB8016F17127
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/31/2018
ms.openlocfilehash: 577c5602c1cbc331564c3034b3f0c11a4b97bc0c
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70279845"
---
# <a name="building-user-interfaces-with-the-ios-designer"></a>Создание пользовательских интерфейсов с помощью конструктора iOS

_Xamarin Designer для iOS является визуальным конструктором для раскадровки iOS и форматов Interface Builder, полностью интегрированных с Visual Studio для Mac и Visual Studio. Конструктор iOS поддерживает полную совместимость с форматами Storyboard и XIB, чтобы файлы можно было изменять либо в Visual Studio для Mac, либо в Visual Studio в дополнение к Interface Builderам Xcode. Кроме того, Xamarin Designer для iOS поддерживает дополнительные функции, такие как пользовательские элементы управления, отображаемые во время разработки в редакторе._

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

[![конструктор iOS в Visual Studio для Mac](images/designer-vsmac-sml.png "Конструктор iOS")](images/designer-vsmac.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![конструктор iOS в Visual Studio](images/designer-vs.png "Конструктор iOS")](images/designer-vs.png#lightbox)

-----

## <a name="availability"></a>Доступность

Xamarin Designer для iOS доступен в Visual Studio для Mac и в Visual Studio 2017 в Windows.

В этих руководствах предполагается знание содержимого, описываемого в [руководствах по начало работы Xamarin. iOS](~/ios/get-started/index.md).

## <a name="ios-designer-basicsintroductionmd"></a>[Основные сведения о конструкторе iOS](introduction.md)

В этом руководстве описываются функции конструктора Xamarin iOS. Он охватывает основы конструктора, показывая, как использовать конструктор для визуального размещения элементов управления и изменения свойств.

## <a name="designable-controls-overviewios-designable-controls-overviewmd"></a>[Общие сведения о конструированных элементах управления](ios-designable-controls-overview.md)

В этом руководство подробно рассматриваются пользовательские элементы управления, способы их создания и требования, которым они должны удовлетворять для подготовки к просмотру в области конструктора. Кроме того, в нем показано, как выполнять отладку распространенных проблем, которые могут возникнуть при использовании элементов управления, подопределяющих разработку.

## <a name="walkthrough---using-custom-controls-with-ios-designerios-designable-controls-walkthroughmd"></a>[Пошаговое руководство. Использование настраиваемых элементов управления с помощью конструктора iOS](ios-designable-controls-walkthrough.md)

В этой статье представлено пошаговое пошаговое руководство, в котором показано, как создать пользовательский элемент управления и использовать его в конструкторе iOS. В нем показано, как сделать элемент управления доступным на панели элементов конструктора, чтобы его можно было перетаскивать на представление. Кроме того, в нем показано, как реализовать элемент управления, чтобы он правильно отображался во время разработки и в среде выполнения, а также как создавать свойства, которые могут быть установлены во время разработки.

## <a name="auto-layout-with-the-xamarin-ios-designerdesigner-auto-layoutmd"></a>[Автоматический макет с помощью конструктора Xamarin iOS](designer-auto-layout.md)

В этом разделе представлено описание автоматического макета iOS и рабочего процесса "новые ограничения", доступного в конструкторе iOS.
