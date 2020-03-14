---
title: Метка Xamarin.Forms
description: В этой статье объясняется, как использовать класс Xamarin.Forms метки для отображения одного или нескольких строк текста в приложениях.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/28/2019
ms.openlocfilehash: 64fa15a15468a84ada3a377a9ac85bbf6310099c
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305591"
---
# <a name="xamarinforms-label"></a>Метка Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Отображение текста в Xamarin. Forms_

Представление [`Label`](xref:Xamarin.Forms.Label) используется для отображения текста как с одним, так и с несколькими строками. Метки можно иметь оформление текста, цветным текстом и использовать пользовательские шрифты (семейств, размеры и параметры).

## <a name="text-decorations"></a>Оформление текста

Оформление текста с подчеркиванием и зачеркиванием можно применять к экземплярам [`Label`](xref:Xamarin.Forms.Label) , присвоив свойству `Label.TextDecorations` один или несколько `TextDecorations` членов перечисления:

- `None`
- `Underline`
- `Strikethrough`

В следующем примере XAML показано задание свойства `Label.TextDecorations`:

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

На следующих снимках экрана показаны элементы перечисления `TextDecorations`, применяемые к экземплярам [`Label`](xref:Xamarin.Forms.Label) .

![Метки с оформлением текста](label-images/label-textdecorations.png)

> [!NOTE]
> Оформление текста можно также применить к экземплярам [`Span`](xref:Xamarin.Forms.Span) . Дополнительные сведения о классе `Span` см. в разделе [форматированный текст](#Formatted_Text).

## <a name="character-spacing"></a>Интервал между символами

Интервалы между символами можно применять к экземплярам [`Label`](xref:Xamarin.Forms.Label) , присвоив свойству `Label.CharacterSpacing` значение `double`.

```xaml
<Label Text="Character spaced text"
       CharacterSpacing="10" />
```

Эквивалентный код на C# выглядит так:

```csharp
Label label = new Label { Text = "Character spaced text", CharacterSpacing = 10 };
```

Результат заключается в том, что символы в тексте, отображаемом [`Label`](xref:Xamarin.Forms.Label) , размещаются `CharacterSpacing` независимыми от устройства единицами.

## <a name="colors"></a>Цвета

Метки можно настроить для использования пользовательского цвета текста с помощью свойства BIND [`TextColor`](xref:Xamarin.Forms.Label.TextColor) .

Особое внимание необходимо, чтобы гарантировать, что цвета будет доступна для использования на каждой платформе. Так как каждая платформа имеет различные значения по умолчанию для цветов текста и фона, необходимо соблюдать осторожность выбрать умолчания, которое работает на каждом.

В следующем примере XAML задается цвет текста `Label`:

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

На следующих снимках экрана показан результат установки свойства `TextColor`.

![Пример метки TextColor](label-images/textcolor.png)

Дополнительные сведения о цветах см. в разделе [цвета](~/xamarin-forms/user-interface/colors.md).

## <a name="fonts"></a>Шрифты

Дополнительные сведения об указании шрифтов в `Label`см. в разделе [шрифты](~/xamarin-forms/user-interface/text/fonts.md).

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>Усечение и поддерживающего перенос

Метки могут быть настроены на обработку текста, который не может поместиться в одну строку одним из нескольких способов, предоставляемых свойством `LineBreakMode`. [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) является перечислением со следующими значениями:

- **Хеадтрункатион** &ndash; усекает заголовок текста, отображая его конец.
- **Чарактерврап** &ndash; заключает текст на новую строку с границей символа.
- **Миддлетрункатион** &ndash; отображает начало и конец текста, а середина заменяется многоточием.
- &ndash; без **переноса** не переносит текст, отображая всего столько же текста, сколько может уместиться в одной строке.
- **Таилтрункатион** &ndash; показывает начало текста с усечением конца.
- **WordWrap** &ndash; заключает текст на границе слова.

## <a name="display-a-specific-number-of-lines"></a>Отображение определенного числа строк

Число строк, отображаемых [`Label`](xref:Xamarin.Forms.Label) , можно указать, задав для свойства `Label.MaxLines` значение `int`.

- Если `MaxLines` равно-1, то есть значение по умолчанию, `Label` учитывает значение свойства [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) , чтобы отображалась только одна строка, возможно, обрезанная или все строки со всем текстом.
- Если значение `MaxLines` равно 0, `Label` не отображается.
- Если `MaxLines` равен 1, то результат будет совпадать с установкой свойства [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) в значение [`NoWrap`](xref:Xamarin.Forms.LineBreakMode), [`HeadTruncation`](xref:Xamarin.Forms.LineBreakMode), [`MiddleTruncation`](xref:Xamarin.Forms.LineBreakMode)или [`TailTruncation`](xref:Xamarin.Forms.LineBreakMode). Однако `Label` будет учитывать значение свойства [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) в отношении размещения многоточия (если применимо).
- Если `MaxLines` больше 1, `Label` отображает указанное число строк и учитывает значение свойства [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) в отношении размещения многоточия (если применимо). Однако если для свойства [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) задано значение [`NoWrap`](xref:Xamarin.Forms.LineBreakMode), то установка свойства `MaxLines` равным значению больше 1 не имеет смысла.

В следующем примере XAML показано задание свойства `MaxLines` в [`Label`](xref:Xamarin.Forms.Label):

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

На следующих снимках экрана показан результат установки свойства `MaxLines` равным 2, если текст достаточно длинный, чтобы занимать более 2 строк:

![Пример метки Макслинес](label-images/label-maxlines.png)

## <a name="display-html"></a>Отображать HTML

Класс [`Label`](xref:Xamarin.Forms.Label) имеет свойство `TextType`, которое определяет, должен ли экземпляр `Label` отображать обычный текст или HTML-текст. Этому свойству следует присвоить один из членов перечисления `TextType`:

- `Text` указывает, что `Label` будет отображать обычный текст и является значением по умолчанию свойства `Label.TextType`.
- `Html` указывает, что `Label` будет отображать HTML-текст.

Таким образом, [`Label`](xref:Xamarin.Forms.Label) экземпляры могут отображать HTML, присвоив свойству `Label.TextType` значение `Html`, а свойству `Label.Text` — строке HTML:

```csharp
Label label = new Label
{
    Text = "This is <strong style=\"color:red\">HTML</strong> text.",
    TextType = TextType.Html
};
```

В приведенном выше примере символы двойной кавычки в HTML должны быть экранированы с помощью символа `\`.

В XAML строки HTML могут стать нечитаемыми из-за дополнительного экранирования `<` и `>` символов:

```xaml
<Label Text="This is &lt;strong style=&quot;color:red&quot;&gt;HTML&lt;/strong&gt; text."
       TextType="Html"  />
```

Кроме того, для повышения удобочитаемости HTML может быть встроен в `CDATA` разделе:

```xaml
<Label TextType="Html">
    <![CDATA[
    This is <strong style="color:red">HTML</strong> text.
    ]]>
</Label>
```

В этом примере свойству `Label.Text` присваивается строка HTML, встроенная в раздел `CDATA`. Это работает потому, что свойство `Text` является `ContentProperty`ом класса `Label`.

На следующих снимках экрана показан [`Label`](xref:Xamarin.Forms.Label) , отображающий HTML:

![Снимки экрана метки, отображающей HTML, в iOS и Android](label-images/label-html.png)

> [!IMPORTANT]
> Отображение HTML в [`Label`](xref:Xamarin.Forms.Label) ограничено HTML-тегами, которые поддерживаются базовой платформой.

<a name="Formatted_Text" />

## <a name="formatted-text"></a>Форматированный текст

Метки предоставляют свойство [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) , которое позволяет просматривать текст с несколькими шрифтами и цветами в одном и том же представлении.

Свойство `FormattedText` имеет тип [`FormattedString`](xref:Xamarin.Forms.FormattedString), который состоит из одного или нескольких экземпляров [`Span`](xref:Xamarin.Forms.Span) , задается через свойство [`Spans`](xref:Xamarin.Forms.FormattedString.Spans) . Для настройки внешнего вида можно использовать следующие свойства `Span`.

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) — цвет фона диапазона.
- `CharacterSpacing` с типом `double` представляет собой интервал между знаками текста `Span`.
- [`Font`](xref:Xamarin.Forms.Span.Font) — шрифт текста в диапазоне.
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes) — атрибуты шрифта для текста в диапазоне.
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily) — семейство шрифтов, которому принадлежит шрифт текста в диапазоне.
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize) — размер шрифта для текста в диапазоне.
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor) — цвет текста в диапазоне. Это свойство устарело и было заменено свойством `TextColor`.
- [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight) — коэффициент, применяемый к высоте строки по умолчанию для диапазона. Дополнительные сведения см. в разделе [Height строки](#line-height).
- [`Style`](xref:Xamarin.Forms.Span.Style) — стиль, применяемый к диапазону.
- [`Text`](xref:Xamarin.Forms.Span.Text) — текст диапазона.
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor) — цвет текста в диапазоне.
- `TextDecorations` — Оформление, применяемое к тексту в диапазоне. Дополнительные сведения см. в разделе [Оформление текста](#text-decorations).

Свойства [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor), [`Text`](xref:Xamarin.Forms.Span.Text)и [`Text`](xref:Xamarin.Forms.Span.Text) BIND имеют режим привязки по умолчанию [`OneWay`](xref:Xamarin.Forms.BindingMode). Дополнительные сведения об этом режиме привязки см. в описании [режима привязки по умолчанию](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#the-default-binding-mode) в разделе " [режим привязки](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md) ".

Кроме того, свойство [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) можно использовать для определения коллекции распознавателей жестов, которые будут реагировать на жесты на [`Span`](xref:Xamarin.Forms.Span).

> [!NOTE]
> Невозможно отобразить HTML в [`Span`](xref:Xamarin.Forms.Span).

В следующем примере XAML показано `FormattedText` свойство, состоящее из трех экземпляров [`Span`](xref:Xamarin.Forms.Span) :

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
> Свойство [`Text`](xref:Xamarin.Forms.Span.Text) `Span` можно задать с помощью привязки данных. Более подробную информацию см. в разделе [Привязка данных](~/xamarin-forms/app-fundamentals/data-binding/index.md).

Обратите внимание, что [`Span`](xref:Xamarin.Forms.Span) может также реагировать на любые жесты, добавленные в коллекцию [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) диапазона. Например, к второй `Span` в приведенных выше примерах кода был добавлен [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) . Таким образом, при нажатии этой `Span` `TapGestureRecognizer` будет реагировать на исполнение `ICommand`, определенного свойством [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) . Дополнительные сведения о распознавателях жестов см. в разделе [жесты Xamarin. Forms](~/xamarin-forms/app-fundamentals/gestures/index.md).

На следующих снимках экрана показан результат установки свойства `FormattedString` в три экземпляра `Span`:

![Пример метки FormattedText](label-images/formattedtext.png)

## <a name="line-height"></a>Высота линии

Вертикальную высоту [`Label`](xref:Xamarin.Forms.Label) и [`Span`](xref:Xamarin.Forms.Span) можно настроить, задав для свойства [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) или [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) значение `double`. В iOS и Android эти значения являются множителями исходной высоты строки, а на универсальная платформа Windows (UWP) значение свойства `Label.LineHeight` является множителем размера шрифта метки.

> [!NOTE]
>
> - В iOS свойства [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) и [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) изменяют высоту строки текста, находящуюся в одной строке, и текст, который переносится на несколько строк.
> - В Android свойства [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) и [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) изменяют только высоту строки текста, которая переносится на несколько строк.
> - В UWP свойство [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) изменяет высоту строки текста, которая переносится на несколько строк, а свойство [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) не оказывает никакого влияния.

В следующем примере XAML показано задание свойства [`LineHeight`](xref:Xamarin.Forms.Label.LineHeight) в [`Label`](xref:Xamarin.Forms.Label):

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

На следующих снимках экрана показан результат установки свойства [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) в значение 1,8:

![Пример метки LineHeight](label-images/label-lineheight.png)

В следующем примере XAML показано задание свойства [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight) в [`Span`](xref:Xamarin.Forms.Span):

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

На следующих снимках экрана показан результат установки свойства [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) в значение 1,8:

![Пример span LineHeight](label-images/span-lineheight.png)

## <a name="padding"></a>Заполнение

Заполнение представляет пространство между элементом и его дочерними элементами и используется для отделения элемента от его собственного содержимого. Заполнение можно применять к экземплярам [`Label`](xref:Xamarin.Forms.Label) , присвоив свойству `Label.Padding` значение [`Thickness`](xref:Xamarin.Forms.Thickness) .

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
> В iOS при создании [`Label`](xref:Xamarin.Forms.Label) , который задает свойство `Padding`, будет применено заполнение, а значение заполнения можно будет обновить позже. Однако при создании `Label`, который не задает свойство `Padding`, попытка его установки в дальнейшем не повлияет.
>
> В Android и универсальная платформа Windows значение свойства `Padding` может быть указано при создании `Label` или более поздней версии.

Дополнительные сведения о заполнении см. в разделе [поля и заполнение](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

## <a name="hyperlinks"></a>Гиперссылки

Текст, отображаемый [`Label`](xref:Xamarin.Forms.Label) и [`Span`](xref:Xamarin.Forms.Span) экземпляры, можно преобразовать в гиперссылки с помощью следующего подхода:

1. Задайте свойства `TextColor` и `TextDecoration` [`Label`](xref:Xamarin.Forms.Label) или [`Span`](xref:Xamarin.Forms.Span).
1. Добавьте [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) в коллекцию [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) [`Label`](xref:Xamarin.Forms.Label) или [`Span`](xref:Xamarin.Forms.Span), свойство [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) которой привязано к `ICommand`, а свойство [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) содержит открываемый URL-адрес.
1. Определите `ICommand`, которые будут выполняться [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer).
1. Напишите код, который будет выполнен `ICommand`.

В следующем примере кода, взятом из примера [демонстрации гиперссылок](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/) , показан [`Label`](xref:Xamarin.Forms.Label) , содержимое которого задается из нескольких экземпляров [`Span`](xref:Xamarin.Forms.Span) :

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

В этом примере первый и третий экземпляры [`Span`](xref:Xamarin.Forms.Span) состоят из текста, а вторая `Span` представляет собой гиперссылку с касанием. Цвет текста установлен синим цветом и имеет подчеркивание текста. При этом создается внешний вид гиперссылки, как показано на следующих снимках экрана:

[![Гиперссылки](label-images/hyperlinks-small.png "Гиперссылки")](label-images/hyperlinks-large.png#lightbox)

При нажатии этой гиперссылки [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) будет реагировать, выполнив `ICommand`, определенный свойством [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) . Кроме того, URL-адрес, заданный свойством [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) , будет передан в `ICommand` в качестве параметра.

Код программной части для страницы XAML содержит реализацию `TapCommand`:

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

`TapCommand` выполняет метод `Launcher.OpenAsync`, передавая значение свойства [`TapGestureRecognizer.CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) в качестве параметра. Метод `Launcher.OpenAsync` предоставляется Xamarin. Essentials и открывается в веб-браузере. Таким образом, общий результат заключается в том, что при касании гиперссылки на странице появляется веб-браузер, и открывается URL-адрес, связанный с гиперссылкой.

### <a name="creating-a-reusable-hyperlink-class"></a>Создание повторно используемого класса гиперссылки

Предыдущий подход к созданию гиперссылки требует написания повторяющегося кода каждый раз, когда требуется гиперссылка в приложении. Однако классы [`Label`](xref:Xamarin.Forms.Label) и [`Span`](xref:Xamarin.Forms.Span) могут быть подклассами для создания классов `HyperlinkLabel` и `HyperlinkSpan` с помощью распознавателя жестов и кода форматирования текста.

В следующем примере кода, взятом из примера [демонстрации гиперссылок](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/) , показан класс `HyperlinkSpan`:

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

Класс `HyperlinkSpan` определяет свойство `Url` и связанное [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), а конструктор задает внешний вид гиперссылки и [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) , которые будут реагировать при касании гиперссылки. При нажатии `HyperlinkSpan` `TapGestureRecognizer` будет отвечать, выполнив метод `Launcher.OpenAsync`, чтобы открыть URL-адрес, указанный свойством `Url`, в веб-браузере.

Класс `HyperlinkSpan` можно использовать, добавив в XAML экземпляр класса:

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

## <a name="styling-labels"></a>Стиль метки

В предыдущих разделах рассматривалось установка [`Label`](xref:Xamarin.Forms.Label) и [`Span`](xref:Xamarin.Forms.Span) свойств для каждого экземпляра. Тем не менее наборы свойств могут быть сгруппированы в один стиль, который последовательно применяется для одного или нескольких представлений. Это может повысить читаемость кода и делать изменения дизайна проще в реализации. Дополнительные сведения см. в разделе [стили](~/xamarin-forms/user-interface/text/styles.md).

## <a name="related-links"></a>Связанные ссылки

- [Текст (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Гиперссылки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks)
- [Создание мобильных приложений с помощью Xamarin. Forms, глава 3](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [API Label](xref:Xamarin.Forms.Label)
- [API span](xref:Xamarin.Forms.Span)
