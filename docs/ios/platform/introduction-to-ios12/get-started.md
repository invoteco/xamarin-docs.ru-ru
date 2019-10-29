---
title: Начало работы с iOS 12, tvOS 12 и watchOS 5
description: В этом документе описывается, как настроить для сборки приложений iOS 12, tvOS 12 и watchOS 5 с помощью Xamarin. В нем описывается загрузка Xcode 10 и обновление Visual Studio для Mac и Visual Studio 2017.
ms.prod: xamarin
ms.assetid: 6C0F0133-1A5F-408B-8BCA-BDCA313A55C2
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/19/2018
ms.openlocfilehash: 1db249a9e07f178461bcb052508d08f54ecea121
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032019"
---
# <a name="get-started-with-ios-12-tvos-12-and-watchos-5"></a>Начало работы с iOS 12, tvOS 12 и watchOS 5

В этом документе описывается, как приступить к созданию приложений Xamarin, которые вызывают интерфейсы API, выпущенные с Xcode 10, для iOS 12, tvOS 12 и watchOS 5.

## <a name="download-and-install"></a>Скачать и установить

1. **Установка Xcode 10** . Зарегистрированные разработчики Apple могут скачать и установить последнюю версию Xcode 10 на [портале разработчика Apple](https://developer.apple.com/download/) или в **магазине приложений**.

2. **Запустите Xcode 10** — запустите Xcode 10 перед обновлением и запуском Visual Studio для Mac или Visual Studio 2017, так как в нем устанавливаются средства, необходимые для работы Xamarin.

3. **Обновление Visual Studio для Mac и Visual Studio 2017** — убедитесь, что у вас установлена последняя стабильная версия Xamarin.

4. _(необязательно)_ **установите iOS 12 на устройствах iOS** —

   Для тестирования приложений, использующих API-интерфейсы, появившиеся в Xcode 10, зарегистрированные разработчики Apple могут [загрузить](https://developer.apple.com/download) и установить операционную систему на своих устройствах.

   > [!TIP]
   > Даже если ваше приложение не использует новые API, обязательно создайте его с новейшими пакетами SDK для Xcode 10 и протестируйте его, чтобы убедиться, что все работает правильно. Если приложение не вызывает новые API, его можно перекомпилировать с помощью этих новых пакетов SDK и протестировать на устройствах, которые еще не были обновлены до новой операционной системы.
   >
   > Перед обновлением устройств до последних версий операционной системы с Apple для тестирования приложений Xamarin необходимо выполнить следующие действия:
   >
   > - Сведения об обновлениях операционной системы см. в [заметках о выпуске Apple](https://developer.apple.com/download/) .
   > - Ознакомьтесь с [записью блога](https://releases.xamarin.com/preview-release-xcode-10-beta-6/)о предварительной версии Xamarin.

## <a name="related-links"></a>Связанные ссылки

- [Скачать Xcode](https://developer.apple.com/download/)
