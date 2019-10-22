---
title: Возможности на платформе Android
description: В этой статье объясняется, как добавить в приложения Xamarin. Forms специальные функции для Android.
ms.prod: xamarin
ms.assetid: E24168F3-0138-4814-86EA-B467F6B8A545
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/24/2019
ms.openlocfilehash: 73e838b3a63132230cf594a3461c9d7ee6f302b8
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696952"
---
# <a name="android-platform-features"></a>Возможности на платформе Android

Для разработки приложений Xamarin. Forms для Android требуется Visual Studio. На [странице требования](~/get-started/requirements.md) содержатся дополнительные сведения о предварительных требованиях.

## <a name="platform-specifics"></a>Особенности платформы

Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов.

Для представлений, страниц и макетов Xamarin. Forms в Android доступны следующие функции для конкретной платформы:

- Управление Z-порядком визуальных элементов для определения порядка рисования. Дополнительные сведения см. [в статье повышение прав висуалелемент на Android](visualelement-elevation.md).
- Отключение устаревшего цветового режима на поддерживаемом [`VisualElement`](xref:Xamarin.Forms.VisualElement). Дополнительные сведения см. [в разделе режим Висуалелемент прежних цветов в Android](legacy-color-mode.md).

Для представлений Xamarin. Forms на Android доступны следующие функции конкретной платформы:

- Использование заполнения по умолчанию и значений тени для кнопок Android. Дополнительные сведения см. [в разделе Заполнение и тени кнопок на устройствах Android](button-padding-shadow.md).
- Настройка параметров редактора метода ввода для [`Entry`](xref:Xamarin.Forms.Entry). Дополнительные сведения см. [в разделе Параметры редактора метода ввода для Android](entry-ime-options.md).
- Включение тени на `ImageButton`. Дополнительные сведения см. [в статье Drop Shadows on Android](imagebutton-drop-shadow.md).
- Включение быстрой прокрутки в [`ListView`](xref:Xamarin.Forms.ListView) дополнительные сведения см. в статье [Быстрая прокрутка ListView в Android](listview-fast-scrolling.md).
- Управление тем, может ли [`WebView`](xref:Xamarin.Forms.WebView) отображать смешанное содержимое. Дополнительные сведения см. [в статье WebView Mixed Content in Android](webview-mixed-content.md).
- Включение масштабирования [`WebView`](xref:Xamarin.Forms.WebView). Дополнительные сведения см. [в разделе WebView Zoom on Android](webview-zoom-controls.md).

Для ячеек Xamarin. Forms в Android предусмотрены следующие функции для конкретных платформ:

- Включение [`ViewCell`ных](xref:Xamarin.Forms.ViewCell) действий контекста в устаревшем режиме, чтобы меню контекстных действий не обновлялось при изменении выбранного элемента в [`ListView`](xref:Xamarin.Forms.ListView) . Дополнительные сведения см. [в статье ViewCell context Actions on Android](viewcell-context-actions.md).

Для страниц Xamarin. Forms в Android предусмотрены следующие функции для конкретных платформ:

- Задание высоты панели навигации на [`NavigationPage`](xref:Xamarin.Forms.NavigationPage). Дополнительные сведения см. [в разделе высота Навигатионпаже Bar на устройстве Android](navigationpage-bar-height.md).
- Отключение анимации переходов при переходе по страницам в [`TabbedPage`](xref:Xamarin.Forms.TabbedPage). Дополнительные сведения см. [в разделе Анимация перехода на страницу таббедпаже в Android](tabbedpage-transition-animations.md).
- Включение прокрутки между страницами в [`TabbedPage`](xref:Xamarin.Forms.TabbedPage). Дополнительные сведения см. [в разделе Таббедпаже Page прокрутка на Android](tabbedpage-page-swiping.md).
- Задание расположения и цвета панели инструментов на [`TabbedPage`](xref:Xamarin.Forms.TabbedPage). Дополнительные сведения см. [в статье размещение и цвет панели инструментов таббедпаже в Android](tabbedpage-toolbar-placement-color.md).

Для класса [`Application`](xref:Xamarin.Forms.Application) Xamarin. Forms в Android предусмотрены следующие специальные функции платформы:

- Установка режима работы экранной клавиатуры. Дополнительные сведения см. [в разделе режим ввода с клавиатуры](soft-keyboard-input-mode.md).
- Отключение [`Disappearing`](xref:Xamarin.Forms.Page.Appearing) и [`Appearing`](xref:Xamarin.Forms.Page.Appearing) событий жизненного цикла страницы при приостановке и возобновлении соответственно для приложений, использующих AppCompat. Дополнительные сведения см. [в разделе события жизненного цикла страницы в Android](page-lifecycle-events.md).

## <a name="platform-support"></a>Поддержка платформ

Изначально проект Xamarin. Forms для Android по умолчанию использовал более старый стиль визуализации элементов управления, который был распространен до Android 5,0. Приложения, созданные с помощью шаблона, имеют `FormsApplicationActivity` в качестве базового класса их основного действия.

## <a name="material-design-via-appcompat"></a>Проектирование материалов с помощью AppCompat

Проекты Android в Xamarin. Forms теперь используют `FormsAppCompatActivity` в качестве базового класса их основного действия. Этот класс использует функции **AppCompat** , предоставляемые Android, для реализации тем дизайна материалов.

Чтобы добавить темы дизайна материалов в проект Xamarin. Forms для Android, следуйте [инструкциям по установке для поддержки AppCompat](appcompat-material-design.md) .

Ниже приведен пример **TODO** с `FormsApplicationActivity` по умолчанию:

[![](images/before-appcompat-sml.png "Todo Sample Application Without AppCompat")](images/before-appcompat.png#lightbox "Todo Sample Application Without AppCompat")

И это тот же код после обновления проекта для использования `FormsAppCompatActivity` (и добавления дополнительных сведений о теме):

[![](images/post-appcompat-sml.png "Todo Sample Application With AppCompat and Theming")](images/post-appcompat.png#lightbox "Todo Sample Application With AppCompat and Theming")

> [!NOTE]
> При использовании `FormsAppCompatActivity` [базовые классы для некоторых настраиваемых модулей подготовки Android](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md) будут отличаться.

## <a name="related-links"></a>Связанные ссылки

- [Добавление поддержки дизайна материалов](appcompat-material-design.md)
