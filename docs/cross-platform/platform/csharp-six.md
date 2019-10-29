---
title: C#6. Обзор новых функций
description: Версия 6 C# языка продолжит развивать язык с меньшим объемом стандартного кода, улучшенной наглядностью и более согласованностью. Синтаксис инициализации очистки, возможность использования оператора await в блоках catch/finally и условных обозначений null? особенно полезен.
ms.prod: xamarin
ms.assetid: 4B4E41A8-68BA-4E2B-9539-881AC19971B
ms.custom: xamu-video
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 1db7ee95ec261739463fa2584f4acf493ac71217
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014821"
---
# <a name="c-6-new-features-overview"></a>C#6. Обзор новых функций

_Версия 6 C# языка продолжит развивать язык с меньшим объемом стандартного кода, улучшенной наглядностью и более согласованностью. Синтаксис инициализации очистки, возможность использования оператора await в блоках catch/finally и условных обозначений null? особенно полезен._

> [!NOTE]
> Сведения о последней версии C# языка — версия 7 — см. статью новые возможности [в 7,0 C# ](/dotnet/csharp/whats-new/csharp-7)

В этом документе представлены новые возможности C# 6. Он полностью поддерживается компилятором Mono, и разработчики могут приступить к использованию новых функций на всех целевых платформах Xamarin.

> [!VIDEO https://youtube.com/embed/7UdV7zGPfMU]

**Новые возможности в C# 6 видео**

## <a name="using-c-6"></a>Использование C# 6

C# 6-компилятор используется во всех последних версиях Visual Studio для Mac.
С помощью компиляторов командной строки следует убедиться, что `mcs --version` возвращает 4,0 или более поздней версии.
Visual Studio для Mac пользователи могут проверить, установлены ли на них моно 4 (или более новая версия), обратившись к **Visual Studio для Mac > Visual Studio для Mac > отобразить сведения**.

## <a name="less-boilerplate"></a>Шаблон less
### <a name="using-static"></a>using static
Перечисления и некоторые классы, такие как `System.Math`, в основном считаются статическими значениями и функциями. В C# 6 можно импортировать все статические члены типа с помощью одной инструкции`using static`. Сравните обычную функцию тригонометрии в C# 5 и C# 6:

```csharp
// Classic C#
class MyClass
{
    public static Tuple<double,double> SolarAngleOld(double latitude, double declination, double hourAngle)
    {
        var tmp = Math.Sin (latitude) * Math.Sin (declination) + Math.Cos (latitude) * Math.Cos (declination) * Math.Cos (hourAngle);
        return Tuple.Create (Math.Asin (tmp), Math.Acos (tmp));
    }
}

// C# 6
using static System.Math;

class MyClass
{
    public static Tuple<double, double> SolarAngleNew(double latitude, double declination, double hourAngle)
    {
        var tmp = Asin (latitude) * Sin (declination) + Cos (latitude) * Cos (declination) * Cos (hourAngle);
        return Tuple.Create (Asin (tmp), Acos (tmp));
    }
}
```

`using static` не делает открытые поля `const`, такие как `Math.PI` и `Math.E`, напрямую доступными:

```csharp
for (var angle = 0.0; angle <= Math.PI * 2.0; angle += Math.PI / 8) ... 
//PI is const, not static, so requires Math.PI
```

### <a name="using-static-with-extension-methods"></a>Использование функции static с методами расширения

`using static`ное средство работает несколько иначе с помощью методов расширения. Несмотря на то, что методы расширения написаны с помощью `static`, они не имеют смысла без экземпляра, на котором выполняется. Поэтому при использовании `using static` с типом, который определяет методы расширения, методы расширения становятся доступными в их целевом типе (тип `this` метода). Например, `using static System.Linq.Enumerable` можно использовать для расширения API объектов `IEnumerable<T>`, не прибегая ко всем типам LINQ:

```csharp
using static System.Linq.Enumerable;
using static System.String;

class Program
{
    static void Main()
    {
        var values = new int[] { 1, 2, 3, 4 };
        var evenValues = values.Where (i => i % 2 == 0);
        System.Console.WriteLine (Join(",", evenValues));
    }
}
```

В предыдущем примере демонстрируется различие в поведении: метод расширения `Enumerable.Where` связан с массивом, а статический метод `String.Join` может быть вызван без ссылки на тип `String`.

### <a name="nameof-expressions"></a>Выражения NameOf
Иногда требуется ссылаться на имя, присвоенное переменной или полю. В C# 6`nameof(someVariableOrFieldOrType)`будет возвращать строку`"someVariableOrFieldOrType"`. Например, при вызове `ArgumentException` вам, скорее всего, потребуется дать имя, которое является недопустимым аргументом:

```csharp
throw new ArgumentException ("Problem with " + nameof(myInvalidArgument))
```

Главным преимуществом `nameof` выражений является то, что они проверены на наличие типов и совместимы с оптимизацией на основе инструментов. Проверка типа `nameof` выражений особенно приветствует в ситуациях, когда `string` используется для динамического связывания типов. Например, в iOS `string` используется для указания типа, используемого для создания прототипа `UITableViewCell` объектов в `UITableView`. `nameof` может гарантировать, что эта ассоциация не завершится ошибкой из-за ошибки или сыром рефакторинга:

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (nameof(CellTypeA), indexPath);
    cell.TextLabel.Text = objects [indexPath.Row].ToString ();
    return cell;
}
```

Хотя можно передать полное имя в `nameof`, возвращается только последний элемент (после последнего `.`). Например, можно добавить привязку данных в Xamarin. Forms:

```csharp
var myReactiveInstance = new ReactiveType ();
var myLabelOld.BindingContext = myReactiveInstance;
var myLabelNew.BindingContext = myReactiveInstance;
var myLabelOld.SetBinding (Label.TextProperty, "StringField");
var myLabelNew.SetBinding (Label.TextProperty, nameof(ReactiveType.StringField));
```

Два вызова `SetBinding` передают идентичные значения: `nameof(ReactiveType.StringField)` `"StringField"`, а не `"ReactiveType.StringField"` как можно изначально ожидать.

## <a name="null-conditional-operator"></a>Условный оператор null
Более ранние C# обновления предоставили основные понятия типов, допускающих значение null, и оператор объединения со значением NULL`??`, чтобы уменьшить объем стандартного кода при обработке значений, допускающих значение null. C#6. Эта тема будет содержать условный оператор "null-Conditional"`?.`. При использовании объекта в правой части выражения условный оператор null возвращает значение элемента, если объект не `null` и `null` в противном случае:

```csharp
var ss = new string[] { "Foo", null };
var length0 = ss [0]?.Length; // 3
var length1 = ss [1]?.Length; // null
var lengths = ss.Select (s => s?.Length ?? 0); //[3, 0]
```

(Как `length0`, так и `length1` выводятся в тип `int?`)

В последней строке предыдущего примера показан `?` операторе NULL-Conditional в сочетании с `??`ным оператором объединения со значением NULL. Новый C# 6 условный оператор null возвращает`null`для второго элемента в массиве, после чего оператор объединения со значением NULL запускается в и передает 0 в массив`lengths`(независимо от того, является ли это подходящим или нет, разумеется, зависит от конкретной проблемы).

Условный оператор NULL должен невероятно сократить объем стандартного проверки значений NULL, необходимый для многих приложений.

Существуют некоторые ограничения для оператора, основанного на значении NULL, из-за неоднозначности. Вы не можете сразу следовать `?` со списком аргументов в круглых скобках, так как вы, возможно, захотите сделать с делегатом:

```csharp
SomeDelegate?("Some Argument") // Not allowed
```

Однако `Invoke` можно использовать для разделения `?` из списка аргументов и по-прежнему является пометкой улучшения по сравнению с `null`ным блоком шаблона:

```csharp
public event EventHandler HandoffOccurred;
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    HandoffOccurred?.Invoke (this, userActivity.UserInfo);
    return true;
}
```

## <a name="string-interpolation"></a>Интерполяция строк
Функция `String.Format` традиционно использовала индексы в качестве заполнителей в строке формата, например, `String.Format("Expected: {0} Received: {1}.", expected, received`). Конечно, Добавление нового значения всегда применяет немало задач по инвентаризации аргументов, перенумерацию заполнителей и вставке нового аргумента в правой последовательности в списке аргументов.

C#6 Новая функция интерполяции строк значительно улучшилась после `String.Format`. Теперь можно напрямую присвоить переменные в строке с префиксом `$`. Пример

```csharp
$"Expected: {expected} Received: {received}."
```

Переменные, конечно, проверены, а также неправильно написанные или недоступные переменные могут вызвать ошибку компилятора.

Заполнители не обязательно должны быть простыми переменными, они могут быть любыми выражениями. Внутри этих заполнителей можно использовать кавычки *без* экранирования этих кавычек. Например, обратите внимание на `"s"` в следующем:

```csharp
var s = $"Timestamp: {DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}"
```

Интерполяция строк поддерживает синтаксис выравнивания и форматирования `String.Format`. Так же, как вы ранее написали C# `{index, alignment:format}`, в 6 вы пишете`{placeholder, alignment:format}`:

```csharp
using static System.Linq.Enumerable;
using System;

class Program
{
    static void Main ()
    {
        var values = new int[] { 1, 2, 3, 4, 12, 123456 };
        foreach (var s in values.Select (i => $"The value is { i,10:N2}.")) {
            Console.WriteLine (s);
        }
    Console.WriteLine ($"Minimum is { values.Min(i => i):N2}.");
    }
}
```

результаты в:

```
The value is       1.00.
The value is       2.00.
The value is       3.00.
The value is       4.00.
The value is      12.00.
The value is 123,456.00.
Minimum is 1.00.
```

Интерполяция строк является синтаксически SugarCRM для `String.Format`: она не может использоваться с `@""` строковыми литералами и несовместима с `const`, даже если не используются заполнители.

```csharp
const string s = $"Foo"; //Error : const requires value
```

В общем сценарии использования для создания аргументов функции с интерполяцией строк необходимо быть осторожным при отсчете ошибок, связанных с экранированием, кодировкой и культурой. Запросы SQL и URL-адреса, конечно же, важны для очистки. Как и в случае с `String.Format`, интерполяция строк использует `CultureInfo.CurrentCulture`. Использование `CultureInfo.InvariantCulture` — немного больше слов:

```csharp
Thread.CurrentThread.CurrentCulture  = new CultureInfo ("de");
Console.WriteLine ($"Today is: {DateTime.Now}"); //"21.05.2015 13:52:51"
Console.WriteLine ($"Today is: {DateTime.Now.ToString(CultureInfo.InvariantCulture)}"); //"05/21/2015 13:52:51"
```

## <a name="initialization"></a>Инициализация

C#6 предоставляет несколько кратких способов задания свойств, полей и элементов.

### <a name="auto-property-initialization"></a>Автоматическая инициализация свойства

Автоматические свойства теперь можно инициализировать в том же кратком виде, что и поля. Неизменяемые автоматические свойства могут быть записаны только с помощью метода получения:

```csharp
class ToDo
{
    public DateTime Due { get; set; } = DateTime.Now.AddDays(1);
    public DateTime Created { get; } = DateTime.Now;
```

В конструкторе можно задать значение автосвойств только для получения:

```csharp
class ToDo
{
    public DateTime Due { get; set; } = DateTime.Now.AddDays(1);
    public DateTime Created { get; } = DateTime.Now;
    public string Description { get; }

    public ToDo (string description)
    {
        this.Description = description; //Can assign (only in constructor!)
    }
```

Эта инициализация автосвойств является как общей функцией экономии пробелов, так и Boon разработчикам, желающим подчеркнуть неизменность своих объектов.

### <a name="index-initializers"></a>См. раздел Инициализаторы индекса.

C#6 вводит инициализаторы индексов, которые позволяют задать как ключ, так и значение в типах, имеющих индексатор. Как правило, это относится к структурам данных в стиле `Dictionary`:

```csharp
partial void ActivateHandoffClicked (WatchKit.WKInterfaceButton sender)
{
    var userInfo = new NSMutableDictionary {
        ["Created"] = NSDate.Now,
        ["Due"] = NSDate.Now.AddSeconds(60 * 60 * 24),
        ["Task"] = Description
    };
    UpdateUserActivity ("com.xamarin.ToDo.edit", userInfo, null);
    statusLabel.SetText ("Check phone");
}
```

### <a name="expression-bodied-function-members"></a>Члены функций Expression-воплощающего

Лямбда-функции имеют несколько преимуществ, одним из которых является простое сохранение пространства. Аналогичным образом, члены класса Expression-воплощающего позволяют выражать небольшие функции более кратко, чем было возможно в предыдущих версиях C# 6.

Члены функций Expression-воплощающего используют синтаксис лямбда-стрелок, а не традиционный синтаксис блока:

```csharp
public override string ToString () => $"{FirstName} {LastName}";
```

Обратите внимание, что синтаксис лямбда-стрелок не использует явный `return`. Для функций, возвращающих `void`, выражение также должно быть оператором:

```csharp
public void Log(string message) => System.Console.WriteLine($"{DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}: {message}");
```

К членам Expression-воплощающего по-прежнему применяются правила, которые `async` поддерживаются для методов, но не для свойств:

```csharp
//A method, so async is valid
public async Task DelayInSeconds(int seconds) => await Task.Delay(seconds * 1000);
//The following property will not compile
public async Task<int> LeisureHours => await Task.FromResult<char> (DateTime.Now.DayOfWeek.ToString().First()) == 'S' ? 16 : 5;
```

## <a name="exceptions"></a>Исключения

В этом нет двух способов: обработка исключений трудно получить правильно. Новые возможности в C# 6 делают обработку исключений более гибкой и стабильной.

### <a name="exception-filters"></a>Фильтры исключений

По определению исключения возникают в необычных обстоятельствах, поэтому может быть очень сложная причина и код по *всем* способам возникновения исключения определенного типа. C#6 предоставляет возможность защиты обработчика выполнения с помощью вычисляемого фильтра времени выполнения. Это можно сделать, добавив шаблон `when (bool)` после обычного объявления `catch(ExceptionType)`. В следующем примере фильтр различает ошибку синтаксического анализа, связанную с параметром `date`, а не с другими ошибками синтаксического анализа.

```csharp
public void ExceptionFilters(string aFloat, string date, string anInt)
{
    try
    {
        var f = Double.Parse(aFloat);
        var d = DateTime.Parse(date);
        var n = Int32.Parse(anInt);
    } catch (FormatException e) when (e.Message.IndexOf("DateTime") > -1) {
        Console.WriteLine ($"Problem parsing \"{nameof(date)}\" argument");
    } catch (FormatException x) {
        Console.WriteLine ("Problem parsing some other argument");
    }
}
```

### <a name="await-in-catchfinally"></a>ожидать при перехватывать... Наконец...

Возможности `async`, появившиеся C# в 5, были сменщиком игр для языка. В C# 5`await`не было разрешено в блоках`catch`и`finally`, неудобством, учитывая значение возможности`async/await`. C#6 снимает это ограничение, позволяя согласованно ожидать асинхронные результаты через программу, как показано в следующем фрагменте кода:

```csharp
async void SomeMethod()
{
    try {
        //...etc...
    } catch (Exception x) {
        var diagnosticData = await GenerateDiagnosticsAsync (x);
        Logger.log (diagnosticData);
    } finally {
        await someObject.FinalizeAsync ();
    }
}
```

## <a name="summary"></a>Сводка

C# Язык продолжит развиваться, чтобы сделать разработчиков более продуктивными, а также повысить эффективность работы и обеспечить поддержку инструментов. В этом документе представлен обзор новых возможностей языка в C# 6 и кратко продемонстрировано, как они используются.

## <a name="related-links"></a>Связанные ссылки

- [Новые возможности языка в C# 6](https://github.com/dotnet/roslyn/wiki/New-Language-Features-in-C%23-6)
