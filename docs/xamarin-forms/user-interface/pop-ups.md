---
title: Отображать всплывающие окна
description: Xamarin. Forms предоставляет три всплывающих элемента пользовательского интерфейса — оповещение, лист действий и запрос. В этой статье демонстрируется использование API предупреждений, листов действий и интерфейсов командной строки для вывода диалоговых окон, предлагающих пользователям простые вопросы, помогающие пользователям выполнять задачи и выводить запросы.
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/17/2020
ms.openlocfilehash: c71153cdaa94a7983b89968abc828011a648f2b1
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306629"
---
# <a name="display-pop-ups"></a>Отображать всплывающие окна

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-pop-ups)

Отображение предупреждения, предоставление пользователю возможности выбора или отображение запроса — это обычная задача пользовательского интерфейса. Xamarin. Forms содержит три метода [`Page`](xref:Xamarin.Forms.Page) класса для взаимодействия с пользователем через всплывающее окно: [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*), [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*)и `DisplayPromptAsync`. Эти элементы визуализируются на каждой платформе с помощью соответствующих собственных элементов управления.

## <a name="display-an-alert"></a>Отображение оповещений

На всех платформах, поддерживаемых Xamarin.Forms, есть модальный всплывающий элемент, позволяющий выводить предупреждения или задавать простые вопросы пользователю. Для вывода предупреждений в Xamarin.Forms используйте метод [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) на любой странице [`Page`](xref:Xamarin.Forms.Page). Следующая строка отображает простое сообщение:

```csharp
await DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "Alert Dialog with One Button")

В этом примере не предполагается получение сведений от пользователя. Предупреждение отображается в модальном режиме, и после его закрытия пользователь продолжает работать с приложением.

Метод [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) можно также использовать для получения ответа от пользователя. Для этого предлагаются две кнопки и возвращается значение типа `boolean`. Для получения ответа на предупреждение предоставьте надписи для обеих кнопок и примените к методу оператор `await`. После того как пользователь выберет один из вариантов, ответ возвращается в код. Обратите внимание на ключевые слова `async` и `await` в примере кода ниже.

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  bool answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[![дисплайалерт](pop-ups-images/alert2-sml.png "Диалоговое окно предупреждения с двумя кнопками")](pop-ups-images/alert2.png#lightbox "Диалоговое окно предупреждения с двумя кнопками")

## <a name="guide-users-through-tasks"></a>Рекомендации для пользователей по задачам

[UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html) — это стандартный элемент пользовательского интерфейса в iOS. Метод Xamarin.Forms [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) позволяет включать этот элемент управления в кроссплатформенные приложения. При этом в Android и UWP будут отрисовываться собственные аналоги.

Чтобы отобразить лист действий, `await` [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) в любом [`Page`](xref:Xamarin.Forms.Page), передав метки сообщения и кнопки в виде строк. Этот метод возвращает надпись кнопки, нажатой пользователем. Вот простой пример.

```csharp
async void OnActionSheetSimpleClicked (object sender, EventArgs e)
{
  string action = await DisplayActionSheet ("ActionSheet: Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
  Debug.WriteLine ("Action: " + action);
}
```

![](pop-ups-images/action.png "ActionSheet Dialog")

Кнопка `destroy` отрисовывается не так, как другие. Ее можно оставить со значением `null` или указать в качестве третьего строкового параметра. В следующем примере используется кнопка `destroy`.

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  string action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[![дисплайактионшит](pop-ups-images/action2-sml.png "Диалоговое окно листа действий с кнопкой "уничтожить"")](pop-ups-images/action2.png#lightbox "Диалоговое окно листа действий с кнопкой "уничтожить"")

## <a name="display-a-prompt"></a>Отображение запроса

Чтобы отобразить запрос, вызовите `DisplayPromptAsync` в любом [`Page`](xref:Xamarin.Forms.Page), передав заголовок и сообщение в качестве `string` аргументов:

```csharp
string result = await DisplayPromptAsync("Question 1", "What's your name?");
```

Запрос отображается в модальном виде:

[![Снимок экрана: модальная строка в iOS и Android](pop-ups-images/simple-prompt.png "Модальная строка")](pop-ups-images/simple-prompt-large.png#lightbox "Модальная строка")

Если нажата кнопка ОК, то возвращенный ответ возвращается в виде `string`. Если нажата кнопка Отмена, возвращается `null`.

Полный список аргументов для метода `DisplayPromptAsync`:

- `title`типа `string`— это заголовок, отображаемый в командной строке.
- `message`типа `string`— это сообщение, отображаемое в командной строке.
- `accept`типа `string`— это текст кнопки Accept. Это необязательный аргумент, значение по умолчанию которого — ОК.
- `cancel`типа `string`— это текст кнопки Отмена. Это необязательный аргумент, значение по умолчанию которого — Cancel.
- `placeholder`типа `string`— это текст заполнителя, отображаемый в командной строке. Это необязательный аргумент, значение по умолчанию которого — `null`.
- `maxLength`типа `int`— это максимальная длина ответа пользователя. Это необязательный аргумент, значение по умолчанию которого равно-1.
- `keyboard`типа `Keyboard`— это тип клавиатуры, используемый для ответа пользователя. Это необязательный аргумент, значение по умолчанию которого — `Keyboard.Default`.
- `initialValue`типа `string`— это предварительно определенный ответ, который будет отображаться и который можно изменить. Это необязательный аргумент, значение по умолчанию которого — пустая `string`.

В следующем примере показано задание некоторых необязательных аргументов:

```csharp
string result = await DisplayPromptAsync("Question 2", "What's 5 + 5?", initialValue: "10", maxLength: 2, keyboard: Keyboard.Numeric);
```

Этот код отображает предопределенный ответ 10, ограничивает число символов, которое может быть введено равным 2, и отображает цифровую клавиатуру для ввода данных пользователем:

[![Снимок экрана: модальная строка в iOS и Android](pop-ups-images/keyboard-prompt.png "Модальная строка")](pop-ups-images/keyboard-prompt-large.png#lightbox "Модальная строка")

> [!NOTE]
> В настоящее время метод `DisplayPromptAsync` реализован только в iOS и Android.

## <a name="related-links"></a>Связанные ссылки

- [PopupsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-pop-ups)
