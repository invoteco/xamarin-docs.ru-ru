---
title: Дополнительные понятия и внутренние компоненты
description: Базовая архитектура для Xamarin. Android и ее разработки API.
ms.prod: xamarin
ms.assetid: CC6A0D52-E9FA-4270-B3FA-84660621D6D5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/21/2018
ms.openlocfilehash: 97382243ac5f767d94a782b895401c1f2f8ae554
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027857"
---
# <a name="advanced-concepts-and-internals"></a>Дополнительные понятия и внутренние компоненты

_В этом разделе содержатся разделы, объясняющие архитектуру, разработку API и ограничения для Xamarin. Android. Кроме того, он включает разделы, объясняющие реализацию сборки мусора и сборки, доступные в Xamarin. Android. Так как Xamarin. Android является [открытым исходным](https://github.com/xamarin/xamarin-android)кодом, можно также понять внутреннюю работу Xamarin. Android, изучив его исходный код._

## <a name="architectureandroidinternalsarchitecturemd"></a>[Архитектура](~/android/internals/architecture.md)

В этой статье описывается базовая архитектура приложения Xamarin. Android. В этой статье объясняется, как приложения Xamarin. Android выполняются в среде выполнения Mono вместе с виртуальной машиной среды выполнения Android и объясняются такие ключевые понятия, как вызываемые оболочки Android и управляемые вызываемые оболочки. 

## <a name="api-designandroidinternalsapi-designmd"></a>[Структура API](~/android/internals/api-design.md)

В дополнение к основным библиотекам базовых классов, которые являются частью Mono, Xamarin. Android поставляется с привязками для различных интерфейсов API Android, позволяя разработчикам создавать собственные приложения Android с Mono.

В ядре Xamarin. Android есть механизм взаимодействия, который связывает C# мир с Java и предоставляет разработчикам доступ к API-интерфейсам Java из C# или других языков .NET.

## <a name="assembliescross-platforminternalsavailable-assembliesmd"></a>[Сборки](~/cross-platform/internals/available-assemblies.md)

Xamarin. Android поставляется с несколькими сборками. Так же как Silverlight — это расширенное подмножество сборок для настольных систем .NET, Xamarin. Android также является расширенным подмножеством нескольких сборок Silverlight и настольных систем .NET. 
