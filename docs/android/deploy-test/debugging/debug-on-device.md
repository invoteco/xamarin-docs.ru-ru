---
title: Отладка на устройстве
description: В этой статье содержатся сведения об отладке приложения Xamarin.Android на физическом устройстве Android.
ms.prod: xamarin
ms.assetid: 153D3746-A27F-198B-48FE-D219C0133A79
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 3ca524e451a7a4eb838805c839b33c4b9dd6bddd
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "75556539"
---
# <a name="debug-on-an-android-device"></a>Отладка на устройстве Android

_В этой статье содержатся сведения об отладке приложения Xamarin.Android на физическом устройстве Android._

Отладку приложения Xamarin.Android можно выполнить на устройстве Android с помощью Visual Studio для Mac или Visual Studio. Перед отладкой устройство необходимо [настроить для разработки](~/android/get-started/installation/set-up-device-for-development.md) и подключить к ПК или Mac.

## <a name="debug-application"></a>Отладка приложения

После подключения устройства к компьютеру отладка приложения Xamarin.Android выполняется так же, как отладка любого продукта Xamarin или приложения .NET. Убедитесь, что в интегрированной среде разработки выбраны конфигурация **Отладка** и внешнее устройство. Это необходимо, чтобы обеспечить доступность соответствующих отладочных символов и возможность подключения среды IDE к запущенному приложению: 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Выбрана конфигурация "Отладка"](debug-on-device-images/image1-vs.png)

Затем в коде устанавливается точка останова:

![Точка останова, установленная в строке кода](debug-on-device-images/image2-vs.png)

После выбора устройства Xamarin.Android подключится к устройству, развернет приложение и запустит его. По достижении точки останова отладчик остановит приложение, которое затем проходит отладку так же, как любые другие приложения C#: 

![Достигнута точка останова](debug-on-device-images/image3-vs.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

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
