---
title: Введение в iOS 11
description: В этом документе содержатся ссылки на различные руководства, в которых описываются функции iOS 11, включая ARKit, CoreML, Мапкит, Пдфкит, SiriKit, концепцию инфраструктуры и многое другое.
ms.prod: xamarin
ms.assetid: 22C38EA6-6DA9-4B92-B41B-814E589033F6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/19/2017
ms.openlocfilehash: fa4744ba217d602dbd3134e616ac3bc4d47f7b76
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/28/2019
ms.locfileid: "70120818"
---
# <a name="introduction-to-ios-11"></a>Введение в iOS 11

![Пример ARKit](images/arkit.png) ![Объекты, размещенные в AR](images/arkit2.png) ![Пример CoreML](images/coreml.png) ![Пример Мапкит](images/mapkit.png) ![Пример прямоугольников концепции](images/vision1.png) ![Пример лиц с видением](images/vision2.png) ![Пример перетаскивания](images/drag-drop.png) ![Пример перетаскивания](images/drag-drop2.png) ![Пример SiriKit](images/sirikit.png)

iOS 11 включает в себя множество новых и усовершенствованных возможностей в различных платформах:

## <a name="preparing-your-app-for-ios-11updating-your-appindexmd"></a>[Подготовка приложения для iOS 11](updating-your-app/index.md)

В Apple появились обновления архитектуры, новые визуальные изменения и обновленный процесс iTunes Connect для iOS 11. Используйте это руководством, чтобы убедиться, что приложение Xamarin. iOS подготовлено для нового выпуска.

## <a name="arkitarkitindexmd"></a>[ARKit](arkit/index.md)

ARKit расширяет возможности iOS, позволяя пользователям взаимодействовать с миром через камеру устройства.
С помощью Xamarin можно также использовать [ARKit с UrhoSharp](arkit/urhosharp.md).

## <a name="coremlcoremlmd"></a>[CoreML](coreml.md)

Модели машинного обучения можно интегрировать в приложения iOS 11 с помощью CoreML. Платформа CoreML предоставляет простой API для внедрения существующих моделей в проекты приложений, чтобы разрешить анализ проблем непосредственно в приложении.

## <a name="corenfccorenfcmd"></a>[CoreNFC](corenfc.md)

устройства iPhone 7 и более поздних версий могут читать Теги NFC, позволяя приложениям обнаруживать в мире продукты, места и т. д.

## <a name="drag-and-dropdrag-and-dropmd"></a>[Перетаскивание](drag-and-drop.md)

Платформа перетаскивания предоставляет поддержку iOS для перемещения данных по сенсорному экрану. На iPad можно перетаскивать как внутри, так и между различными приложениями. на iPhone можно перетащить только в пределах одного приложения. Существует поддержка различных типов настройки, включая расширенные типы данных, анимацию и обработку жестов Мультисенсорная.

## <a name="mapkitmapkitmd"></a>[MapKit](mapkit.md)

Мапкит имеет ряд усовершенствований, включая поддержку автоматического группирования маркеров и добавление компаса в представление.

## <a name="pdfkit"></a>пдфкит

Пдфкит теперь доступен в iOS 11, обеспечивая возможности создания и редактирования PDF-файлов в приложениях.

## <a name="sirikitsirikitmd"></a>[SiriKit](sirikit.md)

Siri теперь поддерживает еще больше взаимодействий, включая списки и примечания, а также другие улучшения, такие как альтернативные имена приложений.

## <a name="visionvisionmd"></a>[зрение](vision.md);

Предоставляет разнообразные функции обработки изображений и анализа в iOS, включая обнаружение и распознавание лиц, CoreML модели, новые API-интерфейсы обнаружения штрихкодов, обнаружение текста и горизонта, а также более общее обнаружение и отслеживание объектов.

## <a name="samples"></a>Примеры

У нас есть несколько C# [примеров](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS11) , которые помогут вам приступить к работе:

- [Пример ARKit](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-arkitsample)
- [ARKit размещения объектов](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-arkitplacingobjects)
- [ARKit и UrhoSharp](arkit/urhosharp.md)
- [Пример распознавания изображений CoreML](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlimagerecognition)
- [CoreML с настраиваемой моделью Azure](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlazuremodel)
- [Пример средства чтения тегов Коренфк](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-nfctagreader)
- [Перетащить & представление таблицы](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-draganddroptableview)
- [Перетаскивание представления коллекции &](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-draganddropcollectionview)
- [Перетаскивание пользовательского представления &](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-draganddropcustomview)
- [Пример перетаскивания & Драгбоард](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-draganddropdragboard)
- [Пример Мапкит](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-mapkitsample)
- [Пример SiriKit](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-sirikitsample)
- [Пример обновленной платформы фото](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-samplephotoapp)
- [Пример & концепции CoreML](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlvision)
- [Пример обнаружения прямоугольников концепции](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-visionrectangles/)
- [Пример обнаружения лиц с видением](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-visionfaces)
- [Пример мини-приложений Пдкфит](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-pdfannotationwidgetsadvanced)
- [Пример водяного знака Пдфкит](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-pdfdocumentwatermark)

## <a name="related-links"></a>Связанные ссылки

- [Примеры Xamarin iOS 11](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS11)
