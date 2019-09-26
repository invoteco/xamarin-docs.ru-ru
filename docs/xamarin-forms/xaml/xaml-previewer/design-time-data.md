---
title: Использование данных времени разработки с предварительным просмотром XAML
description: В этой статье объясняется, как использовать данные времени разработки для отображения макетов с большим объемом данных в средстве предварительного просмотра XAML без запуска приложения.
ms.prod: xamarin
ms.assetid: 0F608019-5951-4BE6-80E0-9EEE1733D642
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/27/2019
ms.openlocfilehash: a6a34615adc9cf290ff6bf9dd344487e5f29cfa2
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2019
ms.locfileid: "69887856"
---
# <a name="use-design-time-data-with-the-xaml-previewer"></a>Использование данных времени разработки с предварительным просмотром XAML

_Некоторые макеты трудно визуализировать без данных. Используйте эти советы, чтобы максимально эффективно использовать предварительный просмотр страниц, интенсивно использующих данные, в средстве предварительного просмотра XAML._

## <a name="design-time-data-basics"></a>Основы данных во время разработки

Данные времени разработки — это фиктивные данные, которые позволяют упростить визуализацию элементов управления в средстве предварительного просмотра XAML. Чтобы приступить к работе, добавьте следующие строки кода в заголовок страницы XAML:

```xaml
xmlns:d="http://xamarin.com/schemas/2014/forms/design"
xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
mc:Ignorable="d"
```

После добавления пространств имен можно разместить `d:` перед любым атрибутом или элементом управления, чтобы показать его в средстве предварительного просмотра XAML. Элементы с `d:` не отображаются во время выполнения.

Например, можно добавить текст к метке, которая обычно привязана к данным.

```xaml
<Label Text="{Binding Name}" d:Text="Name!" />
```

[![Данные времени разработки с текстом в метке] Добавление (xaml-previewer-images/designtimedata-label-sm.png "метки данных времени разработки с помощью текста")](xaml-previewer-images/designtimedata-label-lg.png#lightbox)

В этом примере, без `d:Text`, предварительный просмотр XAML не будет показывать ничего для метки. Вместо этого отображается "Name!" где метка будет содержать реальные данные во время выполнения.

Можно использовать `d:` с любым атрибутом для элемента управления Xamarin. Forms, например цветов, размеров и расстояний. Можно даже добавить его в сам элемент управления:

```xaml
<d:Button Text="Design Time Button" />
```

[![Данные времени разработки с помощью элемента управления Button](xaml-previewer-images/designtimedata-controls-sm.png "Данные времени разработки с помощью элемента управления Button")](xaml-previewer-images/designtimedata-controls-lg.png#lightbox)

В этом примере кнопка отображается только во время разработки. Используйте этот метод, чтобы поместить заполнитель в для [пользовательского элемента управления, не поддерживаемого средством предварительного просмотра XAML](render-custom-controls.md).

## <a name="preview-images-at-design-time"></a>Предварительный просмотр изображений во время разработки

Можно задать источник времени разработки для изображений, которые привязаны к странице или динамически загружаются. В проекте Android добавьте изображение, которое требуется отобразить в средстве предварительного просмотра XAML, в области **ресурсы > нарисованной** папке. В проекте iOS добавьте образ в папку **Resources** . Затем это изображение можно отобразить в средстве предварительного просмотра XAML во время разработки:

```xaml
<Image Source={Binding ProfilePicture} d:Source="DesignTimePicture.jpg" />
```

[![Данные времени разработки с помощью изображений](xaml-previewer-images/designtimedata-image-sm.png "Данные времени разработки с помощью иамжес")](xaml-previewer-images/designtimedata-image-lg.png#lightbox)

## <a name="design-time-data-for-listviews"></a>Данные времени разработки для ListView

ListView — это популярный способ отобразить данные в мобильном приложении. Однако их сложно визуализировать без реальных данных. Чтобы использовать данные времени разработки, необходимо создать массив времени разработки для использования в качестве ItemsSource. Предварительный просмотр XAML отображает содержимое этого массива в ListView во время разработки.

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

[![Данные времени разработки с помощью ListView](xaml-previewer-images/designtimedata-itemssource-sm.png "Данные времени разработки с помощью ListView")](xaml-previewer-images/designtimedata-itemssource-lg.png#lightbox)

В этом примере отображается ListView из трех Текстцеллс в средстве предварительного просмотра XAML. Можно перейти `x:String` к существующей модели данных в проекте.

Более сложный пример см. в [приложении Джеймс Монтеманьо Hanselman. Forms](https://github.com/jamesmontemagno/Hanselman.Forms/blob/vnext/src/Hanselman/Views/Podcasts/PodcastDetailsPage.xaml#L26-L47) .

## <a name="alternative-hardcode-a-static-viewmodel"></a>Следовательно Жестко кодировать статический ViewModel

Если вы не хотите добавлять данные времени разработки в отдельные элементы управления, можно настроить фиктивное хранилище данных для привязки к странице. См. запись блога Джеймс Монтеманьо, [посвященную добавлению данных времени разработки](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data) , чтобы увидеть, как выполнить привязку к статическому VIEWMODEL в XAML.

## <a name="troubleshooting"></a>Устранение неполадок

### <a name="requirements"></a>Требования

Для данных времени разработки требуется минимальная версия Xamarin. Forms 3,6.

### <a name="intellisense-shows-squiggly-lines-under-my-design-time-data"></a>IntelliSense отображает волнистые линии под данными времени разработки

Это известная проблема, которая будет исправлена в предстоящей версии Visual Studio. Проект все равно будет выполнять сборку без ошибок.

### <a name="the-xaml-previewer-stopped-working"></a>Предварительный просмотр XAML перестал работать

Попробуйте закрыть и снова открыть XAML файл, а также очистить и перестроить проект.
