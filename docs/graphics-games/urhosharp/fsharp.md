---
title: Программирование UrhoSharp с помощьюF#
description: В этом документе описывается, как создать простой hello world UrhoSharp приложения с помощью F# в Visual Studio для Mac.
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 6269a7f2fa097136f492657d0ba7c6a1f056c38c
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832321"
---
# <a name="programming-urhosharp-with-f"></a>Программирование UrhoSharp с помощьюF#

UrhoSharp могут быть запрограммированы с F# используя те же библиотеки и основные понятия, используемые C# программистов. [UrhoSharp с помощью](~/graphics-games/urhosharp/using.md) статье представлен обзор ядра UrhoSharp и должны считываться перед в этой статье.

Как и многие библиотеки, изначально созданные в мире C++ многие функции UrhoSharp возвращают логические значения или целые числа, об успехе или неудаче. Следует использовать `|> ignore` игнорировать эти значения.

[Пример программы](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) — «Hello World» для UrhoSharp из F#.

## <a name="creating-an-empty-project"></a>Создание пустого проекта

Существуют не F# шаблоны для UrhoSharp пока доступны, поэтому для создания собственного проекта UrhoSharp, либо можно начать с [пример](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) или выполните следующие действия:

1. Из Visual Studio для Mac, создайте новый **решение**. Выберите **iOS > приложение > приложение с одним представлением** и выберите **F#** в качестве языка реализации. 
1. Удалить **Main.storyboard** файла. Откройте **Info.plist** файл и в **iPhone / iPod сведения о развертывании** области удалить `Main` строка в **главный интерфейс** раскрывающегося списка.
1. Удалить **ViewController.fs** также файл.

## <a name="building-hello-world-in-urho"></a>Построение Hello World в Urho

Теперь вы готовы начать определение классов при продаже игры. Как минимум, необходимо определить подкласс `Urho.Application` и переопределите его `Start` метод. Чтобы создать этот файл, щелкните правой кнопкой мыши ваш F# проекта, выберите **добавить новый файл...**  и добавьте пустой F# класс в проект. Новый файл будет добавлен в конец списка файлов в проекте, но его необходимо перетащить, чтобы он отображался *перед* используется в **AppDelegate.fs**.

1. Добавьте ссылку на пакет Urho NuGet.
1. Из существующего проекта Urho, скопировать (крупный) каталога **CoreData /** и **данных /** в ваш проект **ресурсы /** каталога. В вашей F# проекта, щелкните правой кнопкой мыши **ресурсы** папки и использование **добавить / добавить существующую папку** для добавления всех файлов в проект.

Структуры проекта теперь должен выглядеть так:

![](fsharp-images/solutionpane.png "Структура проекта теперь должен выглядеть так")

Определить только что созданный класс как подтипом `Urho.Application` и переопределите его `Start` метод:

```fsharp
namespace HelloWorldUrho1

open Urho
open Urho.Gui
open Urho.iOS

type HelloWorld(o : ApplicationOptions) =
    inherit Urho.Application(o) 

override this.Start() = 
        let cache = this.ResourceCache
        let helloText = new Text()

        helloText.Value <- "Hello World from Urho3D, Mono, and F#"
        helloText.HorizontalAlignment <- HorizontalAlignment.Center
        helloText.VerticalAlignment <- VerticalAlignment.Center

        helloText.SetColor (new Color(0.f, 1.f, 0.f))
        let f = cache.GetFont("Fonts/Anonymous Pro.ttf")

        helloText.SetFont(f, 30) |> ignore
                  
        this.UI.Root.AddChild(helloText)
            
```

Код выполняется очень просто. Она использует `Urho.Gui.Text` класс, чтобы отобразить строку с выравниванием по центру с определенного размера шрифта и цвета. 

Перед запуском этого кода, однако необходимо инициализировать UrhoSharp. 

Откройте файл AppDelegate.fs и измените `FinishedLaunching` метод следующим образом:

```fsharp
namespace HelloWorldUrho1

open System
open UIKit
open Foundation
open Urho
open Urho.iOS

[<Register ("AppDelegate")>]
type AppDelegate () =
    inherit UIApplicationDelegate ()

    override this.FinishedLaunching (app, options) =
        let o = ApplicationOptions.Default
     
        let g = new HelloWorld(o)
        g.Run() |> ignore
       
        true
```

`ApplicationOptions.Default` Предоставляет значения по умолчанию для приложения альбомной ориентации. Передавать их `ApplicationOptions` конструктора по умолчанию для вашей `Application` подкласс (Обратите внимание, что при определении `HelloWorld` класса строке `inherit Application(o)` вызывает конструктор базового класса).

`Run` Метод вашей `Application` инициирует программы. Он определяется как возврат `int`, который можно передать в `ignore`.

Программы должны выглядеть на этом снимке экрана:

![Снимок экрана программы](fsharp-images/helloworldfsharp.png)

## <a name="related-links"></a>Связанные ссылки

- [Посмотрите на GitHub (пример)](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
