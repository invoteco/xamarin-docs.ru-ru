---
title: Шрифты в Xamarin.Forms
description: В этой статье объясняется, как указать информацию о шрифте на элементах управления, которые отображают текст в приложениях Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2020
ms.openlocfilehash: 4f14d66e5321a0fb19078be4d97ae4df69f688c9
ms.sourcegitcommit: 6f3281a32017cfcebadde8a2d6e10651a277828f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/02/2020
ms.locfileid: "80587433"
---
# <a name="fonts-in-xamarinforms"></a>Шрифты в Xamarin.Forms

[![Скачать](~/media/shared/download.png) образец Скачать образец](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)

В этой статье описывается, как Xamarin.Forms позволяет указать атрибуты шрифта (включая вес и размер) на элементах управления, которые отображают текст. Информация о шрифте может быть [указана в коде](#Setting_Font_in_Code) или [указана в XAML](#Setting_Font_in_Xaml). Также можно использовать [пользовательский шрифт](#Using_a_Custom_Font)и [отображать иконки шрифта.](#display-font-icons)

<a name="Setting_Font_in_Code" />

## <a name="set-the-font-in-code"></a>Установите шрифт в коде

Используйте три связанных с шрифтом свойства любых элементов управления, которые отображают текст:

- **FontFamily** &ndash; `string` имя шрифта.
- **FontSize** &ndash; размер шрифта `double`как .
- **FontAttributes** &ndash; строка, определяющая информацию о стиле, `FontAttributes` как *курсив и* **смелый** (с использованием перечисления в C ).

Этот код показывает, как создать метку и указать размер шрифта и вес для отображения:

```csharp
var about = new Label
{
    FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
    FontAttributes = FontAttributes.Bold,
    Text = "Medium Bold Font"
};
```

<a name="FontSize" />

### <a name="font-size"></a>Размер шрифта

Свойство `FontSize` может быть установлено на двойное значение, например:

```csharp
label.FontSize = 24;
```

Значение размера измеряется в устройствах-независимых единицах. Для получения дополнительной [информации см.](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement)

Xamarin.Forms также определяет поля [`NamedSize`](xref:Xamarin.Forms.NamedSize) в перечислении, которые представляют определенные размеры шрифтов. Для получения дополнительной информации об названных размерах шрифта [см.](#named-font-sizes)

<a name="FontAttributes" />

### <a name="font-attributes"></a>Атрибуты шрифта

Типы шрифтов, такие как **смелые** `FontAttributes` и *курсивные* могут быть установлены на свойстве. В настоящее время поддерживаются следующие значения:

- **Нет**
- **Смелые**
- **Курсив**

Перечисление `FontAttribute` может быть использовано следующим образом (вы `OR` можете указать один атрибут или их вместе):

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="set-font-info-per-platform"></a>Установка информации шрифта на платформу

Кроме того, `Device.RuntimePlatform` свойство может быть использовано для установки различных имен шрифтов на каждой платформе, как показано в этом коде:

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "MarkerFelt-Thin" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/ArimaMadurai-Black.ttf#Arima Madurai",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

Хорошим источником информации о шрифтах для iOS является [iosfonts.com](http://iosfonts.com).

<a name="Setting_Font_in_Xaml" />

## <a name="set-the-font-in-xaml"></a>Установите шрифт в XAML

Xamarin.Forms элементы управления, `FontSize` которые отображают текст все имеют свойство, которое может быть установлено в XAML. Самый простой способ установить шрифт в XAML — использовать значения перечисления именованных размеров, как показано в этом примере:

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

Существует встроенный преобразователь `FontSize` для свойства, который позволяет все настройки шрифта, которые будут выражены как значение строки в XAML. Кроме того, `FontAttributes` свойство может быть использовано для указания атрибутов шрифта:

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

Свойство [`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-specific-values) также может быть использовано в XAML для визуализации разного шрифта на каждой платформе. Приведенный ниже пример использует различные шрифты на каждой платформе:

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="MarkerFelt-Thin" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/ArimaMadurai-Black.ttf#Arima Madurai" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

## <a name="named-font-sizes"></a>Именованные размеры шрифтов 

Xamarin.Forms определяет поля [`NamedSize`](xref:Xamarin.Forms.NamedSize) в перечислении, которые представляют определенные размеры шрифтов. В следующей `NamedSize` таблице показаны участники и их размеры по умолчанию на iOS, Android и универсальной платформе Windows (UWP):

| Участник | iOS | Android | UWP |
| --- | --- | --- | --- |
| `Default` | 16 | 14 | 14 |
| `Micro` | 11 | 10 | 15.667 |
| `Small` | 13 | 14 | 18.667 |
| `Medium` | 16 | 17 | 22.667 |
| `Large` | 20 | 22 | 32 |
| `Body` | 17 | 16 | 14 |
| `Header` | 17 | 96 | 46 |
| `Title` | 28 | 24 | 24 |
| `Subtitle` | 22 | 16 | 20 |
| `Caption` | 12 | 12 | 12 |

Значения размера измеряются в устройствах-независимых единицах. Для получения дополнительной [информации см.](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement)

Названные размеры шрифтов могут быть установлены как через XAML, так и код. Кроме того, `Device.GetNamedSize` метод можно вызвать `double` для возврата, представляющего указанный размер шрифта:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

> [!NOTE]
> На iOS и Android названные размеры шрифтов будут автоматически масштабироваться на основе вариантов доступности операционной системы. Такое поведение может быть отключено на iOS с помощью платформы. Для получения дополнительной информации на [iOS см.](~/xamarin-forms/platform/ios/named-font-size-scaling.md)

<a name="Using_a_Custom_Font" />

## <a name="use-a-custom-font"></a>Используйте пользовательский шрифт

Использование шрифта, кроме встроенных шрифтов, требует определенного кодирования для конкретных платформ. На этом скриншоте показан пользовательский шрифт **Lobster** из шрифтов Google с [открытым исходным кодом,](https://www.google.com/fonts) отображаемый с помощью Xamarin.Forms.

 [![Пользовательский шрифт на iOS и Android](fonts-images/custom-sml.png "Пример пользовательских шрифтов")](fonts-images/custom.png#lightbox "Пример пользовательских шрифтов")

Шаги, необходимые для каждой платформы, изложены ниже. При включении пользовательских файлов шрифтов с приложением убедитесь, что лицензия шрифта позволяет распространяться.

### <a name="ios"></a>iOS

Можно отобразить пользовательский шрифт, сначала гарантируя, что он загружен, а затем ссылаясь на него по имени, используя методы Xamarin.Forms. `Font`
Следуйте инструкциям в [этом блоге:](https://devblogs.microsoft.com/xamarin/custom-fonts-in-ios/)

1. Добавить файл шрифта с **помощью Build Action: BundleResource**и
2. Обновление файла **Info.plist** **(шрифты, предоставленные приложением,** или, `UIAppFonts`ключ), затем
3. Ссылайтесь на него по имени, где вы определяете шрифт в Xamarin.Forms!

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" : null // set only for iOS
}
```

### <a name="android"></a>Android

Xamarin.Forms для Android может ссылаться на пользовательский шрифт, который был добавлен в проект, следуя определенному стандарту именования. Сначала добавьте файл шрифта в папку **«Активы»** в проекте приложения и установите *Build Action: AndroidAsset.* Затем используйте полный путь и *имя шрифта,* разделенные хэшом (я), в качестве имени шрифта в Xamarin.Forms, как фрагмент кода ниже демонстрирует:

```csharp
new Label
{
  Text = "Hello, Forms!",
  FontFamily = Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : null // set only for Android
}
```

### <a name="windows"></a>Windows

Xamarin.Forms для платформ Windows могут ссылаться на пользовательский шрифт, который был добавлен в проект, следуя определенному стандарту именования. Сначала добавьте файл шрифта в **папку /Активы/Шрифты/ в** проект приложения и установите **действие сборки:Содержание**. Затем используйте полный путь и имя файла шрифта, а затем хэш (я) и **имя шрифта**, как фрагмент кода ниже демонстрирует:

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.UWP ? "Assets/Fonts/Lobster-Regular.ttf#Lobster" : null // set only for UWP apps
}
```

> [!NOTE]
> Обратите внимание, что имя файла шрифта и имя шрифта могут быть разными. Чтобы узнать имя шрифта в Windows, нажмите правой кнопкой мыши файла .ttf и выберите **Preview.** Затем имя шрифта можно определить из окна предварительного просмотра.

### <a name="xaml"></a>XAML

Вы также [`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#interact-with-the-ui-from-background-threads) можете использовать в XAML для визуализации пользовательского шрифта:

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="Lobster-Regular" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

## <a name="use-a-custom-font-preview"></a>Используйте пользовательский шрифт (PREVIEW)

Пользовательские шрифты могут быть добавлены в ваш общий проект Xamarin.Forms и использованы проектами платформы без какой-либо дополнительной работы. Чтобы этого добиться, выполните следующие действия.

1. Добавьте шрифт в общий проект Xamarin.Forms в качестве встроенного ресурса **(Build Action: EmbeddedResource).**
1. Зарегистрируйте файл шрифта в сборке, в `ExportFont` файле, например **AssemblyInfo.cs,** используя атрибут. Также можно указать дополнительный псевдоним.

> [!NOTE]
> Встроенные шрифты требуют использования Xamarin.Forms 4.5.0.530 или выше.

Ниже приводится следующий пример, на который отображается шрифт Lobster-Regular, зарегистрированный в сборке, а также псевдоним:

```csharp
using Xamarin.Forms;

[assembly: ExportFont("Lobster-Regular.ttf", Alias = "Lobster")]
```

> [!NOTE]
> Шрифт может быть в любой папке в общем проекте без указания имени папки при регистрации шрифта с сборкой.

Затем шрифт можно использовать на каждой платформе, ссылаясь на его имя, без расширения файла:

```xaml
<!-- Use font name -->
<Label Text="Hello Xamarin.Forms"
       FontFamily="Lobster-Regular" />
```

Кроме того, он может быть использован на каждой платформе, ссылаясь на его псевдоним:

```xaml
<!-- Use font alias -->
<Label Text="Hello Xamarin.Forms"
       FontFamily="Lobster" />
```

Эквивалентный код на C# выглядит так:

```csharp
// Use font name
Label label1 = new Label
{
    Text = "Hello Xamarin.Forms!",
    FontFamily = "Lobster-Regular"
};

// Use font alias
Label label2 = new Label
{
    Text = "Hello Xamarin.Forms!",
    FontFamily = "Lobster"
};
```

На следующих скриншотах изображен пользовательский шрифт:

[![Пользовательский шрифт на iOS и Android](fonts-images/custom-sml.png "Пример пользовательских шрифтов")](fonts-images/custom.png#lightbox "Пример пользовательских шрифтов")

## <a name="display-font-icons"></a>Отображение значков шрифтов

Иконки шрифта могут отображаться приложениями Xamarin.Forms, указывая данные значка шрифта в объекте. `FontImageSource` Этот класс, который вытекает из [`ImageSource`](xref:Xamarin.Forms.ImageSource) класса, имеет следующие свойства:

- `Glyph`— значение символа unicode значка шрифта, указанное как `string`.
- `Size`— `double` значение, указавающее размер в независях от устройства единицзначях от отрисованных значка шрифта. Значение по умолчанию — 30. Кроме того, это свойство может быть установлено на указанный размер шрифта.
- `FontFamily`— `string` представление семейства шрифтов, к которому принадлежит значок шрифта.
- `Color`— дополнительное [`Color`](xref:Xamarin.Forms.Color) значение, используемое при отображении значка шрифта.

Эти данные используются для создания PNG, который может отображаться `ImageSource`любым представлением, которое может отображать. Этот подход позволяет отображать значки шрифтов, такие как смайлики, несколькими представлениями, в отличие [`Label`](xref:Xamarin.Forms.Label)от ограничения отображения значка шрифта одним текстом, представляющим представление, например .

> [!IMPORTANT]
> Иконки шрифта могут быть указаны только в настоящее время по их представлению символов unicode.

Следующий пример XAML имеет одну иконку [`Image`](xref:Xamarin.Forms.Image) шрифта, отображаемую представлением:

```xaml
<Image BackgroundColor="#D1D1D1">
    <Image.Source>
        <FontImageSource Glyph="&#xf30c;"
                         FontFamily="{OnPlatform iOS=Ionicons, Android=ionicons.ttf#}"
                         Size="44" />
    </Image.Source>
</Image>
```

Этот код отображает значок XBox, от семейства шрифтов [`Image`](xref:Xamarin.Forms.Image) Ionicons, в представлении. Обратите внимание, что в то `\uf30c`время как символ unicode для этой `&#xf30c;`иконки, он должен быть бежал в XAML и так становится. Эквивалентный код на C# выглядит так:

```csharp
Image image = new Image { BackgroundColor = Color.FromHex("#D1D1D1") };
image.Source = new FontImageSource
{
    Glyph = "\uf30c",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Ionicons" : "ionicons.ttf#",
    Size = 44
};
```

На следующих скриншотах из образца [bindable Layouts](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts) показано несколько значков шрифтов, отображаемых связуемым макетом:

![Скриншот отображаемых иконок шрифта на iOS и Android](fonts-images/font-image-source.png "Иконки шрифта отображаются в представлении изображения")

## <a name="related-links"></a>Связанные ссылки

- [ШрифтыSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)
- [Текст (образец)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Связные планировки (образец)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [Привязываемые макеты](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
