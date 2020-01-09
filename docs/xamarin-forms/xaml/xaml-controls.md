---
title: Элементы управления XAML
description: На все представления, определенные в Xamarin. Forms, можно ссылаться из файлов XAML.
ms.topic: article
ms.prod: xamarin
ms.assetid: 639BD392-1496-41BB-BB09-7652273AC9D8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2019
ms.openlocfilehash: c1163ba49b987b8ebd3702d296b103b8a2e91d84
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75490244"
---
# <a name="xaml-controls"></a>Элементы управления XAML

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

Представления являются объектами пользовательского интерфейса, такие как метки, кнопки и ползунки, которые обычно называются *элементов управления* или *мини-приложения* в других графических сред программирования. Представления, поддерживаемых Xamarin.Forms являются производными от класса [ `View` ](xref:Xamarin.Forms.View) класса.

На все представления, определенные в Xamarin. Forms, можно ссылаться из файлов XAML.

## <a name="views-for-presentation"></a>Представления для презентации

|     |     |
| --- | --- |
| <h3>BoxView</h3>Отображает прямоугольник определенного цвета.<p align="center">![Снимок экрана Боксвиев](xaml-controls-images/BoxView.png "BoxView")</p>[API](xref:Xamarin.Forms.BoxView) / [Руководство](~/xamarin-forms/user-interface/boxview.md) | <pre valign="center">&lt;BoxView Color="Accent"<br />         WidthRequest="150"<br />         HeightRequest="150"<br />         HorizontalOptions="Center"&gt;</pre></p> |
| <h3>Изображение</h3>Отображает точечный рисунок.<p align="center">![Снимок экрана изображения](xaml-controls-images/Image.png "Изображение")</p>[API](xref:Xamarin.Forms.Image) / [Руководство](~/xamarin-forms/user-interface/images.md) | <pre>&lt;Image Source="https://aka.ms/campus.jpg"<br />       Aspect="AspectFit"<br />       HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>Метка</h3>Отображает одну или несколько строк текста.<p align="center">![Снимок экрана метки](xaml-controls-images/Label.png "Метка")</p>[API](xref:Xamarin.Forms.Label) / [Руководство](~/xamarin-forms/user-interface/text/label.md) | <p valign="center"><pre>&lt;Label Text="Hello, Xamarin.Forms!"<br />       FontSize="Large"<br />       FontAttributes="Italic"<br />       HorizontalTextAlignment="Center" /&gt;</pre></p> |
| <h3>Карта</h3>Отображает карту.<p align="center">![Снимок экрана с картой](xaml-controls-images/Map.png "Карта")</p>[API](xref:Xamarin.Forms.Maps.Map) / [Руководство](~/xamarin-forms/user-interface/map/index.md) | <p valign="center"><pre>&lt;maps:Map ItemsSource="{Binding Locations}" /&gt;</pre></p> |
| <h3>WebView</h3>Отображает веб-страницы или содержимое HTML.<p align="center">![Снимок экрана WebView](xaml-controls-images/WebView.png "WebView")</p>[API](xref:Xamarin.Forms.WebView) / [Руководство](~/xamarin-forms/user-interface/webview.md) | <p valign="center"><pre>&lt;WebView Source="https://docs.microsoft.com/xamarin/"<br/>         VerticalOptions="FillAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-that-initiate-commands"></a>Представления, которые запускают команды

|     |     |
| --- | --- |
| <h3>Кнопка</h3>Отображает текст в прямоугольном объекте.<p align="center">![Снимок экрана кнопки](xaml-controls-images/Button.png "Кнопка")</p>[API](xref:Xamarin.Forms.Button) / [Руководство](~/xamarin-forms/user-interface/button.md) | <p valign="center"><pre>&lt;Button Text="Click Me!"<br />        Font="Large"<br />        BorderWidth="1"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand"<br />        Clicked="OnButtonClicked" /&gt;</pre></p> |
| <h3>ImageButton</h3>Отображает изображение в прямоугольном объекте.<p align="center">![Снимок экрана ImageButton](xaml-controls-images/ImageButton.png "ImageButton")</p>[API](xref:Xamarin.Forms.ImageButton) / [Руководство](~/xamarin-forms/user-interface/imagebutton.md) | <p valign="center"><pre>&lt;ImageButton Source="XamarinLogo.png"<br />             HorizontalOptions="Center"<br />             VerticalOptions="CenterAndExpand"<br />             Clicked="OnImageButtonClicked" /&gt;</pre></p> |
| <h3>RefreshView</h3>Предоставляет функции получения по обновлению для прокручиваемого содержимого.<p align="center">![Снимок экрана Рефрешвиев](xaml-controls-images/RefreshView.png "RefreshView")</p>[Программ](~/xamarin-forms/user-interface/refreshview.md) | <p valign="center"><pre>&lt;RefreshView IsRefreshing="{Binding IsRefreshing}"<br />             Command="{Binding RefreshCommand}" &gt;<br />    &lt;!-- Scrollable control goes here --&gt;<br />&lt;/RefreshView&gt;</pre></p> |
| <h3>Панель поиска</h3> Принимает вводимые пользователем данные, используемые для выполнения поиска.<p align="center">![Снимок экрана Сеарчбар](xaml-controls-images/SearchBar.png "Панель поиска")</p>[Программ](~/xamarin-forms/user-interface/searchbar.md) | <p valign="center"><pre>&lt;SearchBar Placeholder=&quot;Enter search term&quot;<br />           SearchButtonPressed="OnSearchBarButtonPressed" /&gt;</pre></p> |
| <h3>SwipeView</h3> Предоставляет элементы контекстного меню, отображенные с помощью жеста прокрутки.<p align="center">![Снимок экрана Свипевиев](xaml-controls-images/SwipeView.png "SwipeView")</p>[Программ](~/xamarin-forms/user-interface/swipeview.md) | <p valign="center"><pre>&lt;SwipeView&gt;<br />    &lt;SwipeView.LeftItems&gt;<br />        &lt;SwipeItems&gt;<br />            &lt;SwipeItem Text="Delete"<br />                       IconImageSource="delete.png"<br />                       BackgroundColor="LightPink"<br />                       Invoked="OnDeleteInvoked" /&gt;<br />        &lt;/SwipeItems&gt;<br />    &lt;/SwipeView.LeftItems&gt;<br />    &lt;!-- Content --&gt;<br />&lt;/SwipeView&gt;</pre></p> |
|     |     |

## <a name="views-for-setting-values"></a>Представления для задания значений

|     |     |
| --- | --- |
| <h3>CheckBox</h3>Позволяет выбрать значение `boolean`.<p align="center">![Снимок экрана флажка](xaml-controls-images/CheckBox.png "CheckBox")</p> [Программ](~/xamarin-forms/user-interface/checkbox.md) | <p valign="center"><pre>&lt;CheckBox IsChecked="true"<br />          HorizontalOptions="Center"<br />          VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Slider</h3>Позволяет выбрать значение `double` из непрерывного диапазона.<p align="center">![Снимок экрана ползунка](xaml-controls-images/Slider.png "Slider")</p>[API](xref:Xamarin.Forms.Slider) / [Руководство](~/xamarin-forms/user-interface/slider.md) | <p valign="center"><pre>&lt;Slider Minimum="0"<br />        Maximum="100"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Шаговый переключатель</h3>Позволяет выбрать значение `double` из инкрементного диапазона.<p align="center">![Снимок экрана с пошаговыми средствами](xaml-controls-images/Stepper.png "Шаговый переключатель")</p>[API](xref:Xamarin.Forms.Stepper) / [Руководство](~/xamarin-forms/user-interface/stepper.md) | <p valign="center"><pre>&lt;Stepper Minimum="0"<br />         Maximum="10"<br />         Increment="0.1"<br />         HorizontalOptions="Center"<br />         VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Переключение</h3>Позволяет выбрать значение `boolean`.<p align="center">![Снимок экрана параметра](xaml-controls-images/Switch.png "Переключение")</p>[API](xref:Xamarin.Forms.Switch) / [Руководство](~/xamarin-forms/user-interface/switch.md)| <p valign="center"><pre>&lt;Switch IsToggled="false"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>DatePicker</h3>Позволяет выбрать дату.<p align="center">![Снимок экрана DatePicker](xaml-controls-images/DatePicker.png "DatePicker")</p>[API](xref:Xamarin.Forms.DatePicker) / [Руководство](~/xamarin-forms/user-interface/datepicker.md) | <p valign="center"><pre>&lt;DatePicker Format="D"<br/>            VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>TimePicker</h3>Позволяет выбрать время.<p align="center">![Снимок экрана TimePicker](xaml-controls-images/TimePicker.png "TimePicker")</p>[API](xref:Xamarin.Forms.TimePicker) / [Руководство](~/xamarin-forms/user-interface/timepicker.md) | <p valign="center"><pre>&lt;TimePicker Format="T"<br />            VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-for-editing-text"></a>Для редактирования текста

|     |     |
| --- | --- |
| <h3>Элемент</h3>Позволяет указать и изменить одну строку текста.<p align="center">![Снимок экрана записи](xaml-controls-images/Entry.png "Элемент")</p>[API](xref:Xamarin.Forms.Entry) / [Руководство](~/xamarin-forms/user-interface/text/entry.md) | <p valign="center"><pre>&lt;Entry Keyboard="Email"<br />       Placeholder="Enter email address"<br />       VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Редактор</h3>Позволяет указать и изменить несколько строк текста.<p align="center">![Снимок экрана редактора](xaml-controls-images/Editor.png "Метка")</p>[API](xref:Xamarin.Forms.Editor) / [Руководство](~/xamarin-forms/user-interface/text/editor.md) | <p valign="center"><pre>&lt;Editor VerticalOptions="FillAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-to-indicate-activity"></a>Представления, чтобы указать действие

|     |     |
| --- | --- |
| <h3>ActivityIndicator</h3>Отображает анимацию, показывающую, что приложение вовлечено в длительное действие без указания хода выполнения.<p align="center">![Снимок экрана Активитиндикатор](xaml-controls-images/ActivityIndicator.png "ActivityIndicator")</p>[API](xref:Xamarin.Forms.ActivityIndicator) / [Руководство](~/xamarin-forms/user-interface/activityindicator.md) | <p valign="center"><pre>&lt;ActivityIndicator IsRunning="True"<br />                   VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>ProgressBar</h3>Отображает анимацию, показывающую, что приложение проходит продолжительное действие.<p align="center">![Снимок экрана элемента ProgressBar](xaml-controls-images/ProgressBar.png "ProgressBar")</p>[API](xref:Xamarin.Forms.ProgressBar) / [Руководство](~/xamarin-forms/user-interface/progressbar.md) | <p valign="center"><pre>&lt;ProgressBar Progress=".5"<br />             VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-that-display-collections"></a>Представления, отображаемые в коллекции

|     |     |
| --- | --- |
| <h3>CarouselView</h3>Отображает прокручиваемый список элементов данных.<p align="center">![Снимок экрана Карауселвиев](xaml-controls-images/CarouselView.png "CarouselView")</p>[Программ](~/xamarin-forms/user-interface/carouselview/index.md) | <p valign="center"><pre>&lt;CarouselView ItemsSource="{Binding Monkeys}"&gt;<br/>              ItemTemplate="{StaticResource MonkeyTemplate}" /&gt;</pre></p>|
| <h3>CollectionView</h3>Отображает прокручиваемый список выбираемых элементов данных с использованием различных спецификаций макета.<p align="center">![Снимок экрана CollectionView](xaml-controls-images/CollectionView.png "CollectionView")</p>[Программ](~/xamarin-forms/user-interface/collectionview/index.md) | <p valign="center"><pre>&lt;CollectionView ItemsSource="{Binding Monkeys}"&gt;<br/>                ItemTemplate="{StaticResource MonkeyTemplate}"<br />    &lt;CollectionView.ItemsLayout&gt;<br />       &lt;GridItemsLayout Orientation="Vertical"<br />                        Span="2" /&gt;<br />    &lt;/CollectionView.ItemsLayout&gt;<br />&lt;/CollectionView/&gt;</pre></p> |
| <h3>IndicatorView</h3>Отображает индикаторы, представляющие количество элементов в `CarouselView`.<p align="center">![Снимок экрана Индикаторвиев](xaml-controls-images/IndicatorView.png "IndicatorView")</p>[Программ](~/xamarin-forms/user-interface/indicatorview.md) | <p valign="center"><pre>&lt;IndicatorView ItemsSourceBy="carouselView"<br />               IndicatorColor="LightGray"<br />               SelectedIndicatorColor="DarkGray" /&gt;</pre></p> |
| <h3>ListView</h3>Отображает прокручиваемый список выбираемых элементов данных.<p align="center">![Снимок экрана: ListView](xaml-controls-images/ListView.png "ListView")</p>[API](xref:Xamarin.Forms.ListView) / [Руководство](~/xamarin-forms/user-interface/listview/index.md) | <p valign="center"><pre>&lt;ListView ItemsSource="{Binding Monkeys}"&gt;<br />          ItemTemplate="{StaticResource MonkeyTemplate}" /&gt;</pre></p> |
| <h3>Средство выбора</h3>Отображает выбранный элемент из списка текстовых строк.<p align="center">![Снимок экрана средства выбора](xaml-controls-images/Picker.png "Средство выбора")</p>[API](xref:Xamarin.Forms.Picker) / [Руководство](~/xamarin-forms/user-interface/picker/index.md) | <p valign="center"><pre>&lt;Picker Title="Select a monkey"<br />        TitleColor="Red"&gt;<br />  &lt;Picker.ItemsSource&lt;<br />    &lt;x:Array Type="{x:Type x:String}"&gt;<br />      &lt;x:String&gt;Baboon&lt;/x:String&gt;<br />      &lt;x:String&gt;Capuchin Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Blue Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Squirrel Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Golden Lion Tamarin&lt;/x:String&gt;<br />      &lt;x:String&gt;Howler Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Japanese Macaque&lt;/x:String&gt;<br />    &lt;/x:Array&gt;<br />  &lt;/Picker.ItemsSource&gt;<br />&lt;/Picker&gt;</pre></p> |
| <h3>TableView</h3>Отображает список интерактивных строк.<p align="center">![Снимок экрана Таблевиев](xaml-controls-images/TableView.png "TableView")</p>[API](xref:Xamarin.Forms.TableView) / [Руководство](~/xamarin-forms/user-interface/tableview.md) | <p valign="center"><pre>&lt;TableView Intent="Settings"&gt;<br />    &lt;TableRoot&gt;<br />        &lt;TableSection Title="Ring"&gt;<br />            &lt;SwitchCell Text="New Voice Mail" /&gt;<br />            &lt;SwitchCell Text="New Mail" On="true" /&gt;<br />        &lt;/TableSection&gt;<br />    &lt;/TableRoot&gt;<br />&lt;/TableView&gt;</pre></p> |
|     |     |

## <a name="related-links"></a>Связанные ссылки

- [Пример Xamarin.Forms FormsGallery](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Примеры Xamarin.Forms](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Документация по API Xamarin.Forms](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
