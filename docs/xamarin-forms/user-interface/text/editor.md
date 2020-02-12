---
title: Редактор Xamarin.Forms
description: В этой статье объясняется, как использовать элемент управления редактора Xamarin.Forms принимать ввод многострочного текста в приложении.
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/26/2019
ms.openlocfilehash: 1ae176cfebdde31038c30895d1bf562ff3396eaa
ms.sourcegitcommit: ccbf914615c0ce6b3f308d930f7a77418aeb4dbc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/11/2020
ms.locfileid: "77131072"
---
# <a name="xamarinforms-editor"></a>Редактор Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Многострочный ввод текста_

Элемент управления [`Editor`](xref:Xamarin.Forms.Editor) используется для приема многострочного ввода. В этой статье рассматриваются следующие вопросы:

- **[Настройка](#customization)** &ndash; параметры клавиатуры и цвета.
- **[Интерактивные](#interactivity)** &ndash; события, которые можно прослушивать для обеспечения интерактивности.

## <a name="customization"></a>Настройка

### <a name="setting-and-reading-text"></a>Установка и чтение текста

[`Editor`](xref:Xamarin.Forms.Editor), как и другие представления для текста, предоставляет свойство `Text`. Это свойство можно использовать для установки и чтения текста, представленного `Editor`. В следующем примере показано задание свойства `Text` в XAML:

```xaml
<Editor Text="I am an Editor" />
```

В C#:

```csharp
var MyEditor = new Editor { Text = "I am an Editor" };
```

Чтобы прочитать текст, получите доступ к свойству C#`Text` в:

```csharp
var text = MyEditor.Text;
```

### <a name="setting-placeholder-text"></a>Параметр замещающий текст

[`Editor`](xref:Xamarin.Forms.Editor) можно задать для отображения текста заполнителя, если он не хранит данные, вводимые пользователем. Это достигается путем присвоения свойству [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) `string`и часто используется для указания типа содержимого, подходящего для `Editor`. Кроме того, цвет текста заполнителя можно контролировать, присвоив свойству [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor) [`Color`](xref:Xamarin.Forms.Color):

```xaml
<Editor Placeholder="Enter text here" PlaceholderColor="Olive" />
```

```csharp
var editor = new Editor { Placeholder = "Enter text here", PlaceholderColor = Color.Olive };
```

### <a name="preventing-text-entry"></a>Предотвращение ввода текста

Пользователи могут запретить изменение текста в [`Editor`](xref:Xamarin.Forms.Editor) , задав свойство `IsReadOnly`, которое по умолчанию имеет значение `false`, чтобы `true`:

```xaml
<Editor Text="This is a read-only Editor"
        IsReadOnly="true" />
```

```csharp
var editor = new Editor { Text = "This is a read-only Editor", IsReadOnly = true });
```

> [!NOTE]
> Свойство `IsReadonly` не изменяет внешний вид [`Editor`](xref:Xamarin.Forms.Editor), в отличие от свойства `IsEnabled`, которое также изменяет внешний вид `Editor` на серый.

### <a name="limiting-input-length"></a>Ограничение длины входных

Свойство [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) можно использовать для ограничения длины ввода, разрешенной для [`Editor`](xref:Xamarin.Forms.Editor). Это свойство должно быть присвоено положительное целое число:

```xaml
<Editor ... MaxLength="10" />
```

```csharp
var editor = new Editor { ... MaxLength = 10 };
```

Значение свойства [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) , равное 0, указывает, что входные данные не допускаются, и значение `int.MaxValue`, которое является значением по умолчанию для [`Editor`](xref:Xamarin.Forms.Editor), указывает на отсутствие эффективного ограничения на количество вводимых символов.

### <a name="character-spacing"></a>Интервал между символами

Межзнаковые интервалы можно применять к [`Editor`](xref:Xamarin.Forms.Editor) , присвоив свойству `Editor.CharacterSpacing` значение `double`.

```xaml
<Editor ...
        CharacterSpacing="10" />
```

Эквивалентный код на C# выглядит так:

```csharp
Editor editor = new editor { CharacterSpacing = 10 };
```

Результат заключается в том, что символы в тексте, отображаемом [`Editor`](xref:Xamarin.Forms.Editor) , размещаются `CharacterSpacing` независимыми от устройства единицами.

> [!NOTE]
> Значение свойства `CharacterSpacing` применяется к тексту, отображаемому свойствами `Text` и `Placeholder`.

### <a name="auto-sizing-an-editor"></a>Автоматического изменения размера редактор

[`Editor`](xref:Xamarin.Forms.Editor) можно установить для автоматического изменения размера содержимого, задав для свойства [`Editor.AutoSize`](xref:Xamarin.Forms.Editor.AutoSize) значение [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges), которое является значением перечисления [`EditoAutoSizeOption`](xref:Xamarin.Forms.EditorAutoSizeOption) . Это перечисление имеет два значения.

- [`Disabled`](xref:Xamarin.Forms.EditorAutoSizeOption.Disabled) указывает, что автоматическое изменение размера отключено и является значением по умолчанию.
- [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges) указывает, что автоматическое изменение размера включено.

Это можно сделать в коде следующим образом:

```xaml
<Editor Text="Enter text here" AutoSize="TextChanges" />
```

```csharp
var editor = new Editor { Text = "Enter text here", AutoSize = EditorAutoSizeOption.TextChanges };
```

Если автоматическое изменение размера включено, то высота [`Editor`](xref:Xamarin.Forms.Editor) увеличится, когда пользователь заполнит его текстом, а высота будет уменьшаться по мере удаления текста пользователем.

> [!NOTE]
> Если свойство [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) установлено, [`Editor`](xref:Xamarin.Forms.Editor) не будет иметь Авторазмер.

### <a name="customizing-the-keyboard"></a>Настройка клавиатуры

Клавиатура, представленная при взаимодействии пользователей с [`Editor`](xref:Xamarin.Forms.Editor) , может быть задана программно с помощью свойства [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) , к одному из следующих свойств класса [`Keyboard`](xref:Xamarin.Forms.Keyboard) :

- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat) — это текстовая клавиатура с эмодзи.
- [`Default`](xref:Xamarin.Forms.Keyboard.Default) — это клавиатура по умолчанию.
- [`Email`](xref:Xamarin.Forms.Keyboard.Email) используется для ввода адресов электронной почты.
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) — цифровая клавиатура.
- [`Plain`](xref:Xamarin.Forms.Keyboard.Plain) предназначена для ввода текста, если нет заданных [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags).
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone) — клавиатура для ввода телефонных номеров.
- [`Text`](xref:Xamarin.Forms.Keyboard.Text) — текстовая клавиатура.
- [`Url`](xref:Xamarin.Forms.Keyboard.Url) — клавиатура для ввода путей к файлам и веб-адресов.

Это можно сделать в XAML следующим образом:

```xaml
<Editor Keyboard="Chat" />
```

Эквивалентный код на C# выглядит так:

```csharp
var editor = new Editor { Keyboard = Keyboard.Chat };
```

Примеры каждой клавиатуры можно найти в нашем репозитории [рецептов](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/choose-keyboard-for-entry) .

Класс [`Keyboard`](xref:Xamarin.Forms.Keyboard) также имеет фабричный метод [`Create`](xref:Xamarin.Forms.Keyboard.Create*), который может использоваться для настройки клавиатуры, задавая регистр букв, проверку орфографии и режим подсказок. Значения перечисления [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) задаются как аргументы метода, при этом возвращается настроенное свойство `Keyboard`. Перечисление `KeyboardFlags` имеет такие значения:

- [`None`](xref:Xamarin.Forms.KeyboardFlags.None) указывает, что клавиатура не имеет никаких дополнительных функций.
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) указывает, что первые слова во всех вводимых предложениях автоматически начинаются с прописных букв.
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) указывает, что для вводимого текста выполняется проверка орфографии.
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) указывает, что для вводимых слов предлагается завершение.
- [`CapitalizeWord`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeWord) указывает, что все слова автоматически начинаются с прописных букв.
- [`CapitalizeCharacter`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeCharacter) указывает, что все символы автоматически пишутся прописными буквами.
- [`CapitalizeNone`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeNone) указывает, что автоматическая подстановка прописных букв не выполняется.
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) указывает, что для вводимого текста выполняется проверка орфографии, завершение слов и автоматическое написание предложений с прописной буквы.

В следующем примере кода XAML показано, как настроить значение по умолчанию [`Keyboard`](xref:Xamarin.Forms.Keyboard), чтобы включить предложение завершения слов и написание всех символов прописными буквами:

```xaml
<Editor>
    <Editor.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>Suggestions,CapitalizeCharacter</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </Editor.Keyboard>
</Editor>
```

Эквивалентный код на C# выглядит так:

```csharp
var editor = new Editor();
editor.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

### <a name="enabling-and-disabling-spell-checking"></a>Включение и отключение проверка орфографии

Свойство [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) определяет, включена ли проверка орфографии. По умолчанию свойство имеет значение `true`. Как пользователь вводит текст, заданный опечаток.

Однако для некоторых сценариев ввода текста, таких как ввод имени пользователя, проверка орфографии обеспечивает негативную работу, поэтому ее следует отключить, задав для свойства [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) значение `false`.

```xaml
<Editor ... IsSpellCheckEnabled="false" />
```

```csharp
var editor = new Editor { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Если свойство [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) имеет значение `false`, а пользовательская клавиатура не используется, встроенная проверка орфографии будет отключена. Однако если было задано [`Keyboard`](xref:Xamarin.Forms.Keyboard) , которое отключает проверку орфографии, например [`Keyboard.Chat`](xref:Xamarin.Forms.Keyboard.Chat), свойство `IsSpellCheckEnabled` игнорируется. Поэтому свойство не может использоваться для включения проверки орфографии для `Keyboard`, которая явно отключается.

### <a name="enabling-and-disabling-text-prediction"></a>Включение и отключение прогнозирования текста

Свойство `IsTextPredictionEnabled` определяет, включено ли прогнозирование текста и автоматическое исправление текста. По умолчанию свойство имеет значение `true`. Как пользователь вводит текст, представляется word прогнозов.

Однако для некоторых сценариев ввода текста, таких как ввод имени пользователя, прогнозирование текста и автоматическое исправление текста обеспечивают негативную работу и должны быть отключены путем присвоения свойству `IsTextPredictionEnabled` значения `false`.

```xaml
<Editor ... IsTextPredictionEnabled="false" />
```

```csharp
var editor = new Editor { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> Если свойство `IsTextPredictionEnabled` имеет значение `false`, а пользовательская клавиатура не используется, прогнозирование текста и автоматическое исправление текста отключаются. Однако если было задано [`Keyboard`](xref:Xamarin.Forms.Keyboard) , отключающее прогнозирование текста, свойство `IsTextPredictionEnabled` игнорируется. Поэтому свойство не может быть использовано для включения прогнозирования текста для `Keyboard`, который явно отключается.

### <a name="colors"></a>Цвета

`Editor` можно настроить для использования пользовательского цвета фона с помощью свойства `BackgroundColor`. Особое внимание необходимо, чтобы гарантировать, что цвета будет доступна для использования на каждой платформе. Так как каждая платформа имеет различные значения по умолчанию для цвета текста, может потребоваться задать пользовательский цвет фона для каждой платформы. Дополнительные сведения об оптимизации пользовательского интерфейса для каждой платформы см. [в разделе Работа с](~/xamarin-forms/platform/device.md) оптимизациями платформы.

В C#:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        //dark blue on UWP & Android, light blue on iOS
        var editor = new Editor { BackgroundColor = Device.RuntimePlatform == Device.iOS ? Color.FromHex("#A4EAFF") : Color.FromHex("#2c3e50") };
        layout.Children.Add(editor);
    }
}
```

В XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="TextSample.EditorPage"
    Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor>
                <Editor.BackgroundColor>
                    <OnPlatform x:TypeArguments="x:Color">
                        <On Platform="iOS" Value="#a4eaff" />
                        <On Platform="Android, UWP" Value="#2c3e50" />
                    </OnPlatform>
                </Editor.BackgroundColor>
            </Editor>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

![](editor-images/textbackgroundcolor.png "Editor with BackgroundColor Example")

Убедитесь, что цвета фона и текста, которые вы выбрали могут использоваться на каждой платформе и не закрывают любой текст заполнителя.

## <a name="interactivity"></a>Интерактивность

`Editor` предоставляет два события:

- Событие [TextChanged](xref:Xamarin.Forms.InputView.TextChanged) &ndash; возникает при изменении текста в редакторе. Содержит текст до и после изменения.
- [Завершенная](xref:Xamarin.Forms.Editor.Completed) &ndash; возникает, когда пользователь закончил ввод, нажав клавишу Return на клавиатуре.

> [!NOTE]
> Класс [`VisualElement`](xref:Xamarin.Forms.VisualElement) , от которого [`Entry`](xref:Xamarin.Forms.Entry) наследуется, также имеет события [`Focused`](xref:Xamarin.Forms.VisualElement.Focused) и [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused) .

### <a name="completed"></a>Завершено

Событие `Completed` используется для реагирования на завершение взаимодействия с `Editor`. `Completed` возникает, когда пользователь заканчивает ввод с полем, вводя ключ возврата на клавиатуре (или нажав клавишу TAB в UWP). Обработчик события является универсальным обработчиком событий, принимающим отправителя и `EventArgs`:

```csharp
void EditorCompleted (object sender, EventArgs e)
{
    var text = ((Editor)sender).Text; // sender is cast to an Editor to enable reading the `Text` property of the view.
}
```

Завершенное событие можно подписаться в коде и XAML:

В C#:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var editor = new Editor ();
        editor.Completed += EditorCompleted;
        layout.Children.Add(editor);
    }
}
```

В XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.EditorPage"
Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor Completed="EditorCompleted" />
        </StackLayout>
    </ContentPage.Content>
</Contentpage>
```

### <a name="textchanged"></a>TextChanged

Событие `TextChanged` используется для реагирования на изменение содержимого поля.

`TextChanged` возникает при каждом изменении `Text` `Editor`. Обработчик события принимает экземпляр `TextChangedEventArgs`. `TextChangedEventArgs` предоставляет доступ к старым и новым значениям `Editor` `Text` через свойства `OldTextValue` и `NewTextValue`:

```csharp
void EditorTextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

Завершенное событие можно подписаться в коде и XAML:

В коде:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var editor = new Editor ();
        editor.TextChanged += EditorTextChanged;
        layout.Children.Add(editor);
    }
}
```

В XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.EditorPage"
Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor TextChanged="EditorTextChanged" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

## <a name="related-links"></a>Связанные ссылки

- [Текст (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [API редактора](xref:Xamarin.Forms.Editor)
