---
title: Дополнительные изменения в tvOS 10 Frameworks
description: В этом документе описываются незначительные изменения и улучшения, внесенные в существующие платформы в iOS 10. В нем рассматриваются обновления для Авфаундатион, Авкит, основных данных, основной графики, фундамента, GameKit, Гамеплайкит и т. д.
ms.prod: xamarin
ms.assetid: F771640A-F92E-4954-82D5-2D720434971E
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: 88039da5faf911386232d2b189b27a2921f8144c
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70289576"
---
# <a name="additional-tvos-10-frameworks-changes"></a>Дополнительные изменения в tvOS 10 Frameworks

В дополнение к значительным изменениям в tvOS, компания Apple внесла изменения в несколько существующих платформ в tvOS 10.

<a name="AV-Foundation-Framework" />

## <a name="avfoundation-framework-additions"></a>Дополнения Авфаундатион Framework

Платформа Авфаундатион включает следующие усовершенствования.

- В tvOS 10 приложение больше не реализует различные поведения [авплайеритем](https://developer.apple.com/reference/avfoundation/avplayeritem) на основе типа содержимого. Просто задайте `Rate` свойство, и авфаундатион определит, когда будет доступно достаточное содержимое для воспроизведения без ожидания.
- Новый `AVPlayerLooper` класс упрощает циклический перебор заданной части мультимедиа во время воспроизведения.

<a name="AVKit-Framework-Enhancements" />

## <a name="avkit-framework-enhancements"></a>Усовершенствования платформы Авкит

Платформа Авкит включает следующие усовершенствования.

- Теперь приложение имеет контроль над пропуском поведения [авплайервиевконтроллер](https://developer.apple.com/reference/avkit/avplayerviewcontroller) , поэтому жест пропуска может перемещаться к следующему элементу в списке воспроизведения или перемещаться в пределах текущего элемента.

<a name="Core-Data-Enhancements" />

## <a name="core-data-enhancements"></a>Усовершенствования основных данных

tvOS 10 включает следующие усовершенствования для основной платформы данных:

- Корневые объекты [нсманажедобжектконтекст](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) поддерживают одновременную ошибку и выборку без сериализации.
- Класс [нсперсистентсторекурдинатор](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) поддерживает пул хранилищ данных SQLite.
- [Нсманажедобжектконтекст](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) объекты с хранилищами данных SQLite в режиме журнала Wal поддерживают новую функцию создания запросов, где контексты управляемых объектов (MOC) могут быть закреплены в конкретных версиях базы данных для последующей выборки и сбоя транзакций.
- Использование высокого уровня `NSPersistenceContainer` для ссылки на [нсманажедобжектмодел](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) и другие основные ресурсы по `NSPersistentStoreCoordinator`настройке данных.
- Было добавлено несколько новых удобных методов для `NSManagedObject` упрощения выборки и создания подклассов.

Дополнительные сведения см. в [справочнике по основной платформе данных](https://developer.apple.com/reference/coredata)Apple.

<a name="Core-Graphics-Enhancements" />

## <a name="core-graphics-enhancements"></a>Основные улучшения графики

tvOS 10 включает следующие усовершенствования для основной графической платформы:

- Новый класс [кгколорконвертерреф](https://developer.apple.com/reference/coregraphics/cgcolorconverterref) можно использовать для выполнения ряда преобразований цветов.

<a name="Core-Image-Enhancements" />

## <a name="core-image-enhancements"></a>Усовершенствования основных образов

tvOS 10 вносит следующие улучшения в основную платформу образов:

- Метод `ImageWithExtent` класса [CIFilter](https://developer.apple.com/reference/coreimage/cifilter) можно использовать для вставки пользовательской обработки в операцию фильтрации. Основное изображение будет вызывать заданный обратный вызов между фильтрами при обработке изображения для вывода или показа.
- Теперь приложение может обрабатывать изображения в цветовом пространстве за пределами рабочего пространства контекста образа путем преобразования в цветовое пространство и из него до и после обработки.
- Для подготовки к `UIImage` просмотру в `UIImageView` объектах были сделаны некоторые улучшения производительности отрисовки (при резервном копировании в хранилищах образов основных образов). 
- `UIImage`объекты с тегами Wide-охват будут отображаться в виде цвета с широкими охватами в `UIImageView` объектах на устройствах iOS, поддерживающих широкий цвет.
- Код ядра образа ядра теперь может запрашивать определенные форматы вывода пикселей.

Кроме того, добавлены следующие новые фильтры основных образов:

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

<a name="Foundation-Enhancements" />

## <a name="foundation-enhancements"></a>Усовершенствования в фундаменте

В платформу Foundation для tvOS 10 были внесены следующие улучшения:

- Новый класс [нсдатеинтервал](https://developer.apple.com/reference/foundation/nsdateinterval) используется для вычисления интервалов даты и времени, таких как длительность, для сравнения интервалов и тестирования для пересечения интервалов.
- К классу [нслокал](https://developer.apple.com/reference/foundation/nslocale) были добавлены несколько новых свойств для получения локальных сведений и доступных форматов отображаемых данных.
- Используйте новый класс [нсмеасуремент](https://developer.apple.com/reference/foundation/nsmeasurement) для преобразования между разными единицами измерения (ЕИ) или вычисления значений в разных уомс.
- Используйте новый класс [нсмеасурементформаттер](https://developer.apple.com/reference/foundation/nsmeasurementformatter) , чтобы отформатировать локализованные измерения для отображения конечному пользователю.
- Используйте новые классы [нсунит](https://developer.apple.com/reference/foundation/nsunit) и [нсдименсион](https://developer.apple.com/reference/foundation/nsdimension) для представления конкретных уомс.

<a name="GameKit-Enhancements" />

## <a name="gamekit-enhancements"></a>Усовершенствования GameKit

В GameKit Framework в tvOS 10 были внесены следующие улучшения:

- Новый тип учетной записи только iCloud реализован классом [гкклаудплайер](https://developer.apple.com/reference/gamekit/gkcloudplayer) .
- Новый класс [гкгамесессион](https://developer.apple.com/reference/gamekit/gkgamesession) предоставляет обобщенное решение для управления хранилищем постоянных данных на Game Center. `GKGameSession`ведет список игроков, и приложение отвечает за реализацию того, как и когда дата участника сохраняется, извлекается или обменивается между игроками. Во многих случаях игровые сеансы могут заменять существующие соответствия, основанные на включении, совпадения в режиме реального времени или постоянные способы сохранения игр.

<a name="GameplayKit-Enhancements" />

## <a name="gameplaykit-enhancements"></a>Усовершенствования Гамеплайкит

В Гамеплайкит Framework в tvOS 10 были внесены следующие улучшения:

- Было добавлено описание процедурного создания шума, которое можно использовать для улучшения реальных текстур в естественном виде, добавления факта к перемещению камеры и помощи в создании полнофункциональных игр.
- Используйте пространственное секционирование, чтобы секционировать реальные данные игры для эффективного поиска.
- Добавлен новый Монте-Карло специалист по стратегиям ([гкмонтекарлостратегист](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) для исчерпывающего возможного вычисления перемещения.
- Добавлен новый API дерева принятия решений ([гкдеЦисионтри](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) и [гкдеЦисионноде](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) для улучшения искусственного проектирования.
- поддержка трехмерных объектов добавлена к существующим агентам и поведениям поиска пути с помощью новых классов [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) и [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) .
- Используйте новый класс [гкмешграф](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) для обеспечения высокопроизводительных и естественных путей.
- Новые классы [гксцене](https://developer.apple.com/reference/gameplaykit/gkscene) и [Гкскнодекомпонент](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) делают объединение гамеплайкит и SpriteKit проще, чем когда-либо.

<a name="Metal-Enhancements" />

## <a name="metal-enhancements"></a>Усовершенствования в металлах

В tvOS 10 были внесены следующие улучшения в среду металла:

- Трехмерные приложения и игры теперь могут использовать _тесселяцию_ для эффективного отображения сложных сцен и геометрии с помощью GPU.
- Используйте специализацию функции, чтобы создать высокооптимизированный набор функций для комбинирования материалов и освещения для сцены.
- Обеспечение точного контроля за выделением ресурсов для оптимизации производительности металлических приложений с помощью куч ресурсов и целевых объектов визуализации, поддерживающих память.

Дополнительные сведения см. в статье [по программированию для металлического программирования](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)Apple.

<a name="Metal-Performance-Shaders-Enhancements" />

## <a name="metal-performance-shaders-enhancements"></a>Улучшения в построителейх шейдеров производительности

В платформу шейдеров для металлической производительности в tvOS 10 были внесены следующие улучшения:

- В платформу шейдеров металлической производительности добавлено множество новых ядер, позволяющих приложению использовать преимущества высокооптимизированных вычислений с параллельным выполнением данных, таких как преобразование цветового пространства и операции нейронной сети.

<a name="ModelIO-Enhancements" />

## <a name="modelio-enhancements"></a>Усовершенствования Моделио

В Моделио Framework в tvOS 10 были внесены следующие улучшения:

- Теперь поддерживается формат файла USD.
- Используйте новый `MDLMaterialPropertyGraph` класс, чтобы легко поддерживать изменения среды выполнения в моделях.
- В класс [мдлвокселаррай](https://developer.apple.com/reference/modelio/mdlvoxelarray) добавлена поддержка поля со знакомого расстояния.
- Используйте новый `MDLLightProbeIrradianceDataSource` класс, чтобы упростить размещение зонда.

<a name="SceneKit-Enhancements" />

## <a name="scenekit-enhancements"></a>Усовершенствования SceneKit

В SceneKit Framework в tvOS 10 были внесены следующие улучшения:

- SceneKit теперь включает новую систему физической отрисовки (PBR) для более реалистичных результатов с более простым созданием ресурсов.
- Используйте новую модель заливки [скнлигхтингмоделфисикаллибасед](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) для произведения широкого спектра реалистичных эффектов заливки, при этом требуются только три фундаментальных `Metalness` свойства `Roughness`(`Diffuse`и).
- Поскольку заливка PBR лучше всего работает с освещением на основе среды, `LightingEnvironment` используйте свойство, чтобы назначить освещение на основе изображения для всей сцены Tan.
- `IESProfileURL` Используйте свойство для импорта реальных осветительных источников, определяющих основу освещения в реальных значениях, таких как интенсивность (в люменах) и цветовая температура (в градусах Кельвина).
- Класс [скнкамера](https://developer.apple.com/reference/scenekit/scncamera) обеспечивает более высокую реальную работу с помощью функций и эффектов HDR. Используйте адаптивную раскрытие, чтобы создать автоматические эффекты или использовать вигнеттинг, регулировку цвета и цветовую раскраску для добавления филматик эффектов в игру.
- Функции PBR и HDR-камеры предоставляют лучшие результаты по сравнению с традиционными методами отрисовки, и в результате SceneKit теперь выполняет все цветовые вычисления в линейном цветовом пространстве (при отображении цветовой гаммы P3 на устройствах с широким цветом).
- SceneKit теперь цвет соответствует всем цветам, считывая сведения о цветовых профилях.
- SceneKit интерпретирует значения цветовых компонентов в линейном цветовом пространстве RGB для всех типов шейдера.
- Так как SceneKit считывает и корректирует сведения о цветовых профилях в образах текстур, используйте каталоги активов для всех изображений, чтобы обеспечить их использование.
- Как линейное, так и расширенное отображение цветового пространства можно отключить, `SCNDisableLinearSpaceRendering` указав `SCNDisableWideGamut` `Info.plist`ключи и в приложении.
- Создайте произвольный многоугольник приматов (загружается из файлов или создается программно), чтобы указать геометрию с новым классом [скнжеометрипримитиветипеполигон](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) .

<a name="SpriteKit-Enhancements" />

## <a name="spritekit-enhancements"></a>Усовершенствования SpriteKit

В SpriteKit Framework в tvOS 10 были внесены следующие улучшения:

- Тилемапс теперь поддерживает фигуры мозаичных, шестиугольников и изометрических плиток для двумерных, 2,5 и прокрутых игр с `SKTileMapMode`помощью `SKTileGroup`классов `SKTileGroupRule` , `SKTileSet` и.
- Используйте новый `SKWarpGeometry` класс для растяжения или искажения отрисовки [скспритеноде](https://developer.apple.com/reference/spritekit/skspritenode) или [скеффектноде](https://developer.apple.com/reference/spritekit/skeffectnode) . Новый класс [скактион](https://developer.apple.com/reference/spritekit/skaction) можно использовать для анимации переходов между эффектами деформации.
- Пользовательские шейдеры могут предоставлять атрибуты (`SKAttribute`), которые можно настроить отдельно для каждого узла, использующего шейдер, путем предоставления значения атрибута (`SKAttributeValue`).
- Класс [сквиев](https://developer.apple.com/reference/spritekit/skview) предоставляет несколько новых методов, позволяющих точно контролировать время и способ отрисовки сцены.

<a name="UIKit-Enhancements" />

## <a name="uikit-enhancements"></a>Усовершенствования UIKit

В UIKit Framework в tvOS 10 были внесены следующие улучшения:

- API фокуса был усовершенствован для поддержки фокусировки элемента, не являющегося представлением, в дополнение `UIViews`к. Элементы, поддерживающие фокус, должны `IUIFocusItem` реализовывать интерфейс.
- Новый `UIGraphicsRender` класс предоставляет объектно-ориентированный метод создания точечных рисунков или документов PDF из UIKit отрисовки или основной графики и заменяет `UIGraphicsBeginImageContext` устаревший метод.
- Добавлен `UIUserInterfaceStyle` класс, чтобы определить, какая тема пользовательского интерфейса (темная или светлая) сейчас активна.
- Добавлена новая интерактивная поддержка однообъектной анимации на основе объектов, и Van должна быть связана с жестами. Обратитесь к разделу Справочник по [UIViewAnimating Protocol Reference](https://developer.apple.com/reference/uikit/uiviewanimating), [UIViewPropertyAnimator Class Reference](https://developer.apple.com/reference/uikit/uiviewpropertyanimator), [UITimingCurveProvider Protocol Reference](https://developer.apple.com/reference/uikit/uitimingcurveprovider), [UICubicTimingParameters Class Reference](https://developer.apple.com/reference/uikit/uicubictimingparameters) и [UISpringTimingParameter Class Reference](https://developer.apple.com/reference/uikit/uispringtimingparameters).
- Новый `UIPreviewInteraction` и`UIPreviewInteractionDelegate` позволяет приложению предоставлять пользовательский интерфейс для операций просмотра и POP.
- Новый `UIAccessibilityCustomRotor` класс позволяет приложению предоставлять настраиваемые, зависящие от контекста функции для вспомогательных технологий, таких как передача голоса.
- Используйте символы `UIAccessibilityAssistiveTouchStatusDidChangeNotification` и, чтобы определить, включен ли параметр AssistiveTouch. `UIAccessibilityIsAssistiveTouchRunning`
- Используйте символы `UIAccessibilityHearingDevicePairedEarDidChangeNotification` и, чтобы получить состояние любого парного MFiного средства для слуха. `UIAccessibilityHearingDevicePairedEar`
- Новый API [уипастебоард](https://developer.apple.com/reference/uikit/uipasteboard) предоставляет новые параметры (такие как ограничения времени существования) и автоматически объявляет совместимые типы содержимого для общих типов классов.
- Для поддержки динамического типа в метках текстовые поля и текстовые поля используют новый `PreferredFontForTextStyle` метод `UIFont` класса.
- Чтобы решить, должен ли элемент обновлять шрифт при изменении устройства `UIContentSizeCategory` , `AdjustsFontForContentSizeCategory` используйте свойство `UIContentSizeCategoryAdjusting` делегата.
- Теперь приложение может управлять внешним видом значка для элементов панели вкладок, таких как цвет текста и фона.
- Элемент управления обновлением теперь поддерживается во всех подклассах прокрутки и просмотра с прокруткой (например, `UICollectionView`).
- `OpenURL` Метод`UIApplication` класса вызывается асинхронно, теперь поддерживает обработчик завершения, который вызывается после завершения открытия.
- Инициируйте CloudKit общий доступ и измените его свойства с `UICloudSharingController` помощью `UICloudSharingControllerDelegate` новых классов и.
- Воспользуйтесь преимуществами предвыбранных ячеек, чтобы улучшить процесс `UICollectionViews` прокрутки с помощью нового `UICollectionViewDataSourcePrefetching` делегата.



## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [Новые возможности в tvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
