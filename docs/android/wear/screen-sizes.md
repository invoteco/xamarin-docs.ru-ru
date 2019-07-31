---
title: Работа с размером экрана в Xamarin. Android и в ОС с износом
ms.prod: xamarin
ms.assetid: 77831169-C663-4D42-B742-B8B556B1DA4B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 93e6797f2b00df32b8d3ae361f40fd487b7adac3
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68647719"
---
# <a name="working-with-screen-sizes"></a>Работа с размерами экрана

Устройства "износ Android" могут иметь прямоугольный или круглый экран, который также может иметь разные размеры.

![Снимки экрана с прямоугольными и круговыми выизносами](screen-sizes-images/moyeu-wear.png)

## <a name="identifying-screen-type"></a>Определение типа экрана

Библиотека "износ" предоставляет некоторые элементы управления, помогающие обнаруживать и адаптировать различные экранные формы, такие `WatchViewStub` как `BoxInsetLayout`и.

Имейте в виду, что некоторые из других элементов управления библиотеки поддержки ( `GridViewPager`например,) *автоматически* обнаруживают экранную форму и не должны добавляться как дочерние элементы управления, описанные ниже.

### <a name="watchviewstub"></a>ватчвиевстуб

См. пример [ватчвиевстуб](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-watchviewstub) , чтобы узнать, как определить тип экрана и отобразить различные макеты для каждого типа.

Основной файл макета содержит, `android.support.wearable.view.WatchViewStub` который ссылается на различные макеты для прямоугольных и круглых экранов `app:rectLayout` с использованием атрибутов и `app:roundLayout` .

```xml
<android.support.wearable.view.WatchViewStub
    xmlns:app="http://schemas.android.com/apk/res-auto"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:id="@+id/stub"
  app:rectLayout="@layout/rect_layout"
  app:roundLayout="@layout/round_layout" />
```

Решение содержит различные макеты для каждого стиля, который будет выбран во время выполнения:

![Файлы, отображаемые в разделе "ресурсы и макет"](screen-sizes-images/solution.png)


### <a name="boxinsetlayout"></a>боксинсетлайаут

Вместо создания различных макетов для каждого типа экрана можно также создать единое представление, которое адаптируется к прямоугольным или круглым экранам.

В этом [примере Google](https://developer.android.com/training/wearables/ui/layouts.html#same-layout) показано, как использовать `BoxInsetLayout` для использования одного и того же макета на прямоугольных и круглых экранах.


## <a name="wear-ui-designer"></a>Конструктор неизносного пользовательского интерфейса

Xamarin Android Designer поддерживает как прямоугольные, так и скругленные экраны:

![Выбор квадрата износа Android в Xamarin Android Designer](screen-sizes-images/design-screen-type.png)

Область конструктора в прямоугольной области показана ниже:

![Область конструктора в прямоугольном стиле](screen-sizes-images/design-rect.png) 

Область конструктора в круглом стиле показана здесь:

![Область конструктора в круглом стиле](screen-sizes-images/design-round.png)


## <a name="wear-simulator"></a>Износ симулятора

**Диспетчер эмуляторов Google** содержит определения устройств для обоих типов экрана. Для тестирования приложения можно создать прямоугольные и скругленные Эмуляторы.

![Определения износа устройств, показанные в диспетчере эмуляторов Google](screen-sizes-images/emulator-devices.png)

Эмулятор будет отображаться для прямоугольного экрана следующим образом:

![Визуализация прямоугольного экрана эмулятором](screen-sizes-images/recipe-2.png) 

Он будет отображаться следующим образом для круглых экранов:

![Визуализация на круглом экране эмулятора](screen-sizes-images/recipe-2-round.png)

## <a name="video"></a>Видео

[Полноэкранные приложения для Android](https://www.youtube.com/watch?v=naf_WbtFAlY) — это износ из [Developers.Google.com](https://www.youtube.com/channel/UC_x5XG1OV2P6uZZ5FSM9Ttw).

