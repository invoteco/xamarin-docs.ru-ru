---
title: Работа с макетом watchOS в Xamarin
description: В этом документе описывается создание макета watchOS с помощью Xamarin. В нем обсуждаются контроллеры интерфейса, группы, разделители и элементы управления содержимым.
ms.prod: xamarin
ms.assetid: BEDB62A1-2249-4459-986F-413A41E63DF0
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 568d1e354d0ee840aeed980d6e8cc6b83068a1c8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73001546"
---
# <a name="working-with-watchos-layout-in-xamarin"></a>Работа с макетом watchOS в Xamarin

Разработка макетов для Apple Watch [размеров экрана](~/ios/watchos/app-fundamentals/screen-sizes.md) представляет собой уникальные проблемы.

## <a name="design-tips"></a>Советы по проектированию

Ключевой момент заключается в том, чтобы сделать пользовательский интерфейс доступным для чтения и работы на небольшом экране контрольных значений с большим пальцем. Не стоит приходиться на ловушки разработки для **симулятора iOS** (который выглядит очень большой) и **указателя мыши** (который работает с маленькими сенсорными целями)!

- Используйте черный фон. он создает иллюзию большего экрана с черной косметической панелью контрольных значений.

- Не замещайтесь вокруг макета экрана — декоративная панель образует естественное визуальное заполнение.

- Сосредоточьтесь на удобочитаемости. Внимательно используйте размеры и цвета шрифтов, чтобы обеспечить возможность чтения текста. Используйте встроенные стили текста, чтобы обеспечить автоматическую поддержку динамического типа.

![](layout-images/type.png "Example of Dynamic Type support")

- Сосредоточьтесь на размерах целевого объекта касания. Строки таблицы с кнопками и касаниями в виде текстовых меток должны охватывать весь экран. Apple говорит: «никогда не размещайте более трех элементов параллельно» и, если используются значки, а не текстовые метки.

- Используйте [элемент управления`Menu`](~/ios/watchos/user-interface/menu.md) , чтобы предоставить менее часто используемые функции для обеспечения четкого и краткого проектирования приложения.

## <a name="implementation"></a>Реализация

В комплект для просмотра входят следующие элементы управления, которые помогут вам создать привлекательные макеты приложений для просмотра.

### <a name="interface-controller"></a>Контроллер интерфейса

`WKInterfaceController` является базовым классом для всех сцен.

Область конструктора контроллера интерфейса ведет себя как вертикальная **Группа**: можно перетаскивать другие элементы управления на контроллер интерфейса, и они будут автоматически размещены друг над другом:

![](layout-images/controller-scene.png "Controls are automatically laid-out one above the other")

Вы можете задать свойства **расположения** и **размера** для каждого элемента управления, чтобы управлять их внешним видом:

![](layout-images/positionsize-attributes.png "Set the Position and Size properties on each control")

Если размер установлен **относительно контейнера** , можно указать пропорциональное значение и смещение. На этом снимке экрана показана кнопка, для которой настроено использование 80% ширины экрана (**0,8**):

![](layout-images/button-attributes.png "Provide a proportional value and an offset adjustment")

### <a name="group"></a>Группа

`WKInterfaceGroup` — это простой контейнер макета, который можно настроить для вертикального или горизонтального стека элементов управления. Он включает интервал между элементами управления по умолчанию, но можно изменить интервалы (и инсетс) в инспекторе **атрибутов** .

![](layout-images/group-attributes.png "Modify the spacing and insets in the Attributes inspector")

Группы могут сами изменять размер и положение относительно элементов управления, а группы могут быть вложенными для создания сложных макетов.

![](layout-images/group-scene.png "Groups can be nested to create complex layouts")

### <a name="separator"></a>Separator

Элемент управления Separator предназначен для обеспечения визуальных руководств в макете. Используйте разделители (или фоновые цвета или изображения), чтобы помочь пользователю понять, какое содержимое связано с экраном.

![](layout-images/separator-scene.png "Example of Separator usage")

Обратите внимание, что синие и зеленые разделители, которые не используют полную ширину экрана, настроены с **фиксированным** или **относительным размером контейнера** .

### <a name="content-controls"></a>Элементы управления содержимым

Макет не будет завершен без `Label`, `Image`, `Button`, `Switch`, `Slider`, `Map`и [других элементов управления](~/ios/watchos/user-interface/index.md).
Они могут размещаться в макетах с помощью **групп** или параметров размещения и размера для каждого элемента управления.

## <a name="related-links"></a>Связанные ссылки

- [Ватчкиткаталог (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Справочник по макету Apple](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Layout.html)
- [Справочник по цветовой & Apple](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/ColorandTypography.html)
