---
title: Программирование UrhoSharp с помощьюF#
description: В этом документе описывается создание простого приложения Hello World UrhoSharp с помощью F# в Visual Studio для Mac.
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: d87749bd74cf2c478e96284060fed7386d10b853
ms.sourcegitcommit: 0df727caf941f1fa0aca680ec871bfe7a9089e7c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/19/2019
ms.locfileid: "69621005"
---
# <a name="programming-urhosharp-with-f"></a>Программирование UrhoSharp с помощью F\#

UrhoSharp можно программировать с F# помощью тех же самых библиотек и концепций, которые C# используются программистами. В статье [Использование UrhoSharp](~/graphics-games/urhosharp/using.md) приводятся общие сведения о подсистеме UrhoSharp, которые необходимо прочитать до этой статьи.

Как и многие библиотеки, созданные в C++ мире, многие функции UrhoSharp возвращают логические значения или целые числа, указывающие на успех или неудачу. Следует использовать `|> ignore` для игнорирования этих значений.

[Пример программы](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) — это «Hello World» для UrhoSharp из F#.

## <a name="creating-an-empty-project"></a>Создание пустого проекта

F# Шаблоны для UrhoSharp пока недоступны, поэтому для создания собственного проекта UrhoSharp можно либо начать с [примера](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) , либо выполнить следующие действия.

1. В Visual Studio для Mac создайте новое **решение**. Выберите **iOS > приложение > приложение с одним** представлением **F#** и выберите в качестве языка реализации. 
1. Удалите файл **Main. Storyboard** . Откройте файл **info. plist** и в области **сведений о развертывании iPhone/iPod** удалите `Main` строку в раскрывающемся списке **главного интерфейса** .
1. Удалите также файл **ViewController. FS** .

## <a name="building-hello-world-in-urho"></a>Создание Hello World в Урхо

Теперь можно приступать к определению классов игры. Как минимум потребуется определить подкласс `Urho.Application` и переопределить его `Start` метод. Чтобы создать этот файл, щелкните F# проект правой кнопкой мыши, выберите команду **Добавить новый файл...** и добавьте в проект F# пустой класс. Новый файл будет добавлен в конец списка файлов в проекте, но его необходимо перетащить, чтобы он появился *перед* использованием в **AppDelegate. FS**.

1. Добавьте ссылку на пакет NuGet Урхо.
1. В существующем проекте Урхо скопируйте (крупные) каталоги **CoreData/** и **Data/** в **ресурсы или** каталог проекта. В F# проекте щелкните правой кнопкой мыши папку Resources и используйте команду **Добавить или добавить существующую папку** , чтобы добавить все эти файлы в проект.

Теперь структура проекта должна выглядеть следующим образом:

![](fsharp-images/solutionpane.png "Теперь структура проекта должна выглядеть следующим образом:")

Определите созданный вами класс как подтип `Urho.Application` и переопределите его `Start` метод:

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

Код очень прост. Он использует `Urho.Gui.Text` класс для вывода выровненной по центру строки с определенным шрифтом и размером цвета. 

Однако перед выполнением этого кода необходимо инициализировать UrhoSharp. 

Откройте файл AppDelegate. FS и измените `FinishedLaunching` метод следующим образом:

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

`ApplicationOptions.Default` Предоставляет параметры по умолчанию для приложения с альбомным режимом. Передайте `ApplicationOptions` их в конструктор по умолчанию `Application` для своего подкласса (Обратите внимание `HelloWorld` , что при определении `inherit Application(o)` класса строка вызывает конструктор базового класса).

`Run` Метод`Application` запускает программу. Он определяется как возвращающий `int`объект, который можно `ignore`передать.

Итоговая программа должна выглядеть, как на следующем снимке экрана:

![Снимок экрана итоговой программы](fsharp-images/helloworldfsharp.png)

## <a name="related-links"></a>Связанные ссылки

- [Обзор в GitHub (пример)](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
