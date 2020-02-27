---
title: функции платформы iOS в Xamarin. Forms
description: Добавление специальных функций для iOS в приложения Xamarin. Forms.
ms.prod: xamarin
ms.assetid: 634AB62E-68C8-454C-838B-F1CC4E4E21BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/15/2020
ms.openlocfilehash: 015db40ce983d979109d4cce32c011f8c61a729c
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635665"
---
# <a name="ios-platform-features-in-xamarinforms"></a>функции платформы iOS в Xamarin. Forms

Для разработки приложений Xamarin. Forms для iOS требуется Visual Studio. На [странице Поддерживаемые платформы](~/get-started/supported-platforms.md) содержатся дополнительные сведения о предварительных требованиях.

## <a name="platform-specifics"></a>Особенности платформы

Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов.

Для представлений, страниц и макетов Xamarin. Forms в iOS доступны следующие функции для конкретной платформы:

- Поддержка размытия для любого [`VisualElement`](xref:Xamarin.Forms.VisualElement). Дополнительные сведения см. [в разделе Висуалелемент размытие в iOS](visualelement-blur.md).
- Отключение устаревшего цветового режима на поддерживаемом [`VisualElement`](xref:Xamarin.Forms.VisualElement). Дополнительные сведения см. [в разделе режим Висуалелемент прежних цветов в iOS](legacy-color-mode.md).
- Включение тени на [`VisualElement`](xref:Xamarin.Forms.VisualElement). Дополнительные сведения см. [в статье Висуалелемент Drop Shadows on IOS](visualelement-drop-shadow.md).
- Включение объекта [`VisualElement`](xref:Xamarin.Forms.VisualElement) в качестве первого респондента к событиям касания. Дополнительные сведения см. в статье [Висуалелемент First ответчика](visualelement-first-responder.md).

Для представлений Xamarin. Forms в iOS предусмотрены следующие функции, зависящие от платформы.

- Задание цвета фона [`Cell`](xref:Xamarin.Forms.Cell) . Дополнительные сведения см. [в разделе цвет фона ячейки в iOS](cell-background-color.md).
- Управление тем, когда в [`DatePicker`](xref:Xamarin.Forms.DatePicker)происходит выделение элементов. Дополнительные сведения см. [в разделе Выбор элементов DatePicker в iOS](datepicker-selection.md).
- Проверка того, что выводимый текст умещается в [`Entry`](xref:Xamarin.Forms.Entry) , путем настройки размера шрифта. Дополнительные сведения см. [в разделе Размер шрифта записи в iOS](entry-font-size.md).
- Установка цвета курсора в [`Entry`](xref:Xamarin.Forms.Entry). Дополнительные сведения см. [в разделе Ввод цвета курсора в iOS](entry-cursor-color.md).
- Управление тем, [`ListView`](xref:Xamarin.Forms.ListView) ли ячейки заголовка перемещаться во время прокрутки. Дополнительные сведения см. [в разделе стиль заголовка группы ListView в iOS](listview-group-header-style.md).
- Управление тем, будет ли отключена анимация строк при обновлении коллекции [`ListView`](xref:Xamarin.Forms.ListView) Items. Дополнительные сведения см. [в статье анимация строк ListView в iOS](listview-row-animations.md).
- Установка стиля разделителя на [`ListView`](xref:Xamarin.Forms.ListView). Дополнительные сведения см. [в разделе стиль разделителя ListView в iOS](listview-separator-style.md).
- Управление тем, когда в [`Picker`](xref:Xamarin.Forms.Picker)происходит выделение элементов. Дополнительные сведения см. [в разделе Выбор элементов средства выбора в iOS](picker-selection.md).
- Чтобы задать свойство [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) , коснитесь расположения на панели [`Slider`](xref:Xamarin.Forms.Slider) , а не перетащите `Slider` бегунок. Дополнительные сведения см. [в разделе ползунок бегунка в iOS](slider-thumb.md).
- Управление переходом, используемым при открытии `SwipeView`. Дополнительные сведения см. в разделе [Свипевиев считывание режима перехода](swipeview-swipetransitionmode.md).
- Управление тем, когда в [`TimePicker`](xref:Xamarin.Forms.TimePicker)происходит выделение элементов. Дополнительные сведения см. [в статье Выбор элемента TimePicker в iOS](timepicker-selection.md).

Для страниц Xamarin. Forms в iOS предусмотрены следующие функции, зависящие от платформы.

- Скрытие разделителя панели навигации на [`NavigationPage`](xref:Xamarin.Forms.NavigationPage). Дополнительные сведения см. [в разделе разделитель Навигатионпаже Bar в iOS](navigation-bar-separator.md).
- Управление, является ли прозрачные панели навигации. Дополнительные сведения см. [в разделе Панель навигации полупрозрачность в iOS](navigation-bar-translucent.md).
- Управление тем, что цвет текста строки состояния на [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) регулируется в соответствии с яркостью панели навигации. Дополнительные сведения см. [в статье режим цвета текста Навигатионпаже Bar в iOS](status-bar-text-color.md).
- Управление, отображается ли заголовок страницы как крупным заголовком на панели навигации страницы. Дополнительные сведения см. [в статье заголовки больших страниц в iOS](page-large-title.md).
- Установка видимости индикатора Home на [`Page`](xref:Xamarin.Forms.Page). Дополнительные сведения см. [в статье видимость индикатора в iOS](page-home-indicator.md).
- Установка видимости в строке состояния для [`Page`](xref:Xamarin.Forms.Page). Дополнительные сведения см. [в разделе Видимость строки состояния страницы в iOS](page-status-bar-visibility.md).
- Обеспечение содержимое этой страницы находится на области экрана, которое безопасно для всех устройств iOS. Дополнительные сведения см. [в разделе Путеводитель по макету защищенной области в iOS](page-safe-area-layout.md).
- Установка стиля представления для модальных страниц. Дополнительные сведения см. в разделе [стиль представления модальной страницы](page-presentation-style.md).
- Установка режима полупрозрачность для панели вкладок на [`TabbedPage`](xref:Xamarin.Forms.TabbedPage). Дополнительные сведения см. [в статье Таббедпаже таббар in iOS](tabbedpage-translucent-tabbar.md).

Для макетов Xamarin. Forms в iOS предусмотрены следующие функции, зависящие от платформы.

- Управление тем, обрабатывает ли [`ScrollView`](xref:Xamarin.Forms.ScrollView) жест касания или передает его содержимому. Дополнительные сведения см. [в разделе Скроллвиев Content соприкасается on IOS](scrollview-content-touches.md).

Для класса [`Application`](xref:Xamarin.Forms.Application) Xamarin. Forms в iOS предоставляются следующие зависящие от платформы функции.

- Отключение масштабирования специальных возможностей для именованных размеров шрифтов. Дополнительные сведения см. [в разделе масштабирование специальных возможностей для именованных размеров шрифтов в iOS](named-font-size-scaling.md).
- Включение структурой элементов управления и Подготовка к просмотру обновлений, выполняемых в основном потоке. Дополнительные сведения см. [в разделе Основные обновления управления потоком в iOS](main-thread-updates-ui.md).
- Включение [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) в представлении с прокруткой для записи и совместного использования жеста панорамирования с прокруткой. Дополнительные сведения см. [в разделе одновременный распознавание жестов в iOS](application-pan-gesture.md).

## <a name="ios-specific-formatting"></a>форматирование, относящееся к iOS

Xamarin. Forms позволяет устанавливать стили и цвета пользовательского интерфейса на разных платформах, но существуют и другие варианты настройки темы для iOS с помощью API платформы в проекте iOS.

[Узнайте больше](formatting.md) о форматировании пользовательского интерфейса с помощью интерфейсов API для iOS, таких как конфигурация **info. plist** и `UIAppearance` API.

![](images/status-white-sml.png "iOS Theming")

## <a name="other-ios-features"></a>Другие функции iOS

Используя [пользовательские модули подготовки](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)отчетов, [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)и [мессагингцентер](~/xamarin-forms/app-fundamentals/messaging-center.md), можно внедрить разнообразные собственные функции в приложения Xamarin. Forms для iOS.
