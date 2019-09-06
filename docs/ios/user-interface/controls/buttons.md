---
title: Кнопки в Xamarin. iOS
description: Класс Уибуттон используется для представления различных стилей кнопок на экранах iOS. В этом руководством представлены различные варианты работы с кнопками в iOS.
ms.prod: xamarin
ms.assetid: 304229E5-8FA8-41BD-8563-D19E1D2A0296
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/11/2018
ms.openlocfilehash: bbb423b01a477b0589903f96d0f4313c25733b91
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70284341"
---
# <a name="buttons-in-xamarinios"></a>Кнопки в Xamarin. iOS

В iOS `UIButton` класс представляет элемент управления Button.

Свойства кнопки можно изменить программно или с помощью **панель свойств** конструктора iOS:

![Панель свойств конструктора iOS](buttons-images/properties.png "Панель свойств конструктора iOS")

## <a name="creating-a-button-programmatically"></a>Создание кнопки программным способом

`UIButton` Можно создать только с помощью нескольких строк кода.

- Создайте экземпляр кнопки и укажите ее тип:

  ```csharp
  UIButton myButton = new UIButton(UIButtonType.System);
  ```

  Тип кнопки задается `UIButtonType`следующим образом:

  - `UIButtonType.System`— Кнопка общего назначения
  - `UIButtonType.DetailDisclosure`— Указывает доступность подробных сведений, обычно о конкретном элементе в таблице.
  - `UIButtonType.InfoDark`— Указывает доступность сведений о конфигурации; темно-цветной
  - `UIButtonType.InfoLight`— Указывает доступность сведений о конфигурации; светло-цветной
  - `UIButtonType..AddContact`— Указывает, что контакт можно добавить
  - `UIButtonType.Custom`— Настраиваемая кнопка

  Дополнительные сведения о различных типах кнопок см. в следующих статьях:
  
  - Раздел ["пользовательские типы кнопок"](#custom-button-types) этого документа
  - Рецепт [типов кнопок](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/create_different_types_of_buttons)
  - [Рекомендации по работе с человеческим интерфейсом iOS](https://developer.apple.com/design/human-interface-guidelines/ios/controls/buttons/)в Apple.

- Определите размер и расположение кнопки:

  ```csharp
  myButton.Frame = new CGRect(25, 25, 300, 150);
  ```

- Задайте текст кнопки. Используйте метод, для которого требуется текст `UIControlState` и значение: `SetTitle`

  ```csharp
  myButton.SetTitle("Hello, World!", UIControlState.Normal);
  ```

  Дополнительные сведения о оформлении кнопки и задании ее текста см. в следующих источниках:

  - Стиль раздела для [кнопки](#styling-a-button) в этом документе
  - Рецепт [ввода для кнопки Set](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/set_button_text) .

## <a name="handling-a-button-tap"></a>Обработка касания кнопки

Чтобы ответить на касание кнопки, укажите обработчик для `TouchUpInside` события кнопки:

```csharp
button.TouchUpInside += (sender, e) => {
    DoSomething();
};
```

> [!NOTE]
> `TouchUpInside`не является единственным доступным событием кнопки. `UIButton`является дочерним классом `UIControl`, который определяет [множество различных событий](xref:UIKit.UIControlEvent).

### <a name="using-the-ios-designer-to-specify-button-event-handlers"></a>Использование конструктора iOS для задания обработчиков событий кнопок

Используйте вкладку **события** **панель свойств** , чтобы указать обработчики событий для различных событий кнопки.

Для соответствующего события либо введите имя нового обработчика событий, либо выберите его из списка. Это приведет к созданию обработчика событий в коде для контроллера представления кнопки.

![Вкладка "события" Панель свойств](buttons-images/image1.png "Вкладка \"события\" Панель свойств")

## <a name="styling-a-button"></a>Стилизация кнопки

`UIButton`элементы управления могут находиться в разных состояниях, каждое из которых задается `UIControlState` значением — `Disabled` `Normal`, `Focused`, `Highlighted`, и т. д. Каждому состоянию можно присвоить уникальный стиль, заданный программно или с помощью конструктора iOS.

> [!NOTE]
> Чтобы получить полный список всех `UIControlState` значений, Взгляните на[`UIKit.UIControlState enumeration`](xref:UIKit.UIControlState)
> по.

Например, чтобы задать цвет заголовка и цвет тени для `UIControlState.Normal`:

```csharp
button.SetTitleColor(UIColor.White, UIControlState.Normal);
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Следующий код задает в качестве заголовка кнопки строку с атрибутом (стилизацию) для `UIControlState.Normal` и: `UIControlState.Highlighted`

```csharp
var normalAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle(normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle(highlightedAttributedTitle, UIControlState.Highlighted);
```

## <a name="custom-button-types"></a>Типы настраиваемых кнопок

Кнопки с `UIButtonType` параметром `Custom` не имеют стилей по умолчанию. Однако можно настроить внешний вид кнопки, задав для нее изображение в различных состояниях.

```csharp
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand.png"), UIControlState.Normal);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_Highlight.png"), UIControlState.Highlighted);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_On.png"), UIControlState.Selected);
```

В зависимости от того, касается ли пользователь кнопки, она будет отображаться как одно из следующих изображений (`UIControlState.Normal` `UIControlState.Highlighted` и `UIControlState.Selected` состояний соответственно):

![Уиконтролстате. Обычная] (buttons-images/image22.png "Уиконтролстате. Обычная") (buttons-images/image23.png "")
![](buttons-images/image24.png "") ![]Уиконтролстате. выделенный уиконтролстате. выделенный уиконтролстате. Selected уиконтролстате. Selected 


Дополнительные сведения о работе с пользовательскими кнопками см. в разделе [использование изображения для](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/use_an_image_for_a_button) рецепта кнопки.

