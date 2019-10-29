---
title: Общие сведения о главе 3. Глубже в тексте
description: 'Создание мобильных приложений с помощью Xamarin. Forms: сводка в главе 3. Глубже в тексте'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2E5581A6-4D3E-4BD5-9FDB-ACBA0F0FC734
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: 69415b59bbd376330454302981e3216c236a16bb
ms.sourcegitcommit: 93697a20e6fc7da547a8714ac109d7953b61d63f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/28/2019
ms.locfileid: "72980924"
---
# <a name="summary-of-chapter-3-deeper-into-text"></a>Общие сведения о главе 3. Глубже в тексте

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)

В этой главе подробно рассматриваются [`Label`](xref:Xamarin.Forms.Label) представления, в том числе цвет, шрифты и форматирование.

## <a name="wrapping-paragraphs"></a>Заключение абзацев

Если свойство [`Text`](xref:Xamarin.Forms.Label.Text) `Label` содержит длинный текст, `Label` автоматически заключает его в несколько строк, как показано в примере [**баскервиллес**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/Baskervilles) . Можно внедрить коды Юникода, например "\u2014", для длинного тире или C# символов, например "\r", чтобы прервать на новую строку.

Если для свойств [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) и [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) `Label` задано значение `LayoutOptions.Fill`, общий размер `Label` регулируется пространством, которое становится доступным контейнеру. Считается, что `Label` *ограничена*. Размер `Label` — это размер контейнера.

Если для свойств `HorizontalOptions` и `VerticalOptions` заданы значения, отличные от `LayoutOptions.Fill`, размер `Label` регулируется пространством, необходимое для отрисовки текста, вплоть до размера, который его контейнер делает доступным для `Label`. Считается, что `Label` не *ограничена* , и он определяет свой собственный размер.

(Примечание. условия с *ограниченным* и *неограниченным* доступом могут быть интуитивно понятны, так как неограниченное представление обычно меньше, чем ограниченное представление. Кроме того, эти термины не используются постоянно в первых главах книги.)

Такое представление, как `Label`, может быть ограничено одним измерением и не ограничено в другом. `Label` будет переносить текст только на несколько строк, если он ограничен горизонтально.

Если `Label` ограничено, он может занимать значительно больше пространства, чем требуется для текста. Текст можно поместить в общую область `Label`. Задайте для свойства [`HorizontalTextAlignment`](xref:Xamarin.Forms.Label.HorizontalTextAlignment) элемент перечисления [`TextAlignment`](xref:Xamarin.Forms.TextAlignment) ([`Start`](xref:Xamarin.Forms.TextAlignment.Start), [`Center`](xref:Xamarin.Forms.TextAlignment.Center)или [`End`](xref:Xamarin.Forms.TextAlignment.Center)), чтобы управлять выравниванием всех строк абзаца. Значение по умолчанию — `Start` и выровняйте текст по левому краю.

Задайте для свойства [`VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment) элемент перечисления `TextAlignment`, чтобы разместить текст в верхней, Центральной или нижней части области, занятой `Label`.

Задайте для свойства [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) элемент перечисления [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) ([`WordWrap`](xref:Xamarin.Forms.LineBreakMode.WordWrap), [`CharacterWrap`](xref:Xamarin.Forms.LineBreakMode.CharacterWrap), [`NoWrap`](xref:Xamarin.Forms.LineBreakMode.NoWrap), [`HeadTruncation`](xref:Xamarin.Forms.LineBreakMode.HeadTruncation), [`MiddleTruncation`](xref:Xamarin.Forms.LineBreakMode.MiddleTruncation)или [`TailTruncation`](xref:Xamarin.Forms.LineBreakMode.TailTruncation)), чтобы управлять способом множественных строки в разрыве абзаца или усекаются.

## <a name="text-and-background-colors"></a>Цвета текста и фона

Задайте для свойств [`TextColor`](xref:Xamarin.Forms.Label.TextColor) и [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) `Label` значение [`Color`](xref:Xamarin.Forms.Color) , чтобы управлять цветом текста и фона.

`BackgroundColor` применяется к фону всей области, занятой `Label`. В зависимости от свойств `HorizontalOptions` и `VerticalOptions` этот размер может значительно превышать область, необходимую для вывода текста. Можно использовать цвет для экспериментов с различными значениями `HorizontalOptions`, `VerticalOptions`, `HorizontalExeAlignment`и `VerticalTextAlignment`, чтобы увидеть, как они влияют на размер и расположение `Label`, а также размер и расположение текста в `Label`.

## <a name="the-color-structure"></a>Цветовая структура

Структура [`Color`](xref:Xamarin.Forms.Color) позволяет указать цвета в виде значений красного-зеленого-синего (RGB) или оттенков-насыщенности (HSL) или с именем цвета. Для обозначения прозрачности также доступен альфа-канал.

Используйте конструктор `Color`, чтобы указать:

- [серая тень](xref:Xamarin.Forms.Color.%23ctor(System.Double))
- [значение RGB](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double))
- [значение RGB с прозрачностью](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double))

Аргументы — это `double` значения от 0 до 1.

Для создания `Color` значений можно также использовать несколько статических методов:

- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double)) для `double` значений RGB от 0 до 1
- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Int32,System.Int32,System.Int32)) для целочисленных значений RGB от 0 до 255
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Double,System.Double,System.Double,System.Double)) для `double` значений RGB с прозрачностью
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32)) для целочисленных значений RGB с прозрачностью
- [`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) для значений `double` HSL с прозрачностью
- [`Color.FromUint`](xref:Xamarin.Forms.Color.FromUint(System.UInt32)) для `uint` значения (B + 256 \* (G + 256 \* (R + 256 \* a)))
- [`Color.FromHex`](xref:Xamarin.Forms.Color.FromHex(System.String)) для формата `string` шестнадцатеричных цифр в форме "#AARRGGBB", "#RRGGBB" или "#ARGB" или "#RGB", где каждая буква соответствует шестнадцатеричной цифре для каналов альфа, красного, зеленого и синего. Этот метод является первичным для преобразования цветов XAML, как описано в [главе 7, XAML и Code](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md).

После создания `Color` значение является неизменяемым. Характеристики цвета можно получить из следующих свойств:

- [`R`](xref:Xamarin.Forms.Color.R)
- [`G`](xref:Xamarin.Forms.Color.G)
- [`B`](xref:Xamarin.Forms.Color.B)
- [`A`](xref:Xamarin.Forms.Color.A)
- [`Hue`](xref:Xamarin.Forms.Color.Hue)
- [`Saturation`](xref:Xamarin.Forms.Color.Saturation)
- [`Luminosity`](xref:Xamarin.Forms.Color.Luminosity)

Это все `double` значения от 0 до 1.

`Color` также определяет 240 открытых статических полей только для чтения для стандартных цветов. На момент написания книги был доступен только 17 распространенных цветов.

Другое общее статическое поле только для чтения определяет цвет, для которого все цветовые каналы имеют нулевое значение:

- [`Color.Transparent`](xref:Xamarin.Forms.Color.Transparent)

Несколько методов экземпляра позволяют изменять существующий цвет для создания нового цвета:

- [`AddLuminosity`](xref:Xamarin.Forms.Color.AddLuminosity(System.Double))
- [`MultiplyAlpha`](xref:Xamarin.Forms.Color.MultiplyAlpha(System.Double))
- [`WithHue`](xref:Xamarin.Forms.Color.WithHue(System.Double))
- [`WithLuminosity`](xref:Xamarin.Forms.Color.WithLuminosity(System.Double))
- [`WithSaturation`](xref:Xamarin.Forms.Color.WithSaturation(System.Double))

Наконец, два статических свойства только для чтения определяют особое значение цвета:

- [`Color.Default`](xref:Xamarin.Forms.Color.Default), для всех каналов задано значение &ndash;1
- [`Color.Accent`](xref:Xamarin.Forms.Color.Accent)

`Color.Default` предназначено для применения цветовой схемы платформы и, следовательно, имеет другое значение в разных контекстах на разных платформах. По умолчанию цветовые схемы платформы:

- iOS: темный текст на светлом фоне
- Android: светлый текст на темном фоне (в книге) или темной текст на светлом фоне (для дизайна материалов с помощью AppCompat в **главной** ветви репозитория образца кода)
- UWP: темный текст на светлом фоне

Значение `Color.Accent` приводит к цвету, определяемому платформой (и иногда выбираемым пользователем), который отображается на темном или светлом фоне.

## <a name="changing-the-application-color-scheme"></a>Изменение цветовой схемы приложения

Для различных платформ используется цветовая схема по умолчанию, как показано в списке выше.

При нацеливании на Android можно переключиться на неяркий схему, указав светлое тему в файле Android. manifest. XML или [добавив AppCompat и структуру материалов](~/xamarin-forms/platform/android/appcompat-material-design.md).

Для платформ Windows цветовая тема обычно выбирается пользователем, но можно добавить `RequestedTheme` атрибут, заданный как `Light`, так и `Dark` в файле App. XAML платформы. По умолчанию файл App. XAML в проекте UWP содержит атрибут `RequestedTheme`, для которого задано значение `Light`.

## <a name="font-sizes-and-attributes"></a>Размеры и атрибуты шрифта

Задайте для свойства [`FontFamily`](xref:Xamarin.Forms.Label.FontFamily) `Label` строку, например Times Roman, чтобы выбрать семейство шрифтов. Однако необходимо указать семейство шрифтов, которое поддерживается на конкретной платформе, и платформы в этом отношении не соответствуют друг другу.

Задайте для свойства [`FontSize`](xref:Xamarin.Forms.Label.FontSize) `Label` значение `double`, чтобы указать приблизительную высоту шрифта. Дополнительные сведения о выборе размеров шрифтов см. в [главе 5, посвященной работе с размерами](chapter05.md).

Можно также получить один из нескольких предварительно заданных размеров шрифтов, зависящих от платформы. Статический метод [`Device.GetNamedSize`](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type)) и [перегрузка](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,Xamarin.Forms.Element)) возвращают `double` значение размера шрифта, соответствующее платформе на основе членов перечисления [`NamedSize`](xref:Xamarin.Forms.NamedSize) ([`Default`](xref:Xamarin.Forms.NamedSize.Default), [`Micro`](xref:Xamarin.Forms.NamedSize.Micro), [`Small`](xref:Xamarin.Forms.NamedSize.Small), [`Medium`](xref:Xamarin.Forms.NamedSize.Medium)и [`Large`](xref:Xamarin.Forms.NamedSize.Large)). Значение, возвращаемое элементом `Medium`, не обязательно совпадает с `Default`. В образце [**намедфонтсизес**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) отображается текст с такими именованными размерами.

Задайте для свойства [`FontAttributes`](xref:Xamarin.Forms.Label.FontAttributes) `Label` элемент этих [`FontAttributes`](xref:Xamarin.Forms.FontAttributes) перечисления, [`Bold`](xref:Xamarin.Forms.FontAttributes.Bold), [`Italic`](xref:Xamarin.Forms.FontAttributes.Italic)или [`None`](xref:Xamarin.Forms.FontAttributes.None). Члены `Bold` и `Italic` можно объединять с помощью оператора C# побитового или.

## <a name="formatted-text"></a>Форматированный текст

Во всех примерах на данный момент весь текст, отображаемый `Label`, отформатирован единообразно. Чтобы изменить форматирование в текстовой строке, не устанавливайте свойство `Text` `Label`. Вместо этого задайте для свойства [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) объект типа [`FormattedString`](xref:Xamarin.Forms.FormattedString).

`FormattedString` имеет свойство [`Spans`](xref:Xamarin.Forms.FormattedString.Spans) , которое является коллекцией объектов [`Span`](xref:Xamarin.Forms.Span) . Каждый объект `Span` имеет собственные свойства [`Text`](xref:Xamarin.Forms.Span.Text), [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily), [`FontSize`](xref:Xamarin.Forms.Span.FontSize), [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes), [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor)и [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) .

Пример [**вариаблеформаттедтекст**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormText) демонстрирует использование свойства `FormattedText` для одной строки текста, а [**вариаблеформаттедпараграф**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormPara) демонстрирует прием всего абзаца, как показано ниже:

[![Тройной снимок экрана с переменным отформатированным абзацем](images/ch03fg06-small.png "Форматированный текст метки переменной")](images/ch03fg06-large.png#lightbox "Форматированный текст метки переменной")

Программа [**намедфонтсизес**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) использует один `Label` и объект `FormattedString` для вывода всех названий шрифтов для каждой платформы.

## <a name="related-links"></a>Связанные ссылки

- [Глава 3. полный текст (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [Глава 3. примеры](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [Глава 3 F# . примеры](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Метка](~/xamarin-forms/user-interface/text/label.md)
- [Работа с цветами](~/xamarin-forms/user-interface/colors.md)
