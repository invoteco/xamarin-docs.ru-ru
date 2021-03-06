---
title: Оповещения в Xamarin. Mac
description: В этой статье рассматривается работа с предупреждениями в приложении Xamarin. Mac. Здесь описывается создание и отображение предупреждений C# из кода и реагирование на взаимодействия пользователей.
ms.prod: xamarin
ms.assetid: F1DB93A1-7549-4540-AD5E-D7605CCD8435
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 2ef8e81390b5d64971aa3883a05da9fb8795a416
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73001529"
---
# <a name="alerts-in-xamarinmac"></a>Оповещения в Xamarin. Mac

_В этой статье рассматривается работа с предупреждениями в приложении Xamarin. Mac. Здесь описывается создание и отображение предупреждений C# из кода и реагирование на взаимодействия пользователей._

При работе с C# и .NET в приложении Xamarin. Mac у вас есть доступ к тем же предупреждениям, что и разработчик, работающий на *уровне цели-C* и *Xcode* . 

Предупреждение — это особый тип диалогового окна, которое появляется при возникновении серьезной проблемы (например, ошибка) или в виде предупреждения (например, при подготовке к удалению файла). Поскольку оповещение является диалоговым окном, оно также требует ответа пользователя, прежде чем его можно будет закрыть.

[![](alert-images/alert06.png "An example alert")](alert-images/alert06.png#lightbox)

В этой статье рассматриваются основные принципы работы с предупреждениями в приложении Xamarin. Mac. 

<a name="Introduction_to_Alerts" />

## <a name="introduction-to-alerts"></a>Общие сведения о предупреждениях

Предупреждение — это особый тип диалогового окна, которое появляется при возникновении серьезной проблемы (например, ошибка) или в виде предупреждения (например, при подготовке к удалению файла). Поскольку предупреждения нарушают пользователя, так как они должны быть отклонены, прежде чем пользователь сможет продолжить работу с задачей, не следует отображать оповещение, если оно совершенно не требуется.

Apple рекомендует следующие рекомендации:

- Не используйте оповещение только для предоставления сведений о пользователях.
- Не отображать оповещение для распространенных, отменяемых действий. Даже если такая ситуация может привести к потере данных.
- Если ситуация достойной предупреждения, старайтесь не использовать какой-либо другой элемент пользовательского интерфейса или метод для его вывода.
- Значок предупреждения следует использовать осторожно.
- Опишите ситуацию с предупреждениями четко и кратко в сообщении с предупреждением.
- Имя кнопки по умолчанию должно соответствовать действию, описанному в сообщении с предупреждением.

Дополнительные сведения см. в разделе " [оповещения](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAlerts.html#//apple_ref/doc/uid/20000957-CH44-SW1) [" руководства по использованию интерфейса](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) пользователя в ОС Apple.

<a name="Anatomy_of_an_Alert" />

## <a name="anatomy-of-an-alert"></a>Анатомия оповещения

Как упоминалось выше, оповещения должны отображаться для пользователя приложения при возникновении серьезной проблемы или в виде предупреждения о возможной потере данных (например, при закрытии несохраненного файла). В Xamarin. Mac в C# коде создается предупреждение, например:

```csharp
var alert = new NSAlert () {
  AlertStyle = NSAlertStyle.Critical,
  InformativeText = "We need to save the document here...",
  MessageText = "Save Document",
};
alert.RunModal ();
```

Приведенный выше код отображает предупреждение с значком приложения, нажимаемым на значке предупреждения, заголовок, предупреждающее сообщение и одну кнопку **ОК** :

[![](alert-images/alert01.png "An alert with a OK button")](alert-images/alert01.png#lightbox)

Apple предоставляет несколько свойств, которые можно использовать для настройки оповещений.

- **Алертстиле** определяет тип предупреждения как одно из следующих:
  - **Предупреждение** — используется для предупреждения пользователя о текущем или неожидающем событии, которое не является критическим. Это стиль по умолчанию.
  - **Информация** — используется для предупреждения пользователя о текущем или ожидающем событии. В настоящее время нет видимых различий между **предупреждением** и **информационным**
  - **Критический** — используется для предупреждения пользователя о серьезных последствиях незавершенного события (например, при удалении файла). Этот тип предупреждения следует использовать с осторожностью.
- **MessageText** — это основное сообщение или заголовок оповещения, которое должно быстро определить ситуацию для пользователя.
- **Информативетекст** — это текст оповещения, в котором следует четко определить ситуацию и предоставить пользователю возможные варианты.
- **Значок** — позволяет пользователю отображать пользовательский значок.
- **Хелпанчор** & **шовшелп** — позволяет связать оповещение с приложением хелпбук и отобразить справку по этому оповещению.
- **Кнопки** . по умолчанию оповещение содержит только кнопку **ОК** , но коллекция **Buttons** позволяет добавлять дополнительные варианты по мере необходимости.
- **Шовссуппрессионбуттон** — если `true` отображается флажок, который пользователь может использовать для подавления предупреждения при последующих событиях события, которое его инициировало.
- **Акцессоривиев** — позволяет присоединить к предупреждению другое Подпредставление, чтобы предоставить дополнительные сведения, например добавить **текстовое поле** для ввода данных. Если вы устанавливаете новый **акцессоривиев** или изменяете существующий, необходимо вызвать метод `Layout()`, чтобы настроить видимый макет предупреждения.

<a name="Displaying_an_Alert" />

## <a name="displaying-an-alert"></a>Отображение предупреждения

Существует два разных способа отображения предупреждения: свободно-плавающая или как лист. Приведенный ниже код отображает предупреждение как свободный с плавающей запятой.

```csharp
var alert = new NSAlert () {
  AlertStyle = NSAlertStyle.Informational,
  InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
  MessageText = "Alert Title",
};
alert.RunModal ();
```

При выполнении этого кода отображается следующее:

[![](alert-images/alert02.png "A simple alert")](alert-images/alert02.png#lightbox)

Следующий код отображает то же самое предупреждение, что и лист:

```csharp
var alert = new NSAlert () {
  AlertStyle = NSAlertStyle.Informational,
  InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
  MessageText = "Alert Title",
};
alert.BeginSheet (this);
```

При выполнении этого кода будут отображены следующие:

[![](alert-images/alert03.png "An alert displayed as a sheet")](alert-images/alert03.png#lightbox)

<a name="Working_with_Alert_Buttons" />

## <a name="working-with-alert-buttons"></a>Работа с кнопками предупреждений

По умолчанию в предупреждении отображается только кнопка **ОК** . Тем не менее, вы можете создать дополнительные кнопки, добавив их в коллекцию **Buttons** . В следующем коде создается оповещение с произвольным плавающей точкой с помощью кнопки **ОК**, **Отмена** и, **возможно** ,:

```csharp
var alert = new NSAlert () {
  AlertStyle = NSAlertStyle.Informational,
  InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
  MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
var result = alert.RunModal ();
```

Первая добавленная кнопка будет _кнопкой по умолчанию_ , которая будет активирована, если пользователь нажмет клавишу ВВОД. Возвращаемое значение будет целым числом, представляющим нажатую пользователем кнопку. В нашем случае будут возвращены следующие значения:

- **ОК** — 1000.
- **Отмена** — 1001.
- **Возможно** , 1002.

Если выполнить код, будут отображены следующие фрагменты кода:

[![](alert-images/alert04.png "An alert with three button options")](alert-images/alert04.png#lightbox)

Ниже приведен код для того же оповещения, что и лист:

```csharp
var alert = new NSAlert () {
  AlertStyle = NSAlertStyle.Informational,
  InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
  MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.BeginSheetForResponse (this, (result) => {
  Console.WriteLine ("Alert Result: {0}", result);
});
```

При выполнении этого кода будут отображены следующие:

[![](alert-images/alert05.png "A three button alert displayed as a sheet")](alert-images/alert05.png#lightbox)

> [!IMPORTANT]
> В оповещение никогда не следует добавлять более трех кнопок.

<a name="Showing_the_Suppress_Button" />

## <a name="showing-the-suppress-button"></a>Отображение кнопки "подавлять"

Если свойство `ShowSuppressButton` предупреждения имеет значение `true`, то в предупреждении отображается флажок, который пользователь может использовать для подавления предупреждения о последующих событиях события, которое его инициировало. В следующем коде отображается оповещение с произвольным плавающей точкой с кнопкой "подавлять":

```csharp
var alert = new NSAlert () {
  AlertStyle = NSAlertStyle.Informational,
  InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
  MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
var result = alert.RunModal ();
Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
```

Если значение `alert.SuppressionButton.State` равно `NSCellStateValue.On`, пользователь установил флажок подавлять, а в противном случае — нет.

Если код выполняется, будет выведено следующее:

[![](alert-images/alert06.png "An alert with a suppress button")](alert-images/alert06.png#lightbox)

Ниже приведен код для того же оповещения, что и лист:

```csharp
var alert = new NSAlert () {
  AlertStyle = NSAlertStyle.Informational,
  InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
  MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.BeginSheetForResponse (this, (result) => {
  Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
});
```

При выполнении этого кода будут отображены следующие:

[![](alert-images/alert07.png "An alert with a suppress button display as a sheet")](alert-images/alert07.png#lightbox)

<a name="Adding_a_Custom_SubView" />

## <a name="adding-a-custom-subview"></a>Добавление пользовательского подпредставления

Оповещения имеют свойство `AccessoryView`, которое можно использовать для дальнейшей настройки оповещения и добавления таких элементов, как **текстовое поле** для ввода данных пользователем. Следующий код создает оповещение с произвольным плавающей точкой с добавленным текстовым полем ввода:

```csharp
var input = new NSTextField (new CGRect (0, 0, 300, 20));

var alert = new NSAlert () {
AlertStyle = NSAlertStyle.Informational,
InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
  MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.AccessoryView = input;
alert.Layout ();
var result = alert.RunModal ();
Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
```

Ключевые строки здесь являются `var input = new NSTextField (new CGRect (0, 0, 300, 20));`, который создает новое **текстовое поле** , в которое мы добавим предупреждение. `alert.AccessoryView = input;`, который присоединяет **текстовое поле** к предупреждению и вызов метода `Layout()`, который требуется для изменения размера предупреждения в соответствии с новым подпредставлением.

Если выполнить код, будут отображены следующие фрагменты кода:

[![](alert-images/alert08.png "If we run the code, the following will be displayed")](alert-images/alert08.png#lightbox)

Это то же самое предупреждение, что и лист:

```csharp
var input = new NSTextField (new CGRect (0, 0, 300, 20));

var alert = new NSAlert () {
  AlertStyle = NSAlertStyle.Informational,
  InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
  MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.AccessoryView = input;
alert.Layout ();
alert.BeginSheetForResponse (this, (result) => {
  Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
});
```

Если выполнить этот код, отобразятся следующие фрагменты кода:

[![](alert-images/alert09.png "An alert with a custom view")](alert-images/alert09.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье подробно рассматривается работа с оповещениями в приложении Xamarin. Mac. Мы узнали о различных типах и использовании оповещений, о том, как создавать и настраивать оповещения, а также как работать C# с предупреждениями в коде.

## <a name="related-links"></a>Связанные ссылки

- [Маквиндовс (пример)](https://docs.microsoft.com/samples/xamarin/mac-samples/macwindows)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Работа с Windows](~/mac/user-interface/window.md)
- [Рекомендации по работе с человеческим интерфейсом OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Введение в Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [нсалерт](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSAlert_Class/index.html#//apple_ref/doc/uid/TP40004001)
