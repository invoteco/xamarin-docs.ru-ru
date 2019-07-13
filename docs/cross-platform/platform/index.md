---
title: Программирование языковой поддержке в Xamarin
description: В этом документе описывается различных языков программирования, поддерживаемых Xamarin. В нем описывается C#, F#, переносимых Visual Basic.NET и шаблоны Razor.
ms.prod: xamarin
ms.assetid: CEE8C464-67D7-45F4-9614-EAEF5217CACC
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 6c0b8e6de0c414fb708c4027f4c536a21b6b011a
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/13/2019
ms.locfileid: "67864358"
---
# <a name="programming-language-support-in-xamarin"></a>Программирование языковой поддержке в Xamarin

## <a name="c"></a>C# 

### <a name="async-support-overviewcross-platformplatformasyncmd"></a>[Общие сведения о поддержке асинхронного выполнения](~/cross-platform/platform/async.md)

Версия 5 C# появились два новых ключевых слов для выражения асинхронных операций: async и await. Эти ключевые слова можно написать простой код, который использует библиотеку параллельных задач для выполнения длительных операций (например, доступ к сети) в другом потоке и легко получить доступ к результатам по завершении. Последние версии Xamarin.iOS и Xamarin.Android поддерживает async и await — в этом документе приводятся объяснения и пример использования нового синтаксиса с помощью Xamarin.

### <a name="c-6-language-featurescross-platformplatformcsharp-sixmd"></a>[6 возможностей языка C#](~/cross-platform/platform/csharp-six.md)

Последнюю версию C# язык — версии 6 – постоянно развивается язык меньше стандартных действий, внесена большая ясность и согласованность. Более четкий синтаксис инициализации, возможность использования `await` в `catch/finally` блоков и условием null `?` оператор особенно полезны.

## <a name="ffsharpindexmd"></a>[F#](fsharp/index.md)

Создание мобильных приложений с помощью F# и Xamarin.

## <a name="portable-visual-basicnetcross-platformplatformvisual-basicindexmd"></a>[Переносимые Visual Basic.NET](~/cross-platform/platform/visual-basic/index.md)

Visual Studio поддерживает создание переносимых библиотек классов, с помощью Visual Basic.NET, который затем можно включить в приложения Xamarin. В этой статье показано, как создать новый переносимой библиотеки Классов Visual Basic и затем использовать его в образце приложения Xamarin.iOS, Xamarin.Android и Windows Phone.

## <a name="building-html-views-using-razor-templatescross-platformplatformrazor-html-templatesindexmd"></a>[Создание HTML-представления с помощью шаблонов Razor](~/cross-platform/platform/razor-html-templates/index.md)

Xamarin позволяет разработчикам использовать подсистему шаблонов Razor, изначально появилась в ASP.NET MVC, вместе с C# легко объединить данные с HTML, Javascript и CSS, не требуя вручную создавать HTML-строки в коде.
В этой статье показано, как использовать шаблоны Razor с помощью Xamarin для iOS и Android.
