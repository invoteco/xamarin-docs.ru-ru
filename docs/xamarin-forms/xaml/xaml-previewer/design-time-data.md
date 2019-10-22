---
title: Использование данных времени разработки с предварительным просмотром XAML
description: В этой статье объясняется, как использовать данные времени разработки для отображения макетов с большим объемом данных в средстве предварительного просмотра XAML без запуска приложения.
ms.prod: xamarin
ms.assetid: 0F608019-5951-4BE6-80E0-9EEE1733D642
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/27/2019
ms.openlocfilehash: 47171c3853fa8f5eb572971e119d51733cb53a40
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72303240"
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

После добавления пространств имен можно добавить `d:` перед любым атрибутом или элементом управления, чтобы показать его в средстве предварительного просмотра XAML. Элементы с `d:` не отображаются во время выполнения.

Например, можно добавить текст к метке, которая обычно привязана к данным.

```xaml
<Label Text="{Binding Name}" d:Text="Name!" />
```

[![Данные времени разработки с текстом в метке](xaml-previewer-images/designtimedata-label-sm.png "Добавление метки данных времени разработки с помощью текста")](xaml-previewer-images/designtimedata-label-lg.png#lightbox)

В этом примере, без `d:Text`, предварительный просмотр XAML не будет показывать ничего для метки. Вместо этого отображается "Name!" где метка будет содержать реальные данные во время выполнения.

@No__t_0 можно использовать с любым атрибутом для элемента управления Xamarin. Forms, например цветов, размеров и расстояний. Можно даже добавить его в сам элемент управления:

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

В этом примере отображается ListView из трех Текстцеллс в средстве предварительного просмотра XAML. @No__t_0 можно изменить на существующую модель данных в проекте.

Можно также создать массив объектов данных. Например, общие свойства `Monkey`ного объекта данных могут быть созданы как данные времени разработки:

```csharp
namespace Monkeys.Models
{
    public class Monkey
    {
        public string Name { get; set; }
        public string Location { get; set; }
    }
}
```

Чтобы использовать класс в XAML, необходимо импортировать пространство имен в корневой узел:

```xaml
xmlns:models="clr-namespace:Monkeys.Models"
```

```xaml
<StackLayout>
    <ListView ItemsSource="{Binding Items}">
        <d:ListView.ItemsSource>
            <x:Array Type="{x:Type models:Monkey}">
                <models:Monkey Name="Baboon" Location="Africa and Asia"/>
                <models:Monkey Name="Capuchin Monkey" Location="Central and South America"/>
                <models:Monkey Name="Blue Monkey" Location="Central and East Africa"/>
            </x:Array>
        </d:ListView.ItemsSource>
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="models:Monkey">
                <TextCell Text="{Binding Name}"
                          Detail="{Binding Location}" />
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackLayout>
```

Преимуществом здесь является возможность привязки к реальной модели, которую планируется использовать.

## <a name="alternative-hardcode-a-static-viewmodel"></a>Альтернатива: жестко статический ViewModel

Если вы не хотите добавлять данные времени разработки в отдельные элементы управления, можно настроить фиктивное хранилище данных для привязки к странице. См. запись блога Джеймс Монтеманьо, [посвященную добавлению данных времени разработки](https://montemagno.com/xamarin-forms-design-time-data-tips-best-practices/) , чтобы увидеть, как выполнить привязку к статическому VIEWMODEL в XAML.

## <a name="troubleshooting"></a>Устранение неполадок

### <a name="requirements"></a>Требования

Для данных времени разработки требуется минимальная версия Xamarin. Forms 3,6.

### <a name="intellisense-shows-squiggly-lines-under-my-design-time-data"></a>IntelliSense отображает волнистые линии под данными времени разработки

Это известная проблема, которая будет исправлена в предстоящей версии Visual Studio. Проект все равно будет выполнять сборку без ошибок.

### <a name="the-xaml-previewer-stopped-working"></a>Предварительный просмотр XAML перестал работать

Попробуйте закрыть и снова открыть XAML файл, а также очистить и перестроить проект.
