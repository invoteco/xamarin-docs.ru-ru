---
title: Представления Xamarin.Forms
description: Представления Xamarin.Forms являются стандартными блоками кроссплатформенных мобильных пользовательских интерфейсов. В этой статье перечислены представления, которые включены в Xamarin.Forms.
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/11/2019
ms.openlocfilehash: 0094fbc73e88dc4e84d8bf415db30c17f955ddcc
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2019
ms.locfileid: "69976606"
---
# <a name="xamarinforms-views"></a>Представления Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Представления Xamarin.Forms являются стандартными блоками кроссплатформенных мобильных пользовательских интерфейсов._

Представления являются объектами пользовательского интерфейса, такие как метки, кнопки и ползунки, которые обычно называются *элементов управления* или *мини-приложения* в других графических сред программирования. Представления, поддерживаемых Xamarin.Forms являются производными от класса [ `View` ](xref:Xamarin.Forms.View) класса. Их можно разделить на несколько категорий:

## <a name="views-for-presentation"></a>Представления для презентации

### <a name="label"></a>Метка

|     |     |
| --- | --- |
| [`Label`](xref:Xamarin.Forms.Label) Отображает однострочных текстовых строк или многострочные блоки текста, либо с форматированием константные или переменные. Задайте [ `Text` ](xref:Xamarin.Forms.Label.Text) строковое значение для константы форматирования или набора [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText) свойства [ `FormattedString` ](xref:Xamarin.Forms.FormattedString) объекта для переменной форматирование.<br /><br />[Документация по API](xref:Xamarin.Forms.Label) / [руководство](~/xamarin-forms/user-interface/text/label.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![Пример метки](views-images/Label.png "Пример метки")](views-images/Label-Large.png#lightbox "Пример метки")<br /> [Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) |
|     |     |

### <a name="image"></a>Изображение

|     |     |
| --- | --- |
| [`Image`](xref:Xamarin.Forms.Image) Отображает точечный рисунок. Растровые изображения можно загрузить через Интернет, внедрены в качестве ресурсов в проекте common или в проектах платформы, или созданная при помощи .NET `Stream` объекта.<br /><br />[Документация по API](xref:Xamarin.Forms.Image) / [руководство](~/xamarin-forms/user-interface/images.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages) | [![Пример изображения](views-images/Image.png "Пример изображения")](views-images/Image-Large.png#lightbox "Пример изображения")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) |
|     |     |

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](xref:Xamarin.Forms.BoxView) Отображает прямоугольник сплошной цвет [ `Color` ](xref:Xamarin.Forms.BoxView.Color) свойство. `BoxView` имеет размер запроса по умолчанию 40 x 40. Для других размеров, назначить [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) и [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) свойства.<br /><br />[Документация по API](xref:Xamarin.Forms.BoxView) / [руководство](~/xamarin-forms/user-interface/boxview.md) / [пример 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview), [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-textdecoration), [3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-listviewcolors/), [4 ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife), [5](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-dotmatrixclock), и [6](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock) | [![Пример боксвиев](views-images/BoxView.png "Пример боксвиев")](views-images/BoxView-Large.png#lightbox "Пример Боксвиев")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) |
|     |     |

### <a name="webview"></a>WebView

|     |     |
| --- | --- |
| [`WebView`](xref:Xamarin.Forms.WebView) Отображает веб-страницы или HTML-содержимого, в зависимости от [ `Source` ](xref:Xamarin.Forms.WebView.Source) свойству [ `UriWebViewSource` ](xref:Xamarin.Forms.UrlWebViewSource) или [ `HtmlWebViewSource` ](xref:Xamarin.Forms.HtmlWebViewSource) объекта.<br /><br />[Документация по API](xref:Xamarin.Forms.WebView) / [руководство](~/xamarin-forms/user-interface/webview.md) / [пример 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview) и [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview) | [![Пример WebView](views-images/WebView.png "Пример WebView")](views-images/WebView-Large.png#lightbox "Пример WebView")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](xref:Xamarin.Forms.OpenGLView) Отображает рисунки OpenGL в проекты iOS и Android. Не поддерживается для универсальной платформы Windows. Проекты iOS и Android, требуют наличия ссылки **OpenTK 1.0** сборки или **OpenTK** версии 1.0.0.0 сборки. `OpenGLView` проще использовать в общий проект; При использовании в библиотеку .NET Standard, затем зависимостей службы также потребуется (как показано в примере кода).<br /><br />Это единственная графическая возможность, встроенная в Xamarin. Forms, но приложение Xamarin. Forms может также визуализировать графику с [`SkiaSharp`](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)помощью, [`UrhoSharp`](~/xamarin-forms/user-interface/graphics/urhosharp.md)или.<br /><br />[Документация по API](xref:Xamarin.Forms.OpenGLView)<br /><br /> | [![Пример опенглвиев](views-images/OpenGLView.png "Пример опенглвиев")](views-images/OpenGLView-Large.png#lightbox "Пример Опенглвиев")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) |
|     |     |

### <a name="map"></a>Карта

|     |     |
| --- | --- |
| [`Map`](xref:Xamarin.Forms.Maps.Map) Отображает карты. **Xamarin.Forms.Maps** должен быть установлен пакет Nuget. Android и универсальной платформы Windows требуется ключ авторизации карты.<br /><br />[Документация по API](xref:Xamarin.Forms.Maps.Map) / [руководство](~/xamarin-forms/user-interface/map.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps/) | [![Пример Map](views-images/Map.png "Пример Map")](views-images/Map-Large.png#lightbox "Пример Map")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) |
|     |     |

## <a name="views-that-initiate-commands"></a>Представления, которые запускают команды

### <a name="button"></a>Кнопка

|     |     |
| --- | --- |
| [`Button`](xref:Xamarin.Forms.Button) представляет собой прямоугольный объект, который отображает текст, и который запускается [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) событие, когда она нажата.<br /><br />[Документация по API](xref:Xamarin.Forms.Button) / [руководство](~/xamarin-forms/user-interface/button.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos/) | [![Пример кнопки](views-images/Button.png "Пример кнопки")](views-images/Button-Large.png#lightbox "Пример кнопки")<br /> [Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) |
|     |     |

### <a name="imagebutton"></a>ImageButton

|     |     |
| --- | --- |
| `ImageButton` — Это прямоугольный объектов, отображающие образа и которое генерируется `Clicked` событие, когда она нажата.<br /><br /> [Руководство по](~/xamarin-forms/user-interface/imagebutton.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![Пример с ImageButton](views-images/ImageButton.png "Пример с ImageButton")](views-images/ImageButton-Large.png#lightbox "Пример с ImageButton")<br /> [Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageButtonDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml.cs) |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| [`SearchBar`](xref:Xamarin.Forms.SearchBar) Отображает область для пользователя к типу строку текста и кнопку (или клавишу клавиатуры), который сообщает приложению выполнения поиска. [ `Text` ](xref:Xamarin.Forms.SearchBar.Text) Свойство предоставляет доступ к тексту и [ `SearchButtonPressed` ](xref:Xamarin.Forms.SearchBar.SearchButtonPressed) событие указывает, что была нажата кнопка.<br /><br />[Документация по API](xref:Xamarin.Forms.SearchBar) / [руководство](~/xamarin-forms/user-interface/searchbar.md) / [образца](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/SearchBar) | [![Пример сеарчбар](views-images/SearchBar.png "Пример сеарчбар")](views-images/SearchBar-Large.png#lightbox "Пример Сеарчбар")<br /> [Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-for-setting-values"></a>Представления для задания значений

### <a name="checkbox"></a>CheckBox

|     |     |
| --- | --- |
| `CheckBox`позволяет пользователю выбрать логическое значение с помощью типа кнопки, которая может быть либо установлена, либо пустой. Свойство является состоянием `CheckBox`, а `CheckedChanged` событие возникает при изменении состояния. `IsChecked`<br /><br />Документация по API/[образец](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos) [руководства](~/xamarin-forms/user-interface/checkbox.md) /  | [![Пример флажка](views-images/CheckBox.png "Пример флажка")](views-images/CheckBox-Large.png#lightbox "Пример флажка")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CheckBoxDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CheckBoxDemoPage.xaml) |
|     |     |

### <a name="slider"></a>Slider

|     |     |
| --- | --- |
| [`Slider`](xref:Xamarin.Forms.Slider) позволяет пользователю выбрать `double` значение из непрерывного диапазона, заданного с помощью [ `Minimum` ](xref:Xamarin.Forms.Slider.Minimum) и [ `Maximum` ](xref:Xamarin.Forms.Slider.Maximum) свойства.<br /><br />[Документация по API](xref:Xamarin.Forms.Slider) / [руководство](~/xamarin-forms/user-interface/slider.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) | [![Пример ползунка](views-images/Slider.png "Пример ползунка")](views-images/Slider-Large.png#lightbox "Пример ползунка")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) |
|     |     |

### <a name="stepper"></a>Шаг

|     |     |
| --- | --- |
| [`Stepper`](xref:Xamarin.Forms.Stepper) позволяет пользователю выбрать `double` значение из диапазона добавочные значения, указанные с [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum), [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum), и [ `Increment` ](xref:Xamarin.Forms.Stepper.Increment) свойства.<br /><br />[Документация по API](xref:Xamarin.Forms.Stepper)  / [руководство](~/xamarin-forms/user-interface/stepper.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos) | [![Пример многошагового режима](views-images/Stepper.png "Пример многошагового режима")](views-images/Stepper-Large.png#lightbox "Пример многошагового режима")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) |
|     |     |

### <a name="switch"></a>Параметр

|     |     |
| --- | --- |
| [`Switch`](xref:Xamarin.Forms.Switch) принимает форму Признак включения, чтобы разрешить пользователю выбирать логическое значение. [ `IsToggled` ](xref:Xamarin.Forms.Switch.IsToggled) Свойство является состояние коммутатора и [ `Toggled` ](xref:Xamarin.Forms.Switch.Toggled) событие при изменении состояния.<br /><br />[Документация по API](xref:Xamarin.Forms.Switch) / [руководство](~/xamarin-forms/user-interface/switch.md) / [образца](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/SwitchDemos) | [![Пример параметра](views-images/Switch.png "Пример параметра")](views-images/Switch-Large.png#lightbox "Пример параметра")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](xref:Xamarin.Forms.DatePicker) позволяет пользователю выбрать дату с помощью платформы управления DatePicker. Задайте диапазон допустимых дат с [ `MinimumDate` ](xref:Xamarin.Forms.DatePicker.MinimumDate) и [ `MaximumDate` ](xref:Xamarin.Forms.DatePicker.MaximumDate) свойства. [ `Date` ](xref:Xamarin.Forms.DatePicker.Date) Свойство является выбранной даты и [ `DateSelected` ](xref:Xamarin.Forms.DatePicker.DateSelected) событие при изменении этого свойства.<br /><br />[Документация по API](xref:Xamarin.Forms.DatePicker) / [руководство](~/xamarin-forms/user-interface/datepicker.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker) | [![Пример DatePicker](views-images/DatePicker.png "Пример DatePicker")](views-images/DatePicker-Large.png#lightbox "Пример DatePicker")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](xref:Xamarin.Forms.TimePicker) позволяет пользователю выбрать время с средство выбора времени платформы. [ `Time` ](xref:Xamarin.Forms.TimePicker.Time) Свойство имеет установленный период времени. Приложения можно выполнять мониторинг изменений `Time` свойства, установив обработчик для [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged) событий.<br /><br />[Документация по API](xref:Xamarin.Forms.TimePicker) / [руководство](~/xamarin-forms/user-interface/timepicker.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker) | [![Пример TimePicker](views-images/TimePicker.png "Пример TimePicker")](views-images/TimePicker-Large.png#lightbox "Пример TimePicker")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) |
|     |     |

## <a name="views-for-editing-text"></a>Для редактирования текста

Эти классы являются производными от [ `InputView` ](xref:Xamarin.Forms.InputView) класс, определяющий [ `Keyboard` ](xref:Xamarin.Forms.InputView.Keyboard) свойство.

<a name="entry" />

### <a name="entry"></a>Ввод

|     |     |
| --- | --- |
| [`Entry`](xref:Xamarin.Forms.Entry) позволяет пользователю вводить и редактировать одна строка текста. Этот текст будет доступен как [ `Text` ](xref:Xamarin.Forms.Entry.Text) свойство и [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) и [ `Completed` ](xref:Xamarin.Forms.Entry.Completed) события запускаются, когда изменения текста или пользователь Уведомляет о завершении, нажав клавишу ВВОД.<br /><br />Используйте [ `Editor` ](#editor) для ввода и редактирования несколько строк текста.<br /><br />[Документация по API](xref:Xamarin.Forms.Entry) / [руководство](~/xamarin-forms/user-interface/text/entry.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![Пример записи](views-images/Entry.png "Пример записи")](views-images/Entry-Large.png#lightbox "Пример записи")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) |
|     |     |

<a name="editor" />

### <a name="editor"></a>Редактор

|     |     |
| --- | --- |
| [`Editor`](xref:Xamarin.Forms.Editor) позволяет пользователю вводить и редактировать несколько строк текста. Этот текст будет доступен как [ `Text` ](xref:Xamarin.Forms.Editor.Text) свойство и [ `TextChanged` ](xref:Xamarin.Forms.Editor.TextChanged) и [ `Completed` ](xref:Xamarin.Forms.Editor.Completed) события запускаются, когда изменения текста или пользователь Уведомляет о завершении.<br /><br />Используйте [ `Entry` ](#entry) представление для ввода и изменения одной строки текста.<br /><br />[Документация по API](xref:Xamarin.Forms.Editor) / [руководство](~/xamarin-forms/user-interface/text/editor.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![Пример записи](views-images/Editor.png "Пример редактора")](views-images/Editor-Large.png#lightbox "Пример редактора")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) |
|     |     |

## <a name="views-to-indicate-activity"></a>Представления, чтобы указать действие

<a name="activityindicator" />

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) использует анимацию, чтобы показать, что приложение обновляется длительных действий без предоставления какого-либо указания хода выполнения. [ `IsRunning` ](xref:Xamarin.Forms.ActivityIndicator.IsRunning) Свойство управляет анимацией.<br /><br />Если известно, ход выполнения действия, используйте [ `ProgressBar` ](#progressbar) вместо этого.<br /><br />[Документация по API](xref:Xamarin.Forms.ActivityIndicator) / [руководство](~/xamarin-forms/user-interface/activityindicator.md) / [образца](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ActivityIndicatorDemos) | [![Пример активитиндикатор](views-images/ActivityIndicator.png "Пример активитиндикатор")](views-images/ActivityIndicator-Large.png#lightbox "Пример Активитиндикатор")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) |
|     |     |

<a name="progressbar" />

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) использует анимацию, чтобы показать, что приложение выполняется через длительных действий. Задайте [ `Progress` ](xref:Xamarin.Forms.ProgressBar.Progress) свойство значения между 0 и 1 для указания хода выполнения.<br /><br />Если выполняется действие неизвестен, используйте [ `ActivityIndicator` ](#activityindicator) вместо этого.<br /><br />[Документация по API](xref:Xamarin.Forms.ProgressBar) / [руководство](~/xamarin-forms/user-interface/progressbar.md) / [образца](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ProgressBarDemos) | [![Пример для ProgressBar](views-images/ProgressBar.png "Пример для ProgressBar")](views-images/ProgressBar-Large.png#lightbox "Пример для ProgressBar")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-that-display-collections"></a>Представления, отображаемые в коллекции

### <a name="collectionview"></a>CollectionView

|     |     |
| --- | --- |
| [`CollectionView`](xref:Xamarin.Forms.CollectionView)Отображает прокручиваемый список выбираемых элементов данных с использованием различных спецификаций макета. Она нацелена на предоставление более гибкой и производительной альтернативы [`ListView`](xref:Xamarin.Forms.ListView). Присвойте `ItemTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) свойству коллекцию объектов и задайте для свойства объект, описывающий способ форматирования элементов. `ItemsSource` Событие сигнализирует, что сделан выбор, который доступен `SelectedItem` как свойство. `SelectionChanged`<br /><br />[Руководство по](~/xamarin-forms/user-interface/collectionview/index.md) / [образца](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/) | [![Пример CollectionView](views-images/CollectionView.png "Пример CollectionView")](views-images/CollectionView-Large.png#lightbox "Пример CollectionView")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CollectionViewDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CollectionViewDemoPage.xaml) |
|     |     |

<a name="listView" />

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](xref:Xamarin.Forms.ListView) является производным от [ `ItemsView` ](xref:Xamarin.Forms.ItemsView`1) и отображает прокручиваемый список элементов данных, доступный для выбора. Задайте [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) свойство в коллекцию объектов, и набор [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) свойства [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) объект, описывающий, как элементы, для форматирования. [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) Событие сигнализирует, что был сделан выбор, который доступен в виде [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) свойство.<br /><br />[Документация по API](xref:Xamarin.Forms.ListView) / [руководство](~/xamarin-forms/user-interface/listview/index.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview/) | [![Пример ListView](views-images/ListView.png "Пример ListView")](views-images/ListView-Large.png#lightbox "Пример ListView")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) |
|     |     |

### <a name="picker"></a>Средство выбора

|     |     |
| --- | --- |
| [`Picker`](xref:Xamarin.Forms.Picker) Отображение выбранного элемента из списка текстовых строк и выбор этого элемента, при выборе элемента представления. Задайте [ `Items` ](xref:Xamarin.Forms.Picker.Items) свойства списка строк, или [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) свойства для коллекции объектов. [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) Событие возникает, когда элемент выбран.<br /><br />`Picker` Отображает список элементов только в том случае, когда он выбран. Используйте [ `ListView` ](#listView) или [ `TableView` ](#tableView) прокручиваемый список, который остается на странице.<br /><br />[Документация по API](xref:Xamarin.Forms.Picker) / [руководство](~/xamarin-forms/user-interface/picker/index.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo) | [![Пример средства выбора](views-images/Picker.png "Пример средства выбора")](views-images/Picker-Large.png#lightbox "Пример средства выбора")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) |
|     |     |

<a name="tableView" />

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| [`TableView`](xref:Xamarin.Forms.TableView) Отображает список строк типа [ `Cell` ](xref:Xamarin.Forms.Cell) необязательные заголовки и подзаголовки. Задайте [ `Root` ](xref:Xamarin.Forms.TableView.Root) свойство для объекта типа [ `TableRoot` ](xref:Xamarin.Forms.TableRoot)и добавьте [ `TableSection` ](xref:Xamarin.Forms.TableSection) объектов, `TableRoot`. Каждый `TableSection` — это коллекция `Cell` объектов.<br /><br />[Документация по API](xref:Xamarin.Forms.TableView) / [руководство](~/xamarin-forms/user-interface/tableview.md) / [образца](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview) | [![Пример таблевиев](views-images/TableView.png "Пример таблевиев")](views-images/TableView-Large.png#lightbox "Пример Таблевиев")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewDemoPage.cs) / [страницы XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Связанные ссылки

- [Пример Xamarin.Forms FormsGallery](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Примеры Xamarin.Forms](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Документация по API Xamarin.Forms](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
