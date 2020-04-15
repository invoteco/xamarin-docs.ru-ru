---
title: Среда выполнения для приложений Xamarin.iOS
description: Этот документ описывает настройку временных и постоянных переменных среды для приложения Xamarin.iOS. Эти переменные можно установить в свойствах проекта или с помощью дополнительных аргументов для средства упаковки mtouch.
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/05/2017
ms.openlocfilehash: b2452c50fa978f3cabb718afe7330bf0fb701ce3
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "73030245"
---
# <a name="execution-environment-for-xamarinios-apps"></a>Среда выполнения для приложений Xamarin.iOS

*Среда выполнения* — это набор переменных среды, которые влияют на выполнение программ. Установить переменные среды можно временно в свойствах проекта или окончательно, указав дополнительные аргументы для средства упаковки mtouch.

## <a name="temporary-environment-variables"></a>Временные переменные среды

Временные переменные задаются в окне проекта **Свойства**/**Параметры** окна в разделе **Запуск > Общие**. Эти переменные среды действуют только при запуске приложения с помощью Visual Studio для Mac. Если приложение запускается вручную, переменные среды не будут заданы.

## <a name="permanent-environment-variables"></a>Постоянные переменные среды

Переменные среды с постоянной задаются при указании дополнительных аргументов для средства упаковки mtouch. Эти переменные среды скомпилированы в исполняемый файл. Они будут заданы, даже если приложение не запускается из Visual Studio для Mac.

## <a name="example"></a>Пример

```csharp
# log all exceptions to the device log
--setenv:MONO_TRACE=E:all

# to pass multipe environment variables, use --setenv multiple times
--setenv:MONO_TRACE=E:all --setenv:GC_DONT_GC=1
```
