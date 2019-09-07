---
title: Общие сведения о привязках цели-C
description: В этом документе представлен обзор различных способов создания C# привязок для кода цели-C, включая привязки командной строки, проекты привязки и целевые Шарпие. В нем также обсуждается принцип работы привязки.
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
author: conceptdev
ms.author: crdun
ms.date: 11/25/2015
ms.openlocfilehash: db37a6a912cae3c2d53d8838ba2d2bd0224e8df7
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70765587"
---
# <a name="overview-of-objective-c-bindings"></a>Общие сведения о привязках цели-C

_Сведения о принципах работы процесса привязки_

Привязка библиотеки цели-C для использования с Xamarin выполняется из трех шагов:

1. Напишите C# "определение API", чтобы описать, как собственный API предоставляется в .NET и как он сопоставляется с базовой целью — C. Это делается с помощью стандартных C# конструкций, таких `interface` как и различных **атрибутов** привязки (см. Этот [простой пример](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API)).

2. После того как вы записали "определение API C#" в, вы компилируете его для создания сборки "Binding". Это можно сделать в [**командной строке**](#commandline) или с помощью [**проекта привязки**](#bindingproject) в Visual Studio для Mac или Visual Studio.

3. После этого в проект приложения Xamarin добавляется сборка "Binding", поэтому вы можете получить доступ к собственным функциям с помощью определенного API.
   Проект привязки полностью отделен от проектов приложений.

   > [!NOTE]
   > Шаг 1 можно автоматизировать с помощью [**цели Шарпие**](#objectivesharpie). Он проверяет API цели-C и создает предложенное C# определение API. Вы можете настроить файлы, созданные с помощью цели Шарпие, и использовать их в проекте привязки (или в командной строке) для создания сборки привязки. Цель Шарпие не создает привязок сами по себе, она является лишь необязательной частью более крупного процесса.

Вы также можете ознакомиться с более подробными техническими сведениями о [том, как она работает](#howitworks), что поможет вам написать привязки.

<a name="Command_Line_Bindings" /><a name="commandline" />

## <a name="command-line-bindings"></a>Привязки командной строки

Вы можете использовать `btouch-native` для Xamarin. iOS (или `bmac-native` , если вы используете Xamarin. Mac) для непосредственного создания привязок. Это работает путем передачи определений C# API, созданных вручную (или с помощью цели Шарпие), в средство командной строки (`btouch-native` для iOS или `bmac-native` для Mac).

Общий синтаксис для вызова этих средств:

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

Приведенная выше команда создаст файл `cocos2d.dll` в текущем каталоге и будет содержать полностью привязанную библиотеку, которую можно использовать в проекте. Это средство, которое Visual Studio для Mac использует для создания привязок при использовании проекта привязки (описан [ниже](#bindingproject)).

<a name="bindingproject" />

## <a name="binding-project"></a>Проект привязки

Проект привязки можно создать в Visual Studio для Mac или Visual Studio (Visual Studio поддерживает только привязки iOS) и упрощает редактирование и сборку определений API для привязки (и с помощью командной строки).

Следуйте этому [руководству по началу работы](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started) , чтобы узнать, как создать и использовать проект привязки для создания привязки.

<a name="objectivesharpie" />

## <a name="objective-sharpie"></a>Objective Sharpie

Цель Шарпие — это еще один инструмент командной строки, который помогает с начальными этапами создания привязки. Он не создает привязку самостоятельно, а сам по себе автоматизирует начальный шаг создания определения API для целевой собственной библиотеки.

Ознакомьтесь с [документацией по цели Шарпие](~/cross-platform/macios/binding/objective-sharpie/index.md) , чтобы узнать, как анализировать собственные библиотеки, собственные платформы и COCOAPODS в API определения, которые могут быть встроены в привязки.

<a name="howitworks" />

## <a name="how-binding-works"></a>Принцип работы привязки

Можно использовать атрибут [[Register]](xref:Foundation.RegisterAttribute) , атрибут [[Export]](xref:Foundation.ExportAttribute) , а также [ручной вызов выборки на языке c](~/ios/internals/objective-c-selectors.md) , чтобы вручную привязать новые (ранее несвязанные) типы цели-c.

Сначала найдите тип, который вы хотите привязать. В целях обсуждения (и простоты) мы будем привязывать тип [нсенумератор](https://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSEnumerator_Class/Reference/Reference.html) (который уже привязан в [Foundation. нсенумератор](xref:Foundation.NSEnumerator); приведенная ниже реализация представляет собой только для примера).

Во вторых, нам нужно создать C# тип. Скорее всего, мы хотим поместить его в пространство имен. так как цель-C не поддерживает пространства имен, необходимо использовать `[Register]` атрибут, чтобы изменить имя типа, которое Xamarin. iOS будет регистрировать в среде выполнения цели-c. C# Тип должен также наследовать от [Foundation. нсобжект](xref:Foundation.NSObject):

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

В-третьих, ознакомьтесь с документацией по цели — C и Создайте экземпляры [обжкрунтиме. Selector](xref:ObjCRuntime.Selector) для каждого селектора, который вы хотите использовать. Поместите их в тело класса:

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

В четвертом, вашему типу потребуется предоставить конструкторы. *Необходимо* связать вызов конструктора с конструктором базового класса. `[Export]` Атрибуты позволяют коду цели-C вызывать конструкторы с указанным именем селектора:

```csharp
[Export("init")]
public NSEnumerator()
    : base(NSObjectFlag.Empty)
{
    Handle = Messaging.IntPtr_objc_msgSend(this.Handle, selInit.Handle);
}
```

```csharp
// This constructor must be present so that Xamarin.iOS
// can create instances of your type from Objective-C code.
public NSEnumerator(IntPtr handle)
    : base(handle)
{
}
```

Пятая, предоставьте методы для каждого из селекторов, объявленных на шаге 3. Они будут использовать `objc_msgSend()` для вызова селектора в собственном объекте. Обратите внимание на использование [Runtime. жетнсобжект ()](xref:ObjCRuntime.Runtime.GetNSObject*) для преобразования `IntPtr` в соответствующий `NSObject` тип (подтип). Если нужно, чтобы метод был вызываемым из кода цели-C, элемент *должен* быть **виртуальным**.

```csharp
[Export("nextObject")]
public virtual NSObject NextObject()
{
    return Runtime.GetNSObject(
        Messaging.IntPtr_objc_msgSend(this.Handle, selNextObject.Handle));
}
```

```csharp
// Note that for properties, [Export] goes on the get/set method:
public virtual NSArray AllObjects {
    [Export("allObjects")]
    get {
        return (NSArray) Runtime.GetNSObject(
            Messaging.IntPtr_objc_msgSend(this.Handle, selAllObjects.Handle));
    }
}
```

Совместное размещение:

```csharp
using System;
using Foundation;
using ObjCRuntime;

namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        static Selector selInit       = new Selector("init");
        static Selector selAllObjects = new Selector("allObjects");
        static Selector selNextObject = new Selector("nextObject");

        [Export("init")]
        public NSEnumerator()
            : base(NSObjectFlag.Empty)
        {
            Handle = Messaging.IntPtr_objc_msgSend(this.Handle,
                selInit.Handle);
        }

        public NSEnumerator(IntPtr handle)
            : base(handle)
        {
        }

        [Export("nextObject")]
        public virtual NSObject NextObject()
        {
            return Runtime.GetNSObject(
                Messaging.IntPtr_objc_msgSend(this.Handle,
                    selNextObject.Handle));
        }

        // Note that for properties, [Export] goes on the get/set method:
        public virtual NSArray AllObjects {
            [Export("allObjects")]
            get {
                return (NSArray) Runtime.GetNSObject(
                    Messaging.IntPtr_objc_msgSend(this.Handle,
                        selAllObjects.Handle));
            }
        }
    }
}
```
