---
title: Windows в Xamarin. Mac
description: В этой статье рассматривается работа с окнами и панелями в приложении Xamarin. Mac. Здесь описывается создание окон и панелей в Xcode и Interface Builder, загрузка их из файлов Storyboard и XIB и работа с ними программными средствами.
ms.prod: xamarin
ms.assetid: 4F6C67E9-BBFF-44F7-B29E-AB47D7F44287
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: c3355b2696b5c40ee11a289e700239900698dcf1
ms.sourcegitcommit: 55167ad9db910c5c0eb5a84c0923cb07acd2530e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/02/2020
ms.locfileid: "75606714"
---
# <a name="windows-in-xamarinmac"></a>Windows в Xamarin. Mac

_В этой статье рассматривается работа с окнами и панелями в приложении Xamarin. Mac. Здесь описывается создание окон и панелей в Xcode и Interface Builder, загрузка их из файлов Storyboard и XIB и работа с ними программными средствами._

При работе с C# и .NET в приложении Xamarin. Mac у вас есть доступ к тем же окнам и панелям, что и разработчик, работающий на *уровне цели-C* и *Xcode* . Так как Xamarin. Mac интегрируется напрямую с Xcode, можно использовать _Interface Builder_ Xcode для создания и обслуживания окон и панелей (или при необходимости создавать их непосредственно в C# коде).

В зависимости от цели приложение Xamarin. Mac может представлять один или несколько окон на экране для управления и координации отображаемой информации и работы с ней. Основные функции окна:

1. Для предоставления области, в которой можно размещать представления и элементы управления и управлять ими.
2. Для принятия и реагирования на события в ответ на взаимодействие пользователя с клавиатурой и мышью.

Windows можно использовать в немодальном состоянии (например, в текстовом редакторе, который может одновременно открывать несколько документов) или быть модальным (например, диалоговое окно экспорта, которое должно быть закрыто до того, как приложение может быть продолжено).

Панели — это особый вид окна (подкласс базового `NSWindow` класса), который обычно обслуживает вспомогательную функцию в приложении, например окна служебной программы, такие как инспекторы текстовых форматов и средство выбора системных цветов.

[![редактирования окна в Xcode](window-images/intro01.png)](window-images/intro01.png#lightbox)

В этой статье рассматриваются основы работы с окнами и панелями в приложении Xamarin. Mac. Мы настоятельно рекомендуем сначала ознакомиться со статьей [Hello, Mac](~/mac/get-started/hello-mac.md) , в частности [Знакомство с Xcode и Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) , а также с разделом "возможности [и действия](~/mac/get-started/hello-mac.md#outlets-and-actions) ", так как в нем рассматриваются основные понятия и методы, которые мы будем использовать в этой статье.

Возможно, вы захотите ознакомиться с [ C# представлением классов и методов для цели-C](~/mac/internals/how-it-works.md) в документе о внутренних компонентах [Xamarin. Mac](~/mac/internals/how-it-works.md) , а также поясняются `Register` и `Export` команды, используемые для подключения C# классов к объектам и элементам пользовательского интерфейса на языке c.

## <a name="introduction-to-windows"></a>Введение в Windows

Как упоминалось выше, окно предоставляет область, в которой можно размещать представления и элементы управления и управлять ими, а также реагировать на события на основе взаимодействия с пользователем (с помощью клавиатуры или мыши).

В соответствии с Apple существует пять основных типов окон в приложении macOS:

- **Окно документа** — окно документа содержит файловые данные пользователя, такие как электронная таблица или текстовый документ.
- **Окно приложения** — окно приложения — это главное окно приложения, которое не основано на документе (например, приложение календаря на компьютере Mac).
- **Панель** — панель перемещается над другими окнами и предоставляет средства или элементы управления, с которыми пользователи могут работать, пока документы открыты. В некоторых случаях панель может быть полупрозрачной (например, при работе с большими графическими изображениями).
- **Диалоговое** окно — Диалоговое окно появляется в ответ на действие пользователя и, как правило, позволяет пользователям выполнить действие. Перед закрытием диалогового окна необходимо получить от него ответ от пользователя. (См. раздел [Работа с диалоговыми окнами](~/mac/user-interface/dialog.md))
- **Оповещения** . Предупреждение — это особый тип диалогового окна, которое появляется при возникновении серьезной проблемы (например, ошибки) или в виде предупреждения (например, при подготовке к удалению файла). Поскольку оповещение является диалоговым окном, оно также требует ответа пользователя, прежде чем его можно будет закрыть. (См. раздел [Работа с предупреждениями](~/mac/user-interface/alert.md)).

Дополнительные сведения см. в разделе [About Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) статьи [macOS Design темы](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)"Apple".

### <a name="main-key-and-inactive-windows"></a>Основные, ключевые и неактивные окна

Windows в приложении Xamarin. Mac может выглядеть и вести себя по-разному в зависимости от того, как пользователь взаимодействует с ними. _Главное окно_— это окно документа или приложения, которое в данный момент уделяет внимание пользователю. В большинстве случаев это окно также является _ключевым окном_ (окном, в котором в настоящий момент принимается ввод пользователя). Но это не всегда так, например, может открыться палитра цветов, и это окно ключа, с которым взаимодействует пользователь, чтобы изменить состояние элемента в окне документа (которое по-прежнему является главным окном).

Главные и ключевые окна (если они являются отдельными) всегда активны, _неактивные окна_ — это открытые окна, которые не находятся на переднем плане. Например, в приложении текстового редактора может быть открыто несколько документов за раз, только главное окно было бы активным, все остальные будут неактивными. 

Дополнительные сведения см. в разделе [About Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) статьи [macOS Design темы](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)"Apple".

### <a name="naming-windows"></a>Именование окон

Окно может отображать строку заголовка и при отображении заголовка обычно это имя приложения, имя обрабатываемого документа или функция окна (например, инспектор). Некоторые приложения не отображают строку заголовка, так как они распознаются и не работают с документами.

Apple рекомендует следующие рекомендации:

- Используйте имя приложения для заголовка главного окна, не являющегося окном документа. 
- Назовите новое окно документа `untitled`. Для первого нового документа не добавляйте номер в заголовок (например, `untitled 1`). Если пользователь создает еще один новый документ перед сохранением и титульным пользователем, вызовите это окно `untitled 2`, `untitled 3`и т. д.

Дополнительные сведения см. в разделе " [именование Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowNaming.html#//apple_ref/doc/uid/20000957-CH35-SW1) " [темы "MacOS](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)" компании Apple.

### <a name="full-screen-windows"></a>Полноэкранные окна

В macOS окно приложения может перейти в полноэкранный режим, включая строку меню приложения (которая может быть отображена путем перемещения курсора в верхнюю часть экрана), чтобы обеспечить бесполезное взаимодействие с содержимым.

Компания Apple предлагает следующие рекомендации.

- Определите, имеет ли смысл окна полноэкранный режим. Приложения, которые предоставляют короткие взаимодействия (например, калькулятор), не должны обеспечивать полноэкранный режим.
- Отображать панель инструментов, если это требуется для полноэкранной задачи. Обычно панель инструментов скрыта в полноэкранном режиме.
- Полноэкранное окно должно иметь все функции, необходимые пользователям для выполнения задачи.
- По возможности избегайте взаимодействия с поиском, пока пользователь находится в полноэкранном режиме.
- Воспользуйтесь преимуществами увеличенного пространства на экране без перемещения фокуса с главной задачи.

Дополнительные сведения см. в разделе [Windows в полноэкранном](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/FullScreen.html#//apple_ref/doc/uid/20000957-CH61-SW1) [режиме статьи macOS Design Themes](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/).

### <a name="panels"></a>Панели

Панель — это вспомогательное окно, которое содержит элементы управления и параметры, влияющие на активный документ или выделенный фрагмент (например, средство выбора системного цвета):

[![палитры цветов](window-images/panel01.png)](window-images/panel01.png#lightbox)

Панели могут быть либо _специфичными_ для приложения _, либо_общедоступными. Панели, относящиеся к приложению, находятся в верхней части окон документов приложения и исчезают, когда приложение находится в фоновом режиме. Панели на уровне системы (например, панель « **шрифты** ») перемещаются поверх всех открытых окон независимо от приложения. 

Компания Apple предлагает следующие рекомендации.

- Как правило, используйте стандартную панель. прозрачные панели следует использовать только с осторожностью и для задач, требующих больших изображений.
- Используйте панель, чтобы предоставить пользователям простой доступ к важным элементам управления или сведениям, которые непосредственно влияют на их задачи.
- При необходимости скройте и отобразите панели.
- Панели всегда должны содержать заголовок окна.
- Панели не должны включать активную кнопку сворачивания.

#### <a name="inspectors"></a>Инспекторы

Большинство современных macOS приложений предоставляют вспомогательные элементы управления и параметры, влияющие на активный документ или выделенный фрагмент как _инспекторы_ , которые являются частью главного окна (например, приложение " **страницы** ", показанное ниже), вместо использования окон панелей.

[![пример инспектора](window-images/panel02.png)](window-images/panel02.png#lightbox)

Дополнительные сведения см. в разделе " [панели](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowPanels.html#//apple_ref/doc/uid/20000957-CH42-SW1) " темы " [macOS](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/) " и нашем примере приложения [МаЦинспектор](https://docs.microsoft.com/samples/xamarin/mac-samples/macinspector) для полной реализации **интерфейса инспектора** в приложении Xamarin. Mac.

## <a name="creating-and-maintaining-windows-in-xcode"></a>Создание и обслуживание Windows в Xcode

При создании нового приложения Xamarin. Mac Cocoa по умолчанию получается стандартное пустое окно. Эти окна определяются в `.storyboard` файле, который автоматически включается в проект. Чтобы изменить структуру Windows, в **Обозреватель решений**дважды щелкните файл `Main.storyboard`:

[![выбор основной раскадровки](window-images/edit01.png)](window-images/edit01.png#lightbox)

Это приведет к открытию структуры окна в Interface Builder Xcode:

[![редактирования пользовательского интерфейса в Xcode](window-images/edit02.png)](window-images/edit02.png#lightbox)

В **инспекторе атрибутов**имеется несколько свойств, которые можно использовать для определения и управления окном:

- **Title** — это текст, который будет отображаться в строке заголовка окна.
- **Автосохранение** — это _ключ_ , который будет использоваться для идентификации окна, когда его расположение и параметры будут автоматически сохранены.
- **Строка заголовка** — отображает заголовок окна.
- **Унифицированный заголовок и панель инструментов** . Если окно содержит панель инструментов, оно должно быть частью строки заголовка.
- **Полное представление содержимого** . позволяет области содержимого окна в строке заголовка.
- **Тень** . окно имеет тень.
- **Текстурированные** в текстурах окна могут использовать эффекты (например, вибрация), и их можно перемещать, перетаскивая в любом месте текста.
- **Закрыть** — окно содержит кнопку Закрыть.
- **Minimize** — в окне есть кнопка сворачивания.
- **Изменить размер** — окно имеет элемент управления изменением размера.
- **Кнопка панели инструментов** . окно содержит кнопку Скрыть или Показать панель инструментов.
- **Restorable** — расположение и параметры окна автоматически сохраняются и восстанавливаются.
- **Отображается при запуске** — это окно, которое автоматически отображается при загрузке файла `.xib`.
- **Скрыть при деактивации** — окно скрыто, когда приложение переходит в фоновый режим.
- **Выпуск после закрытия** — это окно, очищенное из памяти при его закрытии.
- **Всегда отображать подсказки** — всплывающие подсказки постоянно отображаются.
- **Пересчет цикла просмотра** — это порядок просмотра, пересчитанный перед прорисовкой окна.
- **Пробелы**, **експосé** и **циклически** определяют работу окна в этих macOS средах. 
- Во **весь экран** — определяет, может ли это окно Перейти в полноэкранный режим. 
- **Анимация** — управляет типом анимации, доступной для окна.
- **Внешний вид** — управляет внешним видом окна. Теперь существует только один вид — голубой.

Дополнительные сведения см. в документации Apple [Введение в Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) и [NSWindow](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSWindow_Class/index.html#//apple_ref/occ/cl/NSWindow) .

### <a name="setting-the-default-size-and-location"></a>Задание размера и расположения по умолчанию

Чтобы задать начальное расположение окна и управлять его размером, переключитесь на **Инспектор размеров**:

[![размер и расположение по умолчанию](window-images/edit07.png)](window-images/edit07.png#lightbox)

Здесь можно задать начальный размер окна, присвоить ему минимальный и максимальный размер, задать начальное расположение на экране и управлять границами окна.

### <a name="setting-a-custom-main-window-controller"></a>Настройка пользовательского основного контроллера окна

Чтобы иметь возможность создавать розетки и действия для предоставления элементов пользовательского интерфейса C# коду, приложению Xamarin. Mac потребуется пользовательский контроллер окна.

Выполните следующие действия:

1. Откройте раскадровку приложения в Interface Builder Xcode.
2. Выберите `NSWindowController` в область конструктора.
3. Перейдите в представление **инспектора удостоверений** и введите `WindowController` в качестве **имени класса**: 

    [![присвоение имени класса](window-images/windowcontroller01.png)](window-images/windowcontroller01.png#lightbox)
4. Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации.
5. `WindowController.cs` файл будет добавлен в проект в **Обозреватель решений** в Visual Studio для Mac: 

    [![выборе контроллера Windows](window-images/windowcontroller02.png)](window-images/windowcontroller02.png#lightbox)
6. Повторно откройте раскадровку в Interface Builder Xcode.
7. Файл `WindowController.h` будет доступен для использования: 

    [![редактирования файла Виндовконтроллер. h](window-images/windowcontroller03.png)](window-images/windowcontroller03.png#lightbox)

### <a name="adding-ui-elements"></a>Добавление элементов пользовательского интерфейса

Чтобы определить содержимое окна, перетащите элементы управления из **инспектора библиотек** в **Редактор интерфейса**. Дополнительные сведения об использовании Interface Builder для создания и включения элементов управления см. [в статье Введение в Xcode и Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) документацию.

В качестве примера Перетяните панель инструментов из **инспектора библиотек** в окно **редактора интерфейса**:

[![выбора панели инструментов из библиотеки](window-images/edit03.png)](window-images/edit03.png#lightbox)

Затем перетащите **текстовое представление** и измените его размер, чтобы заполнить область под панелью инструментов:

[![Добавление текстового представления](window-images/edit04.png)](window-images/edit04.png#lightbox)

Так как мы хотим, чтобы **текстовое представление** было сжато и увеличиваться по мере изменения размера окна, давайте перейдем к **редактору ограничений** и добавим следующие ограничения:

[![ограничения редактирования](window-images/edit05.png)](window-images/edit05.png#lightbox)

Щелкнув четыре Красного элемента **I-беамс** в верхней части редактора и нажав кнопку **Добавить 4 ограничения**, мы сообщаем, что текстовое представление будет прикрепляться к заданным координатам X, Y и увеличиваться или уменьшаться по горизонтали и по вертикали при изменении размера окна.

Наконец, предоставьте **текстовое представление** коду с помощью **розетки** (убедитесь, что выбран файл `ViewController.h`):

[![настройке розетки](window-images/edit06.png)](window-images/edit06.png#lightbox)

Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Дополнительные сведения о работе с **розетками** и **действиями**см. в документации по [розеткам и действиям](~/mac/get-started/hello-mac.md#outlets-and-actions) .

### <a name="standard-window-workflow"></a>Рабочий процесс стандартного окна

Для любого окна, созданного и работающего в приложении Xamarin. Mac, процесс по сути аналогичен тому, что мы только что сделали ранее:

1. Для новых окон, которые не добавляются в проект автоматически по умолчанию, добавьте в проект новое определение окна. Это будет подробно рассмотрено ниже.
1. Дважды щелкните файл `Main.storyboard`, чтобы открыть конструктор окон для редактирования в Interface Builder Xcode.
1. Перетащите новое окно в дизайн пользовательского интерфейса и присоедините его к главному окну с помощью _переходов_ (Дополнительные сведения см. в разделе [переходов](~/mac/platform/storyboards/indepth.md#Segues) в документации по [работе с раскадровками](~/mac/platform/storyboards/indepth.md) ).
1. Задайте необходимые свойства окна в **инспекторе атрибутов** и **Инспекторе размеров**.
1. Перетащите элементы управления, необходимые для создания интерфейса, и настройте их в **инспекторе атрибутов**.
1. Используйте **Инспектор размеров** для изменения размера элементов пользовательского интерфейса.
1. Предоставление элементов пользовательского интерфейса окна коду с C# помощью **розеток** и **действий**.
1. Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Теперь, когда создано базовое окно, мы рассмотрим типичные процессы, выполняемые приложением Xamarin. Mac при работе с Windows. 

## <a name="displaying-the-default-window"></a>Отображение окна по умолчанию

По умолчанию новое приложение Xamarin. Mac автоматически отображает окно, определенное в `MainWindow.xib` файле при его запуске:

[![выполняющегося примера окна](window-images/display01.png)](window-images/display01.png#lightbox)

Так как мы изменили структуру этого окна выше, теперь она содержит панель инструментов по умолчанию и элемент управления " **представление текста** ". Следующий раздел в файле `Info.plist` отвечает за отображение этого окна:

[![редактирования info. plist](window-images/display00.png)](window-images/display00.png#lightbox)

Раскрывающийся список **главного интерфейса** используется для выбора раскадровки, которая будет использоваться в качестве основного пользовательского интерфейса приложения (в данном случае `Main.storyboard`).

Контроллер представления автоматически добавляется в проект для управления отображаемыми основными окнами (вместе с основным представлением). Он определен в файле `ViewController.cs` и прикрепляется к **владельцу файла** в Interface Builder в **инспекторе удостоверений**:

[![задание владельца файла](window-images/display02.png)](window-images/display02.png#lightbox)

Для нашего окна нам хотелось бы, чтобы он имел заголовок `untitled` при первом открытии, поэтому Переопределите метод `ViewWillAppear` в `ViewController.cs`, чтобы он выглядел следующим образом:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";
}
```    

> [!NOTE]
> Свойство `Title` окна задается в методе `ViewWillAppear`, а не в методе `ViewDidLoad`, так как, хотя представление может быть загружено в память, оно еще не полностью создается. При доступе к свойству `Title` в методе `ViewDidLoad` будет получено исключение `null`, так как окно еще не создано и не привязано к свойству.

## <a name="programmatically-closing-a-window"></a>Программное закрытие окна

Иногда требуется программно закрыть окно в приложении Xamarin. Mac, не выполняя пользовательское нажатие кнопки **Закрыть** или с помощью пункта меню. macOS предоставляет два разных способа закрытия `NSWindow` программным способом: `PerformClose` и `Close`.

### <a name="performclose"></a>перформклосе

Вызов метода `PerformClose` `NSWindow` имитирует пользователя, нажимая кнопку **закрытия** окна, выделяя кнопку и закрывая окно.

Если приложение реализует событие `WillClose` `NSWindow`, оно будет создано до закрытия окна. Если событие возвращает `false`, окно не будет закрыто. Если окно не содержит кнопку **Закрыть** или не может быть закрыто по какой-либо причине, операционная система выдаст предупреждение.

Например:

```csharp
MyWindow.PerformClose(this);
```

Попытается закрыть экземпляр `NSWindow` `MyWindow`. Если оно прошло успешно, окно будет закрыто, в противном случае будет выдаваться звуковое оповещение и останется открытым.

### <a name="close"></a>закрыть

Вызов метода `Close` `NSWindow` не имитирует нажатие кнопки **закрытия** окна, выделяя кнопку, и просто закрывает окно.

Окно не обязательно должно быть закрыто `NSWindowWillCloseNotification`, и в центр уведомлений по умолчанию будет отправлено уведомление для закрытия окна.

Метод `Close` отличается двумя важными способами из метода `PerformClose`:

1. Он не пытается вызвать событие `WillClose`.
2. Он не имитирует нажатие кнопки « **Закрыть** » пользователем, выделяя кнопку.

Например:

```csharp
MyWindow.Close();
```

Закрывает `MyWindow` экземпляр `NSWindow`.

## <a name="modified-windows-content"></a>Измененное содержимое Windows

В macOS компания Apple предоставила способ информирования пользователя о том, что содержимое окна (`NSWindow`) было изменено пользователем и необходимо сохранить. Если окно содержит измененное содержимое, **в мини-** приложении будет отображаться небольшая черная точка:

[![окна с измененным маркером](window-images/close01.png)](window-images/close01.png#lightbox)

Если пользователь попытается закрыть окно или выйти из приложения Mac при наличии несохраненных изменений в содержимом окна, необходимо предоставить [диалоговое окно](~/mac/user-interface/dialog.md) или [модальный лист](~/mac/user-interface/dialog.md) и разрешить пользователю сохранять свои изменения в первую очередь:

[![лист сохранения, отображаемый при закрытии окна](window-images/close02.png)](window-images/close02.png#lightbox)

### <a name="marking-a-window-as-modified"></a>Пометка окна как измененного

Чтобы пометить окно как имеющее измененное содержимое, используйте следующий код:

```csharp
// Mark Window content as modified
Window.DocumentEdited = true;
```

После сохранения изменений очистите измененный флаг с помощью:

```csharp
// Mark Window content as not modified
Window.DocumentEdited = false;
```

### <a name="saving-changes-before-closing-a-window"></a>Сохранение изменений перед закрытием окна

Чтобы отслеживать закрытие окна пользователем и позволить ему сохранить измененное содержимое заранее, необходимо создать подкласс `NSWindowDelegate` и переопределить его метод `WindowShouldClose`. Например:

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class EditorWindowDelegate : NSWindowDelegate
    {
        #region Computed Properties
        public NSWindow Window { get; set;}
        #endregion

        #region constructors
        public EditorWindowDelegate (NSWindow window)
        {
            // Initialize
            this.Window = window;

        }
        #endregion

        #region Override Methods
        public override bool WindowShouldClose (Foundation.NSObject sender)
        {
            // is the window dirty?
            if (Window.DocumentEdited) {
                var alert = new NSAlert () {
                    AlertStyle = NSAlertStyle.Critical,
                    InformativeText = "Save changes to document before closing window?",
                    MessageText = "Save Document",
                };
                alert.AddButton ("Save");
                alert.AddButton ("Lose Changes");
                alert.AddButton ("Cancel");
                var result = alert.RunSheetModal (Window);

                // Take action based on result
                switch (result) {
                case 1000:
                    // Grab controller
                    var viewController = Window.ContentViewController as ViewController;

                    // Already saved?
                    if (Window.RepresentedUrl != null) {
                        var path = Window.RepresentedUrl.Path;

                        // Save changes to file
                        File.WriteAllText (path, viewController.Text);
                        return true;
                    } else {
                        var dlg = new NSSavePanel ();
                        dlg.Title = "Save Document";
                        dlg.BeginSheet (Window, (rslt) => {
                            // File selected?
                            if (rslt == 1) {
                                var path = dlg.Url.Path;
                                File.WriteAllText (path, viewController.Text);
                                Window.DocumentEdited = false;
                                viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
                                viewController.View.Window.RepresentedUrl = dlg.Url;
                                Window.Close();
                            }
                        });
                        return true;
                    }
                    return false;
                case 1001:
                    // Lose Changes
                    return true;
                case 1002:
                    // Cancel
                    return false;
                }
            }

            return true;
        }
        #endregion
    }
}
```

Используйте следующий код, чтобы присоединить экземпляр этого делегата к окну:

```csharp
// Set delegate
Window.Delegate = new EditorWindowDelegate(Window);
```

### <a name="saving-changes-before-closing-the-app"></a>Сохранение изменений перед закрытием приложения

Наконец, приложение Xamarin. Mac должно проверить, содержит ли какое-либо из окон измененное содержимое, и разрешить пользователю сохранять изменения перед выходом. Для этого измените файл `AppDelegate.cs`, переопределите метод `ApplicationShouldTerminate` и сделайте его следующим:

```csharp
public override NSApplicationTerminateReply ApplicationShouldTerminate (NSApplication sender)
{
    // See if any window needs to be saved first
    foreach (NSWindow window in NSApplication.SharedApplication.Windows) {
        if (window.Delegate != null && !window.Delegate.WindowShouldClose (this)) {
            // Did the window terminate the close?
            return NSApplicationTerminateReply.Cancel;
        }
    }

    // Allow normal termination
    return NSApplicationTerminateReply.Now;
}
```

## <a name="working-with-multiple-windows"></a>Работа с несколькими окнами

Большинство приложений Mac на основе документов могут одновременно редактировать несколько документов. Например, текстовый редактор может одновременно открыть несколько текстовых файлов для редактирования. По умолчанию новое приложение Xamarin. Mac содержит меню **файл** с **новым** элементом, автоматически подпадающее к **действию**`newDocument:`.

Приведенный ниже код активирует этот новый элемент и позволяет пользователю открывать несколько копий главного окна для одновременного редактирования нескольких документов.

Измените файл `AppDelegate.cs` и добавьте следующее вычисленное свойство:

```csharp
public int UntitledWindowCount { get; set;} =1;
```

Используйте этот параметр, чтобы отсортировать количество несохраненных файлов, чтобы мы могли отправить отзыв пользователю (согласно правилам Apple, описанным выше).

Затем добавьте следующий метод:

```csharp
[Export ("newDocument:")]
void NewDocument (NSObject sender) {
    // Get new window
    var storyboard = NSStoryboard.FromName ("Main", null);
    var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

    // Display
    controller.ShowWindow(this);

    // Set the title
    controller.Window.Title = (++UntitledWindowCount == 1) ? "untitled" : string.Format ("untitled {0}", UntitledWindowCount);
}
```

Этот код создает новую версию нашего контроллера окна, загружает новое окно, делает его основным и задает его заголовок. Теперь, если мы запустим приложение и в меню **файл** выберите пункт **создать** , откроется и отобразится новое окно редактора:

[![было добавлено новое окно без заголовка](window-images/display04.png)](window-images/display04.png#lightbox)

Если открыть меню **Windows** , можно увидеть, что приложение автоматически отслеживает и обрабатывает открытые окна:

[![меню Windows](window-images/display05.png)](window-images/display05.png#lightbox)

Дополнительные сведения о работе с меню в приложении Xamarin. Mac см. в документации по [работе с меню](~/mac/user-interface/menu.md) .

### <a name="getting-the-currently-active-window"></a>Получение текущего активного окна

В приложении Xamarin. Mac, которое может открывать несколько окон (документов), бывают случаи, когда необходимо получить текущее, самое верхнее окно (ключевое окно). Следующий код вернет окно ключа:

```csharp
var window = NSApplication.SharedApplication.KeyWindow;
```

Его можно вызвать в любом классе или методе, которому требуется доступ к текущему окну ключа. Если окно в данный момент не открыто, оно возвратит `null`.

### <a name="accessing-all-app-windows"></a>Доступ ко всем окнам приложений

Иногда требуется доступ ко всем окнам, открытым в настоящее время приложением Xamarin. Mac. Например, если файл, который пользователь хочет открыть, уже открыт в окне выхода.

`NSApplication.SharedApplication` поддерживает свойство `Windows`, которое содержит массив всех открытых окон в приложении. Вы можете выполнить итерацию по этому массиву, чтобы получить доступ ко всем текущим окнам приложения. Например:

```csharp
// Is the file already open?
for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
    var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
    if (content != null && path == content.FilePath) {
        // Bring window to front
        NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
        return true;
    }
}
```

В примере кода мы приведем Каждое возвращенное окно к пользовательскому классу `ViewController` в нашем приложении и тестируем значение пользовательского свойства `Path` по пути к файлу, который пользователь хочет открыть. Если файл уже открыт, это окно переносится на передний план.

## <a name="adjusting-the-window-size-in-code"></a>Настройка размера окна в коде

Иногда приложению необходимо изменить размер окна в коде. Чтобы изменить размер и расположение окна, измените его свойство `Frame`. При изменении размера окна обычно необходимо изменить его происхождение, чтобы окно оставалось в том же расположении из-за системы координат macOS.

В отличие от iOS, где верхний левый угол представляет (0, 0), macOS использует математические систему координат, где представляется в левом нижнем углу экрана (0, 0). В iOS координаты увеличиваются по мере перемещения в направлении вправо. В macOS координаты увеличиваются в направлении вправо. 

В следующем примере кода изменяется размер окна:

```csharp
nfloat y = 0;

// Calculate new origin
y = Frame.Y - (768 - Frame.Height);

// Resize and position window
CGRect frame = new CGRect (Frame.X, y, 1024, 768);
SetFrame (frame, true);
```

> [!IMPORTANT]
> При настройке размера и расположения Windows в коде необходимо обеспечить соблюдение минимального и максимального размеров, заданных в Interface Builder. Это не будет учитываться автоматически, и вы сможете сделать окно больше или меньше, чем эти ограничения.

## <a name="monitoring-window-size-changes"></a>Изменения размера окна мониторинга

В некоторых случаях необходимо отслеживать изменения размера окна в приложении Xamarin. Mac. Например, для перерисовки содержимого в соответствии с новым размером.

Чтобы отслеживать изменения размера, сначала убедитесь, что вы назначили пользовательский класс для контроллера окна в Interface Builder Xcode. Например, `MasterWindowController` в следующем:

[![](window-images/resize01.png "The Identity Inspector")](window-images/resize01.png#lightbox)

Затем измените класс пользовательского контроллера окна и отслеживайте событие `DidResize` в окне контроллера, чтобы получать уведомления об изменениях в динамическом размере. Например:

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

    Window.DidResize += (sender, e) => {
        // Do something as the window is being live resized
    };
}
```

При необходимости можно использовать событие `DidEndLiveResize`, чтобы получать уведомления только после того, как пользователь завершит изменение размера окна. Например:

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

        Window.DidEndLiveResize += (sender, e) => {
        // Do something after the user's finished resizing
        // the window
    };
}
```

## <a name="setting-a-windows-title-and-represented-file"></a>Задание заголовка окна и представляемого файла

При работе с окнами, представляющими документы, `NSWindow` имеет свойство `DocumentEdited`, которое в `true` отображает маленькую точку в кнопке "Закрыть", чтобы предоставить пользователю указание о том, что файл был изменен и должен быть сохранен перед закрытием.

Измените файл `ViewController.cs` и внесите следующие изменения:

```csharp
public bool DocumentEdited {
    get { return View.Window.DocumentEdited; }
    set { View.Window.DocumentEdited = value; }
}
...

public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";

    View.Window.WillClose += (sender, e) => {
        // is the window dirty?
        if (DocumentEdited) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to give the user the ability to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        }
    };
}

public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Show when the document is edited
    DocumentEditor.TextDidChange += (sender, e) => {
        // Mark the document as dirty
        DocumentEdited = true;
    };

    // Overriding this delegate is required to monitor the TextDidChange event
    DocumentEditor.ShouldChangeTextInRanges += (NSTextView view, NSValue[] values, string[] replacements) => {
        return true;
    };

}
```

Мы также отслеживаем событие `WillClose` в окне и проверяя состояние свойства `DocumentEdited`. Если это `true` необходимо предоставить пользователю возможность сохранять изменения в файле. Если мы запустим приложение и введете какой бы то ни было текст, появится точка.

[![](window-images/file01.png "A changed window")](window-images/file01.png#lightbox)

При попытке закрыть окно появляется предупреждение:

[![](window-images/file02.png "Displaying a save dialog")](window-images/file02.png#lightbox)

При загрузке документа из файла задайте для заголовка окна имя файла с помощью метода `window.SetTitleWithRepresentedFilename (Path.GetFileName(path));` (учитывая, что `path` является строкой, представляющей открываемый файл). Кроме того, можно задать URL-адрес файла с помощью метода `window.RepresentedUrl = url;`.

Если URL-адрес указывает на тип файла, известного операционной системой, его значок будет отображаться в заголовке окна. Если пользователь щелкает правой кнопкой мыши значок, будет показан путь к файлу.

Измените файл `AppDelegate.cs` и добавьте следующий метод:

```csharp
[Export ("openDocument:")]
void OpenDialog (NSObject sender)
{
    var dlg = NSOpenPanel.OpenPanel;
    dlg.CanChooseFiles = true;
    dlg.CanChooseDirectories = false;

    if (dlg.RunModal () == 1) {
        // Nab the first file
        var url = dlg.Urls [0];

        if (url != null) {
            var path = url.Path;

            // Get new window
            var storyboard = NSStoryboard.FromName ("Main", null);
            var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

            // Display
            controller.ShowWindow(this);

            // Load the text into the window
            var viewController = controller.Window.ContentViewController as ViewController;
            viewController.Text = File.ReadAllText(path);
                    viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
            viewController.View.Window.RepresentedUrl = url;

        }
    }
}
```

Теперь при запуске нашего приложения выберите **Открыть...** в меню **файл** выберите текстовый файл из диалогового окна **Открыть** и откройте его:

[![открытого диалогового окна](window-images/file03.png)](window-images/file03.png#lightbox)

Файл будет отображен, а заголовок будет установлен с помощью значка файла:

[![загруженного содержимого файла](window-images/file04.png)](window-images/file04.png#lightbox)

## <a name="adding-a-new-window-to-a-project"></a>Добавление нового окна в проект

Помимо главного окна документа, приложению Xamarin. Mac может потребоваться отобразить другие типы окон, например настройки или панели инспектора.

Чтобы добавить новое окно, выполните следующие действия.

1. В **Обозреватель решений**дважды щелкните файл `Main.storyboard`, чтобы открыть его для редактирования в Interface Builder Xcode.
2. Перетащите новый **контроллер окна** из **библиотеки** и поместите его на **область конструктора**:

    [![выборе нового контроллера окна в библиотеке](window-images/new01.png)](window-images/new01.png#lightbox)
3. В **инспекторе удостоверений**введите `PreferencesWindow` для **идентификатора раскадровки**: 

    [![установки идентификатора раскадровки](window-images/new02.png)](window-images/new02.png#lightbox)
4. Проектирование интерфейса: 

    [![разработки пользовательского интерфейса](window-images/new03.png)](window-images/new03.png#lightbox)
5. Откройте меню приложения (`MacWindows`), выберите **настройки...** , щелкните элемент управления и перетащите его в новое окно: 

    [![создания перехода](window-images/new05.png)](window-images/new05.png#lightbox)
6. В контекстном меню выберите пункт **Показывать** .
7. Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Если выполнить код и выбрать **Параметры...** в **меню приложение**, откроется окно:

[![пример меню настроек](window-images/new04.png)](window-images/new04.png#lightbox)

## <a name="working-with-panels"></a>Работа с панелями

Как упоминалось в начале этой статьи, панель перемещается над другими окнами и предоставляет средства или элементы управления, с которыми пользователи могут работать, пока документы открыты. 

Как и в случае с любыми другими типами окон, которые вы создаете и работаете в приложении Xamarin. Mac, процесс по сути такой же:

1. Добавление нового определения окна в проект.
2. Дважды щелкните файл `.xib`, чтобы открыть конструктор окон для редактирования в Interface Builder Xcode.
3. Задайте необходимые свойства окна в **инспекторе атрибутов** и **Инспекторе размеров**.
4. Перетащите элементы управления, необходимые для создания интерфейса, и настройте их в **инспекторе атрибутов**.
5. Используйте **Инспектор размеров** для изменения размера элементов пользовательского интерфейса.
6. Предоставление элементов пользовательского интерфейса окна коду с C# помощью **розеток** и **действий**.
7. Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

В **инспекторе атрибутов**имеются следующие параметры, характерные для панелей:

[![инспектора атрибутов](window-images/panel03.png)](window-images/panel03.png#lightbox)

- **Стиль** . позволяет настраивать стиль панели: обычная панель (похожая на стандартное окно), панель служебной программы (содержит меньшее название), панель HUD (полупрозрачная, а строка заголовка — часть фона).
- **Без активации** — определяет в панели окно ключа.
- **Модальный документ** — если документ является модальным, панель перемещается только над окнами приложения, в противном случае — над всеми.

Чтобы добавить новую панель, выполните следующие действия.

1. В **Обозреватель решений**щелкните правой кнопкой мыши проект и выберите **Добавить** > **новый файл...** .
2. В диалоговом окне новый файл выберите **Xamarin. Mac** > **окно Cocoa с контроллером**:

    [![Добавление нового контроллера окна](window-images/panels00.png)](window-images/panels00.png#lightbox)

3. Введите в поле **Имя** значение `DocumentPanel` и нажмите кнопку **Новый**.
4. Дважды щелкните файл `DocumentPanel.xib`, чтобы открыть его для редактирования в Interface Builder: 

    [![редактирования панели](window-images/new02.png)](window-images/new02.png#lightbox)

5. Удалите существующее окно и перетащите панель из **инспектора библиотеки** в **редакторе интерфейса**: 

    [![удаления существующего окна](window-images/panels01.png)](window-images/panels01.png#lightbox)

6. Присоедините панель к **окну** - **владельца файла** - **розетке**: 

    [![перетягивание панели](window-images/panels02.png)](window-images/panels02.png#lightbox)

7. Перейдите в **инспектор удостоверений** и задайте для класса панели `DocumentPanel`: 

    [![установки класса Panel](window-images/panels03.png)](window-images/panels03.png#lightbox)

8. Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.
9. Измените файл `DocumentPanel.cs` и измените определение класса следующим образом: 

    `public partial class DocumentPanel : NSPanel`

10. Сохраните изменения в файле.

Измените файл `AppDelegate.cs` и сделайте метод `DidFinishLaunching` выглядеть следующим образом:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
        // Display panel
    var panel = new DocumentPanelController ();
    panel.Window.MakeKeyAndOrderFront (this);
}

```

Если мы запустим приложение, отобразится панель:

[![панели в работающем приложении](window-images/panels04.png)](window-images/panels04.png#lightbox)

> [!IMPORTANT]
> Окна панели не рекомендуются компанией Apple и должны быть заменены **интерфейсами инспектора**. Полный пример создания **инспектора** в приложении Xamarin. Mac см. в нашем примере приложения [маЦинспектор](https://docs.microsoft.com/samples/xamarin/mac-samples/macinspector) .

## <a name="summary"></a>Сводка

В этой статье подробно рассматривается работа с окнами и панелями в приложении Xamarin. Mac. Мы видели различные типы и использование окон и панелей, как создавать и обслуживать окна и панели в Interface Builder Xcode и как работать с окнами и панелями в C# коде.

## <a name="related-links"></a>Связанные ссылки

- [Маквиндовс (пример)](https://docs.microsoft.com/samples/xamarin/mac-samples/macwindows)
- [МаЦинспектор (пример)](https://docs.microsoft.com/samples/xamarin/mac-samples/macinspector)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Работа с меню](~/mac/user-interface/menu.md)
- [темы проектирования macOS (Apple)](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)
- [Окна, панели и экраны (Apple)](https://developer.apple.com/documentation/appkit/windows_panels_and_screens)
