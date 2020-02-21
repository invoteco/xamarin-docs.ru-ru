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
ms.openlocfilehash: 8eeb2f5b46cac8d2b232c33813b2c5818e588b74
ms.sourcegitcommit: 524fc148bad17272bda83c50775771daa45bfd7e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/20/2020
ms.locfileid: "77480618"
---
# <a name="xaml-controls"></a>Элементы управления XAML

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

Представления — это объекты пользовательского интерфейса, такие как метки, кнопки и ползунки, которые обычно называются *элементами управления* или *мини* -приложениями в других графических средах программирования. Представления, поддерживаемые Xamarin. Forms, являются производными от класса [`View`](xref:Xamarin.Forms.View) .

На все представления, определенные в Xamarin. Forms, можно ссылаться из файлов XAML.

## <a name="views-for-presentation"></a>Визуальные элементы для представления данных

|     |     |
| --- | --- |
| <h3>BoxView</h3>Отображает прямоугольник определенного цвета.<p align="center">![Снимок экрана Боксвиев](xaml-controls-images/BoxView.png "BoxView")</p>[Инструкции по](~/xamarin-forms/user-interface/boxview.md) / [API](xref:Xamarin.Forms.BoxView) | <pre valign="center">&lt;BoxView Color="Accent"<br />         WidthRequest="150"<br />         HeightRequest="150"<br />         HorizontalOptions="Center"&gt;</pre></p> |
| <h3>Образ —</h3>Отображает точечный рисунок.<p align="center">![Снимок экрана изображения](xaml-controls-images/Image.png "Образ —")</p>[Инструкции по](~/xamarin-forms/user-interface/images.md) / [API](xref:Xamarin.Forms.Image) | <pre>&lt;Image Source="https://aka.ms/campus.jpg"<br />       Aspect="AspectFit"<br />       HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>Метка</h3>Отображает одну или несколько строк текста.<p align="center">![Снимок экрана метки](xaml-controls-images/Label.png "Метка")</p>[Инструкции по](~/xamarin-forms/user-interface/text/label.md) / [API](xref:Xamarin.Forms.Label) | <p valign="center"><pre>&lt;Label Text="Hello, Xamarin.Forms!"<br />       FontSize="Large"<br />       FontAttributes="Italic"<br />       HorizontalTextAlignment="Center" /&gt;</pre></p> |
| <h3>Схема</h3>Отображает карту.<p align="center">![Снимок экрана с картой](xaml-controls-images/Map.png "Схема")</p>[Инструкции по](~/xamarin-forms/user-interface/map/index.md) / [API](xref:Xamarin.Forms.Maps.Map) | <p valign="center"><pre>&lt;maps:Map ItemsSource="{Binding Locations}" /&gt;</pre></p> |
| <h3>Веб-представление</h3>Отображает веб-страницы или содержимое HTML.<p align="center">![Снимок экрана WebView](xaml-controls-images/WebView.png "Веб-представление")</p>[Инструкции по](~/xamarin-forms/user-interface/webview.md) / [API](xref:Xamarin.Forms.WebView) | <p valign="center"><pre>&lt;WebView Source="https://docs.microsoft.com/xamarin/"<br/>         VerticalOptions="FillAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-that-initiate-commands"></a>Визуальные элементы, инициирующие команды

|     |     |
| --- | --- |
| <h3>Кнопка</h3>Отображает текст в прямоугольном объекте.<p align="center">![Снимок экрана кнопки](xaml-controls-images/Button.png "Кнопка")</p>[Инструкции по](~/xamarin-forms/user-interface/button.md) / [API](xref:Xamarin.Forms.Button) | <p valign="center"><pre>&lt;Button Text="Click Me!"<br />        Font="Large"<br />        BorderWidth="1"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand"<br />        Clicked="OnButtonClicked" /&gt;</pre></p> |
| <h3>ImageButton</h3>Отображает изображение в прямоугольном объекте.<p align="center">![Снимок экрана ImageButton](xaml-controls-images/ImageButton.png "ImageButton")</p>[Инструкции по](~/xamarin-forms/user-interface/imagebutton.md) / [API](xref:Xamarin.Forms.ImageButton) | <p valign="center"><pre>&lt;ImageButton Source="XamarinLogo.png"<br />             HorizontalOptions="Center"<br />             VerticalOptions="CenterAndExpand"<br />             Clicked="OnImageButtonClicked" /&gt;</pre></p> |
| <h3>RefreshView</h3>Предоставляет функции получения по обновлению для прокручиваемого содержимого.<p align="center">![Снимок экрана Рефрешвиев](xaml-controls-images/RefreshView.png "RefreshView")</p>[Программ](~/xamarin-forms/user-interface/refreshview.md) | <p valign="center"><pre>&lt;RefreshView IsRefreshing="{Binding IsRefreshing}"<br />             Command="{Binding RefreshCommand}" &gt;<br />    &lt;!-- Scrollable control goes here --&gt;<br />&lt;/RefreshView&gt;</pre></p> |
| <h3>Панель поиска</h3> Принимает вводимые пользователем данные, используемые для выполнения поиска.<p align="center">![Снимок экрана Сеарчбар](xaml-controls-images/SearchBar.png "Панель поиска")</p>[Программ](~/xamarin-forms/user-interface/searchbar.md) | <p valign="center"><pre>&lt;SearchBar Placeholder=&quot;Enter search term&quot;<br />           SearchButtonPressed="OnSearchBarButtonPressed" /&gt;</pre></p> |
| <h3>SwipeView</h3> Предоставляет элементы контекстного меню, отображенные с помощью жеста прокрутки.<p align="center">![Снимок экрана Свипевиев](xaml-controls-images/SwipeView.png "SwipeView")</p>[Программ](~/xamarin-forms/user-interface/swipeview.md) | <p valign="center"><pre>&lt;SwipeView&gt;<br />    &lt;SwipeView.LeftItems&gt;<br />        &lt;SwipeItems&gt;<br />            &lt;SwipeItem Text="Delete"<br />                       IconImageSource="delete.png"<br />                       BackgroundColor="LightPink"<br />                       Invoked="OnDeleteInvoked" /&gt;<br />        &lt;/SwipeItems&gt;<br />    &lt;/SwipeView.LeftItems&gt;<br />    &lt;!-- Content --&gt;<br />&lt;/SwipeView&gt;</pre></p> |
|     |     |

## <a name="views-for-setting-values"></a>Визуальные элементы для установки значений

|     |     |
| --- | --- |
| <h3>CheckBox</h3>Позволяет выбрать значение `boolean`.<p align="center">![Снимок экрана флажка](xaml-controls-images/CheckBox.png "CheckBox")</p> [Программ](~/xamarin-forms/user-interface/checkbox.md) | <p valign="center"><pre>&lt;CheckBox IsChecked="true"<br />          HorizontalOptions="Center"<br />          VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Ползунок</h3>Позволяет выбрать значение `double` из непрерывного диапазона.<p align="center">![Снимок экрана ползунка](xaml-controls-images/Slider.png "Ползунок")</p>[Инструкции по](~/xamarin-forms/user-interface/slider.md) / [API](xref:Xamarin.Forms.Slider) | <p valign="center"><pre>&lt;Slider Minimum="0"<br />        Maximum="100"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Шаговый переключатель</h3>Позволяет выбрать значение `double` из инкрементного диапазона.<p align="center">![Снимок экрана с пошаговыми средствами](xaml-controls-images/Stepper.png "Шаговый переключатель")</p>[Инструкции по](~/xamarin-forms/user-interface/stepper.md) / [API](xref:Xamarin.Forms.Stepper) | <p valign="center"><pre>&lt;Stepper Minimum="0"<br />         Maximum="10"<br />         Increment="0.1"<br />         HorizontalOptions="Center"<br />         VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Параметр</h3>Позволяет выбрать значение `boolean`.<p align="center">![Снимок экрана параметра](xaml-controls-images/Switch.png "Параметр")</p>[Инструкции по](~/xamarin-forms/user-interface/switch.md) / [API](xref:Xamarin.Forms.Switch)| <p valign="center"><pre>&lt;Switch IsToggled="false"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>DatePicker</h3>Позволяет выбрать дату.<p align="center">![Снимок экрана DatePicker](xaml-controls-images/DatePicker.png "DatePicker")</p>[Инструкции по](~/xamarin-forms/user-interface/datepicker.md) / [API](xref:Xamarin.Forms.DatePicker) | <p valign="center"><pre>&lt;DatePicker Format="D"<br/>            VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>TimePicker</h3>Позволяет выбрать время.<p align="center">![Снимок экрана TimePicker](xaml-controls-images/TimePicker.png "TimePicker")</p>[Инструкции по](~/xamarin-forms/user-interface/timepicker.md) / [API](xref:Xamarin.Forms.TimePicker) | <p valign="center"><pre>&lt;TimePicker Format="T"<br />            VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-for-editing-text"></a>Визуальные элементы для редактирования текста

|     |     |
| --- | --- |
| <h3>Запись</h3>Позволяет указать и изменить одну строку текста.<p align="center">![Снимок экрана записи](xaml-controls-images/Entry.png "Запись")</p>[Инструкции по](~/xamarin-forms/user-interface/text/entry.md) / [API](xref:Xamarin.Forms.Entry) | <p valign="center"><pre>&lt;Entry Keyboard="Email"<br />       Placeholder="Enter email address"<br />       VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Редактор</h3>Позволяет указать и изменить несколько строк текста.<p align="center">![Снимок экрана редактора](xaml-controls-images/Editor.png "Метка")</p>[Инструкции по](~/xamarin-forms/user-interface/text/editor.md) / [API](xref:Xamarin.Forms.Editor) | <p valign="center"><pre>&lt;Editor VerticalOptions="FillAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-to-indicate-activity"></a>Визуальные элементы для обозначения действий

|     |     |
| --- | --- |
| <h3>ActivityIndicator</h3>Отображает анимацию, показывающую, что приложение вовлечено в длительное действие без указания хода выполнения.<p align="center">![Снимок экрана Активитиндикатор](xaml-controls-images/ActivityIndicator.png "ActivityIndicator")</p>[Инструкции по](~/xamarin-forms/user-interface/activityindicator.md) / [API](xref:Xamarin.Forms.ActivityIndicator) | <p valign="center"><pre>&lt;ActivityIndicator IsRunning="True"<br />                   VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>ProgressBar</h3>Отображает анимацию, показывающую, что приложение проходит продолжительное действие.<p align="center">![Снимок экрана элемента ProgressBar](xaml-controls-images/ProgressBar.png "ProgressBar")</p>[Инструкции по](~/xamarin-forms/user-interface/progressbar.md) / [API](xref:Xamarin.Forms.ProgressBar) | <p valign="center"><pre>&lt;ProgressBar Progress=".5"<br />             VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-that-display-collections"></a>Визуальные элементы для отображения коллекций

|     |     |
| --- | --- |
| <h3>CarouselView</h3>Отображает прокручиваемый список элементов данных.<p align="center">![Снимок экрана Карауселвиев](xaml-controls-images/CarouselView.png "CarouselView")</p>[Программ](~/xamarin-forms/user-interface/carouselview/index.md) | <p valign="center"><pre>&lt;CarouselView ItemsSource="{Binding Monkeys}"&gt;<br/>              ItemTemplate="{StaticResource MonkeyTemplate}" /&gt;</pre></p>|
| <h3>CollectionView</h3>Отображает прокручиваемый список выбираемых элементов данных с использованием различных спецификаций макета.<p align="center">![Снимок экрана CollectionView](xaml-controls-images/CollectionView.png "CollectionView")</p>[Программ](~/xamarin-forms/user-interface/collectionview/index.md) | <p valign="center"><pre>&lt;CollectionView ItemsSource="{Binding Monkeys}"&gt;<br/>                ItemTemplate="{StaticResource MonkeyTemplate}"<br />    &lt;CollectionView.ItemsLayout&gt;<br />       &lt;GridItemsLayout Orientation="Vertical"<br />                        Span="2" /&gt;<br />    &lt;/CollectionView.ItemsLayout&gt;<br />&lt;/CollectionView/&gt;</pre></p> |
| <h3>IndicatorView</h3>Отображает индикаторы, представляющие количество элементов в `CarouselView`.<p align="center">![Снимок экрана Индикаторвиев](xaml-controls-images/IndicatorView.png "IndicatorView")</p>[Программ](~/xamarin-forms/user-interface/indicatorview.md) | <p valign="center"><pre>&lt;IndicatorView IndicatorView.ItemsSourceBy="carouselView"<br />               IndicatorColor="LightGray"<br />               SelectedIndicatorColor="DarkGray" /&gt;</pre></p> |
| <h3>ListView</h3>Отображает прокручиваемый список выбираемых элементов данных.<p align="center">![Снимок экрана: ListView](xaml-controls-images/ListView.png "ListView")</p>[Инструкции по](~/xamarin-forms/user-interface/listview/index.md) / [API](xref:Xamarin.Forms.ListView) | <p valign="center"><pre>&lt;ListView ItemsSource="{Binding Monkeys}"&gt;<br />          ItemTemplate="{StaticResource MonkeyTemplate}" /&gt;</pre></p> |
| <h3>Средство выбора</h3>Отображает выбранный элемент из списка текстовых строк.<p align="center">![Снимок экрана средства выбора](xaml-controls-images/Picker.png "Средство выбора")</p>[Инструкции по](~/xamarin-forms/user-interface/picker/index.md) / [API](xref:Xamarin.Forms.Picker) | <p valign="center"><pre>&lt;Picker Title="Select a monkey"<br />        TitleColor="Red"&gt;<br />  &lt;Picker.ItemsSource&lt;<br />    &lt;x:Array Type="{x:Type x:String}"&gt;<br />      &lt;x:String&gt;Baboon&lt;/x:String&gt;<br />      &lt;x:String&gt;Capuchin Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Blue Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Squirrel Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Golden Lion Tamarin&lt;/x:String&gt;<br />      &lt;x:String&gt;Howler Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Japanese Macaque&lt;/x:String&gt;<br />    &lt;/x:Array&gt;<br />  &lt;/Picker.ItemsSource&gt;<br />&lt;/Picker&gt;</pre></p> |
| <h3>TableView</h3>Отображает список интерактивных строк.<p align="center">![Снимок экрана Таблевиев](xaml-controls-images/TableView.png "TableView")</p>[Инструкции по](~/xamarin-forms/user-interface/tableview.md) / [API](xref:Xamarin.Forms.TableView) | <p valign="center"><pre>&lt;TableView Intent="Settings"&gt;<br />    &lt;TableRoot&gt;<br />        &lt;TableSection Title="Ring"&gt;<br />            &lt;SwitchCell Text="New Voice Mail" /&gt;<br />            &lt;SwitchCell Text="New Mail" On="true" /&gt;<br />        &lt;/TableSection&gt;<br />    &lt;/TableRoot&gt;<br />&lt;/TableView&gt;</pre></p> |
|     |     |

## <a name="related-links"></a>Связанные ссылки

- [Пример Xamarin.Forms FormsGallery](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Примеры Xamarin.Forms](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Документация по API Xamarin.Forms](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
