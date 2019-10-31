---
title: Отладка на устройстве
description: В этой статье содержатся сведения об отладке приложения Xamarin.Android на физическом устройстве Android.
ms.prod: xamarin
ms.assetid: 153D3746-A27F-198B-48FE-D219C0133A79
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: e697459b20a481b1d2bada69677647ad4fbd3023
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021591"
---
# <a name="debug-on-device"></a>Отладка на устройстве

_В этой статье содержатся сведения об отладке приложения Xamarin.Android на физическом устройстве Android._

## <a name="debug-on-device-overview"></a>Обзор отладки на устройстве

Отладку приложения Xamarin.Android можно выполнить на устройстве Android с помощью Visual Studio для Mac или Visual Studio. Перед отладкой устройство необходимо [настроить для разработки](~/android/get-started/installation/set-up-device-for-development.md) и подключить к ПК или Mac.

## <a name="debug-application"></a>Отладка приложения

После подключения устройства к компьютеру отладка приложения Xamarin.Android выполняется так же, как отладка любого продукта Xamarin или приложения .NET. Убедитесь, что в интегрированной среде разработки выбраны конфигурация **Отладка** и внешнее устройство. Это необходимо, чтобы обеспечить доступность соответствующих отладочных символов и возможность подключения среды IDE к запущенному приложению: 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Выбрана конфигурация "Отладка"](debug-on-device-images/image1-vs.png)

Затем в коде устанавливается точка останова:

![Точка останова, установленная в строке кода](debug-on-device-images/image2-vs.png)

После выбора устройства Xamarin.Android подключится к устройству, развернет приложение и запустит его. По достижении точки останова отладчик остановит приложение, которое затем проходит отладку так же, как любые другие приложения C#: 

![Достигнута точка останова](debug-on-device-images/image3-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

![Выбрана конфигурация "Отладка"](debug-on-device-images/image1-xs.png)

Затем в коде устанавливается точка останова:

![Точка останова, установленная в строке кода](debug-on-device-images/image2-xs.png)

После выбора устройства Xamarin.Android подключится к устройству, развернет приложение и запустит его. По достижении точки останова отладчик остановит приложение, которое затем проходит отладку так же, как любые другие приложения C#: 

![Достигнута точка останова](debug-on-device-images/image3-xs.png)

-----

## <a name="summary"></a>Сводка

В этом документе рассматривалась процедура отладки приложения Xamarin.Android путем установки точки останова и выбора целевого устройства.

## <a name="related-links"></a>Связанные ссылки

- [Настройка устройства для разработки](~/android/get-started/installation/set-up-device-for-development.md)
- [Установка атрибута Debuggable](~/android/deploy-test/debuggable-attribute.md)
