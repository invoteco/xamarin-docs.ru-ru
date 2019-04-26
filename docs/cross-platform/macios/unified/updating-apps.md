---
title: Обновление существующих приложений в Unified API
description: Этот документ содержит ссылки на различные руководства, описывающие способ обновления приложения Xamarin на единый API. Здесь рассматриваются приложения Xamarin.iOS, приложения Xamarin.Mac. Приложения Xamarin.Forms, собственные типы в кросс платформенных приложений и проектов привязки.
ms.prod: xamarin
ms.assetid: 8A654C95-5DCA-4BB5-A582-F96C2BECC81C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 2d09be7b85980e5c5a8eb209dc1b4ff3136c34b3
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61195600"
---
# <a name="updating-existing-apps-to-the-unified-api"></a>Обновление существующих приложений в Unified API

> [!IMPORTANT]
> Классический API Xamarin, перед на единый API, является устаревшим. 
> - Последняя версия Xamarin.iOS, для поддержки классический API (monotouch.dll) был Xamarin.iOS 9.10.
> - Xamarin.Mac по-прежнему поддерживает классический API, но больше не обновляется. Так как она устарела, разработчикам следует переместить свои приложения на единый API.

## <a name="how-to-update-your-apps"></a>Обновление приложений

Xamarin University имеет свободно видео на **обновление до iOS Unified API**. Посетите [Xamarin University лекциям](http://university.xamarin.com/lightninglectures) посмотреть!

[ ![](updating-apps-images/xamu-video-sml.png "Xamarin University")](http://university.xamarin.com/lightninglectures)

Чтобы обновить приложения трех этапов:

1. Исправьте все предупреждения компилятора в существующем коде, особенно для тех, относящиеся к устаревшие интерфейсы API.

2. Используйте средство миграции, встроенных в Visual Studio для Mac, чтобы обновить файлы проекта и пространства имен.

3. Исправление, остальные ошибки компилятора, относящиеся к новому [64 типы](~/cross-platform/macios/nativetypes.md) и [другие API-интерфейсы](~/cross-platform/macios/unified/overview.md#deprecated-typos) , были изменены. Ознакомьтесь с [эти советы](~/cross-platform/macios/unified/updating-tips.md) Дополнительные сведения о обновления вручную, которые могут потребоваться.

Для каждого продукта, помогут вам обновить приложения Unified API и поддержка 64-разрядных доступны определенные руководства:

### <a name="xamarinios-appscross-platformmaciosunifiedupdating-ios-appsmd"></a>[Приложения Xamarin.iOS](~/cross-platform/macios/unified/updating-ios-apps.md)

Можно обновить существующие приложения Xamarin.iOS на единый API, с помощью инструмента автоматического перемещения, встроенных в Visual Studio для Mac. Некоторые дополнительные исправления могут затем быть обязательными, как описано в [эти инструкции](~/cross-platform/macios/unified/updating-ios-apps.md) и [советы](~/cross-platform/macios/unified/updating-tips.md).

###  <a name="xamarinmac-appscross-platformmaciosunifiedupdating-mac-appsmd"></a>[Приложения Xamarin.Mac](~/cross-platform/macios/unified/updating-mac-apps.md)

Существующие приложения Xamarin.Mac можно обновить на единый API, с помощью инструмента автоматического перемещения, встроенных в Visual Studio для Mac. Некоторые дополнительные исправления могут затем быть обязательными, как описано в [эти инструкции](~/cross-platform/macios/unified/updating-mac-apps.md) и [советы](~/cross-platform/macios/unified/updating-tips.md).

###  <a name="xamarinforms-appscross-platformmaciosunifiedupdating-xamarin-forms-appsmd"></a>[Приложения Xamarin.Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)

Следуйте этим инструкциям, чтобы заменить существующее решение Xamarin.Forms в проект iOS для использования на единый API. Единой API поддерживается только в Xamarin.Forms 1.3 и более поздние версии, поэтому [инструкциям](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md) также объясняется, как выполнить обновление до версии 1.3 приложение Xamarin.Forms. Эти [советы](~/cross-platform/macios/unified/updating-tips.md) может помочь обновление любой машинный код для iOS в пользовательские модули подготовки отчетов или служб зависимостей.

## <a name="working-with-native-types-in-cross-platform-appscross-platformmaciosnativetypesmd"></a>[Работа с собственными типами в кроссплатформенных приложениях](~/cross-platform/macios/nativetypes.md)

В этой статье рассматривается использование нового iOS Unified API, собственные типы (nint, nuint, nfloat) в приложении кросс платформенных, где код используется совместно с устройства без iOS, например, Android или ОС Windows Phone. Он позволяет понять, когда следует использовать собственные типы и предоставляет несколько возможных решений для случаев, где необходимо использовать новый тип с кросс платформенного кода.

## <a name="update-bindings-to-the-unified-api"></a>Обновить привязки в Unified API

Клиенты, которые созданы привязки для библиотек Objective-C будет необходимо обновить проект привязки в соответствии с изменениями в основной API (где некоторые типы теперь будет 64-разрядных).
Следуйте указаниям, приведенным к [обновить существующий проект привязки для поддержки на единый API](~/cross-platform/macios/unified/update-binding.md).

## <a name="related-links"></a>Связанные ссылки

- [Обновление приложения для iOS](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Обновление приложения Mac](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Обновление приложений Xamarin.Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Обновление привязки](~/cross-platform/macios/unified/update-binding.md)
- [Обновление советы](~/cross-platform/macios/unified/updating-tips.md)
- [Классический и отличия Unified API](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
