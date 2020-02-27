---
title: Шрифты в Xamarin.Forms
description: В этой статье объясняется, как указать информацию о шрифте для элементов управления, отображающие текст в приложениях Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/20/2020
ms.openlocfilehash: 75cf5acdb862a15d722075269b2f736264be680f
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/26/2020
ms.locfileid: "77636172"
---
# <a name="fonts-in-xamarinforms"></a>Шрифты в Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)

В этой статье описывается, как Xamarin.Forms позволяет указать атрибуты шрифта (включая вес и размер) в элементах управления, отображающие текст. Сведения о шрифтах можно [указать в коде](#Setting_Font_in_Code) или [в XAML](#Setting_Font_in_Xaml). Также можно использовать [пользовательский шрифт](#Using_a_Custom_Font)и [Отображать значки шрифтов](#display-font-icons).

<a name="Setting_Font_in_Code" />

## <a name="set-the-font-in-code"></a>Установка шрифта в коде

Используйте три связанные со шрифтами свойства любых элементов управления, отображающие текст:

- **FontFamily** &ndash; имя шрифта `string`.
- **FontSize** &ndash; размер шрифта как `double`.
- **Фонтаттрибутес** &ndash; строку, указывающую сведения о стиле, такие как *курсив* и **полужирный шрифт** (с помощью перечисления `FontAttributes` в C#).

Этот код показано, как создать метку и укажите размер и вес для отображения:

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

Для свойства `FontSize` можно задать значение Double, например:

```csharp
label.FontSize = 24;
```

Значение размера измеряется в единицах, не зависящих от устройства. Дополнительные сведения см. в разделе [единицы измерения](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement).

Xamarin. Forms также определяет поля в перечислении [`NamedSize`](xref:Xamarin.Forms.NamedSize) , представляющие конкретные размеры шрифтов. Дополнительные сведения об именованных размерах шрифтов см. в разделе [размеры именованных](#named-font-sizes)шрифтов.

<a name="FontAttributes" />

### <a name="font-attributes"></a>Атрибуты шрифта

Стили шрифта, такие как **Bold** и *Italic* , можно задать в свойстве `FontAttributes`. В настоящее время поддерживаются следующие значения:

- **None**
- **Полужирный**
- **Деле**

Перечисление `FontAttribute` можно использовать следующим образом (можно указать один атрибут или `OR` вместе):

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="set-font-info-per-platform"></a>Задание сведений о шрифте для платформы

Кроме того, свойство `Device.RuntimePlatform` можно использовать для задания различных названий шрифтов на каждой платформе, как показано в следующем коде:

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "MarkerFelt-Thin" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/ArimaMadurai-Black.ttf#Arima Madurai",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

Хорошим источником сведений о шрифтах для iOS является [iosfonts.com](http://iosfonts.com).

<a name="Setting_Font_in_Xaml" />

## <a name="set-the-font-in-xaml"></a>Установка шрифта в XAML

Элементы управления Xamarin. Forms, отображающие текст, имеют свойство `FontSize`, которое можно задать в XAML. Самый простой способ задать шрифт в XAML является использование значений перечисления из указанных размеров, как показано в следующем примере:

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

Существует встроенный преобразователь для свойства `FontSize`, который позволяет выражать все параметры шрифта как строковое значение в XAML. Кроме того, для указания атрибутов шрифта можно использовать свойство `FontAttributes`.

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

Свойство [`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-specific-values) также можно использовать в XAML для отображения различных шрифтов на каждой платформе. В приведенном ниже примере используются разные шрифты на каждой платформе:

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

## <a name="named-font-sizes"></a>Размеры именованных шрифтов

Xamarin. Forms определяет поля в перечислении [`NamedSize`](xref:Xamarin.Forms.NamedSize) , представляющие конкретные размеры шрифтов. В следующей таблице показаны элементы `NamedSize` и их размеры по умолчанию для iOS, Android и универсальная платформа Windows (UWP).

| Член | iOS | Android | UWP |
| --- | --- | --- | --- |
| `Default` | 16 | 14 | 14 |
| `Micro` | 11 | 10 | 15,667 |
| `Small` | 13 | 14 | 18,667 |
| `Medium` | 16 | 17 | 22,667 |
| `Large` | 20 | 22 | 32 |
| `Body` | 17 | 16 | 14 |
| `Header` | 17 | 96 | 46 |
| `Title` | 28 | 24 | 24 |
| `Subtitle` | 22 | 16 | 20 |
| `Caption` | 12 | 12 | 12 |

Значения размера измеряются в единицах, не зависящих от устройства. Дополнительные сведения см. в разделе [единицы измерения](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement).

Именованные размеры шрифтов можно задать с помощью XAML и кода. Кроме того, можно вызвать метод `Device.GetNamedSize`, чтобы вернуть `double`, представляющий именованный размер шрифта:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

> [!NOTE]
> В iOS и Android именованные размеры шрифтов будут автоматически масштабироваться на основе специальных возможностей операционной системы. Это поведение можно отключить в iOS с конкретной платформой. Дополнительные сведения см. [в разделе масштабирование специальных возможностей для именованных размеров шрифтов в iOS](~/xamarin-forms/platform/ios/named-font-size-scaling.md).

<a name="Using_a_Custom_Font" />

## <a name="use-a-custom-font"></a>Использовать пользовательский шрифт

Пользовательские шрифты можно добавить в общий проект Xamarin. Forms и использовать в проектах платформы без дополнительной работы. Чтобы этого добиться, выполните следующие действия.

1. Добавьте шрифт в общий проект Xamarin. Forms в качестве внедренного ресурса (**действие сборки: EmbeddedResource**).
1. Зарегистрируйте файл шрифта в сборке в файле, например **AssemblyInfo.CS**, с помощью атрибута `ExportFont`.

В следующем примере показан Лобстер-Regular шрифт, регистрируемый в сборке:

```csharp
using Xamarin.Forms;

[assembly: ExportFont("Lobster-Regular.ttf")]
```

> [!NOTE]
> Шрифт может находиться в любой папке в общем проекте без указания имени папки при регистрации шрифта в сборке.

Затем шрифт можно использовать на каждой платформе, ссылаясь на его имя, без расширения файла:

```xaml
<Label Text="Hello Xamarin.Forms"
       FontFamily="Lobster-Regular" />
```

Эквивалентный код на C# выглядит так:

```csharp
Label label = new Label
{
    Text = "Hello Xamarin.Forms!",
    FontFamily = "Lobster-Regular"
};
```

На следующих снимках экрана показан пользовательский шрифт:

[![Пользовательский шрифт в iOS и Android](fonts-images/custom-sml.png "Пример пользовательских шрифтов")](fonts-images/custom.png#lightbox "Пример пользовательских шрифтов")

## <a name="display-font-icons"></a>Отображение значков шрифтов

Значки шрифтов могут отображаться приложениями Xamarin. Forms путем указания данных значка шрифта в объекте `FontImageSource`. Этот класс, производный от класса [`ImageSource`](xref:Xamarin.Forms.ImageSource) , имеет следующие свойства:

- `Glyph` — значение символа Юникода значка шрифта, указанного как `string`.
- `Size` — `double` значение, указывающее размер (в единицах, независимых от устройства) значка отображаемого шрифта. Значение по умолчанию — 30. Кроме того, для этого свойства можно задать именованный размер шрифта.
- `FontFamily` — `string`, представляющий семейство шрифтов, которому принадлежит значок шрифта.
- `Color` — необязательное [`Color`](xref:Xamarin.Forms.Color) значение, используемое при отображении значка шрифта.

Эти данные используются для создания PNG-изображения, которое может отображаться в любом представлении, которое может отображать `ImageSource`. Такой подход позволяет отображать значки шрифтов, такие как эмодзи, в нескольких представлениях, в отличие от отображения значков шрифта в одном представлении представления текста, например [`Label`](xref:Xamarin.Forms.Label).

> [!IMPORTANT]
> Значки шрифтов могут быть заданы только в символьном представлении в Юникоде.

В следующем примере XAML отображается один значок шрифта, отображаемый [`Image`](xref:Xamarin.Forms.Image) представлением:

```xaml
<Image BackgroundColor="#D1D1D1">
    <Image.Source>
        <FontImageSource Glyph="&#xf30c;"
                         FontFamily="{OnPlatform iOS=Ionicons, Android=ionicons.ttf#}"
                         Size="44" />
    </Image.Source>
</Image>
```

Этот код отображает значок XBox из семейства шрифтов Иониконс в представлении [`Image`](xref:Xamarin.Forms.Image) . Обратите внимание, что хотя символ Юникода для этого значка является `\uf30c`, он должен быть экранирован в XAML, и поэтому становится `&#xf30c;`. Эквивалентный код на C# выглядит так:

```csharp
Image image = new Image { BackgroundColor = Color.FromHex("#D1D1D1") };
image.Source = new FontImageSource
{
    Glyph = "\uf30c",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Ionicons" : "ionicons.ttf#",
    Size = 44
};
```

На следующих снимках экрана из примера " [связываемые макеты](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts) " показаны несколько значков шрифтов, отображаемых с помощью связываемого макета:

![Снимок экрана с отображаемыми значками шрифтов в iOS и Android](fonts-images/font-image-source.png "Значки шрифтов, отображаемые в представлении изображений")

## <a name="related-links"></a>Связанные ссылки

- [фонтссампле](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)
- [Текст (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Макеты с возможностью привязки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [Привязываемые макеты](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
