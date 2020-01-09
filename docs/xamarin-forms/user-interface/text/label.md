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
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75490042"
---
# <a name="xamarinforms-label"></a>Метка Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Отображаемый текст в Xamarin.Forms_

[ `Label` ](xref:Xamarin.Forms.Label) Представление используется для отображения текста, одной или нескольких строк. Метки можно иметь оформление текста, цветным текстом и использовать пользовательские шрифты (семейств, размеры и параметры).

## <a name="text-decorations"></a>Оформление текста

Оформление текста подчеркивания и зачеркивания могут применяться к [ `Label` ](xref:Xamarin.Forms.Label) экземпляров, задав `Label.TextDecorations` свойство к одному или нескольким `TextDecorations` членов перечисления:

- `None`
- `Underline`
- `Strikethrough`

В следующем примере XAML показано параметр `Label.TextDecorations` свойство:

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

Ниже показаны снимки экрана `TextDecorations` членов перечисления, применяется к [ `Label` ](xref:Xamarin.Forms.Label) экземпляров:

![Метки с оформлением текста](label-images/label-textdecorations.png)

> [!NOTE]
> Оформление текста могут также применяться к [ `Span` ](xref:Xamarin.Forms.Span) экземпляров. Дополнительные сведения о `Span` , представлена в разделе [текст в формате](#Formatted_Text).

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

Метки, которые можно задать использование пользовательского текста цвета с помощью привязки [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) свойство.

Особое внимание необходимо, чтобы гарантировать, что цвета будет доступна для использования на каждой платформе. Так как каждая платформа имеет различные значения по умолчанию для цветов текста и фона, необходимо соблюдать осторожность выбрать умолчания, которое работает на каждом.

В следующем примере XAML задает цвет текста `Label`:

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

На следующих снимках экрана показано результат действия параметра `TextColor` свойство:

![Пример метки TextColor](label-images/textcolor.png)

Дополнительные сведения о цветах см. в разделе [цвета](~/xamarin-forms/user-interface/colors.md).

## <a name="fonts"></a>Fonts

Дополнительные сведения о задании шрифты в `Label`, см. в разделе [шрифты](~/xamarin-forms/user-interface/text/fonts.md).

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>Усечение и поддерживающего перенос

Метки можно задать для обработки текста, которые не помещаются на одной строке в одном из нескольких способов, предоставляемых `LineBreakMode` свойство. [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) представляет собой перечисление со следующими значениями:

- **HeadTruncation** &ndash; усекает заголовок текста, отображение окончания.
- **CharacterWrap** &ndash; переносит текст на новую строку на границе символ.
- **MiddleTruncation** &ndash; отображает начало и конец текста, с помощью средней replace многоточие.
- **NoWrap** &ndash; не переносит текст, только отображение текста, можете помещается в одну строку.
- **TailTruncation** &ndash; показано начало текста, усечение окончания.
- **WordWrap** &ndash; переносит текст на границе слова.

## <a name="display-a-specific-number-of-lines"></a>Отображение определенного числа строк

Количество строк, отображаемых по [ `Label` ](xref:Xamarin.Forms.Label) можно указать, задав `Label.MaxLines` свойства `int` значение:

- Если `MaxLines` равно-1, то есть значение по умолчанию, `Label` учитывает значение свойства [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) , чтобы отображалась только одна строка, возможно, обрезанная или все строки со всем текстом.
- Если значение `MaxLines` равно 0, `Label` не отображается.
- Когда `MaxLines` -1, результат идентичен параметр [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) свойства [ `NoWrap` ](xref:Xamarin.Forms.LineBreakMode), [ `HeadTruncation` ](xref:Xamarin.Forms.LineBreakMode), [ `MiddleTruncation` ](xref:Xamarin.Forms.LineBreakMode), или [ `TailTruncation` ](xref:Xamarin.Forms.LineBreakMode). Тем не менее `Label` будет учитывать значение [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) свойство по отношению к размещение многоточие, если применимо.
- При `MaxLines` больше 1, `Label` будет отображаться до указанного числа строк, соблюдая значение [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) свойство по отношению к размещение многоточие, если применимо. Однако можно присвоить `MaxLines` свойства со значением больше 1 не оказывает влияния [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) свойству [ `NoWrap` ](xref:Xamarin.Forms.LineBreakMode).

В следующем примере XAML показано параметр `MaxLines` свойство [ `Label` ](xref:Xamarin.Forms.Label):

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

На следующих снимках экрана показано результат действия параметра `MaxLines` значение 2, когда текст достаточно длинным, чтобы занимать более чем на две строки:

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

`FormattedText` Свойство имеет тип [ `FormattedString` ](xref:Xamarin.Forms.FormattedString), который состоит из одного или нескольких [ `Span` ](xref:Xamarin.Forms.Span) экземпляров, настроенных с помощью [ `Spans` ](xref:Xamarin.Forms.FormattedString.Spans) свойство . Следующие `Span` свойства можно использовать, чтобы задать внешний вид:

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) — цвет фона span.
- `CharacterSpacing` с типом `double` представляет собой интервал между знаками текста `Span`.
- [`Font`](xref:Xamarin.Forms.Span.Font) — шрифт для текста в элементе управления span.
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes) — атрибуты шрифта для текста в элементе управления span.
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily) — Семейство шрифтов, к которой принадлежит шрифт для текста в элементе управления span.
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize) — размер шрифта для текста в элементе управления span.
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor) — цвет для текста в элементе управления span. Это свойство является устаревшим и будет заменен `TextColor` свойство.
- [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight) — Множитель, который следует применить к высота строки по умолчанию диапазона. Дополнительные сведения см. в разделе [высота строки](#line-height).
- [`Style`](xref:Xamarin.Forms.Span.Style) — стиль, применяемый к диапазону.
- [`Text`](xref:Xamarin.Forms.Span.Text) — текст диапазона.
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor) — цвет для текста в элементе управления span.
- `TextDecorations` -оформление для применения к тексту в элементе управления span. Дополнительные сведения см. в разделе [оформление текста](#text-decorations).

Свойства [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor), [`Text`](xref:Xamarin.Forms.Span.Text)и [`Text`](xref:Xamarin.Forms.Span.Text) BIND имеют режим привязки по умолчанию [`OneWay`](xref:Xamarin.Forms.BindingMode). Дополнительные сведения об этом режиме привязки см. в описании [режима привязки по умолчанию](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#the-default-binding-mode) в разделе " [режим привязки](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md) ".

Кроме того [ `GestureRecognizers` ](xref:Xamarin.Forms.GestureElement.GestureRecognizers) свойство может использоваться для определения коллекции распознавателей жестов, которые будет отвечать на жесты на [ `Span` ](xref:Xamarin.Forms.Span).

> [!NOTE]
> Невозможно отобразить HTML в [`Span`](xref:Xamarin.Forms.Span).

Ниже приведен пример XAML `FormattedText` свойство, которое состоит из трех [ `Span` ](xref:Xamarin.Forms.Span) экземпляров:

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
> [ `Text` ](xref:Xamarin.Forms.Span.Text) Свойство `Span` можно задать с помощью привязки данных. Более подробную информацию см. в разделе [Привязка данных](~/xamarin-forms/app-fundamentals/data-binding/index.md).

Обратите внимание, что [ `Span` ](xref:Xamarin.Forms.Span) также может отвечать на все жесты, которые добавляются в диапазон [ `GestureRecognizers` ](xref:Xamarin.Forms.GestureElement.GestureRecognizers) коллекции. Например [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) был добавлен второй `Span` в приведенных выше примерах кода. Таким образом, если это `Span` шифрованию `TapGestureRecognizer` ответит, выполнив `ICommand` определяется [ `Command` ](xref:Xamarin.Forms.TapGestureRecognizer.Command) свойство. Дополнительные сведения о распознавателей жестов, см. в разделе [Xamarin.Forms жесты](~/xamarin-forms/app-fundamentals/gestures/index.md).

На следующих снимках экрана показано результат действия параметра `FormattedString` свойство до трех `Span` экземпляров:

![Пример метки FormattedText](label-images/formattedtext.png)

## <a name="line-height"></a>Высота линии

Высоты [ `Label` ](xref:Xamarin.Forms.Label) и [ `Span` ](xref:Xamarin.Forms.Span) можно настроить, задав [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) свойство или [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) для `double` значение. В iOS и Android эти значения являются множители исходная высота строки, что и на универсальной платформе Windows (UWP) `Label.LineHeight` свойство имеет значение множителя от размера шрифта метки.

> [!NOTE]
>
> - В iOS [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) и [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) свойства изменить высоту строки текста, которое может уместиться на одной строке и текст, который переносится на несколько строк.
> - В Android [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) и [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) свойства только изменить высоту строки текста, который переносится на несколько строк.
> - На UWP [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) свойство изменяет высоту строки текста, который переносится на несколько строк, и [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) не оказывает никакого влияния.

В следующем примере XAML показано параметр [ `LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) свойство [ `Label` ](xref:Xamarin.Forms.Label):

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

На следующих снимках экрана показано результат действия параметра [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) свойство до версии 1.8:

![Пример метки LineHeight](label-images/label-lineheight.png)

В следующем примере XAML показано параметр [ `LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) свойство [ `Span` ](xref:Xamarin.Forms.Span):

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

На следующих снимках экрана показано результат действия параметра [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) свойство до версии 1.8:

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

Предыдущих разделах описаны параметр [ `Label` ](xref:Xamarin.Forms.Label) и [ `Span` ](xref:Xamarin.Forms.Span) свойств для каждого экземпляра. Тем не менее наборы свойств могут быть сгруппированы в один стиль, который последовательно применяется для одного или нескольких представлений. Это может повысить читаемость кода и делать изменения дизайна проще в реализации. Дополнительные сведения см. в разделе [стили](~/xamarin-forms/user-interface/text/styles.md).

## <a name="related-links"></a>Связанные ссылки

- [Текст (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Гиперссылки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks)
- [Создание мобильных приложений с помощью Xamarin.Forms, Глава 3](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [API Label](xref:Xamarin.Forms.Label)
- [Диапазон API](xref:Xamarin.Forms.Span)
