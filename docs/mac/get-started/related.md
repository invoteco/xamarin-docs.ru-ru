---
title: Xamarin.Mac — сопутствующая документация
description: 'В этом документе приведены ссылки на документацию, актуальную для разработчиков Xamarin.Mac: документацию по Xamarin.iOS, центр Apple для разработчиков на Mac, а также различные руководства, описывающие создание с помощью Xamarin.Mac пользовательских интерфейсов.'
ms.prod: xamarin
ms.assetid: 0a282c58-1c37-4f73-8440-85de2daf454a
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 12/02/2016
ms.openlocfilehash: 10450bbb87ed974475001102920ec6fb90cf7976
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "73029982"
---
# <a name="xamarinmac-related-documentation"></a>Xamarin.Mac — сопутствующая документация

Кроме раздела, посвященного Mac на веб-сайте [документации Майкрософт](~/mac/get-started/index.md), доступны еще три источника сведений, где также можно найти ответы на вопросы об использовании Xamarin.Mac:

- [**Документация по Xamarin.iOS**](~/ios/get-started/index.md). Многие API (в основном вне AppKit/UIKit) имеют лишь незначительные различия в версиях iOS и macOS. В некоторых случаях, когда одному из API iOS задано имя `UIFoo`, в macOS можно найти аналогичный API с именем `NSFoo`. Как правило, эти примеры будут уже написаны на C#.

- **[Центр разработки для Mac](https://developer.apple.com/devcenter/mac/) Apple**. Примеры вызовов API-интерфейсов на языке Objective-C можно без труда преобразовать в примеры на языке C#. Сведения о том, как это сделать, см. в статье [Общие сведения об API-интерфейсах Mac](~/mac/app-fundamentals/mac-apis.md).

- [**Stack Overflow**](https://stackoverflow.com/). Отличный ресурс, где можно найти простые ответы на такие вопросы, как ["Как автоматически развернуть все узлы в NSOutlineView"](https://stackoverflow.com/questions/519751/nsoutlineview-auto-expand-all-nodes). Часто эти примеры будут написаны на языке Objective-C, поэтому их необходимо преобразовать на C#, однако есть ряд ответов и на C#.

## <a name="user-interface"></a>Пользовательский интерфейс

При работе с C# и .NET в приложении Xamarin.Mac разработчику доступны те же элементы управления пользовательского интерфейса, что и специалисту, работающему с *Objective-C* и *Xcode*. Поскольку Xamarin.Mac напрямую интегрируется с Xcode, можно использовать конструктор _Interface Builder_ для Xcode, чтобы создавать и обслуживать пользовательские интерфейсы приложений (или при необходимости создать их непосредственно в коде C#).

В перечисленных ниже руководствах содержатся подробные сведения о работе с элементами macOS в приложении Xamarin.Mac:

- [Windows](~/mac/user-interface/window.md)
- [Диалоговые окна](~/mac/user-interface/dialog.md)
- [Оповещения](~/mac/user-interface/alert.md)
- [Меню](~/mac/user-interface/menu.md)
- [Панели инструментов](~/mac/user-interface/toolbar.md)
- [Представления таблиц](~/mac/user-interface/table-view.md)
- [Представления структур](~/mac/user-interface/outline-view.md)
- [Списки источников](~/mac/user-interface/source-list.md)
