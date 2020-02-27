---
title: Возможности платформы Windows
description: В этой статье описывается поддержка платформы Windows, доступная в Xamarin. Forms.
ms.prod: xamarin
ms.assetid: F6EA9E49-FB3E-442F-AF13-B7AD0C80D11F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/16/2020
ms.openlocfilehash: 694ec24697937b114036eceab1cafd5aae3617d8
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635553"
---
# <a name="windows-platform-features"></a>Возможности платформы Windows

Для разработки приложений Xamarin. Forms для платформ Windows требуется Visual Studio. На [странице Поддерживаемые платформы](~/get-started/supported-platforms.md) содержатся дополнительные сведения о предварительных требованиях.

![](images/allhanselman.png "Xamarin.Forms Applications Running on Windows")

## <a name="platform-specifics"></a>Особенности платформы

Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов.

Для представлений, страниц и макетов Xamarin. Forms на универсальная платформа Windows (UWP) предоставляются следующие специальные функции платформы.

- Установка ключа доступа для [`VisualElement`](xref:Xamarin.Forms.VisualElement). Дополнительные сведения см. [в разделе ключи доступа висуалелемент в Windows](visualelement-access-keys.md).
- Отключение устаревшего цветового режима на поддерживаемом [`VisualElement`](xref:Xamarin.Forms.VisualElement). Дополнительные сведения см. [в разделе режим Висуалелемент прежних цветов в Windows](legacy-color-mode.md).

Для представлений Xamarin. Forms в UWP предусмотрены следующие функции для конкретных платформ:

- Обнаружение порядка чтения из текстового содержимого в [`Entry`](xref:Xamarin.Forms.Entry), [`Editor`](xref:Xamarin.Forms.Editor)и экземплярах [`Label`](xref:Xamarin.Forms.Label) . Дополнительные сведения см. [в разделе Инпутвиев Read Order on Windows](inputview-reading-order.md).
- Включение поддержки жестов касания в [`ListView`](xref:Xamarin.Forms.ListView). Дополнительные сведения см. [в разделе ListView SelectionMode в Windows](listview-selectionmode.md).
- Включение изменения направления извлечения `RefreshView`. Дополнительные сведения см. [в статье направление извлечения рефрешвиев в Windows](refreshview-pulldirection.md).
- Включение [`SearchBar`](xref:Xamarin.Forms.SearchBar) для взаимодействия с модулем проверки орфографии. Дополнительные сведения см. [в разделе сеарчбар проверка орфографии on Windows](searchbar-spell-check.md).
- Включение [`WebView`](xref:Xamarin.Forms.WebView) для вывода оповещений JavaScript в диалоговом окне сообщения UWP. Дополнительные сведения см. [в статье WebView JavaScript Alerts on Windows](webview-javascript-alert.md).

Для страниц Xamarin. Forms в UWP предусмотрены следующие функции для конкретной платформы:

- Свертывание панели навигации [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) . Дополнительные сведения см. [в разделе Мастердетаилпаже навигационная Bar on Windows](masterdetailpage-navigation-bar.md).
- Настройка параметров размещения панели инструментов. Дополнительные сведения см. [в разделе расположение панели инструментов страницы в Windows](page-toolbar-placement.md).
- Включение отображения значков страниц на панели инструментов [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . Дополнительные сведения см. в статье о [значках TabbedPage в Windows](tabbedpage-icons.md).

Для класса [`Application`](xref:Xamarin.Forms.Application) Xamarin. Forms в UWP предусмотрены следующие специальные функции платформы:

- Указание каталога в проекте, из которого будут загружаться ресурсы изображений. Дополнительные сведения см. [в разделе Каталог образов по умолчанию в Windows](default-image-directory.md).

## <a name="platform-support"></a>Поддержка платформы

Шаблоны Xamarin. Forms, доступные в Visual Studio, содержат проект универсальная платформа Windows (UWP).

> [!NOTE]
> Xamarin. Forms 1. x и 2. x поддерживают _Windows Phone 8 Silverlight_, _Windows Phone 8,1_и _Windows 8.1_ разработки приложений. Однако эти типы проектов являются устаревшими.

## <a name="getting-started"></a>Начало работы

Перейдите в **файл > новый проект >** в Visual Studio и выберите один из шаблонов **между платформами > пустое приложение (Xamarin. Forms)** , чтобы приступить к работе.

Старые решения Xamarin. Forms, созданные на macOS, не будут содержать все перечисленные выше проекты Windows (но их необходимо добавить вручную). Если нужная платформа Windows еще не находится в решении, ознакомьтесь с [инструкциями по установке](installation/index.md) , чтобы добавить нужный тип проекта Windows/с.

## <a name="samples"></a>Примеры

[Все примеры](https://github.com/xamarin/xamarin-forms-book-preview-2) для книги Чарльз Петцольд, [*создающей мобильные приложения с помощью Xamarin. Forms, включают в*](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) себя универсальная платформа Windows проектов (для Windows 10).

[Демонстрационное приложение "Скотт Hanselman"](https://github.com/jamesmontemagno/Hanselman.Forms) доступно отдельно, а также включает проекты Apple Watch и Android (с использованием Xamarin. iOS и Xamarin. Android соответственно, Xamarin. Forms не выполняется на этих платформах).

## <a name="related-links"></a>Связанные ссылки

- [Настройка проектов Windows](~/xamarin-forms/platform/windows/installation/index.md)
