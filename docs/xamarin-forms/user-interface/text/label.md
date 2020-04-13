---
title: Этикетка Xamarin.Forms
description: В этой статье объясняется, как использовать класс Xamarin.Forms Label для отображения одно- и многолинейного текста в приложениях.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/09/2020
ms.openlocfilehash: bed6b8a774ecb352427f16b78d10e50821f92701
ms.sourcegitcommit: 765b69ed451a0f48625ea597c3f39de95f3ae693
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/09/2020
ms.locfileid: "80987599"
---
# <a name="xamarinforms-label"></a>Этикетка Xamarin.Forms

[![Скачать](~/media/shared/download.png) образец Скачать образец](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Отображение текста в Xamarin.Forms_

Представление [`Label`](xref:Xamarin.Forms.Label) используется для отображения текста, как одноименного, так и многолинейного. Этикетки могут иметь текстовые украшения, цветной текст и использовать пользовательские шрифты (семьи, размеры и опции).

## <a name="text-decorations"></a>Текстовые украшения

Подчеркивание и забастовочные [`Label`](xref:Xamarin.Forms.Label) текстовые украшения `Label.TextDecorations` могут быть `TextDecorations` применены к экземплярам, установив свойство для одного или нескольких участников перечисления:

- `None`
- `Underline`
- `Strikethrough`

Следующий пример XAML демонстрирует `Label.TextDecorations` настройку свойства:

```xaml
<Label Text="This is underlined text." TextDecorations="Underline"  />
<Label Text="This is text with strikethrough." TextDecorations="Strikethrough" />
<Label Text="This is underlined text with strikethrough." TextDecorations="Underline, Strikethrough" />
```

Эквивалентный код на C# выглядит так:

```csharp
var underlineLabel = new Label { Text = "This is underlined text.", TextDecorations = TextDecorations.Underline };
var strikethroughLabel = new Label { Text = "This is text with strikethrough.", TextDecorations = TextDecorations.Strikethrough };
var bothLabel = new Label { Text = "This is underlined text with strikethrough.", TextDecorations = TextDecorations.Underline | TextDecorations.Strikethrough };
```

На следующих скриншотах показаны `TextDecorations` участники [`Label`](xref:Xamarin.Forms.Label) перечисления, применяемые к экземплярам:

![Этикетки с текстовыми украшениями](label-images/label-textdecorations.png)

> [!NOTE]
> Текстовые украшения также могут [`Span`](xref:Xamarin.Forms.Span) быть применены к экземплярам. Для получения дополнительной `Span` информации о классе см. [Formatted Text](#Formatted_Text)

## <a name="character-spacing"></a>Интервалы между символами

Расстояние между персонажами может [`Label`](xref:Xamarin.Forms.Label) быть применено `Label.CharacterSpacing` к `double` экземплярам, установив свойство к значению:

```xaml
<Label Text="Character spaced text"
       CharacterSpacing="10" />
```

Эквивалентный код на C# выглядит так:

```csharp
Label label = new Label { Text = "Character spaced text", CharacterSpacing = 10 };
```

Результатом является то, что символы [`Label`](xref:Xamarin.Forms.Label) в тексте отображается расставлены `CharacterSpacing` устройства-независимых единиц друг от друга.

## <a name="new-lines"></a>Символы перевода строки

Есть два основных метода [`Label`](xref:Xamarin.Forms.Label) для принуждения текста в новой строке, от XAML:

1. Используйте символ подачи линии unicode, который является "&#10;".
1. Укажите текст с помощью синтаксиса *элемента свойства.*

В следующем коде показан пример обоих методов:

```xaml
<!-- Unicode line feed character -->
<Label Text="First line &#10; Second line" />

<!-- Property element syntax -->
<Label>
    <Label.Text>
        First line
        Second line
    </Label.Text>
</Label>
```

В C, текст может быть вытеснен на новую строку с символом «зн»:

```csharp
Label label = new Label { Text = "First line\nSecond line" };
```

## <a name="colors"></a>Цвета

Этикетки могут быть установлены для использования пользовательского цвета текста через связываемые [`TextColor`](xref:Xamarin.Forms.Label.TextColor) свойства.

Особая осторожность необходима для обеспечения того, чтобы цвета были пригодны для удостаивания на каждой платформе. Поскольку каждая платформа имеет различные значения по умолчанию для цветов текста и фона, вам нужно быть осторожным, чтобы выбрать по умолчанию, который работает на каждом.

Следующий пример XAML устанавливает цвет `Label`текста:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo">
    <StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a green label." />
    </StackLayout>
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();

        var layout = new StackLayout { Padding = new Thickness(5,10) };
        var label = new Label { Text="This is a green label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
        layout.Children.Add(label);
        this.Content = layout;
    }
}
```

Следующие скриншоты показывают результат `TextColor` настройки свойства:

![Пример этикетки TextColor](label-images/textcolor.png)

Для получения дополнительной информации о цветах, [см.](~/xamarin-forms/user-interface/colors.md)

## <a name="fonts"></a>Шрифты

Для получения дополнительной информации об `Label`указании шрифтов на, см. [Fonts](~/xamarin-forms/user-interface/text/fonts.md)

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>Трубация и упаковка

Этикетки могут быть настроены для обработки текста, который не может поместиться на одной строке одним из нескольких способов, подверженных свойству. `LineBreakMode` [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode)это перечисление со следующими значениями:

- **HeadTruncation** &ndash; усечения головы текста, показывая конец.
- **CharacterWrap** &ndash; заворачивает текст на новую строку на границе символов.
- **MiddleTruncation** &ndash; отображает начало и конец текста, при этом середина заменяется эллипсисом.
- **NoWrap** &ndash; не заворачивает текст, отображая только столько текста, сколько может поместиться на одной строке.
- **TailTruncation** &ndash; показывает начало текста, усечение конца.
- **WordWrap** &ndash; обертывает текст на границе слова.

## <a name="display-a-specific-number-of-lines"></a>Отобразить определенное количество строк

Количество строк, отображаемых a, [`Label`](xref:Xamarin.Forms.Label) может быть определено путем установки `Label.MaxLines` свойства на `int` значение:

- Когда `MaxLines` -1, что является его `Label` значением по умолчанию, уважает значение [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) свойства либо показать только одну строку, возможно, усеченную, или все строки со всем текстом.
- Когда `MaxLines` 0, `Label` не отображается.
- Когда `MaxLines` 1, результат идентичен [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) настройке [`HeadTruncation`](xref:Xamarin.Forms.LineBreakMode) [`MiddleTruncation`](xref:Xamarin.Forms.LineBreakMode)свойства, [`TailTruncation`](xref:Xamarin.Forms.LineBreakMode)или [`NoWrap`](xref:Xamarin.Forms.LineBreakMode). Тем не `Label` менее, будет [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) уважать стоимость имущества в отношении размещения эллипсиса, если это применимо.
- Когда `MaxLines` больше, чем `Label` 1, будет отображаться до указанного количества строк, [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) при соблюдении стоимости имущества в отношении размещения эллипсис, если это применимо. Однако установка `MaxLines` свойства на значение, превышающее [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) 1, не [`NoWrap`](xref:Xamarin.Forms.LineBreakMode)имеет эффекта, если свойство настроено на.

Следующий пример XAML демонстрирует `MaxLines` настройку [`Label`](xref:Xamarin.Forms.Label)свойства на:

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       MaxLines="2" />
```

Эквивалентный код на C# выглядит так:

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  MaxLines = 2
};
```

Следующие скриншоты показывают результат `MaxLines` настройки свойства до 2, когда текст достаточно долго, чтобы занять более 2 строк:

![Пример этикетки MaxLines](label-images/label-maxlines.png)

## <a name="display-html"></a>Отображение HTML

Класс [`Label`](xref:Xamarin.Forms.Label) имеет `TextType` свойство, которое `Label` определяет, должен ли экземпляр отображать простой текст или HTML-текст. Это свойство должно быть установлено `TextType` для одного из членов перечисления:

- `Text`указывает на `Label` то, что будет отображаться простой текст, и значение по умолчанию `Label.TextType` свойства.
- `Html`указывает на `Label` то, что будет отображаться HTML текст.

Таким [`Label`](xref:Xamarin.Forms.Label) образом, экземпляры могут `Label.TextType` отображать `Html`HTML, `Label.Text` установив свойство на строку HTML:

```csharp
Label label = new Label
{
    Text = "This is <strong style=\"color:red\">HTML</strong> text.",
    TextType = TextType.Html
};
```

В приведенном выше примере, двойные символы цитаты `\` в HTML должны быть избежаны с помощью символа.

В XAML HTML строки могут стать нечитаемыми из-за дополнительного побега `<` и `>` символов:

```xaml
<Label Text="This is &lt;strong style=&quot;color:red&quot;&gt;HTML&lt;/strong&gt; text."
       TextType="Html"  />
```

Кроме того, для большей читаемости HTML `CDATA` может быть выровнен в разделе:

```xaml
<Label TextType="Html">
    <![CDATA[
    This is <strong style="color:red">HTML</strong> text.
    ]]>
</Label>
```

В этом примере свойство `Label.Text` устанавливается на строку HTML, которая встроена `CDATA` в раздел. Это работает, `Text` потому `ContentProperty` что `Label` свойство для класса.

На следующих скриншотах отображается [`Label`](xref:Xamarin.Forms.Label) HTML:

![Скриншоты этикетки, отображающей HTML, на iOS и Android](label-images/label-html.png)

> [!IMPORTANT]
> Отображение [`Label`](xref:Xamarin.Forms.Label) HTML в а ограничено HTML-тегами, поддерживаемыми базовой платформой.

<a name="Formatted_Text" />

## <a name="formatted-text"></a>Форматированный текст

Этикетки разоблачают свойство, [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) позволяющее представить текст несколькими шрифтами и цветами в одном представлении.

Свойство `FormattedText` типа [`FormattedString`](xref:Xamarin.Forms.FormattedString), который включает в [`Span`](xref:Xamarin.Forms.Span) себя один или [`Spans`](xref:Xamarin.Forms.FormattedString.Spans) несколько экземпляров, установленных через свойство. Следующие `Span` свойства могут быть использованы для установки внешнего вида:

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor)- цвет фона диапазона.
- `CharacterSpacing` с типом `double` представляет собой интервал между знаками текста `Span`.
- [`Font`](xref:Xamarin.Forms.Span.Font)- шрифт для текста в пролете.
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes)— атрибуты шрифта для текста в диапазоне.
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily)- семейство шрифтов, к которому принадлежит шрифт для текста в диапазоне.
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize)- размер шрифта для текста в пролете.
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor)- цвет текста в пролете. Это свойство устарело и `TextColor` было заменено свойством.
- [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight)- множитель для применения к высоте линии по умолчанию диапазона. Для получения дополнительной информации [см.](#line-height)
- [`Style`](xref:Xamarin.Forms.Span.Style)- стиль, чтобы применить к пролету.
- [`Text`](xref:Xamarin.Forms.Span.Text)- текст пролета.
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor)- цвет текста в пролете.
- `TextDecorations`- украшения для применения к тексту в пролете. Для получения дополнительной [информации,](#text-decorations)см Текст украшения .

В [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) [`Text`](xref:Xamarin.Forms.Span.Text), [`Text`](xref:Xamarin.Forms.Span.Text) и связываемые свойства [`OneWay`](xref:Xamarin.Forms.BindingMode)имеют режим связывания по умолчанию . Для получения дополнительной информации об этом режиме [Binding Mode](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md) связывания [см.](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#the-default-binding-mode)

Кроме того, [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) свойство может быть использовано для определения коллекции распознавания [`Span`](xref:Xamarin.Forms.Span)жестов, которые будут реагировать на жесты на .

> [!NOTE]
> Отобразить HTML в [`Span`](xref:Xamarin.Forms.Span).

Следующий пример XAML `FormattedText` демонстрирует свойство, [`Span`](xref:Xamarin.Forms.Span) состоящее из трех экземпляров:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo - XAML">
    <StackLayout Padding="5,10">
        ...
        <Label LineBreakMode="WordWrap">
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Red Bold, " TextColor="Red" FontAttributes="Bold" />
                    <Span Text="default, " Style="{DynamicResource BodyStyle}">
                        <Span.GestureRecognizers>
                            <TapGestureRecognizer Command="{Binding TapCommand}" />
                        </Span.GestureRecognizers>
                    </Span>
                    <Span Text="italic small." FontAttributes="Italic" FontSize="Small" />
                </FormattedString>
            </Label.FormattedText>
        </Label>
    </StackLayout>
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
public class LabelPageCode : ContentPage
{
    public LabelPageCode ()
    {
        var layout = new StackLayout{ Padding = new Thickness (5, 10) };
        ...
        var formattedString = new FormattedString ();
        formattedString.Spans.Add (new Span{ Text = "Red bold, ", ForegroundColor = Color.Red, FontAttributes = FontAttributes.Bold });

        var span = new Span { Text = "default, " };
        span.GestureRecognizers.Add(new TapGestureRecognizer { Command = new Command(async () => await DisplayAlert("Tapped", "This is a tapped Span.", "OK")) });
        formattedString.Spans.Add(span);
        formattedString.Spans.Add (new Span { Text = "italic small.", FontAttributes = FontAttributes.Italic, FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)) });

        layout.Children.Add (new Label { FormattedText = formattedString });
        this.Content = layout;
    }
}
```

> [!IMPORTANT]
> Свойство [`Text`](xref:Xamarin.Forms.Span.Text) `Span` может быть установлено с помощью связывания данных. Более подробную информацию см. в разделе [Привязка данных](~/xamarin-forms/app-fundamentals/data-binding/index.md).

Обратите внимание, что a [`Span`](xref:Xamarin.Forms.Span) также может реагировать на [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) любые жесты, которые добавляются в коллекцию диапазона. Например, [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) a был добавлен `Span` ко второму в приведенных выше примерах кода. Таким образом, `Span` когда `TapGestureRecognizer` это постучал будет реагировать, исполняя `ICommand` определяется [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) свойством. Для получения дополнительной информации о распознавании жестов, см [Xamarin.Forms жесты](~/xamarin-forms/app-fundamentals/gestures/index.md).

Следующие скриншоты показывают результат `FormattedString` настройки `Span` свойства до трех экземпляров:

![Пример formattedText этикетки](label-images/formattedtext.png)

## <a name="line-height"></a>Высота линии

Вертикальная высота [`Label`](xref:Xamarin.Forms.Label) и [`Span`](xref:Xamarin.Forms.Span) может быть настроена [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) путем настройки свойства или [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) `double` значения. На iOS и Android эти значения являются мультипликаторами первоначальной высоты `Label.LineHeight` линии, а на универсальной платформе Windows (UWP) значение свойства является множественником размера шрифта этикетки.

> [!NOTE]
>
> - На iOS, [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) и свойства изменить высоту строки текста, который помещается на одной строке, и текст, который обертывания на несколько строк.
> - На Android, [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) и свойства только изменить высоту линии текста, который обертывания на несколько строк.
> - На UWP [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) свойство изменяет высоту строки текста, которая [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) обертывается на несколько строк, и свойство не имеет эффекта.

Следующий пример XAML демонстрирует [`LineHeight`](xref:Xamarin.Forms.Label.LineHeight) настройку [`Label`](xref:Xamarin.Forms.Label)свойства на:

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       LineHeight="1.8" />
```

Эквивалентный код на C# выглядит так:

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  LineHeight = 1.8
};
```

Следующие скриншоты показывают результат [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) установки свойства до 1.8:

![Пример линии этикетки](label-images/label-lineheight.png)

Следующий пример XAML демонстрирует [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight) настройку [`Span`](xref:Xamarin.Forms.Span)свойства на:

```xaml
<Label LineBreakMode="WordWrap">
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In a tincidunt sem. Phasellus mollis sit amet turpis in rutrum. Sed aliquam ac urna id scelerisque. "
                  LineHeight="1.8"/>
            <Span Text="Nullam feugiat sodales elit, et maximus nibh vulputate id."
                  LineHeight="1.8" />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

Эквивалентный код на C# выглядит так:

```csharp
var formattedString = new FormattedString();
formattedString.Spans.Add(new Span
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In a tincidunt sem. Phasellus mollis sit amet turpis in rutrum. Sed aliquam ac urna id scelerisque. ",
  LineHeight = 1.8
});
formattedString.Spans.Add(new Span
{
  Text = "Nullam feugiat sodales elit, et maximus nibh vulputate id.",
  LineHeight = 1.8
});
var label = new Label
{
  FormattedText = formattedString,
  LineBreakMode = LineBreakMode.WordWrap
};
```

Следующие скриншоты показывают результат [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) установки свойства до 1.8:

![Пример линии Span](label-images/span-lineheight.png)

## <a name="padding"></a>Заполнение

Обивка представляет пространство между элементом и его элементами ребенка и используется для отделения элемента от собственного содержимого. Обивка может [`Label`](xref:Xamarin.Forms.Label) быть применена `Label.Padding` к [`Thickness`](xref:Xamarin.Forms.Thickness) экземплярам, установив свойство к значению:

```xaml
<Label Padding="10">
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Lorem ipsum" />
            <Span Text="dolor sit amet." />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

Эквивалентный код на C# выглядит так:

```csharp
FormattedString formattedString = new FormattedString();
formattedString.Spans.Add(new Span
{
  Text = "Lorem ipsum"
});
formattedString.Spans.Add(new Span
{
  Text = "dolor sit amet."
});
Label label = new Label
{
    FormattedText = formattedString,
    Padding = new Thickness(20)
};
```

> [!IMPORTANT]
> На iOS, [`Label`](xref:Xamarin.Forms.Label) когда создается `Padding` свойство, будет применена обивка, а значение заполнения может быть обновлено позже. Однако при `Label` создании создается, что `Padding` не устанавливает свойство, попытка установить его позже не будет иметь никакого эффекта.
>
> На Android и универсальной `Padding` платформе Windows значение свойства `Label` может быть определено при создании или позже.

Для получения дополнительной информации о обивке, [см.](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)

## <a name="hyperlinks"></a>Гиперссылки

Текст, отображаемый экземплярами, [`Label`](xref:Xamarin.Forms.Label) [`Span`](xref:Xamarin.Forms.Span) может быть превращен в гиперссылки со следующим подходом:

1. Установите `TextColor` `TextDecoration` и свойства [`Label`](xref:Xamarin.Forms.Label) [`Span`](xref:Xamarin.Forms.Span)или .
1. Добавить [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) в [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) коллекцию [`Label`](xref:Xamarin.Forms.Label) [`Span`](xref:Xamarin.Forms.Span)или [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) , чье свойство связывается с `ICommand`, и чье [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) свойство содержит URL, чтобы открыть.
1. Определите, `ICommand` что будет выполнено [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer).
1. Напишите код, который будет `ICommand`выполнен .

На следующем примере кода, взятом из образца [Hyperlink Demos,](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/) показан о содержимом [`Label`](xref:Xamarin.Forms.Label) которого устанавливается из нескольких [`Span`](xref:Xamarin.Forms.Span) экземпляров:

```xaml
<Label>
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Alternatively, click " />
            <Span Text="here"
                  TextColor="Blue"
                  TextDecorations="Underline">
                <Span.GestureRecognizers>
                    <TapGestureRecognizer Command="{Binding TapCommand}"
                                          CommandParameter="https://docs.microsoft.com/xamarin/" />
                </Span.GestureRecognizers>
            </Span>
            <Span Text=" to view Xamarin documentation." />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

В этом примере первая и третья [`Span`](xref:Xamarin.Forms.Span) экземпляры `Span` включают текст, а второй представляет собой tappable гиперссылку. Он имеет свой цвет текста набор синий, и имеет подчеркнутое оформление текста. Это создает видимость гиперссылки, как показано на следующих скриншотах:

[![Гиперссылки](label-images/hyperlinks-small.png "Гиперссылки")](label-images/hyperlinks-large.png#lightbox)

При нажатии гиперссылки [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) будет реагировать, исполняя `ICommand` определяемое ее [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) свойством. Кроме того, URL, [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) указанный свойством, `ICommand` будет передан в качестве параметра.

Закодирование для страницы XAML `TapCommand` содержит реализацию:

```csharp
public partial class MainPage : ContentPage
{
    // Launcher.OpenAsync is provided by Xamarin.Essentials.
    public ICommand TapCommand => new Command<string>(async (url) => await Launcher.OpenAsync(url));

    public MainPage()
    {
        InitializeComponent();
        BindingContext = this;
    }
}
```

Выполняет `TapCommand` `Launcher.OpenAsync` метод, передавая [`TapGestureRecognizer.CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) значение свойства в качестве параметра. Метод `Launcher.OpenAsync` предоставляется Xamarin.Essentials, и открывает URL в веб-браузере. Таким образом, общий эффект заключается в том, что при нажатии гиперссылки на странице появляется веб-браузер и отображается URL,связанный с гиперссылкой.

### <a name="creating-a-reusable-hyperlink-class"></a>Создание многоразового класса гиперссылки

Предыдущий подход к созданию гиперссылки требует написания повторяющихся кодов каждый раз, когда требуется гиперссылка в приложении. Тем не [`Label`](xref:Xamarin.Forms.Label) менее, как классы, так и [`Span`](xref:Xamarin.Forms.Span) классы могут быть подклассными для создания `HyperlinkLabel` и `HyperlinkSpan` классов, с добавлением там распознавания жестов и кода форматирования текста.

Следующий пример кода, взятый из образца `HyperlinkSpan` [Hyperlink Demos,](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/) показывает класс:

```csharp
public class HyperlinkSpan : Span
{
    public static readonly BindableProperty UrlProperty =
        BindableProperty.Create(nameof(Url), typeof(string), typeof(HyperlinkSpan), null);

    public string Url
    {
        get { return (string)GetValue(UrlProperty); }
        set { SetValue(UrlProperty, value); }
    }

    public HyperlinkSpan()
    {
        TextDecorations = TextDecorations.Underline;
        TextColor = Color.Blue;
        GestureRecognizers.Add(new TapGestureRecognizer
        {
            // Launcher.OpenAsync is provided by Xamarin.Essentials.
            Command = new Command(async () => await Launcher.OpenAsync(Url))
        });
    }
}
```

Класс `HyperlinkSpan` определяет свойство `Url` и [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)связанное с ним свойство, а [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) конструктор устанавливает внешний вид гиперссылки и то, что будет реагировать при нажатии гиперссылки. `HyperlinkSpan` При нажатии будет `TapGestureRecognizer` ответчик, выражая `Launcher.OpenAsync` метод открытия URL-адреса, `Url` указанный свойством, в веб-браузере.

Класс `HyperlinkSpan` можно использовать, добавив экземпляр класса в XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:HyperlinkDemo"
             x:Class="HyperlinkDemo.MainPage">
    <StackLayout>
        ...
        <Label>
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Alternatively, click " />
                    <local:HyperlinkSpan Text="here"
                                         Url="https://docs.microsoft.com/appcenter/" />
                    <Span Text=" to view AppCenter documentation." />
                </FormattedString>
            </Label.FormattedText>
        </Label>
    </StackLayout>
</ContentPage>
```

## <a name="styling-labels"></a>Укладка этикетки

Предыдущие разделы [`Label`](xref:Xamarin.Forms.Label) охватывали настройки и [`Span`](xref:Xamarin.Forms.Span) свойства на основе в каждом экземпляре. Однако наборы свойств можно сгруппировать в один стиль, который постоянно применяется к одному или многим представлениям. Это может повысить читаемость кода и упростить реализацию изменений в дизайне. Для получения дополнительной информации, см [Стили](~/xamarin-forms/user-interface/text/styles.md).

## <a name="related-links"></a>Связанные ссылки

- [Текст (образец)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Гиперссылки (образец)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks)
- [Создание мобильных приложений с помощью Xamarin.Forms, глава 3](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [API Label](xref:Xamarin.Forms.Label)
- [Span API](xref:Xamarin.Forms.Span)
