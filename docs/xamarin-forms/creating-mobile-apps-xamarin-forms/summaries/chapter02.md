---
title: Сводка Глава 2. Анатомия приложения
description: 'Создание мобильных приложений с помощью Xamarin.Forms: Сводка Глава 2. Анатомия приложения'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8764EB7D-8331-4CF7-9BE1-26D0DEE9E0BB
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
ms.openlocfilehash: f900cb1532ba4415127c95b07e777881e1d74994
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724998"
---
# <a name="summary-of-chapter-2-anatomy-of-an-app"></a>Сводка Глава 2. Анатомия приложения

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)

> [!NOTE]
> Заметки на этой странице указывать области, где различаются Xamarin.Forms материал, представленный в книге.

В приложении Xamarin. Forms объекты, занимающие место на экране, называются *визуальными элементами*, инкапсулированными классом [`VisualElement`](xref:Xamarin.Forms.VisualElement) . Визуальные элементы можно разделить на три категории, соответствующие этих классов:

- [Страница](xref:Xamarin.Forms.Page)
- [Макет](xref:Xamarin.Forms.Layout)
- [View](xref:Xamarin.Forms.View) (Вид)

Производная `Page` занимает весь экран или почти весь экран. Часто дочерняя страница страницы является `Layout` производной для организации дочерних визуальных элементов. Дочерние элементы `Layout` могут быть другими `Layout`ными классами или `View` производными (часто называемыми *элементами*), которые являются привычными объектами, такими как текст, точечные рисунки, ползунки, кнопки, списки и т. д.

В этой главе показано, как создать приложение с помощью [`Label`](xref:Xamarin.Forms.Label), которое является `View` производным, отображающим текст.

## <a name="say-hello"></a>Поприветствуйте

На платформе Xamarin установлен можно создать новое решение Xamarin.Forms в Visual Studio или Visual Studio для Mac. Решение [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello) использует переносимую библиотеку классов для общего кода.

> [!NOTE]
> Переносимые библиотеки классов были заменены библиотеки .NET Standard. Все примеры кода из книги будет преобразована использовать стандартные библиотеки .NET.

В этом примере демонстрируется решение Xamarin.Forms, созданные в Visual Studio без изменений. Решение состоит из четырех проектов:

- [**Привет**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello), Переносимая библиотека классов (PCL), совместно используемая другими проектами
- [**Hello. Droid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid), проект приложения для Android
- [**Hello. iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS), проект приложения для iOS
- [**Hello. UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP), проект приложения для универсальная платформа Windows (Windows 10 и Windows 10 Mobile)

> [!NOTE]
> Xamarin.Forms больше не поддерживает Windows 8.1, Windows Phone 8.1 или Windows 10 Mobile, но приложения Xamarin.Forms выполняются на рабочем столе Windows 10.

Внесите любые из этих проектов приложений запускаемого проекта и затем постройте и запустите программу на устройства или симулятора.

Во многих приложениях Xamarin.Forms не изменяя проектов приложений. Они часто остаются tiny заглушки просто для того, чтобы запустить программу. Большая часть фокус будет библиотека, общими для всех приложений.

## <a name="inside-the-files"></a>В файлах

Визуальные элементы, отображаемые программой **Hello** , определяются в конструкторе класса [`App`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs) . `App` является производным от класса Xamarin. Forms [`Application`](xref:Xamarin.Forms.Application).

> [!NOTE]
> Шаблоны решений Visual Studio для Xamarin.Forms создайте страницу с файлом XAML. КОД XAML в этой книге не рассматривается до начала [главы 7](chapter07.md).

Раздел **References** проекта **Hello** PCL содержит следующие сборки Xamarin. Forms:

- **Xamarin. Forms. Core**
- **Xamarin. Forms. XAML**
- **Xamarin. Forms. Platform**

Разделы **References** из пяти проектов приложений включают дополнительные сборки, применимые к отдельным платформам:

- **Xamarin. Forms. Platform. Android**
- **Xamarin. Forms. Platform. iOS**
- **Xamarin. Forms. Platform. UWP**
- **Xamarin. Forms. Platform. WinRT**
- **Xamarin. Forms. Platform. WinRT. планшет**
- **Xamarin. Forms. Platform. WinRT. Phone**

> [!NOTE]
> Разделы **ссылок** в этих проектах больше не содержат сборки. Вместо этого файл проекта содержит теги **PackageReference** , ссылающиеся на пакет NuGet для Xamarin. Forms. Раздел **References** в Visual Studio перечисляет пакет **Xamarin. Forms** , а не сборки Xamarin. Forms.

Каждый проект приложения содержит вызов статического метода `Forms.Init` в пространстве имен `Xamarin.Forms`. Это инициализирует библиотеку Xamarin.Forms. Для каждой платформы определена другая версия `Forms.Init`. Вызовы этого метода можно найти в следующих классов:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [класс`App`, метод `OnLaunched`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)

Кроме того, каждая платформа должна создать экземпляр расположения `App` класса в общей библиотеке. Это происходит при вызове `LoadApplication` в следующих классах:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs)

В противном случае эти проекты приложений — это обычный программы «ничего не делать».

## <a name="pcl-or-sap"></a>PCL или SAP?

Имеется возможность создать решение Xamarin.Forms с общим кодом в переносимой библиотеки классов (PCL) или общего ресурса проекта (SAP). Создание решения SAP, выберите параметр Shared в Visual Studio. Решение [**хеллосап**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap) ДЕМОНСТРИРУЕТ шаблон SAP без изменений.

> [!NOTE]
> Переносимые библиотеки классов была заменена библиотеки .NET Standard. Все примеры кода из книги будет преобразована использовать стандартные библиотеки .NET. В противном случае библиотеки PCL и .NET Standard концептуально очень похожи.

Пакеты библиотеки подход, общего кода в проекте библиотеки ссылаться проекты приложения платформы. При использовании SAP общий код фактически существует во всех проектах приложения платформы и распределяется между ними.

Большинство разработчиков Xamarin.Forms предпочитают подход библиотеки. В этой книге большую часть решений использовать библиотеку. В именах проектов, в которых используется SAP, есть суффикс **SAP** .

Благодаря подходу SAP код в общем проекте может выполнять разные коды для различных платформ, используя C# директивы препроцессора (`#if`, #`elif`и `#endif`) с этими предопределенными идентификаторами:

- iOS: `__IOS__`
- Android: `__ANDROID__`
- UWP: `WINDOWS_UWP`

В общей библиотеке выяснить, какие платформы, вы используете во время выполнения, как вы увидите далее в этой главе.

## <a name="labels-for-text"></a>Метки для текста

В решении " [**поздравления**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings) " показано, как C# добавить новый файл в проект **поздравлений** . Этот файл определяет класс с именем `GreetingsPage`, производный от `ContentPage`. В этой книге большинство проектов содержат один производный `ContentPage`, имя которого является именем проекта с суффиксом `Page` добавить.

Конструктор `GreetingsPage` создает экземпляр [`Label`](xref:Xamarin.Forms.Label) представления, которое является представлением Xamarin. Forms, отображающим текст. Для свойства [`Text`](xref:Xamarin.Forms.Label.Text) задается текст, отображаемый `Label`. Эта программа задает `Label` свойству `Content` `ContentPage`. Затем конструктор класса `App` создает экземпляр `GreetingsPage` и присваивает его свойству `MainPage`.

Текст отображается в левом верхнем углу страницы. В iOS это означает, что он перекрывает строки состояния страницы. Существует несколько решений этой проблемы:

### <a name="solution-1-include-padding-on-the-page"></a>Решение 1. Включить внутренние поля на странице

Задайте свойство [`Padding`](xref:Xamarin.Forms.Page.Padding) на странице. `Padding` имеет тип [`Thickness`](xref:Xamarin.Forms.Thickness), структура с четырьмя свойствами:

- [`Left`](xref:Xamarin.Forms.Thickness.Left)
- [`Top`](xref:Xamarin.Forms.Thickness.Top)
- [`Right`](xref:Xamarin.Forms.Thickness.Right)
- [`Bottom`](xref:Xamarin.Forms.Thickness.Bottom)

`Padding` определяет область внутри страницы, где содержимое исключено. Это позволяет `Label` избежать перезаписи строки состояния iOS.

### <a name="solution-2-include-padding-just-for-ios-sap-only"></a>Решение 2. Включить внутренние поля только для iOS (только для SAP)

Задать свойство «Заполнение» только для iOS с помощью SAP с C# директивы препроцессора. Это продемонстрировано в решении [**гритингссап**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap) .

### <a name="solution-3-include-padding-just-for-ios-pcl-or-sap"></a>Решение 3. Включить внутренние поля только для iOS (переносимой библиотеки Классов или SAP)

В версии Xamarin. Forms, используемой для книги, можно выбрать свойство `Padding`, характерное для iOS либо в PCL, либо в SAP, используя статический метод [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) или [`Device.OnPlatform<T>`](xref:Xamarin.Forms.Device.OnPlatform*) . Эти методы являются нерекомендуемыми

Методы `Device.OnPlatform` используются для выполнения кода, зависящего от платформы, или для выбора значений, зависящих от платформы. На внутреннем уровне они используют [`Device.OS`](xref:Xamarin.Forms.Device.OS) статическое свойство только для чтения, которое возвращает член перечисления [`TargetPlatform`](xref:Xamarin.Forms.TargetPlatform) :

- [`iOS`](xref:Xamarin.Forms.TargetPlatform.iOS)
- [`Android`](xref:Xamarin.Forms.TargetPlatform.Android)
- [`Windows`](xref:Xamarin.Forms.TargetPlatform.Windows) для устройств UWP.

Методы `Device.OnPlatform`, свойство `Device.OS` и перечисление `TargetPlatform` теперь являются нерекомендуемыми. Вместо этого используйте свойство [`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform) и сравните `string` возвращаемое значение со следующими статическими полями:

- [`iOS`](xref:Xamarin.Forms.Device.iOS), строка "iOS"
- [`Android`](xref:Xamarin.Forms.Device.Android), строка "Android"
- [`UWP`](xref:Xamarin.Forms.Device.UWP), строка "UWP", ссылающаяся на универсальная платформа Windows

[`Device.Idiom`](xref:Xamarin.Forms.Device.Idiom) статическое свойство только для чтения связано. Он возвращает элемент [`TargetIdiom`](xref:Xamarin.Forms.TargetIdiom), который имеет следующие члены:

- [`Desktop`](xref:Xamarin.Forms.TargetIdiom.Desktop)
- [`Tablet`](xref:Xamarin.Forms.TargetIdiom.Tablet)
- [`Phone`](xref:Xamarin.Forms.TargetIdiom.Phone)
- [`Unsupported`](xref:Xamarin.Forms.TargetIdiom.Unsupported) не используется

Для iOS и Android отсечение между `Tablet` и `Phone` имеет портретную ширину 600 единиц. Для платформы Windows `Desktop` указывает на приложение UWP под управлением Windows 10, а `Phone` указывает на приложение UWP, работающее под управлением Windows 10.

## <a name="solution-3a-set-margin-on-the-label"></a>Решение 3a. Задание границы метки

Свойство [`Margin`](xref:Xamarin.Forms.View.Margin) было введено слишком поздно для включения в книгу, но оно также имеет тип `Thickness` и его можно задать на `Label`, чтобы определить область за пределами представления, включенного в вычисление макета представления.

Свойство `Padding` доступно только для производных [`Layout`](xref:Xamarin.Forms.Layout) и [`Page`](xref:Xamarin.Forms.Page) . Свойство `Margin` доступно для всех производных [`View`](xref:Xamarin.Forms.View) .

## <a name="solution-4-center-the-label-within-the-page"></a>Решение 4. Center метку в странице

Можно центрировать `Label` в `Page` (или поместить его в одно из восьми других мест), задав для свойств [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) и [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) `Label` значение типа [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions). Структура `LayoutOptions` определяет два свойства:

- Свойство [`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment) типа [`LayoutAlignment`](xref:Xamarin.Forms.LayoutAlignment), перечисление с четырьмя членами: [`Start`](xref:Xamarin.Forms.LayoutAlignment.Start), которое означает Left или top в зависимости от ориентации, [`Center`](xref:Xamarin.Forms.LayoutAlignment.Center), [`End`](xref:Xamarin.Forms.LayoutAlignment.End), что означает право или низ в зависимости от ориентации и [`Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill).

- Свойство [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) типа `bool`.

Обычно эти свойства не используются напрямую. Вместо этого сочетания этих двух свойств предоставляются восемью статическими свойствами только для чтения типа `LayoutOptions`:

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

`HorizontalOptions` и `VerticalOptions` являются наиболее важными свойствами в макете Xamarin. Forms и более подробно обсуждаются в [**главе 4. Прокрутка стека**](chapter04.md).

Ниже приведен результат, в котором свойства `HorizontalOptions` и `VerticalOptions` `Label` оба имеют значение `LayoutOptions.Center`:

[![Тройной снимок экрана программы поздравления](images/ch02fg05-small.png "Горизонтальная и вертикальная метка по центру")](images/ch02fg05-large.png#lightbox "Горизонтальная и вертикальная метка по центру")

## <a name="solution-5-center-the-text-within-the-label"></a>Решение 5. Центрирование текста в метке

Можно также центрировать текст (или поместить его в восемь других расположений на странице), установив свойства [`HorizontalTextAlignment`](xref:Xamarin.Forms.Label.HorizontalTextAlignment) и [`VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment) `Label` на элемент перечисления [`TextAlignment`](xref:Xamarin.Forms.TextAlignment) .

- [`Start`](xref:Xamarin.Forms.TextAlignment.Start), то есть влево или сверху (в зависимости от ориентации)
- [`Center`](xref:Xamarin.Forms.TextAlignment.Center)
- [`End`](xref:Xamarin.Forms.TextAlignment.End), то есть право или снизу (в зависимости от ориентации)

Эти два свойства определяются только `Label`, тогда как свойства `HorizontalAlignment` и `VerticalAlignment` определяются `View` и наследуются всеми производными `View`. Результаты visual покажутся схожими, но они сильно отличаются как показано в следующей главе.

## <a name="related-links"></a>Связанные ссылки

- [Глава 2. полный текст (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch02-Apr2016.pdf)
- [Глава 2. примеры](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)
- [Глава 2 F# . примеры](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/FS)
- [Начало работы с помощью Xamarin. Forms](~/get-started/index.yml)
