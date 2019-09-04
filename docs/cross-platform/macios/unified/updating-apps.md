---
title: Обновление существующих приложений до Unified API
description: В этом документе содержатся ссылки на различные руководства, в которых описывается обновление приложений Xamarin на Unified API. В нем обсуждаются приложения Xamarin. iOS, приложения Xamarin. Mac. Приложения Xamarin. Forms, собственные типы в кросс-платформенных приложениях и привязки проектов.
ms.prod: xamarin
ms.assetid: 8A654C95-5DCA-4BB5-A582-F96C2BECC81C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 021139c817d2b0eb430a6c8fe09dbd07b7c17813
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/03/2019
ms.locfileid: "70226151"
---
# <a name="updating-existing-apps-to-the-unified-api"></a>Обновление существующих приложений до Unified API

> [!IMPORTANT]
> Classic API Xamarin, который предшествует Unified API, является устаревшим.
> - Последняя версия Xamarin. iOS для поддержки Classic API (с помощью файла. dll) была Xamarin. iOS 9,10.
> - Xamarin. Mac по-прежнему поддерживает Classic API, но больше не обновляется. Поскольку это не рекомендуется, разработчики должны переместить свои приложения на Unified API.

## <a name="how-to-update-your-apps"></a>Обновление приложений

Обновление приложений выполняется в три этапа.

1. Исправьте все предупреждения компилятора в существующем коде, в частности те, которые относятся к устаревшим API.

2. Используйте встроенное средство миграции, чтобы Visual Studio для Mac для обновления файлов проекта и пространств имен.

3. Исправьте оставшиеся ошибки компилятора, относящиеся к новым [типам 64-](~/cross-platform/macios/nativetypes.md) и [другим API](~/cross-platform/macios/unified/overview.md#deprecated-typos) , которые были изменены. Ознакомьтесь с [этими советами](~/cross-platform/macios/unified/updating-tips.md) , чтобы получить дополнительные сведения о ручных обновлениях, которые могут потребоваться.

Для каждого продукта доступны специальные руководства, которые помогут вам обновить приложения до Unified API и 64-разрядной поддержки:

### <a name="xamarinios-appscross-platformmaciosunifiedupdating-ios-appsmd"></a>[Приложения Xamarin. iOS](~/cross-platform/macios/unified/updating-ios-apps.md)

Существующие приложения Xamarin. iOS можно обновить до Unified API, используя средство автоматической миграции, встроенное в для Visual Studio для Mac. После этого могут потребоваться некоторые дополнительные исправления, как описано в [этих инструкциях](~/cross-platform/macios/unified/updating-ios-apps.md) и [советах](~/cross-platform/macios/unified/updating-tips.md).

### <a name="xamarinmac-appscross-platformmaciosunifiedupdating-mac-appsmd"></a>[Приложения Xamarin. Mac](~/cross-platform/macios/unified/updating-mac-apps.md)

Существующие приложения Xamarin. Mac можно обновить до Unified API, используя средство автоматической миграции, встроенное в для Visual Studio для Mac. После этого могут потребоваться некоторые дополнительные исправления, как описано в [этих инструкциях](~/cross-platform/macios/unified/updating-mac-apps.md) и [советах](~/cross-platform/macios/unified/updating-tips.md).

### <a name="xamarinforms-appscross-platformmaciosunifiedupdating-xamarin-forms-appsmd"></a>[приложения Xamarin.Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md).

Выполните эти инструкции, чтобы обновить существующее решение Xamarin. Forms с помощью проекта iOS для использования Unified API. Поддержка Unified API доступна только в Xamarin. Forms 1,3 и более поздних версиях, поэтому [в инструкциях](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md) также объясняется, как обновить приложение Xamarin. Forms до версии 1,3. Эти [Советы](~/cross-platform/macios/unified/updating-tips.md) могут помочь в обновлении любого собственного кода iOS в пользовательских модулях подготовки отчетов или службах зависимостей.

## <a name="working-with-native-types-in-cross-platform-appscross-platformmaciosnativetypesmd"></a>[Работа с собственными типами в кроссплатформенных приложениях](~/cross-platform/macios/nativetypes.md)

В этой статье рассматривается использование новых типов iOS Unified API Native Type (НИНТ, нуинт, нфлоат) в кросс-платформенном приложении, где код используется для устройств, не относящихся к iOS, таких как Android или Windows Phoneные ОС. Он предоставляет подробные сведения о том, когда следует использовать собственные типы и предоставляет несколько возможных решений для случаев, когда новый тип должен использоваться с кодом кросс-платформенного кода.

## <a name="update-bindings-to-the-unified-api"></a>Обновление привязок к Unified API

Клиентам, которые создали привязки к библиотекам цели-C, потребуется обновить проект привязки для отражения изменений в базовом API (где некоторые типы теперь будут 64-разрядными).
Выполните эти инструкции, чтобы [обновить существующий проект привязки для поддержки Unified API](~/cross-platform/macios/unified/update-binding.md).

## <a name="related-links"></a>Связанные ссылки

- [Обновление приложений iOS](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Обновление приложений Mac](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Обновление приложений Xamarin. Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Обновление привязок](~/cross-platform/macios/unified/update-binding.md)
- [Обновление советов](~/cross-platform/macios/unified/updating-tips.md)
- [Различия между классическими и Unified APIми](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
