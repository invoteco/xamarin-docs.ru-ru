---
title: Использовать данные времени разработки с помощью средства предварительного просмотра XAML
description: В этой статье описываются способы использования данных времени разработки для отображения макеты массивами данных в средстве предварительного просмотра XAML, не запуская приложение.
ms.prod: xamarin
ms.assetid: 0F608019-5951-4BE6-80E0-9EEE1733D642
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/27/2019
ms.openlocfilehash: 60074c3c1b69a57d313ad0243246ba6db93dde3d
ms.sourcegitcommit: 0cb62b02a7efb5426f2356d7dbdfd9afd85f2f4a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/13/2019
ms.locfileid: "65557433"
---
# <a name="use-design-time-data-with-the-xaml-previewer"></a>Использовать данные времени разработки с помощью средства предварительного просмотра XAML

_Некоторые раскладки трудно визуализировать без данных. Используйте эти советы для наиболее эффективно использовать предварительный просмотр страниц данных доступно в средстве предварительного просмотра XAML._

## <a name="design-time-data-basics"></a>Основные сведения о данных времени разработки

Данные времени разработки является фиктивными данными, которые устанавливаются для упрощения ваших элементов управления для визуализации в средстве предварительного просмотра XAML. Чтобы начать работу, добавьте следующие строки кода в заголовок страницы XAML:

```xaml
xmlns:d="http://xamarin.com/schemas/2014/forms/design"
xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
mc:Ignorable="d"
```

После добавления пространств имен, можно поместить `d:` перед любой атрибут или элемент управления, он будет отображаться в средстве предварительного просмотра XAML. Элементы с `d:` не отображаются во время выполнения.

Например можно добавить текст к метке, обычно имеет связанные с ним данные.

```xaml
<Label Text="{Binding Name}" d:Text="Name!" />
```

[![Проектирование данных времени с текстом в метку](xaml-previewer-images/designtimedata-label-sm.png "разработки времени данных с текстом метки")](xaml-previewer-images/designtimedata-label-lg.png#lightbox)

В этом примере без `d:Text`, средство предварительного просмотра XAML не будет указывать ничего для метки. Вместо этого показывается «Name»! где метка будет иметь реальные данные во время выполнения.

Можно использовать `d:` с любой атрибут для элемента управления Xamarin.Forms, как цвета, размеры шрифтов и интервал. Можно даже добавить к самим элементом управления:

```xaml
<d:Button Text="Design Time Button" />
```

[![Проектирование данных времени с помощью управления "Кнопка"](xaml-previewer-images/designtimedata-controls-sm.png "проектирования данных времени с помощью управления \"Кнопка\"")](xaml-previewer-images/designtimedata-controls-lg.png#lightbox)

В этом примере кнопка отображается только во время разработки. Этот метод позволяет размещать заполнитель в для [пользовательского элемента управления, не поддерживается средством просмотра XAML](render-custom-controls.md).

## <a name="preview-images-at-design-time"></a>Изображения для предварительного просмотра во время разработки

Можно задать время разработки источника для изображений, которые привязаны к странице или динамически загружаться. В проект Android, добавьте образа, который нужно отобразить в средстве предварительного просмотра XAML для **ресурсы > Drawable** папки. В проекте iOS, добавлении образа **ресурсы** папки. Для демонстрации этого образа в средстве предварительного просмотра XAML во время разработки:

```xaml
<Image Source={Binding ProfilePicture} d:Source="DesignTimePicture.jpg" />
```
[![Проектирование времени данных с изображениями](xaml-previewer-images/designtimedata-image-sm.png "проектирования данных временных iamges")](xaml-previewer-images/designtimedata-image-lg.png#lightbox)

## <a name="design-time-data-for-listviews"></a>Данные времени разработки для ListViews

ListViews — это популярный способ отображения данных в мобильном приложении. Но их сложно представить, без реальных данных. Чтобы использовать данные времени разработки с ними, необходимо создать массив времени разработки для использования в качестве ItemsSource. Средство предварительного просмотра XAML отображает то, что в этот массив в вашей ListView во время разработки.

```xaml
<StackLayout>
    <ListView ItemsSource="{Binding Items}">
        <d:ListView.ItemsSource>
            <x:Array Type="{x:Type x:String}">
                <x:String>Item One</x:String>
                <x:String>Item Two</x:String>
                <x:String>Item Three</x:String>
            </x:Array>
        </d:ListView.ItemsSource>
        <ListView.ItemTemplate>
            <DataTemplate>
                <TextCell Text="{Binding ItemName}"
                          d:Text="{Binding .}" />
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackLayout>
```

[![Проектирование данных времени с помощью ListView](xaml-previewer-images/designtimedata-itemssource-sm.png "проектирования данных временных ListView")](xaml-previewer-images/designtimedata-itemssource-lg.png#lightbox)

В этом примере показано ListView три TextCells в средстве предварительного просмотра XAML. Вы можете изменить `x:String` к существующей модели данных в проекте.

Ссылаться на [Монтеманьо Hanselman.Forms приложения](https://github.com/jamesmontemagno/Hanselman.Forms/blob/vnext/src/Hanselman/Views/Podcasts/PodcastDetailsPage.xaml#L26-L47) более сложный пример.

## <a name="alternative-hardcode-a-static-viewmodel"></a>Альтернативный способ. Жестко задавать статические ViewModel

Если вы не хотите добавить данные времени разработки на отдельные элементы управления, можно настроить в хранилище данных макетов для привязки к странице. Ссылаться на Монтеманьо [записи блога о добавлении данных во время разработки](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data) чтобы узнать, как выполнить привязку к статическим ViewModel в XAML.

## <a name="troubleshooting"></a>Устранение неполадок

### <a name="requirements"></a>Требования

Данные времени разработки требуется Минимальная версия Xamarin.Forms 3.6.

### <a name="intellisense-shows-squiggly-lines-under-my-design-time-data"></a>IntelliSense отображает волнистые линии под данными времени разработки

Это известная проблема и будет устранена в последующих версиях Visual Studio. Проект по-прежнему будет построен без ошибок.

### <a name="the-xaml-previewer-stopped-working"></a>Средство предварительного просмотра XAML завершил свою работу

Попробуйте закрыть и повторно открыть файл XAML и очистки и перестроения проекта.
