---
title: Представления Xamarin.Forms
description: Представления Xamarin.Forms являются стандартными блоками кроссплатформенных мобильных пользовательских интерфейсов. В этой статье перечислены представления, которые включены в Xamarin.Forms.
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/14/2020
ms.openlocfilehash: 09bcb49db7f257a415518b259672ca8e776cdbc4
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/07/2020
ms.locfileid: "78916866"
---
# <a name="xamarinforms-views"></a>Представления Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Представления Xamarin. Forms — это стандартные блоки межплатформенных мобильных пользовательских интерфейсов._

Представления — это объекты пользовательского интерфейса, такие как метки, кнопки и ползунки, которые обычно называются *элементами управления* или *мини* -приложениями в других графических средах программирования. Представления, поддерживаемые Xamarin. Forms, являются производными от класса [`View`](xref:Xamarin.Forms.View) . Их можно разделить на несколько категорий:

## <a name="views-for-presentation"></a>Визуальные элементы для представления данных

### <a name="label"></a>Метка

|     |     |
| --- | --- |
| [`Label`](xref:Xamarin.Forms.Label) отображает однострочные текстовые строки или многострочные блоки текста с постоянным форматированием или переменной. Задайте для свойства [`Text`](xref:Xamarin.Forms.Label.Text) строку для форматирования константы или задайте для свойства [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) значение [`FormattedString`](xref:Xamarin.Forms.FormattedString) объекта для форматирования переменной.<br /><br />[Документация по API](xref:Xamarin.Forms.Label) / [руководство](~/xamarin-forms/user-interface/text/label.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![Пример метки](views-images/Label.png "Пример метки")](views-images/Label-Large.png#lightbox "Пример метки")<br /> [Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) |
|     |     |

### <a name="image"></a>Образ —

|     |     |
| --- | --- |
| [`Image`](xref:Xamarin.Forms.Image) отображает точечный рисунок. Точечные рисунки можно загружать через Интернет, внедрять как ресурсы в проекты общих проектов или платформ или создавать с помощью объекта `Stream` .NET.<br /><br />[Документация по API](xref:Xamarin.Forms.Image) / [руководство](~/xamarin-forms/user-interface/images.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages) | [![Пример изображения](views-images/Image.png "Пример изображения")](views-images/Image-Large.png#lightbox "Пример изображения")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) |
|     |     |

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](xref:Xamarin.Forms.BoxView) отображается сплошной прямоугольник, окрашенный в свойство [`Color`](xref:Xamarin.Forms.BoxView.Color) . `BoxView` имеет запрос размера по умолчанию для 40x40. Для других размеров назначьте свойства [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) и [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) .<br /><br />[Документация по API](xref:Xamarin.Forms.BoxView) / [руководство](~/xamarin-forms/user-interface/boxview.md) / [Пример 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview), [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-textdecoration), [3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-listviewcolors/), [4](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife), [5](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-dotmatrixclock)и [6](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock) | [![Пример Боксвиев](views-images/BoxView.png "Пример Боксвиев")](views-images/BoxView-Large.png#lightbox "Пример Боксвиев")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) |
|     |     |

### <a name="webview"></a>Веб-представление

|     |     |
| --- | --- |
| [`WebView`](xref:Xamarin.Forms.WebView) отображает веб-страницы или содержимое HTML в зависимости от того, имеет ли свойство [`Source`](xref:Xamarin.Forms.WebView.Source) значение [`UriWebViewSource`](xref:Xamarin.Forms.UrlWebViewSource) или объект [`HtmlWebViewSource`](xref:Xamarin.Forms.HtmlWebViewSource) .<br /><br />[Документация по API](xref:Xamarin.Forms.WebView) / [руководство](~/xamarin-forms/user-interface/webview.md) / [Пример 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview) и [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview) | [![Пример WebView](views-images/WebView.png "Пример WebView")](views-images/WebView-Large.png#lightbox "Пример WebView")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](xref:Xamarin.Forms.OpenGLView) отображает графические изображения OpenGL в проектах iOS и Android. Не поддерживается для универсальной платформы Windows. Для проектов iOS и Android требуется ссылка на сборку **опентк-1,0** или сборку **опентк** версии 1.0.0.0. `OpenGLView` проще в использовании в общем проекте; Если используется в библиотеке .NET Standard, потребуется также служба зависимостей (как показано в примере кода).<br /><br />Это единственный графический механизм, встроенный в Xamarin. Forms, но приложение Xamarin. Forms может также визуализировать графические объекты с помощью [`SkiaSharp`](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)или [`UrhoSharp`](~/xamarin-forms/user-interface/graphics/urhosharp.md).<br /><br />[Документация по API](xref:Xamarin.Forms.OpenGLView)<br /><br /> | [![Пример Опенглвиев](views-images/OpenGLView.png "Пример Опенглвиев")](views-images/OpenGLView-Large.png#lightbox "Пример Опенглвиев")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) |
|     |     |

### <a name="map"></a>Схема

|     |     |
| --- | --- |
| [`Map`](xref:Xamarin.Forms.Maps.Map) отображает карту. Необходимо установить пакет NuGet для **Xamarin. Forms. Maps** . Android и универсальной платформы Windows требуется ключ авторизации карты.<br /><br />[Документация по API](xref:Xamarin.Forms.Maps.Map) / [руководство](~/xamarin-forms/user-interface/map/index.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps/) | [![Пример Map](views-images/Map.png "Пример Map")](views-images/Map-Large.png#lightbox "Пример Map")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) |
|     |     |

### <a name="mediaelement"></a>MediaElement

|     |     |
| --- | --- |
| [`MediaElement`](xref:Xamarin.Forms.MediaElement) воспроизводит видео или аудио. Мультимедиа можно воспроизводить с URL-адреса или из локального файла в зависимости от того, задано ли для свойства [`Source`](xref:Xamarin.Forms.MediaElement.Source) [`UriMediaSource`](xref:Xamarin.Forms.UriMediaSource) или [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource).<br /><br />[Документация по API](xref:Xamarin.Forms.MediaElement) / [руководство](~/xamarin-forms/user-interface/mediaelement.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos) | [![Пример MediaElement](views-images/MediaElement.png "Пример MediaElement")](views-images/MediaElement-Large.png#lightbox "Пример MediaElement")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MediaElementDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MediaElementDemoPage.xaml) |
|     |     |

## <a name="views-that-initiate-commands"></a>Визуальные элементы, инициирующие команды

### <a name="button"></a>Кнопка

|     |     |
| --- | --- |
| [`Button`](xref:Xamarin.Forms.Button) — прямоугольный объект, отображающий текст, который запускает событие [`Clicked`](xref:Xamarin.Forms.Button.Clicked) при нажатии.<br /><br />[Документация по API](xref:Xamarin.Forms.Button) / [руководство](~/xamarin-forms/user-interface/button.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos/) | [![Пример кнопки](views-images/Button.png "Пример кнопки")](views-images/Button-Large.png#lightbox "Пример кнопки")<br /> [Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) |
|     |     |

### <a name="imagebutton"></a>ImageButton

|     |     |
| --- | --- |
| `ImageButton` — прямоугольный объект, отображающий изображение, который запускает событие `Clicked` при нажатии.<br /><br /> [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) / [Guide](~/xamarin-forms/user-interface/imagebutton.md) | [![Пример с ImageButton](views-images/ImageButton.png "Пример с ImageButton")](views-images/ImageButton-Large.png#lightbox "Пример с ImageButton")<br /> [Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageButtonDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml.cs) |
|     |     |

### <a name="refreshview"></a>RefreshView

|     |     |
| --- | --- |
| `RefreshView` — это контейнерный элемент управления, который предоставляет функции получения обновлений для прокручиваемого содержимого. `ICommand`, определяемые свойством `Command`, выполняется при активации обновления, а свойство `IsRefreshing` указывает текущее состояние элемента управления.<br /><br /> [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) / [Guide](~/xamarin-forms/user-interface/refreshview.md) | [![Пример Рефрешвиев](views-images/RefreshView.png "Пример Рефрешвиев")](views-images/RefreshView-Large.png#lightbox "Пример Рефрешвиев")<br /> [Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RefreshViewDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RefreshViewDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RefreshViewDemoPage.xaml.cs) |
|     |     |

### <a name="searchbar"></a>Панель поиска

|     |     |
| --- | --- |
| [`SearchBar`](xref:Xamarin.Forms.SearchBar) отображает область, в которой пользователь должен ввести текстовую строку, и кнопку (или клавишу с клавиатуры), которая сигнализирует приложению выполнить поиск. Свойство [`Text`](xref:Xamarin.Forms.InputView.Text) предоставляет доступ к тексту, а событие [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed) указывает на нажатие кнопки.<br /><br />[Документация по API](xref:Xamarin.Forms.SearchBar) / [руководство](~/xamarin-forms/user-interface/searchbar.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/) | [![Пример Сеарчбар](views-images/SearchBar.png "Пример Сеарчбар")](views-images/SearchBar-Large.png#lightbox "Пример Сеарчбар")<br /> [Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) |
|     |     |

### <a name="swipeview"></a>SwipeView

|     |     |
| --- | --- |
| `SwipeView` — это контейнерный элемент управления, который обходит элемент содержимого и предоставляет элементы контекстного меню, которые выводятся с помощью жеста прокрутки. Каждый элемент меню представлен `SwipeItem`, у которого есть свойство `Command`, которое выполняет `ICommand` при касании элемента.<br /><br /> [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) / [Guide](~/xamarin-forms/user-interface/swipeview.md) | [![Пример Свипевиев](views-images/SwipeView.png "Пример Свипевиев")](views-images/SwipeView-Large.png#lightbox "Пример Свипевиев")<br /> [Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwipeViewDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwipeViewDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwipeViewDemoPage.xaml.cs) |
|     |     |

## <a name="views-for-setting-values"></a>Визуальные элементы для установки значений

### <a name="checkbox"></a>CheckBox

|     |     |
| --- | --- |
| `CheckBox` позволяет пользователю выбрать логическое значение с помощью типа кнопки, которая может быть либо установлена, либо пустой. Свойство `IsChecked` — это состояние `CheckBox`, а событие `CheckedChanged` срабатывает при изменении состояния.<br /><br />Документация или [руководство](~/xamarin-forms/user-interface/checkbox.md) по API / [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos) | [![Пример флажка](views-images/CheckBox.png "Пример флажка")](views-images/CheckBox-Large.png#lightbox "Пример флажка")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CheckBoxPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CheckBoxPage.xaml) |
|     |     |

### <a name="slider"></a>Ползунок

|     |     |
| --- | --- |
| [`Slider`](xref:Xamarin.Forms.Slider) позволяет пользователю выбрать `double` значение из непрерывного диапазона, указанного с помощью свойств [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) и [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) .<br /><br />[Документация по API](xref:Xamarin.Forms.Slider) / [руководство](~/xamarin-forms/user-interface/slider.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) | [![Пример ползунка](views-images/Slider.png "Пример ползунка")](views-images/Slider-Large.png#lightbox "Пример ползунка")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) |
|     |     |

### <a name="stepper"></a>Шаговый переключатель

|     |     |
| --- | --- |
| [`Stepper`](xref:Xamarin.Forms.Stepper) позволяет пользователю выбрать `double` значение из диапазона добавочных значений, указанных в свойствах [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum), [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum)и [`Increment`](xref:Xamarin.Forms.Stepper.Increment) .<br /><br />[Документация по API](xref:Xamarin.Forms.Stepper)  / [руководство](~/xamarin-forms/user-interface/stepper.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos) | [![Пример многошагового режима](views-images/Stepper.png "Пример многошагового режима")](views-images/Stepper-Large.png#lightbox "Пример многошагового режима")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) |
|     |     |

### <a name="switch"></a>Параметр

|     |     |
| --- | --- |
| [`Switch`](xref:Xamarin.Forms.Switch) принимает форму переключателя вкл./откл. Это позволяет пользователю выбрать логическое значение. Свойство [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) — это состояние переключателя, а событие [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) возникает при изменении состояния.<br /><br />[Документация по API](xref:Xamarin.Forms.Switch) / [руководство](~/xamarin-forms/user-interface/switch.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/) | [![Пример параметра](views-images/Switch.png "Пример параметра")](views-images/Switch-Large.png#lightbox "Пример параметра")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](xref:Xamarin.Forms.DatePicker) позволяет пользователю выбрать дату с помощью средства выбора даты платформы. Задайте диапазон допустимых дат с помощью свойств [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) и [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) . Свойство [`Date`](xref:Xamarin.Forms.DatePicker.Date) является выбранной датой, а событие [`DateSelected`](xref:Xamarin.Forms.DatePicker.DateSelected) возникает при изменении этого свойства.<br /><br />[Документация по API](xref:Xamarin.Forms.DatePicker) / [руководство](~/xamarin-forms/user-interface/datepicker.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker) | [![Пример DatePicker](views-images/DatePicker.png "Пример DatePicker")](views-images/DatePicker-Large.png#lightbox "Пример DatePicker")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](xref:Xamarin.Forms.TimePicker) позволяет пользователю выбрать время с помощью средства выбора времени платформы. Свойство [`Time`](xref:Xamarin.Forms.TimePicker.Time) является выбранным временем. Приложение может отслеживать изменения в свойстве `Time`, устанавливая обработчик для события [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) .<br /><br />[Документация по API](xref:Xamarin.Forms.TimePicker) / [руководство](~/xamarin-forms/user-interface/timepicker.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker) | [![Пример TimePicker](views-images/TimePicker.png "Пример TimePicker")](views-images/TimePicker-Large.png#lightbox "Пример TimePicker")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) |
|     |     |

## <a name="views-for-editing-text"></a>Визуальные элементы для редактирования текста

Эти два класса являются производными от класса [`InputView`](xref:Xamarin.Forms.InputView) , который определяет свойство [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) .

### <a name="entry"></a>Запись

|     |     |
| --- | --- |
| [`Entry`](xref:Xamarin.Forms.Entry) позволяет пользователю вводить и редактировать одну строку текста. Текст доступен как свойство [`Text`](xref:Xamarin.Forms.InputView.Text) , а события [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) и [`Completed`](xref:Xamarin.Forms.Entry.Completed) запускаются при изменении текста или при завершении пользователем путем нажатия клавиши ВВОД.<br /><br />Используйте [`Editor`](#editor) для ввода и редактирования нескольких строк текста.<br /><br />[Документация по API](xref:Xamarin.Forms.Entry) / [руководство](~/xamarin-forms/user-interface/text/entry.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![Пример записи](views-images/Entry.png "Пример записи")](views-images/Entry-Large.png#lightbox "Пример записи")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) |
|     |     |

### <a name="editor"></a>Редактор

|     |     |
| --- | --- |
| [`Editor`](xref:Xamarin.Forms.Editor) позволяет пользователю вводить и редактировать несколько строк текста. Текст доступен как свойство [`Text`](xref:Xamarin.Forms.InputView.Text) , а события [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) и [`Completed`](xref:Xamarin.Forms.Editor.Completed) запускаются при изменении текста или при завершении пользователем.<br /><br />Используйте представление [`Entry`](#entry) для ввода и редактирования одной строки текста.<br /><br />[Документация по API](xref:Xamarin.Forms.Editor) / [руководство](~/xamarin-forms/user-interface/text/editor.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![Пример записи](views-images/Editor.png "Пример редактора")](views-images/Editor-Large.png#lightbox "Пример редактора")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) |
|     |     |

## <a name="views-to-indicate-activity"></a>Визуальные элементы для обозначения действий

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| в [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) используется анимация, показывающая, что приложение вовлечено в длительное действие без указания хода выполнения. Свойство [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning) управляет анимацией.<br /><br />Если процесс выполнения действия известен, используйте вместо него [`ProgressBar`](#progressbar) .<br /><br />[Документация по API](xref:Xamarin.Forms.ActivityIndicator) / [руководство](~/xamarin-forms/user-interface/activityindicator.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/) | [![Пример Активитиндикатор](views-images/ActivityIndicator.png "Пример Активитиндикатор")](views-images/ActivityIndicator-Large.png#lightbox "Пример Активитиндикатор")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) |
|     |     |

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) использует анимацию, чтобы увидеть, что приложение проходит продолжительное действие. Задайте для свойства [`Progress`](xref:Xamarin.Forms.ProgressBar.Progress) значения от 0 до 1, чтобы указать ход выполнения.<br /><br />Если ход выполнения действия неизвестен, используйте вместо него [`ActivityIndicator`](#activityindicator) .<br /><br />[Документация по API](xref:Xamarin.Forms.ProgressBar) / [руководство](~/xamarin-forms/user-interface/progressbar.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/) | [![Пример для ProgressBar](views-images/ProgressBar.png "Пример для ProgressBar")](views-images/ProgressBar-Large.png#lightbox "Пример для ProgressBar")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-that-display-collections"></a>Визуальные элементы для отображения коллекций

### <a name="carouselview"></a>CarouselView

|     |     |
| --- | --- |
| [`CarouselView`](xref:Xamarin.Forms.CarouselView) отображает прокручиваемый список элементов данных. Задайте для свойства `ItemsSource` коллекцию объектов и задайте для свойства `ItemTemplate` объект [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , описывающий форматирование элементов. `CurrentItemChanged` событие сигнализирует, что отображаемый в данный момент элемент был изменен, который доступен как свойство `CurrentItem`.<br /><br />[Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/) / [Guide](~/xamarin-forms/user-interface/carouselview/index.md) | [![Пример Карауселвиев](views-images/CarouselView.png "Пример Карауселвиев")](views-images/CarouselView-Large.png#lightbox "Пример Карауселвиев")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselViewDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselViewDemoPage.xaml) |
|     |     |

### <a name="collectionview"></a>CollectionView

|     |     |
| --- | --- |
| [`CollectionView`](xref:Xamarin.Forms.CollectionView) отображает прокручиваемый список выбираемых элементов данных с использованием различных спецификаций макета. Она нацелена на предоставление более гибкой и производительной альтернативы [`ListView`](xref:Xamarin.Forms.ListView). Задайте для свойства `ItemsSource` коллекцию объектов и задайте для свойства `ItemTemplate` объект [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , описывающий форматирование элементов. `SelectionChanged` событие сигнализирует о том, что сделан выбор, который доступен как свойство `SelectedItem`.<br /><br />[Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/) / [Guide](~/xamarin-forms/user-interface/collectionview/index.md) | [![Пример CollectionView](views-images/CollectionView.png "Пример CollectionView")](views-images/CollectionView-Large.png#lightbox "Пример CollectionView")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CollectionViewDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CollectionViewDemoPage.xaml) |
|     |     |

### <a name="indicatorview"></a>IndicatorView

|     |     |
| --- | --- |
| `IndicatorView` отображает индикаторы, представляющие количество элементов в `CarouselView`. Задайте для свойства `CarouselView.IndicatorView` объект `IndicatorView`, чтобы отобразить индикаторы для `CarouselView`. <br /><br />[Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/) / [Guide](~/xamarin-forms/user-interface/indicatorview.md) | [![Пример Индикаторвиев](views-images/IndicatorView.png "Пример Индикаторвиев")](views-images/IndicatorView-Large.png#lightbox "Пример Индикаторвиев")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/IndicatorViewDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/IndicatorViewDemoPage.xaml) |
|     |     |

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](xref:Xamarin.Forms.ListView) является производным от [`ItemsView`](xref:Xamarin.Forms.ItemsView`1) и отображает прокручиваемый список выбираемых элементов данных. Задайте для свойства [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) коллекцию объектов и задайте для свойства [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) объект [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , описывающий форматирование элементов. [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) событие сигнализирует о том, что сделан выбор, который доступен как свойство [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) .<br /><br />[Документация по API](xref:Xamarin.Forms.ListView) / [руководство](~/xamarin-forms/user-interface/listview/index.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview/) | [![Пример ListView](views-images/ListView.png "Пример ListView")](views-images/ListView-Large.png#lightbox "Пример ListView")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) |
|     |     |

### <a name="picker"></a>Средство выбора

|     |     |
| --- | --- |
| [`Picker`](xref:Xamarin.Forms.Picker) отображает выбранный элемент из списка текстовых строк и позволяет выбрать этот элемент при касании представления. Задайте для свойства [`Items`](xref:Xamarin.Forms.Picker.Items) список строк или свойство [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) коллекции объектов. Событие [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) срабатывает при выборе элемента.<br /><br />`Picker` отображает список элементов, только если он выбран. Используйте [`ListView`](#listview) или [`TableView`](#tableview) для прокручиваемого списка, который остается на странице.<br /><br />[Документация по API](xref:Xamarin.Forms.Picker) / [руководство](~/xamarin-forms/user-interface/picker/index.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo) | [![Пример средства выбора](views-images/Picker.png "Пример средства выбора")](views-images/Picker-Large.png#lightbox "Пример средства выбора")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) |
|     |     |

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| [`TableView`](xref:Xamarin.Forms.TableView) отображает список строк типа [`Cell`](xref:Xamarin.Forms.Cell) с необязательными заголовками и подзаголовками. Задайте для свойства [`Root`](xref:Xamarin.Forms.TableView.Root) объект типа [`TableRoot`](xref:Xamarin.Forms.TableRoot)и добавьте [`TableSection`](xref:Xamarin.Forms.TableSection) объекты в этот `TableRoot`. Каждый `TableSection` является коллекцией объектов `Cell`.<br /><br />[Документация по API](xref:Xamarin.Forms.TableView) / [руководство](~/xamarin-forms/user-interface/tableview.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview) | [![Пример Таблевиев](views-images/TableView.png "Пример Таблевиев")](views-images/TableView-Large.png#lightbox "Пример Таблевиев")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewFormDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewFormDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Связанные ссылки

- [Пример Xamarin.Forms FormsGallery](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Примеры Xamarin.Forms](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Документация по API Xamarin.Forms](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
