---
title: Среда выполнения для приложений Xamarin.iOS
description: Этот документ описывает настройку временных и постоянных переменных среды для приложения Xamarin.iOS. Эти переменные можно установить в свойствах проекта или с помощью дополнительных аргументов для средства упаковки mtouch.
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/05/2017
ms.openlocfilehash: 3d85fa063580e9619ef433e98f6e6a0e4121ee37
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70289002"
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

