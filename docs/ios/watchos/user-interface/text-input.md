---
title: Работа с вводом текста watchOS в Xamarin
description: В этом документе описывается ввод текста watchOS в Xamarin. В нем обсуждается метод Пресенттекстинпутконтроллер, Scribble, обычный текст, эмодзи и диктовка.
ms.prod: xamarin
ms.assetid: E9CDF1DE-4233-4C39-99A9-C0AA643D314D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: f77c48cbec6a672a67808cda4e7b8fe887b492af
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/31/2019
ms.locfileid: "70200148"
---
# <a name="working-with-watchos-text-input-in-xamarin"></a>Работа с вводом текста watchOS в Xamarin

В Apple Watch не предусмотрена клавиатура для ввода текста пользователем, однако она поддерживает некоторые альтернативные контрольные значения:

- Выбор из предварительно определенного списка параметров текста
- Siri Диктовка,
- Выбор эмодзи,
- Зарисованное распознавание рукописного письма по буквам (представленное в watchOS 3).

Симулятор в настоящее время не поддерживает диктовку, но вы по-прежнему можете проверить другие параметры контроллера ввода текста, например Scribble, как показано ниже:

![](text-input-images/textinput-sml.png "Тестирование параметра Scribble")

Для приема ввода текста в приложении для просмотра контрольных данных:

1. Создайте массив строк предопределенных параметров.
2. Вызовите метод `PresentTextInputController` с массивом, следует ли разрешить эмодзи, `Action` а также метод, вызываемый после завершения работы пользователя.
3. В действии завершения проверьте наличие входного результата и выполните соответствующее действие в приложении (возможно задание текстового значения метки).

В следующем фрагменте кода представлено три предварительно определенных параметра для пользователя:

```csharp
var suggest = new string[] {"Get groceries", "Buy gas", "Post letter"};

PresentTextInputController (suggest, WatchKit.WKTextInputMode.AllowEmoji, (result) => {
    // action when the "text input" is complete
    if (result != null && result.Count > 0) {
    // this only works if result is a text response (Plain or AllowEmoji)
        enteredText = result.GetItem<NSObject>(0).ToString();
        Console.WriteLine (enteredText);
        // do something, such as myLabel.SetText(enteredText);
    }
});
```

`WKTextInputMode` Перечисление имеет три значения:

- Plain
- алловеможи
- аллованиматедеможи

## <a name="plain"></a>Plain

Если задан обычный режим, пользователь может выбрать:

- Диктофона
- Scribble или
- из предварительно определенного списка, предоставленного приложением.

[![](text-input-images/plain-scribble-sml.png "Диктовка, Рисованная кривая или из предварительно определенного списка, предоставляемого приложением")](text-input-images/plain-scribble.png#lightbox)

Результат всегда возвращается в виде `NSObject` , который можно привести `string`к типу.

## <a name="emoji"></a>Символ

Существует два типа эмодзи:

- Регулярный формат эмодзи (Юникод)
- Анимированные изображения

Когда пользователь выбирает Юникод эмодзи, он возвращается в виде строки.

Если выбрано анимированное изображение эмодзи, `result` в обработчике завершения будет содержаться объект, содержащий символ `UIImage` `NSData` эмодзи.

## <a name="accepting-dictation-only"></a>Принятие только диктовки

Чтобы перевести пользователя непосредственно на экран диктовки, не отображая никаких предложений (или параметра Scribble), сделайте следующее:

- передайте пустой массив для списка предложений и
- Задайте `WatchKit.WKTextInputMode.Plain`значение.

```csharp
PresentTextInputController (new string[0], WatchKit.WKTextInputMode.Plain, (result) => {
    // action when the "text input" is complete
    if (result != null && result.Count > 0) {
        dictatedText = result.GetItem<NSObject>(0).ToString();
        Console.WriteLine (dictatedText);
        // do something, such as myLabel.SetText(dictatedText);
    }
});
```

Когда пользователь говорит, на экране контрольные значения отображается следующий экран, который содержит текст, как он понимает (например, "это тест"):

![](text-input-images/dictation.png "Когда пользователь говорит, на экране контрольные значения отображается текст, как он понимается.")

После нажатия кнопки **done (Готово** ) будет возвращен текст.



## <a name="related-links"></a>Связанные ссылки

- [Документ Apple Text and Labels](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/TextandLabels.html)
- [Введение в watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
