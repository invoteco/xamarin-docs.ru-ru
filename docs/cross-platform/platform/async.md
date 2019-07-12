---
title: Обзор поддержки асинхронного выполнения
description: В этом документе описывается программирование с использованием async и await, принципами, представленными в C# 5, чтобы было проще писать асинхронный код.
ms.prod: xamarin
ms.assetid: F87BF587-AB64-4C60-84B1-184CAE36ED65
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: cca147f0c5dd1a217f464ffbed2a1ad2618c9b80
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830196"
---
# <a name="async-support-overview"></a>Обзор поддержки асинхронного выполнения

_C#5 представлен параметр два ключевых слова для упрощения асинхронного программирования: async и await. Эти ключевые слова можно написать простой код, который использует библиотеку параллельных задач для выполнения длительных операций (например, доступ к сети) в другом потоке и легко получить доступ к результатам по завершении. Последние версии Xamarin.iOS и Xamarin.Android поддерживает async и await — в этом документе приводятся объяснения и пример использования нового синтаксиса с помощью Xamarin._

Поддержка асинхронного выполнения Xamarin основаны на Mono 3.0 и обновляет профиль API мобильной версию Silverlight, чтобы быть мобильной версии .NET 4.5 не.

## <a name="overview"></a>Обзор

В этом документе представлены новые async и await ключевые слова, проходит через несколько простых примеров реализации асинхронных методов в Xamarin.iOS и Xamarin.Android.

Более полное описание новой асинхронной возможности C# 5 (включая много примеров и различных сценариях использования) см. в статье [асинхронного программирования](https://docs.microsoft.com/dotnet/csharp/async).

Пример приложения создает простой асинхронный веб-запроса (без блокировки основного потока), а затем обновляет пользовательский Интерфейс с Скачанный html и количество символов.

 [![](async-images/AsyncAwait_427x368.png "Пример приложения создает простой асинхронный веб-запроса без блокирования основного потока, а затем обновляет пользовательский Интерфейс с Скачанный html и количество символов")](async-images/AsyncAwait.png#lightbox)

Поддержка асинхронного выполнения Xamarin основаны на Mono 3.0 и обновляет профиль API мобильной версию Silverlight, чтобы быть мобильной версии .NET 4.5 не.

## <a name="requirements"></a>Требования

C#5 функции требуют 3.0 Mono, который включен в Xamarin.iOS 6.4 и Xamarin.Android 4.8. Вам будет предложено обновить ваш Mono, Xamarin.iOS, Xamarin.Android и Xamarin.Mac для его преимущества.

## <a name="using-async-amp-await"></a>Использование метода async &amp; await

 `async` и `await` новых C# языковые компоненты, которые работают в сочетании с библиотеке параллельных задач, чтобы упростить процесс для написания многопоточного кода выполнять длительные задачи без блокирования основного потока приложения.

## <a name="async"></a>async

### <a name="declaration"></a>Объявление

`async` Ключевое слово помещается в объявлении метода (или на лямбда-выражение или анонимный метод) для указания, что он содержит код, который может выполняться асинхронно, т. е. не блокируют вызывающий поток.

Метод, помеченный атрибутом `async` должен содержать по крайней мере одно выражение или оператор await. Если не `await`s присутствуют в методе, он будет выполняться синхронно (же, как если бы не `async` модификатор). Это также привести Предупреждение компилятора (но не ошибка).

### <a name="return-types"></a>Типы возвращаемых значений

Асинхронный метод должен возвращать `Task`, `Task<TResult>` или `void`.

Укажите `Task` тип возвращаемого значения, если метод не возвращает любое другое значение.

Укажите `Task<TResult>` Если метод должен возвращать значение, где `TResult` является возвращается тип (такие как `int`, например).

`void` Возвращают тип используется главным образом для обработчиков событий, которые ее требуют. Код, который вызывает асинхронные методы, возвращающие void невозможно `await` на результат.

### <a name="parameters"></a>Параметры

Асинхронные методы не могут объявлять `ref` или `out` параметров.

## <a name="await"></a>await

Оператор await могут применяться к задаче в метод, помеченный как async. Он вызывает метод для остановки выполнения в этой точке и дождаться завершения задачи.

С помощью await не блокирует вызывающий поток – вместо этого управление возвращается вызывающему объекту. Это означает, что вызывающий поток не заблокирован, поэтому в примере пользователь потока интерфейса не будет блокироваться при ожидании задачи.

После завершения задачи, метод возобновляет выполнение в той же точке в коде. Сюда входят, возврат к области try блока try-catch-finally (при его наличии). await не может использоваться в блоке catch или finally.

Дополнительные сведения о [await](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/await) на сайте документации Майкрософт.

## <a name="exception-handling"></a>Обработка исключений

Исключения, возникающие внутри асинхронного метода хранятся в задаче и создается, если задача является `await`ed. Эти исключения может быть перехвачено и обработано внутри блока try-catch.

## <a name="cancellation"></a>Отмена

Асинхронные методы, которые принимают много времени для завершения должны поддерживать отмену. Как правило отмены вызывается следующим образом:

- Объект `CancellationTokenSource` создается объект.
- `CancellationTokenSource.Token` Отменяемых асинхронный метод передается экземпляр.
- Запрошена отмена путем вызова `CancellationTokenSource.Cancel` метод.

Затем задача задача отменяет сама себя и подтверждает отмену.

Дополнительные сведения об отмене см. в разделе [Настройка асинхронного приложения (C#)](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/fine-tuning-your-async-application).

## <a name="example"></a>Пример

Скачайте [пример решения Xamarin](https://developer.xamarin.com/samples/mobile/AsyncAwait/) (для iOS и Android) в рабочий пример см. в разделе `async` и `await` в мобильных приложениях. В примере кода рассматривается более подробно в этом разделе.

### <a name="writing-an-async-method"></a>Написание асинхронный метод

Следующий метод показывает, как код `async` метод с `await`ed задачи:

```csharp
public async Task<int> DownloadHomepage()
{
    var httpClient = new HttpClient(); // Xamarin supports HttpClient!

    Task<string> contentsTask = httpClient.GetStringAsync("http://xamarin.com"); // async method!

    // await! control returns to the caller and the task continues to run on another thread
    string contents = await contentsTask;

    ResultEditText.Text += "DownloadHomepage method continues after async call. . . . .\n";

    // After contentTask completes, you can calculate the length of the string.
    int exampleInt = contents.Length;

    ResultEditText.Text += "Downloaded the html and found out the length.\n\n\n";

    ResultEditText.Text += contents; // just dump the entire HTML

    return exampleInt; // Task<TResult> returns an object of type TResult, in this case int
}
```

Следует отметить следующие моменты:

-  Объявление метода включает в себя `async` ключевое слово.
-  Возвращаемый тип — `Task<int>` чтобы вызывающий код может обращаться к `int` значение, которое вычисляется в этом методе.
-  Оператор return находится `return exampleInt;` которого является объект целое число — тот факт, что метод возвращает `Task<int>` является частью улучшений языка.


### <a name="calling-an-async-method-1"></a>Вызов асинхронного метода 1

События нажатия этой кнопки обработчик можно найти в Android примера приложения для вызова метода, описанных выше:

```csharp
GetButton.Click += async (sender, e) => {

    Task<int> sizeTask = DownloadHomepage();

    ResultTextView.Text = "loading...";
    ResultEditText.Text = "loading...\n";

    // await! control returns to the caller
    var intResult = await sizeTask;

    // when the Task<int> returns, the value is available and we can display on the UI
    ResultTextView.Text = "Length: " + intResult ;
    // "returns" void, since it's an event handler
};
```

Примечания.

-  Анонимный делегат имеет префикс ключевого слова async.
-  Асинхронный метод DownloadHomepage Возвращает задачу, <int> , хранится в переменной sizeTask.
-  Код ожидание sizeTask переменной.  *Это* — расположение, которое метод приостанавливается и управление возвращается вызывающему коду, до завершения асинхронной задачи в отдельном потоке.
-  Выполняет *не* приостановить при создании задачи в первой строке метода, несмотря на то что создаваемая задача. Ключевое слово await указывает расположение, где выполнение приостановлено.
-  После завершения асинхронной задачи, intResult устанавливается, и выполнение продолжается в исходном потоке, из строки await.


### <a name="calling-an-async-method-2"></a>Вызов асинхронного метода 2

В примере приложения iOS пример написан немного по-разному для демонстрации в качестве альтернативы. Вместо того чтобы использовать анонимный делегат в этом примере объявляется `async` обработчик событий, который назначается как обработчик регулярных событий:

```csharp
GetButton.TouchUpInside += HandleTouchUpInside;
```

Метод обработчика событий затем определяется, как показано ниже:

```csharp
async void HandleTouchUpInside (object sender, EventArgs e)
{
    ResultLabel.Text = "loading...";
    ResultTextView.Text = "loading...\n";

    // await! control returns to the caller
    var intResult = await DownloadHomepage();

    // when the Task<int> returns, the value is available and we can display on the UI
    ResultLabel.Text = "Length: " + intResult ;
}
```

Некоторые важные моменты:

-  Метод помечен как `async` , но возвращает `void` . Это обычно выполняется только для обработчиков событий (в противном случае вы будете возвращать `Task` или `Task<TResult>` ).
-  Код `await` s на `DownloadHomepage` метод непосредственно для присвоения значения переменной ( `intResult` ) в отличие от предыдущего примера, где мы использовали промежуточного `Task<int>` переменной для ссылки на задачу.  *Это* — это расположение, где управление возвращается вызывающему объекту до завершения асинхронного метода в другом потоке.
-  Когда асинхронный метод завершения и возвращает, выполнение возобновляется `await` означающее целочисленный результат возвращается и выводятся в элемент пользовательского интерфейса.


## <a name="summary"></a>Сводка

Использование метода async и await значительно упрощает код, необходимый для создания длительные операции в фоновых потоках без блокировки основного потока. Они также упрощают доступ к результатам, если задача была завершена.

В этом документе предоставил Общие сведения о новых ключевых слов и примеры для Xamarin.iOS и Xamarin.Android.



## <a name="related-links"></a>Связанные ссылки

- [AsyncAwait (пример)](https://developer.xamarin.com/samples/mobile/AsyncAwait/)
- [Обратные вызовы, как наши поколений, перейдите к инструкции](https://tirania.org/blog/archive/2013/Aug-15.html)
- [Данные (iOS) (пример)](https://developer.xamarin.com/samples/monotouch/Data/)
- [HttpClient (iOS) (пример)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
- [MapKitSearch (iOS) (пример)](https://github.com/xamarin/monotouch-samples/tree/master/MapKitSearch)
- [Асинхронное программирование](https://docs.microsoft.com/dotnet/csharp/async)
- [Fine-Tuning Your Async Application (C#)](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/fine-tuning-your-async-application) (Тонкая настройка асинхронного приложения в C#)
- [Await и пользовательский Интерфейс и взаимоблокировки. Вот это да!](https://devblogs.microsoft.com/pfxteam/await-and-ui-and-deadlocks-oh-my/)
- [Обработка задач по мере их завершения)](https://devblogs.microsoft.com/pfxteam/processing-tasks-as-they-complete/)
- [Task-based Asynchronous Pattern (TAP)](https://msdn.microsoft.com/library/hh873175.aspx) (Асинхронный шаблон, основанный на задачах (TAP))
- [Асинхронность в C# 5 (блог Эрика Липперта) — о введении ключевых слов](http://blogs.msdn.com/b/ericlippert/archive/2010/11/11/whither-async.aspx)
