---
title: Элементы управления XAML
description: Все представления, которые определены в Xamarin.Forms можно ссылаться из файлов XAML.
ms.topic: article
ms.prod: xamarin
ms.assetid: 639BD392-1496-41BB-BB09-7652273AC9D8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/03/2019
ms.openlocfilehash: 8217c6436cc8cfe8f4b9c6cc3445157319e8c0bf
ms.sourcegitcommit: d3f48bfe72bfe03aca247d47bc64bfbfad1d8071
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/06/2019
ms.locfileid: "66741116"
---
# <a name="xaml-controls"></a>Элементы управления XAML

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)

Представления являются объектами пользовательского интерфейса, такие как метки, кнопки и ползунки, которые обычно называются *элементов управления* или *мини-приложения* в других графических сред программирования. Представления, поддерживаемых Xamarin.Forms являются производными от класса [ `View` ](xref:Xamarin.Forms.View) класса.

Все представления, которые определены в Xamarin.Forms можно ссылаться из файлов XAML.

## <a name="views-for-presentation"></a>Представления для презентации

|     |     |
| --- | --- |
| <h3>BoxView</h3>Отображает прямоугольник определенного цвета.<p align="center">![Снимок экрана BoxView](xaml-controls-images/BoxView.png "BoxView")</p>[API](xref:Xamarin.Forms.BoxView) / [руководство](https://developer.xamarin.com/guides/xamarin-forms/user-interface/boxview/) | <pre valign="center">&lt;BoxView Color="Accent"<br />         WidthRequest="150"<br />         HeightRequest="150"<br />         HorizontalOptions="Center"&gt;</pre></p> |
| <h3>Изображение</h3>Отображает точечный рисунок.<p align="center">![Снимок экрана с изображением](xaml-controls-images/Image.png "образа")</p>[API](xref:Xamarin.Forms.Image) / [руководство](~/xamarin-forms/user-interface/images.md) | <pre>&lt;Image Source="https://aka.ms/campus.jpg"<br />       Aspect="AspectFit"<br />       HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>Метка</h3>Отображает один или несколько строк текста.<p align="center">![Снимок экрана метку](xaml-controls-images/Label.png "метки")</p>[API](xref:Xamarin.Forms.Label) / [руководство](~/xamarin-forms/user-interface/text/label.md) | <p valign="center"><pre>&lt;Label Text="Hello, Xamarin.Forms!"<br />       FontSize="Large"<br />       FontAttributes="Italic"<br />       HorizontalTextAlignment="Center" /&gt;</pre></p> |
| <h3>Карта</h3>Отображает карты.<p align="center">![Снимок экрана карту](xaml-controls-images/Map.png "карты")</p>[API](xref:Xamarin.Forms.Maps.Map) / [руководство](~/xamarin-forms/user-interface/map.md) | <p valign="center"><pre>&lt;maps:Map ItemsSource="{Binding Locations}" /&gt;</pre></p> |
| <h3>WebView</h3>Отображает веб-страницы или HTML-содержимого.<p align="center">![Снимок экрана: веб-представление](xaml-controls-images/WebView.png "WebView")</p>[API](xref:Xamarin.Forms.WebView) / [руководство](~/xamarin-forms/user-interface/webview.md) | <p valign="center"><pre>&lt;WebView Source="https://docs.microsoft.com/xamarin/"<br/>         VerticalOptions="FillAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-that-initiate-commands"></a>Представления, которые запускают команды

|     |     |
| --- | --- |
| <h3>Кнопка</h3>Отображает текст в прямоугольный объект.<p align="center">![Снимок экрана: кнопка](xaml-controls-images/Button.png "кнопки")</p>[API](xref:Xamarin.Forms.Button) / [руководство](~/xamarin-forms/user-interface/button.md) | <p valign="center"><pre>&lt;Button Text="Click Me!"<br />        Font="Large"<br />        BorderWidth="1"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand"<br />        Clicked="OnButtonClicked" /&gt;</pre></p> |
| <h3>ImageButton</h3>Отображает изображение в объекте прямоугольной.<p align="center">![Снимок экрана ImageButton](xaml-controls-images/ImageButton.png "ImageButton")</p>[API](xref:Xamarin.Forms.ImageButton) / [руководство](~/xamarin-forms/user-interface/imagebutton.md) | <p valign="center"><pre>&lt;ImageButton Source="XamarinLogo.png"<br />             HorizontalOptions="Center"<br />             VerticalOptions="CenterAndExpand"<br />             Clicked="OnImageButtonClicked" /&gt;</pre></p> |
| <h3>Панель поиска</h3>Отображает панель поиска, поиска.<p align="center">![Снимок экрана SearchBar](xaml-controls-images/SearchBar.png "SearchBar")</p>[API](xref:Xamarin.Forms.SearchBar) | <p valign="center"><pre>&lt;SearchBar Placeholder="Xamarin.Forms Property"<br />           SearchButtonPressed="OnSearchBarButtonPressed" /&gt;</pre></p> |
|     |     |

## <a name="views-for-setting-values"></a>Представления для задания значений

|     |     |
| --- | --- |
| <h3>Slider</h3>Позволяет выбрать `double` значение из непрерывного диапазона.<p align="center">![Снимок экрана ползунка](xaml-controls-images/Slider.png "\"ползунок\"")</p>[API](xref:Xamarin.Forms.Slider) / [руководство](~/xamarin-forms/user-interface/slider.md) | <p valign="center"><pre>&lt;Slider Minimum="0"<br />        Maximum="100"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Шаговый переключатель</h3>Позволяет выбрать `double` значение из диапазона добавочной.<p align="center">![Снимок экрана несопоставимого](xaml-controls-images/Stepper.png "несопоставимого")</p>[API](xref:Xamarin.Forms.Stepper) / [руководство](~/xamarin-forms/user-interface/stepper.md) | <p valign="center"><pre>&lt;Stepper Minimum="0"<br />         Maximum="10"<br />         Increment="0.1"<br />         HorizontalOptions="Center"<br />         VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Параметр</h3>Позволяет выбрать `boolean` значение.<p align="center">![Снимок экрана: параметр](xaml-controls-images/Switch.png "коммутатора")</p>[API](xref:Xamarin.Forms.Switch) | <p valign="center"><pre>&lt;Switch IsToggled="false"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>DatePicker</h3>Позволяет выбрать дату.<p align="center">![Снимок экрана DatePicker](xaml-controls-images/DatePicker.png "DatePicker")</p>[API](xref:Xamarin.Forms.DatePicker) / [руководство](~/xamarin-forms/user-interface/datepicker.md) | <p valign="center"><pre>&lt;DatePicker Format="D"<br/>            VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>TimePicker</h3>Позволяет выбрать времени.<p align="center">![Снимок экрана TimePicker](xaml-controls-images/TimePicker.png "TimePicker")</p>[API](xref:Xamarin.Forms.TimePicker) / [руководство](~/xamarin-forms/user-interface/timepicker.md) | <p valign="center"><pre>&lt;TimePicker Format="T"<br />            VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-for-editing-text"></a>Для редактирования текста

|     |     |
| --- | --- |
| <h3>Ввод</h3>Позволяет одной строки текста, чтобы вводить и редактировать.<p align="center">![Снимок экрана: запись](xaml-controls-images/Entry.png "запись")</p>[API](xref:Xamarin.Forms.Entry) / [руководство](~/xamarin-forms/user-interface/text/entry.md) | <p valign="center"><pre>&lt;Entry Keyboard="Email"<br />       Placeholder="Enter email address"<br />       VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Редактор</h3>Позволяет несколько строк текста, чтобы вводить и редактировать.<p align="center">![Снимок экрана редактора](xaml-controls-images/Editor.png "метки")</p>[API](xref:Xamarin.Forms.Editor) / [руководство](~/xamarin-forms/user-interface/text/editor.md) | <p valign="center"><pre>&lt;Editor VerticalOptions="FillAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-to-indicate-activity"></a>Представления, чтобы указать действие

|     |     |
| --- | --- |
| <h3>ActivityIndicator</h3>Отображает анимации, чтобы показать, что приложение участвует в длительной операции, без предоставления какого-либо указания хода выполнения.<p align="center">![Снимок экрана ActivityIndicator](xaml-controls-images/ActivityIndicator.png "ActivityIndicator")</p>[API](xref:Xamarin.Forms.ActivityIndicator) | <p valign="center"><pre>&lt;ActivityIndicator IsRunning="True"<br />                   VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>ProgressBar</h3>Отображает анимации, чтобы показать, что приложение выполняется через длительных действий.<p align="center">![Снимок экрана ProgressBar](xaml-controls-images/ProgressBar.png "ProgressBar")</p>[API](xref:Xamarin.Forms.ProgressBar) | <p valign="center"><pre>&lt;ProgressBar Progress=".5"<br />             VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-that-display-collections"></a>Представления, отображаемые в коллекции

|     |     |
| --- | --- |
| <h3>CollectionView</h3>прокручиваемый список элементов доступный для выбора данных, используя спецификации другой макет.<p align="center">![Снимок экрана CollectionView](xaml-controls-images/CollectionView.png "CollectionView")</p>[Руководство по](~/xamarin-forms/user-interface/collectionview/index.md) | <p valign="center"><pre>&lt;CollectionView ItemsSource="{Binding Monkeys}"&gt;<br/>                ItemTemplate="{StaticResource MonkeyTemplate}"<br />    &lt;CollectionView.ItemsLayout&gt;<br />       &lt;GridItemsLayout Orientation="Vertical"<br />                        Span="2" /&gt;<br />    &lt;/CollectionView.ItemsLayout&gt;<br />&lt;/CollectionView/&gt;</pre></p> |
| <h3>ListView</h3>Прокручиваемый список элементов данных, доступный для выбора.<p align="center">![Снимок экрана ListView](xaml-controls-images/ListView.png "ListView")</p>[API](xref:Xamarin.Forms.ListView) / [руководство](~/xamarin-forms/user-interface/listview/index.md) | <p valign="center"><pre>&lt;ListView ItemsSource="{Binding Monkeys}"&gt;<br />          ItemTemplate="{StaticResource MonkeyTemplate}" /&gt;</pre></p> |
| <h3>Средство выбора</h3>Отображается, выберите элемент из списка текстовых строк.<p align="center">![Снимок экрана управляющий элемент выбора](xaml-controls-images/Picker.png "средства выбора")</p>[API](xref:Xamarin.Forms.Picker) / [руководство](~/xamarin-forms/user-interface/picker/index.md) | <p valign="center"><pre>&lt;Picker Title="Select a monkey"<br />        TitleColor="Red"&gt;<br />  &lt;Picker.ItemsSource&lt;<br />    &lt;x:Array Type="{x:Type x:String}"&gt;<br />      &lt;x:String&gt;Baboon&lt;/x:String&gt;<br />      &lt;x:String&gt;Capuchin Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Blue Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Squirrel Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Golden Lion Tamarin&lt;/x:String&gt;<br />      &lt;x:String&gt;Howler Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Japanese Macaque&lt;/x:String&gt;<br />    &lt;/x:Array&gt;<br />  &lt;/Picker.ItemsSource&gt;<br />&lt;/Picker&gt;</pre></p> |
| <h3>TableView</h3>Отображает список интерактивных строк.<p align="center">![Снимок экрана TableView](xaml-controls-images/TableView.png "TableView")</p>[API](xref:Xamarin.Forms.TableView) / [руководство](~/xamarin-forms/user-interface/tableview.md) | <p valign="center"><pre>&lt;TableView Intent="Settings"&gt;<br />    &lt;TableRoot&gt;<br />        &lt;TableSection Title="Ring"&gt;<br />            &lt;SwitchCell Text="New Voice Mail" /&gt;<br />            &lt;SwitchCell Text="New Mail" On="true" /&gt;<br />        &lt;/TableSection&gt;<br />    &lt;/TableRoot&gt;<br />&lt;/TableView&gt;</pre></p> |
|     |     |

## <a name="related-links"></a>Связанные ссылки

- [Пример Xamarin.Forms FormsGallery](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Примеры Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Документация по API Xamarin.Forms](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
