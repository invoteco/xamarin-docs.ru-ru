---
title: Запись Xamarin.Forms
description: В этой статье описываются способы использования класса входа Xamarin.Forms для принятия однострочный текст или пароля, вводимого в приложении.
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/25/2019
ms.openlocfilehash: c7daad8898d3048f10c9489ee4e2c78f147c6e52
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306617"
---
# <a name="xamarinforms-entry"></a>Запись Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Ввод текста или пароля в одной строке_

[`Entry`](xref:Xamarin.Forms.Entry) Xamarin. Forms используется для однострочного ввода текста. `Entry`, как и представление [`Editor`](xref:Xamarin.Forms.Editor) , поддерживает несколько типов клавиатуры. Кроме того, `Entry` можно использовать в качестве поля пароля.

## <a name="display-customization"></a>Настройки отображения

### <a name="setting-and-reading-text"></a>Установка и чтение текста

`Entry`, как и другие представления для текста, предоставляет свойство [`Text`](xref:Xamarin.Forms.InputView.Text) . Это свойство можно использовать для установки и чтения текста, представленного `Entry`. В следующем примере показано задание свойства `Text` в XAML:

```xaml
<Entry Text="I am an Entry" />
```

В C#:

```csharp
var MyEntry = new Entry { Text = "I am an Entry" };
```

Чтобы прочитать текст, получите доступ к свойству C#`Text` в:

```csharp
var text = MyEntry.Text;
```

### <a name="setting-placeholder-text"></a>Параметр замещающий текст

[`Entry`](xref:Xamarin.Forms.Entry) можно задать для отображения текста заполнителя, если он не хранит данные, вводимые пользователем. Это достигается путем присвоения свойству [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) `string`и часто используется для указания типа содержимого, подходящего для `Entry`. Кроме того, цвет текста заполнителя можно контролировать, присвоив свойству [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor) [`Color`](xref:Xamarin.Forms.Color):

```xaml
<Entry Placeholder="Username" PlaceholderColor="Olive" />
```

```csharp
var entry = new Entry { Placeholder = "Username", PlaceholderColor = Color.Olive };
```

> [!NOTE]
> Ширину `Entry` можно определить, задав ее свойство `WidthRequest`. Не зависят от ширины `Entry` определяется в зависимости от значения свойства `Text`.

### <a name="preventing-text-entry"></a>Предотвращение ввода текста

Пользователи могут запретить изменение текста в [`Entry`](xref:Xamarin.Forms.Entry) , задав свойство `IsReadOnly`, которое по умолчанию имеет значение `false`, чтобы `true`:

```xaml
<Entry Text="This is a read-only Entry"
       IsReadOnly="true" />
```

```csharp
var entry = new Entry { Text = "This is a read-only Entry", IsReadOnly = true });
```

> [!NOTE]
> Свойство `IsReadonly` не изменяет внешний вид [`Entry`](xref:Xamarin.Forms.Entry), в отличие от свойства `IsEnabled`, которое также изменяет внешний вид `Entry` на серый.

### <a name="limiting-input-length"></a>Ограничение длины входных

Свойство [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) можно использовать для ограничения длины ввода, разрешенной для [`Entry`](xref:Xamarin.Forms.Entry). Это свойство должно быть присвоено положительное целое число:

```xaml
<Entry ... MaxLength="10" />
```

```csharp
var entry = new Entry { ... MaxLength = 10 };
```

Значение свойства [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) , равное 0, указывает, что входные данные не допускаются, и значение `int.MaxValue`, которое является значением по умолчанию для [`Entry`](xref:Xamarin.Forms.Entry), указывает на отсутствие эффективного ограничения на количество вводимых символов.

### <a name="character-spacing"></a>Интервал между символами

Межзнаковые интервалы можно применять к [`Entry`](xref:Xamarin.Forms.Entry) , присвоив свойству `Entry.CharacterSpacing` значение `double`.

```xaml
<Entry ...
       CharacterSpacing="10" />
```

Эквивалентный код на C# выглядит так:

```csharp
Entry entry = new Entry { CharacterSpacing = 10 };
```

Результат заключается в том, что символы в тексте, отображаемом [`Entry`](xref:Xamarin.Forms.Entry) , размещаются `CharacterSpacing` независимыми от устройства единицами.

> [!NOTE]
> Значение свойства `CharacterSpacing` применяется к тексту, отображаемому свойствами `Text` и `Placeholder`.

### <a name="password-fields"></a>Поля пароля

`Entry` предоставляет свойство `IsPassword`. Если `IsPassword` `true`, содержимое поля будет представлено черными кружками:

В XAML:

```xaml
<Entry IsPassword="true" />
```

В C#:

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![Пример ввода пароля](entry-images/password.png)

Заполнители можно использовать с экземплярами `Entry`, которые настроены в качестве полей паролей:

В XAML:

```xaml
<Entry IsPassword="true" Placeholder="Password" />
```

В C#:

```csharp
var MyEntry = new Entry { IsPassword = true, Placeholder = "Password" };
```

![Пример ввода пароля и заполнителя](entry-images/passwordplaceholder.png)

### <a name="setting-the-cursor-position-and-text-selection-length"></a>Задание позиции курсора и длину выделения текста

Свойство [`CursorPosition`](xref:Xamarin.Forms.Entry.CursorPosition) можно использовать для возвращения или установки позиции, в которую следующий символ будет вставлен в строку, хранящуюся в свойстве [`Text`](xref:Xamarin.Forms.InputView.Text) :

```xaml
<Entry Text="Cursor position set" CursorPosition="5" />
```

```csharp
var entry = new Entry { Text = "Cursor position set", CursorPosition = 5 };
```

Значение по умолчанию для свойства [`CursorPosition`](xref:Xamarin.Forms.Entry.CursorPosition) равно 0, что означает, что текст будет вставлен в начало `Entry`.

Кроме того, можно использовать свойство [`SelectionLength`](xref:Xamarin.Forms.Entry.SelectionLength) , чтобы вернуть или задать длину выделенного текста в `Entry`:

```xaml
<Entry Text="Cursor position and selection length set" CursorPosition="2" SelectionLength="10" />
```

```csharp
var entry = new Entry { Text = "Cursor position and selection length set", CursorPosition = 2, SelectionLength = 10 };
```

Значение по умолчанию для свойства [`SelectionLength`](xref:Xamarin.Forms.Entry.SelectionLength) равно 0, что означает, что текст не выбран.

### <a name="displaying-a-clear-button"></a>Отображение кнопки "очистить"

Свойство `ClearButtonVisibility` можно использовать для управления отображением в [`Entry`](xref:Xamarin.Forms.Entry) кнопки Clear, которая позволяет пользователю очистить текст. Этому свойству должно быть присвоено значение члена перечисления `ClearButtonVisibility`:

- `Never` указывает, что кнопка Clear никогда не отображается. Это значение по умолчанию для свойства `Entry.ClearButtonVisibility`.
- `WhileEditing` указывает, что кнопка Clear будет отображаться в [`Entry`](xref:Xamarin.Forms.Entry), а текст — в фокусе и.

В следующем примере показано задание свойства в XAML:

```xaml
<Entry Text="Xamarin.Forms"
       ClearButtonVisibility="WhileEditing" />
```

Эквивалентный код на C# выглядит так:

```csharp
var entry = new Entry { Text = "Xamarin.Forms", ClearButtonVisibility = ClearButtonVisibility.WhileEditing };
```

На следующих снимках экрана показана [`Entry`](xref:Xamarin.Forms.Entry) с включенной кнопкой Clear:

![Снимок экрана записи с кнопкой "очистить" в iOS и Android](entry-images/entry-clear-button.png)

### <a name="customizing-the-keyboard"></a>Настройка клавиатуры

Клавиатура, представленная при взаимодействии пользователей с [`Entry`](xref:Xamarin.Forms.Entry) , может быть задана программно с помощью свойства [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) , к одному из следующих свойств класса [`Keyboard`](xref:Xamarin.Forms.Keyboard) :

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
<Entry Keyboard="Chat" />
```

Эквивалентный код на C# выглядит так:

```csharp
var entry = new Entry { Keyboard = Keyboard.Chat };
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
<Entry Placeholder="Enter text here">
    <Entry.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>Suggestions,CapitalizeCharacter</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </Entry.Keyboard>
</Entry>
```

Эквивалентный код на C# выглядит так:

```csharp
var entry = new Entry { Placeholder = "Enter text here" };
entry.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

#### <a name="customizing-the-return-key"></a>Настройка клавиша Return

Внешний вид ключа возврата на экранной клавиатуре, который отображается, когда [`Entry`](xref:Xamarin.Forms.Entry) имеет фокус, может быть настроен путем присвоения свойству [`ReturnType`](xref:Xamarin.Forms.Entry.ReturnType) значения перечисления [`ReturnType`](xref:Xamarin.Forms.ReturnType) :

- [`Default`](xref:Xamarin.Forms.ReturnType.Default) — указывает, что Специальный ключ возврата не требуется и будет использоваться платформа по умолчанию.
- [`Done`](xref:Xamarin.Forms.ReturnType.Done) — указывает ключ возврата "Done".
- [`Go`](xref:Xamarin.Forms.ReturnType.Go) — указывает ключ возврата "Go".
- [`Next`](xref:Xamarin.Forms.ReturnType.Next) — указывает на "следующий" ключ возврата.
- [`Search`](xref:Xamarin.Forms.ReturnType.Search) — указывает ключ возврата поиска.
- [`Send`](xref:Xamarin.Forms.ReturnType.Send) — указывает ключ возврата "Send".

В следующем примере XAML показано, как задать возвращаемое ключ:

```xaml
<Entry ReturnType="Send" />
```

Эквивалентный код на C# выглядит так:

```csharp
var entry = new Entry { ReturnType = ReturnType.Send };
```

> [!NOTE]
> Клавиша return точный вид зависит от платформы. В iOS клавиша return — это кнопка, основанные на тексте. Тем не менее Android и универсальной платформы Windows, клавиша return находится кнопка значки.

При нажатии клавиши Return вызывается событие [`Completed`](xref:Xamarin.Forms.Entry.Completed) и выполняется любое `ICommand`, заданное свойством [`ReturnCommand`](xref:Xamarin.Forms.Entry.ReturnCommand) . Кроме того, все `object`, заданные свойством [`ReturnCommandParameter`](xref:Xamarin.Forms.Entry.ReturnCommandParameter) , передаются в `ICommand` в качестве параметра. Дополнительные сведения о командах см. в разделе [Командный интерфейс](~/xamarin-forms/app-fundamentals/data-binding/commanding.md).

### <a name="enabling-and-disabling-spell-checking"></a>Включение и отключение проверка орфографии

Свойство [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) определяет, включена ли проверка орфографии. По умолчанию свойство имеет значение `true`. Как пользователь вводит текст, заданный опечаток.

Однако для некоторых сценариев ввода текста, таких как ввод имени пользователя, проверка орфографии обеспечивает негативную работу и должна быть отключена путем присвоения свойству [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) значения `false`.

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Если свойство [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) имеет значение `false`, а пользовательская клавиатура не используется, встроенная проверка орфографии будет отключена. Однако если было задано [`Keyboard`](xref:Xamarin.Forms.Keyboard) , которое отключает проверку орфографии, например [`Keyboard.Chat`](xref:Xamarin.Forms.Keyboard.Chat), свойство `IsSpellCheckEnabled` игнорируется. Поэтому свойство не может использоваться для включения проверки орфографии для `Keyboard`, которая явно отключается.

### <a name="enabling-and-disabling-text-prediction"></a>Включение и отключение прогнозирования текста

Свойство [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) определяет, включено ли прогнозирование текста и автоматическое исправление текста. По умолчанию свойство имеет значение `true`. Как пользователь вводит текст, представляется word прогнозов.

Однако для некоторых сценариев ввода текста, таких как ввод имени пользователя, прогнозирование текста и автоматическое исправление текста обеспечивают негативную работу и должны быть отключены путем присвоения свойству [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) значения `false`.

```xaml
<Entry ... IsTextPredictionEnabled="false" />
```

```csharp
var entry = new Entry { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> Если свойство [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) имеет значение `false`, а пользовательская клавиатура не используется, прогнозирование текста и автоматическое исправление текста отключаются. Однако если было задано [`Keyboard`](xref:Xamarin.Forms.Keyboard) , отключающее прогнозирование текста, свойство `IsTextPredictionEnabled` игнорируется. Поэтому свойство не может быть использовано для включения прогнозирования текста для `Keyboard`, который явно отключается.

### <a name="colors"></a>Цвета

Использовать настраиваемый фон и цвета текста с помощью следующих привязываемые свойства можно задать запись.

- **TextColor** &ndash; задает цвет текста.
- **BackgroundColor** &ndash; задает цвет, отображаемый за текстом.

Особое внимание необходимо, чтобы гарантировать, что цвета будет доступна для использования на каждой платформе. Так как каждая платформа имеет различные значения по умолчанию для цветов текста и фона, часто необходимо задать оба, если задается один.

Чтобы задать цвет текста записи, используйте следующий код:

В XAML:

```xaml
<Entry TextColor="Green" />
```

В C#:

```csharp
var entry = new Entry();
entry.TextColor = Color.Green;
```

![Пример записи TextColor](entry-images/textcolor.png)

Обратите внимание, что указанный `TextColor`не влияет на заполнитель.

Чтобы задать цвет фона в XAML:

```xaml
<Entry BackgroundColor="#2c3e50" />
```

В C#:

```csharp
var entry = new Entry();
entry.BackgroundColor = Color.FromHex("#2c3e50");
```

![Пример записи BackgroundColor](entry-images/textbackgroundcolor.png)

Следите за тем, чтобы убедиться в том, что цвета фона и текста, которые вы выбрали могут использоваться на каждой платформе и не закрывают любой текст заполнителя.

## <a name="events-and-interactivity"></a>События и интерактивность

Запись предоставляет два события:

- [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) &ndash; возникает при изменении текста в записи. Содержит текст до и после изменения.
- [`Completed`](xref:Xamarin.Forms.Entry.Completed) &ndash; возникает, когда пользователь завершает ввод, нажав клавишу Return на клавиатуре.

> [!NOTE]
> Класс [`VisualElement`](xref:Xamarin.Forms.VisualElement) , от которого [`Entry`](xref:Xamarin.Forms.Entry) наследуется, также имеет события [`Focused`](xref:Xamarin.Forms.VisualElement.Focused) и [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused) .

### <a name="completed"></a>Завершено

Событие `Completed` используется для реагирования на завершение взаимодействия с записью. `Completed` возникает, когда пользователь заканчивает ввод с полем, нажав клавишу Return на клавиатуре (или нажав клавишу TAB в UWP). Обработчик события является универсальным обработчиком событий, принимающим отправителя и `EventArgs`:

```csharp
void Entry_Completed (object sender, EventArgs e)
{
    var text = ((Entry)sender).Text; //cast sender to access the properties of the Entry
}
```

Завершенное событие можно подписаться в XAML:

```xaml
<Entry Completed="Entry_Completed" />
```

и C#:

```csharp
var entry = new Entry ();
entry.Completed += Entry_Completed;
```

После срабатывания события [`Completed`](xref:Xamarin.Forms.Entry.Completed) выполняется любое `ICommand`, заданное свойством [`ReturnCommand`](xref:Xamarin.Forms.Entry.ReturnCommand) , с `object`, указанным свойством [`ReturnCommandParameter`](xref:Xamarin.Forms.Entry.ReturnCommandParameter) , передаваемым в `ICommand`.

### <a name="textchanged"></a>TextChanged

Событие `TextChanged` используется для реагирования на изменение содержимого поля.

`TextChanged` возникает при каждом изменении `Text` `Entry`. Обработчик события принимает экземпляр `TextChangedEventArgs`. `TextChangedEventArgs` предоставляет доступ к старым и новым значениям `Entry` `Text` через свойства `OldTextValue` и `NewTextValue`:

```csharp
void Entry_TextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

На языке XAML можно подписываться на событие `TextChanged`:

```xaml
<Entry TextChanged="Entry_TextChanged" />
```

и C#:

```csharp
var entry = new Entry ();
entry.TextChanged += Entry_TextChanged;
```

## <a name="related-links"></a>Связанные ссылки

- [Текст (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [API Entry](xref:Xamarin.Forms.Entry)
