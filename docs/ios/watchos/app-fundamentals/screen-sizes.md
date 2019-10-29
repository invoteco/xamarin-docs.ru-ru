---
title: Работа с размерами экрана watchOS в Xamarin
description: В этом документе описывается работа с различными размерами экрана watchOS. В нем обсуждается конструктор интерфейсов watchOS, симулятор watchOS и графические ресурсы.
ms.prod: xamarin
ms.assetid: 840DF939-2F59-4ABA-87D8-92AAC8A92BC4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: aeaa1bb1273bc062e0ac76eaa09722827f15797f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028394"
---
# <a name="working-with-watchos-screen-sizes-in-xamarin"></a>Работа с размерами экрана watchOS в Xamarin

Apple Watch доступен на двух размерах экрана:

- **38**
  - 136 x 170 логических пикселов (272 x 340 пикселей)

- **Часы**
  - 156 x 195 логические Пиксели (312 x 390 пикселей).

При проектировании и тестировании приложений следует принять во внимание размер экрана.

## <a name="watchos-interface-designer"></a>Конструктор интерфейса watchOS

По умолчанию в конструкторе Visual Studio для Mac отображаются контроллеры интерфейса наблюдения в **любом Apple Watch**.

![](screen-sizes-images/screen-any-sml.png "The Designer displays watch interface controllers at Any Apple Watch")

Используйте меню Размер, чтобы изменить и просмотреть раскадровку на любом из доступных размеров экрана: **38** или **часы**:

![](screen-sizes-images/screen-menu-sml.png "Selecting the 38mm or 42mm size")

Увеличение размера экрана иногда приводит к отображению содержимого, которое было бы усечено или скрыто на уменьшенном экране.
Обязательно проверьте оба размера.

### <a name="interface-design"></a>Разработка интерфейса

Приложение должно отображать на экране одно и то же содержимое, независимо от размера, а также расширять или изменять элементы контракта соответствующим образом. В конструкторе Visual Studio для Mac в инспекторе атрибутов следует использовать параметр **относительно контейнера** или **размера, чтобы вместить содержимое в соответствии** с предпочтениями к фиксированным размерам.

![](screen-sizes-images/sizeattributepanel-sml.png "Use Relative to Container or Size to Fit Content in preference to fixed sizes")

Так как экран контрольных значений окружается черной косметической панелью, не рекомендуется использовать заполнение вокруг интерфейса. Разрешите элементам находиться на край экрана и разрешите декоративной панели форму естественной границы вокруг приложения.

## <a name="watchos-simulator"></a>Симулятор watchOS

При тестировании в симуляторе можно легко переключаться между двумя размерами экрана с помощью меню **оборудования > устройства** .

![](screen-sizes-images/simulator.png "The simulator can switch between the two screen sizes using the Hardware Device menu")

## <a name="image-resources"></a>Ресурсы изображений

Если один ресурс-контейнер не выглядит хорошо в разных размерах, следует использовать несколько ресурсов изображения. Каталоги ресурсов изображений позволяют указывать отдельные точечные рисунки для каждого размера:

![](screen-sizes-images/images-xcassets.png "Image asset catalog editor")

```csharp
// specify the asset name, the correct size will automatically be loaded
staticImage.SetImage(UIImage.FromBundle("Walkway"));
```

Кроме того, можно использовать код для определения размера экрана и загрузки различных изображений:

```csharp
bool large = WKInterfaceDevice.CurrentDevice.ScreenBounds.Size.Width > 136.0;
// Load image depending on screen size
using (var image = UIImage.FromBundle (large ? "42mm-Walkway" : "38mm-Walkway"))
{
   myImage.SetImage (image);

}
```

Узнайте больше об использовании [элемента управления Image](~/ios/watchos/user-interface/image.md).

## <a name="related-links"></a>Связанные ссылки

- [Введение в watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
