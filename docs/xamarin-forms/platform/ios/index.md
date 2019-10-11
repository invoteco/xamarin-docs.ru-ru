---
title: функции платформы iOS в Xamarin. Forms
description: Добавление специальных функций для iOS в приложения Xamarin. Forms.
ms.prod: xamarin
ms.assetid: 634AB62E-68C8-454C-838B-F1CC4E4E21BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/22/2019
ms.openlocfilehash: c90cfc297914b585403ae84e7dbac11fd6e02836
ms.sourcegitcommit: eb23b7d745d1090376f9def07e0f11cb089494d0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72170941"
---
# <a name="ios-platform-features-in-xamarinforms"></a>функции платформы iOS в Xamarin. Forms

Для разработки приложений Xamarin. Forms для iOS требуется Visual Studio. На [странице требования](~/get-started/requirements.md) содержатся дополнительные сведения о предварительных требованиях.

## <a name="platform-specifics"></a>Особенности платформы

Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов.

Для представлений, страниц и макетов Xamarin. Forms в iOS доступны следующие функции для конкретной платформы:

- Размытие поддержки для любого [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Дополнительные сведения см. [в разделе Висуалелемент размытие в iOS](visualelement-blur.md).
- Отключение режима устаревших цвет в поддерживаемой [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Дополнительные сведения см. [в разделе режим Висуалелемент прежних цветов в iOS](legacy-color-mode.md).
- Включение тень [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Дополнительные сведения см. [в статье Висуалелемент Drop Shadows on IOS](visualelement-drop-shadow.md).

Для представлений Xamarin. Forms в iOS предусмотрены следующие функции, зависящие от платформы.

- Установка цвета фона [`Cell`](xref:Xamarin.Forms.Cell) . Дополнительные сведения см. [в разделе цвет фона ячейки в iOS](cell-background-color.md).
- Обеспечение, введена текст встраивается в [ `Entry` ](xref:Xamarin.Forms.Entry) , корректируя размер шрифта. Дополнительные сведения см. [в разделе Размер шрифта записи в iOS](entry-font-size.md).
- Задание цвета курсора [ `Entry` ](xref:Xamarin.Forms.Entry). Дополнительные сведения см. [в разделе Ввод цвета курсора в iOS](entry-cursor-color.md).
- Управление перемещением ячеек заголовка [`ListView`](xref:Xamarin.Forms.ListView) во время прокрутки. Дополнительные сведения см. [в разделе стиль заголовка группы ListView в iOS](listview-group-header-style.md).
- Управление тем, будет ли отключена анимация строк при обновлении коллекции элементов [`ListView`](xref:Xamarin.Forms.ListView) . Дополнительные сведения см. [в статье анимация строк ListView в iOS](listview-row-animations.md).
- Установка стиль разделителя [ `ListView` ](xref:Xamarin.Forms.ListView). Дополнительные сведения см. [в разделе стиль разделителя ListView в iOS](listview-separator-style.md).
- Управление, когда происходит выбор элементов в [ `Picker` ](xref:Xamarin.Forms.Picker). Дополнительные сведения см. [в разделе Выбор элементов средства выбора в iOS](picker-selection.md).
- Включение [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) свойство для задания можно просмотреть, выбрав для положения на [ `Slider` ](xref:Xamarin.Forms.Slider) панели, а не по необходимости перетащите `Slider` бегунка. Дополнительные сведения см. [в разделе ползунок бегунка в iOS](slider-thumb.md).

Для страниц Xamarin. Forms в iOS предусмотрены следующие функции, зависящие от платформы.

- Скрытие разделителя панели навигации на [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). Дополнительные сведения см. [в разделе разделитель Навигатионпаже Bar в iOS](navigation-bar-separator.md).
- Управление, является ли прозрачные панели навигации. Дополнительные сведения см. [в разделе Панель навигации полупрозрачность в iOS](navigation-bar-translucent.md).
- Управление цветом ли текст строки состояния на [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) настраивается в соответствии с яркость панели навигации. Дополнительные сведения см. [в статье режим цвета текста Навигатионпаже Bar в iOS](status-bar-text-color.md).
- Управление, отображается ли заголовок страницы как крупным заголовком на панели навигации страницы. Дополнительные сведения см. [в статье заголовки больших страниц в iOS](page-large-title.md).
- Установка видимости индикатора Home на [`Page`](xref:Xamarin.Forms.Page). Дополнительные сведения см. [в статье видимость индикатора в iOS](page-home-indicator.md).
- Установка видимость полосы состояния [ `Page` ](xref:Xamarin.Forms.Page). Дополнительные сведения см. [в разделе Видимость строки состояния страницы в iOS](page-status-bar-visibility.md).
- Обеспечение содержимое этой страницы находится на области экрана, которое безопасно для всех устройств iOS. Дополнительные сведения см. [в разделе Путеводитель по макету защищенной области в iOS](page-safe-area-layout.md).
- Установка стиля представления для модальных страниц. Дополнительные сведения см. в разделе [стиль представления модальной страницы](page-presentation-style.md).

Для макетов Xamarin. Forms в iOS предусмотрены следующие функции, зависящие от платформы.

- Управление ли [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) жест касания обрабатывает или передает его на его содержимое. Дополнительные сведения см. [в разделе Скроллвиев Content соприкасается on IOS](scrollview-content-touches.md).

Для работы с классом Xamarin. Forms [`Application`](xref:Xamarin.Forms.Application) в iOS предусмотрены следующие специальные функции платформы:

- Отключение масштабирования специальных возможностей для именованных размеров шрифтов. Дополнительные сведения см. [в разделе масштабирование специальных возможностей для именованных размеров шрифтов в iOS](named-font-size-scaling.md).
- Включение структурой элементов управления и Подготовка к просмотру обновлений, выполняемых в основном потоке. Дополнительные сведения см. [в разделе Основные обновления управления потоком в iOS](main-thread-updates-ui.md).
- Включение [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) в области прокрутки для записи и совместно использовать жест pan с представления с прокруткой. Дополнительные сведения см. [в разделе одновременный распознавание жестов в iOS](application-pan-gesture.md).

## <a name="ios-specific-formatting"></a>форматирование, относящееся к iOS

Xamarin. Forms позволяет устанавливать стили и цвета пользовательского интерфейса на разных платформах, но существуют и другие варианты настройки темы для iOS с помощью API платформы в проекте iOS.

[Узнайте больше](formatting.md) о форматировании пользовательского интерфейса с помощью интерфейсов API для iOS, таких как конфигурация **info. plist** и API `UIAppearance`.

![](images/status-white-sml.png "данные для iOS")

## <a name="other-ios-features"></a>Другие функции iOS

Используя [пользовательские модули подготовки](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)отчетов, [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)и [мессагингцентер](~/xamarin-forms/app-fundamentals/messaging-center.md), можно внедрить разнообразные собственные функции в приложения Xamarin. Forms для iOS.
