---
title: Поддержка UrhoSharp Mac
description: В этом документе рассматривается поддержка UrhoSharp в macOS. Он описывает, как создать проект и ссылки на пример кода.
ms.prod: xamarin
ms.assetid: 95FFBD36-14E9-4C17-B1E8-9A04E81E824D
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: ee0a03d168b6e628893b18a27d73b46d3fa2fbc2
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832663"
---
# <a name="urhosharp-mac-support"></a>Поддержка UrhoSharp Mac

_Настройка определенных MAC и компоненты_

Хотя Urho является переносимой библиотеке классов и разрешает один и тот же API для использования на различных платформах для логики игр по-прежнему необходимо инициализировать Urho в конкретного драйвера, платформы, а в некоторых случаях, вам потребуется воспользоваться преимуществами определенных функций платформы .

В указанные ниже страницы предполагается, что `MyGame` является подклассом `Application` класса.

## <a name="macos"></a>macOS

**Поддерживаемые архитектуры:** x86/x86-64 для 32-разрядных и 64-разрядная версия.

## <a name="creating-a-project"></a>Создание проекта

Создайте проект консольного, ссылаются на Urho NuGet и убедитесь, что можно найти ресурсы (каталоги, содержащие каталог данных).

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

## <a name="example"></a>Пример

[Полный пример](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Cocoa)
