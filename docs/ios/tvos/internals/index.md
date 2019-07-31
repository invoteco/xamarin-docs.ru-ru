---
title: tvOS в Xamarin — внутренние
description: Документы, описывающие внутреннюю работу tvOS в Xamarin, основанную на Xamarin. iOS. Содержимое ссылки описывает сборки, целевые платформы и связанные понятия iOS.
ms.prod: xamarin
ms.assetid: 8C076FED-9C03-44DE-9723-0E20272DD16B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/07/2016
ms.openlocfilehash: 45341418779405e52251eb07576c8e5fa9f09297
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68647115"
---
# <a name="tvos-in-xamarin-internals"></a>tvOS в Xamarin — внутренние 

## <a name="assembliesiostvosinternalsassembliesmd"></a>[Сборки](~/ios/tvos/internals/assemblies.md)

Список сборок, поддерживаемых Xamarin для приложений Xamarin. tvOS.

## <a name="target-frameworksiostvosinternalsframeworksmd"></a>[Целевые платформы](~/ios/tvos/internals/frameworks.md)

В этой статье рассматриваются типы целевых платформ (библиотеки базовых классов), доступные в Xamarin. tvOS, и последствия выбора конкретной цели для приложения Xamarin. tvOS.

## <a name="related-ios-articles"></a>Связанные статьи по iOS

Следующие статьи относятся только к iOS, но относятся к tvOS (поскольку tvOS 9 является подмножеством iOS 9).

### <a name="unified-apicross-platformmaciosunifiedindexmd"></a>[Unified API](~/cross-platform/macios/unified/index.md)

В появились новые унифицированные интерфейсы API, позволяющие упростить совместное использование кода между Apple TV и базой кода iOS, а также поддержку 64-разрядных API и 64-разрядной компиляции.  

### <a name="api-designiosinternalsapi-designindexmd"></a>[Структура API](~/ios/internals/api-design/index.md)

Описывает принципы разработки, лежащие в основе привязки API.

### <a name="limitationsiosinternalslimitationsmd"></a>[Ограничения](~/ios/internals/limitations.md)

В этом разделе описываются ловушки и ограничения, которые следует учитывать при работе с Xamarin. iOS, многие из которых применимы к Xamarin. tvOS.

### <a name="linkeriosdeploy-testlinkermd"></a>[Компоновщик](~/ios/deploy-test/linker.md)

Описание работы компоновщика для обеспечения минимального возможного пакета приложения, а также изменения параметров и использования.

### <a name="localization-and-internationalizationiosapp-fundamentalslocalizationindexmd"></a>[Локализация и Международная связь](~/ios/app-fundamentals/localization/index.md)

В этом руководстве описывается добавление кодировок в приложение Xamarin. iOS для поддержки интернационализации.

### <a name="mtouchiosdeploy-testmtouchmd"></a>[mtouch](~/ios/deploy-test/mtouch.md)

Сведения о средстве командной строки mtouch.exe, которое преобразует проект в приложение, которое можно использовать в iOS.

### <a name="linking-native-librariesiosplatformnative-interopmd"></a>[Связывание собственных библиотек](~/ios/platform/native-interop.md)

Xamarin. iOS поддерживает связывание с собственными библиотеками C и библиотеками цели-C. В этом документе описывается, как связать собственные библиотеки C с проектом Xamarin. iOS. Сведения о том, как сделать то же самое для библиотек цели-c&nbsp; , см. в документе « [Binding цели-c](~/ios/platform/binding-objective-c/index.md)&nbsp;» Types.

## <a name="objective-c-selectorsiosinternalsobjective-c-selectorsmd"></a>[Селекторы цели-C](~/ios/internals/objective-c-selectors.md)

Примечания и использование для вызова селекторов цели-C (методов) напрямую.

### <a name="systemdataiosdata-cloudsystemdatamd"></a>[System.Data](~/ios/data-cloud/system.data.md)

Сведения и инструкции по использованию System. Data для доступа к встроенной системе базы данных SQLite.

### <a name="threadingiosapp-fundamentalsthreadingmd"></a>[Работа с потоками](~/ios/app-fundamentals/threading.md)

Примечания по использованию потоков в приложениях Xamarin. iOS.

### <a name="xib-code-generationiosinternalsxib-code-generationmd"></a>[Создание кода для XIB-файлов](~/ios/internals/xib-code-generation.md)

Как Visual Studio для Mac интегрируется с Interface Builder Xcode, чтобы можно было использовать Interface Builder для разработки пользовательского интерфейса.

## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [Руководства по tvOSму интерфейсу](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Руководством по программированию приложений для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
