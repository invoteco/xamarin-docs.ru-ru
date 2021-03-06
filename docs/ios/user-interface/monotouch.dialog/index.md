---
title: Общие сведения о "несенсорном". диалоговое окно для Xamarin. iOS
description: В этом документе описывается котушь. Dialog (MT. D) — платформа для быстрой и декларативной разработки пользовательского интерфейса с помощью Xamarin. iOS. В нем обсуждается использование API-интерфейсов с возможностью касания для создания интерфейса в коде или JSON и использование таких функций, как извлечение и обновление, поиск, Загрузка фонового изображения и многое другое.
ms.prod: xamarin
ms.assetid: 52A35B24-C23B-8461-A8FF-5928A2128FB0
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: davidortinau
ms.author: daortin
ms.openlocfilehash: 68f8349fd6c8f90b36fb5edb2838dfec352a5800
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73002573"
---
# <a name="introduction-to-monotouchdialog-for-xamarinios"></a>Общие сведения о "несенсорном". диалоговое окно для Xamarin. iOS

Очень касание. диалоговое окно, называемое MT. D — это быстрый набор средств разработки пользовательского интерфейса, позволяющий разработчикам создавать экраны приложений и переходы с помощью информации, а не долгой создание контроллеров представления, таблиц и т. д. Таким образом, он обеспечивает значительное упрощение разработки пользовательского интерфейса и сокращения кода. Например, рассмотрим следующий снимок экрана:

 [![](images/image1.png "For example, consider this screenshot")](images/image1.png#lightbox)

Следующий код использовался для определения всего экрана:

```csharp
public enum Category
{
    Travel,
    Lodging,
    Books
}
        
public class Expense
{
    [Section("Expense Entry")]

    [Entry("Enter expense name")]
    public string Name;
    [Section("Expense Details")]
  
    [Caption("Description")]
    [Entry]
    public string Details;
        
    [Checkbox]
    public bool IsApproved = true;
    [Caption("Category")]
    public Category ExpenseCategory;
}
```

При работе с таблицами в iOS часто существует масса какой кода.
Например, каждый раз, когда требуется таблица, для заполнения этой таблицы требуется источник данных. В приложении с двумя экранами на основе таблиц, подключенными через контроллер навигации, каждый экран использует много одного и того же кода.

Машин. D упрощает это, инкапсулируют весь этот код в универсальный API для создания таблицы. Затем он предоставляет абстракцию поверх этого API, который обеспечивает декларативный синтаксис привязки объектов, что делает его еще проще. Таким образом, в MT доступны два API. Четырехмерного

- **API элементов нижнего уровня** — *API элементов* основан на создании иерархического дерева элементов, представляющих экраны и их компоненты. API элементов предоставляет разработчикам наибольшую гибкость и возможности управления при создании пользовательских интерфейсов. Кроме того, в API элементов реализована расширенная поддержка декларативного определения через JSON, что позволяет применять как невероятно быстрое объявление, так и динамическое создание пользовательского интерфейса с сервера. 
- **API-интерфейс высокоуровневого отражения** — также известный как*API* привязки, в котором классы записываются с помощью подсказок пользовательского интерфейса, а затем MT. D автоматически создает экраны на основе объектов и предоставляет привязку отображения (и при необходимости редактирования) на экране, а также резервное копирование базового объекта. В приведенном выше примере демонстрируется использование API отражения. Этот API не предоставляет детализированный элемент управления, который делает API элементов, но уменьшает сложность еще больше за счет автоматического создания иерархии элементов на основе атрибутов класса. 

Машин. D упакован с большим набором встроенных элементов пользовательского интерфейса для создания экрана, но он также распознает необходимость в настраиваемых элементах и дополнительных макетах экрана. Таким образом, расширяемость является первым классом, помогут в API. Разработчики могут расширять существующие элементы или создавать новые, а затем легко интегрировать их.

Кроме того, MT. У D есть ряд стандартных функций для работы с iOS, таких как поддержка "Pull-to-Refresh", асинхронная загрузка образов и поддержка поиска.

В этой статье подробно рассматривается работа с MT. Г, включая:

- **MT. D компонентов** — основное внимание уделяется пониманию классов, СОСТАВЛЯЮЩИХ MT. D для быстрого ускорения работы. 
- **Справочник по элементам** — полный список встроенных элементов MT. Четырехмерного. 
- **Расширенное использование** . здесь рассматриваются такие расширенные функции, как извлечение, поиск, Загрузка фоновых изображений, использование LINQ для построения иерархий элементов и создание пользовательских элементов, ячеек и контроллеров для использования с MT. Четырехмерного. 

## <a name="setting-up-mtd"></a>Настройка MT. Четырехмерного

Машин. D распространяется с помощью Xamarin. iOS. Чтобы использовать его, щелкните правой кнопкой мыши узел **ссылки** проекта Xamarin. iOS в Visual Studio 2017 или Visual Studio для Mac и добавьте ссылку на сборку " **котушь. Dialog-1** ". Затем при необходимости добавьте в исходный код инструкции `using MonoTouch.Dialog`.

## <a name="understanding-the-pieces-of-mtd"></a>Понимание частей MT. Четырехмерного

Даже при использовании API отражения MT. D создает иерархию элементов внутри, точно так же, как если бы она была создана через API элементов напрямую. Кроме того, поддержка JSON, упомянутая в предыдущем разделе, также создает элементы. По этой причине важно иметь базовое представление о составных частях MT. Четырехмерного.

Машин. D создает экраны, используя следующие четыре части:

- **диалогвиевконтроллер**
- **RootElement**
- **Section**
- **Элемент**

### <a name="dialogviewcontroller"></a>диалогвиевконтроллер

*Диалогвиевконтроллер*или *DVC* для short наследует от `UITableViewController` и, следовательно, представляет экран с таблицей. DVCs может быть отправлен на контроллер навигации так же, как обычный Уитаблевиевконтроллер.

### <a name="rootelement"></a>RootElement

*RootElement* — это контейнер верхнего уровня для элементов, которые переходят в DVC. Он содержит разделы, которые затем могут содержать элементы. Рутелементс не подготавливаются к просмотру; Вместо этого они просто являются контейнерами для того, что на самом деле готовится к просмотру. RootElement назначается DVC, а затем DVC отображает его дочерние элементы.

### <a name="section"></a>Раздел

Раздел — это группа ячеек в таблице. Как и в случае с обычным разделом таблицы, он может иметь верхний и нижний колонтитулы, которые могут быть либо текстовыми, либо даже пользовательскими представлениями, как показано на следующем снимке экрана:

 [![](images/image2.png "As with a normal table section, it can optionally have a header and footer that can either be text, or even custom views, as in this screenshot")](images/image2.png#lightbox)

### <a name="element"></a>Элемент

Элемент представляет фактическую ячейку в таблице. Машин. D упакован с широким спектром элементов, представляющих различные типы данных или различные входные данные. Например, на следующих снимках экрана показаны некоторые из доступных элементов:

 [![](images/image3.png "For example, this screenshots illustrate a few of the available elements")](images/image3.png#lightbox)

## <a name="more-on-sections-and-rootelements"></a>Дополнительные сведения о разделах и Рутелементс

Давайте теперь обсудим Рутелементс и разделы более подробно.

### <a name="rootelements"></a>рутелементс

По крайней мере один RootElement требуется для запуска процесса "некасание".

Если RootElement инициализируется с помощью значения Section/element, это значение используется для наведения дочернего элемента, который предоставит сводку по конфигурации, которая отображается в правой части экрана. Например, на следующем снимке экрана показана таблица слева с ячейкой, содержащей заголовок экрана сведений справа, "десерт", а также значение выбранной программы.

 [![](images/image4.png "На этом снимке экрана слева находится таблица, содержащая заголовок экрана сведений справа, десерт, а также значение выбранной автопередачи.")](images/image4.png#lightbox)[![](images/image5.png "На следующем снимке экрана показана Левая таблица с ячейкой, содержащей заголовок экрана сведений справа, десерт, а также значение выбранной автопередачи.")](images/image5.png#lightbox)

Корневые элементы можно также использовать внутри разделов для активации загрузки новой вложенной страницы конфигурации, как показано выше. При использовании в этом режиме указанный заголовок используется при отображении внутри раздела и также используется в качестве заголовка для вложенной страницы. Пример:

```csharp
var root = new RootElement ("Meals") {
    new Section ("Dinner") {
        new RootElement ("Dessert", new RadioGroup ("dessert", 2)) {
            new Section () {
                new RadioElement ("Ice Cream", "dessert"),
                new RadioElement ("Milkshake", "dessert"),
                new RadioElement ("Chocolate Cake", "dessert")
            }
        }
    }
};
```

В приведенном выше примере, когда пользователь нажмет "десерт", в диалоговом окне будет создана новая страница и выполнен переход к ней с корнем "десерт" и наличием группы переключателей с тремя значениями.

В этом конкретном примере группа переключателей выберет «шоколадный торт» в разделе «десерт», так как мы передали значение «2» в RadioGroup. Это означает выбор третьего элемента в списке (нулевой индекс).

При вызове метода Add или с C# помощью синтаксиса инициализатора 4 добавляются разделы.
Для вставки разделов с анимацией предоставляются методы Insert.

При создании RootElement с экземпляром группы (вместо RadioGroup) сводное значение RootElement, отображаемое в разделе, будет считаться совокупным количеством всех Булеанелементс и Чеккбокселементс, имеющих тот же ключ, что и значение Group. key.

### <a name="sections"></a>Разделы

Разделы используются для группировки элементов на экране, и они являются единственным допустимым прямым потомком RootElement. Разделы могут содержать любые стандартные элементы, включая New Рутелементс.

Рутелементс, внедренный в раздел, используется для перехода к новому более глубокому уровню.

Разделы могут иметь верхние и нижние колонтитулы как строки или как Уивиевс.
Обычно используются только строки, но для создания настраиваемых пользовательских интерфейсов можно использовать любой UIView в качестве верхнего или нижнего колонтитула. Можно использовать строку, чтобы создать их следующим образом:

```csharp
var section = new Section ("Header", "Footer");
```

Чтобы использовать представления, просто передайте представления в конструктор:

```csharp
var header = new UIImageView (Image.FromFile ("sample.png"));
var section = new Section (header);
```

### <a name="getting-notified"></a>Получение уведомления

#### <a name="handling-nsaction"></a>Обработка Нсактион

Машин. D предоставляет `NSAction` в качестве делегата для обработки обратных вызовов.
Например, предположим, что требуется выполнить обработку события касания для ячейки таблицы, созданной MT. Четырехмерного. При создании элемента с помощью MT. D, просто укажите функцию обратного вызова, как показано ниже:

```csharp
new Section () {
    new StringElement ("Demo Callback", delegate { Console.WriteLine ("Handled"); })
}
```

#### <a name="retrieving-element-value"></a>Получение значения элемента

В сочетании со свойством `Element.Value` обратный вызов может получить значение, заданное в других элементах. Рассмотрим следующий пример кода:

```csharp
var element = new EntryElement (task.Name, "Enter task description", task.Description);
                
var taskElement = new RootElement (task.Name) {
    new Section () { element },
    new Section () { new DateElement ("Due Date", task.DueDate) },
    new Section ("Demo Retrieving Element Value") {
        new StringElement ("Output Task Description", delegate { Console.WriteLine (element.Value); })
    }
};
```

Этот код создает пользовательский интерфейс, как показано ниже. Полное пошаговое руководство по этому примеру см. в разделе [Пошаговое руководство по API элементов](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) .

 [![](images/image6.png "Combined with the Element.Value property, the callback can retrieve the value set in other elements")](images/image6.png#lightbox)

Когда пользователь нажимает нижнюю ячейку таблицы, код в анонимной функции выполняется, записывая значение из `element` экземпляра на панель **выходных данных приложения** в Visual Studio для Mac.

## <a name="built-in-elements"></a>Встроенные элементы

Машин. D поставляется с рядом встроенных элементов ячеек таблицы, известных как элементы.
Эти элементы используются для вывода множества различных типов ячеек таблицы, таких как строки, числа с плавающей запятой, даты и даже изображения, для наименования всего лишь нескольких. Каждый элемент позаботится о правильном отображении типа данных. Например, логический элемент будет отображать переключатель для переключения его значения. Аналогично, элемент с плавающей запятой будет отображать ползунок для изменения значения float.

Существуют еще более сложные элементы для поддержки более разнообразных типов данных, таких как изображения и HTML. Например, элемент HTML, который будет открывать Уивебвиев для загрузки веб-страницы при выборе, отображает заголовок в ячейке таблицы.

### <a name="working-with-element-values"></a>Работа со значениями элементов

Элементы, используемые для записи вводимых пользователем данных, предоставляют открытое свойство `Value`, которое хранит текущее значение элемента в любое время. Он обновляется автоматически по мере того, как пользователь использует приложение.

Это поведение для всех элементов, которые являются частью одноэлементного диалогового окна, но не являются обязательными для элементов, созданных пользователем.

### <a name="string-element"></a>Строковый элемент

`StringElement` отображает заголовок слева от ячейки таблицы и строковое значение в правой части ячейки.

 [![](images/image7.png "A StringElement shows a caption on the left side of a table cell and the string value on the right side of the cell")](images/image7.png#lightbox)

Чтобы использовать `StringElement` в качестве кнопки, укажите делегат.

```csharp
new StringElement ("Click me", () => { 
    new UIAlertView("Tapped", "String Element Tapped", null, "ok", null).Show();
});
```

 [![](images/image8.png "To use a StringElement as a button, provide a delegate")](images/image8.png#lightbox)

### <a name="styled-string-element"></a>Элемент строки в стиле

`StyledStringElement` позволяет представлять строки либо с помощью встроенных стилей ячеек таблицы, либо с помощью пользовательского форматирования.

 [![](images/image9.png "A StyledStringElement allows strings to be presented using either built-in table cell styles or with custom formatting")](images/image9.png#lightbox)

Класс `StyledStringElement` является производным от `StringElement`, но позволяет разработчикам настраивать несколько свойств, таких как шрифт, цвет текста, цвет ячейки фона, режим разрыва строки, число отображаемых строк и необходимость отображения принадлежности.

### <a name="multiline-element"></a>Многострочный элемент

 [![](images/image10.png "Multiline Element")](images/image10.png#lightbox)

### <a name="entry-element"></a>Entry, элемент

`EntryElement`, как предполагается в названии, используется для получения вводимых пользователем данных. Он поддерживает обычные строки или пароли, в которых символы скрыты.

 [![](images/image11.png "The EntryElement is used to get user input")](images/image11.png#lightbox)

Он инициализируется тремя значениями:

- Заголовок для записи, которая будет отображаться для пользователя.
- Замещающий текст (это серый текст, предоставляющий пользователю подсказку). 
- Значение текста.

Заполнитель и значение могут быть равны NULL. Однако заголовок является обязательным.

В любой момент доступ к свойству Value может получить значение `EntryElement`.

Кроме того, во время создания свойства `KeyboardType` можно задать стиль клавиатуры, необходимый для ввода данных. Это можно использовать для настройки клавиатуры, используя значения `UIKeyboardType`, как указано ниже.

- Numeric
- Телефон
- URL-адрес
- Адрес эл. почты

### <a name="boolean-element"></a>Логический элемент

 [![](images/image12.png "Boolean Element")](images/image12.png#lightbox)

### <a name="checkbox-element"></a>Элемент CheckBox

 [![](images/image13.png "Checkbox Element")](images/image13.png#lightbox)

### <a name="radio-element"></a>Элемент Radio

Для `RadioElement` требуется указать `RadioGroup` в `RootElement`.

```csharp
mtRoot = new RootElement ("Demos", new RadioGroup("MyGroup", 0));
```

 [![](images/image14.png "A RadioElement requires a RadioGroup to be specified in the RootElement")](images/image14.png#lightbox)

 `RootElements` также используются для координации элементов радио. Элементы `RadioElement` могут охватывать несколько разделов (например, для реализации чего-то, как в селекторе мелодии звонка, а также для отдельных мелодий звонка из системных колец). В представлении "Сводка" будет показан элемент "переключатель", выбранный в данный момент. Чтобы использовать его, создайте `RootElement` с помощью конструктора группы, как в следующем:

```csharp
var root = new RootElement ("Meals", new RadioGroup ("myGroup", 0));
```

Имя группы в `RadioGroup` используется для отображения выбранного значения на содержащей странице (если оно есть), а значение, равное нулю, является индексом первого выбранного элемента.

### <a name="badge-element"></a>Элемент эмблемы

 [![](images/image15.png "Badge Element")](images/image15.png#lightbox)

### <a name="float-element"></a>Float, элемент

 [![](images/image16.png "Float Element")](images/image16.png#lightbox)

### <a name="activity-element"></a>Элемент Activity

 [![](images/image17.png "Activity Element")](images/image17.png#lightbox)

### <a name="date-element"></a>Элемент Date

 ![](images/image18.png "Date Element")

Когда выбрана ячейка, соответствующая Датилемент, отображается элемент выбора даты, как показано ниже:

 [![](images/image19.png "When the cell corresponding to the DateElement is selected, a date picker is presented as shown")](images/image19.png#lightbox)

### <a name="time-element"></a>Элемент Time

 [![](images/image20.png "Time Element")](images/image20.png#lightbox)

Когда выбрана ячейка, соответствующая Тимилемент, отображается средство выбора времени, как показано ниже:

 [![](images/image21.png "When the cell corresponding to the TimeElement is selected, a time picker is presented as shown")](images/image21.png#lightbox)

### <a name="datetime-element"></a>Элемент DateTime

 [![](images/image22.png "DateTime Element")](images/image22.png#lightbox)

Если выбрана ячейка, соответствующая Датетимилемент, отображается средство выбора даты и времени, как показано ниже:

 [![](images/image23.png "When the cell corresponding to the DateTimeElement is selected, a datetime picker is presented as shown")](images/image23.png#lightbox)

### <a name="html-element"></a>HTML-элемент

 [![](images/image24.png "HTML Element")](images/image24.png#lightbox)

`HTMLElement` отображает значение его свойства `Caption` в ячейке таблицы. Вхе выбрано, `Url`, назначенный элементу, загружается в элемент управления `UIWebView`, как показано ниже:

 [![](images/image25.png "Whe selected, the Url assigned to the element is loaded in a UIWebView control as shown below")](images/image25.png#lightbox)

### <a name="message-element"></a>Элемент Message

 [![](images/image26.png "Message Element")](images/image26.png#lightbox)

### <a name="load-more-element"></a>Загрузить дополнительный элемент

Используйте этот элемент, чтобы разрешить пользователям загружать больше элементов в список. Можно настроить нормальную и загрузку заголовков, а также цвет шрифта и текста.
Индикатор `UIActivity` запускает анимацию, а заголовок загрузки отображается, когда пользователь касается ячейки, а затем выполняется `NSAction`, переданный в конструктор. После завершения выполнения кода в `NSAction` индикатор `UIActivity` останавливает анимацию, а стандартный заголовок отображается снова.

### <a name="uiview-element"></a>UIView, элемент

Кроме того, любое пользовательское `UIView` можно отобразить с помощью `UIViewElement`.

### <a name="owner-drawn-element"></a>Рисуемый владельцем элемент

Этот элемент должен быть подклассом, так как он является абстрактным классом. Следует переопределить метод `Height(RectangleF bounds)`, в котором следует возвращать высоту элемента, а также `Draw(RectangleF bounds, CGContext context, UIView view)`, в которой необходимо выполнять все настроенные рисунки в пределах заданных границ, используя параметры контекста и представления. Этот элемент выполняет тяжелую работу с подклассом `UIView`и помещением его в возвращаемую ячейку, при этом необходимо только реализовать два простых переопределения. В примере приложения в файле `DemoOwnerDrawnElement.cs` можно увидеть лучшую реализацию примера.

Вот очень простой пример реализации класса:

```csharp
public class SampleOwnerDrawnElement : OwnerDrawnElement
{
    public SampleOwnerDrawnElement (string text) : base(UITableViewCellStyle.Default, "sampleOwnerDrawnElement")
    {
        this.Text = text;
    }

    public string Text { get; set; }

    public override void Draw (RectangleF bounds, CGContext context, UIView view)
    {
        UIColor.White.SetFill();
        context.FillRect(bounds);

        UIColor.Black.SetColor();   
        view.DrawString(this.Text, new RectangleF(10, 15, bounds.Width - 20, bounds.Height - 30), UIFont.BoldSystemFontOfSize(14.0f), UILineBreakMode.TailTruncation);
    }

    public override float Height (RectangleF bounds)
    {
        return 44.0f;
    }
}
```

### <a name="json-element"></a>Элемент JSON

`JsonElement` является подклассом `RootElement`, который расширяет `RootElement`, чтобы иметь возможность загружать содержимое вложенного дочернего элемента с локального или удаленного URL-адреса.

`JsonElement` — это `RootElement`, который можно создать в двух формах. Одна версия создает `RootElement`, который будет загружать содержимое по запросу. Они создаются с помощью конструкторов `JsonElement`, которые принимают дополнительный аргумент в конце, URL-адрес для загрузки содержимого:

```csharp
var je = new JsonElement ("Dynamic Data", "https://tirania.org/tmp/demo.json");
```

Другая форма создает данные из локального файла или существующего `System.Json.JsonObject`, который уже проанализирован:

```csharp
var je = JsonElement.FromFile ("json.sample");
using (var reader = File.OpenRead ("json.sample"))
    return JsonElement.FromJson (JsonObject.Load (reader) as JsonObject, arg);
```

Дополнительные сведения об использовании JSON с MT. Г. см. [Пошаговое руководство по элементу JSON](https://docs.microsoft.com/xamarin/ios/user-interface/monotouch.dialog/json-element-walkthrough) .

## <a name="other-features"></a>Другие функции

### <a name="pull-to-refresh-support"></a>Поддержка по запросу на обновление

 Получение *к* *обновлению* — это визуальный элемент, изначально находящийся в приложении *Tweetie2* , который стал популярным воздействием на многие приложения.

Чтобы добавить в диалоговых окнах автоматическую поддержку обновления по запросу, необходимо выполнить два действия: подключить обработчик событий, чтобы получать уведомления, когда пользователь извлекает данные и уведомляет `DialogViewController` о загрузке данных для возврата к состоянию по умолчанию.

Присоединиться к уведомлению очень просто; Просто подключитесь к событию `RefreshRequested` на `DialogViewController`следующим образом:

```csharp
dvc.RefreshRequested += OnUserRequestedRefresh;
```

Затем в методе, `OnUserRequestedRefresh`, вы поставите в очередь какую-либо загрузку данных, запросите некоторые данные из сети или прокрутить поток, чтобы вычислить данные. После загрузки данных необходимо уведомить `DialogViewController` о том, что в них находятся новые данные, и восстановить представление в состояние по умолчанию, вызвав `ReloadComplete`:

```csharp
dvc.ReloadComplete ();
```

### <a name="search-support"></a>Поддержка поиска

Для поддержки поиска задайте свойство `EnableSearch` в `DialogViewController`. Можно также задать свойство `SearchPlaceholder` для использования в качестве текста водяного знака в строке поиска.

При поиске содержимое представления изменится на тип пользователя. Он выполняет поиск видимых полей и показывает их пользователю. `DialogViewController` предоставляет три метода для программной инициации, завершения или запуска новой операции фильтра для результатов. Ниже перечислены эти методы.

- `StartSearch`
- `FinishSearch`
- `PerformFilter`

Система является расширяемой, поэтому при необходимости можно изменить это поведение.

### <a name="background-image-loading"></a>Фоновая загрузка изображений

В диалоговом окне "некасание" содержится загрузчик образов приложения [твитстатион](https://github.com/migueldeicaza/TweetStation) . Этот загрузчик изображений можно использовать для загрузки изображений в фоновом режиме, поддерживает кэширование и может уведомлять ваш код о загрузке изображения.

Кроме того, будет ограничено количество исходящих сетевых подключений.

Загрузчик изображений реализован в классе `ImageLoader`, все, что нужно сделать, — это вызвать метод `DefaultRequestImage`, необходимо предоставить универсальный код ресурса (URI) для загружаемого образа, а также экземпляр интерфейса `IImageUpdated`, который будет вызываться, когда изображение загружено.

Например, следующий код загружает изображение из URL-адреса в `BadgeElement`:

```csharp
string uriString = "http://some-server.com/some image url";

var rootElement = new RootElement("Image Loader") {
    new Section() {
        new BadgeElement( ImageLoader.DefaultRequestImage( new Uri(uriString), this), "Xamarin")
    }
};
```

Класс ImageLoader предоставляет метод очистки, который можно вызвать, если требуется освободить все образы, которые в данный момент находятся в кэше в памяти. В текущем коде имеется кэш для изображений 50. Если вы хотите использовать другой размер кэша (например, если вы ожидаете, что образы будут слишком велики, что 50 изображений будет слишком большим), можно просто создать экземпляры ImageLoader и передать количество образов, которые нужно хранить в кэше.

## <a name="using-linq-to-create-element-hierarchy"></a>Использование LINQ для создания иерархии элементов

Благодаря разумному использованию синтаксиса инициализации C#LINQ и LINQ можно использовать для создания иерархии элементов. Например, следующий код создает экран из некоторых массивов строк и обрабатывает выделение ячеек с помощью анонимной функции, которая передается в каждый `StringElement`:

```csharp
var rootElement = new RootElement ("LINQ root element") {
    from x in new string [] { "one", "two", "three" }
    select new Section (x) {
        from y in "Hello:World".Split (':')
        select (Element) new StringElement (y, delegate { Debug.WriteLine("cell tapped"); })
    }
};
```

Это можно легко объединить с хранилищем данных XML или данными из базы данных, чтобы создавать сложные приложения практически полностью на основе данных.

## <a name="extending-mtd"></a>Расширение MT. Четырехмерного

### <a name="creating-custom-elements"></a>Создание настраиваемых элементов

Вы можете создать собственный элемент путем наследования из существующего элемента или путем наследования от корневого элемента класса.

Чтобы создать собственный элемент, потребуется переопределить следующие методы:

```csharp
// To release any heavy resources that you might have
void Dispose (bool disposing);

// To retrieve the UITableViewCell for your element
// you would need to prepare the cell to be reused, in the
// same way that UITableView expects reusable cells to work
UITableViewCell GetCell (UITableView tv);

// To retrieve a "summary" that can be used with
// a root element to render a summary one level up.  
string Summary ();

// To detect when the user has tapped on the cell
void Selected (DialogViewController dvc, UITableView tableView, NSIndexPath path);

// If you support search, to probe if the cell matches the user input
bool Matches (string text);
```

Если элемент может иметь переменный размер, необходимо реализовать интерфейс `IElementSizing`, который содержит один метод:

```csharp
// Returns the height for the cell at indexPath.Section, indexPath.Row
float GetHeight (UITableView tableView, NSIndexPath indexPath);
```

Если вы планируете реализовать метод `GetCell` путем вызова `base.GetCell(tv)` и настройки возвращаемой ячейки, необходимо также переопределить свойство `CellKey`, чтобы возвращался ключ, который будет уникальным для элемента, например:

```csharp
static NSString MyKey = new NSString ("MyKey");
protected override NSString CellKey {
    get {
        return MyKey;
    }
}
```

Это работает для большинства элементов, но не для `StringElement` и `StyledStringElement`, так как они используют собственный набор ключей для различных сценариев отрисовки. Вам пришлось бы реплицировать код в этих классах.

### <a name="dialogviewcontrollers-dvcs"></a>Диалогвиевконтроллерс (DVCs)

Как отражение, так и API элементов используют один и тот же `DialogViewController`. Иногда требуется настроить внешний вид представления или использовать некоторые функции `UITableViewController`, которые выходят за рамки базового создания пользовательских интерфейсов.

`DialogViewController` является просто подклассом `UITableViewController` и его можно настроить так же, как и `UITableViewController`.

Например, если вы хотите изменить стиль списка так, чтобы он `Grouped` или `Plain`, это значение можно задать, изменив свойство при создании контроллера следующим образом:

```csharp
var myController = new DialogViewController (root, true) {
    Style = UITableViewStyle.Grouped;
}
```

Для более сложных настроек `DialogViewController`, например для настройки его фона, необходимо создать подкласс для него и переопределить соответствующие методы, как показано в следующем примере:

```csharp
class SpiffyDialogViewController : DialogViewController {
    UIImage image;

    public SpiffyDialogViewController (RootElement root, bool pushing, UIImage image) 
        : base (root, pushing) 
    {
        this.image = image;
    }

    public override LoadView ()
    {
        base.LoadView ();
        var color = UIColor.FromPatternImage(image);
        TableView.BackgroundColor = UIColor.Clear;
        ParentViewController.View.BackgroundColor = color;
    }
}
```

Еще одна точка настройки является следующими виртуальными методами в `DialogViewController`:

```csharp
public override Source CreateSizingSource (bool unevenRows)
```

Этот метод должен возвращать подкласс `DialogViewController.Source` для случаев, когда ячейки имеют равномерное изменение размера, или подкласс `DialogViewController.SizingSource`, если ячейки нечетными.

Это переопределение можно использовать для захвата любого из методов `UITableViewSource`. Например, [твитстатион](https://github.com/migueldeicaza/TweetStation) использует этот метод для наблюдения за прокруткой пользователя к верху и обновления соответствующим числом непрочтенных твитов.

## <a name="validation"></a>Проверка

Элементы не предоставляют сами проверки, так как модели, которые хорошо подходят для веб-страниц и классических приложений, не сопоставляются непосредственно с моделью взаимодействия iPhone.

Если вы хотите выполнить проверку данных, это следует делать, когда пользователь запускает действие с введенными данными. Например, кнопка **Готово** или **Далее** на верхней панели инструментов или некоторые `StringElement`, используемые в качестве кнопки для перехода к следующему этапу.

Именно здесь выполняется базовая проверка ввода и, возможно, более сложная проверка, например проверка допустимости сочетания пользователя и пароля с сервером.

Уведомление пользователя об ошибке зависит от приложения. Вы можете открыть `UIAlertView` или Показать подсказку.

## <a name="summary"></a>Сводка

В этой статье мы рассмотрели множество сведений о том, как было бы очень Touch. В нем обсуждались фундаментальные сведения о том, как MT. D работает и охватывает различные компоненты, составляющие MT. Четырехмерного. Он также показал широкий массив элементов и настроек таблиц, поддерживаемых MT. D и обсуждалось, как MT. D можно расширить с помощью настраиваемых элементов. Кроме того, в нем объясняется поддержка JSON в MT. D, позволяющий динамически создавать элементы из JSON.

## <a name="related-links"></a>Связанные ссылки

- [Пошаговое руководство. Создание приложения с помощью API элементов](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Пошаговое руководство. Создание приложения с помощью API отражения](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Пошаговое руководство. Использование элемента JSON для создания пользовательского интерфейса](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [Разметка JSON с несенсорным касанием](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Диалоговое окно с несенсорным касанием на GitHub](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [Справочник по классам Уитаблевиевконтроллер](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Справочник по классам Уинавигатионконтроллер](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
