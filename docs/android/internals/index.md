---
title: Дополнительные понятия и внутренние компоненты
description: Базовая архитектура для Xamarin. Android и ее разработки API.
ms.prod: xamarin
ms.assetid: CC6A0D52-E9FA-4270-B3FA-84660621D6D5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/21/2018
ms.openlocfilehash: 4f860d493c5709e2f6c7f89e6f3a50981cf62dc3
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757824"
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
