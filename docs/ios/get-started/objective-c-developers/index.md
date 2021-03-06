---
title: Xamarin для разработчиков на Objective-C
description: Этот документ описывает Xamarin.iOS для разработчиков Objective-C. Он содержит ссылки на руководства по переходу из Objective-C в C#, привязке библиотеки Objective-C для использования в C# и созданию кроссплатформенных мобильных приложений.
ms.prod: xamarin
ms.assetid: 9F3C86A3-403E-4025-99CA-99FCA86DC828
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/05/2017
ms.openlocfilehash: 2c5451b379968230d75599601052e3a33f5b5951
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "73022446"
---
# <a name="xamarin-for-objective-c-developers"></a>Xamarin для разработчиков на Objective-C

Xamarin позволяет разработчикам, создающим решения для iOS, перемещать существующий код (без пользовательского интерфейса) в независящий от платформ код C#, который они смогут использовать везде, где доступен язык C#, включая Android (через Xamarin.Android) и любые версии Windows. При этом возможность использовать C# с Xamarin подразумевает применение существующих навыков работы с кодом Objective-C. Более того, знание Objective-C даже повышает вашу ценность как разработчика для Xamarin.iOS, так как Xamarin предоставляет все хорошо вам знакомые API-интерфейсы iOS и OS X, включая UIKit, Core Animation, Core Foundation, Core Graphics и многие другие. В то же время вы получаете в свое распоряжение всю мощь C#, включая такие возможности, как LINQ и универсальные шаблоны, а также библиотеки базовых классов .NET. Все это вы сможете применять в своих приложениях.

Кроме того, Xamarin позволяет использовать существующие ресурсы Objective-C через специальную технологию привязок. Вам нужно лишь создать статическую библиотеку на Objective-C и предоставить ее в C# через привязку, как показано на следующей схеме.

 [![](images/01-bindings.png "A static library in Objective-C exposed to C# via a binding")](images/01-bindings.png#lightbox)

Такой подход применим не только для кода без пользовательского интерфейса. Привязки могут предоставлять и код пользовательского интерфейса, разработанный на Objective-C.

## <a name="transitioning-from-objective-c"></a>Переход от Objective-C

На нашем сайте доступно много документации, которая поможет быстрее перейти к использованию Xamarin, интегрируя код C# со знакомыми вам технологиями. Вот некоторые важные сведения, которые помогут приступить к работе:

- [Руководство по C# для разработчиков Objective-C](primer.md) быстро введет в курс дела тех разработчиков, которые уже знакомы с Objective-C и намерены перейти к использованию Xamarin и C#. 
- [Пошаговое руководство: привязка библиотек Objective-C](~/ios/platform/binding-objective-c/walkthrough.md) поможет применить существующий код Objective-C в приложении Xamarin.iOS. 

## <a name="binding-objective-c"></a>Привязка Objective-C

Когда вы разберетесь, чем C# отличается от Objective-C, и ознакомитесь с описанной выше технологией привязки, вы можете переходить на платформу Xamarin. Дополнительные сведения о технологии Xamarin.iOS для дальнейшего изучения, включая подробную документацию по привязке, вы найдете в разделе [о привязке Objective-C](~/ios/platform/binding-objective-c/index.md).

## <a name="cross-platform-development"></a>Межплатформенная разработка

Когда вы перейдете на Xamarin.iOS, вам будут полезно ознакомиться с нашими руководствами. Они содержат практические примеры уже разработанных приложений и рекомендации по [созданию кроссплатформенного и повторного используемого кода](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md).
